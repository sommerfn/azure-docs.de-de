---
title: Abrufen eines Modells mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/18/2019
ms.author: diberry
ms.openlocfilehash: 8cefd8357893657d94959cb853004b34b0ec9d8d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505843"
---
## <a name="prerequisites"></a>Voraussetzungen

* Starterschlüssel.
* Importieren Sie die [TravelAgent-App](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) aus dem GitHub-Repository „cognitive-services-language-understanding“.
* Die LUIS-Anwendungs-ID für die importierte TravelAgent-App. Die Anwendungs-ID wird auf dem Anwendungsdashboard angezeigt.
* Die Versions-ID in der Anwendung, die die Äußerungen empfängt. Die Standard-ID lautet „0.1“.
* [Python 3.6](https://www.python.org/downloads/) oder höher
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>JSON-Datei mit Beispieläußerungen

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]


## <a name="change-model-programmatically"></a>Programmgesteuertes Ändern des Modells

Verwenden Sie Go, um der Anwendung eine maschinell gelernte Entitäts-[API](https://aka.ms/luis-apim-v3-authoring) hinzuzufügen. 

1. Erstellen Sie eine neue Datei mit dem Namen `model.py`. Fügen Sie den folgenden Code hinzu:

    ```python
    ########### Python 3.6 #############
    import requests
    
    # Starter key
    LUIS_authoringKey  = "YOUR-KEY"
    
    LUIS_APP_ID = "YOUR-APP-ID"
    
    # Authoring endpoint, example: westus.api.cognitive.microsoft.com
    LUIS_ENDPOINT = "YOUR-ENDPOINT"

    # The version number of your LUIS app
    LUIS_APP_VERSION = "0.1"
    
    URI_AddUtterances = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_ID}/examples'
    URI_Train = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_ID}/train'
    
    HEADERS = {'Ocp-Apim-Subscription-Key': LUIS_authoringKey}
    
    def addUtterances():
        r = requests.post(URI_AddUtterances,headers=HEADERS)
        print(r.json())
    
    def train():
        r = requests.post(URI_Train,headers=HEADERS)
        print(r.json())
    
    def trainStatus():
        r = requests.get(URI_Train,headers=HEADERS)
        print(r.json())
    
    addUtterances()
    train()
    trainStatus()
    ```
1. Ersetzen Sie die folgenden Werte:

    * `YOUR-KEY` mit ihrem Starterschlüssel
    * `YOUR-ENDPOINT` mit ihrem Endpunkt, z. B. `westus2.api.cognitive.microsoft.com`
    * `YOUR-APP-ID` mit der ID Ihrer App

1. Geben Sie in dem Verzeichnis, in dem Sie die Datei erstellt haben, an einer Eingabeaufforderung den folgenden Befehl ein, um die Datei auszuführen:

    ```console
    python model.py
    ```  

## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit dieser Schnellstartanleitung fertig sind, löschen Sie die Datei aus dem Dateisystem. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bewährte Methoden für eine App](../luis-concept-best-practices.md)