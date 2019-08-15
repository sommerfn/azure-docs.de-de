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
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: ecdaec6dcade033bf99842dd384be095dd363a05
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933405"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Temperature-Entität für eine LUIS-App
Die Temperaturentität extrahiert verschiedene Temperaturtypen. Da diese Entität bereits trainiert wurde, müssen Sie der Anwendung keine Beispieläußerungen mit Temperaturen hinzufügen. Die Temperaturentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-temperature"></a>Temperaturtypen
Die Entität „temperature“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) verwaltet.

## <a name="resolution-for-prebuilt-temperature-entity"></a>Auflösung der vorgefertigten Temperaturentität

### <a name="api-version-2x"></a>API-Version 2.x

Im folgenden Beispiel wird die Auflösung der Entität **builtin.temperature** veranschaulicht.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
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
}
```

### <a name="preview-api-version-3x"></a>Vorschau-API-Version 3.x

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
{
    "query": "set the temperature to 30 degrees",
    "prediction": {
        "normalizedQuery": "set the temperature to 30 degrees",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.656305432
            }
        },
        "entities": {
            "temperature": [
                {
                    "number": 30,
                    "unit": "Degree"
                }
            ]
        }
    }
}
```

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
{
    "query": "set the temperature to 30 degrees",
    "prediction": {
        "normalizedQuery": "set the temperature to 30 degrees",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.656305432
            }
        },
        "entities": {
            "temperature": [
                {
                    "number": 30,
                    "unit": "Degree"
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
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den [Prozentsatz](luis-reference-prebuilt-percentage.md)-, [Anzahl](luis-reference-prebuilt-number.md)- und [Altersentitäten](luis-reference-prebuilt-age.md). 
