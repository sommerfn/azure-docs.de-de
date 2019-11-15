---
title: Verbessern der Wissensdatenbank – QnA Maker
titleSuffix: Azure Cognitive Services
description: Nutzen Sie aktives Lernen, um die Qualität Ihrer Wissensdatenbank zu verbessern. Sie können Einträge überprüfen, annehmen oder ablehnen und hinzufügen, ohne vorhandene Fragen zu entfernen oder zu ändern.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: add4bbead880fb9b74d342abc1d4b3c0e9475fad
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721180"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Nutzen des aktiven Lernens zum Verbessern Ihrer Wissensdatenbank

Durch aktives Lernen lässt sich die Qualität Ihrer Wissensdatenbank verbessern, indem auf der Grundlage von Benutzerbeiträgen alternative Fragen zu Ihrem Frage-Antwort-Paar vorgeschlagen werden. Sie überprüfen diese Vorschläge, indem Sie sie entweder zu vorhandenen Fragen hinzufügen oder ablehnen. 

Ihre Wissensdatenbank wird nicht automatisch geändert. Damit Änderungen wirksam werden, müssen Sie die Vorschläge akzeptieren. Mit diesen Vorschlägen werden Fragen hinzugefügt, vorhandene Fragen werden aber weder geändert noch entfernt.

## <a name="what-is-active-learning"></a>Was ist aktives Lernen?

QnA Maker erlernt neue Fragevarianten über implizites und explizites Feedback.
 
