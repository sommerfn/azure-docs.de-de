---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer Sicherung | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage).
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
ms.openlocfilehash: 890a9701615a05186b34883f4e953bbc045e906f
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350087"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen

Standardmäßig werden Sicherungen von SQL-Datenbank im georeplizierten Blobspeicher (RA-GRS) gespeichert. Die folgenden Optionen stehen für die Datenbankwiederherstellung mithilfe [automatisierter Datenbanksicherungen](sql-database-automated-backups.md) zur Verfügung:

- Erstellen einer neuen Datenbank auf demselben SQL-Datenbank-Server, die auf den Zustand zu einem angegebenen Zeitpunkt innerhalb der Aufbewahrungsdauer wiederhergestellt wird.
- Erstellen einer Datenbank auf demselben SQL-Datenbank-Server, die auf den Zustand zum Zeitpunkt des Löschens einer gelöschten Datenbank wiederhergestellt wird.
- Erstellen einer neuen Datenbank auf einem beliebigen SQL-Datenbank-Server in derselben Region, die auf den Zustand zum Zeitpunkt der neuesten Sicherung wiederhergestellt wird.
- Erstellen einer neuen Datenbank auf einem beliebigen SQL-Datenbank-Server in einer beliebigen anderen Region, die auf den Zustand zum Zeitpunkt der neuesten replizierten Sicherung wiederhergestellt wird.

Wenn Sie die [langfristige Aufbewahrung von Sicherungen](sql-database-long-term-retention.md) konfiguriert haben, können Sie auch eine neue Datenbank aus jeder LTR-Sicherung auf einem beliebigen SQL-Datenbank-Server erstellen.

> [!IMPORTANT]
> Sie können eine vorhandene Datenbank während der Wiederherstellung nicht überschreiben.

Bei Verwendung der Standard- oder Premium-Dienstebene ist eine wiederhergestellte Datenbank unter folgenden Bedingungen mit zusätzlichen Speicherkosten verbunden:

- Wiederherstellung von P11–P15 bis S4–S12 oder P1–P6, wenn die maximale Datenbankgröße 500 GB überschreitet.
- Wiederherstellung von P1–P6 in S4–S12, wenn die maximale Datenbankgröße 250 GB überschreitet.

Die zusätzlichen Kosten entstehen, wenn die maximale Größe der wiederhergestellten Datenbank die mit der Dienstebene und Leistungsstufe der Zieldatenbank verbundene Menge des Speicherplatzes überschreitet. Der oberhalb des inbegriffenen Volumens bereitgestellte Speicher wird extra in Rechnung gestellt. Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/). Wenn die tatsächlich verwendete Speichermenge kleiner als die enthaltene Speichermenge ist, können Sie diese zusätzlichen Kosten durch Festlegen der maximalen Datenbankgröße auf die enthaltene Menge vermeiden.

> [!NOTE]
> [Automatische Datenbanksicherungen](sql-database-automated-backups.md) werden verwendet, wenn Sie eine [Datenbankkopie](sql-database-copy.md) erstellen.

## <a name="recovery-time"></a>Wiederherstellungszeit

Die Zeit, die zum Wiederherstellen einer Datenbank mit automatisierten Sicherungen benötigt wird, hängt von mehreren Faktoren ab:

- Größe der Datenbank
- Computegröße der Datenbank
- Anzahl der beteiligten Transaktionsprotokolle
- Menge der erneut auszuführenden Aktivitäten, um den Wiederherstellungspunkt wiederherzustellen
- Netzwerkbandbreite, sofern die Wiederherstellung in einer anderen Region erfolgt
- Anzahl der gleichzeitigen Wiederherstellungsanforderungen, die aktuell in der Zielregion verarbeitet werden

Bei einer großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Wenn es sich um einen längeren Ausfall in einer Region handelt, müssen möglicherweise andere Regionen eine große Anzahl von Geowiederherstellungsanforderungen verarbeiten. Wenn viele Anforderungen vorliegen, erhöht sich dadurch unter Umständen die Wiederherstellungsdauer für Datenbanken in dieser Region. Der Großteil der Datenbankwiederherstellungen erfolgt in weniger als 12 Stunden.

