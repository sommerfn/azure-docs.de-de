---
title: Live Metrics Stream mit benutzerdefinierten Metriken und Diagnosen in Azure Application Insights | Microsoft-Dokumentation
description: Überwachen Sie Ihre Web-App mit benutzerdefinierten Metriken in Echtzeit, und diagnostizieren Sie Probleme mit einem Livefeed zu Fehlern, Ablaufverfolgungen und Ereignissen.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/22/2019
ms.reviewer: sdash
ms.openlocfilehash: 69aaa61bb0be9a5f07de85ff4ef81b28a86aefaa
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575617"
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Live Metrics Stream: Überwachung und Diagnose mit einer Latenzzeit von 1 Sekunde

Überprüfen Sie die Funktionsfähigkeit Ihrer Livewebanwendung in der Produktion mithilfe von Live Metrics Stream von [Application Insights](../../azure-monitor/app/app-insights-overview.md). Wählen und filtern Sie Metriken und Leistungsindikatoren für die Überwachung in Echtzeit, ohne dass der Dienst beeinträchtigt wird. Überprüfen Sie Stapelüberwachungen von fehlerhaften Anforderungen und Ausnahmen. Zusammen mit dem [Profiler](../../azure-monitor/app/profiler.md) und dem [Momentaufnahmedebugger](../../azure-monitor/app/snapshot-debugger.md) stellt Live Metrics Stream ein leistungsfähiges und nicht invasives Diagnosetool für Ihre Live-Website dar.

Mit Live Metrics Stream haben Sie folgende Möglichkeiten:

* Überprüfen Sie eine veröffentlichte Korrektur durch Überwachen von Leistungs- und Fehlerindikatoren.
* Überwachen Sie die Auswirkungen von Testauslastungen, und diagnostizieren Sie Probleme im laufenden Betrieb. 
* Konzentrieren Sie sich auf bestimmte Testsitzungen, oder filtern Sie bekannte Probleme heraus, indem Sie die zu überwachenden Metriken auswählen und filtern.
* Rufen Sie Ausnahmeablaufverfolgungen in Echtzeit ab.
* Experimentieren Sie mit Filtern, um die wichtigsten KPIs zu ermitteln.
* Überwachen Sie Windows-Leistungsindikatoren live.
* Identifizieren Sie mühelos einen Server mit Problemen, und filtern Sie nach allen KPIs/dem Livefeed nur für diesen Server.

