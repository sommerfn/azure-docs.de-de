---
title: 'Schnellstart: Erstellen einer App – LUIS'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 748c51e74db20ac101dc2dff0d924567acded114
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703238"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Schnellstart: Verwenden der vordefinierten Home Automation-App

In dieser Schnellstartanleitung erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel benötigen Sie ein kostenloses LUIS-Konto, das Sie im LUIS-Portal unter [https://www.luis.ai](https://www.luis.ai) erstellt haben. 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-a-new-app"></a>Erstellen einer neuen App
Sie können Ihre Anwendungen auf der Seite **Meine Apps** erstellen und verwalten. 

1. Klicken Sie auf **Create new app** (Neue App erstellen).

    [![Screenshot der App-Liste](media/luis-quickstart-new-app/app-list.png "Screenshot der App-Liste")](media/luis-quickstart-new-app/app-list.png)

1. Geben Sie im Dialogfeld den Namen „Home Automation“ für Ihre Anwendung ein.

    [![Screenshot des Popupdialogfelds zum Erstellen einer neuen App](media/luis-quickstart-new-app/create-new-app-dialog.png "Screenshot des Popupdialogfelds zum Erstellen einer neuen App")](media/luis-quickstart-new-app/create-new-app-dialog.png)

1. Wählen Sie Ihre Anwendungskultur aus. Wählen Sie für die Home Automation-App die Kultur „English“ (Englisch) aus. Wählen Sie dann **Fertig** aus. LUIS erstellt die Home Automation-App. 

    >[!NOTE]
    >Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

## <a name="add-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

Klicken Sie im linken Navigationsbereich auf **Prebuilt domains** (Vordefinierte Domänen). Suchen Sie dann nach „Home“. Wählen Sie **Add domain** (Domäne hinzufügen) aus.

[![Screenshot der Domäne „HomeAutomation“ im Menü „Prebuilt domains“ (Vordefinierte Domänen)](media/luis-quickstart-new-app/home-automation.png "Screenshot der Domäne „HomeAutomation“ im Menü „Prebuilt domains“ (Vordefinierte Domänen)")](media/luis-quickstart-new-app/home-automation.png)

Wenn die Domäne erfolgreich hinzugefügt wurde, enthält das Feld für die vordefinierte Domäne eine Schaltfläche **Remove domain** (Domäne entfernen).

[![Screenshot der Domäne „HomeAutomation“ mit Schaltfläche zum Entfernen](media/luis-quickstart-new-app/remove-domain.png "Screenshot der Domäne „HomeAutomation“ mit Schaltfläche zum Entfernen")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Absichten und Entitäten

Klicken Sie im linken Navigationsbereich auf **Intents** (Absichten), um die Absichten der Domäne „HomeAutomation“ zu überprüfen. Für jede Absicht sind Beispieläußerungen verfügbar.

![Screenshot der HomeAutomation-Absichtsliste](media/luis-quickstart-new-app/home-automation-intents.png "Screenshot der HomeAutomation-Absichtsliste")]

> [!NOTE]
> **None** (Keine) ist eine Absicht, die von allen LUIS-Apps bereitgestellt wird. Diese Absicht verwenden Sie zum Behandeln von Äußerungen, die keiner Funktionalität Ihrer App entsprechen. 

Wählen Sie die Absicht **HomeAutomation.TurnOff** aus. Wie Sie sehen, enthält die Absicht eine Liste von Äußerungen, die mit Entitäten bezeichnet sind.

[![Screenshot der Absicht „HomeAutomation.TurnOff“](media/luis-quickstart-new-app/home-automation-turnoff.png "Screenshot der Absicht „HomeAutomation.TurnOff“")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Testen Ihrer App
Nachdem Sie Ihre App trainiert haben, können Sie sie testen. Klicken Sie auf der oberen Navigationsleiste auf **Test** (Testen). Geben Sie eine Testäußerung wie „Turn off the lights“ (Mach das Licht aus) in den Bereich für interaktive Tests ein, und drücken Sie die EINGABETASTE. 

```
Turn off the lights
```

Überprüfen Sie, ob die Absicht mit der höchsten Bewertung der Absicht entspricht, die Sie für die einzelnen Testäußerungen erwartet haben.

In diesem Beispiel wurde `Turn off the lights` korrekt als Absicht mit der höchsten Bewertung von **HomeAutomation.TurnOff** identifiziert.

[![Screenshot des Testbereichs mit hervorgehobener Äußerung](media/luis-quickstart-new-app/test.png "Screenshot des Testbereichs mit hervorgehobener Äußerung")](media/luis-quickstart-new-app/test.png)


Wählen Sie **Überprüfen** aus, um weitere Informationen zur Vorhersage anzuzeigen.

![Screenshot des Testbereichs mit hervorgehobener Äußerung](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

Klicken Sie erneut auf **Test** (Testen), um den Testbereich zuzuklappen. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-v2-api-prediction-endpoint"></a>Abfragen des V2-API-Vorhersageendpunkts

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Gehen Sie in der Adresse an das Ende der URL, geben Sie `turn off the living room light` ein, und drücken Sie dann die EINGABETASTE. 

    #### <a name="v2-prediction-endpointtabv2"></a>[V2-Vorhersageendpunkt](#tab/V2)

    `https://<region>.api.cognitive.microsoft.com/luis/**v2.0**/apps/<appID>?subscription-key=<YOUR_KEY>&**q=<user-utterance-text>**`

    Der Browser zeigt die **V2-API**-Version der JSON-Antwort Ihres HTTP-Endpunkts an.

    ```json
    {
      "query": "turn off the lights",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.995867
      },
      "entities": [
        {
          "entity": "lights",
          "type": "HomeAutomation.DeviceType",
          "startIndex": 13,
          "endIndex": 18,
          "resolution": {
            "values": [
              "light"
            ]
          }
        }
      ]
    }
    ```
    
    #### <a name="v3-prediction-endpointtabv3"></a>[V3-Vorhersageendpunkt](#tab/V3)

    Ändern Sie für eine [V3-API-Abfrage](luis-migration-api-v3.md) im Browser die HTTPS-Anforderung der GET-Methode. Ersetzen Sie dabei die Werte in eckigen Klammern durch Ihre eigenen Werte.     

    `https://<region>.api.cognitive.microsoft.com/luis/**v3.0-preview**/apps/<appID>/**slots**/**production**/**predict**?subscription-key=<YOUR_KEY>&**query=<user-utterance-text>**`

    ```json
    {
        "query": "turn off the lights",
        "prediction": {
            "normalizedQuery": "turn off the lights",
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.99649024
                }
            },
            "entities": {
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ]
            }
        }
    }
    ```


    Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).
    
    * * * 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können den Endpunkt im Code aufrufen:

> [!div class="nextstepaction"]
> [Aufrufen eines LUIS-Endpunkts mithilfe von Code](luis-get-started-cs-get-intent.md)
