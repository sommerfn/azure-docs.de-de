---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer Sicherung | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage).
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
ms.date: 09/26/2019
ms.openlocfilehash: f316f77d0f4ca3132a2ae77d807e2dd66ba62a43
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71846286"
---
# <a name="recover-an-azure-sql-database-by-using-automated-database-backups"></a>Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen

Standardmäßig werden Sicherungen von Azure SQL-Datenbanken im georeplizierten Blobspeicher gespeichert. Die folgenden Optionen stehen für die Datenbankwiederherstellung mithilfe [automatisierter Datenbanksicherungen](sql-database-automated-backups.md) zur Verfügung. Ihre Möglichkeiten:

- Erstellen einer neuen Datenbank auf dem gleichen SQL-Datenbank-Server, wobei der Zustand für einen angegebenen Zeitpunkt innerhalb des Aufbewahrungszeitraums wiederhergestellt wird.
- Erstellen einer Datenbank auf dem gleichen SQL-Datenbank-Server, wobei der Zustand wiederhergestellt wird, in dem sich eine Datenbank zum Zeitpunkt der Datenbanklöschung befand.
- Erstellen einer neuen Datenbank auf einem beliebigen SQL-Datenbank-Server in derselben Region, die auf den Zustand zum Zeitpunkt der neuesten Sicherung wiederhergestellt wird.
- Erstellen einer neuen Datenbank auf einem beliebigen SQL-Datenbank-Server in einer beliebigen anderen Region, die auf den Zustand zum Zeitpunkt der neuesten replizierten Sicherung wiederhergestellt wird.

Wenn Sie die [langfristige Aufbewahrung von Sicherungen](sql-database-long-term-retention.md) konfiguriert haben, können Sie auch eine neue Datenbank aus jeder Langzeitaufbewahrungssicherung auf einem beliebigen SQL-Datenbank-Server erstellen.

> [!IMPORTANT]
> Sie können eine vorhandene Datenbank während der Wiederherstellung nicht überschreiben.

Wenn Sie die Dienstebene „Standard“ oder „Premium“ verwenden, fallen bei der Wiederherstellung Ihrer Datenbanken u. U. zusätzliche Speicherkosten an. Die zusätzlichen Kosten entstehen, wenn die maximale Größe der wiederhergestellten Datenbank die mit der Dienstebene und Leistungsstufe der Zieldatenbank verbundene Menge des Speicherplatzes überschreitet. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/). Wenn die tatsächlich verwendete Speichermenge kleiner als die enthaltene Speichermenge ist, können Sie diese zusätzlichen Kosten durch Festlegen der maximalen Datenbankgröße auf die enthaltene Menge vermeiden.

> [!NOTE]
> Beim Erstellen einer [Datenbankkopie](sql-database-copy.md) verwenden Sie [automatisierte Datenbanksicherungen](sql-database-automated-backups.md).

## <a name="recovery-time"></a>Wiederherstellungszeit

Die Zeit, die zum Wiederherstellen einer Datenbank mit automatisierten Sicherungen benötigt wird, hängt von mehreren Faktoren ab:

- Größe der Datenbank
- Computegröße der Datenbank
- Anzahl der beteiligten Transaktionsprotokolle
- Menge der erneut auszuführenden Aktivitäten, um den Wiederherstellungspunkt wiederherzustellen
- Netzwerkbandbreite, sofern die Wiederherstellung in einer anderen Region erfolgt
- Anzahl der gleichzeitigen Wiederherstellungsanforderungen, die aktuell in der Zielregion verarbeitet werden

Bei einer großen oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Wenn es sich um einen längeren Ausfall in einer Region handelt, müssen möglicherweise andere Regionen eine große Anzahl von Geowiederherstellungsanforderungen verarbeiten. Wenn viele Anforderungen vorliegen, erhöht sich dadurch u. U. die Wiederherstellungsdauer für Datenbanken in dieser Region. Der Großteil der Datenbankwiederherstellungen erfolgt in weniger als 12 Stunden.

