---
title: Kopieren einer Datenbank
description: Erstellen Sie eine hinsichtlich Transaktionen konsistente Kopie einer vorhandenen Azure SQL-Datenbank-Instanz auf demselben oder einem anderen Server.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sashan
ms.reviewer: carlrab
ms.date: 09/04/2019
ms.openlocfilehash: ebf63d14a8fb883158d1ac3e0a8f3d6658920aa7
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73826655"
---
# <a name="copy-a-transactionally-consistent-copy-of-an-azure-sql-database"></a>Kopieren einer transaktionskonsistenten Kopie einer Azure SQL-Datenbank-Instanz

Azure SQL-Datenbank verfügt über mehrere Methoden für die Erstellung einer transaktionskonsistenten Kopie einer vorhandenen Azure SQL-Datenbank ([Einzeldatenbank](sql-database-single-database.md)) auf demselben Server oder auf einem anderen Server. Sie können eine SQL-Datenbank über das Azure-Portal, über PowerShell oder über T-SQL kopieren. 

## <a name="overview"></a>Übersicht

Eine Datenbankkopie ist eine Momentaufnahme der Quelldatenbank zum Zeitpunkt der Kopieranforderung. Sie können denselben Server oder einen anderen auswählen. Sie können auch wahlweise Dienstebene und Computegröße des Servers beibehalten oder eine andere Computegröße innerhalb derselben Dienstebene (Edition) verwenden. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie zu einer voll funktionsfähigen, unabhängigen Datenbank. Zu diesem Zeitpunkt können Sie ein Upgrade oder Downgrade auf jede beliebige Edition durchführen. Anmeldungen, Benutzer und Berechtigungen können unabhängig verwaltet werden. Die Kopie wird mit der Technologie für die Georeplikation erstellt, und nachdem das Seeding abgeschlossen ist, wird der Link für die Georeplikation automatisch eingestellt. Alle Anforderungen zur Nutzung der Georeplikation gelten für den Datenbank-Kopiervorgang. Weitere Informationen finden Sie unter [Aktive Georeplikation – Übersicht](sql-database-active-geo-replication.md).

> [!NOTE]
> [Automatische Datenbanksicherungen](sql-database-automated-backups.md) werden verwendet, wenn Sie eine Datenbankkopie erstellen.

## <a name="logins-in-the-database-copy"></a>Anmeldungen in der Datenbankkopie

Wenn Sie eine Datenbank auf denselben SQL-Datenbank-Server kopieren, können für beide Datenbanken die gleichen Anmeldedaten verwendet werden. Der Sicherheitsprinzipal, den Sie zum Kopieren der Datenbank verwenden, wird zum Datenbankbesitzer der neuen Datenbank. Alle Datenbankbenutzer, ihre Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Kopie der Datenbank kopiert.  

Wenn Sie eine Datenbank auf einen anderen SQL-Datenbank-Server kopieren, wird der Sicherheitsprinzipal auf dem neuen Server zum Datenbankbesitzer für die neue Datenbank. Wenn Sie [eigenständige Datenbankbenutzer](sql-database-manage-logins.md) für den Datenzugriff verwenden, stellen Sie sicher, dass sowohl für die primäre als auch für die sekundäre Datenbank immer die gleichen Anmeldeinformationen verwendet werden, sodass Sie nach Abschluss des Kopiervorgang mit diesen Anmeldeinformationen sofort auf die Datenbank zugreifen können. 

Wenn Sie [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verwenden, müssen Sie die Anmeldeinformationen in der Kopie überhaupt nicht verwalten. Wenn Sie jedoch die Datenbank auf einen neuen Server kopieren, funktioniert der anmeldungsbasierte Zugriff eventuell nicht, da die Anmeldungen auf dem neuen Server nicht vorhanden sind. Informationen zum Verwalten von Anmeldungen beim Kopieren einer Datenbank auf einen anderen SQL-Datenbank-Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md). 

Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (der Datenbankbesitzer), zum Anmelden bei der neuen Datenbank verwendet werden. Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Kopieren einer Datenbank über das Azure-Portal

