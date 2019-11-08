---
title: Vordefinierte Temperature-Entität – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Temperaturentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: f2ea08694419caaaf54e4fed45c7c1589be2473d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499518"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Temperature-Entität für eine LUIS-App
Die Temperaturentität extrahiert verschiedene Temperaturtypen. Da diese Entität bereits trainiert wurde, müssen Sie der Anwendung keine Beispieläußerungen mit Temperaturen hinzufügen. Die Temperaturentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-temperature"></a>Temperaturtypen
Die Entität „temperature“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) verwaltet.

## <a name="resolution-for-prebuilt-temperature-entity"></a>Auflösung der vorgefertigten Temperaturentität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`set the temperature to 30 degrees`


#### <a name="v3-responsetabv3"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ],
    "$instance": {
        "temperature": [
            {
                "type": "builtin.temperature",
                "text": "30 degrees",
                "startIndex": 23,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-responsetabv2"></a>[V2-Antwort](#tab/V2)

Im folgenden Beispiel wird die Auflösung der Entität **builtin.temperature** veranschaulicht.

```json
"entities": [
    {
        "entity": "30 degrees",
        "type": "builtin.temperature",
        "startIndex": 23,
        "endIndex": 32,
        "resolution": {
        "unit": "Degree",
        "value": "30"
        }
    }
]
```
* * * 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr zu den [Prozentsatz](luis-reference-prebuilt-percentage.md)-, [Anzahl](luis-reference-prebuilt-number.md)- und [Altersentitäten](luis-reference-prebuilt-age.md). 
