---
title: Entitätstyp „Pattern.any“
titleSuffix: Language Understanding - Azure Cognitive Services
description: Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines Musters verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: f3e5bd068b5df99bdfabff0bfe18ccebff980fc6
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480114"
---
# <a name="patternany-entity"></a>Entität „Pattern.any“ 

Pattern.any ist ein Platzhalter variabler Länge, der nur in der Vorlagenäußerung eines Musters verwendet wird, um zu kennzeichnen, wo die Entität beginnt und endet.  

Pattern.any-Entitäten müssen in [Pattern](luis-how-to-model-intent-pattern.md)-Vorlagenbeispielen gekennzeichnet werden, nicht in den Absichtsbenutzerbeispielen.

**Diese Entität ist gut geeignet, wenn Folgendes gilt**:

* Die Endung der Entität kann leicht mit dem verbleibenden Text der Äußerung verwechselt werden. 

## <a name="usage"></a>Verwendung

Bei einer Clientanwendung, mit der anhand des Titels nach Büchern gesucht wird, wird mit pattern.any der vollständige Titel extrahiert. Eine Vorlagenäußerung mit pattern.any für diese Buchsuche lautet `Was {BookTitle} written by an American this year[?]` (Wurde {Buchtitel} in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?). 

In der folgenden Tabelle enthält jede Zeile zwei Versionen der Äußerung. Die obere Äußerung entspricht dem, wie die Äußerung anfänglich in LUIS dargestellt wird. Es ist nicht klar, wo der Buchtitel beginnt und endet. Die untere Äußerung verwendet eine Pattern.any-Entität, um Anfang und Ende der Entität zu markieren. 

|Äußerung mit Entität in Fettformatierung|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>Wurde das Buch **The Man Who Mistook His Wife for a Hat and Other Clinical Tales** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>Wurde **Half Asleep in Frog Pajamas** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>Wurde **The Particular Sadness of Lemon Cake: A Novel** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>Wurde **There's A Wocket In My Pocket!** in diesem Jahr von einem Autor bzw. einer Autorin aus den USA geschrieben?|
||

## <a name="example-json"></a>JSON-Beispiel

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-tutorial-pattern-any.md) verwenden Sie die Entität **Pattern.any**, um Daten aus Äußerungen zu extrahieren, in denen die Äußerungen wohlgeformt sind und das Ende der Daten nicht leicht von den restlichen Wörtern der Äußerung unterschieden werden kann.