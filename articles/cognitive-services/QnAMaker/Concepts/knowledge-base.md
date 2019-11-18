---
title: 'Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 355556e98300ecad6aa3141f0f4ab14b834cd91e
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794898"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Was ist eine QnA Maker-Wissensdatenbank?

Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.

## <a name="key-knowledge-base-concepts"></a>Wichtige Konzepte für Wissensdatenbanken

* **Fragen**: Eine Frage enthält Text, der eine Benutzerabfrage bestmöglich darstellt. 
* **Antworten**: Eine Antwort ist die Reaktion, die zurückgegeben wird, wenn eine Benutzerabfrage mit der zugeordneten Frage übereinstimmt.  
* **Metadaten**: Metadaten sind Tags in Form von Schlüssel-Wert-Paaren, die einem QnA-Paar zugeordnet sind. Metadatentags werden verwendet, um QnA-Paare zu filtern und die Datenmenge einzuschränken, für die ein Abfrageabgleich vorgenommen wird.

Eine einzelnes QnA-Element, dargestellt durch eine numerische QnA-ID, verfügt über mehrere Varianten einer Frage (alternative Fragen), die alle auf einer einzigen Antwort abgebildet werden. Außerdem können jedem dieser Paare mehrere Metadatenfelder zugeordnet sein: ein Schlüssel und ein Wert.

