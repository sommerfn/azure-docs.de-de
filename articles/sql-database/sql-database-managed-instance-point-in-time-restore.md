---
title: 'Verwaltete SQL-Datenbank-Instanz: Point-in-Time-Wiederherstellung | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie den Zustand einer Datenbank in einer verwalteten SQL-Instanz zu einem früheren Zeitpunkt wiederherstellen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, mathoma
ms.date: 08/25/2019
ms.openlocfilehash: 67f13d16dcf6bbe4fa13fe3a6e78d3e4d61e1999
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861997"
---
# <a name="restore-a-sql-managed-instance-database-to-a-previous-point-in-time"></a>Wiederherstellen des Zustands einer verwalteten SQL-Datenbank-Instanz zu einem früheren Zeitpunkt

Mit der Point-in-Time-Wiederherstellung (Point-In-Time Restore, PITR) können Sie eine Datenbank als Kopie einer anderen Datenbank in einem früheren Zustand erstellen. In diesem Artikel erfahren Sie, wie Sie eine Point-in-Time-Wiederherstellung einer Datenbank in einer verwalteten Instanz durchführen.

Die Point-in-Time-Wiederherstellung kann in Wiederherstellungsszenarien (etwa bei Vorfällen aufgrund von Fehlern, falsch geladenen Daten, gelöschten wichtigen Daten oder anderen Problemen) sowie zu Test- oder Überprüfungszwecken verwendet werden. Sicherungsdateien werden je nach Datenbankeinstellungen für einen Zeitraum zwischen sieben und 35 Tagen aufbewahrt.

Die Point-in-Time-Wiederherstellung kann für Folgendes verwendet werden:

- Wiederherstellen einer Datenbank auf der Grundlage einer vorhandenen Datenbank
- Wiederherstellen einer Datenbank auf der Grundlage einer gelöschten Datenbank

Im Falle einer verwalteten Instanz kann die Point-in-Time-Wiederherstellung außerdem für Folgendes verwendet werden: 

- Wiederherstellen einer Datenbank in der gleichen verwalteten Instanz
- Wiederherstellen einer Datenbank in einer anderen verwalteten Instanz


> [!NOTE]
> Eine Point-in-Time-Wiederherstellung einer gesamten verwalteten Instanz ist nicht möglich. Sie können aber wie in diesem Artikel beschrieben eine Point-in-Time-Wiederherstellung einer in einer verwalteten Instanz gehosteten Datenbank durchführen.


## <a name="limitations"></a>Einschränkungen

Bei der Wiederherstellung in einer anderen verwalteten Instanz müssen sich beide Instanzen im gleichen Abonnement und in der gleichen Region befinden. Regions- und abonnementübergreifende Wiederherstellungen werden derzeit nicht unterstützt.

> [!WARNING]
> Achten Sie auf die Speichergröße Ihrer verwalteten Instanz: Abhängig von der Größe der Wiederherstellungsdaten steht möglicherweise nicht genügend Instanzspeicher zur Verfügung. Sollte der Speicherplatz für die wiederhergestellten Daten nicht ausreichen, verwenden Sie einen anderen Ansatz.

Die folgende Tabelle enthält Point-in-Time-Wiederherstellungsszenarien für eine verwaltete Instanz:

|           |Wiederherstellen einer vorhandenen Datenbank| Wiederherstellen einer vorhandenen Datenbank|Wiederherstellen einer gelöschten Datenbank| Wiederherstellen einer gelöschten Datenbank|
|:----------|:----------|:----------|:----------|:----------|
|Ziel| Gleiche verwaltete Instanz|Andere verwaltete Instanz |Gleiche verwaltete Instanz|Andere verwaltete Instanz |
|Azure-Portal| Ja|Nein |Nein|Nein|
|Azure-Befehlszeilenschnittstelle|Ja |Ja |Nein|Nein|
|PowerShell| Ja|Ja |Ja|Ja|


## <a name="restore-existing-database"></a>Wiederherstellen einer vorhandenen Datenbank

Eine vorhandene Datenbank kann über das Azure-Portal, mithilfe von PowerShell oder über die Azure CLI in der gleichen Instanz wiederhergestellt werden. Wenn Sie eine Datenbank in einer anderen Instanz wiederherstellen möchten, verwenden Sie PowerShell oder die Azure CLI, und geben Sie die verwaltete Zielinstanz sowie die Ressourcengruppeneigenschaften an. Ohne Angabe dieser Parameter wird die Datenbank standardmäßig in der vorhandenen Instanz wiederhergestellt. Über das Azure-Portal kann derzeit keine Wiederherstellung in einer anderen Instanz durchgeführt werden. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihrer verwalteten Instanz, und wählen Sie die wiederherzustellende Datenbank aus. 
1. Wählen Sie auf der Datenbankseite die Option **Wiederherstellen** aus. 

    ![Wiederherstellen einer vorhandenen Datenbank](media/sql-database-managed-instance-point-in-time-restore/restore-database-to-mi.png)

