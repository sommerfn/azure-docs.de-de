---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 838037804baad9105b4636934de957c2e5f3e810
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612067"
---
# <a name="using-azure-ultra-disks"></a>Verwenden von Azure Ultra-Datenträgern

Azure Ultra-Datenträger bieten hohen Durchsatz, einen hohen IOPS-Wert und einen Datenträgerspeicher mit durchgängig geringer Latenz für virtuelle Azure IaaS-Computer (VMs). Dieses neue Angebot bietet Spitzenleistung auf den gleichen Verfügbarkeitsebenen wie unsere vorhandenen Datenträgerangebote. Ein Hauptvorteil von Ultra-Datenträgern ist die Möglichkeit zum dynamischen Ändern der SSD-Leistung zusammen mit Ihren Workloads, ohne dass Sie Ihre VMs neu starten müssen. Ultra-Datenträger eignen sich für datenintensive Workloads wie SAP HANA, führende Datenbanksysteme und Workloads mit vielen Transaktionen.

## <a name="ga-scope-and-limitations"></a>Umfang und Einschränkungen für allgemeine Verfügbarkeit

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]

## <a name="determine-vm-size-and-region-availability"></a>Ermitteln der VM-Größe und Regionsverfügbarkeit

Sie müssen ermitteln, in welcher Verfügbarkeitszone Sie sich befinden, um Ultra-Datenträger verwenden zu können. Nicht jede Region unterstützt jede VM-Größe bei Ultra-Datenträgern. Führen Sie einen der folgenden Befehle aus, um zu ermitteln, ob Ihre Region, Zone und VM-Größe Ultra-Datenträger unterstützt. Ersetzen Sie dazu die Werte für **region**, **vmSize** und **subscription** entsprechend:

CLI:

```bash
$subscription = "<yourSubID>"
$region = "<yourLocation>, example value is southeastasia"
$vmSize = "<yourVMSize>, example value is Standard_E64s_v3"

az vm list-skus --resource-type virtualMachines  --location $region --query "[?name=='$vmSize'].locationInfo[0].zoneDetails[0].Name" --subscription $subscription
```

PowerShell:

```powershell
$region = "southeastasia"
$vmSize = "Standard_E64s_v3"
(Get-AzComputeResourceSku | where {$_.Locations.Contains($region) -and ($_.Name -eq $vmSize) -and $_.LocationInfo[0].ZoneDetails.Count -gt 0})[0].LocationInfo[0].ZoneDetails
```

Die Antwort liegt in der nachstehend gezeigten Form vor, wobei X für die Zone steht, die für die Bereitstellung in Ihrer ausgewählten Region zu verwenden ist. X kann entweder 1, 2 oder 3 sein.

Behalten Sie den Wert für **Zones** bei. Er steht für Ihre Verfügbarkeitszone, und Sie benötigen ihn zum Bereitstellen eines Ultra-Datenträgers.

|ResourceType  |NAME  |Location  |Zones  |Einschränkung  |Funktion  |Wert  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Kommt vom Befehl keine Antwort, wird die ausgewählte VM-Größe für Ultra-Datenträger in der ausgewählten Region nicht unterstützt.

Nachdem Sie nun wissen, in welcher Zone die Bereitstellung erfolgen muss, führen Sie die Bereitstellungsschritte in diesem Artikel aus, um entweder einen virtuellen Computer mit einem zugeordneten Ultra-Datenträger bereitzustellen oder einem vorhandenen virtuellen Computer einen Ultra-Datenträger zuzuordnen.

## <a name="deploy-an-ultra-disk-using-azure-resource-manager"></a>Bereitstellen eines Ultra-Datenträgers über Azure Resource Manager

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations).

Wenn Sie einen virtuellen Computer mit mehreren Ultra-Datenträgern erstellen möchten, hilft Ihnen das Beispiel zum [Erstellen einer VM mit mehreren Ultra-Datenträgern](https://aka.ms/ultradiskArmTemplate) weiter.

Falls Sie Ihre eigene Vorlage verwenden möchten, sollten Sie sicherstellen, dass **apiVersion** für `Microsoft.Compute/virtualMachines` und `Microsoft.Compute/Disks` auf `2018-06-01` (oder höher) festgelegt ist.

Legen Sie die Datenträger-SKU auf **UltraSSD_LRS** fest, und geben Sie anschließend die Datenträgerkapazität, den IOPS-Wert, die Verfügbarkeitszone und den Durchsatz in MBit/s an, um einen Ultra-Datenträger zu erstellen.

Nachdem der virtuelle Computer bereitgestellt wurde, können Sie die Datenträger für Daten partitionieren und formatieren sowie für Ihre Workloads konfigurieren.

## <a name="deploy-an-ultra-disk-using-cli"></a>Bereitstellen eines Ultra-Datenträgers über CLI

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations).

