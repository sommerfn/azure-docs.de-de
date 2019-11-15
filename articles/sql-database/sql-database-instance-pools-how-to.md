---
title: Schrittanleitung zu Instanzenpools (Vorschauversion)
description: In diesem Artikel wird beschrieben, wie Sie Azure SQL-Datenbank-Instanzenpools (Vorschau) erstellen und verwalten.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: 13c58ddf5f51e5b63d2dbe425b3ec795e21dabb8
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73810347"
---
# <a name="azure-sql-database-instance-pools-preview-how-to-guide"></a>Schrittanleitung zu Azure SQL-Datenbank-Instanzenpools (Vorschau)

Dieser Artikel enthält ausführliche Informationen zum Erstellen und Verwalten von [Instanzenpools](sql-database-instance-pools.md).

## <a name="instance-pool-operations"></a>Vorgänge für Instanzenpools

In der folgenden Tabelle sind die verfügbaren Vorgänge im Zusammenhang mit Instanzenpools und deren Verfügbarkeit im Azure-Portal und in PowerShell aufgeführt.

|Get-Help|Azure-Portal|PowerShell|
|:---|:---|:---|
|Instanzenpool erstellen|Nein|Ja|
|Instanzenpool aktualisieren (begrenzte Anzahl von Eigenschaften)|Nein |Ja |
|Verwendung und Eigenschaften von Instanzenpools überprüfen|Nein|Ja |
|Instanzenpool löschen|Nein|Ja|
|Verwaltete Instanz in Instanzenpool erstellen|Nein|Ja|
|Ressourcennutzung verwalteter Instanzen aktualisieren|Ja |Ja|
|Verwendung und Eigenschaften verwalteter Instanzen überprüfen|Ja|Ja|
|Verwaltete Instanz aus dem Pool löschen|Ja|Ja|
|Datenbank in einer im Pool abgelegten verwalteten Instanz erstellen|Ja|Ja|
|Datenbank aus verwalteter Instanz löschen|Ja|Ja|

