---
title: Worum handelt es sich bei Language Understanding (LUIS)?
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: overview
ms.date: 11/04/2019
ms.author: diberry
ms.openlocfilehash: 8ee22a2a8a12eb85439e191bc21e6cf391bea3f8
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612848"
---
# <a name="what-is-language-understanding-luis"></a>Worum handelt es sich bei Language Understanding (LUIS)?

Language Understanding (LUIS) ist ein cloudbasierter API-Dienst, der benutzerdefinierte Machine Learning-Intelligenz auf natürliche Konversationssprachtexte eines Benutzers anwendet, um die allgemeine Bedeutung vorherzusagen sowie relevante und detaillierte Informationen abzurufen. 

Bei einer Clientanwendung für LUIS handelt es sich um eine beliebige Konversationsanwendung, die zum Durchführen einer Aufgabe mit einem Benutzer in natürlicher Sprache kommuniziert. Beispiele für Clientanwendungen sind Social Media Apps, Chatbots und sprachfähige Desktopanwendungen.  

![Konzeptionelle Abbildung dreier Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten](./media/luis-overview/luis-entry-point.png "Konzeptionelle Abbildung dreier Clientanwendungen, die mit Cognitive Services Language Understanding (LUIS) arbeiten")

## <a name="use-luis-in-a-chat-bot"></a>Verwendung von LUIS in einen Chatbot

<a name="Accessing-LUIS"></a>

Sobald die LUIS-App veröffentlicht ist, sendet eine Clientanwendung Äußerungen (Text) an die LUIS-Endpunkt-[API][endpoint-apis] für die Verarbeitung von natürlicher Sprache und empfängt die Ergebnisse als JSON-Antworten. Eine gängige Clientanwendung für LUIS ist ein Chatbot.


![Konzeptionelle Abbildungen von LUIS in Zusammenarbeit mit dem Chatbot zur Vorhersage von Benutzertext mithilfe des Verstehens natürlicher Sprache](./media/luis-overview/LUIS-chat-bot-request-response.svg "Konzeptionelle Abbildungen von LUIS in Zusammenarbeit mit dem Chatbot zur Vorhersage von Benutzertext mithilfe des Verstehens natürlicher Sprache")

|Schritt|Aktion|
|:--|:--|
|1|Die Clientanwendung sendet die _Benutzeräußerung_ (Text in den Worten des Benutzers) „Ich möchte meinen Personalverantwortlichen anrufen“ als HTTP-Anforderung an den LUIS-Endpunkt.|
|2|LUIS ermöglicht Ihnen das Erstellen eigener Sprachmodelle, um Ihrer Anwendung Intelligenz zu verleihen. Maschinell erlernte Sprachmodelle verwenden den unstrukturierten Eingabetext des Benutzers und geben eine Antwort im JSON-Format mit der wichtigsten Absicht, `HRContact`, zurück. Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die Entität _Kontakttyp_ extrahieren.|
|3|Die Clientanwendung verwendet die JSON-Antwort für Entscheidungen darüber, wie die Anforderungen des Benutzer erfüllt werden. Diese Entscheidungen können eine Entscheidungsstruktur im Frameworkcode des Bots und Aufrufe anderer Dienste umfassen. |

Die LUIS-App stellt Intelligenz bereit, damit die Clientanwendung intelligente Entscheidungen treffen kann. Die Auswahlmöglichkeiten werden nicht von LUIS bereitgestellt. 

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Verarbeitung natürlicher Sprache

Ihre LUIS-App enthält ein domänenspezifisches Modell für natürliche Sprache. Sie können die LUIS-App mit einem vordefinierten Domänenmodell beginnen, ein eigenes Modell erstellen oder Teile einer vordefinierten Domäne mit Ihren eigenen benutzerdefinierten Informationen mischen.

* **Vordefiniertes Modell**: LUIS verfügt über eine Vielzahl vordefinierter Domänenmodelle, einschließlich Absichten, Äußerungen und vordefinierter Entitäten. Sie können die vordefinierten Entitäten nutzen, ohne die Absichten und Äußerungen des vordefinierten Modells verwenden zu müssen. [Vordefinierte Domänenmodelle](luis-how-to-use-prebuilt-domains.md) enthalten den gesamten Entwurf und bieten eine hervorragende Möglichkeit für ein schnelle Verwendung von LUIS.

* **Benutzerdefiniertes Modell** LUIS bietet Ihnen mehrere Möglichkeiten, eigene benutzerdefinierte Modelle zu definieren, einschließlich Absichten und Entitäten. Entitäten umfassen auf maschinellem Lernen basierende Entitäten, bestimmte oder literale Entitäten sowie eine Kombination von literalen und auf maschinellem Lernen basierenden Entitäten.

