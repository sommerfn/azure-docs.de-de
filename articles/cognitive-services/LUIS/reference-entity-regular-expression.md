---
title: Entität vom Typ „RegEx“ (Regulärer Ausdruck) – LUIS
titleSuffix: Azure Cognitive Services
description: Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Zeichenebene (nicht auf Tokenebene) angewandt.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: ae46df875d588186cd083134820f349158d7e307
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695221"
---
# <a name="regular-expression-entity"></a>Entität vom Typ „RegEx“ 

Eine RegEx-Entität extrahiert eine Entität anhand eines regulären Ausdrucks, den Sie bereitstellen.

Ein regulärer Ausdruck ist am besten für unformatierten Text von Äußerungen geeignet. Die Groß-/Kleinschreibung sowie die Kultur werden ignoriert.  Reguläre Ausdrücke werden nach Änderungen durch die Rechtschreibprüfung auf Zeichenebene (nicht auf Tokenebene) angewandt. Wenn der reguläre Ausdruck zu komplex ist (z.B. mit zu vielen Klammern), können Sie den Ausdruck dem Modell nicht hinzufügen. Es wird ein Teil der [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions)-Bibliothek verwendet, aber nicht die gesamte Bibliothek. 

**Diese Entität ist gut geeignet, wenn Folgendes gilt**:

* Die Daten sind einheitlich formatiert, und auch bei den Abweichungen herrscht Einheitlichkeit.
* Für den regulären Ausdruck sind nicht mehr als zwei Schachtelungsebenen erforderlich. 

![Entität vom Typ „RegEx“](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Überlegungen zur Verwendung

Reguläre Ausdrücke finden möglicherweise mehr, als Sie erwarten. Ein Beispiel hierfür ist eine numerische Wortsuche, wie z. B. `one` und `two`. Ein Beispiel ist der folgende reguläre Ausdruck, der die Zahl `one` zusammen mit anderen Zahlen findet:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
``` 

Dieser RegEx-Ausdruck findet auch alle Wörter, die auf diese Zahlen enden, wie z. B. `phone`. Um Probleme wie dieses zu beheben, stellen Sie sicher, dass bei Übereinstimmungen für reguläre Ausdrücke die Wortgrenzen berücksichtigt werden. Der reguläre Ausdruck mit Berücksichtigung von Wortgrenzen für dieses Beispiel wird im folgenden regulären Ausdruck verwendet:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>JSON-Beispiel

Wenn Sie `kb[0-9]{6}` als Definition der RegEx-Entität verwenden, stellt die folgende JSON-Antwort eine Beispieläußerung für die zurückgegebenen RegEx-Entitäten für die folgende Abfrage dar:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

```JSON
"entities": [
  {
    "entity": "kb123456",
    "type": "KB number",
    "startIndex": 9,
    "endIndex": 16
  }
]
```


#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)


Dies ist der JSON-Code, wenn `verbose=false` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Dies ist der JSON-Code, wenn `verbose=true` in der Abfragezeichenfolge festgelegt ist:

```json
"entities": {
    "KB number": [
        "kb123456"
    ],
    "$instance": {
        "KB number": [
            {
                "type": "KB number",
                "text": "kb123456",
                "startIndex": 9,
                "length": 8,
                "modelTypeId": 8,
                "modelType": "Regex Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-quickstart-intents-regex-entity.md) erstellen Sie eine App zum Extrahieren einheitlich formatierter Daten aus einer Äußerung mithilfe der **RegEx**-Entität.