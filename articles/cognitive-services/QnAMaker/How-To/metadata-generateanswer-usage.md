---
title: 'Verwenden von Metadaten mit der GenerateAnswer-API: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker ermöglicht Ihnen das Hinzufügen von Metadaten in Form von Schlüssel-Wert-Paaren zu Ihren Frage-Antwort-Sätzen. Mit diesen Informationen können Sie Ergebnisse nach Benutzerabfragen filtern und zusätzliche Informationen speichern, die in Folgekonversationen verwendet werden können.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: tulasim
ms.openlocfilehash: 2454e07e4fc4600f846acc7afbcc19cc0b677450
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792235"
---
# <a name="get-a-knowledge-answer-with-the-generateanswer-api-and-metadata"></a>Abrufen einer wissensbasierten Antwort mit der GenerateAnswer-API und Metadaten

Um die vorhergesagte Antwort auf die Frage eines Benutzers zu erhalten, verwenden Sie die GenerateAnswer-API. Wenn Sie eine Wissensdatenbank veröffentlichen, werden diese Informationen zur Verwendung dieser API auf der Veröffentlichungsseite angezeigt. Sie können die API auch so konfigurieren, dass sie Antworten anhand von Metadatentags filtert und die Wissensdatenbank vom Endpunkt aus mit dem Testabfrage-Zeichenfolgenparameter testet.