1. Wählen Sie auf der Seite **Wiederherstellen** den Zeitpunkt in der Vergangenheit (Datum und Uhrzeit) für die Datenbankwiederherstellung aus.
1. Wählen Sie **Bestätigen** aus, um Ihre Datenbank wiederherzustellen. Daraufhin wird der Wiederherstellungsprozess gestartet. Hierbei wird eine neue Datenbank erstellt und mit Daten aus der ursprünglichen Datenbank zum gewünschten Zeitpunkt aufgefüllt. Weitere Informationen zum Wiederherstellungsprozess finden Sie unter [Wiederherstellungszeit](sql-database-recovery-using-backups.md#recovery-time). 

1. Suchen Sie nach der verwalteten Instanz.
1. Wählen Sie die Datenbank aus, die Sie wiederherstellen möchten.
1. Klicken Sie im Datenbankbildschirm auf die Aktion „Wiederherstellen“.
1. Wählen Sie im Bildschirm „Wiederherstellen“ den Zeitpunkt in der Vergangenheit (Datum und Uhrzeit) für die Datenbankwiederherstellung aus.
1. Nach einer Bestätigung wird der Wiederherstellungsprozess gestartet. Dabei wird abhängig von der Größe der Datenbank eine neue Datenbank erstellt und mit Daten aus der ursprünglichen Datenbank zum gewünschten Zeitpunkt aufgefüllt. Informationen zur Dauer des Wiederherstellungsprozesses finden Sie im Artikel zu Wiederherstellungen mit Sicherungen.


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Sollte Azure PowerShell noch nicht installiert sein, lesen Sie [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps).

Wenn Sie die Datenbank unter Verwendung von PowerShell wiederherstellen möchten, aktualisieren Sie die Parameter mit Ihren Werten, und führen Sie den folgenden Befehl aus:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source-database>"
$pointInTime = "2018-06-27T08:51:39.3882806Z"
$targetDatabase = "<Name of new database to be created>"
 
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId
 
Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                               -InstanceName $managedInstanceName `
                               -Name $databaseName `
                               -PointInTime $pointInTime `
                               -TargetInstanceDatabaseName $targetDatabase `
```

Soll die Datenbank in einer anderen verwalteten Instanz wiederhergestellt werden, legen Sie den Namen der Zielressourcengruppe und den Namen der verwalteten Zielinstanz fest.  

```powershell-interactive
$targetResourceGroupName = "<Resource group of target managed instance>"
$targetInstanceName = "<Target managed instance name>"

Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
                              -ResourceGroupName $resourceGroupName `
                              -InstanceName $managedInstanceName `
                              -Name $databaseName `
                              -PointInTime $pointInTime `
                              -TargetInstanceDatabaseName $targetDatabase `
                              -TargetResourceGroupName $targetResourceGroupName `
                              -TargetInstanceName $targetInstanceName 
```

Ausführliche Informationen finden Sie unter [Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase).


# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sollte die Azure CLI noch nicht installiert sein, lesen Sie [Installieren der Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

Wenn Sie die Datenbank unter Verwendung der Azure CLI wiederherstellen möchten, aktualisieren Sie die Parameter mit Ihren Werten, und führen Sie den folgenden Befehl aus:


```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename |
-n mymanageddbname --dest-name targetmidbname --time "2018-05-20T05:34:22"
```


Soll die Datenbank in einer anderen verwalteten Instanz wiederhergestellt werden, legen Sie den Namen der Zielressourcengruppe und den Namen der verwalteten Zielinstanz fest.  

```azurecli-interactive
az sql midb restore -g mygroupname --mi myinstancename -n mymanageddbname |
       --dest-name targetmidbname --time "2018-05-20T05:34:22" |
       --dest-resource-group mytargetinstancegroupname |
       --dest-mi mytargetinstancename
```

Eine ausführliche Erläuterung der verfügbaren Parameter finden Sie unter [az sql midb restore](https://docs.microsoft.com/cli/azure/sql/midb?view=azure-cli-latest#az-sql-midb-restore). 

---

## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank 
 
Gelöschte Datenbanken können nur über PowerShell wiederhergestellt werden. Die Datenbank kann in der gleichen oder in einer anderen Instanz wiederhergestellt werden. 

Wenn Sie eine gelöschte Datenbank unter Verwendung von PowerShell wiederherstellen möchten, aktualisieren Sie die Parameter mit Ihren Werten, und führen Sie den folgenden Befehl aus:

```powershell-interactive
$subscriptionId = "<Subscription ID>"
Get-AzSubscription -SubscriptionId $subscriptionId
Select-AzSubscription -SubscriptionId $subscriptionId

$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$deletedDatabaseName = "<Source database name>"

$deleted_db = Get-AzSqlDeletedInstanceDatabaseBackup -ResourceGroupName $resourceGroupName `
            -InstanceName $managedInstanceName -DatabaseName $deletedDatabaseName 

$pointInTime = "2018-06-27T08:51:39.3882806Z"
$properties = New-Object System.Object
$properties | Add-Member -type NoteProperty -name CreateMode -Value "PointInTimeRestore"
$properties | Add-Member -type NoteProperty -name RestorePointInTime -Value $pointInTime
$properties | Add-Member -type NoteProperty -name RestorableDroppedDatabaseId -Value $deleted_db.Id
```

Soll die gelöschte Datenbank in einer anderen Instanz wiederhergestellt werden, ändern Sie den Namen der Ressourcengruppen und den Namen der verwalteten Instanz.

Der location-Parameter muss dem Standort der Ressourcengruppe und der verwalteten Instanz entsprechen.

```powershell-interactive
$resourceGroupName = "<Second resource group name>"
$managedInstanceName = "<Second managed instance name>"

$location = "West Europe"

$restoredDBName = "WorldWideImportersPITR"
$resource_id = "subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/managedInstances/$managedInstanceName/databases/$restoredDBName"

New-AzResource -Location $location -Properties $properties `
        -ResourceId $resource_id -ApiVersion "2017-03-01-preview" -Force
```

## <a name="overwrite-existing-database"></a>Überschreiben der vorhandenen Datenbank 
 
Wenn Sie eine vorhandene Datenbank überschreiben möchten, ist außerdem Folgendes erforderlich:

1. Löschen Sie die vorhandene Datenbank, die Sie überschreiben möchten, mithilfe von „DROP“.
1. Legen Sie den Namen der mittels Point-in-Time-Wiederherstellung wiederhergestellten Datenbank auf den Namen der gelöschten Datenbank fest. 


### <a name="drop-original-database"></a>Löschen der ursprünglichen Datenbank mithilfe von „DROP“ 
 
Die Datenbank kann über das Azure-Portal, mithilfe von PowerShell oder über die Azure CLI gelöscht werden. 

Sie können die Datenbank auch löschen, indem Sie eine direkte Verbindung mit der verwalteten Instanz herstellen, SQL Server Management Studio (SSMS) starten und den folgenden T-SQL-Befehl (Transact-SQL) ausführen:

```sql
DROP DATABASE WorldWideImporters;
```

Verwenden Sie eine der folgenden Methoden, um eine Verbindung mit der Datenbank der verwalteten Instanz herzustellen: 

- [Virtueller SQL-Computer](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Point-to-Site](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Öffentlicher Endpunkt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Wählen Sie im Azure-Portal die Datenbank der verwalteten Instanz und anschließend **Löschen** aus.

   ![Wiederherstellen einer vorhandenen Datenbank](media/sql-database-managed-instance-point-in-time-restore/delete-database-from-mi.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Verwenden Sie den folgenden PowerShell-Befehl, um eine vorhandene Datenbank aus einer verwalteten Instanz zu löschen: 

```powershell
$resourceGroupName = "<Resource group name>"
$managedInstanceName = "<Managed instance name>"
$databaseName = "<Source database>"

Remove-AzSqlInstanceDatabase -Name $databaseName -InstanceName $managedInstanceName -ResourceGroupName $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den folgenden Azure CLI-Befehl, um eine vorhandene Datenbank aus einer verwalteten Instanz zu löschen: 

```azurecli-interactive
az sql midb delete -g mygroupname --mi myinstancename -n mymanageddbname
```

---


### <a name="alter-new-database-name-to-original"></a>Ändern des Namens der neuen Datenbank in den ursprünglichen Namen mithilfe von „ALTER“

Stellen Sie eine direkte Verbindung mit der verwalteten Instanz her, starten Sie SQL Server Management Studio, und führen Sie die folgende T-SQL-Abfrage (Transact-SQL) aus, um den Namen der wiederhergestellten Datenbank in den Namen der gelöschten Datenbank zu ändern, die Sie überschreiben möchten: 


```sql
ALTER WorldWideImportersPITR MODIFY NAME = WorldWideImporters;
```


Verwenden Sie eine der folgenden Methoden, um eine Verbindung mit der Datenbank der verwalteten Instanz herzustellen: 

- [Virtueller SQL-Computer](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vm)
- [Point-to-Site](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-p2s)
- [Öffentlicher Endpunkt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Langzeitaufbewahrung](sql-database-long-term-retention.md) und [automatisierte Sicherungen](sql-database-automated-backups.md). 
