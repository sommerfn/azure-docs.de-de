---
title: Azure Statusmonitor v2 – Erste Schritte | Microsoft-Dokumentation
description: Schnellstartanleitung für Statusmonitor v2 Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: a0c836c8ef947e190a0090b3435eec1c53ded436
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326262"
---
# <a name="get-started-with-status-monitor-v2"></a>Erste Schritte mit Statusmonitor v2

Dieser Artikel enthält die Schnellstartbefehle, die für die meisten Umgebungen geeignet sein dürften.
Die Anweisungen richten sich nach dem für das Verteilen von Updates verwendeten PowerShell-Katalog.
Diese Befehle unterstützen den `-Proxy`-Parameter von PowerShell.

Eine Erläuterung der Befehle, Anpassungsanweisungen und Informationen zur Problembehandlung finden Sie unter [Ausführliche Anleitungen](status-monitor-v2-detailed-instructions.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="download-and-install-via-powershell-gallery"></a>Herunterladen und Installieren über PowerShell-Katalog

### <a name="install-prerequisites"></a>Installieren der erforderlichen Komponenten
Führen Sie PowerShell als Administrator aus.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
Schließen Sie PowerShell.

### <a name="install-status-monitor-v2"></a>Installieren von Statusmonitor v2
Führen Sie PowerShell als Administrator aus.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>Aktivieren der Überwachung
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>Manuelles Herunterladen und Installieren (Offline-Option)
### <a name="download-the-module"></a>Herunterladen des Moduls
Laden Sie die neueste Version des Moduls aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/Az.ApplicationMonitor) manuell herunter.

### <a name="unzip-and-install-status-monitor-v2"></a>Extrahieren und Installieren von Statusmonitor v2
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>Aktivieren der Überwachung
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```



## <a name="next-steps"></a>Nächste Schritte

 Anzeigen der Telemetrie:

- [Untersuchen Sie Metriken](../../azure-monitor/app/metrics-explorer.md) zum Überwachen der Leistung und Nutzung.
- [Durchsuchen Sie Ereignisse und Protokolle](../../azure-monitor/app/diagnostic-search.md), um Probleme zu diagnostizieren.
- Verwenden Sie [Analytics](../../azure-monitor/app/analytics.md) für erweiterte Abfragen.
- [Erstellen Sie Dashboards](../../azure-monitor/app/overview-dashboard.md).

 Hinzufügen weiterer Telemetrieelemente:

- [Erstellen Sie Webtests](monitor-web-app-availability.md), um sicherzustellen, dass Ihre Website live bleibt.
- [Fügen Sie Webclient-Telemetriedaten hinzu](../../azure-monitor/app/javascript.md), um Ausnahmen von Webseitencode anzuzeigen und Ablaufverfolgungsaufrufe zu aktivieren.
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.

Weitere Möglichkeiten zur Verwendung von Statusmonitor v2:

- Beachten Sie die [ausführlichen Anleitungen](status-monitor-v2-detailed-instructions.md). Dort finden Sie eine Erläuterung der hier verwendeten Befehle.
- Verwenden Sie unsere Anleitung für die [Problembehandlung](status-monitor-v2-troubleshoot.md) von Statusmonitor v2.
