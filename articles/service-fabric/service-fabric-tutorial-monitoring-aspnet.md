---
title: Überwachung und Diagnose der ASP.NET Core-Dienste auf Service Fabric in Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie eine Überwachung und Diagnose für eine ASP.NET Core-Anwendung in Azure Service Fabric einrichten.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2019
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 1f18aef12978b3df1ba1fd654ea4a0e9548a4b46
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228078"
---
# <a name="tutorial-monitor-and-diagnose-an-aspnet-core-application-on-service-fabric-using-application-insights"></a>Tutorial: Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric mithilfe von Application Insights

Dieses Tutorial ist der fünfte Teil einer Reihe. Es beschreibt die Schritte zur Einrichtung von Überwachung und Diagnose für eine ASP.NET Core-Anwendung, die auf einem Service Fabric-Cluster mithilfe von Application Insights ausgeführt wird. Aus der Anwendung, die im ersten Teil des Tutorials erstellt wurde ([Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)), werden Telemetriedaten gesammelt.

Im vierten Teil der Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Konfigurieren von Application Insights für Ihre Anwendung
> * Sammeln von Antworttelemetriedaten zur Ablaufverfolgung der HTTP-Kommunikation zwischen Diensten
> * Verwenden der App-Übersichtsfunktion in Application Insights
> * Hinzufügen von benutzerdefinierten Ereignissen mithilfe der Application Insights-API

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * [Bereitstellen der Anwendung in einem Remotecluster](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Hinzufügen eines HTTPS-Endpunkts zu einem ASP.NET Core-Front-End-Dienst](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Konfigurieren von CI/CD mit Azure Pipelines](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * Einrichten der Überwachung und Diagnose für die Anwendung

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Installieren Sie Visual Studio 2019](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
* [Installieren Sie das Service Fabric SDK](service-fabric-get-started.md).

## <a name="download-the-voting-sample-application"></a>Laden Sie die Beispielanwendung „Voting“ herunter.

Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster oder -terminal den folgenden Befehl aus, um das Beispielrepository für die App auf Ihren lokalen Computer zu klonen.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-an-application-insights-resource"></a>Einrichten einer Application Insights-Ressource

Application Insights ist eine Azure-Plattform zum Verwalten von Anwendungsleistungen und die für Service Fabric empfohlene Plattform für die Überwachung und Diagnose von Anwendungen.

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), um eine Application Insights-Ressource zu erstellen. Klicken Sie im linken Navigationsmenü auf **Ressource erstellen**, um den Azure Marketplace zu öffnen. Klicken Sie auf **Überwachung und Verwaltung** und dann auf **Application Insights**.

![Erstellen einer neuen AI-Ressource](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource.png)

Sie müssen nun die erforderlichen Informationen zu den Attributen der Ressource ausfüllen, die erstellt werden sollen. Geben Sie einen entsprechenden *Namen*, eine *Ressourcengruppe* und ein *Abonnement* ein. Legen Sie den *Speicherort* auf den Ort fest, auf dem Sie Ihren Service Fabric-Cluster in Zukunft bereitstellen möchten. In diesem Tutorial wird die App auf einem lokalen Cluster bereitgestellt, sodass das Feld *Speicherort* nicht relevant ist. Der *Anwendungstyp* sollte unverändert bei „ASP.NET-Webanwendung“ bleiben.

![AI-Ressourcenattribute](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource-attrib.png)

Sobald Sie die erforderlichen Informationen ausgefüllt haben, klicken Sie auf **Erstellen**, um die Ressource bereitzustellen. Dies sollte ungefähr eine Minute dauern.
<!-- When completed, navigate to the newly deployed resource, and find the "Instrumentation Key" (visible in the "Essentials" drop down section). Copy it to clipboard, since we will need it in the next step. -->

## <a name="add-application-insights-to-the-applications-services"></a>Hinzufügen von Application Insights zum Anwendungsdienst

Klicken Sie zum Starten von Visual Studio 2019 mit erhöhten Rechten mit der rechten Maustaste auf das Visual Studio-Symbol im Startmenü, und wählen Sie **Als Administrator ausführen** aus. Klicken Sie auf **Datei** > **Öffnen** > **Projekt/Projektmappe**, und navigieren Sie zur Abstimmungsanwendung (die entweder in Teil 1 des Tutorials erstellt oder von Git geklont wurde). Öffnen Sie *Voting.sln*. Klicken Sie auf **Ja**, wenn Sie zum Wiederherstellen der NuGet-Pakete der Anwendung aufgefordert werden.

