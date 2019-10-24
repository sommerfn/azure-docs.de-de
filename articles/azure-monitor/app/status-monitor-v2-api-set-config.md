---
title: 'API-Referenz für den Azure Application Insights-Agent: Festlegen der Konfiguration | Microsoft-Dokumentation'
description: API-Referenz für den Application Insights-Agent. Set-ApplicationInsightsMonitoringConfig. Überwachen Sie die Websiteleistung ohne erneute Bereitstellung der Website. Funktioniert mit ASP.NET-Web-Apps, die lokal, auf virtuellen Computern oder in Azure gehostet werden.
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
ms.openlocfilehash: 2ab941b5587a8836f1e472fbce3966b12bfa1e11
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388258"
---
# <a name="application-insights-agent-api-set-applicationinsightsmonitoringconfig"></a>Application Insights-Agent-API: Set-ApplicationInsightsMonitoringConfig

In diesem Dokument wird ein Cmdlet des [PowerShell-Moduls „Az.ApplicationMonitor“](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/) beschrieben.

## <a name="description"></a>BESCHREIBUNG

Legt die Konfigurationsdatei fest, ohne eine vollständige Neuinstallation auszuführen.
Starten Sie IIS neu, damit die Änderungen wirksam werden.

> [!IMPORTANT] 
> Dieses Cmdlet erfordert eine PowerShell-Sitzung mit Administratorberechtigungen.


## <a name="examples"></a>Beispiele

### <a name="example-with-a-single-instrumentation-key"></a>Beispiel mit einem einzelnen Instrumentierungsschlüssel
In diesem Beispiel wird allen Apps auf dem aktuellen Computer ein einzelner Instrumentierungsschlüssel zugewiesen.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Beispiel mit Zuordnung des Instrumentierungsschlüssels
In diesem Beispiel:
- `MachineFilter` sucht mithilfe des Platzhalters `'.*'` nach dem aktuellen Computer.
- `AppFilter='WebAppExclude'` gibt `null` als Instrumentierungsschlüssel an. Die angegebene App wird nicht instrumentiert.
- `AppFilter='WebAppOne'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- `AppFilter='WebAppTwo'` weist der angegebenen App einen eindeutigen Instrumentierungsschlüssel zu.
- Abschließend verwendet `AppFilter` ebenfalls den Platzhalter `'.*'`, um alle anderen Web-Apps zu suchen, die nicht von den früheren Regeln gefunden wurden, und weist einen Standardinstrumentierungsschlüssel zu.
- Zur besseren Lesbarkeit werden Leerzeichen hinzugefügt.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parameter

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Erforderlich.** Verwenden Sie diesen Parameter, um einen einzelnen Instrumentierungsschlüssel für die Verwendung durch alle Apps auf dem Zielcomputer bereitzustellen.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Erforderlich.** Verwenden Sie diesen Parameter, um mehrere Instrumentierungsschlüssel und eine Zuordnung der von jeder einzelnen App verwendeten Instrumentierungsschlüssel bereitzustellen.
Durch Festlegen von `MachineFilter` können Sie ein einzelnes Installationsskript für mehrere Computer erstellen.

> [!IMPORTANT]
> Apps werden anhand der Regeln in der Reihenfolge gesucht, in der diese Regeln bereitgestellt werden. Geben Sie daher zuerst die spezifischsten Regeln und am Schluss die allgemeinsten Regeln an.

#### <a name="schema"></a>Schema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** ist ein erforderlicher regulärer C#-Ausdruck des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ComputerName“ findet nur Computer mit dem angegebenen Namen.
- **AppFilter** ist ein erforderlicher regulärer C#-Ausdruck des Computer- oder VM-Namens.
    - „.*“ findet alle
    - „ApplicationName“ findet nur IIS-Apps mit dem angegebenen Namen.
- **InstrumentationKey** ist erforderlich, um die Überwachung der Apps zu aktivieren, die den beiden obigen Filtern entsprechen.
    - Behalten Sie für diesen Wert NULL bei, wenn Sie Regeln zum Ausschließen der Überwachung definieren möchten.


### <a name="-verbose"></a>-Verbose
**Allgemeiner Parameter.** Verwenden Sie diesen Parameter, um detaillierte Protokolle anzuzeigen.


## <a name="output"></a>Output

Standardmäßig erfolgt keine Ausgabe.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Beispielhafte ausführliche Ausgabe beim Festlegen der Konfigurationsdatei mit -InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Beispielhafte ausführliche Ausgabe beim Festlegen der Konfigurationsdatei mit -InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
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
- [Fügen Sie Ihrem Code das Application Insights SDK hinzu](../../azure-monitor/app/asp-net.md), um Ablaufverfolgungs- und Protokollaufrufe einfügen zu können.
 
 Weitere Verwendungsmöglichkeiten für den Application Insights-Agent:
 - Verwenden Sie unsere Anleitung zur [Problembehandlung](status-monitor-v2-troubleshoot.md) für den Application Insights-Agent.
 - [Rufen Sie die Konfiguration ab](status-monitor-v2-api-get-config.md), um sicherzustellen, dass Ihre Einstellungen korrekt erfasst wurden.
 - [Rufen Sie den Status ab](status-monitor-v2-api-get-status.md), um die Überwachung zu überprüfen.
