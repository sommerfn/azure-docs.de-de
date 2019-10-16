---
title: Beheben von Problemen beim Starten von Linux-VMs aufgrund von Fehlern in „fstab“ | Microsoft-Dokumentation
description: Erläutert, warum Linux-VMs nicht gestartet werden können und wie das Problem behoben wird.
services: virtual-machines-linux
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 10/09/2019
ms.author: v-six
ms.openlocfilehash: 868a0238092786d0999a6a41de71d30011bbef7a
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246035"
---
# <a name="troubleshoot-linux-vm-starting-issues-due-to-fstab-errors"></a>Beheben von Problemen beim Starten von Linux-VMs aufgrund von Fehlern in „fstab“

Sie können über Secure Shell (SSH) keine Verbindung mit einem virtuellen Azure-Linux-Computer (VM) herstellen. Wenn Sie die [Startdiagnose](https://portal.azure.com/) im [Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) ausführen, werden Protokolleinträge aufgeführt, die den folgenden Beispielen ähneln:

## <a name="examples"></a>Beispiele

Im Folgenden finden Sie einige Beispiele für mögliche Fehler.

### <a name="example-1-a-disk-is-mounted-by-the-scsi-id-instead-of-the-universally-unique-identifier-uuid"></a>Beispiel 1: Ein Datenträger wird anhand der SCSI-ID anstelle des universell eindeutigen Bezeichners (UUID) eingebunden.

```
[K[[1;31m TIME [0m] Timed out waiting for device dev-incorrect.device.
[[1;33mDEPEND[0m] Dependency failed for /data.
[[1;33mDEPEND[0m] Dependency failed for Local File Systems.
…
Welcome to emergency mode! After logging in, type “journalctl -xb” to viewsystem logs, “systemctl reboot” to reboot, “systemctl default” to try again to boot into default mode.
Give root password for maintenance
(or type Control-D to continue)
```

### <a name="example-2-an-unattached-device-is-missing-on-centos"></a>Beispiel 2: In CentOS fehlt ein nicht angefügtes Gerät.

```
Checking file systems…
fsck from util-linux 2.19.1
Checking all file systems.
/dev/sdc1: nonexistent device (“nofail” fstab option may be used to skip this device)
/dev/sdd1: nonexistent device (“nofail” fstab option may be used to skip this device)
/dev/sde1: nonexistent device (“nofail” fstab option may be used to skip this device)

[/sbin/fsck.ext3 (1) — /CODE] sck.ext3 -a /dev/sdc1
fsck.ext3: No such file or directory while trying to open /dev/sdc1

/dev/sdc1:
The superblock could not be read or does not describe a correct ext2
filesystem. If the device is valid and it really contains an ext2
filesystem (and not swap or ufs or something else), then the superblock
is corrupt, and you might try running e2fsck with an alternate superblock:

e2fsck -b 8193 <device>

[/sbin/fsck.xfs (1) — /GLUSTERDISK] fsck.xfs -a /dev/sdd1
/sbin/fsck.xfs: /dev/sdd1 does not exist
[/sbin/fsck.ext3 (1) — /DATATEMP] fsck.ext3 -a /dev/sde1 fsck.ext3: No such file or directory while trying to open /dev/sde1
```

### <a name="example-3-a-vm-cannot-start-because-of-an-fstab-misconfiguration-or-because-the-disk-is-no-longer-attached"></a>Beispiel 3: Ein virtueller Computer kann aufgrund einer fehlerhafter Konfiguration der Datei „fstab“ oder eines nicht mehr angefügten Datenträgers nicht gestartet werden.

```
The disk drive for /var/lib/mysql is not ready yet or not present.
Continue to wait, or Press S to skip mounting or M for manual recovery
```

### <a name="example-4-a-serial-log-entry-shows-an-incorrect-uuid"></a>Beispiel 4: Ein serieller Protokolleintrag weist eine falsche UUID auf.

```
Checking filesystems
Checking all file systems.
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1: clean, 70442/1905008 files, 800094/7608064 blocks
[/sbin/fsck.ext4 (1) — /datadrive] fsck.ext4 -a UUID="<UUID>"
fsck.ext4: Unable to resolve UUID="<UUID>"
[FAILED

*** An error occurred during the file system check.
*** Dropping you to a shell; the system will reboot
*** when you leave the shell.
*** Warning — SELinux is active
*** Disabling security enforcement for system recovery.
*** Run ‘setenforce 1’ to reenable.
type=1404 audit(1428047455.949:4): enforcing=0 old_enforcing=1 auid=<AUID> ses=4294967295
Give root password for maintenance
(or type Control-D to continue)
```

Dieses Problem tritt möglicherweise auf, wenn die Syntax der Dateisystemtabelle (File Systems Table, fstab) nicht korrekt ist oder wenn ein erforderlicher Datenträger, der einem Eintrag in der Datei „/etc/fstab“ zugeordnet ist, nicht an die VM angefügt ist.

## <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, starten Sie den virtuellen Computer über die serielle Konsole für Azure Virtual Machines im Notfallmodus. Verwenden Sie dann das Tool zum Reparieren des Dateisystems. Wenn die serielle Konsole auf Ihrem virtuellen Computer nicht aktiviert ist, wechseln Sie zum Abschnitt [Reparieren des virtuellen Computers im Offlinestatus](#repair-the-vm-offline).

## <a name="use-the-serial-console"></a>Verwenden der seriellen Konsole

1. Stellen Sie eine Verbindung mit der [seriellen Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux) her.
2. Melden Sie sich mit einem lokalen Benutzer und Kennwort beim System an.

   > [!Note]
   > Sie können keinen SSH-Schlüssel verwenden, um sich an der seriellen Konsole beim System anzumelden.

3. Suchen Sie nach dem Fehler, der angibt, dass der Datenträger nicht eingebunden wurde. Im folgenden Beispiel wurde im System versucht, einen nicht mehr vorhandenen Datenträger anzufügen:

   ```
   [DEPEND] Dependency failed for /datadisk1.
   [DEPEND] Dependency failed for Local File Systems.
   [DEPEND] Dependency failed for Relabel all filesystems, if necessary.
   [DEPEND] Dependency failed for Migrate local... structure to the new structure.
   Welcome to emergency mode! After logging in, type "journalctl -xb" to view
   system logs, "systemctl reboot" to reboot, "systemctl default" or ^D to try again to boot into default mode.
   Give root password for maintenance
   (or type Control-D to continue):
   ```

4. Stellen Sie über das Root-Kennwort eine Verbindung mit der VM her (Red Hat-basierte VMs).

5. Öffnen Sie die Datei „fstab“ mit Ihrem bevorzugten Text-Editor. Nachdem der Datenträger eingebunden wurde, führen Sie den folgenden Befehl für Nano aus:

   ```
   $ nano /mnt/troubleshootingdisk/etc/fstab
   ```

6. Überprüfen Sie die aufgeführten Dateisysteme. Jede Zeile in der Datei „fstab“ gibt ein Dateisystem an, das beim Starten des virtuellen Computers eingebunden wird. Weitere Informationen zur Syntax der Datei „fstab“ erhalten Sie mit dem Befehl „man fstab“. Um einen Startfehler zu beheben, überprüfen Sie jede Zeile, um sicherzustellen, dass sowohl ihre Struktur als auch ihr Inhalt korrekt sind.

   > [!Note]
   > * Felder in den einzelnen Zeilen werden durch Tabstopps oder Leerzeichen voneinander getrennt. Leere Zeilen werden ignoriert. Zeilen, die als erstes Zeichen ein Nummernzeichen (#) aufweisen, sind Kommentare. Auskommentierte Zeilen können in der Datei „fstab“ verbleiben, werden aber nicht verarbeitet. Es wird empfohlen, dass Sie die Zeilen in „fstab“, bei denen Sie unsicher sind, nicht entfernen, sondern auskommentieren.
   > * Damit der virtuelle Computer wiederhergestellt und gestartet werden kann, sollten die Dateisystempartitionen die einzigen erforderlichen Partitionen sein. Auf dem virtuellen Computer treten möglicherweise Anwendungsfehler zu zusätzlichen kommentierten Partitionen auf. Der virtuelle Computer sollte jedoch ohne die zusätzlichen Partitionen gestartet werden. Sie können die Auskommentierung etwaiger kommentierter Zeilen später wieder aufheben.
   > * Es wird empfohlen, Datenträger auf Azure-VMs über die UUID der Dateisystempartition einzubinden. Führen Sie beispielsweise den folgenden Befehl aus: ``/dev/sdc1: LABEL="cloudimg-rootfs" UUID="<UUID>" TYPE="ext4" PARTUUID="<PartUUID>"``
   > * Führen Sie den Befehl „blkid“ aus, um die UUID des Dateisystems zu ermitteln. Weitere Informationen zur Syntax erhalten Sie durch Ausführen des Befehls „blkid“.
   > * Mit der Option „nofail“ können Sie sicherstellen, dass der virtuelle Computer auch dann gestartet wird, wenn das Dateisystem beschädigt oder beim Start nicht vorhanden ist. Es wird empfohlen, die Option „nofail“ in der Datei „fstab“ zu verwenden, damit der Startvorgang nach Fehlern in Partitionen, die für den Start des virtuellen Computers nicht erforderlich sind, fortgesetzt werden kann.

7. Ändern Sie falsche oder unnötige Zeilen in der Datei „fstab“, oder kommentieren Sie sie aus, damit der virtuelle Computer ordnungsgemäß gestartet werden kann.

8. Speichern Sie die Änderungen an der Datei „fstab“.

9. Starten Sie den virtuellen Computer neu.

10. Wenn die Auskommentierung oder Korrektur der Einträge erfolgreich war, sollte das System eine Bash-Eingabeaufforderung im Portal erreichen. Überprüfen Sie, ob Sie eine Verbindung mit der VM herstellen können.

11. Überprüfen Sie die Bereitstellungspunkte, wenn Sie Änderungen an „fstab“ testen, indem Sie den Befehl „mount -a“ ausführen. Wenn keine Fehler auftreten, sollten die Bereitstellungspunkte in Ordnung sein.

## <a name="repair-the-vm-offline"></a>Reparieren des virtuellen Computers im Offlinestatus

1. Fügen Sie den Systemdatenträger des virtuellen Computers als Datenträger für Daten an eine Wiederherstellungs-VM (beliebige funktionierende Linux-VM) an. Hierzu können Sie [CLI-Befehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux) verwenden, oder Sie können die Einrichtung der Wiederherstellungs-VM mithilfe der [VM-Reparaturbefehle](repair-linux-vm-using-azure-virtual-machine-repair-commands.md) automatisieren.

2. Nachdem Sie den Systemdatenträger als Datenträger für Daten auf der Wiederherstellungs-VM eingebunden haben, sichern Sie die Datei „fstab“, bevor Sie Änderungen vornehmen, und führen Sie dann die folgenden Schritte aus, um die Datei „fstab“ zu korrigieren.

3.  Suchen Sie nach dem Fehler, der angibt, dass der Datenträger nicht eingebunden wurde. Im folgenden Beispiel wurde im System versucht, einen nicht mehr vorhandenen Datenträger anzufügen:

    ```
    [DEPEND] Dependency failed for /datadisk1.
    [DEPEND] Dependency failed for Local File Systems.
    [DEPEND] Dependency failed for Relabel all filesystems, if necessary.
    [DEPEND] Dependency failed for Migrate local... structure to the new structure.
    Welcome to emergency mode! After logging in, type "journalctl -xb" to view system logs, "systemctl reboot" to reboot, "systemctl default" or ^D to try again to boot into default mode.
    Give root password for maintenance (or type Control-D to continue):
    ```

4. Stellen Sie über das Root-Kennwort eine Verbindung mit der VM her (Red Hat-basierte VMs).

5. Öffnen Sie die Datei „fstab“ mit Ihrem bevorzugten Text-Editor. Nachdem der Datenträger eingebunden wurde, führen Sie den folgenden Befehl für Nano aus. Stellen Sie sicher, dass Sie an der Datei „fstab“ arbeiten, die sich auf dem eingebundenen Datenträger befindet, und nicht an der auf der Notfall-VM.

   ```
   $ nano /mnt/troubleshootingdisk/etc/fstab
   ```

6. Überprüfen Sie die aufgeführten Dateisysteme. Jede Zeile in der Datei „fstab“ gibt ein Dateisystem an, das beim Starten des virtuellen Computers eingebunden wird. Weitere Informationen zur Syntax der Datei „fstab“ erhalten Sie mit dem Befehl „man fstab“. Um einen Startfehler zu beheben, überprüfen Sie jede Zeile, um sicherzustellen, dass sowohl ihre Struktur als auch ihr Inhalt korrekt sind.

   > [!Note]
   > * Felder in den einzelnen Zeilen werden durch Tabstopps oder Leerzeichen voneinander getrennt. Leere Zeilen werden ignoriert. Zeilen, die als erstes Zeichen ein Nummernzeichen (#) aufweisen, sind Kommentare. Auskommentierte Zeilen können in der Datei „fstab“ verbleiben, werden aber nicht verarbeitet. Es wird empfohlen, dass Sie die Zeilen in „fstab“, bei denen Sie unsicher sind, nicht entfernen, sondern auskommentieren.
   > * Damit der virtuelle Computer wiederhergestellt und gestartet werden kann, sollten die Dateisystempartitionen die einzigen erforderlichen Partitionen sein. Auf dem virtuellen Computer treten möglicherweise Anwendungsfehler zu zusätzlichen kommentierten Partitionen auf. Der virtuelle Computer sollte jedoch ohne die zusätzlichen Partitionen gestartet werden. Sie können die Auskommentierung etwaiger kommentierter Zeilen später wieder aufheben.
   > * Es wird empfohlen, Datenträger auf Azure-VMs über die UUID der Dateisystempartition einzubinden. Führen Sie beispielsweise den folgenden Befehl aus: ``/dev/sdc1: LABEL="cloudimg-rootfs" UUID="<UUID>" TYPE="ext4" PARTUUID="<PartUUID>"``
   > * Führen Sie den Befehl „blkid“ aus, um die UUID des Dateisystems zu ermitteln. Weitere Informationen zur Syntax erhalten Sie durch Ausführen des Befehls „blkid“. Beachten Sie, dass der Datenträger, den Sie wiederherstellen möchten, jetzt auf einem neuen virtuellen Computer eingebunden wird. Obwohl die UUIDs konsistent sein sollten, unterscheiden sich die Gerätepartitions-IDs (z. B. „/dev/sda1“) auf dieser VM. Die Dateisystempartitionen der ursprünglichen fehlerhaften VM, die sich auf einer Nicht-System-VHD befinden, sind für die Wiederherstellungs-VM [über CLI-Befehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux) nicht verfügbar.
   > * Mit der Option „nofail“ können Sie sicherstellen, dass der virtuelle Computer auch dann gestartet wird, wenn das Dateisystem beschädigt oder beim Start nicht vorhanden ist. Es wird empfohlen, die Option „nofail“ in der Datei „fstab“ zu verwenden, damit der Startvorgang nach Fehlern in Partitionen, die für den Start des virtuellen Computers nicht erforderlich sind, fortgesetzt werden kann.

7. Ändern Sie falsche oder unnötige Zeilen in der Datei „fstab“, oder kommentieren Sie sie aus, damit der virtuelle Computer ordnungsgemäß gestartet werden kann.

8. Speichern Sie die Änderungen an der Datei „fstab“.

9. Starten Sie den virtuellen Computer neu, oder erstellen Sie die ursprüngliche VM neu.

10. Wenn die Auskommentierung oder Korrektur der Einträge erfolgreich war, sollte das System eine Bash-Eingabeaufforderung im Portal erreichen. Überprüfen Sie, ob Sie eine Verbindung mit der VM herstellen können.

11. Überprüfen Sie die Bereitstellungspunkte, wenn Sie Änderungen an „fstab“ testen, indem Sie den Befehl „mount -a“ ausführen. Wenn keine Fehler auftreten, sollten die Bereitstellungspunkte in Ordnung sein.

12. Heben Sie die Einbindung der ursprünglichen virtuellen Festplatte auf, und trennen Sie sie. Erstellen Sie dann eine VM mit dem ursprünglichen Systemdatenträger. Hierzu können Sie [CLI-Befehle](troubleshoot-recovery-disks-linux.md) verwenden oder die [VM-Reparaturbefehle](repair-linux-vm-using-azure-virtual-machine-repair-commands.md), wenn Sie diese zum Erstellen der Wiederherstellungs-VM verwendet haben.

13. Wenn Sie den virtuellen Computer erneut erstellt haben und über SSH eine Verbindung mit ihm herstellen können, führen Sie die folgenden Aktionen aus:
    * Überprüfen Sie alle im Rahmen der Wiederherstellung geänderten oder auskommentierten Zeilen in der Datei „fstab“.
    * Stellen Sie sicher, dass Sie die UUID und die Option „nofail“ ordnungsgemäß verwenden.
    * Testen Sie alle vorgenommenen Änderungen an „fstab“, bevor Sie den virtuellen Computer neu starten. Führen Sie dazu den folgenden Befehl aus: ``$ sudo mount -a``
    * Erstellen Sie eine zusätzliche Kopie der korrigierten Datei „fstab“ für zukünftige Wiederherstellungsszenarien.

## <a name="next-steps"></a>Nächste Schritte

* [Beheben von Problemen einer Linux-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM mithilfe der Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-troubleshoot-recovery-disks)
* [Beheben von Problemen einer Linux-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM mit dem Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-recovery-disks-portal)

