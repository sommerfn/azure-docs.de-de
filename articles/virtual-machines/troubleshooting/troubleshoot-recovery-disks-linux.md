---
title: Verwenden eines virtuellen Linux-Computers zur Problembehandlung mithilfe der Azure CLI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme auf einer Linux-VM beheben, indem Sie mithilfe der Azure-CLI eine Verbindung zwischen dem Betriebssystemdatenträger und einer Wiederherstellungs-VM herstellen
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: genli
ms.openlocfilehash: 1b91a39e1297d8952da67a4f8d3b8568cefe04ce
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73620558"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli"></a>Beheben von Problemen einer Linux-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM mithilfe der Azure CLI
Wenn für Ihren virtuellen Linux-Computer (VM) ein Start- oder Datenträgerfehler auftritt, müssen Sie möglicherweise Schritte zur Problembehebung auf der virtuellen Festplatte selbst ausführen. Ein gängiges Beispiel wäre ein ungültiger Eintrag in `/etc/fstab`, der den erfolgreichen Start der VM verhindert. In diesem Artikel wird erläutert, wie die Azure CLI die Verbindung zwischen Ihrer virtuellen Festplatte und einer anderen Linux-VM herstellt, um alle Fehler zu beheben und dann Ihre ursprüngliche VM neu zu erstellen. 

## <a name="recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess
Der Problembehebungsprozess sieht wie folgt aus:

1. Beenden Sie die betroffene VM.
1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers der VM.
1. Erstellen Sie einen Datenträger aus der Momentaufnahme vom Betriebssystemdatenträger.
1. Ordnen Sie den neuen Betriebssystemdatenträger zu einer anderen Linux-VM zu, die als Problembehebungs-VM fungieren soll, und binden Sie den neuen Datenträger in diese VM ein.
1. Stellen Sie eine Verbindung mit der Problembehebungs-VM her. Bearbeiten Sie Dateien, oder führen Sie ein beliebiges Tool aus, um die Probleme auf dem neuen Betriebssystemdatenträger zu beheben.
1. Heben Sie die Einbindung des neuen Betriebssystemdatenträgers in die Problembehebungs-VM auf, und trennen Sie diesen Datenträger von der Problembehebungs-VM.
1. Ändern Sie den Betriebssystemdatenträger für die betroffene VM.

