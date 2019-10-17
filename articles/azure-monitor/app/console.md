---
title: Azure Application Insights für Konsolenanwendungen | Microsoft-Dokumentation
description: Überwachen Sie Webanwendungen auf Verfügbarkeit, Leistung und Auslastung.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/30/2019
ms.reviewer: lmolkova
ms.author: mbullwin
ms.openlocfilehash: 53a765cd2e71b5b1eb1ac2c70506fd55aec6736e
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72274129"
---
# <a name="application-insights-for-net-console-applications"></a>Application Insights für .NET-Konsolenanwendungen

Mit [Application Insights](../../azure-monitor/app/app-insights-overview.md) können Sie Ihre Webanwendung auf Verfügbarkeit, Leistung und Nutzung überwachen.

Sie benötigen ein Abonnement für [Microsoft Azure](https://azure.com). Melden Sie sich mit einem Microsoft-Konto an, das Sie möglicherweise für Windows, Xbox Live oder andere Microsoft-Clouddienste verwenden. Falls Ihr Team über ein Unternehmensabonnement für Azure verfügt, können Sie den Besitzer bitten, Sie über Ihr Microsoft-Konto hinzuzufügen.

> [!NOTE]
> Es gibt ein neues Application Insights Beta-SDK mit Namen [Microsoft.ApplicationInsights.WorkerService](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService), mit dem Application Insights für beliebige Konsolenanwendungen aktiviert werden kann. Es wird empfohlen, dieses Paket und die zugehörigen Anleitungen [hier](../../azure-monitor/app/worker-service.md) zu verwenden. Dieses Paket hat [`NetStandard2.0`](https://docs.microsoft.com/dotnet/standard/net-standard) zum Ziel und kann daher in .NET Core 2.0 oder höher und .NET Framework 4.7.2 oder höher verwendet werden.
Dieses Dokument wird als veraltet markiert, sobald eine stabile Version dieses neuen Pakets veröffentlicht wird.

## <a name="getting-started"></a>Erste Schritte

* Erstellen Sie im [Azure-Portal](https://portal.azure.com) [eine Application Insights-Ressource](../../azure-monitor/app/create-new-resource.md). Wählen Sie für den Typ **Allgemein** aus.
* Erstellen Sie eine Kopie des Instrumentierungsschlüssels. Diesen finden Sie in der Dropdownliste **Essentials** der neuen Ressource, die Sie erstellt haben. 
* Installieren Sie das neueste [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights)-Paket.
* Legen Sie den Instrumentierungsschlüssel im Code fest, bevor Sie Telemetriedaten nachverfolgen (oder legen Sie die Umgebungsvariable APPINSIGHTS_INSTRUMENTATIONKEY fest). Anschließend sollten Sie Telemetriedaten manuell nachverfolgen und im Azure-Portal anzeigen können.

```csharp
// you may use different options to create configuration as shown later in this article
TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();
configuration.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient(configuration);
telemetryClient.TrackTrace("Hello World!");
```

* Installieren Sie die neueste Version des Pakets [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector). Es verfolgt automatisch HTTP-, SQL- und einige andere externe Abhängigkeitsaufrufe.

Sie können Application Insights im Code initialisieren und konfigurieren oder dazu die Datei `ApplicationInsights.config` verwenden. Stellen Sie sicher, dass die Initialisierung so früh wie möglich erfolgt. 

> [!NOTE]
> Anweisungen für **ApplicationInsights.config** gelten nur für Apps, die auf .NET Framework abzielen. Sie gelten nicht für .NET Core-Anwendungen.

### <a name="using-config-file"></a>Verwenden der Konfigurationsdatei
Standardmäßig sucht das Application Insights SDK im Arbeitsverzeichnis nach der Datei `ApplicationInsights.config`, wenn `TelemetryConfiguration` erstellt wird.

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

Sie können den Pfad zu der Konfigurationsdatei aber auch angeben.

```csharp
using System.IO;
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration(File.ReadAllText("C:\\ApplicationInsights.config"));
var telemetryClient = new TelemetryClient(configuration);
```

Weitere Informationen finden Sie in der [Referenz zu Konfigurationsdateien](configuration-with-applicationinsights-config.md).

Ein vollständiges Beispiel der Konfigurationsdatei erhalten Sie, wenn Sie die neueste Version des Pakets [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) installieren. Dies ist die **minimale** Konfiguration für die Abhängigkeitssammlung, die dem Codebeispiel entspricht.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Konfigurieren der Telemetrieerfassung im Code
> [!NOTE]
> Das Lesen von Konfigurationsdateien wird unter .NET Core nicht unterstützt. Unter Umständen ist es ratsam, das [Application Insights-SDK für ASP.NET Core](../../azure-monitor/app/asp-net-core.md) zu verwenden.

* Erstellen und konfigurieren Sie während des Starts der Anwendung die `DependencyTrackingTelemetryModule`-Instanz – sie muss ein Singleton sein und für die Lebensdauer der Anwendung beibehalten werden.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Hinzufügen häufiger Telemetrieinitialisierer

```csharp
// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

Wenn Sie die Konfiguration mit dem einfachen `TelemetryConfiguration()`-Konstruktor erstellt haben, müssen Sie außerdem Korrelationsunterstützung aktivieren. **Dies ist nicht erforderlich,** wenn Sie die Konfiguration aus einer Datei lesen oder `TelemetryConfiguration.CreateDefault()` oder `TelemetryConfiguration.Active` verwendet haben.

```csharp
configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
```

* Sie können auch das Leistungsindikator-Sammlermodul installieren und initialisieren. Eine Beschreibung finden Sie [hier](https://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/).


#### <a name="full-example"></a>Vollständiges Beispiel

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            TelemetryConfiguration configuration = TelemetryConfiguration.CreateDefault();

            configuration.InstrumentationKey = "removed";
            configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

            var telemetryClient = new TelemetryClient(configuration);
            using (InitializeDependencyTracking(configuration))
            {
                // run app...

                telemetryClient.TrackTrace("Hello World!");

                using (var httpClient = new HttpClient())
                {
                    // Http dependency is automatically tracked!
                    httpClient.GetAsync("https://microsoft.com").Wait();
                }

            }

            // before exit, flush the remaining data
            telemetryClient.Flush();

            // flush is not blocking so wait a bit
            Task.Delay(5000).Wait();

        }

        static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
        {
            var module = new DependencyTrackingTelemetryModule();

            // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

            // enable known dependency tracking, note that in future versions, we will extend this list. 
            // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

            // initialize the module
            module.Initialize(configuration);

            return module;
        }
    }
}

```

## <a name="next-steps"></a>Nächste Schritte
* [Überwachen Sie Abhängigkeiten](../../azure-monitor/app/asp-net-dependencies.md), um zu überprüfen, ob REST-, SQL- oder andere externe Ressourcen die Leistung beeinträchtigen.
* [Verwenden Sie die API](../../azure-monitor/app/api-custom-events-metrics.md) , um Ihre eigenen Ereignisse und Metriken für eine detailliertere Ansicht der Leistung und Nutzung Ihrer App zu senden.
