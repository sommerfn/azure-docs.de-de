---
title: Language Understanding-Bot C# v4
titleSuffix: Azure Cognitive Services
description: Erstellen Sie mithilfe von C# einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Der Bot wird mit Bot Framework, Version 4 und dem Azure Web-App-Bot-Dienst erstellt.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 08/30/2019
ms.author: diberry
ms.openlocfilehash: c78359920ebc5faab2e0a678a901bcb8581a4e45
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2019
ms.locfileid: "70207330"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-c"></a>Tutorial: Verwenden eines Web-App-Bots mit aktiviertem Language Understanding in C#

Erstellen Sie mithilfe von C# einen in Language Understanding (LUIS) integrierten Chatbot. Der Bot wird mit der [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/)-Ressource von Azure und [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) V4 erstellt.

**In diesem Tutorial lernen Sie Folgendes:**

> [!div class="checklist"]
> * Erstellen eines Web-App-Bots. In diesem Vorgang wird eine neue LUIS-App erstellt.
> * Herunterladen des vom Webbotdienst erstellten Botprojekts
> * Lokales Starten von Bot und Emulator auf dem eigenen Computer
> * Anzeigen von Äußerungsergebnissen im Bot

## <a name="prerequisites"></a>Voraussetzungen

* [Bot-Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-a-web-app-bot-resource"></a>Erstellen einer Web-App-Bot-Ressource

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) **Neue Ressource erstellen** aus.

1. Suchen Sie im Suchfeld nach **Web-App-Bot**, und wählen Sie den Eintrag aus. Klicken Sie auf **Erstellen**.