[![Video zu Live Metrics Stream](./media/live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Livemetriken werden derzeit für ASP.NET-, ASP.NET Core-, Azure Functions-, Java- und Node.js-Apps unterstützt.

## <a name="get-started"></a>Erste Schritte

1. Wenn Sie Application Insights noch nicht in Ihrer Web-App [installiert](../../azure-monitor/azure-monitor-app-hub.md) haben, holen Sie das jetzt nach.
2. Zusätzlich zu den Application Insights-Standardpaketen ist [Microsoft.ApplicationInsights.PerfCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) erforderlich, um Live Metrics Stream zu aktivieren.
3. Führen Sie ein **Update auf die neueste Version** des Application Insights-Pakets durch. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Öffnen Sie die Registerkarte **Updates**, und wählen Sie alle Microsoft.ApplicationInsights.*-Pakete aus.

    Stellen Sie Ihre App erneut bereit.

3. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource für Ihre App und anschließend Live Stream.

4. [Sichern Sie den Steuerkanal](#secure-the-control-channel), wenn Sie sensible Daten (z.B. Kundennamen) in Ihren Filtern verwenden möchten.

### <a name="no-data-check-your-server-firewall"></a>Sie sehen keine Daten? Überprüfen Sie die Serverfirewall

Überprüfen Sie, ob die [ausgehenden Ports für Live Metrics Stream](../../azure-monitor/app/ip-addresses.md#outgoing-ports) in der Firewall Ihrer Server geöffnet sind.

## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Wie unterscheidet sich Live Metrics Stream von Metrik-Explorer und Analytics?

| |Live Stream | Metrik-Explorer und Analytics |
|---|---|---|
|Latenz|Daten werden innerhalb einer Sekunde angezeigt.|Aggregation innerhalb mehrerer Minuten|
|Keine Beibehaltung|Daten werden beibehalten, solange sie im Diagramm angezeigt werden, und dann verworfen.|[Daten werden 90 Tage lang beibehalten.](../../azure-monitor/app/data-retention-privacy.md#how-long-is-the-data-kept)|
|On-Demand-Streaming|Daten werden beim Öffnen von Live Metrics gestreamt.|Daten werden gesendet, sobald das SDK installiert und aktiviert wird.|
|Kostenlos|Keine Gebühren für Live Stream-Daten|[Gebührenpflichtig](../../azure-monitor/app/pricing.md)
|Stichproben|Alle ausgewählten Metriken und Indikatoren werden übertragen. Für Fehler und Stapelüberwachungen werden Stichproben erstellt. Telemetrieprozessoren werden nicht angewendet.|Für Ereignisse können [Stichproben](../../azure-monitor/app/api-filtering-sampling.md) erstellt werden.|
|Steuerkanal|Filtersteuersignale werden an das SDK gesendet. Es empfiehlt sich, diesen Kanal zu sichern.|Unidirektionale Kommunikation zum Portal|

## <a name="select-and-filter-your-metrics"></a>Auswählen und Filtern der Metriken

(Verfügbar mit ASP.NET, ASP.NET Core und Azure Functions (v2).)

Sie können benutzerdefinierte KPIs live überwachen, indem Sie im Portal beliebige Filter auf Application Insights-Telemetrie anwenden. Klicken Sie auf das Filtersteuerelement, das angezeigt wird, wenn Sie den Mauszeiger über eines der Diagramme bewegen. Das folgende Diagramm stellt eine benutzerdefinierte KPI für die Anforderungsanzahl mit Filtern für die Attribute „URL“ und „Dauer“ dar. Überprüfen Sie Ihre Filter in der Streamvorschau, die jederzeit einen Livefeed von Telemetriedaten anzeigt, die den von Ihnen angegeben Kriterien entsprechen.

![Benutzerdefinierte Anforderungs-KPI](./media/live-stream/live-stream-filteredMetric.png)

Sie können einen anderen Wert als die Anzahl überwachen. Die Optionen hängen von der Typ des Streams ab. Bei diesem kann es sich um eine beliebige Application Insights-Telemetrie handeln: Anforderungen, Abhängigkeiten, Ausnahmen, Ablaufverfolgungen, Ereignisse oder Metriken. Sie können auch eine eigene [benutzerdefinierte Messung](../../azure-monitor/app/api-custom-events-metrics.md#properties) verwenden:

![Wertoptionen](./media/live-stream/live-stream-valueoptions.png)

Neben Application Insights-Telemetrie können Sie auch einen beliebigen Windows-Leistungsindikator überwachen, indem Sie diese Streamoptionen auswählen und den Namen des Leistungsindikators angeben.

Livemetriken werden an zwei Punkten aggregiert: lokal auf jedem Server und übergreifend auf allen Servern. Sie können die Standardeinstellung durch die Auswahl anderer Optionen in der entsprechenden Dropdownliste ändern.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Beispieltelemetrie: Benutzerdefinierte Livediagnoseereignisse
Der Livefeed von Ereignissen zeigt standardmäßig Beispiele für fehlgeschlagene Anforderungen und Abhängigkeitsaufrufe, Ausnahmen, Ereignisse und Ablaufverfolgungen an. Klicken Sie jederzeit auf das Filtersymbol, um die angewendeten Kriterien anzuzeigen. 

![Standard-Livefeed](./media/live-stream/live-stream-eventsdefault.png)

Wie bei Metriken können Sie beliebige Kriterien für alle Application Insights-Telemetrietypen angeben. In diesem Beispiel wählen wir bestimmte Anforderungsfehler, Ablaufverfolgungen und Ereignisse aus. Wir wählen außerdem alle Ausnahmen und Abhängigkeitsfehler aus.

![Benutzerdefinierter Livefeed](./media/live-stream/live-stream-events.png)

Hinweis: Aktuell muss für meldungsbasierte Ausnahmekriterien die Meldung für äußere Ausnahmen verwendet werden. Verwenden Sie im vorangehenden Beispiel zum Herausfiltern mit der Meldung für innere Ausnahmen (folgt auf das Trennzeichen „<--“) „Der Client wurde getrennt.“ eine Meldung, die keine Kriterien vom Typ „Fehler beim Lesen des Anforderungsinhalts“ enthält.

Klicken Sie auf ein Element im Livefeed, um die Details dazu anzuzeigen. Sie können den Feed anhalten, indem Sie auf **Anhalten** klicken, einfach nach unten scrollen oder auf ein Element klicken. Der Livefeed wird fortgesetzt, wenn Sie wieder nach oben scrollen oder auf den Zähler der Elemente klicken, die im angehaltenen Zustand erfasst wurden.

![Auswahl der Live-Fehler](./media/live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Filtern nach Serverinstanz

Wenn Sie eine bestimmte Serverrolleninstanz überwachen möchten, können Sie nach Server filtern.

![Auswahl der Live-Fehler](./media/live-stream/live-stream-filter.png)

## <a name="secure-the-control-channel"></a>Sichern des Steuerkanals
Die von Ihnen angegebenen benutzerdefinierten Filterkriterien werden an die Livemetrikkomponente des Application Insights SDK zurückgesendet. Der Filter können potenziell vertrauliche Informationen wie z.B. Kunden-IDs enthalten. Zum Sichern des Kanals können Sie neben dem Instrumentierungsschlüssel auch einen geheimen API-Schlüssel verwenden.
### <a name="create-an-api-key"></a>Erstellen eines API-Schlüssels

![API-Schlüssel erstellen](./media/live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>Hinzufügen eines API-Schlüssels zur Konfiguration

### <a name="classic-aspnet"></a>ASP.NET (klassisch)

Fügen Sie den API-Authentifizierungsschlüssel in der Datei „applicationinsights.config“ dem QuickPulseTelemetryModule-Element hinzu:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
Oder legen Sie ihn im Code auf dem QuickPulseTelemetryModule-Element fest:

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;

             TelemetryConfiguration configuration = new TelemetryConfiguration();
            configuration.InstrumentationKey = "YOUR-IKEY-HERE";

            QuickPulseTelemetryProcessor processor = null;

            configuration.TelemetryProcessorChainBuilder
                .Use((next) =>
                {
                    processor = new QuickPulseTelemetryProcessor(next);
                    return processor;
                })
                        .Build();

            var QuickPulse = new QuickPulseTelemetryModule()
            {

                AuthenticationApiKey = "YOUR-API-KEY"
            };
            QuickPulse.Initialize(configuration);
            QuickPulse.RegisterTelemetryProcessor(processor);
            foreach (var telemetryProcessor in configuration.TelemetryProcessors)
                {
                if (telemetryProcessor is ITelemetryModule telemetryModule)
                    {
                    telemetryModule.Initialize(configuration);
                    }
                }

```

### <a name="azure-function-apps"></a>Azure Function-Apps

Für Azure-Funktions-Apps (v2) kann der Kanal über eine Umgebungsvariable mit einem API-Schlüssel abgesichert werden.

Erstellen Sie einen API-Schlüssel in Ihrer Application Insights-Ressource, und wechseln Sie zu **Anwendungseinstellungen** für Ihre Funktionen-App. Wählen Sie **Neue Einstellung hinzuzufügen** aus, und geben Sie den Namen `APPINSIGHTS_QUICKPULSEAUTHAPIKEY` und einen Wert entsprechend Ihrem API-Schlüssel ein.

### <a name="aspnet-core-requires-application-insights-aspnet-core-sdk-230-beta-or-greater"></a>ASP.NET Core (erfordert mindestens Application Insights ASP.NET Core SDK 2.3.0-beta)

Ändern Sie die Datei „startup.cs“ wie folgt:

Fügen Sie zunächst Folgendes hinzu

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
```

Fügen Sie dann in der ConfigureServices-Methode Folgendes hinzu:

```csharp
services.ConfigureTelemetryModule<QuickPulseTelemetryModule> ((module, o) => module.AuthenticationApiKey = "YOUR-API-KEY-HERE");
```

Wenn Sie jedoch alle verbundenen Server erkennen und ihnen vertrauen, können Sie die benutzerdefinierte Filter ohne den authentifizierten Kanal ausprobieren. Diese Option steht sechs Monate lang zur Verfügung. Diese Überschreibung muss einmal für jede neue Sitzung, oder wenn ein neuer Server online geschaltet wird, durchgeführt werden.

![Authentifizierungsoptionen für Livemetriken](./media/live-stream/live-stream-auth.png)

>[!NOTE]
>Es wird dringend empfohlen, dass Sie den authentifizierten Kanal vor dem Eingeben von potenziell vertraulichen Informationen wie der Kunden-ID in den Filterkriterien einrichten.
>

## <a name="supported-features-table"></a>Tabelle unterstützter Features

| Sprache                         | Grundlegende Metriken       | Leistungsmetriken | Benutzerdefinierte Filterung    | Beispieltelemetrie    | CPU-Aufteilung nach Prozessen |
|----------------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:---------------------|
| .NET                             | Unterstützt (V2.7.2+) | Unterstützt (V2.7.2+) | Unterstützt (V2.7.2+) | Unterstützt (V2.7.2+) | Unterstützt (V2.7.2+)  |
| .NET Core (target=.NET Framework)| Unterstützt (V2.4.1+) | Unterstützt (V2.4.1+) | Unterstützt (V2.4.1+) | Unterstützt (V2.4.1+) | Unterstützt (V2.4.1+)  |
| .NET Core (target=.NET Core)     | Unterstützt (V2.4.1+) | Unterstützt*          | Unterstützt (V2.4.1+) | Unterstützt (V2.4.1+) | **Nicht unterstützt**    |
| Azure Functions v2               | Unterstützt           | Unterstützt           | Unterstützt           | Unterstützt           | **Nicht unterstützt**    |
| Java                             | Unterstützt (V2.0.0+) | Unterstützt (V2.0.0+) | **Nicht unterstützt**   | **Nicht unterstützt**   | **Nicht unterstützt**    |
| Node.js                          | Unterstützt (V1.3.0+) | Unterstützt (V1.3.0+) | **Nicht unterstützt**   | Unterstützt (V1.3.0+) | **Nicht unterstützt**    |

Grundlegende Metriken umfassen Anforderung, Abhängigkeit und Ausnahmerate. Leistungsmetriken (Leistungsindikatoren) umfassen Arbeitsspeicher und CPU. Beispieltelemetriedaten zeigen einen Datenstrom ausführlicher Informationen zu nicht erfolgreichen Anforderungen und Abhängigkeiten, Ausnahmen, Ereignissen und Ablaufverfolgungen.

 \* PerfCounters-Unterstützung unterscheidet sich geringfügig von .NET Core-Versionen, die nicht für .NET Framework bestimmt sind:

- PerfCounters-Metriken werden unterstützt, wenn sie in Azure App Service für Windows ausgeführt werden. (AspNetCore SDK Version 2.4.1 oder höher)
- PerfCounters werden unterstützt, wenn die App auf einem BELIEBIGEN Windows-Computer (VM oder Clouddienst oder lokal usw.) ausgeführt wird (AspNetCore SDK Version 2.7.1 oder höher), aber für Apps, die für .NET Core 2.0 oder höher bestimmt sind.
- PerfCounters werden unterstützt, wenn die App AN BELIEBIGEM ORT (Linux, Windows, App Service für Linux, Container usw.) in der neuesten Betaversion ausgeführt wird (d. h. AspNetCore SDK Version 2.8.0-beta1 oder höher), aber für Apps, die für .NET Core 2.0 oder höher bestimmt sind.

Standardmäßig ist Live Metrics im Node.js SDK deaktiviert. Um Live Metrics zu aktivieren, fügen Sie `setSendLiveMetrics(true)` Ihren [Konfigurationsmethoden](https://github.com/Microsoft/ApplicationInsights-node.js#configuration) hinzu, während Sie das SDK initialisieren.

## <a name="troubleshooting"></a>Problembehandlung

Sie sehen keine Daten? Wenn sich Ihre Anwendung in einem geschützten Netzwerk befindet: Live Metrics Stream verwendet andere IP-Adressen als sonstige Application Insights-Telemetriedaten. Stellen Sie sicher, dass [diese IP-Adressen](../../azure-monitor/app/ip-addresses.md) in Ihrer Firewall geöffnet sind.

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen der Verwendung mit Application Insights](../../azure-monitor/app/usage-overview.md)
* [Verwenden der Diagnosesuche](../../azure-monitor/app/diagnostic-search.md)
* [Profiler](../../azure-monitor/app/profiler.md)
* [Snapshot Debugger](../../azure-monitor/app/snapshot-debugger.md)