QnA Maker ermöglicht das Hinzufügen von Metadaten in Form von Schlüssel-Wert-Paaren zu Ihren Frage-Antwort-Sätzen. Mit diesen Informationen können Sie Ergebnisse nach Benutzerabfragen filtern und zusätzliche Informationen speichern, die in Folgekonversationen verwendet werden können. Weitere Informationen finden Sie unter [Knowledge Base](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="storing-questions-and-answers-with-a-qna-entity"></a>Speichern von Fragen und Antworten mit einer QnA-Entität

Zunächst ist es wichtig zu verstehen, wie QnA Maker die Frage-Antwort-Daten speichert. Die folgende Abbildung zeigt eine QnA-Entität:

![QnA-Entität](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Jede QnA-Entität hat eine eindeutige und dauerhafte ID. Die ID kann verwendet werden, um bestimmte QnA-Entitäten zu aktualisieren.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>Abrufen von Antwortvorhersagen mit der GenerateAnswer-API

Sie verwenden die GenerateAnswer-API in Ihrem Bot oder Ihrer Anwendung, um die Knowledge Base zu einer Benutzerfrage abzufragen und die beste Übereinstimmung aus den Frage-Antwort-Sätzen zu erhalten.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>Veröffentlichen am GenerateAnswer-Endpunkt 

Nachdem Sie Ihre Knowledge Base im [QnA Maker-Portal](https://www.qnamaker.ai) oder mithilfe der [API](https://go.microsoft.com/fwlink/?linkid=2092179) veröffentlicht haben, können Sie die Details von Ihrem GenerateAnswer-Endpunkt abrufen.

So rufen Sie Endpunktdetails ab
1. Melden Sie sich bei [https://www.qnamaker.ai](https://www.qnamaker.ai) an.
1. Klicken Sie in **My knowledge bases** (Meine Knowledge Bases) für Ihre Knowledge Base auf **Code anzeigen**.
    ![My Knowledge Bases (Meine Knowledge Bases)](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. Rufen Sie Ihre GenerateAnswer-Endpunktdetails ab.

    ![Endpunktdetails](../media/qnamaker-how-to-metadata-usage/view-code.png)

Sie können Ihre Endpunktdetails auch auf der Registerkarte **Einstellungen** Ihrer Knowledge Base abrufen.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>GenerateAnswer-Anforderungskonfiguration

Sie rufen GenerateAnswer über eine HTTP POST-Anforderung auf. Beispielcode, der zeigt, wie GenerateAnswer aufgerufen wird, finden Sie unter [Schnellstarts](../quickstarts/csharp.md).

Die **Anforderungs-URL** weist das folgende Format auf: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

|HTTP-Anforderungseigenschaft|NAME|Type|Zweck|
|--|--|--|--|
|URL-Routenparameter|Wissensdatenbank-ID|Zeichenfolge|GUID der Knowledge Base|
|URL-Routenparameter|QnAMaker-Endpunkthost|Zeichenfolge|Hostname des Endpunkts, der in Ihrem Azure-Abonnement bereitgestellt wurde Dieser Wert ist auf der Seite „Einstellungen“ verfügbar, nachdem Sie die Wissensdatenbank veröffentlicht haben. |
|Header|Content-Typ|Zeichenfolge|Medientyp des an die API gesendeten Texts Der Standardwert ist ``.|
|Header|Autorisierung|Zeichenfolge|Ihr Endpunktschlüssel (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Posttext|JSON-Objekt|JSON|Die Frage mit Einstellungen|


Der JSON-Text verfügt über mehrere Einstellungen:

|JSON-Texteigenschaft|Erforderlich|Type|Zweck|
|--|--|--|--|
|`question`|required|Zeichenfolge|Eine Benutzerfrage, die an die Wissensdatenbank gesendet werden soll.|
|`top`|optional|integer|Anzahl der priorisierten Ergebnisse für die Ausgabe Der Standardwert ist 1.|
|`userId`|optional|Zeichenfolge|Eindeutige ID zur Identifizierung des Benutzers. Diese ID wird in den Chatprotokollen aufgezeichnet.|
|`scoreThreshold`|optional|integer|Es werden nur Antworten mit einer Zuverlässigkeitsbewertung über diesem Schwellenwert zurückgegeben. Der Standardwert ist 0.|
|`isTest`|optional|boolean|Wenn auf TRUE festgelegt, werden Ergebnisse aus dem `testkb`-Suchindex anstatt aus dem veröffentlichten Index zurückgegeben.|
|`strictFilters`|optional|Zeichenfolge|Weist (sofern angegeben) QnA Maker an, nur Antworten mit den angegebenen Metadaten zurückzugeben. Verwenden Sie `none`, um festzulegen, dass die Antwort keine Metadatenfilter enthalten soll. |

Ein JSON-Beispieltext sieht folgendermaßen aus:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>GenerateAnswer-Antworteigenschaften

Eine erfolgreiche Antwort gibt den Status 200 und eine JSON-Antwort zurück. 

|Answers-Eigenschaft (sortiert nach Bewertung)|Zweck|
|--|--|
|Ergebnis Ihrer App|Rangfolgewert zwischen 0 und 100|
|id|Die der Antwort zugewiesene eindeutige ID|
|Fragen|Fragen des Benutzers|
|answer (Annehmen)|Die Antwort auf die Frage|
|source|Name der Quelle, aus der die Antwort extrahiert oder in der Wissensdatenbank gespeichert wurde|
|metadata|Die der Antwort zugeordneten Metadaten|
|metadata.name|Metadatenname (Zeichenfolge, maximale Länge: 100, erforderlich)|
|metadata.value: Metadatenwert (Zeichenfolge, maximale Länge: 100, erforderlich)|


```json
{
    "answers": [
        {
            "score": 28.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
            "metadata": [
                {
                    "name": "category",
                    "value": "api"
                }
            ]
        }
    ]
}
```

<a name="metadata-example"></a>

## <a name="using-metadata-allows-you-to-filter-answers-by-custom-metadata-tags"></a>Mithilfe von Metadaten können Sie Antworten nach benutzerdefinierten Metadatentags filtern.

Durch das Hinzufügen von Metadaten können Sie die Antworten nach diesen Metadatentags filtern. Fügen Sie die Metadatenspalte aus dem Menü **Ansichtsoptionen** hinzu. Fügen Sie Ihrer Wissensdatenbank Metadaten hinzu, indem Sie auf das Metadatensymbol **+** klicken, um ein Metadatenpaar hinzuzufügen. Dieses Paar besteht aus einem Schlüssel und einen Wert.

![Hinzufügen von Metadaten](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Filtern von Ergebnissen mit „strictFilters“ nach Metadatentags

Betrachten Sie die Benutzerfrage „When does this hotel close?“ (Wann schließt dieses Hotel?) mit dem Restaurant „Paradise“ als implizierter Absicht.

Da nur Ergebnisse für das Restaurant „Paradise“ erforderlich sind, können Sie im GenerateAnswer-Aufruf wie folgt einen Filter für die Metadaten zum Restaurantnamen festlegen.

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

<name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Verwenden von Frage und Antwortergebnis zum Beibehalten des Unterhaltungskontexts

Die Antwort auf den GenerateAnswer-Aufruf enthält die entsprechenden Metadateninformationen des übereinstimmenden Frage-Antwort-Satzes. Diese Informationen können in Ihrer Clientanwendung verwendet werden, um den Kontext der vorherigen Unterhaltung für die Verwendung in späteren Unterhaltungen zu speichern. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Die Veröffentlichungsseite enthält auch Informationen zum Generieren einer Antwort mit [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) und [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Erstellen einer Wissensdatenbank](./create-knowledge-base.md)