Führen Sie die folgenden Schritte aus, um Application Insights für VotingWeb- und VotingData-Dienste zu konfigurieren:

1. Klicken Sie mit der rechten Maustaste auf den Namen des Diensts, und klicken Sie dann auf **Hinzufügen > Verbundene Dienste > Überwachung mit Application Insights**.

    ![Konfigurieren von AI](./media/service-fabric-tutorial-monitoring-aspnet/configure-ai.png)
>[!NOTE]
>Abhängig vom Projekttyp müssen Sie nach dem Rechtsklick auf den Dienstnamen auf „Hinzufügen“ > „Application Insights-Telemetrie...“ klicken.

2. Klicken Sie auf **Erste Schritte**.
3. Melden Sie sich bei dem Konto an, mit dem Sie Ihr Azure-Abonnement eingerichtet haben, und wählen Sie das Abonnement aus, in dem Sie die Application Insights-Ressource erstellt haben. Suchen Sie die Ressource unter *Vorhandene Application Insights-Ressource* im Dropdownmenü „Ressource“. Klicken Sie auf **Registrieren**, um Application Insights zu Ihrem Dienst hinzuzufügen.

    ![Registrieren von AI](./media/service-fabric-tutorial-monitoring-aspnet/register-ai.png)

4. Klicken Sie auf **Fertig stellen**, sobald das angezeigte Dialogfeld die Aktion abgeschlossen hat.

> [!NOTE]
> Stellen Sie sicher, dass Sie die oben aufgeführten Schritte für **beide** Dienste der Anwendung durchführen, um die Konfiguration von Application Insights für die Anwendung abzuschließen.
> Für beide Dienste wird dieselbe Application Insights-Ressource verwendet, damit eingehende und ausgehende Anforderungen und die Kommunikation zwischen den Diensten angezeigt werden.

## <a name="add-the-microsoftapplicationinsightsservicefabricnative-nuget-to-the-services"></a>Hinzufügen des NuGet-Pakets „Microsoft.ApplicationInsights.ServiceFabric.Native“ zu den Diensten

Application Insights verfügt über zwei für Service Fabric spezifische NuGet-Pakete, die abhängig vom Szenario verwendet werden können. Eines wird mit den nativen Service Fabric-Diensten verwendet, das andere mit Containern und ausführbaren Gastanwendungsdateien. In diesem Fall wird das NuGet-Paket „Microsoft.ApplicationInsights.ServiceFabric.Native“ verwendet, um das Verständnis des Dienstkontexts zu nutzen, das dieses mit sich bringt. Weitere Informationen zum Application Insights SDK und den für Service Fabric spezifischen NuGet-Paketen finden Sie unter [Microsoft Application Insights für Service Fabric](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/master/README.md).

Folgende Schritte sind für das Einrichten des NuGet-Pakets erforderlich:

1. Klicken Sie ganz oben im Projektmappen-Explorer mit der rechten Maustaste auf die **Projektmappe „Voting“** , und klicken Sie auf **NuGet-Pakete für Projektmappe verwalten...** .
2. Klicken Sie im oberen Navigationsmenü des Fensters „NuGet – Projektmappe“ auf **Durchsuchen**, und aktivieren Sie das Feld **Vorabversion einbeziehen** neben der Suchleiste.
>[!NOTE]
>Gegebenenfalls müssen Sie vor der Installation des Application Insights-Pakets auch das Paket „Microsoft.ServiceFabric.Diagnostics.Internal“ installieren, sofern es nicht bereits vorinstalliert war.

3. Suchen Sie nach `Microsoft.ApplicationInsights.ServiceFabric.Native`, und klicken Sie auf das entsprechende NuGet-Paket.
4. Klicken Sie auf der rechten Seite auf die zwei Kontrollkästchen neben den beiden Diensten der Anwendung (**VotingWeb** und **VotingData**), und klicken Sie auf **Installieren**.
    ![AI sdk Nuget](./media/service-fabric-tutorial-monitoring-aspnet/ai-sdk-nuget-new.png)
