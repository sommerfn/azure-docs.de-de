---
title: 'API-Referenz für den Azure Application Insights-Agent: Abrufen des Status | Microsoft-Dokumentation'
description: API-Referenz für den Application Insights-Agent. Get-ApplicationInsightsMonitoringStatus. Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
services: application-insights
documentationcenter: .net
author: TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 6e103e1856e338669224540a991c4b9ea6b10d6d
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389867"
---
# <a name="application-insights-agent-api-get-applicationinsightsmonitoringstatus"></a>Application Insights-Agent-API: Get-ApplicationInsightsMonitoringStatus

In diesem Artikel wird ein Cmdlet des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) beschrieben.

## <a name="description"></a>BESCHREIBUNG

Dieses Cmdlet bietet Problembehandlungsinformationen zu Statusmonitor.
Sie können mithilfe dieses Cmdlets den Überwachungsstatus, die Version des PowerShell-Moduls und den laufenden Prozess untersuchen.
Dieses Cmdlet meldet Versionsinformationen und Informationen über Schlüsseldateien, die für die Überwachung erforderlich sind.

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorberechtigungen.

## <a name="examples"></a>Beispiele

### <a name="example-application-status"></a>Beispiel: Anwendungsstatus

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus` aus, um den Status der Überwachung von Websites anzuzeigen.

```
PS C:\Windows\system32> Get-ApplicationInsightsMonitoringStatus
Machine Identifier:
811D43F7EC807E389FEA2E732381288ACCD70AFFF9F569559AC3A75F023FA639

IIS Websites:

SiteName               : Default Web Site
ApplicationPoolName    : DefaultAppPool
SiteId                 : 1
SiteState              : Stopped

SiteName               : DemoWebApp111
ApplicationPoolName    : DemoWebApp111
SiteId                 : 2
SiteState              : Started
ProcessId              : not found

SiteName               : DemoWebApp222
ApplicationPoolName    : DemoWebApp222
SiteId                 : 3
SiteState              : Started
ProcessId              : 2024
Instrumented           : true
InstrumentationKey     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123

SiteName               : DemoWebApp333
ApplicationPoolName    : DemoWebApp333
SiteId                 : 4
SiteState              : Started
ProcessId              : 5184
AppAlreadyInstrumented : true
```

In diesem Beispiel gilt Folgendes:
- **Machine Identifier** ist eine anonyme ID, mit der der Server eindeutig identifiziert wird. Wenn Sie eine Supportanfrage erstellen, benötigen wir diese ID, um die Protokolle für Ihren Server zu finden.
- **Default Web Site** wird in IIS beendet.
- **DemoWebApp111** wurde in IIS gestartet, hat aber noch keine Anforderungen empfangen. Dieser Bericht zeigt an, dass dieser Prozess derzeit nicht ausgeführt wird („ProcessId: not found“).
- **DemoWebApp222** wird ausgeführt und überwacht („Instrumented: true“). Basierend auf der Benutzerkonfiguration wurde der Instrumentierungsschlüssel „xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123“ für diese Website gefunden.
- **DemoWebApp333** wurde manuell mit dem Application Insights-SDK instrumentiert. Statusmonitor hat das SDK ermittelt und wird diese Website nicht überwachen.


### <a name="example-powershell-module-information"></a>Beispiel: Informationen zum PowerShell-Modul

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` aus, um Informationen über das aktuelle Modul anzuzeigen:

```
PS C:\> Get-ApplicationInsightsMonitoringStatus -PowerShellModule

PowerShell Module version:
0.4.0-alpha

Application Insights SDK version:
2.9.0.3872

Executing PowerShell Module Assembly:
Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.11432, Culture=neutral, PublicKeyToken=31bf3856ad364e35

PowerShell Module Directory:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.2\content\PowerShell

Runtime Paths:
ParentDirectory (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content

ConfigurationPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config

ManagedHttpModuleHelperPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll

RedfieldIISModulePath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

InstrumentationEngine86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll

InstrumentationEngine64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll

InstrumentationEngineExtensionHost86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll

InstrumentationEngineExtensionHost64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll

InstrumentationEngineExtensionConfig86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config

InstrumentationEngineExtensionConfig64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config

ApplicationInsightsSdkPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll
```

### <a name="example-runtime-status"></a>Beispiel: Laufzeitstatus

Sie können den Prozess auf dem instrumentierten Computer überprüfen, um herauszufinden, ob alle DLLs geladen wurden. Wenn die Überwachung funktioniert, sollten mindestens 12 DLLS geladen worden sein.

Führen Sie den Befehl `Get-ApplicationInsightsMonitoringStatus -InspectProcess` aus:


```
PS C:\> Get-ApplicationInsightsMonitoringStatus -InspectProcess

iisreset.exe /status
Status for IIS Admin Service ( IISADMIN ) : Running
Status for Windows Process Activation Service ( WAS ) : Running
Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
Status for World Wide Web Publishing Service ( W3SVC ) : Running

handle64.exe -accepteula -p w3wp
BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

listdlls64.exe -accepteula w3wp
0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
```

## <a name="parameters"></a>Parameter

### <a name="no-parameters"></a>(Keine Parameter)

Standardmäßig meldet dieses Cmdlet den Überwachungsstatus von Webanwendungen.
Verwenden Sie diese Option, um zu überprüfen, ob Ihre Anwendung erfolgreich instrumentiert wurde.
Sie können auch überprüfen, welcher Instrumentierungsschlüssel Ihrer Website zugeordnet wurde.


### <a name="-powershellmodule"></a>-PowerShellModule
**Optional**. Dieser Schalter meldet die Versionsnummern und Pfade von DLLs, die für die Überwachung erforderlich sind.
Verwenden Sie diese Option, wenn Sie die Version einer DLL ermitteln müssen (einschließlich Application Insights SDK).

### <a name="-inspectprocess"></a>-InspectProcess

**Optional**. Verwenden Sie diesen Schalter, um zu melden, ob IIS ausgeführt wird.
Damit werden auch externe Tools heruntergeladen, um zu ermitteln, ob die erforderlichen DLLs in die IIS-Laufzeit geladen werden.


Wenn dieser Prozess aus irgendeinem Grund fehlschlägt, können Sie diese Befehle auch manuell ausführen:
- iisreset.exe /status
- [handle64.exe](https://docs.microsoft.com/sysinternals/downloads/handle) -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
- [listdlls64.exe](https://docs.microsoft.com/sysinternals/downloads/listdlls) w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"


### <a name="-force"></a>-Force

**Optional**. Nur mit InspectProcess verwendet. Verwenden Sie diese Option, um die Benutzereingabeaufforderung zu überspringen, die angezeigt wird, bevor zusätzliche Tools heruntergeladen werden.


## <a name="next-steps"></a>Nächste Schritte

 Weitere Verwendungsmöglichkeiten für den Application Insights-Agent:
 - Verwenden Sie unsere Anleitung zur [Problembehandlung](status-monitor-v2-troubleshoot.md) für den Application Insights-Agent.