Sie müssen einen virtuellen Computer erstellen, der Ultra-Datenträger verwenden kann. Nur dann können Sie einen Ultra-Datenträger zuordnen.

Ersetzen Sie die Variablen **$vmname**, **$rgname**, **$diskname**, **$location**, **$password** und **$user** durch Ihre eigenen Werte, bzw. legen Sie sie fest. Legen Sie **$zone** auf den Wert Ihrer Verfügbarkeitszone fest, den Sie am [Anfang dieses Artikels](#determine-vm-size-and-region-availability) ermittelt haben. Führen Sie anschließend den folgenden CLI-Befehl aus, um eine Ultra-fähige VM zu erstellen:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-disk-using-cli"></a>Erstellen eines Ultra-Datenträgers über CLI

Nachdem Sie nun über eine VM verfügen, der Ultra-Datenträger zugeordnet werden können, können Sie einen Ultra-Datenträger erstellen und diesen der VM zuordnen.

```azurecli-interactive
$location="eastus2"
$subscription="xxx"
$rgname="ultraRG"
$diskname="ssd1"
$vmname="ultravm1"
$zone=123

#create an ultra disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

## <a name="attach-an-ultra-disk-to-a-vm-using-cli"></a>Zuordnen eines Ultra-Datenträgers zu einer VM über CLI

Alternativ können Sie, wenn sich Ihre vorhandene VM in einer Region/Verfügbarkeitszone befindet, in der Ultra-Datenträger verwendet werden können, Ultra-Datenträger verwenden, ohne eine neue VM erstellen zu müssen.

```bash
$rgName = "<yourResourceGroupName>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$subscriptionId = "<yourSubscriptionID>"

az vm disk attach -g $rgName --vm-name $vmName --disk $diskName --subscription $subscriptionId
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-cli"></a>Anpassen der Leistung eines Ultra-Datenträgers über CLI

Ultra-Datenträger haben eine einzigartige Funktion, mit der Sie deren Leistung anpassen können. Mit dem folgenden Befehl wird die Verwendung dieser Funktion veranschaulicht:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-disk-using-powershell"></a>Bereitstellen eines Ultra-Datenträgers über PowerShell

Bestimmen Sie zuerst die Größe des bereitzustellenden virtuellen Computers. Eine Liste der unterstützten VM-Größen finden Sie im Abschnitt [Umfang und Einschränkungen für allgemeine Verfügbarkeit](#ga-scope-and-limitations). Dort finden Sie weitere Informationen zu den jeweiligen VM-Größen.

Um Ultra-Datenträger zu verwenden, müssen Sie eine VM erstellen, für die das Verwenden von Ultra-Datenträgern unterstützt wird. Ersetzen Sie die Variablen **$resourcegroup** und **$vmName** durch Ihre eigenen Werte. Legen Sie **$zone** auf den Wert Ihrer Verfügbarkeitszone fest, den Sie am [Anfang dieses Artikels](#determine-vm-size-and-region-availability) ermittelt haben. Führen Sie anschließend den folgenden [New-AzVm](/powershell/module/az.compute/new-azvm)-Befehl aus, um eine Ultra-fähige VM zu erstellen:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-an-ultra-disk-using-powershell"></a>Erstellen eines Ultra-Datenträgers über PowerShell

Nachdem Sie nun über eine VM verfügen, für die Ultra-Datenträger verwendet werden können, können Sie einen Ultra-Datenträger erstellen und diesen der VM zuordnen:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

## <a name="attach-an-ultra-disk-to-a-vm-using-powershell"></a>Zuordnen eines Ultra-Datenträgers zu einer VM über PowerShell

Alternativ können Sie, wenn sich Ihre vorhandene VM in einer Region/Verfügbarkeitszone befindet, in der Ultra-Datenträger verwendet werden können, Ultra-Datenträger verwenden, ohne eine neue VM erstellen zu müssen.

```powershell
# add disk to VM
$subscription = "<yourSubscriptionID>"
$resourceGroup = "<yourResourceGroup>"
$vmName = "<yourVMName>"
$diskName = "<yourDiskName>"
$lun = 1
Login-AzureRMAccount -SubscriptionId $subscription
$vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
$disk = Get-AzDisk -ResourceGroupName $resourceGroup -Name $diskName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun $lun
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

### <a name="adjust-the-performance-of-an-ultra-disk-using-powershell"></a>Anpassen der Leistung eines Ultra-Datenträgers über PowerShell

Ultra-Datenträger haben eine einzigartige Funktion, mit der Sie die Leistung anpassen können. Der folgende Befehl ist ein Beispiel für eine Anpassung der Leistung, ohne dass der Datenträger getrennt werden muss:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie den neuen Datenträgertyp ausprobieren möchten, [fordern Sie Zugriff über diese Umfrage an](https://aka.ms/UltraDiskSignup).