---
title: 'Tutorial: Application Insights, C#: LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial werden Application Insights-Telemetriedatenspeichern Informationen aus Bots und Language Understanding (LUIS) hinzugefügt.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 036ecbbbd2ea562f3e809691a1b3af62578893f5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498968"
---
# <a name="tutorial-add-luis-results-to-application-insights-from-a-bot-in-c"></a>Tutorial: Hinzufügen von LUIS-Ergebnissen zu Application Insights von einem Bot in C#

In diesem Tutorial werden [Application Insights](https://azure.microsoft.com/services/application-insights/)-Telemetriedatenspeichern Informationen aus Bots und Language Understanding (LUIS) hinzugefügt. Sobald Sie über diese Daten verfügen, können Sie mit der Sprache Kusto oder mit Power BI Absichten und Entitäten der Äußerung in Echtzeit analysieren, aggregieren und Berichte dafür erstellen. Diese Analyse hilft Ihnen dabei, zu ermitteln, ob Sie die Absichten und Entitäten aus Ihrer LUIS-App hinzufügen oder bearbeiten sollten.

[!INCLUDE [Waiting for Bot refresh](./includes/wait-bot-upgrade.md)]

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erfassen von Bot- und Language Understanding-Daten in Application Insights
> * Abfragen von Application Insights auf Language Understanding-Daten

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Bot Service-Bot, der mit aktiviertem Application Insights erstellt wurde
* Botcode, der aus dem vorherigen **[Tutorial](luis-csharp-tutorial-bf-v4.md)** zu Bots herunterladen wurde 
* [Bot-Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Der gesamte Code in diesem Tutorial ist im [GitHub-Repository mit Azure-Beispielen zu Language Understanding](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry) verfügbar. 

## <a name="add-application-insights-to-web-app-bot-project"></a>Hinzufügen von Application Insights zu einem Web-App-Bot-Projekt

Derzeit sammelt der Application Insights-Dienst, der in diesem Web-App-Bot verwendet wird, allgemeine Statustelemetriedaten für den Bot. Er sammelt keine LUIS-Informationen. 

Damit der Web-App-Bot LUIS-Informationen erfassen kann, muss das NuGet-Paket **[Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** für das Projekt installiert und konfiguriert sein.  

1. Fügen Sie in Visual Studio die Abhängigkeit der Projektmappe hinzu. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **NuGet-Pakete verwalten** aus. Im NuGet-Paket-Manager wird eine Liste der installierten Pakete angezeigt. 
1. Klicken Sie auf **Durchsuchen**, und suchen Sie nach **Microsoft.ApplicationInsights**.
1. Installieren Sie das Paket. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Erfassen und Senden von LUIS-Abfrageergebnissen an Application Insights

1. Öffnen Sie die Datei `LuisHelper.cs`, und ersetzen Sie den Inhalt durch den folgenden Code. Die **LogToApplicationInsights**-Methode erfasst die Bot- und LUIS-Daten und sendet sie als Ablaufverfolgungsereignis namens `LUIS` an Application Insights.

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Hinzufügen des Application Insights-Instrumentierungsschlüssels 

Um Daten zu Application Insights hinzufügen zu können, benötigen Sie den Instrumentierungsschlüssel.

1. Suchen Sie in einem Browser im [Azure-Portal](https://portal.azure.com) die **Application Insights**-Ressource Ihres Bots. Ihr Name besteht größtenteils aus dem Namen des Bots und dann aus Zufallszeichen am Ende des Namens, wie beispielsweise `luis-csharp-bot-johnsmithxqowom`. 
1. Kopieren Sie in der Application Insights-Ressource auf der Seite **Übersicht** den **Instrumentierungsschlüssel**.
1. Öffnen Sie in Visual Studio im Stammverzeichnis des Botprojekts die Datei **appsettings.json**. Diese Datei enthält alle Ihre Umgebungsvariablen.
1. Fügen Sie die neue Variable `BotDevAppInsightsKey` mit dem Wert Ihres Instrumentierungsschlüssels hinzu. Der Wert in muss in Anführungszeichen gesetzt werden. 

## <a name="build-and-start-the-bot"></a>Erstellen und Starten des Bots

1. Erstellen Sie den Bot in Visual Studio, und führen Sie ihn aus. 
1. Starten Sie den Bot-Emulator, und öffnen Sie den Bot. Diesen [Schritt](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) finden Sie im vorherigen Tutorial.

1. Stellen Sie dem Bot eine Frage. Diesen [Schritt](luis-csharp-tutorial-bf-v4.md##use-the-bot-emulator-to-test-the-bot) finden Sie im vorherigen Tutorial.

## <a name="view-luis-entries-in-application-insights"></a>Anzeigen von LUIS-Einträgen in Application Insights

Öffnen Sie Application Insights, um die LUIS-Einträge anzuzeigen. Es dauert ein paar Minuten, bis die Daten in Application Insights angezeigt werden.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource des Bots. 
1. Wenn sich die Ressource öffnet, wählen Sie **Suchen** aus, und suchen Sie nach allen Daten der letzten **30 Minuten** mit dem Ereignistyp **Ablaufverfolgung**. Wählen Sie die Ablaufverfolgung mit dem Namen **LUIS** aus. 
1. Die Bot- und LUIS-Informationen finden Sie unter **Benutzerdefinierte Eigenschaften**. 

    ![Überprüfen der in Application Insights gespeicherten benutzerdefinierten LUIS-Eigenschaften](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Abfragen von Application Insights nach Absichten, Bewertungen und Äußerungen
In Application Insights können Sie Daten mit der Sprache [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) abfragen und nach [Power BI](https://powerbi.microsoft.com) exportieren. 

1. Wählen Sie **Protokoll (Analytics)** aus. Es wird ein neues Fenster mit einem Abfragefenster oben und einem Datentabellenfenster darunter geöffnet. Wenn Sie bereits Datenbanken verwendet haben, ist Ihnen diese Anordnung vertraut. Die Abfrage stellt Ihre zuvor gefilterten Daten dar. Die Spalte **CustomDimensions** enthält die Bot- und LUIS-Informationen.
1. Um die besten Absichten, Bewertungen und Äußerungen abzurufen, fügen Sie Folgendes direkt über der letzten Zeile (der Zeile `|top...`) im Abfragefenster ein:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. Führen Sie die Abfrage aus. Die neuen Spalten „topIntent“, „score“ und „utterance“ sind verfügbar. Wählen Sie die zu sortierende Spalte „topIntent“ aus.

Erfahren Sie mehr über die [Abfragesprache Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries), oder [exportieren Sie die Daten nach Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Weitere Informationen über das Bot-Framework

Erfahren Sie mehr über das [Bot-Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen, die Sie möglicherweise den Application Insights-Daten hinzufügen sollten, sind z.B. App-ID, Versions-ID, Datum der letzten Modelländerung, Datum des letzten Trainings, Datum der letzten Veröffentlichung. Diese Werte können über die Endpunkt-URL (App-ID und Versions-ID) oder über einen Aufruf der Erstellungs-API abgerufen und dann in den Web-App-Boteinstellungen festgelegt und dort abgerufen werden.  

Wenn Sie dasselbe Endpunktabonnement für mehrere LUIS-Apps verwenden, sollten Sie auch die Abonnement-ID und eine Eigenschaft, die besagt, dass es sich um einen gemeinsam verwendeten Schlüssel handelt, einschließen.

> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](luis-how-to-add-example-utterances.md)
