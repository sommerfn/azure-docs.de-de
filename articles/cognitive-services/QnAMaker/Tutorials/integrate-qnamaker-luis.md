---
title: 'LUIS und QnAMaker: Botintegration'
titleSuffix: Azure Cognitive Services
description: Wenn Ihre QnA Maker-Wissensdatenbank eine bestimmte Größe erreicht, wird es schwierig, sie als einzelnen monolithischen Satz zu warten, und es zeigt sich die Notwendigkeit, die Wissensdatenbank in kleinere logische Abschnitte aufzuteilen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 09/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 7e1ea234bde96ce84259841bbc592bf6373bc639
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802810"
---
# <a name="use-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Verwenden des Bots mit QnA Maker und LUIS zum Verteilen Ihrer Wissensdatenbank
Wenn Ihre QnA Maker-Wissensdatenbank eine bestimmte Größe erreicht, wird es schwierig, sie als einzelnen monolithischen Satz zu warten, und es zeigt sich die Notwendigkeit, die Wissensdatenbank in kleinere logische Abschnitte aufzuteilen.

Zwar ist es ein geradliniger Vorgang, mehrere Wissensdatenbanken in QnA Maker zu erstellen, Sie benötigen jedoch geeignete Logik, um die eingehende Frage an die passende Wissensdatenbank weiterzuleiten. Das können Sie mithilfe von LUIS erledigen.

In diesem Artikel wird das Bot Framework v3 SDK verwendet. Wenn Sie sich für die Bot Framework v4 SDK-Version dieser Informationen interessieren, lesen Sie diesen [Artikel zu Bot Framework](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp).

## <a name="architecture"></a>Architecture

