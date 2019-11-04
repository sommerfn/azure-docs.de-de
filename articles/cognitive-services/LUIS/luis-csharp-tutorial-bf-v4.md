---
title: 'Tutorial: Language Understanding-Bot C# v4'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie mithilfe von C# einen in LUIS (Language Understanding Intelligent Service) integrierten Chatbot. Der Bot wird mit Bot Framework, Version 4 und dem Azure Web-App-Bot-Dienst erstellt.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 213449a78baf8fc2b7cb6c74709efeaf2a50d5b2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495464"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-c"></a>Tutorial: Verwenden eines Web-App-Bots mit aktiviertem Language Understanding in C#

Erstellen Sie mithilfe von C# einen in Language Understanding (LUIS) integrierten Chatbot. Der Bot wird mit der [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/)-Ressource von Azure und [Bot Framework](https://github.com/Microsoft/botbuilder-dotnet) V4 erstellt.

[!INCLUDE [Waiting for Bot refresh](./includes/wait-bot-upgrade.md)]


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
|GetWeather|`what's the weather like?`|
|Keine|Alles, was sich außerhalb der Domäne der App befindet.|

## <a name="test-the-bot-in-web-chat"></a>Testen des Bots im Webchat

1. Wählen Sie im Azure-Portal für den neuen Bot die Option **Test in Web Chat** (Test im Webchat) aus. 
1. Geben Sie im Textfeld **Nachricht eingeben** den Text `Book a flight from Seattle to Berlin tomorrow` ein. Der Bot antwortet mit der Bestätigung, ob ein Flug gebucht werden soll. 

    ![Screenshot des Azure-Portals, Eingeben des Texts „hello“](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

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

1. Öffnen Sie zum Senden der Benutzeräußerung an den LUIS-Vorhersageendpunkt die Datei **FlightBookingRecognizer.cs**. Das ist die Stelle, an der in den Bot eingegebene Benutzeräußerungen an LUIS gesendet werden. Die Antwort von LUIS wird über die Methode **RecognizeAsync** zurückgegeben.  

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    
    namespace Microsoft.BotBuilderSamples
    {
        public class FlightBookingRecognizer : IRecognizer
        {
            private readonly LuisRecognizer _recognizer;
    
            public FlightBookingRecognizer(IConfiguration configuration)
            {
                var luisIsConfigured = !string.IsNullOrEmpty(configuration["LuisAppId"]) && !string.IsNullOrEmpty(configuration["LuisAPIKey"]) && !string.IsNullOrEmpty(configuration["LuisAPIHostName"]);
                if (luisIsConfigured)
                {
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]);
    
                    _recognizer = new LuisRecognizer(luisApplication);
                }
            }
    
            // Returns true if luis is configured in the appsettings.json and initialized.
            public virtual bool IsConfigured => _recognizer != null;
    
            public virtual async Task<RecognizerResult> RecognizeAsync(ITurnContext turnContext, CancellationToken cancellationToken)
                => await _recognizer.RecognizeAsync(turnContext, cancellationToken);
    
            public virtual async Task<T> RecognizeAsync<T>(ITurnContext turnContext, CancellationToken cancellationToken)
                where T : IRecognizerConvert, new()
                => await _recognizer.RecognizeAsync<T>(turnContext, cancellationToken);
        }
    }
    ````

