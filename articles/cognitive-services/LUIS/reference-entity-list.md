---
title: Entität vom Typ „List“ – LUIS
titleSuffix: Azure Cognitive Services
description: Listenentitäten stellen einen festen, abgeschlossenen Satz verwandter Wörter zusammen mit ihren Synonymen dar. LUIS ermittelt keine zusätzlichen Werte für Listenentitäten. Suchen Sie mithilfe des Empfehlungsfeatures nach Vorschlägen für neue Wörter basierend auf der aktuellen Liste.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: ca9f8b570ee28b1913c8ec81c66a5b70827c04d6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559954"
---
# <a name="list-entity"></a>Entität vom Typ „List“ 

Listenentitäten stellen einen festen, abgeschlossenen Satz verwandter Wörter zusammen mit ihren Synonymen dar. LUIS ermittelt keine zusätzlichen Werte für Listenentitäten. Suchen Sie mithilfe des Features **Empfehlen** nach Vorschlägen für neue Wörter basierend auf der aktuellen Liste. Wenn mehr als eine Listenentität mit demselben Wert vorhanden ist, wird in der Endpunktabfrage jede Entität zurückgegeben. 

Eine Listenentität wird nicht maschinell gelernt. Sie stellt eine genaue Textübereinstimmung dar. LUIS kennzeichnet jede Übereinstimmung für ein Element in einer Liste in der Antwort als eine Entität. 

**Diese Entität ist gut geeignet, wenn für die Textdaten Folgendes gilt**:

* Es handelt sich um einen bekannten Satz.
* Sie werden nicht oft geändert. Wenn Sie die Liste häufig ändern oder möchten, dass die Liste selbstständig erweitert wird, stellt eine einfache Entität, die um eine Begriffsliste erweitert wurde, eine bessere Wahl dar. 
* Für den Satz werden die maximalen LUIS-[Grenzen](luis-boundaries.md) dieses Entitätstyps nicht überschritten.
* Der Text in der Äußerung ist eine exakte Übereinstimmung mit einem Synonym oder dem kanonischen Namen. LUIS verwendet die Liste über genaue Textübereinstimmungen hinaus nicht. Fuzzyübereinstimmungen, Nicht-Berücksichtigung der Groß- und Kleinschreibung, Wortstammerkennung, Pluralformen und andere Varianten lassen sich mit einer List-Entität nicht auflösen. Um Varianten zu behandeln, sollten Sie die Verwendung eines [Musters](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) mit der optionalen Textsyntax in Erwägung ziehen.

![Entität vom Typ „List“](./media/luis-concept-entities/list-entity.png)

## <a name="example-json"></a>JSON-Beispiel

Angenommen, die App enthält die Liste `Cities`, die Variationen von Städtenamen einschließlich Ort des Flughafens (Sea-tac), Flughafencode (SEA), Postleitzahl (98101) und Vorwahl (206) ermöglicht.

|Listenelement|Elementsynonyme|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

In der Äußerung oben wird das Wort `paris` dem Element Paris als Teil der Listenentität `Cities` zugeordnet. Die Listenentität findet Übereinstimmungen sowohl des normalisierten Elementnamens als auch der Synonyme des Elements.

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Eine weitere Beispieläußerung mit einem Synonym für Paris:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Entität vom Typ „Simple“|`Customer`|`bob jones`|

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-quickstart-intent-and-list-entity.md) erfahren Sie, wie Sie eine **Listenentität** verwenden, um exakte Textübereinstimmungen aus einer Liste bekannter Elemente zu extrahieren. 