![QnA Maker mit Language Understanding-Architektur](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Im Szenario oben ruft QnA Maker zuerst die Absicht der eingehenden Frage bei einem LUIS-Modell ab und verwendet diese anschließend, um die Frage an die richtige QnA Maker-Wissensdatenbank weiterzuleiten.

## <a name="create-a-luis-app"></a>Erstellen einer LUIS-App

1. Melden Sie sich beim [LUIS](https://www.luis.ai/)-Portal an.
1. [Erstellen Sie eine App](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
1. [Fügen Sie eine Absicht](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) für jede QnA Maker-Wissensdatenbank hinzu. Die exemplarischen Äußerungen sollten den Fragen in den QnA Maker-Wissensdatenbanken entsprechen.
1. [Trainieren der LUIS-App](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) und [Veröffentlichen der LUIS-App](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp).
1. Notieren Sie sich im Abschnitt **Verwalten** Ihre LUIS-App-ID, den LUIS-Endpunktschlüssel und den [benutzerdefinierten Domänennamen](../../cognitive-services-custom-subdomains.md). Diese Werte werden Sie später benötigen. 

## <a name="create-qna-maker-knowledge-bases"></a>Erstellen von QnA Maker-Wissensdatenbanken

1. Melden Sie sich bei [QnA Maker](https://qnamaker.ai) an.
1. [Erstellen](https://www.qnamaker.ai/Create) Sie eine Wissensdatenbank für jede Absicht in der LUIS-App.
1. Testen und veröffentlichen Sie die Wissensdatenbanken. Wenn Sie die einzelnen Wissensdatenbanken veröffentlichen, notieren Sie sich die zugehörige ID, den Ressourcennamen (benutzerdefinierte Unterdomäne vor _.azurewebsites.net/qnamaker_) und den Schlüssel für den Autorisierungsendpunkt. Diese Werte werden Sie später benötigen. 

    In diesem Artikel wird davon ausgegangen, dass die Wissensdatenbanken alle in demselben Azure QnA Maker-Abonnement erstellt wurden.

    ![QnA Maker-HTTP-Anforderung](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Web-App-Bot

1. [Erstellen Sie einen „einfachen“ Web-App-Bot](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0), der automatisch eine LUIS-App enthält. Wählen Sie die Programmiersprache C# aus.

1. Nachdem der Web-App-Bot erstellt wurde, wählen Sie im Azure-Portal den Web-App-Bot aus.
1. Wählen Sie **Anwendungseinstellungen** in der Dienstnavigation des Web-App-Bots, und scrollen Sie dann nach unten zum Abschnitt **Anwendungseinstellungen** der verfügbaren Einstellungen.
1. Ändern Sie die **LuisAppId** in den Wert der LUIS-App, die im vorhergehenden Abschnitt erstellt wurde, und wählen Sie dann **Speichern** aus.


## <a name="change-code-in-basicluisdialogcs"></a>Ändern des Codes in BasicLuisDialog.cs
1. Wählen Sie im Abschnitt **Bot Management** (Botverwaltung) der Web-App-Bot-Navigation im Azure-Portal **Build** (Erstellen) aus.
2. Wählen Sie **Open online code editor** (Onlinecode-Editor öffnen) aus. Es wird eine neue Browser-Registerkarte mit der Onlinebearbeitungsumgebung geöffnet. 
3. Wählen Sie im Abschnitt **WWWWROOT** das Verzeichnis **Dialoge** aus und öffnen Sie dann **BasicLuisDialog.cs**.
4. Fügen Sie Abhängigkeiten am Anfang der Datei **BasicLuisDialog.cs** hinzu:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Luis;
    using Microsoft.Bot.Builder.Luis.Models;
    using Newtonsoft.Json;
    using System.Text;
    ```

5. Fügen Sie die folgenden Klassen hinzu, um die Antwort von QnA Maker zu deserialisieren:

    ```csharp
    public class Metadata
    {
        public string name { get; set; }
        public string value { get; set; }
    }

    public class Answer
    {
        public IList<string> questions { get; set; }
        public string answer { get; set; }
        public double score { get; set; }
        public int id { get; set; }
        public string source { get; set; }
        public IList<object> keywords { get; set; }
        public IList<Metadata> metadata { get; set; }
    }

    public class QnAAnswer
    {
        public IList<Answer> answers { get; set; }
    }
    ```


6. Fügen Sie die folgende Klasse hinzu, um eine HTTP-Anfrage an den QnA Maker-Dienst zu stellen. Beachten Sie, dass der Headerwert von **Autorisierung** das Wort `EndpointKey` mit einem Leerzeichen hinter dem Wort enthält. Das JSON-Ergebnis wird in die vorhergehenden Klassen deserialisiert und die erste Antwort wird zurückgegeben.

    ```csharp
    [Serializable]
    public class QnAMakerService
    {
        private string qnaServiceResourceName;
        private string knowledgeBaseId;
        private string endpointKey;

        public QnAMakerService(string resourceName, string kbId, string endpointkey)
        {
            qnaServiceResourceName = resourceName;
            knowledgeBaseId = kbId;
            endpointKey = endpointkey;

        }
        async Task<string> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Authorization", "EndpointKey " + endpointKey);

                var response = await client.SendAsync(request);
                return  await response.Content.ReadAsStringAsync();
            }
        }
        public async Task<string> GetAnswer(string question)
        {
            string uri = qnaServiceResourceName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer";
            string questionJSON = "{\"question\": \"" + question.Replace("\"","'") +  "\"}";

            var response = await Post(uri, questionJSON);

            var answers = JsonConvert.DeserializeObject<QnAAnswer>(response);
            if (answers.answers.Count > 0)
            {
                return answers.answers[0].answer;
            }
            else
            {
                return "No good match found.";
            }
        }
    }
    ```


7. Ändern Sie die BasicLuisDialog-Klasse. Jede LUIS-Absicht sollte eine mit **LuisIntent** dekorierte Methode aufweisen. Der Parameter für die Dekoration ist der eigentliche LUIS-Absichtsname. Der Name der dekorierten Methode _sollte_ für Lesbarkeit und Wartbarkeit der LUIS-Absichtsname sein. Dieser muss aber zur Entwurfs- oder Laufzeit nicht derselbe sein.  

    ```csharp
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        // LUIS Settings
        static string LUIS_appId = "<LUIS APP ID>";
        static string LUIS_apiKey = "<LUIS API KEY>";
        static string LUIS_hostRegion = "westus.api.cognitive.microsoft.com";

        // QnA Maker global settings
        // assumes all KBs are created with same Azure service
        static string qnamaker_endpointKey = "<QnA Maker endpoint KEY>";
        static string qnamaker_resourceName = "my-qnamaker-s0-s";
        
        // QnA Maker Human Resources Knowledge base
        static string HR_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // QnA Maker Finance Knowledge base
        static string Finance_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // Instantiate the knowledge bases
        public QnAMakerService hrQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", HR_kbID, qnamaker_endpointKey);
        public QnAMakerService financeQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", Finance_kbID, qnamaker_endpointKey);

        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            LUIS_appId,
            LUIS_apiKey,
            domain: LUIS_hostRegion)))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            HttpClient client = new HttpClient();
            await this.ShowLuisResult(context, result);
        }

        // HR Intent
        [LuisIntent("HR")]
        public async Task HumanResourcesIntent(IDialogContext context, LuisResult result)
        {
            // Ask the HR knowledge base
            var qnaMakerAnswer = await hrQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }

        // Finance intent
        [LuisIntent("Finance")]
        public async Task FinanceIntent(IDialogContext context, LuisResult result)
        {
            // Ask the finance knowledge base
            var qnaMakerAnswer = await financeQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }
        private async Task ShowLuisResult(IDialogContext context, LuisResult result)
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
    }
    ```


## <a name="build-the-bot"></a>Erstellen des Bots
1. Klicken Sie im Code-Editor mit der rechten Maustaste auf `build.cmd`, und wählen Sie **Run from Console** (In Konsole ausführen) aus.

    ![run from console (In Konsole ausführen)](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Die Codeansicht wird durch ein Terminalfenster ersetzt, in dem der Fortschritt und die Ergebnisse des Builds angezeigt werden.

    ![console build (Build in der Konsole)](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Testen des Bots
Wählen Sie im Azure-Portal **Test in Web Chat** (Im Webchat testen) aus, um den Bot zu testen. Geben Sie Nachrichten aus verschiedenen Absichten ein, um die Antwort von der entsprechenden Wissensdatenbank zu erhalten.

![Webchattest](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Geschäftskontinuitätsplans für QnA Maker](../How-To/business-continuity-plan.md)