Für ein einzelnes Abonnement gibt es Einschränkungen der Anzahl gleichzeitiger Wiederherstellungsanforderungen.  Diese Einschränkungen gelten für eine beliebige Kombination von Point-in-Time-Wiederherstellungen, Geowiederherstellungen und Wiederherstellungen aus einer langfristig aufbewahrten Sicherung:

| | **Max. Anzahl gleichzeitiger Anforderungen, die verarbeitet werden** | **Max. Anzahl gleichzeitiger Anforderungen, die übermittelt werden** |
| :--- | --: | --: |
|Einzeldatenbank (pro Abonnement)|10|60|
|Pool für elastische Datenbanken (pro Pool)|4|200|
||||

Derzeit gibt es keine integrierte Methode, um den gesamten Server wiederherzustellen. Das Skript [Azure SQL-Datenbank: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) (Vollständige Serverwiederherstellung) ist ein Beispiel dafür, wie Sie diese Aufgabe erreichen können.

> [!IMPORTANT]
> Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ im Abonnement oder Besitzer des Abonnements sein. Weitere Informationen dazu finden Sie unter [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md). Sie können die Wiederherstellung mithilfe von Azure-Portal, PowerShell oder der REST-API durchführen. Die Nutzung von Transact-SQL ist nicht möglich.

## <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung

