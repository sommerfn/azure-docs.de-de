---
title: Verwalten der langfristigen Aufbewahrung von Sicherungen
description: Hier erfahren Sie, wie Sie automatisierte Sicherungen im SQL Azure-Speicher speichern und dann wiederherstellen.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 08/21/2019
ms.openlocfilehash: ea9a1da775a64f8ee405ced52df01d0824836c42
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820018"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Verwalten der langfristigen Aufbewahrung von Sicherungen in Azure SQL-Datenbank

In Azure SQL-Datenbank können Sie eine Einzel- oder Pooldatenbank mit einer Richtlinie für die [Langzeitaufbewahrung](sql-database-long-term-retention.md) (Long-Term Retention, LTR) konfigurieren, um die Datenbanksicherungen automatisch in separaten Azure Blob Storage-Containern bis zu 10 Jahre lang aufzubewahren. Sie können dann mit dem Azure-Portal oder mit PowerShell eine Datenbank anhand dieser Sicherungen wiederherstellen.

> [!IMPORTANT]
> Die [verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance.md) unterstützt derzeit keine langfristigen Aufbewahrung von Sicherungen.

## <a name="use-the-azure-portal-to-manage-long-term-backups"></a>Verwenden des Azure-Portals zum Verwalten langfristiger Sicherungen

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe des Azure-Portals die langfristige Aufbewahrung konfigurieren, Sicherungen in der langfristigen Aufbewahrung anzeigen und Sicherungen aus der langfristigen Aufbewahrung wiederherstellen.

### <a name="configure-long-term-retention-policies"></a>Konfigurieren von Richtlinien für die langfristige Aufbewahrung

Sie können SQL-Datenbank [zur Aufbewahrung von automatisierten Sicherungen](sql-database-long-term-retention.md) für einen längeren Zeitraum konfigurieren, als gemäß der Aufbewahrungsdauer für Ihre Dienstebene vorgesehen ist. 

