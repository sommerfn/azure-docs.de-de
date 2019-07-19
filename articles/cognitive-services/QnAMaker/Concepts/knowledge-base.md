---
title: 'Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 7e40af9b2362ee52a1d00f29cdc112d3c2b9a842
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565847"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>Was ist eine QnA Maker-Wissensdatenbank?

Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Paaren (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.

## <a name="key-knowledge-base-concepts"></a>Wichtige Konzepte für Wissensdatenbanken

* **Fragen**: Eine Frage enthält Text, der eine Benutzerabfrage am besten darstellt. 
* **Antworten**: Eine Antwort ist die Reaktion, die zurückgegeben wird, wenn eine Benutzerabfrage mit der zugehörigen Frage übereinstimmt.  
* **Metadaten**: Metadaten sind Tags, die einem QnA-Paar zugeordnet sind und als Schlüssel-Wert-Paare dargestellt werden. Metadatentags werden verwendet, um QnA-Paare zu filtern und die Datenmenge einzuschränken, für die ein Abfrageabgleich vorgenommen wird.

Eine einzelnes QnA-Element, dargestellt durch eine numerische QnA-ID, verfügt über mehrere Varianten einer Frage (alternative Fragen), die alle auf einer einzigen Antwort abgebildet werden. Außerdem können jedem dieser Paare mehrere Metadatenfelder zugeordnet sein: ein Schlüssel und ein Wert.

![QnA Maker-Wissensdatenbanken](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Inhaltsformat der Wissensdatenbank

Wenn Sie reichhaltige Inhalte in einer Wissensdatenbank erfassen, versucht QnA Maker, die Inhalte in Markdown zu konvertieren. Lesen Sie [diesen](https://aka.ms/qnamaker-docs-markdown-support) Blog, um die Markdownformate zu verstehen, die von den meisten Chatclients verstanden werden.

Metadatenfelder bestehen aus Schlüssel-Wert-Paaren, die durch einen Doppelpunkt **(Produkt:Aktenvernichter)** getrennt werden. Schlüssel und Wert müssen ausschließlich aus Text bestehen. Der Metadatenschlüssel darf keine Leerzeichen enthalten. Metadaten unterstützt nur einen Wert pro Schlüssel.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Verarbeitung einer Benutzerabfrage mit QnA Maker, um die beste Antwort auszuwählen

Die trainierte und [veröffentlichte](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) QnA Maker-Wissensdatenbank empfängt eine Benutzerabfrage von einem Bot oder einer anderen Clientanwendung über die [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage). Im folgenden Diagramm ist der Prozess nach dem Empfang der Benutzerabfrage dargestellt.

![Einstufungsprozess für eine Benutzerabfrage](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

Der Prozess wird in der folgenden Tabelle erläutert:

|Schritt|Zweck|
|--|--|
|1|Die Clientanwendung sendet die Benutzerabfrage an die [GenerateAnswer-API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage).|
|2|QnA Maker führt die Vorverarbeitung der Benutzerabfrage mit Spracherkennung, Rechtschreibprüfung und Worttrennung durch.|
|3|Diese Vorverarbeitung erfolgt, um die Benutzerabfrage so anzupassen, dass die besten Suchergebnisse erzielt werden.|
|4|Diese geänderte Abfrage wird an den Azure Search-Index gesendet, und die mit `top` festgelegte Anzahl von Ergebnissen wird empfangen. Falls diese Ergebnisse die richtige Antwort nicht enthalten, sollten Sie den Wert von `top` leicht erhöhen. Im Allgemeinen funktioniert der Wert „10“ für `top` bei 90 % aller Abfragen.|
|5|QnA Maker wendet die erweiterte Featurebereitstellung an, um die Richtigkeit der abgerufenen Azure Search-Ergebnisse für die Benutzerabfrage zu ermitteln. |
|6|Für das trainierte Rangfolgemodell wird die Featurebewertung aus Schritt 5 verwendet, um die Einstufung der Azure Search-Ergebnisse vorzunehmen.|
|7|Die neuen Ergebnisse werden in der Rangfolge an die Clientanwendung zurückgegeben.|
|||

Zu den verwendeten Features gehören u. a. Semantik auf Wortebene, Wichtigkeit auf Begriffsebene in einem Korpus und Deep Learning-Semantikmodelle, um die Ähnlichkeit und Relevanz zwischen zwei Textzeichenfolgen zu ermitteln.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklungslebenszyklus einer Wissensdatenbank](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Weitere Informationen

[Übersicht über QnA Maker](../Overview/overview.md)
