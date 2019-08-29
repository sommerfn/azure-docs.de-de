---
title: Zurücksetzen des lokalen Linux-Kennworts auf Azure-VMs | Microsoft-Dokumentation
description: Führen Sie die Schritte zum Zurücksetzen des lokalen Linux-Kennworts auf der Azure-VM ein
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: delhan
ms.openlocfilehash: 8fc51dfb90158316b3fe6c11b5265f1cf3251505
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641049"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Zurücksetzen des lokalen Linux-Kennworts auf Azure-VMs

In diesem Artikel werden mehrere Methoden zum Zurücksetzen des lokalen Kennworts des virtuellen Linux-Computers (VM) vorgestellt. Wenn das Benutzerkonto abgelaufen ist oder Sie ein neues Konto erstellen möchten, können Sie die folgenden Methoden nutzen, um ein neues lokales Administratorkonto zu erstellen und erneut Zugriff auf den virtuellen Computer zu erhalten.

## <a name="symptoms"></a>Symptome

Sie können sich nicht am virtuellen Computer anmelden, und Sie erhalten eine Nachricht, die angibt, dass das von Ihnen verwendete Kennwort falsch ist. Darüber hinaus können Sie VMAgent nicht verwenden, um Ihr Kennwort im Azure-Portal zurückzusetzen.

## <a name="manual-password-reset-procedure"></a>Verfahren zur manuellen Kennwortzurücksetzung

> [!NOTE]
> Die folgenden Schritte gelten nicht für eine VM mit einem nicht verwalteten Datenträger.

1. Erstellen Sie eine Momentaufnahme für den Betriebssystemdatenträger der betroffenen VM, erstellen Sie aus der Momentaufnahme einen Datenträger, und fügen Sie diesen an eine Problembehandlungs-VM an. Weitere Informationen finden Sie unter [Beheben von Problemen mit einer Windows-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM im Azure-Portal](troubleshoot-recovery-disks-portal-linux.md).

2. Stellen Sie über Remotedesktop eine Verbindung mit der Problembehandlungs-VM her.

3.  Führen Sie den folgenden SSH-Befehl auf der Problembehandlungs-VM aus, um ein Administrator zu werden.

    ```bash
    sudo su
    ```

4.  Führen Sie **fdisk -l** aus, oder sehen Sie sich die Systemprotokolle an, um den neu angefügten Datenträger zu finden. Suchen Sie den bereitzustellenden Laufwerknamen. Schauen Sie sich anschließend auf dem temporären virtuellen Computer die entsprechende Protokolldatei an.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    Im Folgenden finden Sie eine Beispielausgabe des „grep“-Befehls:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  Erstellen Sie einen Bereitstellungspunkt mit dem Namen **tempmount**.

    ```bash
    mkdir /tempmount
    ```

6.  Stellen Sie den Betriebssystemdatenträger auf dem Bereitstellungspunkt bereit. Sie müssen in der Regel *sdc1* oder *sdc2* bereitstellen. Dies hängt von der Hostpartition im Verzeichnis */etc* auf der fehlerhaften VM-Festplatte ab.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Erstellen Sie Kopien der Kernanmeldeinformations-Dateien, bevor Sie Änderungen vornehmen:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Setzen Sie das Kennwort des Benutzers zurück, das Sie benötigen:

    ```bash
    passwd <<USER>> 
    ```

9.  Verschieben Sie die bearbeiteten Dateien an den richtigen Ort auf der Festplatte des fehlerhaften Computers.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Wechseln Sie zurück in das Stammverzeichnis, und heben Sie die Einbindung des Datenträgers auf.

    ```bash
    cd /
    umount /tempmount
    ```

11. Trennen Sie im Azure-Portal den Datenträger von der Problembehandlungs-VM.

12. [Ändern des Betriebssystemdatenträgers für die betroffene VM](troubleshoot-recovery-disks-portal-linux.md#swap-the-os-disk-for-the-vm)

## <a name="next-steps"></a>Nächste Schritte

* [Beheben von Problemen bei virtuellen Azure-Computern durch Anfügen von Betriebssystem-Datenträgern an andere Azure-VMs](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure-Befehlszeilenschnittstelle: Löschen und erneutes Bereitstellen virtueller Computer von einer VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
