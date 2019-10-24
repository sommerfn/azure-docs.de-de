---
title: Filterung und Vorverarbeitung im Azure Application Insights-SDK | Microsoft Docs
description: Schreiben Sie Telemetrieprozessoren und Telemetrieinitialisierer für das SDK, um die Daten zu filtern oder ihnen Eigenschaften hinzuzufügen, bevor die Telemetriedaten an das Application Insights-Portal gesendet werden.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: cae035927217a7e2677cf6ebfcce1b53782e4c01
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72248742"
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filterung und Vorverarbeitung von Telemetriedaten im Application Insights-SDK

Sie können Plug-Ins für das Application Insights SDK schreiben und konfigurieren, um anzupassen, wie Telemetriedaten vor dem Senden an den Application Insights-Dienst angereichert und verarbeitet werden.

* [Erstellen von Stichproben](sampling.md) verringert sich der Umfang der Telemetriedaten, ohne Statistiken zu verfälschen. Zusammengehörige Datenpunkte werden dabei zusammengehalten, sodass Sie bei der Diagnose von Problemen zwischen diesen navigieren können. Im Portal wird die Gesamtanzahl multipliziert, um eine Kompensation der Stichproben zu erreichen.
* Durch das Filtern mit Telemetrieprozessoren können Sie Telemetriedaten im SDK herausfiltern, bevor sie an den Server gesendet werden. Sie können beispielsweise den Umfang der Telemetriedaten verringern, indem Sie Anforderungen von Robots ausschließen. Filtern stellt ein grundlegenderes Verfahren zur Senkung des Datenverkehrs dar als das Erstellen von Stichproben. Sie können so besser steuern, was übertragen wird. Jedoch müssen Sie beachten, dass dies Auswirkungen auf die Statistik hat – wenn Sie z. B. alle erfolgreichen Anforderungen herausfiltern.
* [Telemetrieinitialisierer](#add-properties) fügen Eigenschaften zu beliebigen von der App gesendeten Telemetriedaten hinzu oder ändern sie, einschließlich Telemetriedaten von Standardmodulen. Sie könnten z. B. berechnete Werte hinzufügen oder Versionsnummern, nach denen Sie die Daten im Portal filtern.
* [SDK-API](../../azure-monitor/app/api-custom-events-metrics.md) wird zum Senden benutzerdefinierter Ereignisse und Metriken verwendet.

Vorbereitung:

* Installieren Sie das für Ihre Anwendung geeignete SDK in Ihrer App: [ASP.NET](asp-net.md), [ASP.NET Core](asp-net-core.md), [Nicht-HTTP/Worker für .NET/.NET Core](worker-service.md) oder [Java](../../azure-monitor/app/java-get-started.md).

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a>Filtern: ITelemetryProcessor

Mit dieser Technik können Sie direkt kontrollieren, welche Daten in den Telemetriedatenstrom ein- oder daraus ausgeschlossen werden sollen. Filtern Sie Telemetrieelemente vor dem Senden an Application Insights heraus. Sie können sie zusammen mit der Stichprobenerstellung oder einzeln verwenden.

Zum Filtern von Telemetriedaten schreiben Sie einen Telemetrieprozessor und registrieren ihn bei `TelemetryConfiguration`. Alle Telemetriedaten durchlaufen den Prozessor. Dabei können Sie auswählen, welche Daten aus dem Datenstrom gelöscht und welche an den nächsten Prozessor in der Kette weitergegeben werden sollen. Dies schließt Telemetriedaten aus den Standardmodulen wie etwa dem HTTP-Anforderungssammler und der Abhängigkeitserfassung ein sowie von Ihnen selbst überwachte Telemetriedaten. Sie können z. B. Telemetriedaten zu Anforderungen von Robots oder erfolgreiche Abhängigkeitsaufrufe herausfiltern.

> [!WARNING]
> Die Filterung der vom SDK gesendeten Telemetriedaten mithilfe von Prozessoren kann die im Portal angezeigten Statistiken verfälschen und die Nachverfolgung verwandter Elemente erschweren.
>
> Verwenden Sie stattdessen [Sampling](../../azure-monitor/app/sampling.md).
>
>

### <a name="create-a-telemetry-processor-c"></a>Erstellen eines Telemetrieprozessors (C#)

1. Implementieren Sie `ITelemetryProcessor`, um einen Filter zu erstellen:

    Beachten Sie, dass Telemetrieprozessoren eine Verarbeitungskette erstellen. Beim Instanziieren eines Telemetrieprozessors erhalten Sie einen Verweis auf den nächsten Prozessor in der Kette. Wenn ein Telemetriedatenpunkt an die Verarbeitungsmethode übergeben wird, führt er seine Aufgaben aus und ruft den nächsten Telemetrieprozessor in der Kette auf (oder nicht).

    ```csharp
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
    {
        private ITelemetryProcessor Next { get; set; }

        // next will point to the next TelemetryProcessor in the chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }

        public void Process(ITelemetry item)
        {
            // To filter out an item, return without calling the next processor.
            if (!OKtoSend(item)) { return; }

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }
    }
    ```

2. Fügen Sie den Prozessor hinzu.

**ASP.NET-Apps**: Fügen Sie den folgenden Codeausschnitt in „ApplicationInsights.config“ ein:

```xml
<TelemetryProcessors>
  <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
     <!-- Set public property -->
     <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
  </Add>
</TelemetryProcessors>
```

Sie können durch die Bereitstellung von öffentlich benannten Eigenschaften in Ihrer Klasse Zeichenfolgenwerte aus der .config-Datei übergeben.

> [!WARNING]
> Achten Sie darauf, dass die Typnamen und sonstige Eigenschaftennamen in der .config-Datei mit den Klassen- und Eigenschaftennamen im Code übereinstimmen. Wenn die .config-Datei auf einen nicht vorhandenen Typ oder eine nicht vorhandene Eigenschaft verweist, könnte das SDK möglicherweise automatisch keine Telemetriedaten mehr senden.
>

**Alternativ** können Sie den Filter im Code initialisieren. Fügen Sie in einer geeigneten Initialisierungsklasse, z. B. AppStart in `Global.asax.cs`, Ihren Prozessor in die Kette ein:

```csharp
var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
builder.Use((next) => new SuccessfulDependencyFilter(next));

// If you have more processors:
builder.Use((next) => new AnotherProcessor(next));

builder.Build();
```

Nach diesem Punkt erstellte TelemetryClients-Elemente verwenden Ihre Prozessoren.

**ASP.NET Core/Workerdienst-Apps**

> [!NOTE]
> Das Hinzufügen eines Prozessors mithilfe von `ApplicationInsights.config` oder `TelemetryConfiguration.Active` ist für ASP.NET Core-Anwendungen sowie für das SDK „Microsoft.ApplicationInsights.WorkerService“ ungültig.

Bei Apps, die mit [ASP.NET Core](asp-net-core.md#adding-telemetry-processors) oder [WorkerService](worker-service.md#adding-telemetry-processors) geschrieben wurden, wird ein neuer `TelemetryProcessor`, wie im folgenden Beispiel veranschaulicht, durch Anwenden der Erweiterungsmethode `AddApplicationInsightsTelemetryProcessor` auf `IServiceCollection` hinzugefügt. Diese Methode wird in der Methode `ConfigureServices` Ihrer `Startup.cs`-Klasse aufgerufen.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<SuccessfulDependencyFilter>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<AnotherProcessor>();
    }
```

### <a name="example-filters"></a>Beispielfilter

#### <a name="synthetic-requests"></a>Synthetische Anforderungen

Filtern Sie Bots und Webtests heraus. Mit dem Metrik-Explorer haben Sie die Möglichkeit, synthetische Quellen herauszufiltern. Dabei werden jedoch der Datenverkehr und der Umfang der erfassten Daten verringert, da die Quellen im SDK selbst gefiltert werden.

```csharp
public void Process(ITelemetry item)
{
  if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

  // Send everything else:
  this.Next.Process(item);
}
```

#### <a name="failed-authentication"></a>Fehler bei der Authentifizierung

Filtern Sie Abfragen mit der Antwort „401“ heraus.

```csharp
public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, return without calling the next processor.
        return;
    }

    // Send everything else
    this.Next.Process(item);
}
```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Herausfiltern schneller Remoteabhängigkeitsaufrufe

Wenn Sie langsame Aufrufe diagnostizieren möchten, filtern Sie die schnellen Aufrufe heraus.

> [!NOTE]
> Dadurch wird die im Portal angezeigte Statistik verfälscht.
>
>

```csharp
public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}
```

#### <a name="diagnose-dependency-issues"></a>Diagnostizieren von Abhängigkeitsproblemen

[diesem Blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) wird ein Projekt zur Diagnose von Abhängigkeitsproblemen beim automatischen Senden regulärer Pings an Abhängigkeiten beschrieben.

<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a>Hinzufügen von Eigenschaften: ITelemetryInitializer

Verwenden Sie Telemetrieinitialisierer, um Telemetriedaten mit zusätzlichen Informationen anzureichern und/oder die von Standardtelemetriemodulen festgelegten Telemetrieeigenschaften zu überschreiben.

Das Application Insights for Web-Paket erfasst beispielsweise Telemetriedaten zu HTTP-Anforderungen. Standardmäßig kennzeichnet es jede Anforderung mit einem Antwortcode >= 400 als fehlerhaft. Wenn 400 jedoch als erfolgreich behandelt werden soll, können Sie einen Telemetrieinitialisierer angeben, der die Success-Eigenschaft festlegt.

Wenn Sie einen Telemetrieinitialisierer angeben, wird dieser immer aufgerufen, wenn eine der Track*()-Methoden aufgerufen wird. Dies umfasst auch `Track()`-Methoden, die von den Standardtelemetriemodulen aufgerufen werden. Nach Abmachung legen diese Module keine Eigenschaft fest, die bereits durch einen Initialisierer festgelegt wurde. Telemetrieinitialisierer werden vor Telemetrieprozessoren aufgerufen. Daher sind alle von Initialisierern durchgeführten Anreicherungen für Prozessoren sichtbar.

**Definieren des Initialisierers**

*C#*

```csharp
using System;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that overrides the default SDK
   * behavior of treating response codes >= 400 as failed requests
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var requestTelemetry = telemetry as RequestTelemetry;
        // Is this a TrackRequest() ?
        if (requestTelemetry == null) return;
        int code;
        bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
        if (!parsed) return;
        if (code >= 400 && code < 500)
        {
            // If we set the Success property, the SDK won't change it:
            requestTelemetry.Success = true;

            // Allow us to filter these requests in the portal:
            requestTelemetry.Properties["Overridden400s"] = "true";
        }
        // else leave the SDK to set the Success property
    }
  }
}
```

**ASP.NET-Apps: Laden des Initialisierers**

In "ApplicationInsights.config":

```xml
<ApplicationInsights>
  <TelemetryInitializers>
    <!-- Fully qualified type name, assembly name: -->
    <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
    ...
  </TelemetryInitializers>
