---
title: VM-Start bleibt in Azure bei „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen. | Microsoft-Dokumentation
description: 'Hier werden die Schritte zur Behebung des Problems erläutert, bei dem der virtuelle Computer beim Starten hängen bleibt und die folgende Meldung angezeigt wird: „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen.'
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: 4263afe33caa4d6471848c8e7dbf9bc1eeec4bee
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332519"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>VM-Start bleibt in Azure bei „Windows wird vorbereitet. Computer nicht ausschalten.“ hängen.

In diesem Artikel werden die Bildschirme „Wird vorbereitet...“ und „Windows wird vorbereitet“ beschrieben, die ggf. beim Start eines virtuellen Windows-Computers (virtual machine, VM) in Microsoft Azure angezeigt werden. Er bietet Schritte, mit denen Sie Daten für ein Supportticket sammeln können.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="symptoms"></a>Symptome

Ein virtueller Windows-Computer startet nicht. Wenn Sie die **Startdiagnose** verwenden, um den Screenshot des virtuellen Computers zu erhalten, sehen Sie unter Umständen, dass der virtuelle Computer die Meldung „Wird vorbereitet...“ oder „Windows wird vorbereitet“ anzeigt.

![Beispielmeldung für Windows Server 2012 R2](./media/troubleshoot-vm-configure-update-boot/message1.png)

![Meldungsbeispiel](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Ursache

Dieses Problem tritt in der Regel auf, wenn der Server nach einer Änderung der Konfiguration den letzten Neustart ausführt. Die Konfigurationsänderung wurde möglicherweise durch Windows-Updates oder durch Änderungen an den Rollen/Features des Servers initiiert. Bei großen Windows-Updates benötigt das Betriebssystem mehr Zeit, um die Änderungen neu zu konfigurieren.

## <a name="collect-an-os-memory-dump"></a>Erfassen eines Betriebssystemspeicherabbilds

Wenn Sie auf die Verarbeitung der Änderungen gewartet haben und das Problem trotzdem bestehen bleibt, müssen Sie sich mit einer Speicherabbilddatei an den Support wenden. Um die Sicherungsdatei zu sammeln, gehen Sie folgendermaßen vor:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Anfügen des Betriebssystemdatenträgers an eine VM für die Wiederherstellung

1. Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des betroffenen virtuellen Computers als Sicherung. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Datenträgers](../windows/snapshot-copy-managed-disk.md).
2. [Fügen Sie den Betriebssystemdatenträger an einen virtuellen Computer für die Wiederherstellung an](../windows/troubleshoot-recovery-disks-portal.md).
3. Remotedesktopverbindung mit der Wiederherstellungs-VM. 
4. Ist der Betriebssystemdatenträger verschlüsselt, müssen Sie die Verschlüsselung deaktivieren, bevor Sie mit dem nächsten Schritt fortfahren. Weitere Informationen zum Entschlüsseln des verschlüsselten Betriebssystemdatenträgers des virtuellen Computers, der nicht gestartet werden kann, finden Sie [hier](troubleshoot-bitlocker-boot-error.md#solution).

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Suche nach der Sicherungsdatei und Senden eines Supporttickets

1. Navigieren Sie auf der Wiederherstellungs-VM zum Windows-Ordner im angefügten Betriebssystemdatenträger. Wenn der Laufwerkbuchstabe, der dem angefügten Betriebssystemdatenträger zugewiesen ist, „F“ lautet, müssen Sie zu „F:\Windows“ navigieren.
2. Suchen Sie nach der Datei „memory.dmp“, und [senden Sie dann ein Supportticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) mit der Sicherungsdatei. 

Wenn Sie die Sicherungsdatei nicht finden können, fahren Sie mit dem nächsten Schritt zum Aktivieren des Sicherungsprotokolls und der seriellen Konsole fort.

### <a name="enable-dump-log-and-serial-console"></a>Aktivieren des Sicherungsprotokolls und der seriellen Konsole

Um das Sicherungsprotokoll und die serielle Konsole zu aktivieren, führen Sie das folgende Skript aus.

1. Öffnen Sie die Eingabeaufforderungssitzung mit erhöhten Rechten (Als Administrator ausführen).
2. Führen Sie das folgende Skript aus:

    In diesem Skript wird davon ausgegangen, dass der dem angefügten Betriebssystemdatenträger zugewiesene Laufwerkbuchstabe „F“ ist. Ersetzen Sie ihn durch den entsprechenden Wert Ihrer VM.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

    1. Stellen Sie sicher, dass genügend Speicherplatz auf dem Datenträger vorhanden ist, um genauso viel Arbeitsspeicher wie der RAM zuzuweisen. Dieser hängt von der Größe ab, die Sie für diese VM auswählen.
    2. Wenn nicht genügend Speicherplatz vorhanden ist, oder es sich um eine VM beträchtlicher Größe (der Serie G, GS oder E) handelt, können Sie den Speicherort, in dem diese Datei erstellt wird, ändern und auf einen anderen Datenträger verweisen, der der VM angefügt ist. Hierzu müssen Sie den folgenden Schlüssel ändern:

            reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

            REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
            REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

            reg unload HKLM\BROKENSYSTEM

3. [Trennen Sie den Betriebssystemdatenträger, und fügen Sie ihn wieder an die betroffene VM an](../windows/troubleshoot-recovery-disks-portal.md).
4. Starten Sie den virtuellen Computer, und greifen Sie auf die serielle Konsole zu.
5. Wählen Sie **Nicht maskierbaren Interrupt (NMI) senden** aus, um die Generierung des Speicherabbilds auszulösen.
    ![Abbildung, die zeigt, wo der nicht maskierbare Interrupt gesendet wird](./media/troubleshoot-vm-configure-update-boot/run-nmi.png)
6. Fügen Sie den Betriebssystemdatenträger erneut an einen virtuellen Wiederherstellungscomputer an, und rufen Sie die Abbilddatei ab.

## <a name="contact-microsoft-support"></a>Microsoft-Support kontaktieren

Nachdem Sie die Abbilddatei erfasst haben, können Sie sich an den [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) wenden, um die Grundursache zu analysieren.