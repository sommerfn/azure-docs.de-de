---
title: Azure Application Insights für ASP.NET Core-Anwendungen | Microsoft-Dokumentation
description: Überwachen Sie ASP.NET Core-Webanwendungen auf Verfügbarkeit, Leistung und Auslastung.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/22/2019
ms.openlocfilehash: 5b9b92cd39e8d540f784d82d6c7f4a5754c85b62
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677724"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights für ASP.NET Core-Anwendungen

In diesem Artikel wird beschrieben, wie Sie Application Insights für eine [ASP.NET Core](https://docs.microsoft.com/aspnet/core)-Anwendung aktivieren. Nach Abschluss der Schritte in diesem Artikel erfasst Application Insights Anforderungen, Abhängigkeiten, Ausnahmen, Leistungsindikatoren, Heartbeats und Protokolle für Ihre ASP.NET Core-Anwendung.

Sie verwenden im folgenden Beispiel eine [MVC-Anwendung](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) für `netcoreapp2.2`. Die Anleitung in diesem Artikel können Sie allerdings für alle ASP.NET Core-Anwendungen nutzen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Mit dem [Application Insights SDK für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) können Sie Anwendungen unabhängig davon überwachen, wo und wie sie ausgeführt werden. Wenn Ihre Anwendung ausgeführt wird und über eine Netzwerkverbindung mit Azure verfügt, können Telemetriedaten erfasst werden. Die Application Insights-Überwachung wird in allen Umgebungen unterstützt, in denen auch .NET Core unterstützt wird. Dazu zählen:
* **Betriebssystem**: Windows, Linux oder Mac.
* **Hostingmethode:** In-Process oder Out-of-Process.
* **Bereitstellungsmethode:** Abhängig vom Framework oder eigenständig.
* **Webserver:** IIS (Internet Information Server) oder Kestrel.
* **Hostingplattform:** Beispielsweise das Web-Apps-Feature von Azure App Service, Azure Virtual Machines, Docker oder Azure Kubernetes Service (AKS).
* **.NET Core-Runtimeversion:** 1.XX, 2.XX oder 3.XX
* **IDE:** Visual Studio, VS Code oder Befehlszeile.

> [!NOTE]
> Wenn Sie ASP.NET Core 3.0 zusammen mit Application Insights nutzen, verwenden Sie Version [2.8.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.8.0) oder höher. Dies ist die einzige Version, die ASP.NET Core 3.0 unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Eine funktionierende ASP.NET Core-Anwendung. Wenn Sie eine ASP.NET Core-Anwendung erstellen müssen, führen Sie die Schritte im entsprechenden [ASP.NET Core-Tutorial](https://docs.microsoft.com/aspnet/core/getting-started/) aus.
- Ein gültiger Application Insights-Instrumentierungsschlüssel. Dieser ist erforderlich, um Telemetriedaten an Application Insights zu senden. Wenn Sie eine neue Application Insights-Ressource erstellen müssen, um einen Instrumentierungsschlüssel abzurufen, finden Sie unter [Erstellen einer Application Insights-Ressource](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) weitere Informationen.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Aktivieren der serverseitigen Telemetrie für Application Insights (Visual Studio)

1. Öffnen Sie Ihr Projekt in Visual Studio.

    > [!TIP]
    > Sie können bei Bedarf die Quellcodeverwaltung für Ihr Projekt einrichten, sodass alle Änderungen erfasst werden, die Application Insights vornimmt. Klicken Sie auf **Datei** > **Zur Quellcodeverwaltung hinzufügen**, um die Quellcodeverwaltung zu aktivieren.

2. Wählen Sie **Projekt** > **Application Insights-Telemetrie hinzufügen** aus.

3. Wählen Sie **Erste Schritte** aus. Je nach verwendeter Version von Visual Studio kann der Text des Steuerelements abweichen. Bei einigen älteren Versionen wird stattdessen die Schaltfläche **Kostenlos starten** angezeigt.

4. Wählen Sie Ihr Abonnement aus. Klicken Sie dann auf **Ressource** > **Registrieren**.

5. Überprüfen Sie nach dem Hinzufügen von Application Insights zu Ihrem Projekt, ob Sie das neueste stabile Release des SDK verwenden. Wechseln Sie zu **Projekt** > **NuGet-Pakete verwalten** > **Microsoft.ApplicationInsights.AspNetCore**. Klicken Sie bei Bedarf auf **Aktualisieren**.

     ![Screenshot: Auswahl des Application Insights-Pakets, das aktualisiert werden soll](./media/asp-net-core/update-nuget-package.png)

6. Wenn Sie den optionalen Tipp umgesetzt und für Ihr Projekt die Quellcodeverwaltung aktiviert haben, wechseln Sie zu **Ansicht** > **Team Explorer** > **Änderungen**. Klicken Sie dann auf eine Datei, um sich eine Vergleichsansicht für Änderungen anzeigen zu lassen, die vom Application Insights-Telemetriefeature vorgenommen wurden.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Aktivieren der serverseitigen Telemetrie für Application Insights (ohne Visual Studio)

1. Installieren Sie das [Application Insights SDK-NuGet-Paket für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Sie sollten immer die neueste stabile Version verwenden. Vollständige Versionshinweise für das SDK finden Sie im [Open-Source-GitHub-Repository](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    Im folgenden Beispielcode wird gezeigt, welche Elemente Sie der `.csproj`-Datei Ihres Projekts hinzufügen müssen.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.8.0" />
        </ItemGroup>
    ```

2. Fügen Sie wie im folgenden Beispiel dargestellt `services.AddApplicationInsightsTelemetry();` der `ConfigureServices()`-Methode in Ihrer `Startup`-Klasse hinzu:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Richten Sie den Instrumentierungsschlüssel ein.

    Sie können einen Instrumentierungsschlüssel als Argument für `AddApplicationInsightsTelemetry` bereitstellen. Empfohlen wird aber, den Instrumentierungsschlüssel in der Konfiguration anzugeben. Im folgenden Beispielcode wird veranschaulicht, wie Sie in `appsettings.json` einen Instrumentierungsschlüssel angeben. Stellen Sie sicher, dass `appsettings.json` während der Veröffentlichung in den Stammordner der Anwendung kopiert wird.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Alternativ können Sie den Instrumentierungsschlüssel auch in einer der folgenden Umgebungsvariablen angeben:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Beispiel:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    In der Regel wird mit `APPINSIGHTS_INSTRUMENTATIONKEY` der Instrumentierungsschlüssel für Anwendungen angegeben, die in Azure-Web-Apps bereitgestellt werden.

    > [!NOTE]
    > Ein Instrumentierungsschlüssel, der im Code angegeben ist, erhält Vorrang vor der Umgebungsvariable `APPINSIGHTS_INSTRUMENTATIONKEY`, die wiederum Vorrang vor anderen Optionen hat.

## <a name="run-your-application"></a>Ausführen der Anwendung

Führen Sie Ihre Anwendung aus, und senden Sie Anforderungen an diese. Nun sollten Telemetriedaten an Application Insights übermittelt werden. Mit dem Application Insights SDK werden eingehende Webanforderungen an die Anwendung sowie die folgenden Telemetriedaten automatisch erfasst.

### <a name="live-metrics"></a>Livemetriken

Mit [Livemetriken](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) kann schnell überprüft werden, ob die Application Insights-Überwachung ordnungsgemäß konfiguriert ist. Es kann einige Minuten dauern, bis die Telemetriedaten im Portal und in der Analyse angezeigt werden. In Livemetriken wird die CPU-Auslastung des laufenden Prozesses dagegen nahezu in Echtzeit angezeigt. Außerdem können andere Telemetriedaten wie z. B. Anforderungen, Abhängigkeiten oder Ablaufverfolgungen angezeigt werden.

### <a name="ilogger-logs"></a>ILogger-Protokolle

Protokolle, die über `ILogger` mit dem Schweregrad `Warning` oder höher ausgegeben werden, werden automatisch erfasst. Informationen zur Anpassung der in Application Insights erfassten Protokollebenen finden Sie in der [Dokumentation zu ILogger](ilogger.md#control-logging-level).

### <a name="dependencies"></a>Abhängigkeiten

Die Abhängigkeitssammlung ist standardmäßig aktiviert. In [diesem Artikel](asp-net-dependencies.md#automatically-tracked-dependencies) werden die Abhängigkeiten, die automatisch gesammelt werden, sowie die Schritte zur manuellen Nachverfolgung erläutert.

### <a name="performance-counters"></a>Leistungsindikatoren

Für die Unterstützung von [Leistungsindikatoren](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) in ASP.NET Core gelten die folgenden Einschränkungen:

* Die SDK-Versionen 2.4.1 und höher erfassen Leistungsindikatoren, wenn die Anwendung in Azure-Web-Apps (Windows) ausgeführt wird.
* Die SDK-Versionen 2.7.1 und höher erfassen Leistungsindikatoren, wenn die Anwendung unter Windows läuft und `NETSTANDARD2.0` oder höher als Zielframework verwendet wird.
* Für Anwendungen, die für das .NET Framework bestimmt sind, werden Leistungsindikatoren in allen Versionen des SDK unterstützt.
* Die SDK-Versionen 2.8.0 und höher unterstützen Leistungsindikatoren für CPU und Arbeitsspeicher unter Linux. Es werden kein weiteren Leistungsindikatoren unter Linux unterstützt. Die empfohlene Vorgehensweise für Systemleistungsindikatoren unter Linux (und in anderen Nicht-Windows-Umgebungen) ist die Verwendung von [EventCounters](#eventcounter).

### <a name="eventcounter"></a>EventCounter

`EventCounterCollectionModule` ist standardmäßig aktiviert und sammelt einen Standardsatz von Leistungsindikatoren von .NET Core 3.0-Apps. Im Tutorial zu [EventCounter](eventcounters.md) ist der Standardsatz der gesammelten Leistungsindikatoren aufgeführt. Außerdem enthält es Anweisungen zum Anpassen der Liste.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Aktivieren der clientseitigen Telemetrie für Webanwendungen

Wenn Sie die vorherigen Schritte ausgeführt haben, können Sie serverseitige Telemetriedaten erfassen. Wenn Ihre Anwendung über clientseitige Komponenten verfügt, sollten Sie die unten angegebenen Schritte ausführen, um mit dem Erfassen von [Nutzungstelemetriedaten](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) zu beginnen.

1. Nehmen Sie in `_ViewImports.cshtml` eine Einfügung vor:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. Fügen Sie in `_Layout.cshtml` am Ende des `<head>`-Abschnitts `HtmlHelper` vor allen anderen Skripts ein. Wenn Sie benutzerdefinierte JavaScript-Telemetriedaten für die Seite übermitteln möchten, müssen Sie den Code dafür nach diesem Ausschnitt einfügen:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

Die `.cshtml`-Dateinamen, auf die vorher verwiesen wurde, stammen aus der Standardvorlage für MVC-Anwendungen. Wenn Sie die clientseitige Überwachung für Ihre Anwendung ordnungsgemäß aktivieren möchten, muss der JavaScript-Codeausschnitt im `<head>`-Abschnitt jeder Anwendungsseite vorhanden sein, die Sie überwachen möchten. Für diese Anwendungsvorlage müssen Sie dazu den JavaScript-Codeausschnitt `_Layout.cshtml` hinzufügen. 

Sie können für Ihr Projekt die [clientseitige Überwachung](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring) auch dann aktivieren, wenn dieses nicht `_Layout.cshtml` enthält. Fügen Sie hierzu den JavaScript-Codeausschnitt einer entsprechenden Datei hinzu, die den `<head>`-Abschnitt aller Seiten Ihrer App steuert. Sie können den Codeausschnitt auch mehreren Seiten hinzufügen. Diese Lösung lässt sich allerdings auf Dauer nur schwierig umsetzen und wird daher nicht empfohlen.

## <a name="configure-the-application-insights-sdk"></a>Konfigurieren des Application Insights SDK

Sie können die Standardkonfiguration des Application Insights SDK für ASP.NET Core anpassen. Benutzer des ASP.NET SDK für Application Insights können die Konfiguration über `ApplicationInsights.config` oder durch das Ändern von `TelemetryConfiguration.Active` anpassen. Wenn Sie die Konfiguration für ASP.NET Core anpassen möchten, müssen Sie anders vorgehen. Fügen Sie das ASP.NET Core SDK der Anwendung hinzu, und konfigurieren Sie es mithilfe des integrierten [Dependency-Injection-Systems](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) von ASP.NET Core. Falls nicht anders angegeben, sollten Sie alle Konfigurationsänderungen in der `ConfigureServices()`-Methode Ihrer `Startup.cs`-Klasse vornehmen. Weitere Informationen finden Sie in den folgenden Abschnitten.

> [!NOTE]
> In ASP.NET Core-Anwendungen wird die Konfiguration durch Änderung von `TelemetryConfiguration.Active` nicht unterstützt.

### <a name="using-applicationinsightsserviceoptions"></a>Verwenden von ApplicationInsightsServiceOptions

Sie können wie im folgenden Beispiel einige allgemeine Einstellungen ändern, indem Sie der `AddApplicationInsightsTelemetry`-Methode `ApplicationInsightsServiceOptions` übergeben:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    // Disables adaptive sampling.
    aiOptions.EnableAdaptiveSampling = false;

    // Disables QuickPulse (Live Metrics stream).
    aiOptions.EnableQuickPulseMetricStream = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
}
```

Vollständige Liste der Einstellungen in `ApplicationInsightsServiceOptions`

|Einstellung | BESCHREIBUNG | Standard
|---------------|-------|-------
|EnableQuickPulseMetricStream | „LiveMetrics“-Feature aktivieren/deaktivieren | true
|EnableAdaptiveSampling | Adaptive Stichprobenerstellung aktivieren/deaktivieren | true
|EnableHeartbeat | Heartbeats-Feature aktivieren/deaktivieren, das in regelmäßigen Abständen (Standardwert: 15 Minuten) eine benutzerdefinierte Metrik namens „HeartBeatState“ mit Informationen zur Laufzeit wie .NET-Version, ggf. Informationen zur Azure-Umgebung usw. sendet. | true
|AddAutoCollectedMetricExtractor | Extraktor für „AutoCollectedMetrics“ aktivieren/deaktivieren, bei dem es sich um einen Telemetrieprozessor handelt, der vorab aggregierte Metriken zu Anforderungen/Abhängigkeiten sendet, bevor die Stichprobenerstellung stattfindet. | true
|RequestCollectionOptions.TrackExceptions | Berichterstellung über die Nachverfolgung von Ausnahmefehlern durch das Anforderungserfassungsmodul aktivieren/deaktivieren. | In NETSTANDARD2.0 „false“ (da Ausnahmen mit „ApplicationInsightsLoggerProvider“ nachverfolgt werden), andernfalls „true“.

Die aktuellste Liste finden Sie unter den [konfigurierbaren Einstellungen in `ApplicationInsightsServiceOptions`](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Stichproben

Das Application Insights SDK für ASP.NET Core unterstützt sowohl die feste als auch die adaptive Stichprobenerstellung. Die adaptive Stichprobenerstellung ist standardmäßig aktiviert. 

Weitere Informationen finden Sie unter [Konfigurieren der adaptiven Stichprobenerstellung für ASP.NET Core-Anwendungen](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Hinzufügen von TelemetryInitializers

Mit [Telemetrieinitialisierern](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) legen Sie die globalen Eigenschaften fest, die zusammen mit allen Telemetriedaten gesendet werden.

Fügen Sie wie im folgenden Code gezeigt dem `DependencyInjection`-Container einen neuen `TelemetryInitializer` hinzu. Das SDK erfasst automatisch alle `TelemetryInitializer`, die dem `DependencyInjection`-Container hinzugefügt werden.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
}
```

### <a name="removing-telemetryinitializers"></a>Entfernen von TelemetryInitializer-Elementen

Telemetrieinitialisierer sind standardmäßig vorhanden. Wenn Sie alle oder nur bestimmte Telemetrieinitialisierer entfernen möchten, können Sie den folgenden Beispielcode *nach* dem Aufrufen von `AddApplicationInsightsTelemetry()` verwenden.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // Remove a specific built-in telemetry initializer
    var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                        (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
    if (tiToRemove != null)
    {
        services.Remove(tiToRemove);
    }

    // Remove all initializers
    // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
    services.RemoveAll(typeof(ITelemetryInitializer));
}
```

### <a name="adding-telemetry-processors"></a>Hinzufügen von Telemetrieprozessoren

Sie können `TelemetryConfiguration` benutzerdefinierte Telemetrieprozessoren hinzufügen, indem Sie die Erweiterungsmethode `AddApplicationInsightsTelemetryProcessor` in `IServiceCollection` verwenden. Telemetrieprozessoren werden in [komplexen Filterszenarien](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) verwendet. Verwenden Sie das folgende Beispiel.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddApplicationInsightsTelemetry();
    services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

    // If you have more processors:
    services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
}
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Konfigurieren oder Entfernen von TelemetryModule-Standardelementen

Application Insights verwendet Telemetriemodule, um automatisch nützliche Telemetriedaten zu bestimmten Workloads zu erfassen, ohne dass eine manuelle Nachverfolgung durch den Benutzer erforderlich ist.

Die unten aufgeführten Module sind standardmäßig aktiviert. Diese sind verantwortlich für die automatische Erfassung von Telemetriedaten. Sie können sie deaktivieren oder ihr Standardverhalten anpassen.

* `RequestTrackingTelemetryModule`: Erfasst Anforderungstelemetriedaten (RequestTelemetry) von eingehenden Webanforderungen.
* `DependencyTrackingTelemetryModule`: Erfasst Abhängigkeitstelemetriedaten (DependencyTelemetry) von ausgehenden HTTP-Aufrufen und SQL-Aufrufen.
* `PerformanceCollectorModule`: Erfasst Windows-Leistungsindikatoren (PerformanceCounters).
* `QuickPulseTelemetryModule`: Erfasst Telemetriedaten zur Anzeige im Live Metrics-Portal.
* `AppServicesHeartbeatTelemetryModule`: Erfasst Heartbeats (die als benutzerdefinierte Metriken gesendet werden) über die Azure App Service-Umgebung, in der die Anwendung gehostet wird.
* `AzureInstanceMetadataTelemetryModule`: Erfasst Heartbeats (die als benutzerdefinierte Metriken gesendet werden) über die Azure-VM-Umgebung, in der die Anwendung gehostet wird.
* `EventCounterCollectionModule`: Erfasst [EventCounters](eventcounters.md). Dieses Modul ist ein neues Feature, das ab SDK-Version 2.8.0 verfügbar ist.

Verwenden Sie zum Konfigurieren von `TelemetryModule`-Standardelementen die Erweiterungsmethode `ConfigureTelemetryModule<T>` in `IServiceCollection`. Dies ist im Beispiel unten dargestellt.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // The following configures DependencyTrackingTelemetryModule.
    // Similarly, any other default modules can be configured.
    services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
            {
                module.EnableW3CHeadersInjection = true;
            });

    // The following removes all default counters from EventCounterCollectionModule, and adds a single one.
    services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                module.Counters.Clear();
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

    // The following removes PerformanceCollectorModule to disable perf-counter collection.
    // Similarly, any other default modules can be removed.
    var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
    if (performanceCounterService != null)
    {
        services.Remove(performanceCounterService);
    }
}
```

### <a name="configuring-a-telemetry-channel"></a>Konfigurieren eines Telemetriekanals

Der Standardkanal ist `ServerTelemetryChannel`. Sie können diesen wie im folgenden Beispiel dargestellt überschreiben.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Dynamisches Deaktivieren von Telemetrie

Wenn Sie Telemetriedaten bedingt und dynamisch deaktivieren möchten, können Sie eine `TelemetryConfiguration`-Instanz mit einem ASP.NET Core-Dependency-Injection-Container an einer beliebigen Stelle im Code auflösen und ein `DisableTelemetry`-Flag dafür festlegen.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

Durch den oben genannten Vorgang wird nicht verhindert, dass Telemetriedaten von Modulen für die automatische Erfassung gesammelt werden. Nur das Senden von Telemetriedaten an Application Insights wird mit der oben beschriebenen Methode deaktiviert. Wenn ein bestimmtes Modul für die automatische Sammlung nicht gewünscht wird, ist es am besten, [das Telemetriemodul zu entfernen](#configuring-or-removing-default-telemetrymodules).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="does-application-insights-support-aspnet-core-30"></a>Wird ASP.NET Core 3.0 in Application Insights unterstützt?

Ja. Führen Sie ein Update auf [Application Insights SDK für ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) Version 2.8.0 oder höher durch. In älteren Versionen des SDK wird ASP.NET Core 3.0 nicht unterstützt.

Wenn Sie die [hier](#enable-application-insights-server-side-telemetry-visual-studio) beschriebenen Anweisungen zu Visual Studio befolgen, führen Sie ein Update auf die neueste Version von Visual Studio 2019 (16.3.0) durch, um das Onboarding durchzuführen. In früheren Versionen von Visual Studio wird das automatische Onboarding für ASP.NET Core 3.0-Apps nicht unterstützt.

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Wie kann ich Telemetriedaten nachverfolgen, die nicht automatisch erfasst werden?

Rufen Sie eine Instanz von `TelemetryClient` ab, indem Sie die Abhängigkeit über den Konstruktor übergeben (Constructor Injection) und die erforderliche `TrackXXX()`-Methode aufrufen. Nicht empfohlen wird das Erstellen neuer `TelemetryClient`-Instanzen in einer ASP.NET Core-Anwendung. Eine Singletoninstanz von `TelemetryClient` ist bereits im `DependencyInjection`-Container registriert, der `TelemetryConfiguration` für alle sonstigen Telemetriedaten verwendet. Sie sollten nur dann eine neue `TelemetryClient`-Instanz erstellen, wenn für diese eine Konfiguration erforderlich ist, die sich von der für die sonstigen Telemetriedaten unterscheidet.

Im folgenden Beispiel wird veranschaulicht, wie Sie zusätzliche Telemetrie über einen Controller nachverfolgen.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Wie Sie Daten in Application Insights benutzerdefiniert erfassen können, erfahren Sie unter [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Für einige Visual Studio-Vorlagen wurde die Erweiterungsmethode „UseApplicationInsights()“ in IWebHostBuilder verwendet, um Application Insights zu aktivieren. Ist diese Verwendung noch gültig?

Obwohl die Erweiterungsmethode `UseApplicationInsights()` weiterhin unterstützt wird, ist sie ab Application Insights SDK Version 2.8.0 als veraltet markiert. Sie wird in der nächsten Hauptversion des SDK entfernt. Die empfohlene Vorgehensweise zur Aktivierung von Application Insights-Telemetriedaten ist die Verwendung von `AddApplicationInsightsTelemetry()`, da so Überladungen zum Steuern einiger Konfigurationen möglich sind. In ASP.NET Core 3.0-Apps ist `services.AddApplicationInsightsTelemetry()` außerdem die einzige Möglichkeit zum Aktivieren von Application Insights.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ich stelle meine ASP.NET Core-Anwendung in Web-Apps bereit. Sollte ich trotzdem die Application Insights-Erweiterung über Web-Apps aktivieren?

Wenn das SDK wie in diesem Artikel gezeigt zur Buildzeit installiert wird, ist es nicht erforderlich, die [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps)-Erweiterung über das App Service-Portal zu aktivieren. Auch bei installierter Erweiterung werden keine Schritte ausgeführt, wenn erkannt wird, dass das SDK der Anwendung bereits hinzugefügt wurde. Wenn Sie Application Insights über die Erweiterung aktivieren, müssen Sie das SDK nicht installieren und aktualisieren. Wenn Sie Application Insights jedoch mithilfe der Anweisungen in diesem Artikel aktivieren, sind Sie aus den folgenden Gründen flexibler:

   * Die Application Insights-Telemetriefunktion funktioniert weiterhin
       * auf allen Betriebssystemen einschließlich Windows, Linux und Mac.
       * mit allen Veröffentlichungsmodi einschließlich dem eigenständigen oder frameworkabhängigen Modus.
       * allen Zielframeworks, z. B. vollständigem .NET Framework.
       * mit allen Hostserveroptionen einschließlich Web-Apps, VMs, Linux, Containern, Azure Kubernetes Service und anderer Hostinglösungen als Azure.
       * mit allen .NET Core-Versionen, einschließlich Vorschauversionen.
   * Sie können Telemetriedaten lokal beim Debuggen in Visual Studio anzeigen.
   * Sie können mit der `TrackXXX()`-API zusätzliche benutzerdefinierte Telemetriedaten nachverfolgen.
   * Die vollständige Steuerung der Konfiguration ist Ihnen überlassen.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Kann ich die Application Insights-Überwachung mit Tools wie dem Statusmonitor aktivieren?

Nein. Der [Statusmonitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) und der [Statusmonitor v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) unterstützen aktuell ausschließlich ASP.NET 4.x.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Ist Application Insights für meine ASP.NET Core 2.0-Anwendung automatisch aktiviert?

Im `Microsoft.AspNetCore.All` 2.0-Metapaket ist das Application Insights SDK (Version 2.1.0) enthalten. Wenn Sie die Anwendung im Visual Studio-Debugger ausführen, wird Application Insights von Visual Studio aktiviert, und die Telemetriedaten werden lokal in der IDE angezeigt. Telemetriedaten werden nicht an Application Insights gesendet, falls nicht explizit ein Instrumentierungsschlüssel angegeben wird. Wir empfehlen Ihnen, die Anleitungen in diesem Artikel zu befolgen, um Application Insights zu aktivieren (auch für 2.0-Apps).

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Werden alle Features unterstützt, wenn ich meine Anwendung unter Linux ausführe?

Ja. Die Featureunterstützung für das SDK ist auf allen Plattformen gleich. Es gelten lediglich die folgenden Ausnahmen:

* Leistungsindikatoren werden nur unter Windows unterstützt.
* Auch wenn `ServerTelemetryChannel` standardmäßig aktiviert ist, wird über den Kanal bei der Anwendungsausführung unter Linux oder macOS nicht automatisch ein lokaler Speicherordner erstellt, um die Telemetriedaten bei Netzwerkproblemen vorübergehend zu speichern. Aufgrund dieser Einschränkung gehen Telemetriedaten verloren, wenn vorübergehende Netzwerk- oder Serverprobleme auftreten. Sie können das Problem umgehen, indem Sie einen lokalen Ordner für den Kanal konfigurieren:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="is-this-sdk-supported-for-the-new-net-core-30-worker-service-template-applications"></a>Wird dieses SDK für die neuen .NET Core 3.0 Worker Service-Vorlagenanwendungen unterstützt?

Dieses SDK erfordert `HttpContext` und kann daher nicht in Nicht-HTTP-Anwendungen ausgeführt werden, dazu gehören z. B. auch .NET Core 3.0 Worker Service-Anwendungen. Informationen zum Aktivieren von Application Insights in diesen Anwendungen unter Verwendung des neu veröffentlichten SDK Microsoft.ApplicationInsights.WorkerService finden Sie [hier](worker-service.md).

## <a name="open-source-sdk"></a>Open Source SDK

[Lesen und Hinzufügen von Code](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- In diesem externen Video finden Sie ausführliche Informationen zur [grundlegenden Konfiguration von Application Insights mit .NET Core und Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t).
- In diesem externen Video finden Sie ausführliche Informationen zur [grundlegenden Konfiguration von Application Insights mit .NET Core und Visual Studio Code](https://youtu.be/ygGt84GDync).

## <a name="next-steps"></a>Nächste Schritte

* [Machen Sie sich Benutzerflows vertraut](../../azure-monitor/app/usage-flows.md), um die Benutzernavigation in Ihrer App nachzuvollziehen.
* [Konfigurieren Sie eine Momentaufnahmensammlung](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger), um den Status des Quellcodes und der Variablen zu dem Zeitpunkt anzuzeigen, an dem eine Ausnahme ausgelöst wurde.
* [Verwenden Sie die API](../../azure-monitor/app/api-custom-events-metrics.md), um Ihre eigenen Ereignisse und Metriken für eine detaillierte Ansicht der Leistung und Nutzung Ihrer App zu senden.
* Überprüfen Sie Ihre App mit [Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md) fortwährend von jedem Ort der Welt aus.
* [Dependency Injection in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)
