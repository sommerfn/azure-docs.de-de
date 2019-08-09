---
title: Iteratives App-Design – LUIS
titleSuffix: Azure Cognitive Services
description: LUIS lernt am besten in einem iterativen Zyklus aus Modelländerungen, Äußerungsbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 2a540606a6f9cfa790a2244628e7f0b7bef35986
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639259"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Erstellungszyklus für Ihre LUIS-App
LUIS lernt am besten in einem iterativen Zyklus aus Modelländerungen, Äußerungsbeispielen, Veröffentlichungen und dem Sammeln von Daten aus Endpunktabfragen. 

![Erstellungszyklus](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Erstellen eines LUIS-Modells
Mithilfe des Modells soll ermittelt werden, was der Benutzer fragt (Absicht) und welche Teile der Frage Details (Entitäten) bereitstellen, mit deren Hilfe die Antwort bestimmt werden kann. 

Das Modell muss spezifisch für die App-Domäne sein, um Wörter und Ausdrücke zu ermitteln, die relevant sind und in einer typischen Reihenfolge vorliegen. 

Das Modell erfordert Absichten und _sollte über Entitäten verfügen_. 

## <a name="add-training-examples"></a>Hinzufügen von Trainingsbeispielen
LUIS benötigt Beispieläußerungen in Absichten. Für die Beispiele sind ausreichend viele Variationen bei der Wortauswahl und Wortreihenfolge erforderlich, um die Absicht der Äußerung zu bestimmen. Bei sämtlichen Beispieläußerung müssen alle erforderlichen Daten als Entitäten bezeichnet sein. 

Sie weisen LUIS an, Äußerungen zu ignorieren, die für Ihre App-Domäne nicht relevant sind, indem Sie die Äußerung der Absicht **None** zuweisen. Alle Wörter oder Ausdrücke, die nicht aus einer Äußerung gepullt werden sollen, müssen nicht bezeichnet werden. Es sind keine Bezeichnungen für Wörter oder Ausdrücke erforderlich, die ignoriert werden sollen. 

## <a name="train-and-publish-the-app"></a>Trainieren und Veröffentlichen der App
Wenn Sie zu jeder Absicht 15 bis 30 verschiedene Äußerungen haben und die erforderlichen Entitäten mit Bezeichnungen versehen sind, führen Sie das [Training](luis-how-to-train.md) und dann die [Veröffentlichung](luis-how-to-publish-app.md) durch. Verwenden Sie den Link aus der Veröffentlichungsbenachrichtigung, um zu Ihren Endpunkten zu gelangen. Stellen Sie sicher, dass Ihre App erstellt und veröffentlicht wurde, damit sie in den [Endpunktregionen](luis-reference-regions.md) verfügbar ist. 

## <a name="https-endpoint-testing"></a>HTTPS-Tests an Endpunkten
Sie können Ihre LUIS-App von dem HTTPS-Endpunkt aus testen. Durch Tests vom Endpunkt kann LUIS alle Äußerungen mit niedrigen Zuverlässigkeitswerten zur [Überprüfung](luis-how-to-review-endpoint-utterances.md) auswählen.  

## <a name="recycle"></a>Recyceln

Wenn Sie einen Erstellungszyklus abgeschlossen haben, können Sie erneut beginnen. Beginnen Sie mit dem [Überprüfen der Endpunktäußerungen](luis-how-to-review-endpoint-utterances.md), die LUIS mit niedrigen Zuverlässigkeitswerten gekennzeichnet hat. Überprüfen Sie diese Äußerungen auf Absichten und Entitäten. Nach dem Überprüfen der Äußerungen sollte die Prüfliste leer sein.  

Denken Sie daran, die aktuelle Version in eine neue Version zu [klonen](luis-concept-version.md#clone-a-version), und beginnen Sie dann mit Ihren Änderungen in der neuen Version. 

## <a name="batch-testing"></a>Testen in Batches

Durch das [Testen in Batches](luis-concept-batch-test.md) können Sie sehen, wie viele Beispieläußerungen von LUIS bewertet werden. Die Beispiele sollten für LUIS neu sein, und sie sollten richtig mit den Absichten und Entitäten bezeichnet sein, die LUIS finden soll. Die Testergebnisse zeigen an, wie gut LUIS diese Gruppe von Äußerungen auswertet. 

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über die Konzepte der [Zusammenarbeit](luis-concept-collaborator.md).
