---
title: Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions
description: In diesem Dokument wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder einen Verfügbarkeitstest erstellen, falls er in Ihrer Region nicht verfügbar ist.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: morgangrobin
ms.author: mogrobin
ms.date: 10/11/2019
ms.openlocfilehash: 900228e1f9bdf9d367fa37b9ec90a6148faec656
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72880249"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>Erstellen und Ausführen von benutzerdefinierten Verfügbarkeitstests mit Azure Functions

In diesem Artikel wird beschrieben, wie Sie eine Azure-Funktion mit „TrackAvailability()“ erstellen, die gemäß der Konfiguration der Funktion „TimerTrigger“ regelmäßig ausgeführt wird. Die Ergebnisse dieses Tests werden an Ihre Application Insights-Ressource gesendet. Dort können Sie für die Ergebnisdaten zur Verfügbarkeit Abfragen durchführen und Warnungen einrichten. So können Sie angepasste Tests auf ähnliche Weise wie per [Verfügbarkeitsüberwachung](../../azure-monitor/app/monitor-web-app-availability.md) im Portal erstellen. Mit angepassten Tests können Sie komplexere Verfügbarkeitstests als bei Verwendung der Portalbenutzeroberfläche schreiben, eine App in Ihrem virtuellen Azure-Netzwerk (VNET) überwachen, die Endpunktadresse ändern oder auch dann einen Verfügbarkeitstest erstellen, falls dieses Feature in Ihrer Region nicht verfügbar ist.


## <a name="create-timer-triggered-function"></a>Erstellen einer Funktion mit Auslösung per Timer

