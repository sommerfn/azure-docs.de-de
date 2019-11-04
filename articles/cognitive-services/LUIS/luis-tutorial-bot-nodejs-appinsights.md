---
title: 'Tutorial: Application Insights, Node.js: LUIS'
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
ms.openlocfilehash: b0bc4a93df7db7145accf2b485c45256f53c324d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498951"
---
# <a name="tutorial-add-luis-results-to-application-insights-from-a-bot-in-nodejs"></a>Tutorial: Hinzufügen von LUIS-Ergebnissen zu Application Insights von einem Bot in Node.js
In diesem Tutorial werden [Application Insights](https://azure.microsoft.com/services/application-insights/)-Telemetriedatenspeichern Informationen aus Bots und Language Understanding (LUIS) hinzugefügt. Sobald Sie über diese Daten verfügen, können Sie mit der Sprache Kusto oder mit Power BI Absichten und Entitäten der Äußerung in Echtzeit analysieren, aggregieren und Berichte dafür erstellen. Diese Analyse hilft Ihnen dabei, zu ermitteln, ob Sie die Absichten und Entitäten aus Ihrer LUIS-App hinzufügen oder bearbeiten sollten.

[!INCLUDE [Waiting for Bot refresh](./includes/wait-bot-upgrade.md)]

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erfassen von Bot- und Language Understanding-Daten in Application Insights
> * Abfragen von Application Insights auf Language Understanding-Daten

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Bot Service-Bot, der mit aktiviertem Application Insights erstellt wurde
* Botcode, der aus dem vorherigen **[Tutorial](luis-nodejs-tutorial-bf-v4.md)** zu Bots herunterladen wurde 
* [Bot-Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Der gesamte Code in diesem Tutorial ist im [GitHub-Repository mit Azure-Beispielen zu Language Understanding](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-nodejs-bot-johnsmith-src-telemetry) verfügbar. 

## <a name="add-application-insights-to-web-app-bot-project"></a>Hinzufügen von Application Insights zu einem Web-App-Bot-Projekt
Derzeit sammelt der Application Insights-Dienst, der in diesem Web-App-Bot verwendet wird, allgemeine Statustelemetriedaten für den Bot. Er sammelt keine LUIS-Informationen. 

Damit der Web-App-Bot LUIS-Informationen erfassen kann, muss das NPM-Paket **[Application Insights](https://www.npmjs.com/package/applicationinsights)** für das Projekt installiert und konfiguriert sein.  

1. Fügen Sie im integrierten Visual Studio Code-Terminal im Stammverzeichnis des Botprojekts die folgenden NPM-Pakete mit dem gezeigten Befehl hinzu: 

    ```console
    npm install applicationinsights && npm install underscore
    ```
    
    Das **Unterstrich**-Paket wird verwendet, um die JSON-Struktur von LUIS zu vereinfachen, damit sie leichter einzusehen und in Application Insights zu verwenden ist.
    


## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Erfassen und Senden von LUIS-Abfrageergebnissen an Application Insights

1. Erstellen Sie in Visual Studio Code die neue Datei **appInsightsLog.js**, und fügen Sie den folgenden Code hinzu:

    ```javascript
    const appInsights = require('applicationinsights');
    const _ = require("underscore");
    
    // Log LUIS results to Application Insights
    // must flatten as name/value pairs
    var appInsightsLog = (botContext,luisResponse) => {

        appInsights.setup(process.env.MicrosoftApplicationInsightsInstrumentationKey).start();
        const appInsightsClient = appInsights.defaultClient;

        // put bot context and LUIS results into single object
        var data = Object.assign({}, {'botContext': botContext._activity}, {'luisResponse': luisResponse});
    
        // Flatten data into name/value pairs
        flatten = (x, result, prefix) => {
            if(_.isObject(x)) {
                _.each(x, (v, k) => {
                    flatten(v, result, prefix ? prefix + '_' + k : k)
                })
            } else {
                result["LUIS_" + prefix] = x
            }
            return result;
        }
    
        // call fn to flatten data
        var flattenedData = flatten(data, {});
    
        // ApplicationInsights Trace 
        console.log(JSON.stringify(flattenedData));
    
        // send data to Application Insights
        appInsightsClient.trackTrace({message: "LUIS", severity: appInsights.Contracts.SeverityLevel.Information, properties: flattenedData});
    }
    
    module.exports.appInsightsLog = appInsightsLog;
    ```

    Diese Datei verwendet den Botkontext und die LUIS-Antwort, vereinfacht beide Objekte und fügt sie in Application Insights in ein Ereignis des Typs **Ablaufverfolgung** ein. Der Name des Ereignisses ist **LUIS**. 

1. Öffnen Sie den Ordner **dialogs** und dann die Datei **luisHelper.js**. Fügen Sie die neue Datei **appInsightsLog.js** als erforderliche Datei hinzu, und erfassen Sie den Botkontext und die LUIS-Antwort. Der vollständige Code für diese Demo ist wie folgt: 

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    const { appInsightsLog } = require('../appInsightsLog');
    
    class LuisHelper {
        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {*} logger
         * @param {TurnContext} context
         */
        static async executeLuisQuery(logger, context) {
            const bookingDetails = {};
    
            try {
                const recognizer = new LuisRecognizer({
                    applicationId: process.env.LuisAppId,
                    endpointKey: process.env.LuisAPIKey,
                    endpoint: `https://${ process.env.LuisAPIHostName }`
                }, {}, true);
    
                const recognizerResult = await recognizer.recognize(context);
    
                // APPINSIGHT: Log results to Application Insights
                appInsightsLog(context,recognizerResult);
    
    
                const intent = LuisRecognizer.topIntent(recognizerResult);
    
                bookingDetails.intent = intent;
    
                if (intent === 'Book_flight') {
                    // We need to get the result from the LUIS JSON which at every level returns an array
    
                    bookingDetails.destination = LuisHelper.parseCompositeEntity(recognizerResult, 'To', 'Airport');
                    bookingDetails.origin = LuisHelper.parseCompositeEntity(recognizerResult, 'From', 'Airport');
    
                    // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                    // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                    bookingDetails.travelDate = LuisHelper.parseDatetimeEntity(recognizerResult);
                }
            } catch (err) {
                logger.warn(`LUIS Exception: ${ err } Check your LUIS configuration`);
            }
            return bookingDetails;
        }
    
        static parseCompositeEntity(result, compositeName, entityName) {
            const compositeEntity = result.entities[compositeName];
            if (!compositeEntity || !compositeEntity[0]) return undefined;
    
            const entity = compositeEntity[0][entityName];
            if (!entity || !entity[0]) return undefined;
    
            const entityValue = entity[0][0];
            return entityValue;
        }
    
        static parseDatetimeEntity(result) {
            const datetimeEntity = result.entities['datetime'];
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;
    
            const timex = datetimeEntity[0]['timex'];
            if (!timex || !timex[0]) return undefined;
    
            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }
    
    module.exports.LuisHelper = LuisHelper;
    ```

## <a name="add-application-insights-instrumentation-key"></a>Hinzufügen des Application Insights-Instrumentierungsschlüssels 

Um Daten zu Application Insights hinzufügen zu können, benötigen Sie den Instrumentierungsschlüssel.

1. Suchen Sie in einem Browser im [Azure-Portal](https://portal.azure.com) die **Application Insights**-Ressource Ihres Bots. Ihr Name besteht größtenteils aus dem Namen des Bots und dann aus Zufallszeichen am Ende des Namens, wie beispielsweise `luis-nodejs-bot-johnsmithxqowom`. 
1. Kopieren Sie auf der Seite **Übersicht** in der Application Insights-Ressource den **Instrumentierungsschlüssel**.
1. Öffnen Sie in Visual Studio Code die **ENV**-Datei im Stammverzeichnis des Botprojekts. Diese Datei enthält alle Ihre Umgebungsvariablen.  
1. Fügen Sie die neue Variable `MicrosoftApplicationInsightsInstrumentationKey` mit dem Wert Ihres Instrumentierungsschlüssels hinzu. Setzen Sie den Wert nicht in Anführungszeichen. 

## <a name="start-the-bot"></a>Starten des Bots

1. Starten Sie in Visual Studio Code den Bot im integrierten Terminal:
    
    ```console
    npm start
    ```

1. Starten Sie den Bot-Emulator, und öffnen Sie den Bot. Diesen [Schritt](luis-nodejs-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) finden Sie im vorherigen Tutorial.

1. Stellen Sie dem Bot eine Frage. Diesen [Schritt](luis-nodejs-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) finden Sie im vorherigen Tutorial.

## <a name="view-luis-entries-in-application-insights"></a>Anzeigen von LUIS-Einträgen in Application Insights

Öffnen Sie Application Insights, um die LUIS-Einträge anzuzeigen. Es dauert ein paar Minuten, bis die Daten in Application Insights angezeigt werden.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) die Application Insights-Ressource des Bots. 
1. Wenn sich die Ressource öffnet, wählen Sie **Suchen** aus, und suchen Sie nach allen Daten der letzten **30 Minuten** mit dem Ereignistyp **Ablaufverfolgung**. Wählen Sie die Ablaufverfolgung mit dem Namen **LUIS** aus. 
1. Die Bot- und LUIS-Informationen finden Sie unter **Benutzerdefinierte Eigenschaften**. 

    ![Überprüfen der in Application Insights gespeicherten benutzerdefinierten LUIS-Eigenschaften](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-nodejs.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Abfragen von Application Insights nach Absichten, Bewertungen und Äußerungen
In Application Insights können Sie Daten mit der Sprache [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) abfragen und nach [Power BI](https://powerbi.microsoft.com) exportieren. 

1. Wählen Sie **Protokoll (Analytics)** aus. Es wird ein neues Fenster mit einem Abfragefenster oben und einem Datentabellenfenster darunter geöffnet. Wenn Sie bereits Datenbanken verwendet haben, ist Ihnen diese Anordnung vertraut. Die Abfrage stellt Ihre zuvor gefilterten Daten dar. Die Spalte **CustomDimensions** enthält die Bot- und LUIS-Informationen.
1. Um die besten Absichten, Bewertungen und Äußerungen abzurufen, fügen Sie Folgendes direkt über der letzten Zeile (der Zeile `|top...`) im Abfragefenster ein:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_intent)
    | extend score = todouble(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_score)
    | extend utterance = tostring(customDimensions.LUIS_luisResponse_text)
    ```

1. Führen Sie die Abfrage aus. Die neuen Spalten „topIntent“, „score“ und „utterance“ sind verfügbar. Wählen Sie die zu sortierende Spalte „topIntent“ aus.

Erfahren Sie mehr über die [Abfragesprache Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries), oder [exportieren Sie die Daten nach Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen, die Sie möglicherweise den Application Insights-Daten hinzufügen sollten, sind z.B. App-ID, Versions-ID, Datum der letzten Modelländerung, Datum des letzten Trainings, Datum der letzten Veröffentlichung. Diese Werte können über die Endpunkt-URL (App-ID und Versions-ID) oder über einen Aufruf der Erstellungs-API abgerufen und dann in den Web-App-Boteinstellungen festgelegt und dort abgerufen werden.  

Wenn Sie dasselbe Endpunktabonnement für mehrere LUIS-Apps verwenden, sollten Sie auch die Abonnement-ID und eine Eigenschaft, die besagt, dass es sich um einen gemeinsam verwendeten Schlüssel handelt, einschließen. 

> [!div class="nextstepaction"]
> [Weitere Informationen zu Beispieläußerungen](luis-how-to-add-example-utterances.md)
