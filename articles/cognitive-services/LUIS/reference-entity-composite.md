---
title: Entität vom Typ „Composite“ – LUIS
titleSuffix: Azure Cognitive Services
description: Eine zusammengesetzte Entität besteht aus anderen Entitäten, z. B. vom Typ Vordefiniert, Einfach, Regulärer Ausdruck oder Liste. Die einzelnen Entitäten bilden zusammen die gesamte Entität.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: ea258275cf954bc6e06da03324c2ae93de0e7fde
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563232"
---
# <a name="composite-entity"></a>Entität vom Typ „Composite“ 

Eine zusammengesetzte Entität besteht aus anderen Entitäten, z. B. vom Typ Vordefiniert, Einfach, Regulärer Ausdruck oder Liste. Die einzelnen Entitäten bilden zusammen die gesamte Entität. 

**Diese Entität ist gut geeignet, wenn für die Daten Folgendes gilt**:

* Sind aufeinander bezogen. 
* Sie sind im Kontext der Äußerung miteinander verknüpft.
* Verwenden eine Vielzahl von Entitätstypen.
* Müssen gruppiert und von der Clientanwendung als eine Informationseinheit verarbeitet werden.
* Verfügen über viele verschiedene Benutzeräußerungen, für die maschinelles Lernen erforderlich ist.

![Entität vom Typ „Composite“](./media/luis-concept-entities/composite-entity.png)

## <a name="example-json"></a>JSON-Beispiel

Betrachten Sie beispielsweise eine zusammengesetzte Entität der vordefinierten Entitäten `number` und `Location::ToLocation` bei der folgenden Äußerung:

`book 2 tickets to paris`

Beachten Sie, dass zwischen der Anzahl `2` und der ToLocation `paris` Wörter stehen, die nicht Teil der Entitäten sind. Die grüne Unterstreichung, die in einer bezeichneten Äußerung auf der [LUIS](luis-reference-regions.md)-Website verwendet wird, gibt eine zusammengesetzte Entität an.

![Entität vom Typ „Composite“](./media/luis-concept-data-extraction/composite-entity.png)

Zusammengesetzte Entitäten werden in einem Array vom Typ `compositeEntities` zurückgegeben. Dabei werden auch alle Entitäten in dieser zusammengesetzten Entität im `entities`-Array zurückgegeben:

```JSON

"entities": [
    {
    "entity": "2 tickets to cairo",
    "type": "ticketInfo",
    "startIndex": 0,
    "endIndex": 17,
    "score": 0.67200166
    },
    {
    "entity": "2",
    "type": "builtin.number",
    "startIndex": 0,
    "endIndex": 0,
    "resolution": {
        "subtype": "integer",
        "value": "2"
    }
    },
    {
    "entity": "cairo",
    "type": "builtin.geographyV2",
    "startIndex": 13,
    "endIndex": 17
    }
],
"compositeEntities": [
    {
    "parentType": "ticketInfo",
    "value": "2 tickets to cairo",
    "children": [
        {
        "type": "builtin.geographyV2",
        "value": "cairo"
        },
        {
        "type": "builtin.number",
        "value": "2"
        }
    ]
    }
]
```    

|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Vordefinierte Entität – number|"builtin.number"|"2"|
|Vordefinierte Entität – GeographyV2|"Location::ToLocation"|"paris"|

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-tutorial-composite-entity.md) fügen Sie eine **zusammengesetzte Entität** hinzu, um extrahierte Daten verschiedener Typen in einer einzelnen enthaltenden Entität zu bündeln. Durch Bündeln der Daten kann die Clientanwendung aufeinander bezogene Daten verschiedener Datentypen leicht extrahieren.
