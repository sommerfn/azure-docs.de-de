---
title: EventCounters in Application Insights | Microsoft-Dokumentation
description: Überwachen Sie systemeigene und benutzerdefinierte .NET/.NET Core-EventCounters in Application Insights.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: cithomas
ms.author: cithomas
ms.date: 09/20/2019
ms.openlocfilehash: 0762819239e8fd71a015f317776a94280806db53
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72677152"
---
# <a name="eventcounters-introduction"></a>Einführung in EventCounters

`EventCounter` ist ein .NET/.NET Core-Mechanismus zum Veröffentlichen und Verwenden von Indikatoren oder Statistiken. [Dieses](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md) Dokument gibt einen Überblick über `EventCounters` und zeigt Beispiele, wie diese veröffentlicht und verwendet werden. EventCounters werden auf allen Betriebssystemplattformen unterstützt: Windows, Linux und macOS. Sie können als plattformübergreifende Entsprechung für [PerformanceCounters](https://docs.microsoft.com/dotnet/api/system.diagnostics.performancecounter) angesehen werden, die nur auf Windows-Systemen unterstützt werden.

Während Benutzer beliebige benutzerdefinierte `EventCounters` veröffentlichen können, um ihre Anforderungen zu erfüllen, veröffentlicht die .NET Core 3.0-Runtime standardmäßig einen Satz dieser Indikatoren. In diesem Dokument werden Sie durch die erforderlichen Schritte zum Erfassen und Anzeigen von `EventCounters` (systemdefiniert oder benutzerdefiniert) in Azure Application Insights geführt.

## <a name="using-application-insights-to-collect-eventcounters"></a>Verwenden von Application Insights für das Erfassen von EventCounters

Application Insights unterstützt das Erfassen von `EventCounters` mit `EventCounterCollectionModule`, einem Bestandteil des neu veröffentlichten NuGet-Pakets [Microsoft.ApplicationInsights.EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector). `EventCounterCollectionModule` wird bei Verwendung von [AspNetCore](asp-net-core.md) oder [WorkerService](worker-service.md) automatisch aktiviert. `EventCounterCollectionModule` erfasst Indikatoren mit einer nicht konfigurierbaren Erfassungshäufigkeit von 60 Sekunden. Zum Erfassen von EventCounters sind keine speziellen Berechtigungen erforderlich.

## <a name="default-counters-collected"></a>Erfasste Standardindikatoren

Die folgenden Indikatoren werden für Apps, die in .NET Core 3.0 ausgeführt werden, automatisch vom SDK erfasst. Der Name der Indikatoren hat die Form „Kategorie|Indikator“.

|Category | Indikator|
|---------------|-------|
|`System.Runtime` | `cpu-usage` |
|`System.Runtime` | `working-set` |
|`System.Runtime` | `gc-heap-size` |
|`System.Runtime` | `gen-0-gc-count` |
|`System.Runtime` | `gen-1-gc-count` |
|`System.Runtime` | `gen-2-gc-count` |
|`System.Runtime` | `time-in-gc` |
|`System.Runtime` | `gen-0-size` |
|`System.Runtime` | `gen-1-size` |
|`System.Runtime` | `gen-2-size` |
|`System.Runtime` | `loh-size` |
|`System.Runtime` | `alloc-rate` |
|`System.Runtime` | `assembly-count` |
|`System.Runtime` | `exception-count` |
|`System.Runtime` | `threadpool-thread-count` |
|`System.Runtime` | `monitor-lock-contention-count` |
|`System.Runtime` | `threadpool-queue-length` |
|`System.Runtime` | `threadpool-completed-items-count` |
|`System.Runtime` | `active-timer-count` |
|`Microsoft.AspNetCore.Hosting` | `requests-per-second` |
|`Microsoft.AspNetCore.Hosting` | `total-requests` |
|`Microsoft.AspNetCore.Hosting` | `current-requests` |
|`Microsoft.AspNetCore.Hosting` | `failed-requests` |

> [!NOTE]
> Indikatoren der Kategorie „Microsoft.AspNetCore.Hosting“ werden nur in ASP.NET Core-Anwendungen hinzugefügt.

## <a name="customizing-counters-to-be-collected"></a>Anpassen zu erfassender Indikatoren

Das folgende Beispiel veranschaulicht das Hinzufügen und Entfernen von Indikatoren. Diese Anpassung erfolgt in der `ConfigureServices`-Methode Ihrer Anwendung, nachdem die Erfassung von Telemetriedaten durch Application Insights entweder mit `AddApplicationInsightsTelemetry()` oder `AddApplicationInsightsWorkerService()` aktiviert wurde. Nachfolgend sehen Sie einen Beispielcode aus einer ASP.NET Core-Anwendung. Informationen zu anderen Anwendungstypen finden Sie in [diesem](worker-service.md#configuring-or-removing-default-telemetrymodules) Dokument.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows several customizations done to EventCounterCollectionModule.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

        // The following code removes EventCounterCollectionModule to disable the module completely.
        var eventCounterModule = services.FirstOrDefault<ServiceDescriptor>
                    (t => t.ImplementationType == typeof(EventCounterCollectionModule));
        if (eventCounterModule != null)
        {
            services.Remove(eventCounterModule);
        }
    }
```

## <a name="event-counters-in-metric-explorer"></a>EventCounters im Metrik-Explorer

Zum Anzeigen von EventCounter-Metriken im [Metrik-Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) wählen Sie die Application Insights-Ressource aus, und wählen Sie dann protokollbasierte Metriken als Metriknamespace aus. EventCounter-Metriken werden dann unter der Kategorie „PerformanceCounter“ angezeigt.

> [!div class="mx-imgBorder"]
> ![In Application Insights gemeldete EventCounters](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>EventCounters in Analytics

Sie können Berichte zu EventCounters auch in [Analytics](../../azure-monitor/app/analytics.md) in der Tabelle **performanceCounters** suchen und anzeigen.

Führen Sie z.B. die folgende Abfrage aus, um zu sehen, welche Indikatoren erfasst werden und für die Abfrage verfügbar sind:

```Kusto
performanceCounters | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![In Application Insights gemeldete EventCounters](./media/event-counters/analytics-event-counters.png)

Um ein Diagramm eines bestimmten Indikators (z.B. `ThreadPool Completed Work Item Count`) im aktuellen Zeitraum zu erhalten, führen Sie die folgende Abfrage aus.

```Kusto
performanceCounters 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Diagramm eines einzelnen Indikators in Application Insights](./media/event-counters/analytics-completeditems-counters.png)

Wie andere Telemetriedaten umfasst auch **performanceCounters** eine Spalte `cloud_RoleInstance`, die die Identität der Hostserverinstanz angibt, auf dem Ihre Anwendung ausgeführt wird. Die obige Abfrage zeigt den Indikatorwert pro Instanz und kann zum Vergleichen der Leistung verschiedener Serverinstanzen verwendet werden.

## <a name="alerts"></a>Alerts
Wie bei anderen Metriken können Sie [eine Warnung einrichten](../../azure-monitor/app/alerts.md), damit Sie gewarnt werden, wenn ein EventCounter einen von Ihnen festgelegten Grenzwert überschreitet. Öffnen Sie den Bereich „Warnungen“, und klicken Sie auf „Warnung hinzufügen“.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Werden EventCounters in Livemetriken angezeigt?

Livemetriken zeigen derzeit keine EventCounters an. Verwenden Sie den Metrik-Explorer oder Analytics, um die Telemetrie anzuzeigen.

### <a name="which-platforms-can-i-see-the-default-list-of-net-core-30-counters"></a>Auf welchen Plattformen kann die Standardliste der .NET Core 3.0-Indikatoren angezeigt werden?

EventCounter erfordert keine besonderen Berechtigungen und wird auf allen Plattformen unterstützt, auf denen .NET Core 3.0 unterstützt wird. Dies umfasst:

* **Betriebssystem**: Windows, Linux oder macOS.
* **Hostingmethode:** In-Process oder Out-of-Process.
* **Bereitstellungsmethode:** Abhängig vom Framework oder eigenständig.
* **Webserver:** IIS (Internet Information Server) oder Kestrel.
* **Hostingplattform:** Beispielsweise das Web-Apps-Feature von Azure App Service, Azure Virtual Machines, Docker oder Azure Kubernetes Service (AKS).

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Ich habe Application Insights über das Azure-Web-App-Portal aktiviert. Es werden aber keine EventCounters angezeigt.

 Die [Application Insights-Erweiterung](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) für ASP.NET Core unterstützt dieses Feature noch nicht. Dieses Dokument wird aktualisiert, sobald dieses Feature unterstützt wird.

## <a name="next"></a>Nächste Schritte

* [Abhängigkeitsüberwachung](../../azure-monitor/app/asp-net-dependencies.md)