Für ein einzelnes Abonnement gibt es Einschränkungen hinsichtlich der Anzahl gleichzeitiger Wiederherstellungsanforderungen.  Diese Einschränkungen gelten für eine beliebige Kombination von Point-in-Time-Wiederherstellungen, Geowiederherstellungen und Wiederherstellungen aus einer langfristig aufbewahrten Sicherung.

| | **Max. Anzahl gleichzeitiger Anforderungen, die verarbeitet werden** | **Max. Anzahl gleichzeitiger Anforderungen, die übermittelt werden** |
| :--- | --: | --: |
|Einzeldatenbank (pro Abonnement)|10|60|
|Pool für elastische Datenbanken (pro Pool)|4|200|
||||

Es gibt keine integrierte Methode zum Wiederherstellen des gesamten Servers. Ein Beispiel zum Ausführen dieser Aufgabe finden Sie unter [Azure SQL Database: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) (Azure SQL-Datenbank: Wiederherstellen des gesamten Servers).

> [!IMPORTANT]
> Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ im Abonnement oder Besitzer des Abonnements sein. Weitere Informationen finden Sie unter [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md). Sie können das Azure-Portal, PowerShell oder die REST-API zur Wiederherstellung verwenden. Die Nutzung von Transact-SQL ist nicht möglich.

## <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung

