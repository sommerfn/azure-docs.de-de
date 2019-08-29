---
title: Verwenden einer Linux-Problembehebungs-VM im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme auf einer Linux-VM beheben, indem Sie mithilfe des Azure-Portals eine Verbindung zwischen dem Betriebssystemdatenträger und einer Wiederherstellungs-VM herstellen.
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/19/2019
ms.author: genli
ms.openlocfilehash: 21122847c1b417b00cfe8c69b8324a2f73bf31ea
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641129"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Beheben von Problemen einer Linux-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM mit dem Azure-Portal
Wenn für Ihren virtuellen Linux-Computer (VM) ein Start- oder Datenträgerfehler auftritt, müssen Sie möglicherweise Schritte zur Problembehebung auf der virtuellen Festplatte selbst ausführen. Ein gängiges Beispiel wäre ein ungültiger Eintrag in `/etc/fstab`, der den erfolgreichen Start der VM verhindert. In diesem Artikel wird erläutert, wie das Azure-Portal die Verbindung zwischen Ihrer virtuellen Festplatte und einer anderen Linux-VM herstellt, um alle Fehler zu beheben und dann Ihre ursprüngliche VM neu zu erstellen.

## <a name="recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess
Der Problembehebungsprozess sieht wie folgt aus:

1. Beenden Sie die betroffene VM.
1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des virtuellen Computers.
1. Erstellen Sie eine virtuelle Festplatte aus der Momentaufnahme.
1. Fügen Sie einer anderen Windows-Problembehebungs-VM die virtuelle Festplatte hinzu, und stellen Sie sie bereit.
1. Stellen Sie eine Verbindung mit der Problembehebungs-VM her. Bearbeiten Sie Dateien, oder führen Sie ein beliebiges Tool zum Beheben von Problemen auf der ursprünglichen virtuellen Festplatte aus.
1. Heben Sie die Bereitstellung auf, und trennen Sie die virtuelle Festplatte von der Problembehebungs-VM.
1. Tauschen Sie den Betriebssystemdatenträger für den virtuellen Computer aus.

> [!NOTE]
> Dieser Artikel gilt nicht für eine VM mit einem nicht verwalteten Datenträger.

## <a name="determine-boot-issues"></a>Bestimmen von Problemen beim Start
Überprüfen Sie die Startdiagnose und den Screenshot der VM, um zu bestimmen, warum Ihre VM nicht ordnungsgemäß starten kann. Ein gängiges Beispiel wäre ein ungültiger Eintrag in `/etc/fstab` oder eine zugrunde liegende virtuelle Festplatte, die gelöscht oder verschoben wird.

Wählen Sie Ihre VM im Portal aus, und scrollen Sie dann nach unten zum Abschnitt **Unterstützung + Problembehebung**. Klicken Sie auf **Diagnose starten**, um die Konsolennachrichten anzuzeigen, die von Ihrer VM gestreamt werden. Überprüfen Sie die Konsolenprotokolle, um festzustellen, ob Sie bestimmen können, warum ein Problem für die VM aufgetreten ist. Das folgende Beispiel zeigt eine VM, die im Wartungsmodus hängen geblieben ist und die einen manuellen Eingriff erfordert:

![Anzeigen der Konsolenprotokolle für die VM-Startdiagnose](./media/troubleshoot-recovery-disks-portal-linux/boot-diagnostics-error.png)

Sie können auch am oberen Rand des Startdiagnoseprotokolls auf **Screenshot** klicken, um einen VM-Screenshot herunterzuladen.

