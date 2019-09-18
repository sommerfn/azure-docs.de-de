---
title: Serielle Azure-Konsole für den GRUB- und den Einzelbenutzermodus | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die serielle Konsole für GRUB auf virtuellen Azure-Computern verwenden.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/06/2019
ms.author: alsin
ms.openlocfilehash: 06cb3fe5d551ddfc95fcbd37cd9620adebd825c5
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883933"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>Verwenden der seriellen Konsole für den Zugriff auf den GRUB- und den Einzelbenutzermodus
Der GRand Unified Bootloader (GRUB) ist wahrscheinlich das Erste, was Ihnen beim Starten eines virtuellen Computers angezeigt wird. Weil GRUB vor dem Start des Betriebssystems angezeigt wird, ist ein Zugriff über SSH nicht möglich. In GRUB können Sie unter anderem Ihre Startkonfiguration so ändern, dass das System im Einzelbenutzermodus gestartet wird.

Der Einzelbenutzermodus ist eine minimale Umgebung mit minimaler Funktionalität. Dieser Modus kann beim Untersuchen von Start-, Dateisystem- oder Netzwerkproblemen hilfreich sein. Es können weniger Dienste im Hintergrund ausgeführt werden, und je nach Ausführungsebene kann auch ein Dateisystem nicht automatisch eingebunden werden.

Der Einzelbenutzermodus ist auch in Situationen hilfreich, in denen Ihr virtueller Computer so konfiguriert ist, dass für die Anmeldung nur SSH-Schlüssel akzeptiert werden. In diesem Fall können Sie möglicherweise den Einzelbenutzermodus verwenden, um ein Konto mit Kennwortauthentifizierung zu erstellen. 

> [!NOTE]
> Der Dienst für die serielle Konsole gestattet den Zugriff auf die serielle Konsole einer VM nur Benutzern, die mindestens über die Zugriffsebene *Mitwirkender* verfügen.

Damit Sie in den Einzelbenutzermodus wechseln können, öffnen Sie GRUB beim Starten Ihres virtuellen Computers, und ändern Sie dann die Startkonfiguration in GRUB. Ausführliche Anweisungen zum Öffnen von GRUB finden Sie im nächsten Abschnitt. Wenn Ihre VM für die Anzeige von GRUB konfiguriert wurde, können Sie im Allgemeinen die Schaltfläche „Neu starten“ in der seriellen Konsole der VM verwenden, um die VM neu zu starten und GRUB anzuzeigen.

![Die Schaltfläche „Neu starten“ in der seriellen Konsole unter Linux](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-bar.png)

## <a name="general-grub-access"></a>Allgemeiner GRUB-Zugriff
Um auf GRUB zuzugreifen, starten Sie den virtuellen Computer neu, während der Bereich der seriellen Konsole geöffnet ist. Für einige Distributionen ist zum Anzeigen von GRUB eine Tastatureingabe erforderlich, während andere Distributionen GRUB automatisch einige Sekunden lang anzeigen, damit das Zeitlimit durch Benutzereingaben über die Tastatur abgebrochen werden kann.

Damit Sie auf den Einzelbenutzermodus zugreifen können, müssen Sie sicherstellen, dass GRUB auf Ihrer VM aktiviert ist. Je nach Distribution müssen Sie möglicherweise einige Einrichtungsschritte durchführen, um sicherzustellen, dass GRUB aktiviert ist. Distributionsspezifische Informationen finden Sie im nächsten Abschnitt und auf unserer Seite zum [Support für Linux in Azure](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).

### <a name="restart-your-vm-to-access-grub-in-serial-console"></a>Neustarten der VM zum Zugreifen auf GRUB in der seriellen Konsole
Sie können Ihre VM in der seriellen Konsole neu starten, indem Sie auf die Schaltfläche **Neu starten** zeigen und dann **VM neu starten** auswählen. Am unteren Rand des Bereichs wird eine Benachrichtigung über den Neustart angezeigt.

