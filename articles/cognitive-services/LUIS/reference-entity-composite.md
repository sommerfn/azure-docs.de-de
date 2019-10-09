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
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: a5a1ad467074ee0aa55d14d50ae153ac68304e6f
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695164"
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

`book 2 tickets to cairo`

Beachten Sie, dass zwischen der Anzahl `2` und der ToLocation `cairo` Wörter stehen, die nicht Teil der Entitäten sind. Die grüne Unterstreichung, die in einer bezeichneten Äußerung auf der [LUIS](luis-reference-regions.md)-Website verwendet wird, gibt eine zusammengesetzte Entität an.

![Entität vom Typ „Composite“](./media/luis-concept-data-extraction/composite-entity.png)

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

Zusammengesetzte Entitäten werden in einem Array vom Typ `compositeEntities` zurückgegeben. Dabei werden auch alle Entitäten in dieser zusammengesetzten Entität im `entities`-Array zurückgegeben:

```JSON
  "entities": [
    {
      "entity": "2 tickets to cairo",
      "type": "ticketinfo",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.9214487
    },
    {
      "entity": "cairo",
      "type": "builtin.geographyV2.city",
      "startIndex": 18,
      "endIndex": 22
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "subtype": "integer",
        "value": "2"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "ticketinfo",
      "value": "2 tickets to cairo",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "builtin.geographyV2.city",
          "value": "cairo"
        }
      ]
    }
  ]
```    

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)

Dies ist der JSON-Code, wenn `verbose=false` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ]
        }
    ]
}
```

Dies ist der JSON-Code, wenn `verbose=true` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "ticketinfo": [
        {
            "number": [
                2
            ],
            "geographyV2": [
                "cairo"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 5,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "geographyV2": [
                    {
                        "type": "builtin.geographyV2.city",
                        "text": "cairo",
                        "startIndex": 18,
                        "length": 5,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "$instance": {
        "ticketinfo": [
            {
                "type": "ticketinfo",
                "text": "2 tickets to cairo",
                "startIndex": 5,
                "length": 18,
                "score": 0.9214487,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 


|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Vordefinierte Entität – number|"builtin.number"|"2"|
|Vordefinierte Entität – GeographyV2|"Location::ToLocation"|"cairo"|

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-tutorial-composite-entity.md) fügen Sie eine **zusammengesetzte Entität** hinzu, um extrahierte Daten verschiedener Typen in einer einzelnen enthaltenden Entität zu bündeln. Durch Bündeln der Daten kann die Clientanwendung aufeinander bezogene Daten verschiedener Datentypen leicht extrahieren.