* [Implizites Feedback:](#how-qna-makers-implicit-feedback-works) Die Bewertung erkennt, wenn für eine Benutzerfrage mehrere Antworten vorliegen, deren Bewertungen sehr nah beieinander liegen, und betrachtet dies als Feedback. Hierzu müssen keinerlei Aktionen ausgeführt werden.
* [Explizites Feedback:](#how-you-give-explicit-feedback-with-the-train-api) Wenn von der Wissensdatenbank mehrere Antworten zurückgegeben werden, deren Bewertungen sich kaum unterscheiden, fordert die Clientanwendung den Benutzer auf, die richtige Frage auszuwählen. Das explizite Feedback des Benutzers wird mit der [Trainings-API](#train-api) an QnA Maker gesendet. 

Mit beiden Methoden werden ähnliche (gruppierte) Abfragen für die Bewertung bereitgestellt.

## <a name="how-active-learning-works"></a>Aktives Lernen: Funktionsweise

Aktives Lernen wird basierend auf den Bewertungen der relevantesten Antworten ausgelöst, die von QnA Maker zurückgegeben werden. Liegt der Bewertungsunterschied innerhalb einer geringen Bandbreite, wird die Abfrage als möglicher Vorschlag (alternative Frage) für jedes der möglichen QnA-Paare betrachtet. Nachdem Sie die vorgeschlagene Frage für ein bestimmtes QnA-Paar akzeptiert haben, wird sie für die anderen Paare abgelehnt. Denken Sie daran, nach dem Akzeptieren von Vorschlägen zu speichern und ein Training auszuführen.

Das Feature für aktives Lernen liefert die bestmöglichen Vorschläge in Fällen, in denen die Endpunkte eine angemessene Anzahl und Vielfalt von Nutzungsabfragen erhalten. Wenn mindestens 5 ähnliche Abfragen gruppiert werden, sendet QnA Maker alle 30 Minuten die benutzerbasierten Fragen als Vorschläge an den Entwickler der Wissensdatenbank, um diese zu akzeptieren oder abzulehnen. Alle Vorschläge werden nach Ähnlichkeit gruppiert, und die besten Vorschläge für alternative Fragen werden basierend auf der Häufigkeit der jeweiligen Abfragen von Endbenutzern angezeigt.

Sobald Fragen im QnA Maker-Portal vorgeschlagen werden, müssen Sie diese überprüfen und akzeptieren oder ablehnen. Es gibt keine API zum Verwalten von Vorschlägen.

## <a name="how-qna-makers-implicit-feedback-works"></a>Funktionsweise des impliziten Feedbacks von QnA Maker

Beim impliziten Feedback von QnA Maker wird ein Algorithmus verwendet, um die Nähe von Bewertungen zu ermitteln und anschließend Vorschläge für aktives Lernen bereitzustellen. Der Algorithmus zur Bestimmung der Nähe ist keine einfache Berechnung. Die Bereiche im folgenden Beispiel sind keine statischen Werte, sondern dienen lediglich als Orientierungshilfe, um die Auswirkungen des Algorithmus nachvollziehen zu können.

Wenn die Bewertung einer Frage eine hohe Zuverlässigkeit aufweist, wie z.B. 80%, ist die Bandbreite der für das aktive Lernen in Betracht gezogenen Bewertungen mit etwa 10% recht groß. Wenn der Zuverlässigkeitswert einer Bewertung sinkt, z.B. auf 40%, nimmt auch die Bandbreite der Bewertungen ab und liegt nur etwa innerhalb von 4%. 

## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Abgeben von explizitem Feedback mit der Trainings-API

Es ist wichtig, dass QnA Maker explizites Feedback dazu erhält, welche der Antworten die beste Antwort war. Wie die beste Antwort bestimmt wird, liegt bei Ihnen. Mögliche Optionen sind beispielsweise:

* Benutzerfeedback (Auswählen einer der Antworten)
* Geschäftslogik (etwa Bestimmung eines akzeptablen Bewertungsbereichs)  
* Kombination aus Benutzerfeedback und Geschäftslogik

## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Upgraden Ihrer Laufzeitversion, um aktives Lernen zu nutzen

Das Feature für aktives Lernen wird in der Runtimeversion 4.4.0 und höher unterstützt. Wenn Ihre Wissensdatenbank mit einer Vorgängerversion erstellt wurde, [führen Sie ein Upgrade Ihrer Runtime durch](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates), um dieses Feature zu nutzen. 

## <a name="turn-on-active-learning-to-see-suggestions"></a>Aktivieren des aktiven Lernens, um Vorschläge zu erhalten

Das Feature für aktives Lernen ist standardmäßig deaktiviert. Aktivieren Sie das Feature, um die vorgeschlagenen Fragen anzuzeigen. Nachdem Sie aktives Lernen aktiviert haben, müssen Sie Informationen aus der Client-App an QnA Maker senden. Weitere Informationen finden Sie unter [Architekturfluss für die Verwendung der GenerateAnswer- und der Trainings-API über einen Bot](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Wählen Sie **Veröffentlichen** aus, um die Wissensdatenbank zu veröffentlichen. Aktive Lernabfragen werden nur vom Vorhersageendpunkt der GenerateAnswer-API erfasst. Die Abfragen im Bereich „Test“ im QnA Maker-Portal besitzen keine Auswirkungen auf das aktive Lernen.

1. Wenn Sie aktives Lernen im QnA Maker-Portal aktivieren möchten, wählen Sie in der rechten oberen Ecke Ihren **Namen** aus, und navigieren Sie zu [**Diensteinstellungen**](https://www.qnamaker.ai/UserSettings).  

    ![Aktivieren Sie auf der Seite „Diensteinstellungen“ Alternativen für die vom Feature für aktives Lernen vorgeschlagenen Fragen. Wählen Sie im Menü rechts oben Ihren Benutzernamen und anschließend „Diensteinstellungen“ aus.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Suchen Sie nach dem QnA Maker-Dienst, und aktivieren Sie die Option **Active Learning**. 

    [![Aktivieren Sie auf der Seite „Diensteinstellungen“ das Feature für aktives Lernen. Wenn Sie das Feature nicht aktivieren können, müssen Sie möglicherweise ein Upgrade des Diensts ausführen.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > Die genaue Version auf der vorherigen Abbildung ist nur als Beispiel dargestellt. Ihre Version kann abweichen. 

    Nachdem Sie **Aktives Lernen** aktiviert haben, schlägt die Wissensdatenbank basierend auf den von Benutzern übermittelten Fragen in regelmäßigen Abständen neue Fragen vor. Sie können **Active Learning** deaktivieren, indem Sie die Einstellung wieder umschalten.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Akzeptieren eines Vorschlags des aktiven Lernens in der Wissensdatenbank

Aktives Lernen ändert die Wissensdatenbank oder den Suchdienst, nachdem Sie den Vorschlag bestätigt und dann gespeichert und trainiert haben. Wenn Sie den Vorschlag bestätigen, wird er als alternative Frage hinzugefügt.

1. Um die vorgeschlagenen Fragen anzuzeigen, wählen Sie auf der Seite **Bearbeiten** der Wissensdatenbank **Optionen anzeigen** und danach **Show active learning suggestions** (Vorschläge für aktives Lernen anzeigen) aus. 

    [![Wählen Sie im Bereich „Bearbeiten“ des Portals „Vorschläge anzeigen“ aus, um die Alternativen für neue Fragen des Features für aktives Lernen anzuzeigen.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filtern Sie die Wissensdatenbank nach Frage-Antwort-Paaren, um nur Vorschläge anzuzeigen, indem Sie **Nach Vorschlägen filtern** auswählen.

    [![Verwenden Sie den Umschalter „Nach Vorschlägen filtern“, um nur die vom Feature für aktives Lernen vorgeschlagenen Alternativen für Fragen anzuzeigen.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Bei den einzelnen QnA-Paaren stehen für die vorgeschlagenen neuen Fragealternativen jeweils ein Häkchen (`✔`) zum Akzeptieren der Frage und ein Kreuz (`x`) zum Ablehnen der Vorschläge zur Verfügung. Aktivieren Sie das Häkchen, um die Frage hinzuzufügen. 

    [![Sie können die vorgeschlagenen die vom Feature für aktives Lernen vorgeschlagenen Alternativen für Fragen auswählen oder ablehnen, indem Sie auf das grüne Häkchen oder das rote Löschsymbol klicken.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Sie können _alle Vorschläge_ hinzufügen oder löschen, indem Sie auf der kontextbezogenen Symbolleiste **Alle hinzufügen** oder **Alle ablehnen** auswählen.

1. Wählen Sie **Speichern und trainieren** aus, um die Änderungen an der Wissensdatenbank zu speichern.

1. Wählen Sie **Veröffentlichen** aus, damit die Änderungen über die [GenerateAnswer-API](metadata-generateanswer-usage.md#generateanswer-request-configuration) zur Verfügung stehen.

    Wenn mindestens fünf ähnliche Abfragen gruppiert wurden, schlägt Ihnen QnA Maker alle 30 Minuten die alternativen Fragen vor, die Sie dann akzeptieren oder ablehnen können.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Architekturfluss für die Verwendung der GenerateAnswer- und der Trainings-API über einen Bot

Von einem Bot oder einer anderen Clientanwendung muss zur Verwendung des aktiven Lernens der folgende Architekturfluss verwendet werden:

* Der Bot [ruft die Antwort aus der Wissensdatenbank ab](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) (mit der GenerateAnswer-API) und verwendet dabei die Eigenschaft `top`, um eine Reihe von Antworten zu erhalten.
* Der Bot bestimmt explizites Feedback:
    * Niedrige Bewertungen werden mit Ihrer eigenen [benutzerdefinierten Geschäftslogik](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user) herausgefiltert.
    * Im Bot oder in der Clientanwendung wird dem Benutzer eine Liste mit möglichen Antworten angezeigt und die vom Benutzer ausgewählte Antwort abgerufen.
* Der Bot [sendet die ausgewählte Antwort an QnA Maker zurück](#bot-framework-sample-code) (mit der [Trainings-API](#train-api)).


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Verwenden der Eigenschaft „top“ in der GenerateAnswer-Anforderung, um mehrere passende Antworten abzurufen

Wenn Sie eine Frage zur Beantwortung an QnA Maker senden, legt die Eigenschaft `top` im JSON-Text die Anzahl der zurückzugebenden Antworten fest. 

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Verwenden der Eigenschaft „score“ in Kombination mit Geschäftslogik, um eine Liste mit Antworten abzurufen, die dem Benutzer angezeigt werden kann

Wenn die Clientanwendung (beispielsweise ein Chatbot) die Antwort empfängt, werden die drei relevantesten Fragen zurückgegeben. Verwenden Sie die Eigenschaft `score`, um die Nähe zwischen den Bewertungen zu analysieren. Dieser Nähebereich wird durch Ihre eigene Geschäftslogik bestimmt. 

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Nachverfolgung durch die Clientanwendung bei Fragen mit ähnlicher Bewertung

Ihre Clientanwendung zeigt die Fragen an und gibt dem Benutzer die Möglichkeit, _die am besten geeignete Frage_ auszuwählen. 

Sobald der Benutzer eine der vorhandenen Fragen ausgewählt hat, sendet die Clientanwendung die Auswahl des Benutzers als Feedback mithilfe der Trainings-API von QnA Maker. Dieses Feedback vervollständigt die Feedbackschleifen des Features für aktives Lernen. 

## <a name="train-api"></a>Trainings-API

Feedback zum Feature für aktives Lernen wird über eine POST-Anforderung der Trainings-API an QnA Maker gesendet. Die API-Signatur ist wie folgt:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|HTTP-Anforderungseigenschaft|NAME|type|Zweck|
|--|--|--|--|
|URL-Routenparameter|Wissensdatenbank-ID|Zeichenfolge|GUID der Knowledge Base|
|Benutzerdefinierte Unterdomäne|Name der QnAMaker-Ressource|Zeichenfolge|Der Ressourcenname wird als benutzerdefinierte Unterdomäne für Ihre QnA Maker verwendet. Dieser Wert ist auf der Seite „Einstellungen“ verfügbar, nachdem Sie die Wissensdatenbank veröffentlicht haben. Sie wird als `host` aufgeführt.|
|Header|Content-Type|Zeichenfolge|Medientyp des an die API gesendeten Texts Der Standardwert ist: `application/json`|
|Header|Authorization|Zeichenfolge|Ihr Endpunktschlüssel (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Posttext|JSON-Objekt|JSON|Das Feedback zum Training|

Der JSON-Text verfügt über mehrere Einstellungen:

|JSON-Texteigenschaft|type|Zweck|
|--|--|--|--|
|`feedbackRecords`|array|Liste des Feedbacks|
|`userId`|Zeichenfolge|Die Benutzer-ID der Person, die die vorgeschlagenen Fragen akzeptiert. Das Format der Benutzer-ID bleibt Ihnen überlassen. Beispielsweise kann eine E-Mail-Adresse eine in Ihrer Architektur gültige Benutzer-ID sein. Optional.|
|`userQuestion`|Zeichenfolge|Exakter Text der Benutzerabfrage. Erforderlich.|
|`qnaID`|number|Die ID der Frage in der [GenerateAnswer-Antwort](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Ein JSON-Beispieltext sieht folgendermaßen aus:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Eine erfolgreiche Antwort gibt den Status 204 und einen JSON-Antworttext zurück. 

### <a name="batch-many-feedback-records-into-a-single-call"></a>Zusammenfassen zahlreicher Feedbackdatensätze in einem einzelnen Aufruf

Sie können die Daten in der clientseitigen Anwendung (etwa in einem Bot) speichern und dann mithilfe des Arrays `feedbackRecords` zahlreiche Datensätze in einem einzelnen JSON-Text senden. 

Ein JSON-Beispieltext sieht folgendermaßen aus:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Bot Framework-Beispielcode

Ihr Bot Framework-Code muss die Trainings-API aufrufen, wenn die Abfrage des Benutzers für aktives Lernen verwendet werden soll. Es müssen zwei Codekomponenten geschrieben werden:

* Bestimmen, ob die Abfrage für aktives Lernen verwendet werden soll
* Zurücksenden der Abfrage an die Trainings-API für aktives Lernen von QnA Maker

Im [Azure-Botbeispiel](https://aka.ms/activelearningsamplebot) wurden beide Aktivitäten programmiert. 

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>C#-Beispielcode für die Trainings-API mit Bot Framework 4.x

Der folgende Code veranschaulicht das Zurücksenden von Informationen an QnA Maker mithilfe der Trainings-API. Das [vollständige Codebeispiel](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) finden Sie auf GitHub.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="endpoint">Endpoint URI of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string endpoint, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = endpoint + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Node.js-Beispielcode für die Trainings-API mit Bot Framework 4.x 

Der folgende Code veranschaulicht das Zurücksenden von Informationen an QnA Maker mithilfe der Trainings-API. Das [vollständige Codebeispiel](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) finden Sie auf GitHub.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);
            
            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Aktives Lernen wird in der exportierten Wissensdatenbank gespeichert.

Wenn aktives Lernen für Ihre App aktiviert ist Sie die App exportieren, enthält die Spalte `SuggestedQuestions` in der TSV-Datei die Daten für aktives Lernen. 

Die Spalte `SuggestedQuestions` ist ein JSON-Objekt mit Informationen zu implizitem (`autosuggested`) und explizitem (`usersuggested`) Feedback. Ein Beispiel für dieses JSON-Objekt für eine einzelne, vom Benutzer übermittelte Bitte um `help` ist:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Sie können auch die API zum Herunterladen von Änderungen verwenden, um diese Änderungen mithilfe von REST oder eines der sprachbasierten SDKs zu überprüfen:
* [REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


Wenn Sie diese App erneut importieren, erfasst das aktive Lernen weiterhin Informationen und empfiehlt Vorschläge für Ihre Wissensdatenbank. 



## <a name="best-practices"></a>Bewährte Methoden

Bewährte Methoden bei der Verwendung des Features für aktives Lernen finden Sie unter [Best Practices](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>Nächste Schritte
 
> [!div class="nextstepaction"]
> [Verwenden von Metadaten mit der GenerateAnswer-API](metadata-generateanswer-usage.md)
