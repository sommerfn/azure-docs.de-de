---
title: Linux-VM startet mit Grub Rescue
description: Der virtuelle Computer konnte nicht gestartet werden, da auf ihm eine Rettungskonsole gestartet wurde.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/28/2019
ms.author: tiag
ms.openlocfilehash: 29242b802dbbff4218506422293082a495c4d21e
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685133"
---
# <a name="linux-vm-boots-to-grub-rescue"></a>Linux-VM startet mit Grub Rescue

Wir haben festgestellt, dass auf Ihrem virtuellen Computer (virtual machine, VM) eine Rettungskonsole gestartet wurde. Dieses Problem tritt auf, wenn auf Ihrem virtuellen Linux-Computer kürzlich Kerneländerungen (beispielsweise ein Kernelupgrade) vorgenommen wurden und der Computer aufgrund von Kernelfehlern während des Startprozesses nicht mehr ordnungsgemäß startet. Wenn das Startladeprogramm im Zuge des Startprozesses versucht, den Linux-Kernel zu finden und die Startsteuerung an ihn zu übergeben, diese Übergabe aber nicht erfolgreich ist, wird für den virtuellen Computer eine Rettungskonsole gestartet.

Sollten Sie künftig mit einem virtuellen Computer keine Verbindung herstellen können, können Sie im Azure-Portal über das Startdiagnoseblatt einen Screenshot Ihres virtuellen Computers anzeigen. Dadurch können Sie das Problem unter Umständen diagnostizieren und bestimmen, ob ein ähnlicher Startfehler die Ursache ist.

## <a name="recommended-steps"></a>Empfohlene Schritte

Führen Sie die passenden Problembehandlungsschritte für Ihren Fehler aus:

### <a name="error---unknown-filesystem"></a>Fehler: Unbekanntes Dateisystem

* Der Fehler **Unbekanntes Dateisystem** kann auf eine Beschädigung des Dateisystems auf der Startpartition oder auf eine falsche Kernelkonfiguration zurückzuführen sein.

   * Informationen zur Vorgehensweise bei Dateisystemproblemen finden Sie im Artikel [Linux Recovery: Cannot SSH to Linux VM due to file system errors (fsck, inodes)](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/) (Linux-Wiederherstellung: Aufgrund von Dateisystemfehlern keine SSH-Verbindung mit virtuellem Linux-Computer möglich (fsck, inodes)).
   * Informationen zur Vorgehensweise bei Kernelproblemen finden Sie im Artikel [Linux Recovery: Manually fixing non-boot issues related to Kernel problems](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/) (Linux-Wiederherstellung: Manuelles Beheben nicht startbezogener Probleme im Zusammenhang mit Kernelproblemen) oder unter [Linux Recovery: Fixing non-boot issues related to Kernel problems using chroot](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-fixing-non-boot-issues-related-to-kernel-problems-using-chroot/) (Linux-Wiederherstellung: Beheben nicht startbezogener Probleme im Zusammenhang mit Kernelproblemen mithilfe von chroot).
   
### <a name="error---file-not-found"></a>Fehler: Datei nicht gefunden

* Wenn Sie den Fehler **Error 15: File not found or initial RAM disk** (Fehler 15: Datei nicht gefunden oder anfänglicher RAM-Datenträger) oder **initrd/initramfs file not found** (initrd/initramfs-Datei nicht gefunden) erhalten, führen Sie die folgenden Schritte aus:

    * Gehen Sie für die fehlende Datei `/boot/grub2/grub.cfg` oder `initrd/initramfs` wie folgt vor:

    1. Vergewissern Sie sich, dass `/etc/default/grub` vorhanden ist und über die korrekten/gewünschten Einstellungen verfügt. Die Standardeinstellungen können Sie bei Bedarf auf einem funktionierenden virtuellen Computer ermitteln.

    2. Führen Sie als Nächstes den folgenden Befehl aus, um die Konfiguration erneut zu generieren: `grub2-mkconfig -o /boot/grub2/grub.cfg`

   * Sollte die Datei `/boot/grub/menu.lst` fehlen, gilt dieser Fehler für ältere Betriebssystemversionen (**RHEL 6.x**, **Centos 6.x** und **Ubuntu 14.04**), sodass die Befehle möglicherweise abweichen. Testen Sie anhand eines alten Servers, ob die Befehle korrekt sind.

### <a name="error---no-such-partition"></a>Fehler: Keine entsprechende Partition vorhanden

* Wenn Sie den Fehler **No such partition** (Keine entsprechende Partition vorhanden) erhalten, lesen Sie [Case Scenario : “no such partition” error while trying to start the VM after attempting to extend the OS drive](https://blogs.technet.microsoft.com/shwetanayak/2017/03/12/case-scenario-no-such-partition-error-while-trying-to-start-the-vm-after-attempting-to-extend-the-os-drive/) (Fallszenario: Fehler „no such partition“ (Keine entsprechende Partition vorhanden) beim Starten des virtuellen Computers nach dem Versuch, das Betriebssystemlaufwerk zu erweitern).

### <a name="error---grubcfg-file-not-found"></a>Fehler: Datei „grub.cfg“ nicht gefunden

* Wenn Sie den Fehler **/boot/grub2/grub.cfg file not found** (Datei „/boot/grub2/grub.cfg“ nicht gefunden) erhalten, führen Sie die folgenden Schritte aus:

    * Gehen Sie für die fehlende Datei `/boot/grub2/grub.cfg` oder `initrd/initramfs` wie folgt vor:

    1. Vergewissern Sie sich, dass `/etc/default/grub` vorhanden ist und über die korrekten/gewünschten Einstellungen verfügt. Die Standardeinstellungen können Sie bei Bedarf auf einem funktionierenden virtuellen Computer ermitteln.

    2. Führen Sie als Nächstes den folgenden Befehl aus, um die Konfiguration erneut zu generieren: `grub2-mkconfig -o /boot/grub2/grub.cfg`.

   * Sollte die Datei `/boot/grub/menu.lst` fehlen, gilt dieser Fehler für ältere Betriebssystemversionen (**RHEL 6.x**, **Centos 6.x** und **Ubuntu 14.04**), sodass die Befehle möglicherweise abweichen. Testen Sie anhand eines alten Servers, ob die Befehle korrekt sind.

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über den Agent für virtuelle Azure-Computer](../extensions/agent-windows.md)
* [Erweiterungen und Features für virtuelle Computer für Windows](../extensions/features-windows.md)

