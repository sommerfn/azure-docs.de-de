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
ms.date: 11/04/2019
ms.author: diberry
ms.openlocfilehash: 22a37dbd0b76710a14183aec1795639614207d16
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73613704"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Schnellstart: Verwenden der vordefinierten Home Automation-App

[!INCLUDE [Uses preview portal](./includes/uses-portal-preview.md)]

In dieser Schnellstartanleitung erstellen Sie eine LUIS-App, die die vordefinierte Domäne `HomeAutomation` zum Ein- und Ausschalten von Beleuchtungen und Haushaltsgeräten verwendet. Diese vordefinierte Domäne bietet Absichten, Entitäten und Beispieläußerungen, die Sie nutzen können. Am Ende dieser Schnellstartanleitung verfügen Sie über einen LUIS-Endpunkt in der Cloud.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Artikel benötigen Sie ein kostenloses LUIS-Konto, das Sie im LUIS-Portal unter [https://www.luis.ai](https://www.luis.ai) erstellt haben. 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

[!INCLUDE [Select authoring resource](./includes/select-authoring-resource.md)]

## <a name="create-a-new-app"></a>Erstellen einer neuen App
Sie können Ihre Anwendungen auf der Seite **Meine Apps** erstellen und verwalten. 

1. Wählen Sie im LUIS-Portal in der Liste „Meine Apps“ die Option **+ Erstellen** aus.

    ![Wählen Sie im LUIS-Portal in der Liste „Meine Apps“ die Option „+ Erstellen“ aus.](./media/create-app-in-portal.png)

1. Geben Sie der Anwendung im Dialogfeld den Namen `Home Automation`, und wählen Sie anschließend **Fertig** aus. LUIS erstellt die App. Die Beschreibung ist optional und wird nicht für die Erstellung oder Vorhersage verwendet. Die Vorhersageressource ist auch bei der Erstellung einer LUIS-App optional. Bei der Veröffentlichung Ihrer App in der Produktion müssen Sie eine Vorhersageressource zuweisen, damit Ihre App zahlreiche Anforderungen verarbeiten kann.  

    ![Geben Sie Ihrer Anwendung im Dialogfeld den Namen „Home Automation“.](./media/create-new-app-details.png)

    >[!NOTE]
    >Die Kultur kann nach dem Erstellen der Anwendung nicht mehr geändert werden. 

## <a name="add-prebuilt-domain"></a>Hinzufügen einer vordefinierten Domäne

Wählen Sie die Option **Vordefinierte Domänen** aus, und suchen Sie nach **HomeAutomation**. Wählen Sie auf der Karte „HomeAutomation“ die Option **Domäne hinzufügen** aus.

![Wählen Sie die Option „Vordefinierte Domänen“ aus, und suchen Sie nach „HomeAutomation“. Wählen Sie auf der Karte „HomeAutomation“ die Option „Domäne hinzufügen“ aus.](media/luis-quickstart-new-app/home-automation.png)

Wenn die Domäne erfolgreich hinzugefügt wurde, enthält das Feld für die vordefinierte Domäne eine Schaltfläche **Remove domain** (Domäne entfernen).

## <a name="intents-and-entities"></a>Absichten und Entitäten

Wählen Sie **Absichten** aus, um die Absichten der HomeAutomation-Domäne anzuzeigen. Die Absichten der vordefinierten Domäne verfügen über Beispieläußerungen.

![Screenshot: Liste mit HomeAutomation-Absichten](media/luis-quickstart-new-app/home-automation-intents.png "Screenshot: Liste mit HomeAutomation-Absichten")

> [!NOTE]
> **None** (Keine) ist eine Absicht, die von allen LUIS-Apps bereitgestellt wird. Diese Absicht verwenden Sie zum Behandeln von Äußerungen, die keiner Funktionalität Ihrer App entsprechen. 

Wählen Sie die Absicht **HomeAutomation.TurnOff** aus. Wie Sie sehen, enthält die Absicht eine Liste von Äußerungen, die mit Entitäten bezeichnet sind.

[![Screenshot: Absicht „HomeAutomation.TurnOff“](media/luis-quickstart-new-app/home-automation-turnoff.png "Screenshot: Absicht „HomeAutomation.TurnOff“")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Trainieren der LUIS-App

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="test-your-app"></a>Testen Ihrer App
Nachdem Sie Ihre App trainiert haben, können Sie sie testen. Klicken Sie auf **Test**. Geben Sie eine Testäußerung wie `Turn off the lights` in den interaktiven Testbereich ein, und drücken Sie die EINGABETASTE. 

```
Turn off the lights
```

Überprüfen Sie, ob die Absicht mit der höchsten Bewertung der Absicht entspricht, die Sie für die einzelnen Testäußerungen erwartet haben.

In diesem Beispiel wurde `Turn off the lights` korrekt als Absicht mit der höchsten Bewertung von **HomeAutomation.TurnOff** identifiziert.

![Screenshot des Testbereichs mit hervorgehobener Äußerung](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

Wählen Sie **Überprüfen** aus, um weitere Informationen zur Vorhersage anzuzeigen.

![Screenshot: Testbereich mit Überprüfungsinformationen](media/luis-quickstart-new-app/test.png)

Klicken Sie erneut auf **Test** (Testen), um den Testbereich zuzuklappen. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Veröffentlichen der App zum Abrufen der Endpunkt-URL

[!INCLUDE [LUIS How to Publish steps](./includes/howto-publish.md)]

<a name="query-the-v2-api-prediction-endpoint"></a>

## <a name="query-the-v3-api-prediction-endpoint"></a>Abfragen des V3-API-Vorhersageendpunkts

[!INCLUDE [LUIS How to get endpoint first step](./includes/v3-prediction-endpoint.md)] 

1. Stellen Sie in der Adressleiste des Browsers für die Abfragezeichenfolge sicher, dass die URL die folgenden Name-Wert-Leisten aufweist. Fügen Sie diese Angaben hinzu, falls sie in der Abfragezeichenfolge nicht enthalten sind:

    |Name-Wert-Paar|
    |--|
    |`verbose=true`|
    |`show-all-intents=true`|

1. Navigieren Sie in der Adressleiste des Browsers an das Ende der URL, und geben Sie `turn off the living room light` für den Wert _query_ ein. Drücken Sie anschließend die EINGABETASTE.

    ```json
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.984315455
                },
                "HomeAutomation.QueryState": {
                    "score": 0.009912962
                },
                "HomeAutomation.TurnDown": {
                    "score": 0.00626645749
                },
                "HomeAutomation.TurnUp": {
                    "score": 0.00572059769
                },
                "HomeAutomation.SetDevice": {
                    "score": 0.00379381469
                },
                "HomeAutomation.TurnOn": {
                    "score": 0.00366983772
                },
                "None": {
                    "score": 0.000623856
                }
            },
            "entities": {
                "HomeAutomation.Location": [
                    "living room"
                ],
                "HomeAutomation.DeviceName": [
                    [
                        "living room light"
                    ]
                ],
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ],
                "$instance": {
                    "HomeAutomation.Location": [
                        {
                            "type": "HomeAutomation.Location",
                            "text": "living room",
                            "startIndex": 13,
                            "length": 11,
                            "score": 0.907323956,
                            "modelTypeId": 1,
                            "modelType": "Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceName": [
                        {
                            "type": "HomeAutomation.DeviceName",
                            "text": "living room light",
                            "startIndex": 13,
                            "length": 17,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "HomeAutomation.DeviceType": [
                        {
                            "type": "HomeAutomation.DeviceType",
                            "text": "light",
                            "startIndex": 25,
                            "length": 5,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

    Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).
    

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können den Endpunkt im Code aufrufen:

> [!div class="nextstepaction"]
> [Aufrufen eines LUIS-Endpunkts mithilfe von Code](luis-get-started-cs-get-intent.md)
