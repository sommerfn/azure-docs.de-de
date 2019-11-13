---
title: Verwenden eines virtuellen Windows-Problembehandlungscomputers mit Azure PowerShell | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Probleme mit virtuellen Windows-Computern (VMs) in Azure behandeln, indem Sie den Betriebssystemdatenträger per Azure PowerShell mit einer Wiederherstellungs-VM verbinden.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: 00f6e763006cbb8e5a2724536664291e0381e42f
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73749653"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Beheben von Problemen mit einer Windows-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM per Azure PowerShell
Wenn für Ihre Windows-VM in Azure ein Start- oder Datenträgerfehler auftritt, müssen Sie die Problembehandlung ggf. auf dem Datenträger selbst ausführen. Ein gängiges Beispiel wäre ein ungültiges Anwendungsupdate, das den erfolgreichen Start der VM verhindert. Dieser Artikel beschreibt, wie Sie mit Azure PowerShell eine Verbindung zwischen dem Datenträger und einer anderen Windows-VM herstellen, um Fehler zu beheben und dann Ihre ursprüngliche VM zu reparieren. 

> [!Important]
> Die Skripts in diesem Artikel gelten nur für VMs, die [verwaltete Datenträger](../windows/managed-disks-overview.md) verwenden. 

 

## <a name="recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess
Mit Azure PowerShell können Sie nun den Betriebssystemdatenträger für eine VM ändern. Sie müssen die VM nicht löschen und neu erstellen.

Der Problembehebungsprozess sieht wie folgt aus:

1. Beenden Sie die betroffene VM.
2. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers der VM.
3. Erstellen Sie einen Datenträger aus der Momentaufnahme vom Betriebssystemdatenträger.
4. Fügen Sie den Datenträger der Wiederherstellungs-VM als Datenträger an.
5. Stellen Sie eine Verbindung zur Wiederherstellungs-VM her. Bearbeiten Sie Dateien, oder führen Sie ein beliebiges Tool zum Beheben von Problemen auf dem kopierten Betriebssystemdatenträger aus.
6. Heben Sie die Bereitstellung des Datenträgers auf, und trennen Sie ihn von der Wiederherstellungs-VM.
7. Ändern Sie den Betriebssystemdatenträger für die betroffene VM.

