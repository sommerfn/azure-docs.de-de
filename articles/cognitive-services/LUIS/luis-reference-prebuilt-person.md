---
title: Vordefinierte PersonName-Entität – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten personName-Entität in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a9da5089c9309ec06f29fd3e0868102cfa538049
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933455"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Vordefinierte PersonName-Entität für eine LUIS-App
Die vordefinierte personName-Entität erkennt die Namen von Personen. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit personName hinzufügen. Die personName-Entität wird in den [Kulturen](luis-reference-prebuilt-entities.md) Englisch und Chinesisch unterstützt.

## <a name="resolution-for-personname-entity"></a>Auflösung für die personName-Entität

### <a name="api-version-2x"></a>API-Version 2.x

Im folgenden Beispiel wird die Auflösung der Entität **builtin.personName** veranschaulicht.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  },
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Vorschau-API-Version 3.x

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
{
    "query": "Is Jill Jones in Cairo?",
    "prediction": {
        "normalizedQuery": "is jill jones in cairo?",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6544678
            }
        },
        "entities": {
            "personName": [
                "Jill Jones"
            ]
        }
    }
}
```

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
{
    "query": "Is Jill Jones in Cairo?",
    "prediction": {
        "normalizedQuery": "is jill jones in cairo?",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6544678
            }
        },
        "entities": {
            "personName": [
                "Jill Jones"
            ],
            "$instance": {
                "personName": [
                    {
                        "type": "builtin.personName",
                        "text": "Jill Jones",
                        "startIndex": 3,
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

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md). 
