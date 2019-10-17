---
title: Beheben von Problemen beim Starten von Linux-VMs aufgrund von Dateisystemfehlern | Microsoft-Dokumentation
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
ms.openlocfilehash: a47dc1032115f8bcae0c7bdc37c84ab3b68ec4a8
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72432314"
---
# <a name="troubleshoot-linux-vm-starting-issues-due-to-file-system-errors"></a>Beheben von Problemen beim Starten von Linux-VMs aufgrund von Dateisystemfehlern

Sie können über Secure Shell (SSH) keine Verbindung mit einer Azure-Linux-VM (virtueller Computer) herstellen. Wenn Sie die Startdiagnosefunktion im [Azure-Portal](https://portal.azure.com/) ausführen, werden Protokolleinträge aufgeführt, die den folgenden Beispielen ähneln.

## <a name="examples"></a>Beispiele

Im Folgenden finden Sie einige Beispiele für mögliche Fehler.

### <a name="example-1"></a>Beispiel 1 

```
Checking all file systems.
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1 contains a file system with errors, check forced.
/dev/sda1: Inodes that were part of a corrupted orphan linked list found.
/dev/sda1: UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY
```

### <a name="example-2"></a>Beispiel 2

```
EXT4-fs (sda1): INFO: recovery required on readonly filesystem
EXT4-fs (sda1): write access will be enabled during recovery
EXT4-fs warning (device sda1): ext4_clear_journal_err:4531: Filesystem error recorded from previous mount: IO failure
EXT4-fs warning (device sda1): ext4_clear_journal_err:4532: Making fs in need of filesystem check.
```

### <a name="example-3"></a>Beispiel 3

```
[  14.252404] EXT4-fs (sda1): Couldn’t remount RDWR because of unprocessed orphan inode list.  Please unmount/remount instead
An error occurred while mounting /.
```

### <a name="example-4"></a>Beispiel 4 

Dieses Beispiel wird durch einen sauberen fsck-Befehl verursacht. In diesem Fall sind auch zusätzliche Datenträger an die VM angefügt („/dev/sdc1“ und „/dev/sde1“).

```
Checking all file systems. 
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1: clean, 65405/1905008 files, 732749/7608064 blocks
[/sbin/fsck.ext4 (1) — /tmp] fsck.ext4 -a /dev/sdc1
[/sbin/fsck.ext4 (2) — /backup] fsck.ext4 -a /dev/sde1
/dev/sdc1: clean, 12/1048576 files, 109842/4192957 blocks
/dev/sde1 : clean, 51/67043328 files, 4259482/268173037 blocks
```

Dieses Problem kann auftreten, wenn das Dateisystem nicht ordnungsgemäß heruntergefahren wurde oder Probleme im Zusammenhang mit dem Speicher vorliegen. Zu den Problemen zählen Hardware- oder Softwarefehler, Probleme mit Treibern oder Programmen, Schreibfehler usw. Es ist immer wichtig, über eine Sicherung kritischer Daten zu verfügen. Die in diesem Artikel beschriebenen Tools unterstützen möglicherweise die Wiederherstellung von Dateisystemen, es kann jedoch trotzdem zu Datenverlusten kommen.

Für Linux sind mehrere Tools zur Dateisystemüberprüfung verfügbar. Die häufigsten für die Distributionen in Azure sind [FSCK](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/fsck-fs-specific), [E2FSCK](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/fsck-fs-specific) und [Xfs_repair](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/xfsrepair).

## <a name="resolution"></a>Lösung

Um dieses Problem zu beheben, starten Sie den virtuellen Computer mithilfe der [seriellen Konsole](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux) im Notfallmodus, und reparieren Sie das Dateisystem mit diesem Tool. Wenn die serielle Konsole auf Ihrem virtuellen Computer nicht aktiviert ist, finden Sie im Abschnitt [Reparieren des virtuellen Computers im Offlinestatus](#repair-the-vm-offline) dieses Artikels weitere Informationen.

## <a name="use-the-serial-console"></a>Verwenden der seriellen Konsole

1. Stellen Sie eine Verbindung mit der seriellen Konsole her.

   > [!Note]
   > Weitere Informationen zur seriellen Konsole für Linux finden Sie unter:
   > * [Verwenden der seriellen Konsole für den Zugriff auf den GRUB- und den Einzelbenutzermodus](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console-grub-single-user-mode)
   > * [Verwenden der seriellen Konsole für SysRq- und NMI-Aufrufe](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-nmi-sysrq)

2. Wählen Sie die Schaltfläche des Stromversorgungssymbols und dann „VM neu starten“ aus. (Wenn die serielle Konsole nicht aktiviert oder nicht ordnungsgemäß verbunden ist, wird die Schaltfläche nicht angezeigt.)

   ![IMAGE](./media/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck/restart-vm.png)

3. Starten Sie den virtuellen Computer im Notfallmodus.

4. Geben Sie Ihr Superuser-Kennwort ein, um sich im Notfallmodus anzumelden.

5. Verwenden Sie xfs_repair mit der Option „-n“, um die Fehler im Dateisystem zu erkennen. Im folgenden Beispiel wird davon ausgegangen, dass die Systempartition „/dev/sda1“ ist. Ersetzen Sie dies durch den entsprechenden Wert für Ihre VM:

   ```
   xfs_repair -n /dev/sda1
   ```

6. Führen Sie den folgenden Befehl aus, um das Dateisystem zu reparieren:

   ```
   xfs_repair /dev/sda1
   ```

7. Wenn die Fehlermeldung „FEHLER: Das Dateisystem weist wichtige Metadatenänderungen in einem Protokoll auf, das wiedergegeben werden muss“ ausgegeben wird, erstellen Sie ein temporäres Verzeichnis, und binden Sie das Dateisystem ein:

   ```
   mkdir /temp
   mount /dev/sda2 /temp
   ```

8. Wenn der Datenträger nicht eingebunden werden kann, führen Sie den Befehl „xfs_repair“ mit der Option „-L“ (Schreiben von Nullen in das Protokoll erzwingen) aus:

   ```
   xfs_repair /dev/sda2 -L
   ```

9. Versuchen Sie als Nächstes, das Dateisystem einzubinden. Wenn der Datenträger erfolgreich eingebunden wurde, wird Folgendes ausgegeben:
 
   ```
   XFS (sda2): Mounting V1 Filesystem
   XFS (sda2): Ending clean mount
   ```

10. Starten Sie die VM neu, und überprüfen Sie, ob das Problem behoben wurde.

    ```
    Reboot -f
    ```

## <a name="repair-the-vm-offline"></a>Reparieren des virtuellen Computers im Offlinestatus

1. Fügen Sie den Systemdatenträger des virtuellen Computers als Datenträger für Daten an eine Wiederherstellungs-VM (beliebige funktionierende Linux-VM) an. Hierzu können Sie [CLI-Befehle](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux) verwenden, oder Sie können die Einrichtung der Wiederherstellungs-VM mithilfe der [VM-Reparaturbefehle](repair-linux-vm-using-azure-virtual-machine-repair-commands.md) automatisieren.

2. Suchen Sie die Laufwerkbezeichnung des angefügten Systemdatenträgers. In diesem Fall wird davon ausgegangen, dass die Bezeichnung des angefügten Systemdatenträgers „/dev/sdc1“ lautet. Ersetzen Sie dies durch den entsprechenden Wert für Ihre VM.

3. Verwenden Sie xfs_repair mit der Option „-n“, um die Fehler im Dateisystem zu erkennen.

   ```
   xfs_repair -n /dev/sdc1
   ```

4. Führen Sie den folgenden Befehl aus, um das Dateisystem zu reparieren:

   ```
   xfs_repair /dev/sdc1
   ```

5. Wenn die Fehlermeldung „FEHLER: Das Dateisystem weist wichtige Metadatenänderungen in einem Protokoll auf, das wiedergegeben werden muss“ ausgegeben wird, erstellen Sie ein temporäres Verzeichnis, und binden Sie das Dateisystem ein:

   ```
   mkdir /temp

   mount /dev/sdc1 /temp
   ```

   Wenn der Datenträger nicht eingebunden werden kann, führen Sie den Befehl „xfs_repair“ mit der Option „-L“ (Schreiben von Nullen in das Protokoll erzwingen) aus:

   ```
   xfs_repair /dev/sdc1 -L
   ```

6. Versuchen Sie als Nächstes, das Dateisystem einzubinden. Wenn der Datenträger erfolgreich eingebunden wurde, wird Folgendes ausgegeben:

   ```
   XFS (sdc1): Mounting V1 Filesystem
   
   XFS (sdc1): Ending clean mount
   ```

7. Heben Sie die Einbindung der ursprünglichen virtuellen Festplatte auf, und trennen Sie sie. Erstellen Sie dann eine VM mit dem ursprünglichen Systemdatenträger. Hierzu können Sie [CLI-Befehle](troubleshoot-recovery-disks-linux.md) verwenden oder die [VM-Reparaturbefehle](repair-linux-vm-using-azure-virtual-machine-repair-commands.md), wenn Sie diese zum Erstellen der Wiederherstellungs-VM verwendet haben.

8. Überprüfen Sie, ob das Problem behoben wurde.

## <a name="next-steps"></a>Nächste Schritte

* [Beheben von Problemen einer Linux-VM durch Anfügen des Betriebssystemdatenträgers an eine Wiederherstellungs-VM mithilfe der Azure CLI 2.0](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-troubleshoot-recovery-disks)
* [Anfügen eines Datenträgers an einen virtuellen Linux-Computer mithilfe des Portals](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal)

