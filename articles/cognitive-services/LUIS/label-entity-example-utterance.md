---
title: Bezeichnen einer Entität in einer Beispieläußerung
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie eine durch maschinelles Lernen erworbene Entität mit Unterkomponenten in einer Beispieläußerung auf einer Absichtsdetailseite des LUIS-Portals bezeichnen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 11/15/2019
ms.author: diberry
ms.openlocfilehash: 87b7375b2da0effbc18fff6a7e9d67383c93a403
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134120"
---
# <a name="label-machine-learned-entity-in-an-example-utterance"></a>Bezeichnen einer durch maschinelles Lernen erworbenen Entität in einer Beispieläußerung

Die Bezeichnung einer Entität in einer Beispieläußerung zeigt, dass LUIS über ein Beispiel der Entität verfügt und wo die Entität in der Äußerung auftauchen kann. 

## <a name="labeling-machine-learned-entity"></a>Bezeichnen einer durch maschinelles Lernen erworbenen Entität

Nehmen wir als Beispiel den Satz `hi, please I want a cheese pizza in 20 minutes`. 

1. Wählen Sie den Text auf der linken Seite aus, und wählen Sie anschließend den Text auf der rechten Seite der Entität aus. In der folgenden Abbildung ist _CompleteOrder_ mit einer Bezeichnung versehen.

    > [!div class="mx-imgBorder"]
    > ![Bezeichnen der vollständigen, durch maschinelles Lernen erworbenen Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order.png)

1. Wählen Sie die Entität im Popupfenster aus. Die bezeichnete Entität für die Pizzabestellung enthält alle Wörter (im Englischen von links nach rechts), die bezeichnet sind. 

> [!TIP]
> Die im Popupfenster verfügbaren Entitäten sind relativ zum Kontext des Texts. Wenn Sie also beispielsweise über eine durch maschinelles Lernen erworbene Entität mit fünf Ebenen verfügen und Text auf der dritten Ebene auswählen (angegeben durch einen Entitätsnamen unter der Beispieläußerung), sind die im Popupfenster verfügbaren Entitäten auf den Kontext der Unterkomponenten der dritten Ebene (Unterkomponenten der vierten Ebene) beschränkt. 

## <a name="review-labeled-text"></a>Überprüfen des bezeichneten Texts

Überprüfen Sie im Anschluss an die Bezeichnung die Beispieläußerung. Nach der Bezeichnung wendet LUIS das aktuelle Modell auf die Beispieläußerung an. Die durchgezogene Linie gibt an, dass der Text bezeichnen wurde. 

> [!div class="mx-imgBorder"]
> ![Bezeichnete, vollständige, durch maschinelles Lernen erworbene Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order-labeled.png)

## <a name="when-to-train"></a>Informationen zum Trainieren

Wenn das aktuelle Modell Ihre bezeichnete Entität unterstützen sollte, die Beispieläußerung den Text aber weiterhin wie vorhergesagt, aber ohne Bezeichnung anzeigt, müssen Sie Ihre App trainieren.  

## <a name="confirm-predicted-entity"></a>Bestätigen der vorhergesagten Entität

Befindet sich der visuelle Indikator über der Äußerung, wurde der Text zwar vorhergesagt, aber _noch nicht bezeichnet_. Wählen Sie die Äußerung und anschließend **Entitätsvorhersagen bestätigen** aus, um aus der Vorhersage eine Bezeichnung zu machen.

> [!div class="mx-imgBorder"]
> ![Vorhersagen der vollständigen, durch maschinelles Lernen erworbenen Entität](media/label-utterances/example-1-label-machine-learned-entity-complete-order-predicted.png)

## <a name="label-subcomponent-entity-by-painting-with-entity-palette-cursor"></a>Bezeichnen der Unterkomponentenentität durch Zeichnen mit dem Entitätspalettencursor

1. Öffnen Sie zum Korrigieren von Vorhersagen (Entitäten, die über der Beispieläußerung angezeigt werden) die Entitätspalette. 

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/pizza-entity-palette-with-pizza-type-selected.png)

1. Wählen Sie die Entitätsunterkomponente aus. Diese Aktion wird visuell durch einen neuen Cursor dargestellt. Der Cursor folgt dem Mauszeiger, wenn Sie ihn im Portal bewegen. 

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/pizza-type-entity-palette-cursor.png)

1. _Zeichnen_ Sie die Entität in der Beispieläußerung mithilfe des Cursors. 

    > [!div class="mx-imgBorder"]
    > ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/paint-subcomponent-with-entity-palette-cursor.png)

## <a name="labeling-matching-text-entities-to-a-machine-learned-entity"></a>Bezeichnen von Textentsprechungsentitäten mit einer durch maschinelles Lernen erworbenen Entität

Textentsprechungsentitäten umfassen vorgefertigte Entitäten, Entitäten für reguläre Ausdrücke sowie Listenentitäten. Sie werden einer durch maschinelles Lernen erworbenen Entität als Einschränkungen für eine Unterkomponente hinzugefügt, wenn Sie die durch maschinelles Lernen erworbene Entität erstellen oder bearbeiten. 

**Nach dem Hinzufügen dieser Einschränkungen muss der übereinstimmende Text in der Beispieläußerung nicht bezeichnet werden.**

## <a name="entity-prediction-errors"></a>Fehler bei der Entitätsvorhersage

Im Falle eines Fehlers bei der Entitätsvorhersage wird ein Warnindikator angezeigt. Das bedeutet, dass die vorhergesagte Entität nicht der bezeichneten Entität entspricht. 

> [!div class="mx-imgBorder"]
> ![Entitätspalette für eine durch maschinelles Lernen erworbene Entität](media/label-utterances/example-utterance-indicates-prediction-error.png)

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie das [Dashboard](luis-how-to-use-dashboard.md), und [überprüfen Sie die Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md), um die Vorhersagequalität Ihrer App zu verbessern.