Um eine Datenbank über das Azure-Portal zu kopieren, öffnen Sie die Seite für Ihre Datenbank, und klicken Sie auf **Kopieren**. 

   ![Datenbankkopie](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Kopieren einer Datenbank mit PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Um eine Datenbank über PowerShell zu kopieren, verwenden Sie das [New-AzSqlDatabaseCopy](/powershell/module/az.sql/new-azsqldatabasecopy)-Cmdlet. 

```powershell
New-AzSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Ein vollständiges Beispielskript finden Sie unter [Kopieren einer Datenbank auf einen neuen Server](scripts/sql-database-copy-database-to-new-server-powershell.md).

Der Datenbank-Kopiervorgang ist ein asynchroner Vorgang, aber die Zieldatenbank wird sofort erstellt, nachdem die Anforderung akzeptiert wurde. Wenn Sie den Kopiervorgang abbrechen müssen, während er noch ausgeführt wird, sollten Sie die Zieldatenbank mit dem Cmdlet [Remove-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) löschen.  

## <a name="rbac-roles-to-manage-database-copy"></a>RBAC-Rollen zum Verwalten der Datenbankkopie

Zum Erstellen einer Datenbankkopie müssen Sie über die folgenden Rollen verfügen:

- Besitzer des Abonnements
- Rolle „Mitwirkender von SQL Server“ oder
- Benutzerdefinierte Rolle für die Quell- und Zieldatenbank mit folgender Berechtigung:

   Microsoft.Sql/servers/databases/read   
   Microsoft.Sql/servers/databases/write   

Zum Abbrechen eines Datenbank-Kopiervorgangs müssen Sie über die folgenden Rollen verfügen:

- Besitzer des Abonnements
- Rolle „Mitwirkender von SQL Server“ oder
- Benutzerdefinierte Rolle für die Quell- und Zieldatenbank mit folgender Berechtigung:

   Microsoft.Sql/servers/databases/read   
   Microsoft.Sql/servers/databases/write   
   
Zum Verwalten der Datenbankkopie über das Azure-Portal benötigen Sie außerdem die folgenden Berechtigungen:

&nbsp; &nbsp; &nbsp; Microsoft.Resources/subscriptions/resources/read   
&nbsp; &nbsp; &nbsp; Microsoft.Resources/subscriptions/resources/write   
&nbsp; &nbsp; &nbsp; Microsoft.Resources/deployments/read   
&nbsp; &nbsp; &nbsp; Microsoft.Resources/deployments/write   
&nbsp; &nbsp; &nbsp; Microsoft.Resources/deployments/operationstatuses/read    

Wenn Sie die Vorgänge unter Bereitstellungen in der Ressourcengruppe im Portal anzeigen möchten, und zwar Vorgänge über mehrere Ressourcenanbieter hinweg einschließlich SQL-Vorgängen, benötigen Sie diese zusätzlichen RBAC-Rollen: 

&nbsp; &nbsp; &nbsp; Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read   
&nbsp; &nbsp; &nbsp; Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read



## <a name="copy-a-database-by-using-transact-sql"></a>Kopieren einer Datenbank mit Transact-SQL

Melden Sie sich mit der Prinzipalanmeldung auf Serverebene oder der Anmeldung, mit der die zu kopierende Datenbank erstellt wurde, bei der Datenbank master an. Damit der Datenbankkopiervorgang funktioniert, müssen Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, Mitglieder der Rolle „dbmanager“ sein. Weitere Informationen zu Anmeldungen und zum Herstellen einer Verbindung mit dem Server finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md).

Starten Sie das Kopieren der Quelldatenbank mit der Anweisung [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) . Durch das Ausführen dieser Anweisung wird der Kopiervorgang für die Datenbank initiiert. Da das Kopieren einer Datenbank ein asynchroner Prozess ist, erfolgt die Rückgabe der CREATE DATABASE-Anweisung, bevor die Datenbank den Kopiervorgang abgeschlossen hat.

### <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf denselben Server

Melden Sie sich mit der Prinzipalanmeldung auf Serverebene oder der Anmeldung, mit der die zu kopierende Datenbank erstellt wurde, bei der Datenbank master an. Damit der Datenbankkopiervorgang funktioniert, müssen Anmeldungen, bei denen der Prinzipal auf Serverebene nicht verwendet wird, Mitglieder der Rolle „dbmanager“ sein.

Mit diesem Befehl wird Database1 in eine neue Datenbank mit dem Namen „Database2“ auf demselben Server kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database2 AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einen anderen Server

Melden Sie sich bei der Masterdatenbank auf dem Zielserver an. Dies ist der SQL-Datenbank-Server, auf dem die neue Datenbank erstellt werden soll. Verwenden Sie eine Anmeldung, die den gleichen Namen und das gleiche Kennwort wie der Datenbankbesitzer der Quelldatenbank auf dem SQL-Datenbank-Quellserver verwendet. Die Anmeldung auf dem Zielserver muss auch ein Mitglied der Rolle „dbmanager“ sein, oder es muss sich um die Prinzipalanmeldung auf Serverebene handeln.

Mit diesem Befehl wird Database1 auf server1 in eine neue Datenbank mit dem Namen „Database2“ auf server2 kopiert. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Kopiervorgang abgeschlossen ist.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database2 AS COPY OF server1.Database1;
    
> [!IMPORTANT]
> Die Firewalls beider Server müssen so konfiguriert werden, dass sie eingehende Verbindungen über die IP-Adresse des Clients zulassen, der den T-SQL-COPY-Befehl ausgibt.

### <a name="copy-a-sql-database-to-a-different-subscription"></a>Kopieren einer SQL-Datenbank in ein anderes Abonnement

Sie können Ihre Datenbank mit den im vorherigen Abschnitt beschriebenen Schritten in eine SQL-Datenbank-Serverinstanz in einem anderen Abonnement kopieren. Verwenden Sie unbedingt eine Anmeldung, bei der Name und Kennwort mit denen des Datenbankbesitzers der Quelldatenbank identisch sind, und die ein Mitglied der DBManager-Rolle oder die Prinzipalanmeldung auf Serverebene ist. 

> [!NOTE]
> Das [Azure-Portal](https://portal.azure.com) unterstützt das Kopieren in ein anderes Abonnement nicht, da das Portal die ARM-API aufruft, und es verwendet die Abonnementzertifikate, um auf beide an der Georeplikation beteiligten Server zuzugreifen.  

### <a name="monitor-the-progress-of-the-copying-operation"></a>Überwachen des Fortschritts des Kopiervorgangs

Überwachen Sie den Kopiervorgang, indem Sie die Ansichten „sys.databases“ und „sys.dm_database_copies“ abfragen. Während des Kopiervorgangs wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **COPYING** gesetzt.

* Wenn beim Kopieren ein Fehler auftritt, wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **SUSPECT** gesetzt. Führen Sie die DROP-Anweisung in der neuen Datenbank aus, und wiederholen Sie den Vorgang später noch einmal.
* Wenn der Kopiervorgang erfolgreich ist, wird die Spalte **state_desc** der Ansicht „sys.databases“ für die neue Datenbank auf **ONLINE** gesetzt. Der Kopiervorgang ist abgeschlossen, und die neue Datenbank ist eine normale Datenbank, die unabhängig von der Quelldatenbank geändert werden kann.

> [!NOTE]
> Wenn Sie den Kopiervorgang während der Ausführung abbrechen möchten, führen Sie die Anweisung [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) für die neue Datenbank aus. Alternativ dazu kann der Kopiervorgang auch abgebrochen werden, indem die DROP DATABASE-Anweisung in der Quelldatenbank ausgeführt wird.

> [!IMPORTANT]
> Falls Sie eine Kopie mit einem deutlich niedrigeren SLO-Wert als die Quelle erstellen müssen, verfügt die Zieldatenbank ggf. nicht über genügend Ressourcen für die Durchführung des Seedingprozesses, und für den Kopiervorgang tritt unter Umständen ein Fehler auf. Verwenden Sie in diesem Szenario eine Anforderung einer Geowiederherstellung, um eine Kopie auf einem anderen Server bzw. in einer anderen Region zu erstellen. Weitere Informationen finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen: Geografische Wiederherstellung](sql-database-recovery-using-backups.md#geo-restore).


## <a name="resolve-logins"></a>Auflösen von Anmeldungen

Nachdem die neue Datenbank auf dem Zielserver online ist, können Sie die [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) -Anweisung verwenden, um die Benutzer aus der neuen Datenbank den Anmeldungen auf dem Zielserver zuzuordnen. Informationen zum Auflösen von verwaisten Benutzern finden Sie unter [Problembehandlung bei verwaisten Benutzern](https://msdn.microsoft.com/library/ms175475.aspx). Siehe auch [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

Für alle Benutzer werden in der neuen Datenbank die Berechtigungen beibehalten, über die sie auch in der Quelldatenbank verfügt haben. Der Benutzer, der das Kopieren der Datenbank initiiert hat, wird zum Datenbankbesitzer der neuen Datenbank und erhält eine neue Sicherheits-ID (SID). Nachdem der Kopiervorgang erfolgreich abgeschlossen wurde und bevor andere Benutzer neu zugeordnet werden, kann nur die Anmeldung, über die der Kopiervorgang initiiert wurde (der Datenbankbesitzer), zum Anmelden bei der neuen Datenbank verwendet werden.

Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen SQL-Datenbank-Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Anmeldungen finden Sie unter [Verwalten von Anmeldungen](sql-database-manage-logins.md) und [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* Informationen zum Exportieren einer Datenbank finden Sie unter [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md).
