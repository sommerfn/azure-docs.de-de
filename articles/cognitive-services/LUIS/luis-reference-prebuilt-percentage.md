---
title: 'Vordefinierte Percentage-Entität: LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten Prozentsatzentität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 06dada1633fcb90153358e0f1d048c5fa618db0b
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933442"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Percentage-Entität für eine LUIS-App
Prozentzahlen können als Bruchzahlen (`3 1/2`) oder als Prozentzahlen (`2%`) angezeigt werden. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Prozentsätzen hinzufügen. Die Prozentsatzentität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt. 

## <a name="types-of-percentage"></a>Prozentsatztypen
Die Entität „percentage“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) verwaltet.

## <a name="resolution-for-prebuilt-percentage-entity"></a>Auflösung der vorgefertigten Prozentsatzentität

### <a name="api-version-2x"></a>API-Version 2.x

Im folgenden Beispiel wird die Auflösung der Entität **builtin.percentage** veranschaulicht.

```json
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Vorschau-API-Version 3.x

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
{
    "query": "set a trigger when my stock goes up 2%",
    "prediction": {
        "normalizedQuery": "set a trigger when my stock goes up 2%",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.541765451
            }
        },
        "entities": {
            "percentage": [
                2
            ]
        }
    }
}
```

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
{
    "query": "set a trigger when my stock goes up 2%",
    "prediction": {
        "normalizedQuery": "set a trigger when my stock goes up 2%",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.541765451
            }
        },
        "entities": {
            "percentage": [
                2
            ],
            "$instance": {
                "percentage": [
                    {
                        "type": "builtin.percentage",
                        "text": "2%",
                        "startIndex": 36,
                        "length": 2,
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

Erfahren Sie mehr über die Entitäten [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) und [temperature](luis-reference-prebuilt-temperature.md). 