- Bei Verwendung einer Application Insights-Ressource:
    - Standardmäßig wird von Azure Functions eine Application Insights-Ressource erstellt, aber falls Sie eine Ihrer bereits erstellten Ressourcen verwenden möchten, müssen Sie dies bei der Erstellung angeben.
    - Befolgen Sie die Anleitung zum [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (Stopp vor Bereinigung) mit den folgenden Auswahlmöglichkeiten.
        -  Klicken Sie auf den Application Insights-Abschnitt, bevor Sie **Erstellen** auswählen.

            ![ Erstellen einer Azure Functions-App mit Ihrer eigenen App Insights-Ressource](media/availability-azure-functions/create-function-app.png)

        - Klicken Sie auf **Vorhandene Ressource auswählen**, und geben Sie den Namen Ihrer Ressource ein. Wählen Sie **Übernehmen** aus.

            ![Auswählen einer vorhandenen Application Insights-Ressource](media/availability-azure-functions/app-insights-resource.png)

        - Klicken Sie auf **Erstellen**
- Falls Sie noch keine Application Insights-Ressource für die vom Timer ausgelöste Funktion erstellt haben:
    - Wenn Sie Ihre Azure Functions-Anwendung erstellen, wird standardmäßig eine Application Insights-Ressource für Sie erstellt.
    - Befolgen Sie die Anleitung zum [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (Stopp vor Bereinigung).

## <a name="sample-code"></a>Beispielcode

Kopieren Sie den unten angegebenen Code in die Datei „run.csx“ (ersetzt den vorhandenen Code). Navigieren Sie hierzu zu Ihrer Azure Functions-Anwendung, und wählen Sie links Ihre Funktion mit Auslösung per Timer aus.

![Datei „run.csx“ der Azure-Funktion im Azure-Portal](media/availability-azure-functions/runcsx.png)

> [!NOTE]
> Als Endpunktadresse verwenden Sie Folgendes: `EndpointAddress= https://dc.services.visualstudio.com/v2/track`. Falls sich Ihre Ressource in einer Region wie „Azure Government“ oder „Azure China“ befindet, sollten Sie sich den Artikel zum Thema [Außerkraftsetzen der Standardendpunkte](https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification) durchlesen und den entsprechenden Telemetriekanal-Endpunkt für Ihre Region auswählen.

```C#
using System;
using System.Diagnostics;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

// [CONFIGURATION_REQUIRED] configure test timeout accordingly for which your request should run
private static readonly HttpClient HttpClient = new HttpClient { Timeout = TimeSpan.FromSeconds(30) };

// The Application Insights Instrumentation Key can be changed by going to the overview page of your Function App, selecting configuration, and changing the value of the APPINSIGHTS_INSTRUMENTATIONKEY Application setting.
//DO NOT replace the code below with your instrumentation key, the key's value is pulled from the environment variable/application setting key/value pair.
private static readonly string InstrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY");

// [CONFIGURATION_REQUIRED] Configure EndpointAddress
private static readonly TelemetryConfiguration TelemetryConfiguration = new TelemetryConfiguration(InstrumentationKey, new ServerTelemetryChannel() { EndpointAddress = "<EndpointAddress>" });
private static readonly TelemetryClient TelemetryClient = new TelemetryClient(TelemetryConfiguration);

[FunctionName("Function")]
public static async void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Entering Run at: {DateTime.Now}");

    if (myTimer.IsPastDue)
    {
        log.LogWarning($"[Warning]: Timer is running late! Last ran at: {myTimer.ScheduleStatus.Last}");
    }

    // [CONFIGURATION_REQUIRED] provide {testName} accordingly for your test function
    string testName = "AvailabilityTestFunction";

    // REGION_NAME is a default environment variable that comes with App Service
    string location = Environment.GetEnvironmentVariable("REGION_NAME");

    // [CONFIGURATION_REQUIRED] configure {uri} and {contentMatch} accordingly for your web app. {uri} is the website that you are testing the availability of, make sure to include http:// ot https:// in your url. If {contentMatch} is present on the page, the test will succeed, otherwise it will fail.  
    await AvailabilityTestRun(
        name: testName,
        location: location,
        uri: "<http://example.com>",
        contentMatch: "<Enter a short string of text that is present  in the body of the page your are testing>",
        log: log
    );
}

private static async Task AvailabilityTestRun(string name, string location, string uri, string contentMatch, ILogger log)
{
    log.LogInformation($"Executing availability test run for {name} at: {DateTime.Now}");

    string operationId = Guid.NewGuid().ToString("N");

    var availability = new AvailabilityTelemetry
    {
        Id = operationId,
        Name = name,
        RunLocation = location,
        Success = false
    };

    var stopwatch = new Stopwatch();
    stopwatch.Start();
    bool isMonitoringFailure = false;

    try
    {
        using (var httpResponse = await HttpClient.GetAsync(uri))
        {
            // add test results to availability telemetry property
            availability.Properties.Add("HttpResponseStatusCode", Convert.ToInt32(httpResponse.StatusCode).ToString());

            // check if response content contains specific text
            string content = httpResponse.Content != null ? await httpResponse.Content.ReadAsStringAsync() : "";
            if (httpResponse.IsSuccessStatusCode && content.Contains(contentMatch))
            {
                availability.Success = true;
                availability.Message = $"Test succeeded with response: {httpResponse.StatusCode}";
                log.LogTrace($"[Verbose]: {availability.Message}");
            }
            else if (!httpResponse.IsSuccessStatusCode)
            {
                availability.Message = $"Test failed with response: {httpResponse.StatusCode}";
                log.LogWarning($"[Warning]: {availability.Message}");
            }
            else
            {
                availability.Message = $"Test content does not contain: {contentMatch}";
                log.LogWarning($"[Warning]: {availability.Message}");
            }
        }
    }
    catch (TaskCanceledException e)
    {
        availability.Message = $"Test timed out: {e.Message}";
        log.LogWarning($"[Warning]: {availability.Message}");
    }
    catch (Exception ex)
    {
        // track exception when unable to determine the state of web app
        isMonitoringFailure = true;
        var exceptionTelemetry = new ExceptionTelemetry(ex);
        exceptionTelemetry.Context.Operation.Id = operationId;
        exceptionTelemetry.Properties.Add("TestName", name);
        exceptionTelemetry.Properties.Add("TestLocation", location);
        exceptionTelemetry.Properties.Add("TestUri", uri);
        TelemetryClient.TrackException(exceptionTelemetry);
        log.LogError($"[Error]: {ex.Message}");

        // optional - throw to fail the function
        //throw;
    }
    finally
    {
        stopwatch.Stop();
        availability.Duration = stopwatch.Elapsed;
        availability.Timestamp = DateTimeOffset.UtcNow;

        // do not make an assumption as to the state of the web app when monitoring failures occur
        if (!isMonitoringFailure)
        {
            TelemetryClient.TrackAvailability(availability);
            log.LogInformation($"Availability telemetry for {name} is sent.");
        }

        // call flush to ensure telemetries are sent
        TelemetryClient.Flush();
    }
}
```

Wählen Sie rechts unter „Dateien anzeigen“ die Option **Hinzufügen** aus. Geben Sie der neuen Datei den Namen **function.proj**, und verwenden Sie die folgende Konfiguration.

```C#
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
</Project>

```

![Wählen Sie rechts die Option „Hinzufügen“ aus. Geben Sie der Datei den Namen „function.proj“.](media/availability-azure-functions/addfile.png)

## <a name="check-availability"></a>Überprüfen der Verfügbarkeit

Um sicherzustellen, dass alles funktioniert, können Sie sich das Diagramm auf der Registerkarte „Verfügbarkeit“ Ihrer Application Insights-Ressource ansehen.

![Registerkarte „Verfügbarkeit“ mit erfolgreicher Ergebnisausgabe](media/availability-azure-functions/availtab.png)

Wenn Sie Ihren Test mit Azure Functions einrichten, wird Ihnen Folgendes auffallen: Im Gegensatz zur Verwendung der Option **Test hinzufügen** auf der Registerkarte „Verfügbarkeit“ wird der Name Ihres Tests nicht angezeigt, und Sie können nicht damit interagieren. Die Ergebnisse werden visualisiert, aber Sie erhalten eine Zusammenfassung und nicht die ausführliche Ansicht, die beim Erstellen eines Verfügbarkeitstests über das Portal angezeigt wird.

Wählen Sie zum Anzeigen der End-to-End-Transaktionsdetails unter „Drilldown“ die Option **Erfolgreich** oder **Fehler** aus, und wählen Sie anschließend ein Beispiel aus. Sie können auf die End-to-End-Transaktionsdetails auch zugreifen, indem Sie im Graphen einen Datenpunkt auswählen.

![Auswählen eines Beispielverfügbarkeitstests](media/availability-azure-functions/sample.png)

![End-to-End-Transaktionsdetails](media/availability-azure-functions/end-to-end.png)

## <a name="query-in-logs-analytics"></a>Abfragen in „Protokolle (Analytics)“

Sie können „Protokolle (Analytics)“ verwenden, um Ihre Verfügbarkeitsergebnisse, Abhängigkeiten und mehr anzuzeigen. Weitere Informationen zu Protokollen finden Sie unter [Übersicht über Protokollabfragen](../../azure-monitor/log-query/log-query-overview.md).

![Verfügbarkeitsergebnisse](media/availability-azure-functions/availabilityresults.png)

![Abhängigkeiten](media/availability-azure-functions/dependencies.png)

## <a name="next-steps"></a>Nächste Schritte

- [Anwendungszuordnung](../../azure-monitor/app/app-map.md)
- [Transaktionsdiagnose](../../azure-monitor/app/transaction-diagnostics.md)
