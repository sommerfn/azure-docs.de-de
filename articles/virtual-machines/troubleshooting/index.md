---
layout: LandingPage
description: Hier erfahren Sie, wie Sie Probleme bei VM-Bereitstellungen behandeln.
title: Dokumentation zur Problembehandlung für Azure Virtual Machines | Microsoft-Dokumentation
services: virtual-machines
author: genlin
manager: gwallace
ms.assetid: ''
ms.service: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing-page
ms.date: 10/3/2018
ms.author: genli
ms.openlocfilehash: 1db5829abd52a1b010d38dd8e9151ab2df3f1de5
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854403"
---
# <a name="troubleshooting-azure-virtual-machines"></a>Problembehandlung für virtuelle Azure-Computer

- Fehler bei der Zuordnung
    - [Fehler bei der Zuordnung](allocation-failure.md)
    - [Zuordnungsfehler bei klassischen Bereitstellungen](allocation-failure-classic.md)
- [Startdiagnose](boot-diagnostics.md)
- RDP
    - [Zurücksetzen von RDP](reset-rdp.md)
    - [Problembehandlung für RDP](troubleshoot-rdp-connection.md)
    - [Detaillierte Problembehandlung für RDP](detailed-troubleshoot-rdp.md)
    - [Behandeln spezifischer Fehler](troubleshoot-specific-rdp-errors.md)
- SSH 
    - [Problembehandlung für SSH](troubleshoot-ssh-connection.md)
    - [Detaillierte Problembehandlung für SSH](detailed-troubleshoot-ssh-connection.md)
    - [Häufige Fehlermeldungen](error-messages.md)
- [Offlineinstallation des Windows-VM-Agents](install-vm-agent-offline.md)
- Beheben von Leistungsproblemen im Zusammenhang mit virtuellen Computern
    - [Leistungsprobleme im Zusammenhang mit virtuellen Computern](performance-diagnostics.md)
    - Windows
        - [Verwenden von PerfInsights](how-to-use-perfinsights.md)
        - [Erweiterung für die Leistungsdiagnose](performance-diagnostics-vm-extension.md)
    - Linux
        - [Verwenden von PerfInsights](how-to-use-perfinsights-linux.md)
- Erneutes Bereitstellen eines virtuellen Computers
    - [Linux](redeploy-to-new-node-linux.md)
    - [Windows](redeploy-to-new-node-windows.md)
- Zurücksetzen des Kennworts des virtuellen Computers
    - [Windows](reset-local-password-without-agent.md)
    - [Linux](reset-password.md)
- [Zurücksetzen der NIC](reset-network-interface.md)
- [Neustart oder Größenänderung eines virtuellen Computers](restart-resize-error-troubleshooting.md)
- Verwenden der seriellen Konsole
    - [Linux-VM](serial-console-linux.md)
        - [Serielle Konsole: GRUB-/Einzelbenutzermodus](serial-console-grub-single-user-mode.md)
        - [Serielle Konsole: NMI/SysRq](serial-console-nmi-sysrq.md)
    - [Windows-VM](serial-console-windows.md)
        - [CMD und PowerShell-Befehle](serial-console-cmd-ps-commands.md)
- [Fehler beim Löschen von Speicherressourcen](storage-resource-deletion-errors.md      )
- [Unerwartete Neustarts von virtuellen Computern mit angefügten VHDs](unexpected-reboots-attached-vhds.md)
- [Probleme bei der Windows-Aktivierung](troubleshoot-activation-problems.md)
- [Probleme beim Anwendungszugriff](troubleshoot-app-connection.md)
- Behandeln von Bereitstellungsproblemen
    - [Linux](troubleshoot-deploy-vm-linux.md)
    - [Windows](troubleshoot-deploy-vm-windows.md)
- [Änderung von Gerätenamen](troubleshoot-device-names-problems.md)
- Zugriff auf die VM-Wiederherstellung
    - Windows
        - [PowerShell](troubleshoot-recovery-disks-windows.md)
        - [Azure-Portal](troubleshoot-recovery-disks-portal-windows.md)
    - Linux
        - [BEFEHLSZEILENSCHNITTSTELLE (CLI)](troubleshoot-recovery-disks-linux.md)
    - [Azure-Portal](troubleshoot-recovery-disks-portal-linux.md)
- [Startfehler](boot-error-troubleshoot.md)
- [BitLocker-Fehler](troubleshoot-bitlocker-boot-error.md)
- [Überprüfen von Dateisystemfehlern](troubleshoot-check-disk-boot-error.md)
- [Bluescreen-Fehler](troubleshoot-common-blue-screen-error.md)
- [Drosselungsfehler](troubleshooting-throttling-errors.md)
- [Verwenden der geschachtelten Virtualisierung](troubleshoot-vm-by-use-nested-virtualization.md)
- [Informationen zum Systemneustart](understand-vm-reboot.md)

