---
title: Vordefinierte Modelle für Language Understanding
titleSuffix: Azure Cognitive Services
description: LUIS umfasst einen Satz von vordefinierten Modellen zum schnellen Hinzufügen von allgemeinen Szenarien für Benutzerkonversationen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 2e90a5f3f7cd2cfde2a2ead26674d2348a24ae6f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506785"
---
# <a name="add-prebuilt-models-for-common-usage-scenarios"></a>Hinzufügen vordefinierter Modelle für gängige Verwendungsszenarien 

LUIS umfasst eine Reihe von vordefinierten Absichten aus den vordefinierten Domänen, damit Sie schnell allgemeine Absichten und Äußerungen hinzufügen können. Dies ist eine schnelle und einfache Möglichkeit, um Ihrer Client-App für Konversationen grundlegende Fähigkeiten hinzuzufügen, ohne die Modelle für diese Fähigkeiten entwerfen zu müssen. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="add-a-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

1. Wählen Sie auf der Seite **Meine Apps** Ihre App aus. Daraufhin wird Ihre App im Abschnitt **Build** der App geöffnet. 

1. Wählen Sie auf der Seite **Absichten** auf der Symbolleiste links unten **Add prebuilt domains** (Vordefinierte Domänen hinzufügen) aus. 

1. Wählen Sie die Absicht **Kalender** und dann die Schaltfläche **Domäne hinzufügen** aus.

    ![Hinzufügen der vordefinierten Domäne „Kalender“](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Wählen Sie in der Navigation links **Absichten** aus, um die Kalender-Absichten anzuzeigen. Jede Absicht aus dieser Domäne hat das Präfix `Calendar.`. Zusammen mit den Äußerungen werden der App zwei Entitäten für diese Domäne hinzugefügt: `Calendar.Location` und `Calendar.Subject`. 

### <a name="train-and-publish"></a>Trainieren und Veröffentlichen

1. Trainieren Sie die App nach dem Hinzufügen der Domäne, indem Sie auf der Symbolleiste rechts oben **Trainieren** auswählen. 

1. Wählen Sie auf der oberen Symbolleiste **Veröffentlichen** aus. Veröffentlichen Sie für die **Produktion**. 

1. Wenn die grüne Erfolgsmeldung angezeigt wird, wählen Sie den Link **Refer to the list of endpoints** (Endpunktliste anzeigen) aus, um die Endpunkte anzuzeigen.

1. Wählen Sie einen Endpunkt aus. An diesen Endpunkt wird eine neue Browserregisterkarte geöffnet. Lassen Sie die Browserregisterkarte geöffnet, und fahren Sie mit dem Abschnitt **Testen** fort.

### <a name="test"></a>Test

Testen Sie die neue Absicht am Endpunkt, indem Sie einen Wert für den **q**-Parameter hinzufügen: `Schedule a meeting with John Smith in Seattle next week`.

LUIS gibt die richtige Absicht und das Thema der Besprechung zurück:

```json
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="add-a-prebuilt-intent"></a>Hinzufügen einer vordefinierten Absicht

1. Wählen Sie auf der Seite **Meine Apps** Ihre App aus. Daraufhin wird Ihre App im Abschnitt **Build** der App geöffnet. 

1. Wählen Sie auf der Seite **Absichten** auf der Symbolleiste über der Absichtenliste **Add prebuilt intent** (Vordefinierte Absicht hinzufügen) aus. 

1. Wählen Sie die Absicht **Utilities.Cancel** im Popupdialogfeld aus. 

    ![Hinzufügen der vordefinierten Absicht](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. Wählen Sie die Schaltfläche **Fertig** aus.

### <a name="train-and-test"></a>Trainieren und Testen

1. Trainieren Sie die App nach dem Hinzufügen der Absicht, indem Sie auf der Symbolleiste rechts oben **Trainieren** auswählen. 

1. Testen Sie die neue Absicht, indem Sie auf der rechten Symbolleiste **Testen** auswählen. 

1. Geben Sie im Textfeld Äußerungen für Stornierungen ein:

    |Testen der Äußerung|Vorhersageergebnis|
    |--|:--|
    |Ich möchte meinen Flug stornieren.|0,67|
    |Storniere den Kauf.|0,52|
    |Storniere die Besprechung.|0,56|

    ![Testen der vordefinierten Absicht](./media/luis-prebuilt-intents/test.png)

## <a name="add-a-prebuilt-entity"></a>Hinzufügen einer vordefinierten Entität

1. Öffnen Sie Ihre App, indem Sie auf der Seite **Meine Apps** auf ihren Namen und anschließend links auf **Entitäten** klicken. 

1. Klicken Sie auf der Seite **Entitäten** auf **Add prebuilt entity** (Vordefinierte Entität hinzufügen).

1. Wählen Sie im Dialogfeld **Add prebuilt entities** (Vordefinierte Entitäten hinzufügen) die vordefinierte Entität „datetimeV2“ aus. 

    ![Dialogfeld „Vordefinierte Entität hinzufügen“](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

1. Wählen Sie **Fertig**aus. Nachdem die Entität hinzugefügt wurde, müssen Sie die App nicht trainieren. 

## <a name="publish-to-view-prebuilt-model-from-prediction-endpoint"></a>Veröffentlichen Sie sie, um ein vordefiniertes Modell vom Vorhersageendpunkt anzuzeigen

Die einfachste Möglichkeit, den Wert eines vordefinierten Modells anzuzeigen, ist eine Abfrage an den veröffentlichten Endpunkt. 

## <a name="marking-entities-containing-a-prebuilt-entity-token"></a>Markieren von Entitäten mit einem vordefinierten Entitätstoken
 Wenn Sie Text wie z.B. `HH-1234` als benutzerdefinierte Entität markieren möchten _und_ dem Modell eine [vordefinierte Anzahl](luis-reference-prebuilt-number.md) hinzugefügt haben, können Sie die benutzerdefinierte Entität nicht im LUIS-Portal markieren. Sie können sie mit der API markieren. 

 Um diese Art von Token, bei der ein Teil bereits mit einer vordefinierten Entität markiert ist, zu markieren, entfernen Sie die vordefinierte Entität aus der LUIS-App. Sie müssen die App nicht trainieren. Markieren Sie anschließend das Token mit Ihrer eigenen benutzerdefinierten Entität. Fügen Sie danach die vordefinierte Entität wieder in die LUIS-App ein.

 Betrachten Sie als weiteres Beispiel die Äußerung als eine Liste von Klasseneinstellungen: `I want first year spanish, second year calculus, and fourth year english lit.` Wenn der LUIS-App die vordefinierte Ordnungszahl hinzugefügt wurde, sind `first`, `second` und `fourth` bereits mit Ordnungszahlen markiert. Wenn Sie die Ordnungszahl und die Klasse erfassen möchten, können Sie eine zusammengesetzte Entität erstellen und die vordefinierte Ordnungszahl und die benutzerdefinierte Entität als Klassennamen umschließen.

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Erstellen eines Modells aus CSV mit REST-APIs](./luis-tutorial-node-import-utterances-csv.md)