1. Geben Sie in **Botdienst** die erforderlichen Informationen ein:

    |Einstellung|Zweck|Empfohlene Einstellung|
    |--|--|--|
    |Botname|Ressourcenname|`luis-csharp-bot-` + `<your-name>`, z.B. `luis-csharp-bot-johnsmith`|
    |Subscription|Abonnement, in dem der Bot erstellt werden soll.|Ihr primäres Abonnement.
    |Resource group|Logische Gruppe von Azure-Ressourcen|Erstellen einer neuen Gruppe zum Speichern aller Ressourcen, die mit diesen Bot verwendet werden; benennen Sie die Gruppe `luis-csharp-bot-resource-group`.|
    |Location|Azure-Region: muss nicht die gleiche wie die Erstellungs- oder Veröffentlichungsregion von LUIS sein.|`westus`|
    |Tarif|Für die Grenzwerte der Dienstanforderungen und die Abrechnung verwendet.|`F0` ist der kostenlose Tarif.
    |App-Name|Der Name wird als Unterdomäne verwendet, wenn Ihr Bot in der Cloud bereitgestellt wird (z.B. personalwesenbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, z.B. `luis-csharp-bot-johnsmith`|
    |Botvorlage|Einstellungen für Bot Framework – siehe nächste Tabelle|
    |LUIS-App-Speicherort|Muss mit der LUIS-Ressourcenregion übereinstimmen|`westus`|
    |App Service-Plan/Standort|Ändern Sie den angegebenen Standardwert nicht.|
    |Application Insights|Ändern Sie den angegebenen Standardwert nicht.|
    |Microsoft-App-ID und Kennwort|Ändern Sie den angegebenen Standardwert nicht.|

1. Wählen Sie in **Bot template** (Botvorlage) die folgenden Werte und dann unterhalb dieser Einstellungen die Schaltfläche **Auswählen** aus:

    |Einstellung|Zweck|Auswahl|
    |--|--|--|
    |SDK-Version|Bot-Framework-Version|**SDK v4**|
    |SDK-Sprache|Programmiersprache des Bots|**C#**|
    |Bot|Bottyp|**Basisbot**|
    
1. Klicken Sie auf **Erstellen**. Dadurch wird der Botdienst erstellt und in Azure bereitgestellt. Im Rahmen dieses Vorgangs wird eine LUIS-App mit dem Namen `luis-csharp-bot-XXXX` erstellt. Dieser Name basiert auf dem Namen der Azure Bot Service-App.

    [![Erstellen des Web-App-Bots](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

    Warten Sie, bis der Botdienst erstellt wurde, bevor Sie fortfahren.

## <a name="the-bot-has-a-language-understanding-model"></a>Der Bot beinhaltet ein Language Understanding-Modell

Bei der Erstellung des Botdiensts wird auch eine neue LUIS-App mit Absichten und Beispieläußerungen erstellt. Der Bot stellt die Zuordnung der Absichten zur neuen LUIS-App für die folgenden Absichten bereit: 

|LUIS-Absichten für Basisbots|Beispieläußerung|
|--|--|
|Flugbuchung|`Travel to Paris`|
|Abbrechen|`bye`|
|Keine|Alles, was sich außerhalb der Domäne der App befindet.|

## <a name="test-the-bot-in-web-chat"></a>Testen des Bots im Webchat

1. Wählen Sie im Azure-Portal für den neuen Bot die Option **Test in Web Chat** (Test im Webchat) aus. 
1. Geben Sie im Textfeld **Nachricht eingeben** den Text `hello` ein. Der Bot antwortet mit Informationen zum Bot Framework und mit Beispielabfragen für das spezifische LUIS-Modell, z. B. die Buchung eines Flugs nach Paris. 

    ![Screenshot des Azure-Portals, Eingeben des Texts „hello“](./media/bfv4-csharp/ask-bot-question-in-portal-test-in-web-chat.png)

    Über die Testfunktionen können Sie Ihren Bot schnell testen. Laden Sie für umfangreichere Tests, einschließlich Debuggen, den Botcode herunter, und verwenden Sie Visual Studio. 

## <a name="download-the-web-app-bot-source-code"></a>Herunterladen des Quellcodes für den Web-App-Bot
Um den Code des Web-App-Bots zu entwickeln, laden Sie den Code herunter, und verwenden Sie ihn auf Ihrem lokalen Computer. 

1. Wählen Sie im Azure-Portal im Abschnitt **Bot Management** (Botverwaltung) **Erstellen** aus. 

1. Wählen Sie **Bot-Quellcode herunterladen** aus. 

    [![Herunterladen des Web-App-Bot-Quellcodes für den Basisbot](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Wenn im Popupdialogfeld die Frage **Include app settings in the downloaded zip file?** (App-Einstellungen in die heruntergeladene ZIP-Datei einfügen?) angezeigt wird, wählen Sie **Ja** aus.

1. Wenn der Quellcode gezippt ist, wird in einer Meldung ein Link zum Herunterladen des Codes zur Verfügung gestellt. Wählen Sie den Link aus. 

1. Speichern Sie die ZIP-Datei auf Ihrem lokalen Computer, und extrahieren Sie die Dateien. Öffnen Sie das Projekt in Visual Studio. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Überprüfen des Codes zum Senden einer Äußerung an LUIS und Erhalten einer Antwort

1. Öffnen Sie die Datei **LuisHelper.cs**. Das ist die Stelle, an der in den Bot eingegebene Benutzeräußerungen an LUIS gesendet werden. Die Antwort von LUIS wird über die Methode als **BookDetails**-Objekt zurückgegeben. Wenn Sie einen eigenen Bot erstellen, sollten Sie auch eigene Objekte erstellen, die die Details von LUIS zurückgeben. 


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
        }
    }
    ```

1. Öffnen Sie **BookingDetails.cs**, um zu prüfen, wie das Objekt die LUIS-Informationen abstrahiert. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    namespace Microsoft.BotBuilderSamples
    {
        public class BookingDetails
        {
            public string Destination { get; set; }
    
            public string Origin { get; set; }
    
            public string TravelDate { get; set; }
        }
    }
    ```

1. Öffnen Sie **Dialoge** und dann die Datei „BookingDialog.cs“, um zu sehen, wie das BookingDetails-Objekt zum Verwalten des Konversationsflusses verwendet wird. Die Reisedetails werden in Schritten abgefragt. Anschließend wird die gesamte Buchung bestätigt und schließlich an den Benutzer zurückgegeben. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    
    namespace Microsoft.BotBuilderSamples.Dialogs
    {
        public class BookingDialog : CancelAndHelpDialog
        {
            public BookingDialog()
                : base(nameof(BookingDialog))
            {
                AddDialog(new TextPrompt(nameof(TextPrompt)));
                AddDialog(new ConfirmPrompt(nameof(ConfirmPrompt)));
                AddDialog(new DateResolverDialog());
                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    DestinationStepAsync,
                    OriginStepAsync,
                    TravelDateStepAsync,
                    ConfirmStepAsync,
                    FinalStepAsync,
                }));
    
                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }
    
            private async Task<DialogTurnResult> DestinationStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                if (bookingDetails.Destination == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where would you like to travel to?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Destination, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> OriginStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Destination = (string)stepContext.Result;
    
                if (bookingDetails.Origin == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where are you traveling from?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Origin, cancellationToken);
                }
            }
            private async Task<DialogTurnResult> TravelDateStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Origin = (string)stepContext.Result;
    
                if (bookingDetails.TravelDate == null || IsAmbiguous(bookingDetails.TravelDate))
                {
                    return await stepContext.BeginDialogAsync(nameof(DateResolverDialog), bookingDetails.TravelDate, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.TravelDate, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> ConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.TravelDate = (string)stepContext.Result;
    
                var msg = $"Please confirm, I have you traveling to: {bookingDetails.Destination} from: {bookingDetails.Origin} on: {bookingDetails.TravelDate}";
    
                return await stepContext.PromptAsync(nameof(ConfirmPrompt), new PromptOptions { Prompt = MessageFactory.Text(msg) }, cancellationToken);
            }
    
            private async Task<DialogTurnResult> FinalStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                if ((bool)stepContext.Result)
                {
                    var bookingDetails = (BookingDetails)stepContext.Options;
    
                    return await stepContext.EndDialogAsync(bookingDetails, cancellationToken);
                }
                else
                {
                    return await stepContext.EndDialogAsync(null, cancellationToken);
                }
            }
    
            private static bool IsAmbiguous(string timex)
            {
                var timexProperty = new TimexProperty(timex);
                return !timexProperty.Types.Contains(Constants.TimexTypes.Definite);
            }
        }
    }
    ```


## <a name="start-the-bot-code-in-visual-studio"></a>Starten des Botcodes in Visual Studio

Starten Sie den Bot in Visual Studio. Ein Browserfenster mit der Website des Web-App-Bots wird unter der Adresse `http://localhost:3978/` geöffnet. Auf einer Startseite werden Informationen zu dem Bot angezeigt.

