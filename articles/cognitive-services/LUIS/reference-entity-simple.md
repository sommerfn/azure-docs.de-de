---
title: Entitätstyp „Simple“
titleSuffix: Language Understanding - Azure Cognitive Services
description: Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernens erworben wurde. Da es sich bei einfachen Entitäten normalerweise um Namen handelt, z.B. Unternehmensnamen, Produktnamen oder andere Namen, fügen Sie eine Liste mit Ausdrücken hinzu, wenn Sie eine einfache Entität verwenden, um das Signal für die verwendeten Namen zu verstärken.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 3f03b33f685bb5c7c9ba8f2267b8556c5dadade4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480134"
---
# <a name="simple-entity"></a>Entität vom Typ „Simple“ 

Eine einfache Entität ist eine generische Entität, die ein einzelnes Konzept beschreibt und im Kontext des maschinellen Lernens erworben wurde. Da es sich bei einfachen Entitäten normalerweise um Namen handelt, z.B. Unternehmensnamen, Produktnamen oder andere Namen, sollten Sie wie folgt vorgehen: Fügen Sie eine [Liste mit Ausdrücken](luis-concept-feature.md) hinzu, wenn Sie eine einfache Entität verwenden, um das Signal für die verwendeten Namen zu verstärken. 

**Diese Entität ist gut geeignet, wenn Folgendes gilt**:

* Die Daten sind nicht einheitlich formatiert, aber weisen auf denselben Sachverhalt hin. 

![Entität vom Typ „Simple“](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>JSON-Beispiel

`Bob Jones wants 3 meatball pho`

In der vorherigen Äußerung wird `Bob Jones` als die einfache Entität `Customer` bezeichnet.

Die vom Endpunkt zurückgegebenen Daten enthalten den Namen der Entität, den in der Äußerung ermittelten Text, den Speicherort des erkannten Texts und die Bewertung:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Datenobjekt|Name der Entität|Wert|
|--|--|--|
|Entität vom Typ „Simple“|`Customer`|`bob jones`|

## <a name="next-steps"></a>Nächste Schritte

In diesem [Tutorial](luis-quickstart-primary-and-secondary-data.md) werden maschinell gelernte Daten einer Stellenbezeichnung mithilfe einer **einfachen Entität** aus einer Äußerung extrahiert. Um die Genauigkeit beim Extrahieren zu erhöhen, fügen Sie der einfachen Entität eine [Begriffsliste](luis-concept-feature.md) mit spezifischen Ausdrücken hinzu.