Sie können den Zustand einer eigenständigen, Pool- oder Instanzdatenbank zu einem früheren Zeitpunkt mithilfe von Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) oder [REST-API](https://docs.microsoft.com/rest/api/sql/databases) wiederherstellen. Die Anforderung kann jede Dienstebene oder Computegröße für die wiederhergestellte Datenbank angeben. Stellen Sie sicher, dass auf dem Server, auf dem Sie die Datenbank wiederherstellen, ausreichende Ressourcen vorhanden sind. Nach Abschluss der Wiederherstellung wird eine neue Datenbank auf demselben Server wie die ursprüngliche Datenbank erstellt. Die wiederhergestellte Datenbank wird zu normalen Preisen basierend auf der Dienstebene und Computegröße in Rechnung gestellt. Kosten entstehen erst, wenn die Datenbankwiederherstellung abgeschlossen ist.

Im Allgemeinen wird beim Wiederherstellen der Datenbank der Zustand zu einem früheren Zeitpunkt wiederhergestellt. Sie können die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank behandeln oder als Quelldaten zum Aktualisieren der ursprünglichen Datenbank verwenden.

- **Ersetzung der Datenbank**

  Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank vorgesehen ist, sollten Sie die ursprüngliche Computegröße und Dienstebene der Datenbank angeben. Sie können die ursprüngliche Datenbank dann umbenennen und der wiederhergestellten Datenbank mit dem T-SQL-Befehl [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) den ursprünglichen Namen geben.

- **Wiederherstellung der Daten**

  Wenn Sie planen, zum Wiederherstellen nach einem Benutzer- oder Anwendungsfehler Daten aus der wiederhergestellten Datenbank abzurufen, müssen Sie ein Datenwiederherstellungsskript schreiben und ausführen, das Daten aus der Datenbank extrahiert und auf die ursprüngliche Datenbank anwendet. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die wiederherstellende Datenbank wird jedoch während des Wiederherstellungsvorgangs in der Datenbankliste angezeigt. Wenn Sie die Datenbank während der Wiederherstellung löschen, wird der Wiederherstellungsvorgang abgebrochen, und es fallen keine Kosten für die Datenbank an, deren Wiederherstellung nicht abgeschlossen wurde.
  
### <a name="point-in-time-restore-by-using-azure-portal"></a>Point-in-Time-Wiederherstellung mithilfe des Azure-Portals

Sie können die Wiederherstellung einer einzelnen SQL-Datenbank oder Instanzdatenbank zu einem bestimmten Zeitpunkt auf dem Blatt „Übersicht“ der Datenbank ausführen, die im Azure-Portal wiederhergestellt werden soll.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Um eine Einzel- oder Pooldatenbank mithilfe von Azure-Portal auf den Zustand zu einem bestimmten Zeitpunkt wiederherzustellen, öffnen Sie die Datenbank-Übersichtsseite, und wählen Sie auf der Symbolleiste die Option **Wiederherstellen** aus. Wählen Sie die Sicherungsquelle aus, und wählen Sie den Sicherungspunkt-Zeitpunkt aus, von dem aus eine neue Datenbank erstellt wird. 

  ![Screenshot der Optionen für die Datenbankwiederherstellung](./media/sql-database-recovery-using-backups/pitr-backup-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Um eine Datenbank der verwalteten Instanz mithilfe von Azure-Portal auf den Zustand zu einem bestimmten Zeitpunkt wiederherzustellen, öffnen Sie die Datenbank-Übersichtsseite, und wählen Sie auf der Symbolleiste die Option **Wiederherstellen** aus. Wählen Sie den Sicherungspunkt-Zeitpunkt aus, von dem aus eine neue Datenbank erstellt wird. 

  ![Screenshot der Optionen für die Datenbankwiederherstellung](./media/sql-database-recovery-using-backups/pitr-backup-managed-instance-annotated.png)

> [!TIP]
> Informationen zum programmgesteuerten Wiederherstellen einer Datenbank aus einer Sicherung finden Sie unter [Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen](sql-database-recovery-using-backups.md).

## <a name="deleted-database-restore"></a>Wiederherstellen einer gelöschten Datenbank

Sie können eine gelöschte Datenbank auf demselben SQL-Datenbank-Server oder in der gleichen verwalteten Instanz auf den Zustand zum Zeitpunkt des Löschens oder früher wiederherstellen. Hierzu können Sie das Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) oder [REST (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) verwenden. Die Wiederherstellung einer gelöschten Datenbank erfolgt durch das Erstellen einer neuen Datenbank aus der Sicherung.

> [!IMPORTANT]
> Wenn Sie eine Azure SQL-Datenbank-Serverinstanz oder verwaltete Instanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden. Ein gelöschter Server oder eine gelöschte verwaltete Instanz kann nicht wiederhergestellt werden.

### <a name="deleted-database-restore-by-using-the-azure-portal"></a>Wiederherstellen einer gelöschten Datenbank im Azure-Portal

Die Wiederherstellung gelöschter Datenbanken aus dem Azure-Portal erfolgt aus der Server- und der Instanzressource.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Zum Wiederherstellen einer gelöschten Einzel- oder Pooldatenbank mithilfe des Azure-Portals öffnen Sie die Serverübersichtsseite, und wählen Sie **Gelöschte Datenbanken** aus. Wählen Sie eine gelöschte Datenbank aus, die Sie wiederherstellen möchten, und geben Sie den Namen für die neue Datenbank ein, die mit den aus der Sicherung wiederhergestellten Daten erstellt wird.

  ![Screenshot der Optionen für „Gelöschte Datenbanken“](./media/sql-database-recovery-using-backups/restore-deleted-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Sie können über das Azure-Portal keine gelöschte Datenbank auf einer verwalteten Instanz wiederherstellen. Hierfür können Sie PowerShell verwenden. 

### <a name="deleted-database-restore-by-using-powershell"></a>Wiederherstellen einer gelöschten Datenbank mithilfe von PowerShell

Verwenden Sie die folgenden Beispielskripts, um eine gelöschte Datenbank für Azure SQL-Datenbank und eine verwaltete Instanz mithilfe von PowerShell wiederherzustellen.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Ein PowerShell-Beispielskript, das zeigt, wie eine gelöschte Azure SQL-Datenbank wiederhergestellt wird, finden Sie unter [Wiederherstellen einer gelöschten SQL-Datenbank mithilfe von PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Ein PowerShell-Beispielskript, das zeigt, wie eine gelöschte Instanzdatenbank wiederhergestellt wird, finden Sie unter [Wiederherstellen einer gelöschten Datenbank auf einer verwalteten Instanz mithilfe von PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Informationen zum programmgesteuerten Wiederherstellen einer gelöschten Datenbank finden Sie unter [Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen](sql-database-recovery-using-backups.md).

## <a name="geo-restore"></a>Geografische Wiederherstellung

Sie können eine SQL-Datenbank auf einem beliebigen Server in einer beliebigen Azure-Region aus den letzten georeplizierten Sicherungen wiederherstellen. Geowiederherstellung verwendet eine georeplizierte Sicherung als Quelle. Sie können die Geowiederherstellung auch dann anfordern, wenn aufgrund eines Ausfalls kein Zugriff auf Datenbank oder Rechenzentrum möglich ist.

Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn Ihre Datenbank aufgrund eines Vorfalls in der Hostingregion nicht verfügbar ist. Sie können die Datenbank auf einem Server in einer beliebigen anderen Region wiederherstellen. Es gibt eine Verzögerung zwischen der Erstellung einer Sicherung und der Georeplikation in einem Azure-Blob in einer anderen Region. Daher kann die wiederhergestellte Datenbank bis zu einer Stunde hinter der ursprünglichen Datenbank zurückliegen. Die folgende Abbildung zeigt eine Datenbankwiederherstellung aus der letzten verfügbaren Sicherung in einer anderen Region.

![Darstellung der Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)

### <a name="geo-restore-by-using-the-azure-portal"></a>Durchführen einer Geowiederherstellung über das Azure-Portal

Erstellen Sie im Azure-Portal eine neue Einzeldatenbank oder Datenbank der verwalteten Instanz, und wählen Sie eine verfügbare Sicherung für die Geowiederherstellung aus. Die neu erstellte Datenbank enthält die mittels Geowiederherstellung wiederhergestellten Sicherungsdaten.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Wenn Sie eine einzelne SQL-Einzeldatenbank mittels Geowiederherstellung aus dem Azure-Portal in der Region und auf dem Server Ihrer Wahl wiederherstellen möchten, führen Sie die folgenden Schritte aus:

1. Wählen Sie im **Dashboard** die Option **Hinzufügen** > **SQL-Datenbank erstellen** aus. Geben Sie auf der Registerkarte **Grundeinstellungen** die erforderlichen Informationen ein.
2. Wählen Sie **Zusätzliche Einstellungen** aus.
3. Wählen Sie unter **Vorhandene Daten verwenden** die Option **Sicherung** aus.
4. Wählen Sie unter **Sicherung** in der Liste der verfügbaren Geowiederherstellungssicherungen eine Sicherung aus.

    ![Screenshot der Optionen für „SQL-Datenbank erstellen“](./media/sql-database-recovery-using-backups/geo-restore-azure-sql-database-list-annotated.png)

Schließen Sie den Vorgang des Erstellens einer neuen Datenbank ab. Die erstellte Azure SQL-Einzeldatenbank enthält die mittels Geowiederherstellung wiederhergestellte Sicherung.

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Wenn Sie eine Datenbank der verwalteten Instanz mittels Geowiederherstellung aus Azure-Portal auf einer vorhandenen verwalteten Instanz in einer Region Ihrer Wahl wiederherstellen möchten, wählen Sie die verwaltete Instanz aus, auf der die Datenbank wiederhergestellt werden soll. Folgen Sie diesen Schritten:

1. Wählen Sie **Neue Datenbank** aus.
2. Geben Sie den gewünschten Datenbanknamen ein.
3. Wählen Sie unter **Vorhandene Daten verwenden** die Option **Sicherung** aus.
4. Wählen Sie in der Liste der verfügbaren Geowiederherstellungssicherungen eine Sicherung aus.

    ![Screenshot der Optionen für „Neue Datenbank“](./media/sql-database-recovery-using-backups/geo-restore-sql-managed-instance-list-annotated.png)

Schließen Sie den Vorgang des Erstellens einer neuen Datenbank ab. Die erstellte Instanzdatenbank enthält die mittels Geowiederherstellung wiederhergestellte Sicherung.

### <a name="geo-restore-by-using-powershell"></a>Durchführen einer Geowiederherstellung mithilfe von PowerShell

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Ein PowerShell-Skript, das zeigt, wie die Geowiederherstellung für eine Azure SQL-Einzeldatenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Einzeldatenbank auf den Zustand zu einem früheren Zeitpunkt mithilfe von PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Ein PowerShell-Skript, das zeigt, wie die Geowiederherstellung für eine Datenbank der verwalteten Instanz durchgeführt wird, finden Sie unter [Wiederherstellen einer Datenbank der verwalteten Instanz in einer anderen geografischen Region mithilfe von PowerShell](scripts/sql-managed-instance-restore-geo-backup.md).

### <a name="geo-restore-considerations"></a>Überlegungen zur Geowiederherstellung

Sie können keine Point-in-Time-Wiederherstellung für eine sekundäre Geodatenbank durchführen. Dies ist nur für eine primäre Datenbank möglich. Ausführliche Informationen zum Verwenden der Geowiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Die Geowiederherstellung ist die einfachste in SQL-Datenbank verfügbare Lösung für die Notfallwiederherstellung. Sie beruht auf automatisch erstellten georeplizierten Sicherungen mit RPO (Recovery Point Objective) = 1 Stunde und geschätzter Wiederherstellungszeit von bis zu 12 Stunden. Sie garantiert nicht, dass die Zielregion über die Kapazität zum Wiederherstellen Ihrer Datenbanken nach einem regionalen Ausfall verfügt, da der Bedarf voraussichtlich stark ansteigen wird. Für nicht unternehmenskritische Anwendungen, die relativ kleine Datenbanken verwenden, ist die Geowiederherstellung eine geeignete Lösung für die Notfallwiederherstellung. Für unternehmenskritische Anwendungen, die große Datenbanken verwenden und die Geschäftskontinuität gewährleisten müssen, sind [Autofailover-Gruppen](sql-database-auto-failover-group.md) zu verwenden. Die Lösung bietet erhebliche niedrigere RPO (Recovery Point Objective) und RTO (Recovery Time Objective), und die Kapazität ist immer garantiert. Weitere Informationen zur Optionen für Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

## <a name="programmatically-performing-recovery-by-using-automated-backups"></a>Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen

Sie können auch Azure PowerShell oder die REST-API für die Wiederherstellung verwenden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell-Azure Resource Manager-Modul wird von Azure SQL-Datenbank zwar weiterhin unterstützt, zukünftig wird jedoch für das Az.Sql-Modul entwickelt. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind in großen Teilen identisch.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Wie Sie eine eigenständige Datenbank oder in einem Pool zusammengefasste Datenbank wiederherstellen, erfahren Sie unter [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | BESCHREIBUNG |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Ruft mindestens eine Datenbank ab. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Ruft eine gelöschte Datenbank ab, die Sie wiederherstellen können. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Ruft eine georedundante Sicherung einer Datenbank ab. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Stellt eine SQL-Datenbank wieder her. |

  > [!TIP]
  > Ein PowerShell-Beispielskript, das zeigt, wie eine Point-in-Time-Wiederherstellung einer Datenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer SQL-Datenbank mithilfe von PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Wie Sie die Datenbank einer verwalteten Instanz wiederherstellen, erfahren Sie unter [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | BESCHREIBUNG |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Ruft eine oder mehrere verwaltete Instanzen ab. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Ruft Instanzdatenbanken ab. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Stellt eine Instanzdatenbank wieder her. |

### <a name="rest-api"></a>REST-API

So stellen Sie eine Einzel- oder Pooldatenbank mithilfe der REST-API wieder her:

| API | BESCHREIBUNG |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Stellt eine Datenbank wieder her. |
| [Get Create or Update Database Status](https://docs.microsoft.com/rest/api/sql/operations) |Ruft den Status während eines Wiederherstellungsvorgangs ab. |

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Eine Erläuterung zum Wiederherstellen einer Einzel- oder Pooldatenbank mithilfe der Azure CLI finden Sie unter [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Eine Erläuterung zum Wiederherstellen einer Datenbank der verwalteten Instanz mithilfe der Azure CLI finden Sie unter [az sql midb restore](/cli/azure/sql/midb#az-sql-midb-restore).

## <a name="summary"></a>Zusammenfassung

Automatische Sicherungen schützen Ihre Datenbanken vor Benutzer- und Anwendungsfehlern, versehentlichen Datenbanklöschungen und längeren Ausfällen. Diese integrierte Funktion ist für alle Dienstebenen und Computegrößen verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
- [Langfristige Aufbewahrung](sql-database-long-term-retention.md)
- Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-active-geo-replication.md) oder [Autofailover-Gruppen](sql-database-auto-failover-group.md).
