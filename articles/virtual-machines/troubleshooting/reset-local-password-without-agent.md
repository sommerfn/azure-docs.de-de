---
title: Zurücksetzen eines lokalen Windows-Kennworts ohne Azure-Agent | Microsoft Docs
description: 'Gewusst wie: Zurücksetzen des Kennworts eines lokalen Windows-Benutzerkontos, wenn der Azure-Gast-Agent auf einem virtuellen Computer nicht installiert ist oder nicht ausgeführt wird.'
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: genli
ms.openlocfilehash: 75d6c10ded4038297689835d5ff012f344540e6f
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69638856"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Zurücksetzen eines lokalen Windows-Kennworts im Offlinemodus für einen virtuellen Azure-Computer
Sie können das lokale Windows-Kennwort eines virtuellen Computers in Azure im [Azure-Portal oder mithilfe von Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zurücksetzen, sofern der Azure-Gast-Agent installiert ist. Diese Methode ist die einfachste Möglichkeit zum Zurücksetzen eines Kennworts für einen virtuellen Azure-Computer. Wenn der Azure-Gast-Agent nicht reagiert oder nach dem Hochladen eines benutzerdefinierten Images nicht installiert wird, können Sie ein Windows-Kennwort manuell zurücksetzen. In diesem Artikel wird erläutert, wie das Kennwort eines lokalen Kontos durch Anfügen des virtuellen Quellbetriebssystem-Datenträgers an einen anderen virtuellen Computer zurückgesetzt wird. Die in diesem Artikel beschriebenen Schritte gelten nicht für Windows-Domänencontroller. 

> [!WARNING]
> Führen Sie diesen Vorgang nur als letzte Möglichkeit durch. Versuchen Sie immer zuerst, das Kennwort im [Azure-Portal oder mithilfe von Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zurückzusetzen.

## <a name="overview-of-the-process"></a>Übersicht über den Vorgang
Wenn kein Zugriff auf den Azure-Gast-Agent besteht, sehen die wichtigsten Schritte zum Zurücksetzen eines lokalen Kennworts für einen virtuellen Windows-Computer in Azure wie folgt aus:

1. Beenden Sie die betroffene VM.
1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des virtuellen Computers.
1. Erstellen Sie eine Kopie des Betriebssystemdatenträgers aus der Momentaufnahme.
1. Gehen Sie so vor, dass Sie den kopierten Betriebssystemdatenträger einem anderen virtuellen Windows-Computer zuordnen und in diesen einbinden, und erstellen Sie dann einige Konfigurationsdateien auf dem Datenträger. Die Dateien helfen Ihnen dabei, das Kennwort zurückzusetzen.
1. Heben Sie die Einbindung des kopierten Betriebssystemdatenträgers in die Problembehebungs-VM auf, und trennen Sie diesen Datenträger von der Problembehebungs-VM.
1. Tauschen Sie den Betriebssystemdatenträger für die betroffene VM.

## <a name="detailed-steps-for-the-vm-with-resource-manager-deployment"></a>Ausführliche Schritte für die VM mit Resource Manager-Bereitstellung

> [!NOTE]
> Die Schritte gelten nicht für Windows-Domänencontroller. Sie können nur für einen eigenständigen Server oder einen Server ausgeführt werden, der Mitglied einer Domäne ist.

Versuchen Sie zunächst immer, ein Kennwort im [Azure-Portal oder mithilfe von Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zurückzusetzen, bevor Sie die folgenden Schritte ausführen. Vergewissern Sie sich zunächst, dass Sie über eine Sicherung des virtuellen Computers verfügen.

1. Erstellen Sie eine Momentaufnahme für den Betriebssystemdatenträger der betroffenen VM, erstellen Sie aus der Momentaufnahme einen Datenträger, und fügen Sie diesen an eine VM zur Problembehebung an. Weitere Informationen finden Sie unter [Beheben von Problemen mit einer Windows-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM im Azure-Portal](troubleshoot-recovery-disks-portal-windows.md).
2. Stellen Sie über Remotedesktop eine Verbindung mit der Problembehandlungs-VM her.
3. Erstellen Sie `gpt.ini` in `\Windows\System32\GroupPolicy` auf dem Laufwerk des virtuellen Quellcomputers (wenn die Datei „gpt.ini“ vorhanden ist, benennen Sie sie in „gpt.ini.bak“ um):
   
   > [!WARNING]
   > Stellen Sie sicher, dass Sie die folgenden Dateien nicht versehentlich in „C:\Windows“, dem Betriebssystemlaufwerk für den virtuellen Problembehandlungscomputer, erstellen. Erstellen Sie die folgenden Dateien im Betriebssystemlaufwerk für den virtuellen Quellcomputer, der als Datenträger angefügt ist.
   
   * Fügen Sie in der erstellten Datei `gpt.ini` die folgenden Zeilen ein:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Erstellen von „gpt.ini“](./media/reset-local-password-without-agent/create-gpt-ini.png)

4. Erstellen Sie `scripts.ini` in `\Windows\System32\GroupPolicy\Machines\Scripts\`. Stellen Sie sicher, dass ausgeblendete Ordner angezeigt werden. Erstellen Sie gegebenenfalls den Ordner `Machine` oder `Scripts`.
   
   * Fügen Sie in der erstellten Datei `scripts.ini` die folgenden Zeilen ein:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Erstellen von „scripts.ini“](./media/reset-local-password-without-agent/create-scripts-ini.png)

5. Erstellen Sie `FixAzureVM.cmd` in `\Windows\System32` mit dem folgenden Inhalt, und ersetzen Sie dabei `<username>` und `<newpassword>` durch Ihre eigenen Werte:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Erstellen von „FixAzureVM.cmd“](./media/reset-local-password-without-agent/create-fixazure-cmd.png)
   
    Beim Definieren des neuen Kennworts für den virtuellen Computer müssen Sie die konfigurierten Komplexitätsanforderungen für das Kennwort erfüllen.

6. Trennen Sie im Azure-Portal den Datenträger von der Problembehandlungs-VM.

7. [Wechseln Sie den Betriebssystemdatenträger für die betroffene VM](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm).

8. Nachdem der neue virtuelle Computer ausgeführt wird, stellen Sie mit dem neuen im Skript `FixAzureVM.cmd` angegebenen Kennwort per Remotedesktop eine Verbindung mit dem virtuellen Computer her.

9. Entfernen Sie die folgenden Dateien aus der Remotesitzung mit dem neuen virtuellen Computer, um die Umgebung zu bereinigen:
    
    * In „%windir%\System32“
      * Entfernen von „FixAzureVM.cmd“
    * Aus %windir%\System32\GroupPolicy\Machine\Scripts
      * Entfernen von „scripts.ini“
    * In „%windir%\System32\GroupPolicy“
      * Entfernen von „gpt.ini“ (Wenn „gpt.ini“ bereits vorhanden war und Sie die Datei in „gpt.ini.bak“ umbenannt haben, benennen Sie die BAK-Datei wieder in „gpt.ini“ um.)

## <a name="detailed-steps-for-classic-vm"></a>Ausführliche Schritte für klassische virtuelle Computer

> [!NOTE]
> Die Schritte gelten nicht für Windows-Domänencontroller. Sie können nur für einen eigenständigen Server oder einen Server ausgeführt werden, der Mitglied einer Domäne ist.

Versuchen Sie zunächst immer, ein Kennwort im [Azure-Portal oder mithilfe von Azure PowerShell](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/reset-rdp) zurückzusetzen, bevor Sie die folgenden Schritte ausführen. Vergewissern Sie sich zunächst, dass Sie über eine Sicherung des virtuellen Computers verfügen. 

1. Löschen Sie den betroffenen virtuellen Computer im Azure-Portal. Durch das Löschen des virtuellen Computers werden lediglich die Metadaten, d.h. der Verweis des virtuellen Computers in Azure, gelöscht. Die virtuellen Datenträger werden beim Löschen des virtuellen Computers beibehalten.
   
   * Wählen Sie im Azure-Portal den virtuellen Computer aus, und klicken Sie auf *Löschen*:
     
     ![Löschen einer vorhandener VM](./media/reset-local-password-without-agent/delete-vm-classic.png)

2. Fügen Sie den Betriebssystemdatenträger des virtuellen Quellcomputers an den virtuellen Problembehandlungscomputer an. Der virtuelle Problembehandlungscomputer muss sich in der gleichen Region wie der Betriebssystemdatenträger des virtuellen Quellcomputers befinden (z.B. `West US`):
   
   1. Wählen Sie den virtuellen Problembehandlungscomputer im Azure-Portal aus. Klicken Sie auf *Datenträger* | *Vorhandenen anfügen*:
     
      ![Anfügen eines vorhandenen Datenträgers](./media/reset-local-password-without-agent/disks-attach-existing-classic.png)
     
   2. Wählen Sie *VHD-Datei* und dann das Speicherkonto aus, das den virtuellen Quellcomputer enthält:
     
      ![Auswählen des Speicherkontos](./media/reset-local-password-without-agent/disks-select-storage-account-classic.png)
     
   3. Aktivieren Sie das Kontrollkästchen *Klassische Speicherkonten anzeigen*, und wählen Sie anschließend den Quellcontainer aus. Normalerweise ist *vhds* der Quellcontainer:
     
      ![Auswählen des Speichercontainers](./media/reset-local-password-without-agent/disks-select-container-classic.png)

      ![Auswählen des Speichercontainers](./media/reset-local-password-without-agent/disks-select-container-vhds-classic.png)
     
   4. Wählen Sie die anzufügende Betriebssystem-VHD aus. Klicken Sie auf *Auswählen*, um den Vorgang abzuschließen:
     
      ![Auswählen des virtuellen Quelldatenträgers](./media/reset-local-password-without-agent/disks-select-source-vhd-classic.png)

   5. Klicken auf „OK“, um den Datenträger anzufügen

      ![Anfügen eines vorhandenen Datenträgers](./media/reset-local-password-without-agent/disks-attach-okay-classic.png)

3. Stellen Sie per Remotedesktop eine Verbindung mit dem virtuellen Problembehandlungscomputer her, und prüfen Sie, ob der Betriebssystemdatenträger des virtuellen Quellcomputers angezeigt wird:

   1. Wählen Sie den virtuellen Problembehandlungscomputer im Azure-Portal aus, und klicken Sie auf *Verbinden*.

   2. Öffnen Sie die heruntergeladene RDP-Datei. Geben Sie den Benutzernamen und das Kennwort des virtuellen Problembehandlungscomputers ein.

   3. Suchen Sie im Datei-Explorer den angefügten Datenträger. Wenn die VHD des virtuellen Quellcomputers als einziger Datenträger an den virtuellen Problembehandlungscomputer angefügt ist, sollte es sich um Laufwerk F: handeln:
     
      ![Anzeigen des angefügten Datenträgers](./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer-classic.png)

4. Erstellen Sie `gpt.ini` in `\Windows\System32\GroupPolicy` auf dem Laufwerk des virtuellen Quellcomputers. (Sollte die Datei `gpt.ini` bereits vorhanden sein, benennen Sie sie in `gpt.ini.bak` um.)
   
   > [!WARNING]
   > Achten Sie darauf, die folgenden Dateien nicht versehentlich in `C:\Windows` (dem Betriebssystemlaufwerk für den virtuellen Problembehandlungscomputer) zu erstellen. Erstellen Sie die folgenden Dateien im Betriebssystemlaufwerk für den virtuellen Quellcomputer, der als Datenträger angefügt ist.
   
   * Fügen Sie in der erstellten Datei `gpt.ini` die folgenden Zeilen ein:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Erstellen von „gpt.ini“](./media/reset-local-password-without-agent/create-gpt-ini-classic.png)

5. Erstellen Sie `scripts.ini` in `\Windows\System32\GroupPolicy\Machines\Scripts\`. Stellen Sie sicher, dass ausgeblendete Ordner angezeigt werden. Erstellen Sie gegebenenfalls den Ordner `Machine` oder `Scripts`.
   
   * Fügen Sie in der erstellten Datei `scripts.ini` die folgenden Zeilen ein:

     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Erstellen von „scripts.ini“](./media/reset-local-password-without-agent/create-scripts-ini-classic.png)

6. Erstellen Sie `FixAzureVM.cmd` in `\Windows\System32` mit dem folgenden Inhalt, und ersetzen Sie dabei `<username>` und `<newpassword>` durch Ihre eigenen Werte:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Erstellen von „FixAzureVM.cmd“](./media/reset-local-password-without-agent/create-fixazure-cmd-classic.png)
   
    Beim Definieren des neuen Kennworts für den virtuellen Computer müssen Sie die konfigurierten Komplexitätsanforderungen für das Kennwort erfüllen.

7. Trennen Sie im Azure-Portal den Datenträger vom virtuellen Problembehandlungscomputer:
   
   1. Wählen Sie den virtuellen Problembehandlungscomputer im Azure-Portal aus, und klicken Sie auf *Datenträger*.
   
   2. Wählen Sie den in Schritt 2 angefügten Datenträger aus, klicken Sie auf **Trennen**, und klicken Sie anschließend auf **OK**.

     ![Trennen des Datenträgers](./media/reset-local-password-without-agent/data-disks-classic.png)
     
     ![Trennen des Datenträgers](./media/reset-local-password-without-agent/detach-disk-classic.png)

8. Erstellen Sie einen virtuellen Computer über den Betriebssystemdatenträger des virtuellen Quellcomputers:
   
     ![Erstellen eines virtuellen Computers über eine Vorlage](./media/reset-local-password-without-agent/create-new-vm-from-template-classic.png)

     ![Erstellen eines virtuellen Computers über eine Vorlage](./media/reset-local-password-without-agent/choose-subscription-classic.png)

     ![Erstellen eines virtuellen Computers über eine Vorlage](./media/reset-local-password-without-agent/create-vm-classic.png)

## <a name="complete-the-create-virtual-machine-experience"></a>Fertigstellen der VM-Erstellung

1. Nachdem der neue virtuelle Computer ausgeführt wird, stellen Sie mit dem neuen im Skript `FixAzureVM.cmd` angegebenen Kennwort per Remotedesktop eine Verbindung mit dem virtuellen Computer her.

2. Entfernen Sie die folgenden Dateien aus der Remotesitzung mit dem neuen virtuellen Computer, um die Umgebung zu bereinigen:
    
    * Gehen Sie unter „`%windir%\System32`“ wie folgt vor:
      * Entfernen Sie „`FixAzureVM.cmd`“.
    * Gehen Sie unter „`%windir%\System32\GroupPolicy\Machine\Scripts`“ wie folgt vor:
      * Entfernen Sie „`scripts.ini`“.
    * Gehen Sie unter „`%windir%\System32\GroupPolicy`“ wie folgt vor:
      * Entfernen Sie „`gpt.ini`“. (Falls „`gpt.ini`“ zuvor bereits vorhanden war und Sie die Datei in „`gpt.ini.bak`“ umbenannt haben, benennen Sie „`.bak`“ wieder in „`gpt.ini`“ um.)

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie dennoch keine Verbindung per Remotedesktop herstellen können, finden Sie weitere Informationen in der [Anleitung zur Problembehandlung bei Remotedesktopverbindungen](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). In der [ausführlichen Problembehandlung für Remotedesktopverbindungen](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) werden statt spezifischer Schritte allgemeine Problembehandlungsmethoden aufgezeigt. Sie können zudem [eine Azure-Supportanfrage stellen](https://azure.microsoft.com/support/options/), um praktische Unterstützung zu erhalten.