Verfügbare [PowerShell-Befehle](https://docs.microsoft.com/powershell/module/az.sql/)

|Cmdlet |BESCHREIBUNG |
|:---|:---|
|[New-AzSqlInstancePool](/powershell/module/az.sql/new-azsqlinstancepool/) | Erstellt einen Azure SQL-Datenbank-Instanzenpool |
|[Get-AzSqlInstancePool](/powershell/module/az.sql/get-azsqlinstancepool/) | Gibt Informationen zum Azure SQL-Instanzenpool zurück |
|[Set-AzSqlInstancePool](/powershell/module/az.sql/set-azsqlinstancepool/) | Legt Eigenschaften für einen Azure SQL-Datenbank-Instanzenpool fest |
|[Remove-AzSqlInstancePool](/powershell/module/az.sql/remove-azsqlinstancepool/) | Entfernt einen Azure SQL-Datenbank-Instanzenpool |
|[Get-AzSqlInstancePoolUsage](/powershell/module/az.sql/get-azsqlinstancepoolusage/) | Gibt Informationen zur Verwendung von Azure SQL-Instanzenpools zurück |


Zur Verwendung von PowerShell [installieren Sie die neueste Version von PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell#powershell-core), und befolgen Sie die Anweisungen zum [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps).

Verwenden Sie für Vorgänge für Instanzen innerhalb von Pools sowie für Einzelinstanzen die [Standardbefehle für verwaltete Instanzen](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances). Wenn diese Befehle für eine Instanz in einem Pool verwendet werden, muss allerdings der *Name des Instanzenpools* angegeben werden.

## <a name="how-to-deploy-managed-instances-into-pools"></a>Bereitstellen von verwalteten Instanzen in Pools

Die Bereitstellung einer Instanz in einem Pool erfolgt mit den folgenden zwei Schritten:

1. Einmalige Bereitstellung des Instanzenpools: Dabei handelt es sich um einen zeitintensiven Vorgang, bei dem die Dauer der Bereitstellung einer [in einem leeren Subnetz erstellten Einzelinstanz](sql-database-managed-instance.md#managed-instance-management-operations) entspricht.

2. Wiederholte Bereitstellung der Instanz in einem Instanzenpool: Der Instanzenpoolparameter muss als Teil dieses Vorgangs explizit angegeben werden. Dies ist ein relativ schneller Vorgang, der in der Regel maximal 5 Minuten dauert.

In der Public Preview werden beide Schritte nur unter Verwendung von PowerShell und Resource Manager-Vorlagen unterstützt. Das Azure-Portal steht dafür derzeit nicht zur Verfügung.

Nach dem Bereitstellen einer verwalteten Instanz in einem Pool *können* Sie über das Azure-Portal die zugehörigen Eigenschaften auf der Seite mit den Tarifen ändern.


## <a name="create-an-instance-pool"></a>Erstellen eines Instanzenpools

So erstellen Sie einen Instanzenpool

1. [Erstellen eines virtuellen Netzwerks mit einem Subnetz](#create-a-virtual-network-with-a-subnet)
2. [Erstellen eines Instanzenpools](#create-an-instance-pool)


### <a name="create-a-virtual-network-with-a-subnet"></a>Erstellen eines virtuellen Netzwerks mit einem Subnetz 

Informationen zum Platzieren mehrerer Instanzenpools innerhalb desselben virtuellen Netzwerks finden Sie in den folgenden Artikeln:

- [Bestimmen der VNET-/Subnetzgröße für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-determine-size-vnet-subnet.md)
- Erstellen Sie ein neues virtuelles Netzwerk und Subnetz mithilfe der [Vorlage im Azure-Portal](sql-database-managed-instance-create-vnet-subnet.md), oder befolgen Sie die Anweisungen zum [Vorbereiten eines vorhandenen virtuellen Netzwerks](sql-database-managed-instance-configure-vnet-subnet.md).
 


### <a name="create-an-instance-pool"></a>Erstellen eines Instanzenpools 

Nach Ausführen der vorherigen Schritte können Sie einen Instanzenpool erstellen.

Für Instanzenpools gelten die folgenden Einschränkungen:

- In der Public Preview sind nur die Optionen „Universell“ und „Gen5“ verfügbar.
- Der Poolname darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten, und er darf nicht mit einem Bindestrich beginnen.
- Verwenden Sie `Get-AzVirtualNetworkSubnetConfig -Name "miPoolSubnet" -VirtualNetwork $virtualNetwork`, um die Subnetz-ID abzurufen.
- Wenn Sie den Azure-Hybridvorteil (AHB) nutzen möchten, wird er auf Ebene des Instanzenpools angewandt. Sie können den Lizenztyp während der Poolerstellung festlegen oder jederzeit nach der Erstellung aktualisieren.

> [!IMPORTANT]
> Das Bereitstellen eines Instanzenpools ist ein zeitintensiver Vorgang und dauert ungefähr 4,5 Stunden.

So erstellen Sie einen Instanzenpool

```powershell
$instancePool = New-AzSqlInstancePool `
  -ResourceGroupName "myResourceGroup" `
  -Name "mi-pool-name" `
  -SubnetId "/subscriptions/subscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/miPoolVirtualNetwork/subnets/miPoolSubnet" `
  -LicenseType "LicenseIncluded" `
  -VCore 80 `
  -Edition "GeneralPurpose" `
  -ComputeGeneration "Gen5" `
  -Location "westeurope"
```

> [!IMPORTANT]
> Da die Bereitstellung eines Instanzenpools ein zeitintensiver Vorgang ist, müssen Sie warten, bis er abgeschlossen ist, bevor Sie die nachfolgenden Schritte in diesem Artikel ausführen.

## <a name="create-a-managed-instance-inside-the-pool"></a>Erstellen einer verwalteten Instanz innerhalb des Pools 

Nach der erfolgreichen Bereitstellung des Instanzenpools kann nun eine Instanz darin erstellt werden.

Führen Sie zum Erstellen einer verwalteten Instanz den folgenden Befehl aus:

```powershell
$instanceOne = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 2 -StorageSizeInGB 256
```

Das Bereitstellen einer Instanz innerhalb eines Pools dauert einige Minuten. Nachdem die erste Instanz erstellt wurde, können weitere Instanzen erstellt werden:

```powershell
$instanceTwo = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 4 -StorageSizeInGB 512
```

## <a name="create-a-database-inside-an-instance"></a>Erstellen einer Datenbank innerhalb einer Instanz 

Zum Erstellen und Verwalten von Datenbanken in einer verwalteten Instanz, die sich in einem Pool befindet, verwenden Sie die Befehle für Einzelinstanzen.

So erstellen Sie eine Datenbank innerhalb einer verwalteten Instanz

```powershell
$poolinstancedb = New-AzSqlInstanceDatabase -Name "mipooldb1" -InstanceName "poolmi-001" -ResourceGroupName "myResourceGroup"
```


## <a name="get-instance-pool-usage"></a>Abrufen der Verwendung eines Instanzenpools 
 
So rufen Sie eine Liste von Instanzen innerhalb eines Pools ab

```powershell
$instancePool | Get-AzSqlInstance
```


So rufen Sie die Ressourcennutzung des Pools ab

```powershell
$instancePool | Get-AzSqlInstancePoolUsage
```


So rufen Sie die detaillierte Nutzungsübersicht über den Pool und die darin enthaltenen Instanzen ab

```powershell
$instancePool | Get-AzSqlInstancePoolUsage –ExpandChildren
```

So listen Sie die Datenbanken in einer Instanz auf

```powershell
$databases = Get-AzSqlInstanceDatabase -InstanceName "pool-mi-001" -ResourceGroupName "resource-group-name"
```


> [!NOTE]
> Pro Pool (nicht pro Instanz) können maximal 100 Datenbanken erstellt werden.


## <a name="scale-a-managed-instance-inside-a-pool"></a>Skalieren einer verwalteten Instanz innerhalb eines Pools 


Nachdem Sie eine verwaltete Instanz mit Datenbanken gefüllt haben, erreichen Sie möglicherweise Instanzengrenzwerte in Bezug auf die Speicherung oder Leistung. Wenn in diesem Fall die Poolverwendung nicht überschritten wurde, können Sie die Instanz skalieren.
Die Skalierung einer verwalteten Instanz innerhalb eines Pools dauert einige Minuten. Voraussetzung für die Skalierung sind verfügbare virtuelle Kerne und verfügbarer Speicher auf Ebene des Instanzenpools.

So aktualisieren Sie die Anzahl der virtuellen Kerne und die Speichergröße

```powershell
$instanceOne | Set-AzSqlInstance -VCore 8 -StorageSizeInGB 512 -InstancePoolName "mi-pool-name"
```


So aktualisieren Sie nur die Speichergröße

```powershell
$instance | Set-AzSqlInstance -StorageSizeInGB 1024 -InstancePoolName "mi-pool-name"
```



## <a name="connect-to-a-managed-instance-inside-a-pool"></a>Herstellen einer Verbindung mit einer verwalteten Instanz innerhalb eines Pools

Zum Herstellen einer Verbindung mit einer verwalteten Instanz in einem Pool sind die folgenden zwei Schritte erforderlich:

1. [Aktivieren des öffentlichen Endpunkts für die Instanz](#enable-the-public-endpoint-for-the-instance)
2. [Hinzufügen einer Eingangsregel zur Netzwerksicherheitsgruppe (NSG)](#add-an-inbound-rule-to-the-network-security-group)

Nach Ausführung beider Schritte können Sie unter Verwendung der Adresse, des Ports und der Anmeldeinformationen des öffentlichen Endpunkts, die bei der Erstellung der Instanz angegeben wurden, eine Verbindung mit der Instanz herstellen. 

### <a name="enable-the-public-endpoint-for-the-instance"></a>Aktivieren des öffentlichen Endpunkts für die Instanz

Die Aktivierung des öffentlichen Endpunkts für eine Instanz kann über das Azure-Portal oder den folgenden PowerShell-Befehl erfolgen:


```powershell
$instanceOne | Set-AzSqlInstance -InstancePoolName "pool-mi-001" -PublicDataEndpointEnabled $true
```

Auch dieser Parameter kann bei der Erstellung der Instanz festgelegt werden.

### <a name="add-an-inbound-rule-to-the-network-security-group"></a>Hinzufügen einer Eingangsregel zur Netzwerksicherheitsgruppe 

Dieser Schritt kann über das Azure-Portal oder mithilfe von PowerShell-Befehlen ausgeführt werden. Er kann jederzeit erfolgen, nachdem das Subnetz für die verwaltete Instanz vorbereitet wurde.

Ausführliche Informationen finden Sie unter [Zulassen von Datenverkehr auf dem öffentlichen Endpunkt in der Netzwerksicherheitsgruppe](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group).


## <a name="move-an-existing-single-instance-inside-an-instance-pool"></a>Verschieben einer vorhandenen Einzelinstanz in einem Instanzenpool 
 
Das Verschieben von Instanzen in und aus einem Pool ist eine der Einschränkungen der Public Preview. Eine mögliche Problemumgehung basiert auf der Point-in-Time-Wiederherstellung von Datenbanken einer Instanz außerhalb eines Pools in einer Instanz, die sich bereits in einem Pool befindet. 

Beide Instanzen müssen sich im selben Abonnement und derselben Region befinden. Die regions- und abonnementübergreifende Wiederherstellung wird derzeit nicht unterstützt.

Dieser Vorgang bringt eine gewisse Ausfallzeit mit sich.

So verschieben Sie vorhandene Datenbanken

1. Halten Sie Workloads in der verwalteten Instanz an, von der aus die Migration durchgeführt wird.
2. Generieren Sie Skripts zum Erstellen von Systemdatenbanken, und führen Sie sie in der Instanz aus, die sich im Instanzenpool befindet.
3. Führen Sie eine Point-in-Time-Wiederherstellung der einzelnen Datenbanken der Einzelinstanz in der Instanz im Pool durch.

    ```powershell
    $resourceGroupName = "my resource group name"
    $managedInstanceName = "my managed instance name"
    $databaseName = "my source database name"
    $pointInTime = "2019-08-21T08:51:39.3882806Z"
    $targetDatabase = "name of the new database that will be created"
    $targetResourceGroupName = "resource group of instance pool"
    $targetInstanceName = "pool instance name"
       
    Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
      -ResourceGroupName $resourceGroupName `
      -InstanceName $managedInstanceName `
      -Name $databaseName `
      -PointInTime $pointInTime `
      -TargetInstanceDatabaseName $targetDatabase `
      -TargetResourceGroupName $targetResourceGroupName `
      -TargetInstanceName $targetInstanceName
    ```

4. Richten Sie in der Anwendung einen Verweis auf die neue Instanz ein, und setzen Sie die zugehörigen Workloads fort.

Wiederholen Sie bei mehreren Datenbanken den Vorgang für jede Datenbank.


## <a name="next-steps"></a>Nächste Schritte

- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md).
- Eine Schnellstartanleitung, in der eine verwaltete Instanz erstellt und eine Datenbank von einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md).
- Informationen zur erweiterten Überwachung der Datenbankleistung verwalteter Instanzen mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../azure-monitor/insights/azure-sql.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).
