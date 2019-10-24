---
title: 'API-Referenz für den Azure Application Insights-Agent: Starten der Ablaufverfolgung | Microsoft-Dokumentation'
description: API-Referenz für den Application Insights-Agent. Starten der Ablaufverfolgung. Sammeln von ETW-Protokollen von Statusmonitor und Application Insights SDK.
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
ms.openlocfilehash: b1c5aa34c46a20631b328abfb061dc2477150c72
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389850"
---
# <a name="application-insights-agent-api-start-applicationinsightsmonitoringtrace"></a>Application Insights-Agent-API: Start-ApplicationInsightsMonitoringTrace

In diesem Artikel wird ein Cmdlet des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) beschrieben.

## <a name="description"></a>BESCHREIBUNG

Sammelt [ETW-Ereignisse](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) aus der Runtime des Anfügens ohne Code. Dieses Cmdlet ist eine Alternative zur Ausführung von [PerfView](https://github.com/microsoft/perfview).

Gesammelte Ereignisse werden an der Konsole in Echtzeit ausgegeben und in einer ETL-Datei gespeichert. Die ausgegebene ETL-Datei kann zur weiteren Untersuchung von [PerfView](https://github.com/microsoft/perfview) geöffnet werden.

Dieses Cmdlet wird ausgeführt, bis es die Timeoutdauer erreicht (standardmäßig 5 Minuten) oder manuell gestoppt wird (`Ctrl + C`).

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorberechtigungen.

## <a name="examples"></a>Beispiele

### <a name="how-to-collect-events"></a>Vorgehensweise beim Sammeln von Ereignissen

Normalerweise würden wir Sie auffordern, Ereignisse zu sammeln, um zu untersuchen, warum Ihre Anwendung nicht instrumentiert wird.

Die Runtime des Anfügens ohne Code sendet ETW-Ereignisse beim Start von IIS und beim Start Ihrer Anwendung.

So sammeln Sie diese Ereignisse:
1. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /stop` aus, um IIS und alle Webanwendungen zu deaktivieren.
2. Ausführen dieses Cmdlets
3. Führen Sie in einer Befehlszeilenkonsole mit Administratorrechten `iisreset /start` aus, um IIS zu starten.
4. Versuchen Sie, zu Ihrer App zu navigieren.
5. Nachdem Ihre App geladen ist, können Sie sie manuell beenden (`Ctrl + C`) oder auf das Timeout warten.

### <a name="what-events-to-collect"></a>Welche Ereignisse sollen gesammelt werden?

Beim Sammeln von Ereignissen haben Sie drei Möglichkeiten:
1. Verwenden Sie den Schalter `-CollectSdkEvents` zum Sammeln von Ereignissen, die vom Application Insights SDK ausgegeben werden.
2. Verwenden Sie den Schalter `-CollectRedfieldEvents` zum Sammeln von Ereignissen, die von Statusmonitor und Redfield Runtime ausgegeben werden. Diese Protokolle sind hilfreich zur Diagnose von IIS und Start der Anwendung.
3. Verwenden Sie beide Schalter, um beide Ereignistypen zu sammeln.
4. Wenn kein Schalter angegeben ist, werden standardmäßig beide Ereignistypen gesammelt.


## <a name="parameters"></a>Parameter

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Optional.** Verwenden Sie diesen Parameter, um festzulegen, wie lange das Skript Ereignisse sammeln soll. Die Standardeinstellung ist 5 Minuten.

### <a name="-logdirectory"></a>-LogDirectory
**Optional.** Verwenden Sie diesen Schalter, um das Ausgabeverzeichnis der ETL-Datei festzulegen. Standardmäßig wird diese Datei im PowerShell Modules-Verzeichnis erstellt. Der vollständige Pfad wird während der Ausführung des Skripts angezeigt.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**Optional.** Verwenden Sie diesen Schalter, um Application Insights SDK-Ereignisse zu sammeln.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**Optional.** Verwenden Sie diesen Schalter zum Sammeln von Ereignissen von Statusmonitor und Redfield Runtime.

### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle auszugeben.



## <a name="output"></a>Output


### <a name="example-of-application-startup-logs"></a>Beispiel für Anwendungsstartprotokolle
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Problembehandlung:

- Weitere Schritte zur Problembehandlung finden Sie hier: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Informieren Sie sich in der [API-Referenz](status-monitor-v2-overview.md#powershell-api-reference) über Parameter, die Sie möglicherweise übersehen haben.
- Wenn Sie zusätzliche Hilfe benötigen, können Sie uns auf [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues) kontaktieren.



 Weitere Verwendungsmöglichkeiten für den Application Insights-Agent:
 - Verwenden Sie unsere Anleitung zur [Problembehandlung](status-monitor-v2-troubleshoot.md) für den Application Insights-Agent.
 - [Rufen Sie die Konfiguration ab](status-monitor-v2-api-get-config.md), um sicherzustellen, dass Ihre Einstellungen korrekt erfasst wurden.
 - [Rufen Sie den Status ab](status-monitor-v2-api-get-status.md), um die Überwachung zu überprüfen.
