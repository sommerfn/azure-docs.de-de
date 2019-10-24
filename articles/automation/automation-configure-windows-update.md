---
title: Konfigurieren von Windows Update-Einstellungen für die Zusammenarbeit mit Azure-Updateverwaltung
description: In diesem Artikel werden die Windows Update-Einstellungen beschrieben, die Sie für die Zusammenarbeit mit Updateverwaltung konfigurieren.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f50ca9515f12e8c9b5943904c4d0226f2ca3353c
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377391"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Konfigurieren von Windows Update-Einstellungen für Updateverwaltung

Die Updateverwaltung verlässt sich zum Herunterladen und Installieren von Windows-Updates auf Windows Update. Daher werden viele der Einstellungen von Windows Update berücksichtigt. Wenn Sie Einstellungen verwenden, um Nicht-Windows-Updates zu aktivieren, verwaltet die Updateverwaltung diese Updates ebenfalls. Wenn Sie das Herunterladen der Updates vor der Updatebereitstellung aktivieren, können Bereitstellungen schneller ausgeführt werden und damit eher das Wartungsfenster einhalten.

## <a name="pre-download-updates"></a>Vorabdownload von Updates

Um das automatische Herunterladen von Updates in einer Gruppenrichtlinie zu konfigurieren, können Sie die Einstellung [Automatische Updates konfigurieren](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) auf **3** festlegen. Damit werden die erforderlichen Updates im Hintergrund heruntergeladen, aber nicht installiert. Auf diese Weise behält die Updateverwaltung die Kontrolle über die Zeitpläne, während die Updates gleichzeitig außerhalb des Wartungsfensters für die Updateverwaltung heruntergeladen werden können. Dies kann Fehler durch ein **Überschreiten des Wartungsfensters** bei der Updateverwaltung verhindern.

Sie können dies auch mit PowerShell festlegen. Führen Sie dazu den folgenden PowerShell-Befehl auf einem System aus, auf dem Sie Updates automatisch herunterladen möchten.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="disable-automatic-installation"></a>Deaktivieren der automatischen Installation

Bei Azure-VMs ist die automatische Installation von Updates standardmäßig aktiviert. Dies kann dazu führen, dass Updates installiert werden, bevor Sie deren Installation über die Updateverwaltung planen. Dieses Verhalten können Sie deaktivieren, indem Sie den `NoAutoUpdate`-Registrierungsschlüssel auf `1` festlegen. Der folgende PowerShell-Ausschnitt zeigt eine Möglichkeit, dies zu tun.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

## <a name="configure-reboot-settings"></a>Konfigurieren von Neustarteinstellungen

Die unter [Konfigurieren automatischer Updates durch Bearbeiten der Registrierung](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-rej7uijui7jgistry) und [Zum Verwalten des Neustarts verwendete Registrierungsschlüssel](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) aufgeführten Registrierungsschlüssel können den Neustart Ihrer Computer bewirken, auch Sie in den Einstellungen unter „Updatebereitstellung“ **Nie neu starten** angegeben haben. Sie müssen diese Registrierungsschlüssel für Ihre Umgebung wie gewünscht konfigurieren.

## <a name="enable-updates-for-other-microsoft-products"></a>Aktivieren von Updates für andere Microsoft-Produkte

Standardmäßig stellt Windows Update nur Updates für Windows bereit. Wenn Sie **Updates für andere Microsoft-Produkte bereitstellen, wenn ein Windows-Update ausgeführt wird** aktivieren, erhalten Sie auch Updates für andere Produkte – einschließlich Sicherheitspatches für SQL Server und andere Software von Erstanbietern. Diese Option kann nicht über Gruppenrichtlinien konfiguriert werden. Führen Sie den folgenden PowerShell-Befehl auf den Systemen aus, auf denen Sie Patches anderer Erstanbieter aktivieren möchten. Die Updateverwaltung berücksichtigt diese Einstellung.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS-Konfigurationseinstellungen

**Updateverwaltung**  beachtet die WSUS-Konfigurationseinstellungen. Die Liste der WSUS-Einstellungen, die Sie für die Zusammenarbeit mit Updateverwaltung konfigurieren können, finden Sie nachstehend.

### <a name="intranet-microsoft-update-service-location"></a>Interner Pfad für den Microsoft Updatedienst

Sie können unter [Interner Pfad für den Microsoft Updatedienst](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location) Quellen für das Durchsuchen auf und Herunterladen von Updates angeben.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Windows Update-Einstellungen konfiguriert haben, können Sie eine Updatebereitstellung planen, indem Sie die Schritte unter [Verwalten von Updates und Patches für Ihre Azure-VMs](automation-tutorial-update-management.md) ausführen.