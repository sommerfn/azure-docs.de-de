---
title: 'Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop: Azure'
description: Konfigurieren und Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: helohr
ms.openlocfilehash: e7f565a995e4c2a5338f08437b0dd336846ba154
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679547"
---
# <a name="deploy-a-windows-7-virtual-machine-on-windows-virtual-desktop"></a>Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop

Der Vorgang zum Bereitstellen eines virtuellen Windows 7-Computers (VM) in Windows Virtual Desktop unterscheidet sich geringfügig von der Bereitstellung auf virtuellen Computern, auf denen höhere Versionen von Windows ausgeführt werden. In dieser Anleitung erfahren Sie, wie Sie Windows 7 bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, befolgen Sie die Anleitungen unter [Erstellen eines Hostpools mit PowerShell](create-host-pools-powershell.md), um einen Hostpool zu erstellen. Befolgen Sie anschließend die Anleitungen in [Erstellen von Hostpools in Azure Marketplace](create-host-pools-azure-marketplace.md#optional-assign-additional-users-to-the-desktop-application-group), um der Desktopanwendungsgruppe mindestens einen Benutzer zuzuweisen.

## <a name="configure-a-windows-7-virtual-machine"></a>Konfigurieren eines virtuellen Windows 7-Computers

Nachdem Sie die Voraussetzungen erfüllt haben, können Sie Ihre Windows 7-VM für die Bereitstellung in Windows Virtual Desktop konfigurieren.

So richten Sie einen virtuellen Windows 7-Computer in Windows Virtual Desktop ein:

1. Melden Sie sich am Azure-Portal an, und suchen Sie entweder nach dem Windows 7 Enterprise-Image, oder laden Sie ein eigenes benutzerdefiniertes Windows 7 Enterprise-Image (x64) hoch.  
2. Stellen Sie mindestens einen virtuellen Computer mit Windows 7 Enterprise als Hostbetriebssystem bereit. Stellen Sie sicher, dass die virtuellen Computer das Remotedesktopprotokoll (RDP) zulassen (TCP-Port/Port 3389).
3. Stellen Sie mithilfe von RDP eine Verbindung mit dem Windows 7 Enterprise-Host her, und authentifizieren Sie sich mit den Anmeldeinformationen, die Sie beim Konfigurieren der Bereitstellung definiert haben. 
4. Fügen Sie das Konto, das Sie beim Herstellen einer Verbindung mit dem Host über RDP verwendet haben, der Gruppe „Remotedesktopbenutzer“ hinzu. Wenn Sie nicht so vorgehen, können Sie möglicherweise keine Verbindung mit dem virtuellen Computer herstellen, nachdem Sie ihn Ihrer Active Directory-Domäne hinzugefügt haben.
5. Navigieren Sie auf Ihrem virtuellen Computer zu Windows Update.
6. Installieren Sie alle Windows-Updates in der Kategorie „Wichtig“.
7. Installieren Sie alle Windows-Updates in der Kategorie „Optional“ (mit Ausnahme von Language Packs). Dadurch wird das Update für das Remotedesktopprotokoll 8.0 ([KB2592687](https://www.microsoft.com/download/details.aspx?id=35393)) installiert, das Sie benötigen, um diese Anweisungen abzuschließen.
8. Öffnen Sie den lokalen Gruppenrichtlinien-Editor, und navigieren Sie zu **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Remote Session Environment** (Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Remotedesktopdienste > Remotedesktop-Sitzungshost > Remotesitzungsumgebung).
9. Aktivieren Sie die Richtlinie für das Remotedesktopprotokoll 8.0.
10. Starten Sie den virtuellen Computer neu, indem Sie den folgenden Befehl ausführen:
    
     ```cmd
     shutdown /r /t 0
     ```
    
## <a name="next-steps"></a>Nächste Schritte

Ihre Windows 7-VM ist nun bereit für die Bereitstellung in Windows Virtual Desktop. Befolgen Sie die Anleitungen unter [Vorbereiten der virtuellen Computer für Windows Virtual Desktop-Agent-Installationen](create-host-pools-powershell.md#prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations), um die Bereitstellung abzuschließen.

Eine Liste bekannter Probleme und Anleitungen zur Problembehandlung für Windows 7 in Windows Virtual Desktop finden Sie im Artikel zur Problembehandlung unter [Problembehandlung für virtuelle Windows 7-Computer in Windows Virtual Desktop](troubleshoot-windows-7-vm.md).