1. Wählen Sie im Azure-Portal Ihren SQL Server aus, und klicken Sie dann auf **Sicherungen verwalten**. *Aktivieren Sie auf der Registerkarte **Richtlinien konfigurieren** das Kontrollkästchen für die Datenbank, für die Sie Richtlinien für die langfristige Sicherungsaufbewahrung festlegen oder ändern möchten.* Wenn das Kontrollkästchen neben der Datenbank nicht aktiviert ist, gelten die Änderungen für die Richtlinie nicht für diese Datenbank.  

   ![Link zum Verwalten von Sicherungen](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Wählen Sie im Bereich **Configure policies** (Richtlinien konfigurieren) aus, ob Sie wöchentliche, monatliche oder jährliche Sicherungen aufbewahren möchten, und geben Sie den jeweiligen Aufbewahrungszeitraum an. 

   ![Konfigurieren von Richtlinien](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Klicken Sie auf **Anwenden**, wenn Sie fertig sind.

> [!IMPORTANT]
> Wenn Sie eine Richtlinie für die Langzeitaufbewahrung von Sicherungen aktivieren, kann es bis zu 7 Tage dauern, bevor die erste Sicherung angezeigt wird und wiederhergestellt werden kann. Weitere Informationen dazu, in welchen Intervallen Sicherungskopien für die Langzeitaufbewahrung erstellt werden, finden Sie unter [Langzeitaufbewahrung von Sicherungen](sql-database-long-term-retention.md).

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Anzeigen von Sicherungen und Wiederherstellen einer Sicherung mithilfe des Azure-Portals

Zeigen Sie die Sicherungen an, die für eine bestimmte Datenbank mit einer LTR-Richtlinie aufbewahrt werden, und stellen Sie diese Sicherungen wieder her. 

1. Wählen Sie im Azure-Portal Ihren SQL Server aus, und klicken Sie dann auf **Sicherungen verwalten**. Wählen Sie auf der Registerkarte **Verfügbare Sicherungen** die Datenbank aus, für die Sie verfügbare Sicherungen anzeigen möchten.

   ![Auswählen der Datenbank](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. Überprüfen Sie im Bereich **Verfügbare Sicherungen** die verfügbaren Sicherungen. 

   ![Anzeigen von Sicherungen](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Wählen Sie die Sicherung aus, die Sie wiederherstellen möchten, und geben Sie dann den neuen Datenbanknamen an.

   ![Wiederherstellen](./media/sql-database-long-term-retention/ltr-restore.png)

5. Klicken Sie auf **OK**, um die Datenbank auf Grundlage der Sicherung im Azure SQL-Speicher in der neuen Datenbank wiederherzustellen.

6. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

   ![Status des Wiederherstellungsauftrags](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags die Seite **SQL-Datenbanken**, um die neu wiederhergestellte Datenbank anzuzeigen.

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise [einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-manage-long-term-backups"></a>Verwenden von PowerShell zum Verwalten langfristiger Sicherungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az- und den AzureRm-Modulen sind im Wesentlichen identisch.

In den folgenden Abschnitten wird erläutert, wie Sie mithilfe von PowerShell die langfristige Sicherungsaufbewahrung konfigurieren, Sicherungen in Azure SQL-Speicher anzeigen und eine Sicherung in Azure SQL-Speicher wiederherstellen.


### <a name="rbac-roles-to-manage-long-term-retention"></a>RBAC-Rollen zum Verwalten der langfristigen Aufbewahrung

Für **Get-AzSqlDatabaseLongTermRetentionBackup** und **Restore-AzSqlDatabase** müssen Sie über eine der folgenden Rollen verfügen:

- Rolle „Besitzer des Abonnements“ oder
- Rolle „Mitwirkender von SQL Server“ oder
- Benutzerdefinierte Rolle mit den folgenden Berechtigungen:

   Microsoft.Sql/locations/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read
 
Für **Remove-AzSqlDatabaseLongTermRetentionBackup** müssen Sie über eine der folgenden Rollen verfügen:

- Rolle „Besitzer des Abonnements“ oder
- Benutzerdefinierte Rolle mit der folgenden Berechtigung:

   Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete


> [!NOTE]
> Die Rolle „Mitwirkender von SQL Server“ besitzt keine Berechtigung zum Löschen von LTR-Sicherungen.

RBAC-Berechtigungen können im Bereich *Abonnement* oder *Ressourcengruppe* erteilt werden. Für den Zugriff auf LTR-Sicherungen, die zu einem gelöschten Server gehören, muss die Berechtigung jedoch im Bereich *Abonnement* dieses Servers erteilt werden.


### <a name="create-an-ltr-policy"></a>Erstellen einer LTR-Richtlinie

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subId

# get the server
$server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Anzeigen von LTR-Richtlinien
In diesem Beispiel wird gezeigt, wie Sie die LTR-Richtlinien auf einem Server auflisten.

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Löschen einer LTR-Richtlinie
In diesem Beispiel wird gezeigt, wie Sie eine LTR-Richtlinie aus einer Datenbank löschen.

```powershell
Set-AzSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Anzeigen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie die LTR-Sicherungen auf einem Server auflisten. 

```powershell
# List all LTR backups under the current subscription in a specific Azure region 
# The list includes backups for existing servers and dropped servers grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest backup first.
# Requires Subscription scope permission
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location 

# List the LTR backups under a specific resource group in a specific Azure region 
# The list includes backups from the existing servers only grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest backup first. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ResourceGroupName $resourceGroup

# List the LTR backups under an existing server
# The list includes backups from the existing servers only grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest backup first. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ResourceGroupName $resourceGroup -ServerName $serverName

# List the LTR backups for a specific database 
# The backups are ordered by the timestamp, the earliest backup first. 
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
# The list includes backups for existing servers and dropped servers grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest backup first.  
# Requires Subscription scope permission
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database under a server
$ltrBackups = Get-AzSqlDatabaseLongTermRetentionBackup -Location $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Löschen von LTR-Sicherungen

Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung aus der Liste der Sicherungen löschen.

```powershell
# Remove the earliest backup from the list of backups
$ltrBackup = $ltrBackups[0]
Remove-AzSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```
> [!IMPORTANT]
> Das Löschen der LTR-Sicherung kann nicht rückgängig gemacht werden. Wenn Sie eine LTR-Sicherung löschen möchten, nachdem der Server gelöscht wurde, müssen Sie über die Berechtigung im Bereich „Abonnement“ verfügen. Sie können Benachrichtigungen zu jedem Löschvorgang in Azure Monitor einrichten, indem Sie nach dem Vorgang „Löscht eine Sicherung zur langfristigen Aufbewahrung“ filtern. Das Aktivitätsprotokoll enthält Informationen dazu, wer die Anforderung vorgenommen hat und wann sie erfolgt ist. Ausführliche Anweisungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../azure-monitor/platform/alerts-activity-log.md).
>

### <a name="restore-from-ltr-backups"></a>Wiederherstellen von LTR-Sicherungen
Dieses Beispiel zeigt, wie Sie eine LTR-Sicherung wiederherstellen. Beachten Sie, dass sich diese Schnittstelle nicht geändert hat, der Parameter für die Ressourcen-ID jetzt jedoch die Ressourcen-ID der LTR-Sicherung erfordert. 

```powershell
# Restore a specific LTR backup as an P1 database on the server $serverName of the resource group $resourceGroup 
Restore-AzSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName P1
```

> [!IMPORTANT]
> Um die Wiederherstellung aus einer LTR-Sicherung nach dem Löschen des Servers durchführen zu können, müssen Sie über Berechtigungen im Bereich des Abonnements des Servers verfügen, und dieses Abonnement muss aktiv sein. Sie müssen außerdem den optionalen Parameter -ResourceGroupName auslassen.  
>

> [!NOTE]
> Auf diesem Blatt können Sie mithilfe von SQL Server Management Studio eine Verbindung mit der wiederhergestellten Datenbank herstellen, um erforderliche Aufgaben durchzuführen. Sie können beispielsweise einen Teil der Daten aus der wiederhergestellten Datenbank extrahieren und in die vorhandene Datenbank kopieren oder die vorhandene Datenbank löschen und die wiederhergestellte Datenbank in den vorhandenen Datenbanknamen umbenennen. Siehe [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
