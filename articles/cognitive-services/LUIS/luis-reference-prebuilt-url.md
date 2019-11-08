---
title: Vordefinierte URL-Entitäten – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „url“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 12831ede2b9d9251f2e02fa396ee7d2fb2d61240
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499496"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>Vordefinierte URL-Entität für eine LUIS-App
Die Entität „url“ extrahiert URLs mit Domänennamen oder IP-Adressen. Da diese Entität bereits trainiert wurde, müssen Sie der Anwendung keine Beispieläußerungen mit URLs hinzufügen. Die Entität „url“ wird nur in der Kultur `en-us` unterstützt. 

## <a name="types-of-urls"></a>url-Typen
Die Entität „url“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) verwaltet.

## <a name="resolution-for-prebuilt-url-entity"></a>Auflösung der url-Entität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`https://www.luis.ai is a great cognitive services example of artificial intelligence`

#### <a name="v3-responsetabv3"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ],
    "$instance": {
        "url": [
            {
                "type": "builtin.url",
                "text": "https://www.luis.ai",
                "startIndex": 0,
                "length": 17,
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

Das folgende Beispiel veranschaulicht die Auflösung von https://www.luis.ai und stellt ein hervorragendes Beispiel für die künstliche Intelligenz von Cognitive Services dar.

```json
"entities": [
    {
        "entity": "https://www.luis.ai",
        "type": "builtin.url",
        "startIndex": 0,
        "endIndex": 17
    }
]
```

* * * 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) und [temperature](luis-reference-prebuilt-temperature.md).