![QnA Maker-Wissensdatenbanken](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhaltsformat der Wissensdatenbank

Wenn Sie reichhaltige Inhalte in einer Wissensdatenbank erfassen, versucht QnA Maker, die Inhalte in Markdown zu konvertieren. Informationen zu Markdownformaten, die für die meisten Chatclients verständlich sind, finden Sie in [diesem Blog](https://aka.ms/qnamaker-docs-markdown-support).

Metadatenfelder bestehen aus Schlüssel-Wert-Paaren, die durch einen Doppelpunkt getrennt werden, wie z. B. „Produkt:Aktenvernichter“. Schlüssel und Wert dürfen nur aus Text bestehen. Der Metadatenschlüssel darf keine Leerzeichen enthalten. Metadaten unterstützt nur einen Wert pro Schlüssel.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Verarbeitung einer Benutzerabfrage mit QnA Maker, um die beste Antwort auszuwählen

Die trainierte und [veröffentlichte](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) QnA Maker-Wissensdatenbank empfängt eine Benutzerabfrage von einem Bot oder einer anderen Clientanwendung über die [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage). Im folgenden Diagramm ist der Prozess nach dem Empfang der Benutzerabfrage dargestellt.

![Einstufungsprozess für eine Benutzerabfrage](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

### <a name="ranker-process"></a>Einstufungsprozess

Der Prozess wird in der folgenden Tabelle erläutert:

|Schritt|Zweck|
|--|--|
|1|Die Clientanwendung sendet die Benutzerabfrage an die [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage).|
|2|QnA Maker führt die Vorverarbeitung der Benutzerabfrage mit Spracherkennung, Rechtschreibprüfung und Worttrennung durch.|
|3|Diese Vorverarbeitung hat das Ziel, die Benutzerabfrage so anzupassen, dass die besten Suchergebnisse erzielt werden.|
|4|Die geänderte Abfrage wird an den Azure Cognitive Search-Index gesendet, der die mit `top` festgelegte Anzahl von Ergebnissen empfängt. Falls diese Ergebnisse die richtige Antwort nicht enthalten, sollten Sie den Wert von `top` leicht erhöhen. Im Allgemeinen eignet sich der Wert „10“ für `top` bei 90 % aller Abfragen.|
|5|QnA Maker wendet die erweiterte Featurebereitstellung an, um die Richtigkeit der abgerufenen Suchergebnisse für die Benutzerabfrage zu ermitteln. |
|6|Für das trainierte Rangfolgemodell wird die Featurebewertung aus Schritt 5 verwendet, um die Einstufung der Azure Cognitive Search-Ergebnisse vorzunehmen.|
|7|Die neuen Ergebnisse werden in der Rangfolge an die Clientanwendung zurückgegeben.|
|||

Zu den verwendeten Funktionen gehören u. a. Semantik auf Wortebene, Wichtigkeit auf Begriffsebene in einem Korpus und Deep Learning-Semantikmodelle, um die Ähnlichkeit und Relevanz zwischen zwei Textzeichenfolgen zu ermitteln.

## <a name="http-request-and-response-with-endpoint"></a>HTTP-Anforderung und -Antwort mit Endpunkt
Beim Veröffentlichen Ihrer Wissensdatenbank erstellt der Dienst einen REST-basierten HTTP-Endpunkt, den Sie in Ihre Anwendung integrieren können, etwa in Form eines Chatbots. 

### <a name="the-user-query-request-to-generate-an-answer"></a>Benutzerabfrageanforderung zum Generieren einer Antwort

Eine Benutzerabfrage ist die Frage, die der Benutzer an die Wissensdatenbank richtet, z. B. `How do I add a collaborator to my app?`. Die Abfrage wird häufig in einem natürlichen Sprachformat oder in Form von Schlüsselwörtern formuliert, die die Frage darstellen, z. B. `help with collaborators`. Die Abfrage wird von einer HTTP-Anforderung in der Clientanwendung an Ihre Wissensdatenbank gesendet.

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

Sie steuern die Antwort, indem Sie Eigenschaften wie [scoreThreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) und [strictFilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags) festlegen.

Verwenden Sie [Unterhaltungskontext](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) mit [Mehrfachdurchlauffunktion](../how-to/multiturn-conversation.md), um die Konversation aufrechtzuerhalten und die Fragen und Antworten so zu verfeinern, dass die richtige und abschließende Antwort gefunden wird.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Reaktion auf einen Aufruf zum Generieren einer Antwort

Die HTTP-Antwort ist die Antwort, die aus der Wissensdatenbank basierend auf der besten Übereinstimmung für eine bestimmte Benutzerabfrage abgerufen wird. Darin enthalten ist die eigentliche Antwort und das Vorhersageergebnis. Wenn Sie mit der Eigenschaft `top` mehr als eine Top-Antwort angefordert haben, erhalten Sie mehrere Top-Antworten jeweils mit einer Bewertung. 

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

### <a name="test-and-production-knowledge-base"></a>Test- und Produktionsversion der Wissensdatenbank
Eine Wissensdatenbank ist ein Repository mit Fragen und Antworten, die über QnA Maker erstellt, verwaltet und verwendet wird. Jede QnA Maker-Ebene kann für mehrere Wissensdatenbanken verwendet werden.

Eine Wissensdatenbank verfügt über zwei Statuswerte: *Testversion* und *Veröffentlicht*.

Die *Testversion einer Wissensdatenbank* ist die Version, die bearbeitet, gespeichert und auf Genauigkeit und Vollständigkeit der Antworten getestet wird. Änderungen an der Testversion der Wissensdatenbank wirken sich nicht auf die Benutzer Ihrer Anwendung oder Ihres Chatbots aus. Die Testversion der Wissensdatenbank wird in der HTTP-Anforderung als `test` bezeichnet. 

Die *veröffentlichte Wissensdatenbank* ist die Version, die in Ihrem Chatbot oder Ihrer Anwendung verwendet wird. Bei der Veröffentlichung einer Wissensdatenbank wird der Inhalt der Testversion in die veröffentlichte Version der Wissensdatenbank übertragen. Da es sich bei der veröffentlichten Wissensdatenbank um die Version handelt, die von der Anwendung über den Endpunkt verwendet wird, sollten Sie sicherstellen, dass der Inhalt korrekt und ausreichend getestet ist. Die veröffentlichte Wissensdatenbank wird in der HTTP-Anforderung als `prod` bezeichnet.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklungslebenszyklus einer Wissensdatenbank](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

[Übersicht über QnA Maker](../Overview/overview.md)

Informationen zum Erstellen und Bearbeiten einer Wissensdatenbank finden Sie unter: 
* [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Informationen zum Generieren einer Antwort finden Sie unter: 
* [REST-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