</ApplicationInsights>
```

*Alternativ* können Sie den Initialisierer im Code instanziieren, z.B. in „Global.aspx.cs“:

```csharp
protected void Application_Start()
{
    // ...
    TelemetryConfiguration.Active.TelemetryInitializers.Add(new MyTelemetryInitializer());
}
```

[Weitere Informationen zu diesem Beispiel anzeigen.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

**ASP.NET Core/Workerdienst-Apps: Laden des Initialisierers**

> [!NOTE]
> Das Hinzufügen eines Initialisierers mithilfe von `ApplicationInsights.config` oder `TelemetryConfiguration.Active` ist für ASP.NET Core-Anwendungen sowie für das SDK „Microsoft.ApplicationInsights.WorkerService“ ungültig.

Wenn Sie bei Apps, die mithilfe von [ASP.NET Core](asp-net-core.md#adding-telemetryinitializers) oder [WorkerService](worker-service.md#adding-telemetryinitializers) geschrieben wurden, einen neuen `TelemetryInitializer` hinzufügen möchten, wird dieser wie unten gezeigt dem Container für die Abhängigkeitsinjektion hinzugefügt. Dies erfolgt in der Methode `Startup.ConfigureServices`.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

### <a name="java-telemetry-initializers"></a>Java-Telemetrieinitialisierer

[Java SDK-Dokumentation](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility.telemetryinitializer?view=azure-java-stable)

```Java
public interface TelemetryInitializer
{ /** Initializes properties of the specified object. * @param telemetry The {@link com.microsoft.applicationinsights.telemetry.Telemetry} to initialize. */

void initialize(Telemetry telemetry); }
```

Registrieren Sie anschließend den benutzerdefinierten Initialisierer in der Datei „applicationinsights.xml“.

```xml
<Add type="mypackage.MyConfigurableContextInitializer">
    <Param name="some_config_property" value="some_value" />