Sie können den virtuellen Computer auch neu starten, indem Sie den SysRq-Befehl „b“ ausführen, wenn [SysRq](./serial-console-nmi-sysrq.md) aktiviert ist. Um zu erfahren, was von GRUB bei einem Neustart zu erwarten ist, lesen Sie die distributionsspezifischen Anweisungen in den nächsten Abschnitten.

![Neustart in der seriellen Konsole unter Linux](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-ubuntu.gif)

## <a name="general-single-user-mode-access"></a>Allgemeiner Zugriff auf den Einzelbenutzermodus
Der manuelle Zugriff auf den Einzelbenutzermodus kann erforderlich sein, wenn Sie kein Konto mit Kennwortauthentifizierung konfiguriert haben. Ändern Sie die GRUB-Konfiguration, um manuell in den Einzelbenutzermodus zu wechseln. Lesen Sie anschließend den Abschnitt „Verwenden des Einzelbenutzermodus zum Zurücksetzen oder Hinzufügen eines Kennworts“, um weitere Informationen zu erhalten.

Wenn der virtuelle Computer nicht gestartet werden kann, wird bei Distributionen häufig automatisch der Einzelbenutzer- oder Notfallmodus aktiviert. Andere Distributionen erfordern jedoch eine zusätzliche Einrichtung, beispielsweise das Einrichten eines Stammkennworts, damit der Einzelbenutzer- oder Notfallmodus automatisch aktiviert werden kann.

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Verwenden des Einzelbenutzermodus zum Zurücksetzen oder Hinzufügen eines Kennworts
Wenn Sie sich im Einzelbenutzermodus befinden, gehen Sie wie folgt vor, um einen neuen Benutzer mit sudo-Berechtigungen hinzuzufügen:
1. Führen Sie `useradd <username>` aus, um einen Benutzer hinzuzufügen.
1. Führen Sie `sudo usermod -a -G sudo <username>` aus, um dem neuen Benutzer Stammberechtigungen zu erteilen.
1. Verwenden Sie `passwd <username>`, um das Kennwort für den neuen Benutzer festzulegen. Anschließend können Sie sich als neuer Benutzer anmelden.


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Zugriff für Red Hat Enterprise Linux (RHEL)
Wenn RHEL nicht normal gestartet werden kann, wird automatisch der Einzelbenutzermodus aktiviert. Wenn Sie jedoch keinen Stammzugriff für den Einzelbenutzermodus eingerichtet haben, besitzen Sie kein Stammkennwort und können sich nicht anmelden. Auch wenn es eine Problemumgehung gibt (Abschnitt „Manuelles Aktivieren des Einzelbenutzermodus in RHEL“), empfiehlt es sich, den Stammzugriff gleich anfangs einzurichten.

### <a name="grub-access-in-rhel"></a>GRUB-Zugriff in RHEL
In RHEL ist GRUB standardmäßig aktiviert. Zum Aktivieren von GRUB starten Sie Ihre VM durch Ausführen von `sudo reboot` neu, und drücken Sie eine beliebige Taste. Der GRUB-Bereich sollte angezeigt werden. Stellen Sie andernfalls sicher, dass die folgenden Zeilen in Ihrer GRUB-Datei (`/etc/default/grub`) vorhanden sind:

**Für RHEL 8**

```
GRUB_TIMEOUT=5
GRUB_TERMINAL="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"
```

**Für RHEL 7**

```
GRUB_TIMEOUT=5
GRUB_TERMINAL_OUTPUT="serial console"
GRUB_CMDLINE_LINUX="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300 net.ifnames=0"
```