Sie können die VM-Reparaturbefehle verwenden, um die Schritte 1, 2, 3, 4, 6 und 7 zu automatisieren. Weitergehende Dokumentation und Anleitungen finden Sie unter [Reparieren eines virtuellen Windows-Computers mit dem Reparaturbefehlen virtueller Azure-Computer](repair-windows-vm-using-azure-virtual-machine-repair-commands.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Stellen Sie sicher, dass Sie die [aktuelle Version von Azure PowerShell](/powershell/azure/overview) installiert haben und an Ihrem Abonnement angemeldet sind:

```powershell
Connect-AzAccount
```

Ersetzen Sie in den folgenden Beispielen die Parameternamen durch Ihre eigenen Werte. 

## <a name="determine-boot-issues"></a>Bestimmen von Problemen beim Start
Sie können einen Screenshot Ihrer VM in Azure anzeigen, die Ihnen als Hilfe beim Behandeln von Startproblemen dient. Anhand dieses Screenshots können Sie ermitteln, warum eine VM nicht erfolgreich gestartet werden kann. Im folgenden Beispiel stammt der Screenshot von der Windows-VM mit dem Namen `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`:

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Überprüfen Sie den Screenshot, um zu bestimmen, warum die VM nicht startet. Beachten Sie alle angegebenen spezifischen Fehlermeldungen oder Fehlercodes.

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` angehalten:

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Warten Sie, bis die VM gelöscht wurde, bevor Sie fortfahren.


## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>Erstellen einer Momentaufnahme des Betriebssystemdatenträgers der VM

Das folgende Beispiel erstellt eine Momentaufnahme namens `mySnapshot` vom Betriebssystemdatenträger der VM mit dem Namen „myVM“. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer VHD. Sie kann nicht an eine VM angefügt werden. Im nächsten Schritt erstellen Sie aus dieser Momentaufnahme einen Datenträger.

## <a name="create-a-disk-from-the-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme

Dieses Skript erstellt einen verwalteten Datenträger mit dem Namen `newOSDisk` aus der Momentaufnahme namens `mysnapshot`.  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
$storageType = 'Standard_LRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
Nun haben Sie eine Kopie des ursprünglichen Betriebssystemdatenträgers. Sie können diesen Datenträger für eine andere Windows-VM zur Problembehandlung bereitstellen.

## <a name="attach-the-disk-to-another-windows-vm-for-troubleshooting"></a>Anfügen eines Datenträgers an eine andere Windows-VM zur Problembehandlung

Nun fügen Sie die Kopie des ursprünglichen Betriebssystemdatenträgers einer VM als Datenträger an. Dadurch können Sie Konfigurationsfehler beheben oder zusätzliche Anwendungs- bzw. Systemprotokolldateien überprüfen. Im folgenden Beispiel wird der Datenträger `newOSDisk` der VM mit dem Namen `RecoveryVM` angefügt.

> [!NOTE]
> Um den Datenträger anzufügen, müssen sich die Kopie des ursprünglichen Betriebssystemdatenträgers und der Wiederherstellungs-VM am gleichen Speicherort befinden.

```powershell
$rgName = "myResourceGroup"
$vmName = "RecoveryVM"
$location = "eastus" 
$dataDiskName = "newOSDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="connect-to-the-recovery-vm-and-fix-issues-on-the-attached-disk"></a>Verbinden mit der Wiederherstellungs-VM und Behandeln von Problemen auf dem angefügten Datenträger

1. Stellen Sie per RDP und den entsprechenden Anmeldeinformationen eine Verbindung mit Ihrer Wiederherstellungs-VM her. Im folgenden Beispiel wird die RDP-Verbindungsdatei für die VM mit dem Namen `RecoveryVM` in der Ressourcengruppe mit dem Namen `myResourceGroup` heruntergeladen und in `C:\Users\ops\Documents` abgelegt.

    ```powershell
    Get-AzRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "RecoveryVM" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Der Datenträger wird automatisch erkannt und angefügt. Zeigen Sie die Liste mit den angefügten Volumes an, um den Laufwerkbuchstaben wie folgt zu ermitteln:

    ```powershell
    Get-Disk
    ```

    In der folgenden Beispielausgabe ist der Datenträger mit dem Datenträger **2** verbunden. (Sie können auch `Get-Volume` verwenden, um den Laufwerkbuchstaben anzuzeigen.):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        newOSDisk                                  Healthy             Online       127 GB MBR
    ```

Wenn die Kopie des ursprünglichen Betriebssystemdatenträgers bereitgestellt wurde, können Sie die erforderlichen Wartungs- und Problembehandlungsvorgänge ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.

## <a name="unmount-and-detach-original-os-disk"></a>Aufheben der Bereitstellung und Trennen des ursprünglichen Betriebssystemdatenträgers
Sobald die Fehler behoben sind, heben Sie die Bereitstellung auf, und trennen Sie den vorhandenen Datenträger von Ihrer Wiederherstellungs-VM. Sie können Ihren Datenträger nicht mit einer anderen VM nutzen, bis die Lease freigegeben wird, die die VM der Wiederherstellungs-VM anfügt.

1. Heben Sie in der RDP-Sitzung die Bereitstellung des Datenträgers auf der Wiederherstellungs-VM auf. Sie benötigen die Datenträgernummer aus dem vorherigen `Get-Disk`-Cmdlet. Verwenden Sie anschließend `Set-Disk`, um für den Datenträger den Offlinezustand festzulegen:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Vergewissern Sie sich, dass der Datenträger jetzt offline ist, indem Sie erneut `Get-Disk` verwenden. Die folgende Beispielausgabe zeigt, dass der Datenträger jetzt offline ist:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Beenden Sie die RDP-Sitzung. Entfernen Sie aus der Azure PowerShell-Sitzung den Datenträger `newOSDisk` von der VM „RecoveryVM“.

    ```powershell
    $myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "RecoveryVM"
    Remove-AzVMDataDisk -VM $myVM -Name "newOSDisk"
    Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Ändern des Betriebssystemdatenträgers für die betroffene VM

Sie können mit Azure PowerShell die Betriebssystemdatenträger austauschen. Sie müssen den virtuellen Computer nicht löschen und neu erstellen.

Dieses Beispiel beendet die VM `myVM` und weist den Datenträger `newOSDisk` als neuen Betriebssystemdatenträger zu. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="verify-and-enable-boot-diagnostics"></a>Überprüfen und Aktivieren der Startdiagnose

Im folgenden Beispiel wird die Diagnoseerweiterung in der VM namens `myVMDeployed` in der Ressourcengruppe namens `myResourceGroup` aktiviert:

```powershell
$myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) weiter. Konsultieren Sie [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.

Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
