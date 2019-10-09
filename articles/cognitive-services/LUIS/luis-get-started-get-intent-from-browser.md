---
title: 'Schnellstart: Abrufen der Absicht im Browser – LUIS'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung bestimmen Sie im Browser mithilfe einer verfügbaren öffentlichen LUIS-App aus Konversationstext die Absicht eines Benutzers.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 6282e768ebc51d0d4ec2b15f057727f207a7d81a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703601"
---
# <a name="quickstart-get-intent-with-a-browser"></a>Schnellstart: Abrufen der Absicht im Browser

Um zu verstehen, was ein LUIS-Vorhersageendpunkt zurückgibt, zeigen Sie ein Vorhersageergebnis in einem Webbrowser an. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Abfragen einer öffentlichen App benötigen Sie Folgendes:

* Einen eigenen LUIS-Schlüssel (LUIS). Wenn Sie noch kein Abonnement besitzen, um einen Schlüssel zu erstellen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
* Die ID der öffentlichen App: `df67dcdb-c37d-46af-88e1-8b97951ca1c2` 

## <a name="use-the-browser-to-see-predictions"></a>Verwenden des Browsers zum Anzeigen von Vorhersagen

1. Öffnen Sie einen Webbrowser. 
1. Verwenden Sie die unten aufgeführten vollständigen URLs, und ersetzen Sie `{your-key}` durch Ihren eigenen LUIS-Schlüssel. Die Anforderungen sind GET-Anforderungen und enthalten die Autorisierung mit dem LUIS-Schlüssel als Abfragezeichenfolgen-Parameter.

    #### <a name="v2-prediction-endpoint-requesttabv2"></a>[V2 – Anforderung für Vorhersageendpunkt](#tab/V2)
    
    Das Format der V2-URL für eine **GET**-Endpunktanforderung ist wie folgt:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key={your-key}&q=turn on all lights
    `
    
    #### <a name="v3-prediction-endpoint-requesttabv3"></a>[V3 – Anforderung für Vorhersageendpunkt](#tab/V3)
    
    
    Das Format der V3-URL für eine **GET**-Endpunktanforderung (nach Slots) ist wie folgt:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}
    `
    
    * * *

1. Fügen Sie die URL in ein Browserfenster ein, und drücken Sie die EINGABETASTE. Das im Browser angezeigte JSON-Ergebnis gibt an, dass LUIS die Absicht `HomeAutomation.TurnOn` als die am höchsten bewertete Absicht und die Entität `HomeAutomation.Operation` mit dem Wert `on` erkennt.

    #### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 – Antwort für Vorhersageendpunkt](#tab/V2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    #### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 – Antwort für Vorhersageendpunkt](#tab/V3)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    * * *

1. Fügen Sie den entsprechenden Abfragezeichenfolgen-Parameter hinzu, um alle Absichten anzuzeigen. 

    #### <a name="v2-prediction-endpointtabv2"></a>[V2-Vorhersageendpunkt](#tab/V2)

    Fügen Sie `verbose=true` am Ende der Abfragezeichenfolge hinzu, um **alle Absichten anzuzeigen**:

    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key={your-key}&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    #### <a name="v3-prediction-endpointtabv3"></a>[V3-Vorhersageendpunkt](#tab/V3)

    Fügen Sie `show-all-intents=true` am Ende der Abfragezeichenfolge hinzu, um **alle Absichten anzuzeigen**:

    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "HomeAutomation.TurnOff": {
                     "score": 0.0207554
                },
                "None": {
                     "score": 0.08687421
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    * * * 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

> [!div class="nextstepaction"]
> [Erstellen einer App im LUIS-Portal](get-started-portal-build-app.md)