> [!NOTE]
> Red Hat bietet auch eine Dokumentation zum Start im Wiederherstellungsmodus, Notfallmodus oder Debugmodus sowie zum Zurücksetzen des Stammkennworts. Anweisungen hierzu finden Sie unter [Terminal menu editing during boot](https://aka.ms/rhel7grubterminal) (Menübearbeitung über das Terminal während des Startvorgangs).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>Einrichten des Stammzugriffs für den Einzelbenutzermodus in RHEL
Der Stammbenutzer ist standardmäßig deaktiviert. Für den Einzelbenutzermodus in RHEL muss der Stammbenutzer aktiviert werden. Zum Aktivieren des Einzelbenutzermodus gehen Sie wie folgt vor:

1. Melden Sie sich über SSH beim Red Hat-System an.
1. Wechseln Sie zum Stamm.
1. Aktivieren Sie das Kennwort für den Stammbenutzer, indem Sie die folgenden Schritte ausführen:
    * Führen Sie `passwd root` aus. (Legen Sie ein sicheres Stammkennwort fest.)
1. Stellen Sie sicher, dass der Stammbenutzer sich nur über ttyS0 anmelden kann. Gehen Sie hierzu wie folgt vor:  
    a. Führen Sie `edit /etc/ssh/sshd_config` aus, und stellen Sie sicher, dass „PermitRootLogIn“ auf `no` festgelegt ist.  
    b. Führen Sie `edit /etc/securetty file` aus, um die Anmeldung nur über ttyS0 zuzulassen.

Wenn das System jetzt im Einzelbenutzermodus gestartet wird, können Sie sich mit dem Stammkennwort anmelden.

Alternativ können Sie für RHEL 7.4+ oder 6.9+ den Einzelbenutzermodus in den GRUB-Eingabeaufforderungen aktivieren. Anweisungen dazu finden Sie unter [Booten in den Einzelbenutzermodus](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single).

### <a name="manually-enter-single-user-mode-in-rhel"></a>Manuelles Aktivieren des Einzelbenutzermodus in RHEL
Wenn Sie GRUB und Stammzugriff mit den vorangehenden Anweisungen eingerichtet haben, können Sie den Einzelbenutzermodus wie folgt aktivieren:

1. Drücken Sie beim Neustart der VM die ESC-TASTE, um GRUB zu öffnen.
1. Drücken Sie in GRUB die Taste E, um das Betriebssystem zu bearbeiten, das Sie starten möchten. Das Betriebssystem wird in der Regel in der ersten Zeile aufgeführt.
1. Suchen Sie die Zeile für den Kernel. In Azure beginnt diese mit *linux16*.
1. Drücken Sie STRG+E, um zum Ende der Zeile zu gelangen.
1. Fügen Sie am Ende der Zeile *systemd.unit=rescue.target* hinzu.
    
    Durch diese Aktion erfolgt der Start im Einzelbenutzermodus. Wenn Sie den Notfallmodus verwenden möchten, fügen Sie am Ende der Zeile *systemd.unit=emergency.target* ein (anstelle von *systemd.unit=rescue.target*).

1. Drücken Sie STRG+X, um GRUB zu beenden und einen Neustart mit den angewendeten Einstellungen durchzuführen.

   Sie werden zur Eingabe des Administratorkennworts aufgefordert, bevor Sie in den Einzelbenutzermodus wechseln können. Dieses Kennwort ist das Kennwort, das Sie in den vorherigen Anweisungen erstellt haben.

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>Aktivieren des Einzelbenutzermodus ohne aktiviertes Stammkonto in RHEL
Wenn Sie den Stammbenutzer nicht anhand der vorherigen Anweisungen aktiviert haben, können Sie das Stammkennwort trotzdem zurücksetzen, indem Sie die folgenden Schritte ausführen:

> [!NOTE]
> Wenn Sie SELinux verwenden, müssen Sie beim Zurücksetzen des Stammkennworts die zusätzlichen Schritte ausführen, die in der [Red Hat-Dokumentation](https://aka.ms/rhel7grubterminal)beschrieben werden.

1. Drücken Sie beim Neustart der VM die ESC-TASTE, um GRUB zu öffnen.

1. Drücken Sie in GRUB die Taste E, um das Betriebssystem zu bearbeiten, das Sie starten möchten. Das Betriebssystem wird in der Regel in der ersten Zeile aufgeführt.
1. Suchen Sie die Zeile für den Kernel. In Azure beginnt diese mit *linux16*.
1. Fügen Sie am Ende der Zeile *rd.break* hinzu. Fügen Sie zwischen der Kernelzeile und *rd.break* ein Leerzeichen ein.

    Durch diese Aktion wird der Startprozess unterbrochen, bevor die Steuerung von `initramfs` an `systemd` übergeben wird. (Eine Beschreibung hierzu finden Sie in der [Red Hat-Dokumentation](https://aka.ms/rhel7rootpassword).)
1. Drücken Sie STRG+X, um GRUB zu beenden und einen Neustart mit den angewendeten Einstellungen durchzuführen.

   Nach dem Neustart wird der Notfallmodus mit einem schreibgeschützten Dateisystem aktiviert. 
   
1. Geben Sie in der Shell `mount -o remount,rw /sysroot` ein, um das Stammdateisystem mit Lese-/Schreibberechtigungen erneut einzubinden.
1. Geben Sie nach dem Start im Einzelbenutzermodus `chroot /sysroot` ein, um in das `sysroot`-Jail zu wechseln.
1. Sie befinden sich jetzt im Stammverzeichnis. Sie können Ihr Stammkennwort durch Eingabe von `passwd` zurücksetzen und anschließend mit den obigen Anweisungen in den Einzelbenutzermodus wechseln. 
1. Geben Sie zum Schluss `reboot -f` ein, um einen Neustart durchzuführen.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> [!NOTE]
> Wenn Sie den obigen Anweisungen folgen, gelangen Sie in die Notfallshell, sodass Sie auch Aufgaben wie das Bearbeiten von `fstab` ausführen können. Es wird jedoch normalerweise empfohlen, das Stammkennwort zurückzusetzen und für den Wechsel in den Einzelbenutzermodus zu verwenden.

## <a name="access-for-centos"></a>Zugriff für CentOS
Ähnlich wie in Red Hat Enterprise Linux erfordert der Einzelbenutzermodus in CentOS, dass GRUB und der Stammbenutzer aktiviert sind.

### <a name="grub-access-in-centos"></a>GRUB-Zugriff in CentOS
In CentOS ist GRUB standardmäßig aktiviert. Zum Aktivieren von GRUB starten Sie Ihre VM durch Eingabe von `sudo reboot` neu, und drücken Sie eine beliebige Taste. Durch diese Aktion wird der GRUB-Bereich angezeigt.

### <a name="single-user-mode-in-centos"></a>Einzelbenutzermodus in CentOS
Befolgen Sie die obigen Anweisungen für RHEL, um den Einzelbenutzermodus in CentOS zu aktivieren.

## <a name="access-for-ubuntu"></a>Zugriff für Ubuntu
Für Ubuntu-Images ist kein Stammkennwort erforderlich. Wenn das System im Einzelbenutzermodus gestartet wird, können Sie es ohne zusätzliche Anmeldeinformationen verwenden.

### <a name="grub-access-in-ubuntu"></a>GRUB-Zugriff in Ubuntu
Für den Zugriff auf GRUB halten Sie während des VM-Starts die ESC-TASTE gedrückt.

Standardmäßig wird der GRUB-Bereich für Ubuntu-Images nicht automatisch angezeigt. Sie können die Einstellung wie folgt ändern:
1. Öffnen Sie in einem Text-Editor die Datei */etc/default/grub.d/50-cloudimg-settings.cfg*.

1. Ändern Sie den Wert von `GRUB_TIMEOUT` in einen Wert ungleich null.
1. Öffnen Sie */etc/default/grub* in einem Text-Editor.
1. Kommentieren Sie die Zeile `GRUB_HIDDEN_TIMEOUT=1` aus.
1. Stellen Sie sicher, dass eine Zeile `GRUB_TIMEOUT_STYLE=menu` vorhanden ist.
1. Führen Sie `sudo update-grub`aus.

### <a name="single-user-mode-in-ubuntu"></a>Einzelbenutzermodus in Ubuntu
Wenn Ubuntu nicht normal gestartet werden kann, wird automatisch der Einzelbenutzermodus aktiviert. Gehen Sie folgendermaßen vor, um den Einzelbenutzermodus manuell zu aktivieren:

1. Drücken Sie in GRUB die Taste E, um Ihren Starteintrag (den Ubuntu-Eintrag) zu bearbeiten.
1. Suchen Sie nach der Zeile, die mit *linux* beginnt, und suchen Sie dann nach *ro*.
1. Fügen Sie *single* nach *ro* hinzu, und achten Sie darauf, dass vor und nach *single* ein Leerzeichen vorhanden ist.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und den Einzelbenutzermodus zu aktivieren.

### <a name="use-grub-to-invoke-bash-in-ubuntu"></a>Verwenden von GRUB zum Aufrufen von Bash in Ubuntu
In einigen Situationen (etwa bei einem vergessenen Stammkennwort) sind Sie möglicherweise auch nach dem Ausführen der vorstehenden Anweisungen nicht in der Lage, in Ihrer Ubuntu-VM auf den Einzelbenutzermodus zuzugreifen. Sie können den Kernel auch anweisen, anstelle der Systeminitialisierung `/bin/bash` zur Initialisierung auszuführen. Mit dieser Aktion erhalten Sie eine bash-Shell, die eine Systemwartung ermöglicht. Gehen Sie folgendermaßen vor:

1. Drücken Sie in GRUB die Taste E, um Ihren Starteintrag (den Ubuntu-Eintrag) zu bearbeiten.

1. Suchen Sie nach der Zeile, die mit *linux* beginnt, und suchen Sie dann nach *ro*.
1. Ersetzen Sie *ro* durch *rw init=/bin/bash*.

    Mit dieser Aktion wird das Dateisystem mit Lese-/Schreibzugriff eingebunden und `/bin/bash` als Initialisierungsprozess verwendet.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen.

## <a name="access-for-coreos"></a>Zugriff für CoreOS
Der Einzelbenutzermodus in CoreOS erfordert, dass GRUB aktiviert ist.

### <a name="grub-access-in-coreos"></a>GRUB-Zugriff in CoreOS
Für den Zugriff auf GRUB drücken Sie während des Starts der VM eine beliebige Taste.

### <a name="single-user-mode-in-coreos"></a>Einzelbenutzermodus in CoreOS
Wenn CoreOS nicht normal gestartet werden kann, wird automatisch der Einzelbenutzermodus aktiviert. Gehen Sie folgendermaßen vor, um den Einzelbenutzermodus manuell zu aktivieren:

1. Drücken Sie in GRUB die Taste E, um Ihren Starteintrag zu bearbeiten.

1. Suchen Sie nach der Zeile, die mit *linux$* beginnt. Es sollten zwei Instanzen der Zeile vorhanden sein, die jeweils in einer anderen *if...else*-Klausel eingebettet sind.
1. Fügen Sie *coreos.autologin=ttyS0* an das Ende jeder *linux$* -Zeile an.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und den Einzelbenutzermodus zu aktivieren.

## <a name="access-for-suse-sles"></a>Zugriff für SUSE SLES
Neuere Images von SLES 12 SP3 und höher ermöglichen den Zugriff über die serielle Konsole, wenn das System im Notfallmodus gestartet wird.

### <a name="grub-access-in-suse-sles"></a>GRUB-Zugriff in SUSE SLES
Für den GRUB-Zugriff in SLES ist eine Bootloaderkonfiguration über YaST erforderlich. Gehen Sie zur Erstellung der Konfiguration folgendermaßen vor:

1. Melden Sie sich mit SSH bei der SLES-VM an, und führen Sie dann `sudo yast bootloader` aus. Drücken Sie die TAB-TASTE, drücken Sie die EINGABETASTE, und verwenden Sie dann die Pfeiltasten, um durch das Menü zu navigieren.

1. Wechseln Sie zu **Kernel Parameters** (Kernelparameter), und aktivieren Sie dann das Kontrollkästchen **Use serial console** (Serielle Konsole verwenden).
1. Fügen Sie unter **Console Arguments** (Konsolenargumente) `serial --unit=0 --speed=9600 --parity=no` hinzu.
1. Drücken Sie F10, um Ihre Einstellungen zu speichern und das Tool zu beenden.
1. Starten Sie zum Aktivieren des GRUB-Modus Ihre VM neu, und drücken Sie während der Startsequenz eine beliebige Taste, damit der GRUB-Bereich weiterhin angezeigt wird.

    Das Standardzeitlimit für GRUB beträgt **1 Sekunde**. Sie können diese Einstellung ändern, indem Sie die Variable `GRUB_TIMEOUT` in der Datei */etc/default/grub* bearbeiten.

![Initialisieren der Bootloaderkonfiguration](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>Einzelbenutzermodus in SUSE SLES
Wenn SLES nicht normal gestartet werden kann, wird automatisch die Notfallshell geöffnet. Gehen Sie folgendermaßen vor, um die Notfallshell manuell zu aktivieren:

1. Drücken Sie in GRUB die Taste E, um Ihren Starteintrag (den SLES-Eintrag) zu bearbeiten.

1. Suchen Sie nach der Kernelzeile, die mit *linux* beginnt.
1. Fügen Sie *systemd.unit=emergency.target* am Ende der Kernelzeile an.
1. Drücken Sie STRG+X, um einen Neustart mit diesen Einstellungen durchzuführen und die Notfallshell zu öffnen.

   > [!NOTE]
   > Mit dieser Aktion gelangen Sie in die Notfallshell mit einem schreibgeschützten Dateisystem. Um Dateien zu bearbeiten, binden Sie das Dateisystem mit Lese-/Schreibberechtigungen erneut ein. Geben Sie hierzu `mount -o remount,rw /` in die Shell ein.

## <a name="access-for-oracle-linux"></a>Zugriff für Oracle Linux
Ähnlich wie in Red Hat Enterprise Linux erfordert der Einzelbenutzermodus in Oracle Linux, dass GRUB und der Stammbenutzer aktiviert sind.

### <a name="grub-access-in-oracle-linux"></a>GRUB-Zugriff in Oracle Linux
In Oracle Linux ist GRUB standardmäßig aktiviert. Zum Aktivieren von GRUB starten Sie Ihre VM durch Ausführen von `sudo reboot` neu, und drücken Sie die ESC-TASTE. Durch diese Aktion wird der GRUB-Bereich angezeigt. Wird der GRUB-Bereich nicht angezeigt, stellen Sie sicher, dass *serial console* in der Zeile `GRUB_TERMINAL` enthalten ist: `GRUB_TERMINAL="serial console"`. Erstellen Sie GRUB erneut mit `grub2-mkconfig -o /boot/grub/grub.cfg`.

### <a name="single-user-mode-in-oracle-linux"></a>Einzelbenutzermodus in Oracle Linux
Befolgen Sie die obigen Anweisungen für RHEL, um den Einzelbenutzermodus in Oracle Linux zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur seriellen Konsole:
* [Dokumentation zur seriellen Konsole für Linux](serial-console-linux.md)
* [Verwenden der seriellen Konsole zum Aktivieren von GRUB in verschiedenen Distributionen](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* [Verwenden der seriellen Konsole für NMI- und SysRq-Aufrufe](serial-console-nmi-sysrq.md)
* [Serielle Konsole für Windows-VMs](serial-console-windows.md)
* [Startdiagnose](boot-diagnostics.md)