Sie können eine eigenständige Datenbank, eine Pool- oder Instanzdatenbank mithilfe von Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) oder der [REST-API](https://docs.microsoft.com/rest/api/sql/databases) auf deren Zustand zu einem früheren Zeitpunkt wiederherstellen. Die Anforderung kann jede Dienstebene oder Computegröße für die wiederhergestellte Datenbank angeben. Stellen Sie sicher, dass Sie auf dem Server, auf dem Sie die Datenbank wiederherstellen, über ausreichend Ressourcen verfügen. Nach Abschluss des Vorgangs wird eine neue Datenbank auf demselben Server wie die ursprüngliche Datenbank erstellt. Die wiederhergestellte Datenbank wird basierend auf Dienstebene und Computegröße zum normalen Satz in Rechnung gestellt. Kosten entstehen erst, wenn die Datenbankwiederherstellung abgeschlossen ist.

Im Allgemeinen wird beim Wiederherstellen der Datenbank der Zustand zu einem früheren Zeitpunkt wiederhergestellt. Sie können die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank behandeln oder als Quelldaten zum Aktualisieren der ursprünglichen Datenbank verwenden.

- **Ersetzung der Datenbank**

  Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank vorgesehen ist, sollten Sie die ursprüngliche Computegröße und Dienstebene der Datenbank angeben. Sie können die ursprüngliche Datenbank dann umbenennen und der wiederhergestellten Datenbank mit dem T-SQL-Befehl [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) den ursprünglichen Namen geben.

- **Wiederherstellung der Daten**

  Wenn Sie planen, zum Wiederherstellen nach einem Benutzer- oder Anwendungsfehler Daten aus der wiederhergestellten Datenbank abzurufen, müssen Sie ein Datenwiederherstellungsskript schreiben und ausführen, das Daten aus der Datenbank extrahiert und auf die ursprüngliche Datenbank anwendet. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die wiederherstellende Datenbank wird jedoch während des Wiederherstellungsvorgangs in der Datenbankliste angezeigt. Wenn Sie die Datenbank während der Wiederherstellung löschen, wird der Wiederherstellungsvorgang abgebrochen, und es fallen keine Kosten für die Datenbank an, deren Wiederherstellung nicht abgeschlossen wurde.
  
### <a name="point-in-time-restore-using-azure-portal"></a>Point-in-Time-Wiederherstellung mithilfe des Azure-Portals

Die Wiederherstellung einer einzelnen SQL-Datenbank oder Instanzdatenbank zu einem bestimmten Zeitpunkt wird auf dem Blatt „Übersicht“ der Datenbank ausgeführt, die Sie im Azure-Portal wiederherstellen möchten.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Um eine Einzel- oder Pooldatenbank mithilfe von Azure-Portal auf den Zustand zu einem bestimmten Zeitpunkt wiederherzustellen, öffnen Sie die Datenbank-Übersichtsseite, und klicken Sie auf der Symbolleiste auf **Wiederherstellen**. Wählen Sie die Sicherungsquelle aus, und wählen Sie den Sicherungspunkt-Zeitpunkt aus, von dem aus eine neue Datenbank erstellt wird. 

  ![point-in-time-restore-single-sql-database](./media/sql-database-recovery-using-backups/pitr-backup-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Um eine verwaltete Instanzdatenbank mithilfe von Azure-Portal auf den Zustand zu einem bestimmten Zeitpunkt wiederherzustellen, öffnen Sie die Datenbank-Übersichtsseite, und klicken Sie auf der Symbolleiste auf **Wiederherstellen**. Wählen Sie den Sicherungspunkt-Zeitpunkt aus, von dem aus eine neue Datenbank erstellt wird. 

  ![point-in-time-restore-managed-instance-database](./media/sql-database-recovery-using-backups/pitr-backup-managed-instance-annotated.png)

> [!TIP]
> Informationen zum programmgesteuerten Wiederherstellen einer Datenbank aus einer Sicherung finden Sie unter [Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups).

## <a name="deleted-database-restore"></a>Wiederherstellen einer gelöschten Datenbank

Sie können eine gelöschte Datenbank mithilfe von Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) oder der [REST-API (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) auf demselben SQL-Datenbank-Server oder in der gleichen verwalteten Instanz auf den Zustand zum Zeitpunkt des Löschens oder früher wiederherstellen. Die Wiederherstellung einer gelöschten Datenbank erfolgt durch das Erstellen einer neuen Datenbank aus der Sicherung.

> [!IMPORTANT]
> Wenn Sie eine Azure SQL-Datenbank-Serverinstanz oder verwaltete Instanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden. Das Wiederherstellen eines gelöschten Servers oder einer gelöschten verwalteten Instanz wird zurzeit nicht unterstützt.

### <a name="deleted-database-restore-using-azure-portal"></a>Wiederherstellen einer gelöschten Datenbank mithilfe des Azure-Portals

Die Wiederherstellung gelöschter Datenbanken aus dem Azure-Portal erfolgt auf der Server- und der Instanzressource.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Zum Wiederherstellen einer gelöschten Einzel- oder Pooldatenbank mithilfe des Azure-Portals öffnen Sie die Serverübersichtsseite, und klicken Sie im Navigationsmenü auf **Gelöschte Datenbanken**. Wählen Sie eine gelöschte Datenbank aus, die Sie wiederherstellen möchten, und geben Sie den Namen für die neue Datenbank ein, die mit den aus der Sicherung wiederhergestellten Daten erstellt wird.

  ![deleted-database-restore](./media/sql-database-recovery-using-backups/restore-deleted-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Zurzeit ist die Option zum Wiederherstellen gelöschter Datenbanken für eine verwaltete Instanz im Azure-Portal nicht verfügbar. Sie können PowerShell verwenden, um eine gelöschte Datenbank auf einer verwalteten Instanz wiederherzustellen. Mehr dazu erfahren Sie unter [Wiederherstellen einer gelöschten Datenbank auf einer verwalteten Instanz mithilfe von PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance).

### <a name="deleted-database-restore-using-powershell"></a>Wiederherstellen einer gelöschten Datenbank mithilfe von PowerShell

Verwenden Sie die unten angegebenen Beispielskripts, um eine gelöschte Datenbank für Azure SQL-Datenbank und eine verwaltete Instanz mithilfe von PowerShell wiederherzustellen.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Ein PowerShell-Beispielskript, das zeigt, wie eine gelöschte Azure SQL-Datenbank wiederhergestellt wird, finden Sie unter [Wiederherstellen einer gelöschten SQL-Datenbank mithilfe von PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Ein PowerShell-Beispielskript, das zeigt, wie eine gelöschte Instanzdatenbank wiederhergestellt wird, finden Sie unter [Wiederherstellen einer gelöschten Datenbank auf einer verwalteten Instanz mithilfe von PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Informationen zum programmgesteuerten Wiederherstellen einer gelöschten Datenbank finden Sie unter [Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups).

## <a name="geo-restore"></a>Geografische Wiederherstellung

Sie können eine SQL-Datenbank auf einem beliebigen Server in einer beliebigen Azure-Region aus den letzten georeplizierten Sicherungen wiederherstellen. Geowiederherstellung verwendet eine georeplizierte Sicherung als Quelle. Sie kann auch dann angefordert werden, wenn aufgrund eines Ausfalls kein Zugriff auf Datenbank oder Rechenzentrum möglich ist.

Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn Ihre Datenbank aufgrund eines Vorfalls in der Hostingregion nicht verfügbar ist. Sie können die Datenbank auf einem Server in einer beliebigen anderen Region wiederherstellen. Es gibt eine Verzögerung zwischen der Erstellung einer Sicherung und der Georeplikation in einem Azure-Blob in einer anderen Region. Daher kann die wiederhergestellte Datenbank bis zu einer Stunde hinter der ursprünglichen Datenbank zurückliegen. Die folgende Abbildung zeigt die Wiederherstellung der Datenbank aus der letzten verfügbaren Sicherung in einer anderen Region.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)

### <a name="geo-restore-using-azure-portal"></a>Durchführen einer Geowiederherstellung mithilfe von Azure-Portal

Das allgemeine Konzept zur Geowiederherstellung einer Datenbank aus Azure-Portal besteht aus dem Erstellen einer neuen Einzeldatenbank oder einer Datenbank der verwalteten Instanz und dem Auswählen einer verfügbaren Geowiederherstellungssicherung im Bildschirm zum Erstellen einer Datenbank. Die neu erstellte Datenbank enthält die mittels Geowiederherstellung wiederhergestellten Sicherungsdaten.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Wenn Sie eine Azure SQL-Einzeldatenbank mittels Geowiederherstellung aus Azure-Portal in der Region und auf dem Server Ihrer Wahl wiederherstellen möchten, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Marketplace auf **+Hinzufügen**, und wählen Sie **SQL-Datenbank erstellen** aus. Geben Sie dann die erforderlichen Informationen auf der Registerkarte **Grundeinstellungen** ein.
2. Wählen Sie die Registerkarte **Zusätzliche Einstellungen** aus.
3. Klicken Sie unter „Vorhandene Daten verwenden“ auf **Sicherung**.
4. Wählen Sie aus der Dropdownliste der verfügbaren Geowiederherstellungssicherungen die gewünschte Sicherung aus.

    ![Geowiederherstellung einer Azure SQL-Einzeldatenbank](./media/sql-database-recovery-using-backups/geo-restore-azure-sql-database-list-annotated.png)

Schließen Sie den Vorgang des Erstellens einer neuen Datenbank ab. Nachdem die Azure SQL-Einzeldatenbank erstellt wurde, enthält sie eine mittels Geowiederherstellung wiederhergestellte Sicherung.

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Wenn Sie eine Datenbank der verwalteten Instanz mittels Geowiederherstellung aus Azure-Portal auf einer vorhandenen verwalteten Instanz in einer Region Ihrer Wahl wiederherstellen möchten, wählen Sie die verwaltete Instanz aus, auf der die Datenbank wiederhergestellt werden soll, und führen Sie die folgenden Schritte aus:

1. Klicken Sie auf **+Neue Datenbank**.
2. Geben Sie den gewünschten Datenbanknamen ein.
3. Wählen Sie unter „Vorhandene Daten verwenden“ die Option **Sicherung** aus.
4. Wählen Sie aus der Dropdownliste der verfügbaren Geowiederherstellungssicherungen die gewünschte Sicherung aus.

    ![Wiederherstellen der Datenbank einer verwalteten Instanz mittels Geowiederherstellung](./media/sql-database-recovery-using-backups/geo-restore-sql-managed-instance-list-annotated.png)

Schließen Sie den Vorgang des Erstellens einer neuen Datenbank ab. Nachdem die Instanzdatenbank erstellt wurde, enthält sie eine mittels Geowiederherstellung wiederhergestellte Sicherung.

### <a name="geo-restore-using-powershell"></a>Durchführen einer Geowiederherstellung mithilfe von PowerShell

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Ein PowerShell-Skript, das zeigt, wie die Geowiederherstellung für eine Azure SQL-Einzeldatenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Einzeldatenbank auf den Zustand zu einem früheren Zeitpunkt mithilfe von PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Ein PowerShell-Skript, das zeigt, wie die Geowiederherstellung für eine Datenbank der verwalteten Instanzdatenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer Datenbank der verwalteten Instanz in einer anderen geografischen Region mithilfe von PowerShell](scripts/sql-managed-instance-restore-geo-backup.md).

### <a name="geo-restore-considerations"></a>Überlegungen zur Geowiederherstellung

Point-in-Time-Wiederherstellung für geosekundäre Datenbanken wird derzeit nicht unterstützt. Point-in-Time-Wiederherstellung kann nur für eine primäre Datenbank erfolgen. Ausführliche Informationen zum Verwenden der Geowiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Die Geowiederherstellung ist die einfachste in SQL-Datenbank verfügbare Lösung für die Notfallwiederherstellung. Sie beruht auf automatisch erstellten georeplizierten Sicherungen mit RPO = 1 Stunde und geschätzter Wiederherstellungszeit von bis zu 12 Stunden. Sie garantiert nicht, dass die Zielregion über die Kapazität zum Wiederherstellen Ihrer Datenbank(en) nach einem regionalen Ausfall verfügt, da der Bedarf voraussichtlich stark ansteigen wird. Für nicht unternehmenskritische Geschäftsanwendungen, die relativ kleine Datenbanken verwenden, ist die Geowiederherstellung eine geeignete Lösung zur Wiederherstellung im Notfall. Verwenden Sie für unternehmenskritische Anwendungen, die große Datenbanken verwenden und die Geschäftskontinuität gewährleisten müssen, [Autofailover-Gruppen](sql-database-auto-failover-group.md) für ein transparentes und koordiniertes Failover mehrerer Datenbanken. Er bietet eine niedrigere RPO und RTO, und die Kapazität ist immer garantiert. Weitere Informationen zur Optionen für Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen

Wie bereits erwähnt, kann die Datenbankwiederherstellung nicht nur über Azure-Portal, sondern auch programmgesteuert mithilfe von Azure PowerShell oder der REST-API durchgeführt werden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell-Azure Resource Manager-Modul wird von Azure SQL-Datenbank zwar weiterhin unterstützt, zukünftig wird jedoch für das Az.Sql-Modul entwickelt. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az-Modul und den AzureRm-Modulen sind in großen Teilen identisch.

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

- Wie Sie eine eigenständige Datenbank oder in einem Pool zusammengefasste Datenbank wiederherstellen, erfahren Sie unter [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | BESCHREIBUNG |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Ruft mindestens eine Datenbank ab. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Ruft eine gelöschte Datenbank ab, die Sie wiederherstellen können. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Ruft eine georedundante Sicherung einer Datenbank ab. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Stellt eine SQL-Datenbank wieder her. |

  > [!TIP]
  > Ein PowerShell-Beispielskript, das zeigt, wie eine Point-in-Time-Wiederherstellung einer Datenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

- Wie Sie die Datenbank einer verwalteten Instanz wiederherstellen, erfahren Sie unter [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | BESCHREIBUNG |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Ruft eine oder mehrere verwaltete Instanzen ab. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Ruft Instanzdatenbanken ab. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Stellt eine Instanzdatenbank wieder her. |

### <a name="rest-api"></a>REST-API

So stellen Sie eine Einzel- oder Pooldatenbank mithilfe der REST-API wieder her

| API | BESCHREIBUNG |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Stellt eine Datenbank wieder her. |
| [Get Create or Update Database Status](https://docs.microsoft.com/rest/api/sql/operations) |Ruft den Status während eines Wiederherstellungsvorgangs ab. |

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

#### <a name="single-azure-sql-database"></a>Azure SQL-Einzeldatenbank

Wie Sie eine Einzel- oder Pooldatenbank mithilfe der Azure CLI wiederherstellen, erfahren Sie unter [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).

#### <a name="managed-instance-database"></a>Datenbank der verwalteten Instanz

Wie Sie eine verwaltete Instanzdatenbank mithilfe der Azure-Befehlszeilenschnittstelle wiederherstellen, erfahren Sie unter [az sql midb restore](/cli/azure/sql/midb#az-sql-midb-restore).

## <a name="summary"></a>Zusammenfassung

Automatische Sicherungen schützen Ihre Datenbanken vor Benutzer- und Anwendungsfehlern, versehentlichen Datenbanklöschungen und längeren Ausfällen. Diese integrierte Funktion ist für alle Dienstebenen und Computegrößen verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Aufbewahrung finden Sie im Artikel [Langfristiges Aufbewahren](sql-database-long-term-retention.md).
- Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-active-geo-replication.md) oder [Autofailover-Gruppen](sql-database-auto-failover-group.md).
