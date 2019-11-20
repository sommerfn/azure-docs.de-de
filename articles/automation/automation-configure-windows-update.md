---
title: Konfigurieren von Windows Update-Einstellungen für die Zusammenarbeit mit Azure-Updateverwaltung
description: In diesem Artikel werden die Windows Update-Einstellungen beschrieben, die Sie für die Zusammenarbeit mit der Azure-Updateverwaltung konfigurieren.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 813d34f9c07e6c2909c483f040d4f3bf09b3ad24
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690835"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Konfigurieren von Windows Update-Einstellungen für Updateverwaltung

Die Azure-Updateverwaltung verwendet Windows Update, um Windows-Updates herunterzuladen und zu installieren. Aus diesem Grund berücksichtigt die Updateverwaltung viele der Einstellungen von Windows Update. Wenn Sie Einstellungen für die Aktivierung Windows-fremder Updates verwenden, verwaltet die Updateverwaltung diese Updates ebenfalls. Wenn Sie das Herunterladen von Updates vor der Updatebereitstellung ermöglichen, können Bereitstellungen schneller, effizienter und in der Regel innerhalb des Wartungsfensters durchgeführt werden.

## <a name="pre-download-updates"></a>Vorzeitiges Herunterladen von Updates

Um das automatische Herunterladen von Updates per Gruppenrichtlinie zu konfigurieren, können Sie die Einstellung [Automatische Updates konfigurieren](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) auf **3** festlegen. Diese Einstellung ermöglicht das Herunterladen der erforderlichen Updates im Hintergrund, ohne sie jedoch zu installieren. Dadurch behält die Updateverwaltung die Kontrolle über die Zeitpläne, und die Updates können außerhalb des Wartungsfensters der Updateverwaltung heruntergeladen werden. Dieses Verhalten verhindert Fehler im Zusammenhang mit der Überschreitung des Wartungsfensters in der Updateverwaltung.

Sie können diese Einstellung auch aktivieren, indem Sie auf einem System, das Sie für das automatische Herunterladen von Updates konfigurieren möchten, den folgenden PowerShell-Befehl ausführen:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="disable-automatic-installation"></a>Deaktivieren der automatischen Installation

Auf virtuellen Azure-Computern (virtual machines, VMs) ist die automatische Installation von Updates standardmäßig aktiviert. Dies kann dazu führen, dass Updates installiert werden, bevor Sie deren Installation über die Updateverwaltung planen. Dieses Verhalten können Sie deaktivieren, indem Sie den `NoAutoUpdate`-Registrierungsschlüssel auf `1` festlegen. Der folgende PowerShell-Codeausschnitt zeigt, wie das geht:

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

## <a name="configure-reboot-settings"></a>Konfigurieren von Neustarteinstellungen

Die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können den Neustart Ihrer Computer bewirken, auch wenn Sie in den Einstellungen unter **Updatebereitstellung** die Option **Nie neu starten** angegeben haben. Die Konfiguration dieser Registrierungsschlüssel sollte bestmöglich auf Ihre Umgebung abgestimmt werden.

## <a name="enable-updates-for-other-microsoft-products"></a>Aktivieren von Updates für andere Microsoft-Produkte

Standardmäßig stellt Windows Update nur Updates für Windows bereit. Wenn Sie die Einstellung **Updates für andere Microsoft-Produkte bereitstellen, wenn ein Windows-Update ausgeführt wird** aktivieren, erhalten Sie auch Updates für andere Produkte – einschließlich Sicherheitspatches für Microsoft SQL Server und andere Software von Microsoft. Diese Option kann nicht über Gruppenrichtlinien konfiguriert werden. Führen Sie auf den Systemen, auf denen Sie andere Microsoft-Updates aktivieren möchten, den folgenden PowerShell-Befehl aus. Die Updateverwaltung berücksichtigt diese Einstellung.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS-Konfigurationseinstellungen

Die Updateverwaltung berücksichtigt WSUS-Einstellungen (Windows Server Update Services). Im Anschluss finden Sie eine Liste mit den WSUS-Einstellungen, die Sie für die Updateverwaltung konfigurieren können.

### <a name="intranet-microsoft-update-service-location"></a>Interner Pfad für den Microsoft Updatedienst

Unter [Internen Pfad für den Microsoft Updatedienst angeben](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location) können Sie Quellen für die Suche nach und das Herunterladen von Updates angeben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Windows Update-Einstellungen konfiguriert haben, können Sie eine Updatebereitstellung planen. Eine entsprechende Anleitung finden Sie unter [Verwalten von Updates und Patches für Ihre virtuellen Azure-Computer](automation-tutorial-update-management.md).