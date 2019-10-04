---
title: Referenz zur vordefinierten E-Mail-Entität in LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten E-Mail-Entität in Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 542173a8756b868603c606307c5b682c68d7db25
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933553"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Vordefinierte E-Mail-Entität für eine LUIS-App
Die E-Mail-Entität extrahiert die vollständige E-Mail-Adresse aus einer Äußerung. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit E-Mail-Adressen hinzufügen. Die E-Mail-Entität wird nur in der Kultur `en-us` unterstützt. 

## <a name="resolution-for-prebuilt-email"></a>Auflösung der vorgefertigten E-Mail-Entität

### <a name="api-version-2x"></a>API-Version 2.x

Im folgenden Beispiel wird die Auflösung der Entität **builtin.email** veranschaulicht.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55,
      "resolution": {
        "value": "patti.owens@microsoft.com"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Vorschau-API-Version 3.x

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ]
        }
    }
}
```


Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ],
            "$instance": {
                "email": [
                    {
                        "type": "builtin.email",
                        "text": "patti.owens@microsoft.com",
                        "startIndex": 31,
                        "length": 25,
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

Erfahren Sie mehr zu den [Anzahl](luis-reference-prebuilt-number.md)-, [Ordinal](luis-reference-prebuilt-ordinal.md)- und [Prozentsatzentitäten](luis-reference-prebuilt-percentage.md). 