5. Klicken Sie im angezeigten Dialogfeld *Änderungen überprüfen* auf **OK**, und akzeptieren Sie die *Lizenzbedingungen*. Dadurch wird das Hinzufügen des NuGet-Pakets zu den Diensten abgeschlossen.
6. Sie müssen nun den Telemetrieinitialisierer in beiden Diensten einrichten. Öffnen Sie dazu *VotingWeb.cs* und *VotingData.cs*. Führen Sie für beide folgende Schritte aus:
    1. Fügen Sie diese zwei *using*-Anweisungen jeweils vor *\<ServiceName>.cs* und nach den vorhandenen *using*-Anweisungen hinzu:

    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.ServiceFabric;
    ```

    2. Fügen Sie in beiden Dateien in der geschachtelten *return*-Anweisung von *CreateServiceInstanceListeners()* oder *CreateServiceReplicaListeners()* unter *ConfigureServices* > *services* mit den anderen deklarierten Singleton-Diensten Folgendes hinzu:
    ```csharp
    .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
    ```
    Dadurch wird der *Dienstkontext* zu Ihrer Telemetrie hinzugefügt, durch den Sie die Quelle Ihrer Telemetrie in Application Insights besser nachvollziehen können. Ihre geschachtelte *return*-Anweisung in *VotingWeb.cs* sollte folgendermaßen aussehen:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<HttpClient>(new HttpClient())
                .AddSingleton<FabricClient>(new FabricClient())
                .AddSingleton<StatelessServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext)))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
        .UseUrls(url)
        .Build();
    ```

    In *VotingData.cs* sollte diese folgendermaßen aussehen:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<StatefulServiceContext>(serviceContext)
                .AddSingleton<IReliableStateManager>(this.StateManager)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext)))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
        .UseUrls(url)
        .Build();
    ```

Vergewissern Sie sich, dass die `UseApplicationInsights()`-Methode in den beiden Dateien *VotingWeb.cs* und *VotingData.cs* aufgerufen wird, wie oben gezeigt.

>[!NOTE]
>Diese Beispiel-App verwendet HTTP für die Dienstkommunikation. Wenn Sie eine App mit Dienstremoting V2 entwickeln, müssen Sie am gleichen Ort wie oben auch die folgenden Codezeilen hinzufügen.

```csharp
ConfigureServices(services => services
    ...
    .AddSingleton<ITelemetryModule>(new ServiceRemotingDependencyTrackingTelemetryModule())
    .AddSingleton<ITelemetryModule>(new ServiceRemotingRequestTrackingTelemetryModule())
)
```

An diesem Punkt können Sie Ihre Anwendung bereitstellen. Klicken Sie oben auf **Starten** (oder drücken Sie **F5**), damit Visual Studio die Anwendung erstellt und packt, richten Sie Ihren lokalen Cluster ein, und stellen Sie die Anwendung darauf bereit.

>[!NOTE]
>Unter Umständen wird ein Buildfehler angezeigt, wenn keine aktuelle Version des .NET Core SDK installiert ist.

Sobald die Anwendung bereitgestellt ist, navigieren Sie zu [localhost:8080](localhost:8080). Dort sollte Ihnen die einseitige Beispielanwendung „Voting“ angezeigt werden. Stimmen Sie für einige verschiedene Elemente Ihrer Wahl ab, um Beispieldaten und eine Beispieltelemetrie zu erstellen – ich habe mich für Desserts entschieden.

![AI-Beispielabstimmungen](./media/service-fabric-tutorial-monitoring-aspnet/vote-sample.png)

Sie können auch ein paar der Abstimmungsoptionen *entfernen*, wenn Sie ein paar Stimmen hinzugefügt haben.

## <a name="view-telemetry-and-the-app-map-in-application-insights"></a>Anzeigen der Telemetrie und der App-Zuordnung in Application Insights

Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource.

Klicken Sie auf **Übersicht**, um zurück zur Startseite Ihrer Ressource zu gelangen. Klicken Sie dann oben auf **Suchen**, um die eingehenden Ablaufverfolgungen anzuzeigen. Es dauert ein paar Minuten, bis die Ablaufverfolgungen in Application Insights angezeigt werden. Falls keine Ablaufverfolgungen angezeigt werden, warten Sie eine Minute, und klicken Sie dann oben auf die Schaltfläche **Aktualisieren**.
![Angezeigte AI-Nachverfolgungen](./media/service-fabric-tutorial-monitoring-aspnet/ai-search.png)

Wenn Sie im Fenster *Suche* nach unten scrollen, werden Ihnen alle eingehenden Telemetriedaten angezeigt, die Sie durch Application Insights vordefiniert erhalten. Für jede Aktion, die Sie in der Voting-Anwendung vorgenommen haben, sollten eine ausgehende PUT-Anforderung von *VotingWeb* (PUT-Stimmen/Put [Name]) und eine eingehende PUT-Anforderung von *VotingData* (PUT-Abstimmungsdaten/Put [Name]) vorhanden sein, gefolgt von einem Paar von GET-Anforderungen zum Aktualisieren der angezeigten Daten. Außerdem wird eine Ablaufverfolgung der Abhängigkeit für HTTP von „localhost“ angezeigt, da es sich um HTTP-Anforderungen handelt. In diesem Beispiel wird dargestellt, wie eine Stimme hinzugefügt wird:

![Ablaufverfolgung einer KI-Beispielanforderung](./media/service-fabric-tutorial-monitoring-aspnet/sample-request.png)

Sie können auf eine der Ablaufverfolgungen klicken, um weitere Details anzuzeigen. Dort sind nützliche Informationen zu der von Application Insights bereitgestellten Anforderung enthalten, einschließlich der *Antwortzeit* und der *Anforderungs-URL*. Da Sie die für Service Fabric spezifischen NuGet-Pakete hinzugefügt haben, erhalten Sie ebenfalls Daten zu Ihrer Anwendung im Kontext eines Service Fabric-Clusters im folgenden Abschnitt *Benutzerdefinierte Daten*. Dies schließt den Dienstkontext ein, sodass Ihnen die *PartitionId* und die *ReplicaId* der Quelle der Anforderung angezeigt werden. Außerdem können Sie Probleme besser lokalisieren, wenn Sie Fehler in Ihrer Anwendung diagnostizieren.

![AI-Ablaufverfolgungsdetails](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

Darüber hinaus können Sie im linken Menü der Übersichtsseite auf *Anwendungsübersicht* oder auf das Symbol **App-Übersicht** klicken, um zur App-Übersicht mit Ihren beiden verbundenen Diensten zu gelangen.

![AI-Ablaufverfolgungsdetails](./media/service-fabric-tutorial-monitoring-aspnet/app-map-new.png)

Die App-Übersichtsfunktion kann Sie beim Nachvollziehen der Anwendungstopologie unterstützen, insbesondere wenn Sie mehrere verschiedene Dienste hinzufügen, die zusammenarbeiten. Sie erhalten ebenfalls Daten zur Erfolgsrate der Anforderungen und werden bei der Diagnose fehlgeschlagener Anforderungen unterstützt, um nachzuvollziehen, warum Fehler aufgetreten sind. Weitere Informationen zur App-Zuordnung erhalten Sie unter [Application Map in Application Insights (Anwendungszuordnung in Application Insights)](../azure-monitor/app/app-map.md).

## <a name="add-custom-instrumentation-to-your-application"></a>Hinzufügen einer benutzerdefinierten Instrumentierung zu Ihrer Anwendung

Obwohl Application Insights viele vordefinierte Telemetriedaten bereitstellt, sollten Sie weitere benutzerdefinierte Instrumentierungen hinzufügen. Diese können Sie an die Anforderungen Ihres Unternehmens anpassen oder zur Diagnose verwenden, wenn Fehler in Ihrer Anwendung auftreten. Application Insights verfügt über eine API zum Erfassen benutzerdefinierter Ereignisse und Metriken. Mehr dazu finden Sie [hier](../azure-monitor/app/api-custom-events-metrics.md).

Fügen Sie einige benutzerdefinierte Ereignisse zu *VoteDataController.cs* (unter *VotingData* > *Controller*) hinzu, um zu verfolgen, wann Stimmen dem zugrunde liegenden *votesDictionary* hinzugefügt oder aus diesem gelöscht werden.

1. Fügen Sie `using Microsoft.ApplicationInsights;` zum Ende der anderen using-Anweisungen hinzu.
2. Deklarieren Sie einen neuen *TelemetryClient* am Anfang der Klasse, nach der Erstellung des *IReliableStateManager*: `private TelemetryClient telemetry = new TelemetryClient();`
3. Fügen Sie in der *Put()* -Funktion ein Ereignis hinzu, das bestätigt, dass eine Stimme hinzugefügt wurde. Fügen Sie `telemetry.TrackEvent($"Added a vote for {name}");` direkt vor der return-Anweisung *OkResult* hinzu, nachdem die Transaktion abgeschlossen ist.
4. In *Delete()* ist eine if/else-Bedingung vorhanden, die auf der Bedingung basiert, dass *votesDictionary* Stimmen für eine bestimmte Abstimmungsoption enthält.
    1. Fügen Sie nach *await tx.CommitAsync()* ein Ereignis hinzu, das das Löschen einer Stimme in der *if*-Anweisung bestätigt: `telemetry.TrackEvent($"Deleted votes for {name}");`
    2. Fügen Sie vor der return-Anweisung ein Ereignis hinzu, um anzuzeigen, dass der Löschvorgang in der *else*-Anweisung nicht erfolgt ist: `telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");`

