---
title: 'Vordefinierte KeyPhrase-Entität: LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vorgefertigten keyphrase-Entität in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: e33b5c766781bc49310dfcae55c3d390a032b522
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933522"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>Vordefinierte KeyPhrase-Entität für eine LUIS-App
Die keyPhrase-Entität extrahiert eine Vielzahl von Schlüsselbegriffen aus einer Äußerung. Sie müssen der App keine Beispieläußerungen hinzufügen, die „keyPhrase“ enthalten. Die keyPhrase-Entität wird für [viele Kulturen](luis-language-support.md#languages-supported) im Rahmen der Funktionen der [Textanalyse](../text-analytics/overview.md) unterstützt. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Auflösung der vorgefertigten keyPhrase-Entität

### <a name="api-version-2x"></a>API-Version 2.x

Im folgenden Beispiel wird die Auflösung der Entität **builtin.keyPhrase** veranschaulicht.

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den [Prozentsatz](luis-reference-prebuilt-percentage.md)-, [Anzahl](luis-reference-prebuilt-number.md)- und [Altersentitäten](luis-reference-prebuilt-age.md).