</Add>
```

### <a name="javascript-telemetry-initializers"></a>JavaScript-Telemetrieinitialisierer
*JavaScript*

Fügen Sie einen Telemetrieinitialisierer unmittelbar nach dem Initialisierungscode ein, den Sie vom Portal abgerufen haben:

```JS
<script type="text/javascript">
    // ... initialization code
    ...({
        instrumentationKey: "your instrumentation key"
    });
    window.appInsights = appInsights;


    // Adding telemetry initializer.
    // This is called whenever a new telemetry item
    // is created.

    appInsights.queue.push(function () {
        appInsights.context.addTelemetryInitializer(function (envelope) {
            var telemetryItem = envelope.data.baseData;

            // To check the telemetry items type - for example PageView:
            if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                // this statement removes url from all page view documents
                telemetryItem.url = "URL CENSORED";
            }

            // To set custom properties:
            telemetryItem.properties = telemetryItem.properties || {};
            telemetryItem.properties["globalProperty"] = "boo";

            // To set custom metrics:
            telemetryItem.measurements = telemetryItem.measurements || {};
            telemetryItem.measurements["globalMetric"] = 100;
        });
    });

    // End of inserted code.

    appInsights.trackPageView();
</script>
```

Eine Übersicht der für „telemetryItem“ verfügbaren nicht benutzerdefinierten Eigenschaften finden Sie im [Application Insights-Datenmodell für den Export von Daten](../../azure-monitor/app/export-data-model.md).

Sie können beliebig viele Initialisierer hinzufügen. Diese werden in der Reihenfolge aufgerufen, in der sie hinzugefügt wurden.

### <a name="example-telemetryinitializers"></a>Beispiel: Telemetrieinitialisierer

#### <a name="add-custom-property"></a>Hinzufügen einer benutzerdefinierten Eigenschaft

Mit dem folgenden Beispielinitialisierer wird allen überwachten Telemetriedaten eine benutzerdefinierte Eigenschaft hinzugefügt:

```csharp
public void Initialize(ITelemetry item)
{
  var itemProperties = item as ISupportProperties;
  if(itemProperties != null && !itemProperties.ContainsKey("customProp"))
    {
        itemProperties.Properties["customProp"] = "customValue";
    }
}
```

#### <a name="add-cloud-role-name"></a>Hinzufügen eines Cloudrollennamens

Mit dem folgenden Beispielinitialisierer wird für alle überwachten Telemetriedaten ein Cloudrollenname festgelegt:

```csharp
public void Initialize(ITelemetry telemetry)
{
    if(string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
    {
        telemetry.Context.Cloud.RoleName = "MyCloudRoleName";
    }
}
```

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor und ITelemetryInitializer

Was ist der Unterschied zwischen Telemetrieprozessoren und Telemetrieinitialisierern?

* Bei den Anwendungsmöglichkeiten gibt es einige Überschneidungen: Mit beiden können Sie Eigenschaften von Telemetriedaten hinzufügen oder ändern. Für diesen Zweck wird jedoch die Verwendung von Initialisierern empfohlen.
* Telemetrieinitialisierer werden immer vor Telemetrieprozessoren ausgeführt.
* Telemetrieinitialisierer können mehrmals aufgerufen werden. Laut Konvention legen sie keine Eigenschaften fest, die bereits festgelegt wurden.
* Mit Telemetrieprozessoren können Sie ein Telemetrieelement vollständig ersetzen oder verwerfen.
* Alle registrierten Telemetrieinitialisierer werden für jedes Telemetrieelement garantiert aufgerufen. Bei Telemetrieprozessoren wird mit dem SDK der erste Telemetrieprozessor garantiert aufgerufen. Je nach vorangehenden Telemetrieprozessoren werden die restlichen Prozessoren anschließend aufgerufen oder nicht.
* Mit Telemetrieinitialisierern können Sie Telemetriedaten mit zusätzlichen Eigenschaften anreichern oder die vorhandenen Daten überschreiben. Mit Telemetrieprozessoren können Telemetriedaten herausgefiltert werden.

## <a name="troubleshooting-applicationinsightsconfig"></a>Behandeln von Problemen mit „ApplicationInsights.config“

* Vergewissern Sie sich, dass der vollqualifizierte Typname und der Assemblyname korrekt sind.
* Vergewissern Sie sich, dass sich die Datei „applicationinsights.config“ in Ihrem Ausgabeverzeichnis befindet und auf dem neuesten Stand ist.

## <a name="reference-docs"></a>Referenz

* [API-Übersicht](../../azure-monitor/app/api-custom-events-metrics.md)
* [ASP.NET-Referenz](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a>SDK-Code

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [JavaScript SDK](https://github.com/Microsoft/ApplicationInsights-JS)

## <a name="next"></a>Nächste Schritte
* [Durchsuchen von Ereignissen und Protokollen](../../azure-monitor/app/diagnostic-search.md)
* [Stichproben](../../azure-monitor/app/sampling.md)
* [Problembehandlung](../../azure-monitor/app/troubleshoot-faq.md)