## <a name="take-a-snapshot-of-the-os-disk"></a>Erstellen einer Momentaufnahme des Betriebssystemdatenträgers
Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer virtuellen Festplatte (Virtual Hard Drive, VHD). Es empfiehlt sich, den virtuellen Computer vor dem Erstellen einer Momentaufnahme ordnungsgemäß herunterzufahren, um alle Prozesse, die ausgeführt werden, ordnungsgemäß abzuschließen. Gehen Sie folgendermaßen vor, um eine Momentaufnahme eines Betriebssystemdatenträgers zu erstellen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie auf der Seitenleiste **Virtuelle Computer** und dann den virtuellen Computer aus, auf dem es ein Problem gibt.
1. Klicken Sie im linken Bereich auf **Datenträger**, und wählen Sie dann den Namen des Betriebssystemdatenträgers aus.
    ![Abbildung mit dem Namen des Betriebssystemdatenträgers](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. Wählen Sie auf der Seite **Übersicht** des Betriebssystemdatenträgers die Option **Momentaufnahme erstellen** aus.
1. Erstellen Sie eine Momentaufnahme an dem Speicherort, an dem sich auch der Betriebssystemdatenträger befindet.

## <a name="create-a-disk-from-the-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme
Um einen Datenträger aus der Momentaufnahme zu erstellen, führen Sie die folgenden Schritte aus:

1. Wählen Sie im Azure-Portal **Cloud Shell** aus.

    ![Abbildung zu Öffnen von Cloud Shell](./media/troubleshoot-recovery-disks-portal-windows/cloud-shell.png)
1. Führen Sie die folgenden PowerShell-Befehle aus, um einen verwalteten Datenträger aus der Momentaufnahme zu erstellen. Sie müssen diese Beispielnamen durch die entsprechenden Namen ersetzen.

    ```powershell
    #Provide the name of your resource group
    $resourceGroupName ='myResourceGroup'
    
    #Provide the name of the snapshot that will be used to create Managed Disks
    $snapshotName = 'mySnapshot' 
    
    #Provide the name of theManaged Disk
    $diskName = 'newOSDisk'
    
    #Provide the size of the disks in GB. It should be greater than the VHD file size. In this sample, the size of the snapshot is 127 GB. So we set the disk size to 128 GB.
    $diskSize = '128'
    
    #Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
    $storageType = 'StandardLRS'
    
    #Provide the Azure region (e.g. westus) where Managed Disks will be located.
    #This location should be same as the snapshot location
    #Get all the Azure location using command below:
    #Get-AzLocation
    $location = 'westus'
    
    $snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
     
    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
     
    New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
    ```
3. Wenn die Befehle erfolgreich ausgeführt wurden, wird der neue Datenträger in der Ressourcengruppe angezeigt, die Sie bereitgestellt haben.

## <a name="attach-disk-to-another-vm"></a>Anfügen des Datenträgers an einen anderen virtuellen Computer
Verwenden Sie eine andere Problembehebungs-VM für die nächsten Schritte. Nachdem Sie den Datenträger der Problembehebungs-VM angefügt haben, können Sie den Inhalt des Datenträgers durchsuchen und bearbeiten. Durch diesen Prozess können Sie alle Konfigurationsfehler beheben oder zusätzliche Anwendungs- oder Systemprotokolldateien überprüfen. Führen Sie die folgenden Schritte aus, um den Datenträger an einen anderen virtuellen Computer anzufügen:

1. Wählen Sie im Portal Ihre Ressourcengruppe und dann Ihre Problembehebungs-VM aus. Wählen Sie **Datenträger** und **Bearbeiten** aus, und klicken Sie dann auf **Datenträger hinzufügen**:

    ![Hinzufügen des vorhandenen Datenträgers im Portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Wählen Sie in der Liste **Datenträger** den Betriebssystemdatenträger der VM aus, die Sie identifiziert haben. Wenn der Betriebssystemdatenträger nicht angezeigt wird, stellen Sie sicher, dass sich die fehlerhafte VM und die Betriebssystemdatenträger in derselben Region (am selben Standort) befinden. 
3. Klicken Sie auf **Speichern**, um die Änderungen zu übernehmen.

## <a name="mount-the-attached-data-disk"></a>Bereitstellen des hinzugefügten Datenträgers

> [!NOTE]
> In den folgenden Beispielen sind die Schritte beschrieben, die auf einer Ubuntu-VM erforderlich sind. Bei Verwendung einer anderen Linux-Distribution, z.B. Red Hat Enterprise Linux oder SUSE, sind die Speicherorte für Protokolldateien und `mount`-Befehle ggf. etwas anders. Nähere Informationen zu Ihrer spezifischen Distribution für die entsprechenden Änderungen in Befehlen finden Sie in der Dokumentation.

1. Stellen Sie mithilfe von SSH und den entsprechenden Anmeldeinformationen eine Verbindung zu Ihrer Problembehebungs-VM her. Wenn dieses Laufwerk der erste Datenträger ist, der zu Ihrer Problembehebungs-VM hinzugefügt wurde, ist es wahrscheinlich mit `/dev/sdc` verbunden. Verwenden Sie `dmseg`, um hinzugefügte Datenträger aufzulisten:

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


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Beheben von Problemen auf der ursprünglichen virtuellen Festplatte
Nun, da die vorhandene virtuelle Festplatte bereitgestellt ist, können Sie jetzt alle Schritte zur Wartung und Problembehebung ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Aufheben der Bereitstellung und Trennen der ursprünglichen virtuellen Festplatte
Sobald Ihre Fehler behoben sind, trennen Sie die vorhandene virtuelle Festplatte von Ihrer Problembehebungs-VM. Sie können Ihre virtuelle Festplatte nicht mit einer anderen VM nutzen, bis die Lease freigegeben wird, die die virtuelle Festplatte zur Problembehebungs-VM hinzufügt.

1. Aufheben der Bereitstellung der vorhandenen virtuellen Festplatte von der SSH-Sitzung bis zu Ihrer Problembehebungs-VM. Wechseln Sie aus dem übergeordneten Verzeichnis zu Ihrem Bereitstellungspunkt:

    ```bash
    cd /
    ```

    Heben Sie die Bereitstellung der vorhandenen virtuellen Festplatte auf. Im folgenden Beispiel wird die Bereitstellung des Geräts in `/dev/sdc1` aufgehoben:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Trennen Sie die virtuelle Festplatte von der VM. Wählen Sie Ihre VM im Portal aus, und klicken Sie auf **Datenträger**. Wählen Sie Ihre vorhandene virtuelle Festplatte aus, und klicken Sie dann auf **Trennen**:

    ![Trennen einer vorhandenen virtuellen Festplatte](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Warten Sie, bis die VM den Datenträger erfolgreich getrennt hat, bevor Sie fortfahren.

## <a name="swap-the-os-disk-for-the-vm"></a>Austauschen des Betriebssystemdatenträgers für den virtuellen Computer

Im Azure-Portal wird jetzt der Wechsel des Betriebssystemdatenträgers des virtuellen Computers unterstützt. Gehen Sie dazu folgendermaßen vor:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie auf der Seitenleiste **Virtuelle Computer** und dann den virtuellen Computer aus, auf dem es ein Problem gibt.
1. Wählen Sie im linken Bereich **Datenträger** und dann **Betriebssystemdatenträger austauschen** aus.
        ![Abbildung zum Austauschen des Betriebssystemdatenträgers im Azure-Portal](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Wählen Sie den neuen Datenträger aus, den Sie repariert haben, und geben Sie dann den Namen des virtuellen Computers ein, um die Änderung zu bestätigen. Wird der Datenträger in der Liste nicht angezeigt, warten Sie 10 bis 15 Minuten, nachdem Sie den Datenträger von der Problembehebungs-VM getrennt haben. Vergewissern Sie sich außerdem, dass sich der Datenträger am selben Speicherort wie der virtuelle Computer befindet.
1. Klicken Sie auf „OK“.

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, finden Sie unter [Problembehandlung von SSH-Verbindungen mit einer Azure-VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Hilfestellungen. Konsultieren Sie [Beheben von Anwendungskonnektivitätsproblemen auf einer Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.

Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
