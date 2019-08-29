---
title: Verwenden einer Windows-Problembehebungs-VM im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme auf einer Windows-VM in Azure beheben, indem Sie mithilfe des Azure-Portals eine Verbindung zwischen dem Betriebssystemdatenträger und einer Wiederherstellungs-VM herstellen.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/19/2018
ms.author: genli
ms.openlocfilehash: d1b76479c17a9b1ace149334c7bb451c7bf2cc45
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70103351"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Beheben von Problemen einer Windows-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM mit dem Azure-Portal
Wenn für Ihren virtuellen Windows-Computer in Azure ein Start- oder Datenträgerfehler auftritt, müssen Sie unter Umständen Schritte zur Problembehebung auf der virtuellen Festplatte selbst ausführen. Ein gängiges Beispiel wäre ein ungültiges Anwendungsupdate, das den erfolgreichen Start der VM verhindert. In diesem Artikel wird erläutert, wie das Azure-Portal die Verbindung zwischen Ihrer virtuellen Festplatte und einer anderen Windows-VM herstellt, um alle Fehler zu beheben und dann Ihre ursprüngliche VM neu zu erstellen. 

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

## <a name="take-a-snapshot-of-the-os-disk"></a>Erstellen einer Momentaufnahme des Betriebssystemdatenträgers
Eine Momentaufnahme ist eine vollständige, schreibgeschützte Kopie einer virtuellen Festplatte (Virtual Hard Drive, VHD). Es empfiehlt sich, den virtuellen Computer vor dem Erstellen einer Momentaufnahme ordnungsgemäß herunterzufahren, um alle Prozesse, die ausgeführt werden, ordnungsgemäß abzuschließen. Gehen Sie folgendermaßen vor, um eine Momentaufnahme eines Betriebssystemdatenträgers zu erstellen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **Virtuelle Computer** auf der Seitenleiste aus, und wählen Sie dann den virtuellen Computer aus, auf dem es ein Problem gibt.
1. Klicken Sie im linken Bereich auf **Datenträger**, und wählen Sie dann den Namen des Betriebssystemdatenträgers aus.
    ![Abbildung mit dem Namen des Betriebssystemdatenträgers](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. Wählen Sie auf der **Übersicht**-Seite des Betriebssystemdatenträgers die Option **Momentaufnahme erstellen** aus.
1. Erstellen Sie eine Momentaufnahme in dem Standort, in dem sich auch der Betriebssystemdatenträger befindet.

## <a name="create-a-disk-from-the-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme
Um einen Datenträger aus der Momentaufnahme zu erstellen, führen Sie die folgenden Schritte aus:

1. Wählen Sie **Cloud Shell** im Azure-Portal aus.

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
    
    #Provide the storage type for Managed Disk.  Premium_LRS or Standard_LRS.
    $storageType = 'Standard_LRS'
    
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

## <a name="attach-the-disk-to-another-vm"></a>Zuordnen des Datenträgers zu einem anderen virtuellen Computer
Verwenden Sie eine andere Problembehebungs-VM für die nächsten Schritte. Nachdem Sie den Datenträger der Problembehebungs-VM zugeordnet haben, können Sie den Inhalt des Datenträgers durchsuchen und bearbeiten. Durch diesen Prozess können Sie alle Konfigurationsfehler beheben oder zusätzliche Anwendungs- oder Systemprotokolldateien überprüfen. Führen Sie die folgenden Schritte aus, um den Datenträger einem andere virtuellen Computer zuzuordnen:

1. Wählen Sie im Portal Ihre Ressourcengruppe und dann Ihre Problembehebungs-VM aus. Wählen Sie **Datenträger** und **Bearbeiten** aus, und klicken Sie dann auf **Datenträger hinzufügen**:

    ![Hinzufügen des vorhandenen Datenträgers im Portal](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Wählen Sie in der Liste **Datenträger** den Betriebssystemdatenträger der VM aus, die Sie identifiziert haben. Wenn der Betriebssystemdatenträger nicht angezeigt wird, stellen Sie sicher, dass sich die fehlerhafte VM und die Betriebssystemdatenträger in derselben Region (am selben Standort) befinden. 
3. Klicken Sie auf **Speichern**, um die Änderungen zu übernehmen.

## <a name="mount-the-attached-data-disk-to-the-vm"></a>Bereitstellen des zugeordneten Datenträgers für die VM

1. Öffnen Sie eine Remotedesktopverbindung mit der Problembehebungs-VM. 
2. Öffnen Sie in der Problembehebungs-VM den **Server-Manager**, und wählen Sie dann **Datei- und Speicherdienste** aus. 

    ![Auswählen von Datei- und Speicherdiensten im Server-Manager](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Der Datenträger wird automatisch erkannt und angefügt. Wählen Sie zum Anzeigen einer Liste mit den verbundenen Datenträgern die Option **Datenträger**. Sie können Ihren Datenträger auswählen, um die Volumeinformationen anzuzeigen, z.B. den Laufwerkbuchstaben. Im folgenden Beispiel ist zu sehen, wie der Datenträger angefügt wird und den Buchstaben **F:** aufweist:

    ![Angefügter Datenträger und Volumeinformationen in Server-Manager](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Beheben von Problemen auf der ursprünglichen virtuellen Festplatte
Nun, da die vorhandene virtuelle Festplatte bereitgestellt ist, können Sie jetzt alle Schritte zur Wartung und Problembehebung ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Aufheben der Bereitstellung und Trennen der ursprünglichen virtuellen Festplatte
Sobald Ihre Fehler behoben sind, trennen Sie die vorhandene virtuelle Festplatte von Ihrer Problembehebungs-VM. Sie können Ihre virtuelle Festplatte nicht mit einer anderen VM nutzen, bis die Lease freigegeben wird, die die virtuelle Festplatte zur Problembehebungs-VM hinzufügt.

1. Öffnen Sie in der RDP-Sitzung für Ihre VM den **Server-Manager**, und wählen Sie anschließend die Option **Datei- und Speicherdienste**:

    ![Auswählen von Datei- und Speicherdiensten im Server-Manager](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Wählen Sie die Option **Datenträger**, und wählen Sie dann Ihren Datenträger aus. Klicken Sie mit der rechten Maustaste auf Ihren Datenträger, und wählen Sie die Option **Take Offline** (Offline schalten):

    ![Festlegen des Offlinezustands für den Datenträger in Server-Manager](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Trennen Sie die virtuelle Festplatte von der VM. Wählen Sie Ihre VM im Azure-Portal aus, und klicken Sie auf **Datenträger**. 
4. Klicken Sie auf **Bearbeiten**, wählen Sie den angefügten Betriebssystemdatenträger aus, und klicken Sie dann auf **Trennen**:

    ![Trennen einer vorhandenen virtuellen Festplatte](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Warten Sie, bis die VM den Datenträger erfolgreich getrennt hat, bevor Sie fortfahren.

## <a name="swap-the-os-disk-for-the-vm"></a>Austauschen des Betriebssystemdatenträgers für den virtuellen Computer

Im Azure-Portal wird jetzt der Wechsel des Betriebssystemdatenträgers des virtuellen Computers unterstützt. Gehen Sie dazu folgendermaßen vor:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com). Wählen Sie **Virtuelle Computer** auf der Seitenleiste aus, und wählen Sie dann den virtuellen Computer aus, auf dem es ein Problem gibt.
1. Wählen Sie im linken Bereich **Datenträger** aus, und wählen Sie dann **Betriebssystemdatenträger austauschen** aus.
        ![Abbildung zum Austauschen des Betriebssystemdatenträgers in Azure-Portal](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Wählen Sie den neuen Datenträger aus, den Sie repariert haben, und geben Sie dann den Namen des virtuellen Computers ein, um die Änderung zu bestätigen. Wird der Datenträger in der Liste nicht angezeigt, warten Sie 10 bis 15 Minuten, nachdem Sie den Datenträger von der Problembehebungs-VM getrennt haben. Vergewissern Sie sich außerdem, dass sich der Datenträger am selben Standort wie der virtuelle Computer befindet.
1. Wählen Sie „OK“ aus.

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md) weiter. Konsultieren Sie [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](troubleshoot-app-connection.md) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.

Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).