![Auf einer Startseite werden Informationen zu dem Bot angezeigt.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Verwenden des Bot-Emulators zum Testen des Bots

1. Starten Sie den Bot-Emulator, und wählen Sie **Open Bot** (Bot öffnen) aus.
1. Geben Sie im Popupdialogfeld **Open a bot** (Bot öffnen) die URL des Bots ein, z. B. `http://localhost:3978/api/messages`. Die Route `/api/messages` ist die Webadresse für den Bot.
1. Geben Sie die **Microsoft-App-ID** und das **Microsoft-App-Kennwort** ein, die sich in der Datei **appsettings.json** im Stammverzeichnis des heruntergeladenen Botcodes befinden.

    Optional können Sie eine neue Botkonfiguration erstellen und `appId` und `appPassword` aus der Datei **appsettings.json** in das Visual Studio-Projekt für den Bot kopieren. Der Name der Botkonfigurationsdatei muss mit dem Namen des Bots identisch sein. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from appsettings.json>",
                "appPassword": "<appPassword from appsettings.json>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. Geben Sie im Bot-Emulator `Hello` ein. Sie erhalten dann die gleiche Antwort für den Basisbot, die Sie auch mit **Test in Web Chat** (Test im Webchat) erhalten haben.

    [![Antwort des Basisbots im Emulator](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Stellen einer Frage zur Absicht „Flugbuchung“ im Bot

1. Buchen Sie im Bot-Emulator einen Flug, indem Sie die folgende Äußerung eingeben: 

    ```console
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    Im Bot-Emulator werden Sie zur Bestätigung aufgefordert. 

1. Wählen Sie **Ja** aus. Der Bot antwortet mit einer Zusammenfassung seiner Aktionen. 
1. Wählen Sie im Protokoll des Bot-Emulators die Zeile mit `Luis Trace` aus. Dadurch wird die JSON-Antwort von LUIS für die Absicht und die Entitäten der Äußerung angezeigt.

    [![Antwort des Basisbots im Emulator](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich weitere [Beispiele](https://github.com/microsoft/botframework-solutions) mit Konversationsbots an. 

> [!div class="nextstepaction"]
> [Erstellen einer Language Understanding-App mit einer benutzerdefinierten Motivdomäne](luis-quickstart-intents-only.md)