1. In **Dialogs > MainDialog.cs** wird die Äußerung erfasst und an „executeLuisQuery“ in der Methode „actStep“ gesendet. 

    ```csharp
    public class MainDialog : ComponentDialog
    {
        private readonly FlightBookingRecognizer _luisRecognizer;

        ...

        public MainDialog(FlightBookingRecognizer luisRecognizer, BookingDialog bookingDialog, ILogger<MainDialog> logger)
                    : base(nameof(MainDialog))
        {
            _luisRecognizer = luisRecognizer;
            ...
        }

        private async Task<DialogTurnResult> ActStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
        {
            if (!_luisRecognizer.IsConfigured)
            {
                // LUIS is not configured, we just run the BookingDialog path with an empty BookingDetailsInstance.
                return await stepContext.BeginDialogAsync(nameof(BookingDialog), new BookingDetails(), cancellationToken);
            }

            // Call LUIS and gather any potential booking details. (Note the TurnContext has the response to the prompt.)
            var luisResult = await _luisRecognizer.RecognizeAsync<FlightBooking>(stepContext.Context, cancellationToken);
            switch (luisResult.TopIntent().intent)
            {
                case FlightBooking.Intent.BookFlight:
                    await ShowWarningForUnsupportedCities(stepContext.Context, luisResult, cancellationToken);

                    // Initialize BookingDetails with any entities we may have found in the response.
                    var bookingDetails = new BookingDetails()
                    {
                        // Get destination and origin from the composite entities arrays.
                        Destination = luisResult.ToEntities.Airport,
                        Origin = luisResult.FromEntities.Airport,
                        TravelDate = luisResult.TravelDate,
                    };

                    // Run the BookingDialog giving it whatever details we have from the LUIS call, it will fill out the remainder.
                    return await stepContext.BeginDialogAsync(nameof(BookingDialog), bookingDetails, cancellationToken);

                case FlightBooking.Intent.GetWeather:
                    // We haven't implemented the GetWeatherDialog so we just display a TODO message.
                    var getWeatherMessageText = "TODO: get weather flow here";
                    var getWeatherMessage = MessageFactory.Text(getWeatherMessageText, getWeatherMessageText, InputHints.IgnoringInput);
                    await stepContext.Context.SendActivityAsync(getWeatherMessage, cancellationToken);
                    break;

                default:
                    // Catch all for unhandled intents
                    var didntUnderstandMessageText = $"Sorry, I didn't get that. Please try asking in a different way (intent was {luisResult.TopIntent().intent})";
                    var didntUnderstandMessage = MessageFactory.Text(didntUnderstandMessageText, didntUnderstandMessageText, InputHints.IgnoringInput);
                    await stepContext.Context.SendActivityAsync(didntUnderstandMessage, cancellationToken);
                    break;
            }

            return await stepContext.NextAsync(null, cancellationToken);
        }
        
        ...

    }
    ```

## <a name="start-the-bot-code-in-visual-studio"></a>Starten des Botcodes in Visual Studio

Starten Sie den Bot in Visual Studio 2019. Ein Browserfenster mit der Website des Web-App-Bots wird unter der Adresse `http://localhost:3978/` geöffnet. Auf einer Startseite werden Informationen zu dem Bot angezeigt.

![Auf einer Startseite werden Informationen zu dem Bot angezeigt.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Verwenden des Bot-Emulators zum Testen des Bots

1. Starten Sie den Bot-Emulator, und wählen Sie **Open Bot** (Bot öffnen) aus.
1. Geben Sie im Popupdialogfeld **Open a bot** (Bot öffnen) die URL des Bots ein, z. B. `http://localhost:3978/api/messages`. Die Route `/api/messages` ist die Webadresse für den Bot.
1. Geben Sie die **Microsoft-App-ID** und das **Microsoft-App-Kennwort** ein, die sich in der Datei **appsettings.json** im Stammverzeichnis des heruntergeladenen Botcodes befinden.


1. Geben Sie im Bot-Emulator `Book a flight from Seattle to Berlin tomorrow` ein. Sie erhalten dann die gleiche Antwort für den Basisbot, die Sie auch mit **Test in Web Chat** (Test im Webchat) erhalten haben.

    [![Antwort des Basisbots im Emulator](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. Wählen Sie **Ja** aus. Der Bot antwortet mit einer Zusammenfassung seiner Aktionen. 
1. Wählen Sie im Protokoll des Bot-Emulators die Zeile mit `Luis Trace` aus. Dadurch wird die JSON-Antwort von LUIS für die Absicht und die Entitäten der Äußerung angezeigt.

    [![Antwort des Basisbots im Emulator](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich weitere [Beispiele](https://github.com/microsoft/botframework-solutions) mit Konversationsbots an. 

> [!div class="nextstepaction"]
> [Erstellen einer Language Understanding-App mit einer benutzerdefinierten Motivdomäne](luis-quickstart-intents-only.md)