In diesem Beispiel werden die Funktionen *Put()* und *Delete()* dargestellt, nachdem die Ereignisse hinzugefügt wurden:

```csharp
// PUT api/VoteData/name
[HttpPut("{name}")]
public async Task<IActionResult> Put(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
        await tx.CommitAsync();
    }

    telemetry.TrackEvent($"Added a vote for {name}");
    return new OkResult();
}

// DELETE api/VoteData/name
[HttpDelete("{name}")]
public async Task<IActionResult> Delete(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        if (await votesDictionary.ContainsKeyAsync(tx, name))
        {
            await votesDictionary.TryRemoveAsync(tx, name);
            await tx.CommitAsync();
            telemetry.TrackEvent($"Deleted votes for {name}");
            return new OkResult();
        }
        else
        {
            telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");
            return new NotFoundResult();
        }
    }
}
```

Sobald Sie mit den Änderungen fertig sind, **starten** Sie die Anwendung, sodass diese erstellt und die aktuelle Version bereitgestellt wird. Sobald die Anwendung bereitgestellt ist, navigieren Sie zu [localhost:8080](localhost:8080), und fügen Sie einige Abstimmungsoptionen hinzu oder löschen Sie vorhandene. Wechseln Sie dann zu Ihrer Application Insights-Ressource zurück, um die Ablaufverfolgung der letzten Ausführung anzuzeigen (wie zuvor kann es 1–2 Minuten dauern, bis diese in Application Insights angezeigt wird). Für alle Stimmen, die Sie hinzugefügt oder gelöscht haben, sollte nun „Benutzerdefiniertes Ereignis“\* zusammen mit der Antworttelemetrie angezeigt werden.

![Benutzerdefinierte Ereignisse](./media/service-fabric-tutorial-monitoring-aspnet/custom-events.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:
> [!div class="checklist"]
> * Konfigurieren von Application Insights für Ihre Anwendung
> * Sammeln von Antworttelemetriedaten zur Ablaufverfolgung der HTTP-Kommunikation zwischen Diensten
> * Verwenden der App-Übersichtsfunktion in Application Insights
> * Hinzufügen von benutzerdefinierten Ereignissen mithilfe der Application Insights-API

Da Sie nun die Einrichtung der Überwachung und Diagnose für Ihre ASP.NET-Anwendung abgeschlossen haben, probieren Sie Folgendes aus:

* [Erkunden Sie die Überwachung und Diagnose in Service Fabric.](service-fabric-diagnostics-overview.md)
* [Service Fabric event analysis with Application Insights (Service Fabric-Ereignisanalyse mit Application Insights)](service-fabric-diagnostics-event-analysis-appinsights.md)
* Weitere Informationen zu Application Insights finden Sie unter [Application Insights Documentation (Dokumentation zu Application Insights)](https://docs.microsoft.com/azure/application-insights/)