## <a name="build-the-luis-model"></a>Erstellen des LUIS-Modells
Erstellen Sie das Modell mit den [Dokumenterstellungs](https://go.microsoft.com/fwlink/?linkid=2092087)-APIs oder über das [LUIS-Portal](https://www.luis.ai).

Das LUIS-Modell beginnt mit Kategorien von Benutzerabsichten, die als **[Absichten](luis-concept-intent.md)** bezeichnet werden. Jede Absicht erfordert Beispiele für **[Äußerungen](luis-concept-utterance.md)** von Benutzern. Jede Äußerung kann Daten liefern, die extrahiert werden müssen. 

|Beispieläußerung eines Benutzers|Absicht|Extrahierte Daten|
|-----------|-----------|-----------|
|`Book a flight to __Seattle__?`|FlugBuchen|Seattle|
|`When does your store __open__?`|GeschäftszeitenUndStandorte|öffnen|
|`Schedule a meeting at __1pm__ with __Bob__ in Distribution`|BesprechungPlanen|13 Uhr, Robert|

## <a name="query-prediction-endpoint"></a>Abfragen eines Vorhersageendpunkts

Nachdem Ihre App trainiert und für den Endpunkt veröffentlicht wurde, sendet die Clientanwendung Äußerungen an die API des [Vorhersageendpunkts](https://go.microsoft.com/fwlink/?linkid=2092356). Die API wendet zu Analysezwecken die App auf die Äußerung an und liefert Vorhersageergebnisse in einem JSON-Format zurück.  

Die JSON-Endpunktantwort enthält mindestens die Abfrageäußerung und die Absicht mit der höchsten Bewertung. Sie kann auch Daten wie die folgende Entität **Kontakttyp** und die allgemeine Stimmung extrahieren. 

```JSON
{
    "query": "I want to call my HR rep",
    "prediction": {
        "normalizedQuery": "i want to call my hr rep",
        "topIntent": "HRContact",
        "intents": {
            "HRContact": {
                "score": 0.8582669
            }
        },
        "entities": {
            "Contact Type": [
                "call"
            ]
        },
        "sentiment": {
            "label": "negative",
            "score": 0.103343368
        }
    }
}
```

## <a name="improve-model-prediction"></a>Verbessern der Modellvorhersage

Nachdem die LUIS-App veröffentlicht wurde und echte Benutzeräußerungen empfängt, ermöglicht LUIS zum Verbessern der Vorhersagegenauigkeit [aktives Lernen](luis-concept-review-endpoint-utterances.md) anhand von Endpunktäußerungen. 

<a name="using-luis"></a>

## <a name="development-lifecycle"></a>Lebenszyklus der Entwicklung
LUIS bietet Tools, eine Versionsverwaltung und die Zusammenarbeit mit anderen LUIS-Autoren für die Integration in den vollständigen [Entwicklungslebenszyklus](luis-concept-app-iteration.md). 

## <a name="implementing-luis"></a>Implementieren von LUIS
LUIS (Language Understanding ) kann als REST-API mithilfe einer HTTP-Anforderung mit allen Produkten, Diensten oder Frameworks eingesetzt werden. Die folgende Liste enthält die am häufigsten mit LUIS verwendeten Microsoft-Produkte und -Dienste.

Die am häufigsten verwendete Clientanwendung für LUIS ist:
* [Web-App-Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0) erstellt schnell einen LUIS-fähigen Chatbot für die Kommunikation mit einem Benutzer über die Texteingabe. Verwendet [Bot Framework][bot-framework] Version [4.x](https://github.com/Microsoft/botbuilder-dotnet) für eine vollständige Bot-Erfahrung.

Tools zum schnellen und einfachen Nutzen von LUIS mit einem Bot:
* [LUIS-CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) Mit dem NPM-Paket sind Erstellungen und Vorhersagen entweder mit einem eigenständigen Befehlszeilentool oder als Import möglich. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen ist ein Tool zum Generieren von stark typisiertem C#- und TypeScript-Quellcode aus einem exportierten LUIS-Modell.
* [Dispatch](https://aka.ms/dispatch-tool) ermöglicht die Verwendung mehrerer LUIS- und QnA Maker-Apps aus einer übergeordneten App mithilfe eines Verteilermodells.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown ist ein Befehlszeilentool, mit dem Sprachmodelle für Ihren Bot verwaltet werden können.
* [Bot Framework: Composer](https://github.com/microsoft/BotFramework-Composer): Ein integriertes Entwicklungstool für Entwickler und bereichsübergreifende Teams zum Erstellen von Bots und Konversationsumgebungen mit Microsoft Bot Framework

Weitere Cognitive Services, die mit LUIS verwendet werden:
* [QnA Maker][qnamaker] ermöglicht die Kombination mehrerer Texttypen in einer Wissensdatenbank für Fragen und Antworten.
* Der [Spracherkennungsdienst](../Speech-Service/overview.md) konvertiert Anforderungen in gesprochener Sprache in Text. 
* Das [Unterhaltungslernmodul](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) ermöglicht Ihnen mit LUIS ein schnelleres Erstellen von Bot-Konversationen.

Beispiele mit LUIS:
* GitHub-Repository zu [Konversations-KI](https://github.com/Microsoft/AI)
* [Bot Framework: Beispiele von Bots](https://github.com/microsoft/BotBuilder-Samples)

## <a name="next-steps"></a>Nächste Schritte

* [Neuigkeiten](whats-new.md)
* Erstellen Sie eine neue LUIS-App mit einer [vordefinierten](luis-get-started-create-app.md) oder [benutzerdefinierten](luis-quickstart-intents-only.md) Domäne.
* [Fragen Sie den Endpunkt der Vorhersage](luis-get-started-get-intent-from-browser.md) einer öffentlichen IoT-App ab. 
* [Entwicklerressourcen](developer-reference-resource.md) für LUIS. 

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