Zum Ausführen dieser Schritte zur Problembehandlung muss die neueste [Azure CLI](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mit [az login](/cli/azure/reference-index) bei einem Azure-Konto angemeldet sein.

> [!Important]
> Die Skripts in diesem Artikel gelten nur für VMs, die [verwaltete Datenträger](../linux/managed-disks-overview.md) verwenden. 

Ersetzen Sie in den folgenden Beispielen die Parameternamen durch Ihre eigenen Werte, beispielsweise `myResourceGroup` und `myVM`.

## <a name="determine-boot-issues"></a>Bestimmen von Problemen beim Start
Überprüfen Sie die serielle Ausgabe, um zu bestimmen, warum Ihre VM nicht ordnungsgemäß starten kann. Ein gängiges Beispiel ist ein ungültiger Eintrag in `/etc/fstab` oder eine zugrunde liegende virtuelle Festplatte, die gelöscht oder verschoben wird.

Rufen Sie mit [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics) die Startprotokolle ab. Im folgenden Beispiel wird die serielle Ausgabe der VM namens `myVM` in der Ressourcengruppe namens `myResourceGroup` aufgerufen:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Überprüfen Sie die serielle Ausgabe, um zu bestimmen, warum die VM nicht startet. Wenn die serielle Ausgabe keine Auskunft bietet, müssen Sie möglicherweise Protokolldateien in `/var/log` überprüfen, nachdem Sie die virtuelle Festplatte mit der Problembehebungs-VM verbunden haben.

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Im folgenden Beispiel wird die VM `myVM` in der Ressourcengruppe `myResourceGroup` angehalten:

```azurecli
az vm stop --resource-group MyResourceGroup --name MyVm
```
## <a name="take-a-snapshot-from-the-os-disk-of-the-affected-vm"></a>Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers der betroffenen VM.

Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer VHD. Sie kann nicht an eine VM angefügt werden. Im nächsten Schritt erstellen Sie aus dieser Momentaufnahme einen Datenträger. Das folgende Beispiel erstellt eine Momentaufnahme namens `mySnapshot` vom Betriebssystemdatenträger der VM mit dem Namen „myVM“. 

```azurecli
#Get the OS disk Id 
$osdiskid=(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)

#creates a snapshot of the disk
az snapshot create --resource-group myResourceGroupDisk --source "$osdiskid" --name mySnapshot
```
## <a name="create-a-disk-from-the-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme

Dieses Skript erstellt einen verwalteten Datenträger mit dem Namen `myOSDisk` aus der Momentaufnahme namens `mySnapshot`.  

```azurecli
#Provide the name of your resource group
$resourceGroup=myResourceGroup

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshot=mySnapshot

#Provide the name of the Managed Disk
$osDisk=myNewOSDisk

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize=128

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
$storageType=Premium_LRS

#Provide the OS type
$osType=linux

#Provide the name of the virtual machine
$virtualMachine=myVM

#Get the snapshot Id 
$snapshotId=(az snapshot show --name $snapshot --resource-group $resourceGroup --query [id] -o tsv)

# Create a new Managed Disks using the snapshot Id.

az disk create --resource-group $resourceGroup --name $osDisk --sku $storageType --size-gb $diskSize --source $snapshotId

```

Wenn sich die Ressourcengruppe und die Quellmomentaufnahme nicht in derselben Region befinden, wird beim Ausführen von `az disk create` der Fehler „Ressource wurde nicht gefunden“ angezeigt. In diesem Fall müssen Sie `--location <region>` angeben, um den Datenträger in derselben Region wie die Quellmomentaufnahme zu erstellen.

Nun haben Sie eine Kopie des ursprünglichen Betriebssystemdatenträgers. Sie können diesen neuen Datenträger in eine andere Windows-VM zu Problembehandlungszwecken einbinden.

## <a name="attach-the-new-virtual-hard-disk-to-another-vm"></a>Zuordnen der neuen virtuellen Festplatte zu einer anderen VM
Verwenden Sie eine andere Problembehebungs-VM für die nächsten Schritte. Ordnen Sie den Datenträger zu dieser Problembehebungs-VM zu, um den Inhalt des Datenträgers zu durchsuchen und zu bearbeiten. Durch diesen Prozess können Sie alle Konfigurationsfehler beheben oder zusätzliche Anwendungs- oder Systemprotokolldateien überprüfen.

Mit diesem Skript wird der Datenträger `myNewOSDisk` der VM `MyTroubleshootVM` zugeordnet:

```azurecli
# Get ID of the OS disk that you just created.
$myNewOSDiskid=(az vm show -g myResourceGroupDisk -n myNewOSDisk --query "storageProfile.osDisk.managedDisk.id" -o tsv)

# Attach the disk to the troubleshooting VM
az vm disk attach --disk $diskId --resource-group MyResourceGroup --size-gb 128 --sku Standard_LRS --vm-name MyTroubleshootVM
```
## <a name="mount-the-attached-data-disk"></a>Bereitstellen des hinzugefügten Datenträgers

> [!NOTE]
> In den folgenden Beispielen sind die Schritte beschrieben, die auf einer Ubuntu-VM erforderlich sind. Bei Verwendung einer anderen Linux-Distribution, z.B. Red Hat Enterprise Linux oder SUSE, sind die Speicherorte für Protokolldateien und `mount`-Befehle ggf. etwas anders. Nähere Informationen zu Ihrer spezifischen Distribution für die entsprechenden Änderungen in Befehlen finden Sie in der Dokumentation.

1. Stellen Sie mithilfe von SSH und den entsprechenden Anmeldeinformationen eine Verbindung zu Ihrer Problembehebungs-VM her. Wenn dieses Laufwerk der erste Datenträger ist, der zu Ihrer Problembehebungs-VM hinzugefügt wurde, ist es wahrscheinlich mit `/dev/sdc` verbunden. Verwenden Sie `dmesg`, um hinzugefügte Datenträger anzuzeigen:

    ```bash
    dmesg | grep SCSI
    ```

    Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    Im vorherigen Beispiel befindet sich der Betriebssystemdatenträger in `/dev/sda`, und der temporäre Datenträger, der für jede VM bereitgestellt ist, befindet sich in `/dev/sdb`. Wenn Sie mehrere Datenträger haben, müssen Sie in `/dev/sdd`, `/dev/sde` usw. vorhanden sein.

2. Erstellen Sie ein Verzeichnis, um Ihre vorhandene virtuelle Festplatte bereitzustellen. Im folgenden Beispiel wird ein Verzeichnis namens `troubleshootingdisk` erstellt:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Wenn Sie über mehrere Partitionen auf Ihrer vorhandenen virtuellen Festplatte verfügen, stellen Sie die erforderliche Partition bereit. Das folgende Beispiel stellt die erste primäre Partition in `/dev/sdc1` bereit:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Die bewährte Methode ist das Bereitstellen von Datenträgern auf VMs in Azure mithilfe des Universally Unique Identifiers (UUID) der virtuellen Festplatte. In diesem kurzen Szenario zur Problembehebung ist das Bereitstellen der virtuellen Festplatte mithilfe von UUID nicht erforderlich. Wenn allerdings unter normalen Bedingungen `/etc/fstab` bearbeitet wird, um virtuelle Festplatten mithilfe des Gerätenamens anstatt des UUID bereitzustellen, dann wird die VM möglicherweise nicht erfolgreich starten können.


## <a name="fix-issues-on-the-new-os-disk"></a>Beheben von Problemen auf dem neuen Betriebssystemdatenträger
Nun, da die vorhandene virtuelle Festplatte bereitgestellt ist, können Sie jetzt alle Schritte zur Wartung und Problembehebung ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.


## <a name="unmount-and-detach-the-new-os-disk"></a>Aufheben der Einbindung (Bereitstellung) und Trennen des neuen Betriebssystemdatenträgers
Sobald Ihre Fehler behoben sind, heben Sie die Bereitstellung auf, und trennen Sie die vorhandene virtuelle Festplatte von Ihrer Problembehebungs-VM. Sie können Ihre virtuelle Festplatte nicht mit einer anderen VM nutzen, bis die Lease freigegeben wird, die die virtuelle Festplatte zur Problembehebungs-VM hinzufügt.

1. Aufheben der Bereitstellung der vorhandenen virtuellen Festplatte von der SSH-Sitzung bis zu Ihrer Problembehebungs-VM. Wechseln Sie aus dem übergeordneten Verzeichnis zu Ihrem Bereitstellungspunkt:

    ```bash
    cd /
    ```

    Heben Sie die Bereitstellung der vorhandenen virtuellen Festplatte auf. Im folgenden Beispiel wird die Bereitstellung des Geräts in `/dev/sdc1` aufgehoben:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Trennen Sie die virtuelle Festplatte von der VM. Beenden Sie die SSH-Sitzung zu Ihrer Problembehebungs-VM:

    ```azurecli
    az vm disk detach -g MyResourceGroup --vm-name MyTroubleShootVm --name myNewOSDisk
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Ändern des Betriebssystemdatenträgers für die betroffene VM

Sie können die Azure-Befehlszeilenschnittstelle verwenden, um die Betriebssystemdatenträger zu tauschen. Sie müssen den virtuellen Computer nicht löschen und neu erstellen.

Dieses Beispiel beendet die VM `myVM` und weist den Datenträger `myNewOSDisk` als neuen Betriebssystemdatenträger zu.

```azurecli
# Stop the affected VM
az vm stop -n myVM -g myResourceGroup

# Get ID of the OS disk that is repaired.
$myNewOSDiskid=(az vm show -g myResourceGroupDisk -n myNewOSDisk --query "storageProfile.osDisk.managedDisk.id" -o tsv)

# Change the OS disk of the affected VM to "myNewOSDisk"
az vm update -g myResourceGroup -n myVM --os-disk $myNewOSDiskid

# Start the VM
az vm start -n myVM -g myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, finden Sie unter [Problembehandlung von SSH-Verbindungen mit einer Azure-VM](troubleshoot-ssh-connection.md) Hilfestellungen. Konsultieren Sie [Beheben von Anwendungskonnektivitätsproblemen auf einer Linux-VM](troubleshoot-app-connection.md) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.

