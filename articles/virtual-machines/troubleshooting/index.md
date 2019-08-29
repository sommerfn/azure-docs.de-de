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
ms.topic: landing-page
ms.date: 10/3/2018
ms.author: genli
ms.openlocfilehash: ed1873fa8e74b2af5734411dc80aeb9089b61a7e
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70080449"
---
# <a name="troubleshooting-azure-virtual-machines"></a>Problembehandlung für virtuelle Azure-Computer

## <a name="tools-for-troubleshooting"></a>Tools für die Problembehandlung

- [Serielle Konsole](serial-console-windows.md)
- [Startdiagnose](boot-diagnostics.md)
- [Windows-VM: Anfügen des Betriebssystemdatenträgers an eine andere Windows-VM zur Problembehandlung](troubleshoot-recovery-disks-portal-windows.md)
- [Linux-VM: Anfügen des Betriebssystemdatenträgers an eine andere VM zur Problembehandlung](troubleshoot-recovery-disks-portal-linux.md)

## <a name="cant-connect-to-the-vm"></a>Ich kann keine Verbindung mit dem virtuellen Computer herstellen.

### <a name="windows"></a>Windows

**Allgemeine Lösung**

- [Zurücksetzen von RDP](reset-rdp.md)
- [Problembehandlung für RDP](troubleshoot-rdp-connection.md)
- [Detaillierte Problembehandlung für RDP](detailed-troubleshoot-rdp.md)

**RDP-Fehler**

- [Kein Lizenzserver](troubleshoot-rdp-no-license-server.md)
- [Interner Fehler](Troubleshoot-rdp-internal-error.md)
- [Authentifizierungsfehler](troubleshoot-authentication-error-rdp-vm.md)
- [Behandeln spezifischer Fehler](troubleshoot-specific-rdp-errors.md)

**Fehler beim Starten des virtuellen Computers**

* [BitLocker-Startfehler](troubleshoot-bitlocker-boot-error.md) 
* [Windows zeigt beim Starten „Dateisystem wird überprüft“ an](troubleshoot-check-disk-boot-error.md)
* [Bluescreen-Fehler](troubleshoot-common-blue-screen-error.md)
* [VM-Start bleibt bei „Windows wird vorbereitet“ hängen](troubleshoot-vm-boot-configure-update.md)
* [KRITISCHER SERVERFEHLER auf Bluescreen](troubleshoot-critical-service-failed-boot-error.md)
* [Neustartschleifenproblem](troubleshoot-reboot-loop.md)
* [VM-Start bleibt bei Windows-Update hängen](troubleshoot-stuck-updating-boot-error.md)
* [Starten der VM im abgesicherten Modus](troubleshoot-rdp-safe-mode.md)

**Andere**
- [Zurücksetzen des VM-Kennworts für eine Windows-VM (offline)](reset-local-password-without-agent.md)
- [Zurücksetzen des Netzwerkadapters nach einer Fehlkonfiguration](reset-network-interface.md)

### <a name="linux"></a>Linux

- [Problembehandlung für SSH](troubleshoot-ssh-connection.md)
- [Detaillierte Problembehandlung für SSH](detailed-troubleshoot-ssh-connection.md)
- [Häufige Fehlermeldungen](error-messages.md)
- [Zurücksetzen des VM-Kennworts für eine Linux-VM (offline)](reset-password.md)

## <a name="vm-deployment-issues"></a>VM-Bereitstellungsprobleme

- [Fehler bei der Zuordnung](allocation-failure.md)
- Erneutes Bereitstellen eines virtuellen Computers
    - [Linux](redeploy-to-new-node-linux.md)
    - [Windows](redeploy-to-new-node-windows.md)
- Behandeln von Bereitstellungsproblemen
    - [Linux](troubleshoot-deploy-vm-linux.md)
    - [Windows](troubleshoot-deploy-vm-windows.md)
- [Änderung von Gerätenamen](troubleshoot-device-names-problems.md)
- [Offlineinstallation des Windows-VM-Agents](install-vm-agent-offline.md)
- [Neustart oder Größenänderung eines virtuellen Computers](restart-resize-error-troubleshooting.md)

## <a name="vm-performance-issue"></a>VM-Leistungsproblem
- [Leistungsprobleme im Zusammenhang mit virtuellen Computern](performance-diagnostics.md)
- Windows
    - [Verwenden von PerfInsights](how-to-use-perfinsights.md)
    - [Erweiterung für die Leistungsdiagnose](performance-diagnostics-vm-extension.md)
- Linux
    - [Verwenden von PerfInsights](how-to-use-perfinsights-linux.md)