---
title: Abrufen der Absicht mit einem REST-Aufruf in Go
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/17/2019
ms.author: diberry
ms.openlocfilehash: 6d2955a77c06f371975a7a14675eedceab7c98ae
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74125493"
---
## <a name="prerequisites"></a>Voraussetzungen

* Programmiersprache [Go](https://golang.org/)  
* [Visual Studio Code](https://code.visualstudio.com/)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie Go, um den Vorhersageendpunkt GET [API](https://aka.ms/luis-apim-v3-prediction) abzurufen, um das Vorhersageergebnis zu erhalten.

1. Erstellen Sie eine neue Datei mit dem Namen `predict.go`. Fügen Sie den folgenden Code hinzu:
    
    ```go
    package main
    
    /* Do dependencies */
    import (
        "fmt"
        "net/http"
        "net/url"
        "io/ioutil"
        "log"
    )
    func main() {
        
        // public app
        var appID = "df67dcdb-c37d-46af-88e1-8b97951ca1c2"
        
        // utterance for public app
        var utterance = "turn on all lights"
        
        // YOUR-KEY - your starter or prediction key
        var endpointKey = "YOUR-KEY"
        
        // YOUR-ENDPOINT - example is westus2.api.cognitive.microsoft.com
        var endpoint = "YOUR-ENDPOINT"
    
        endpointPrediction(appID, endpointKey, endpoint, utterance)
    }
    func endpointPrediction(appID string, endpointKey string, endpoint string, utterance string) {
    
        var endpointUrl = fmt.Sprintf("https://%s/luis/prediction/v3.0/apps/%s/slots/production/predict?subscription-key=%s&verbose=true&show-all-intents=true&query=%s", endpoint, appID, endpointKey, url.QueryEscape(utterance))
        
        response, err := http.Get(endpointUrl)
    
        if err!=nil {
            // handle error
            fmt.Println("error from Get")
            log.Fatal(err)
        }
        
        response2, err2 := ioutil.ReadAll(response.Body)
    
        if err2!=nil {
            // handle error
            fmt.Println("error from ReadAll")
            log.Fatal(err2)
        }
    
        fmt.Println("response")
        fmt.Println(string(response2))
    }
    ```

1. Ersetzen Sie die folgenden Werte:

    * `YOUR-KEY` durch Ihren Startschlüssel
    * `YOUR-ENDPOINT` durch Ihren Endpunkt (Beispiel: `westus2.api.cognitive.microsoft.com`)

1. Geben Sie in dem Verzeichnis, in dem Sie die Datei erstellt haben, an einer Eingabeaufforderung den folgenden Befehl ein, um die Go-Datei zu kompilieren:

    ```console
    go build predict.go
    ```  

1. Führen Sie die Go-Anwendung über die Befehlszeile aus, indem Sie den folgenden Text an der Eingabeaufforderung eingeben: 

    ```console
    go run predict.go
    ```
    
    Die Antwort der Eingabeaufforderung sieht wie folgt aus: 
    
    ```console
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value a7b206911f714e71a1ddae36928a61cc
    endpoint has value westus2.api.cognitive.microsoft.com
    utterance has value turn on all lights
    response
    {"query":"turn on all lights","prediction":{"topIntent":"HomeAutomation.TurnOn","intents":{"HomeAutomation.TurnOn":{"score":0.5375382},"None":{"score":0.08687421},"HomeAutomation.TurnOff":{"score":0.0207554}},"entities":{"HomeAutomation.Operation":["on"],"$instance":{"HomeAutomation.Operation":[{"type":"HomeAutomation.Operation","text":"on","startIndex":5,"length":2,"score":0.724984169,"modelTypeId":-1,"modelType":"Unknown","recognitionSources":["model"]}]}}}}
    ```

    Zur besseren Lesbarkeit formatierter JSON-Code:

    ```json
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
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


## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Datei aus dem Dateisystem, nachdem Sie diese Schnellstartanleitung durchgearbeitet haben. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren](../get-started-get-model-rest-apis.md)