---
title: Verwenden von Machine Learning-Endpunkten in Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie benutzerdefinierte Machine Language-Funktionen in Azure Stream Analytics verwendet werden.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 1adb7d58246ea37fd8322cb6fc6ffd53c5f19efb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467820"
---
# <a name="azure-machine-learning-studio-classic-integration-in-stream-analytics-preview"></a>Azure Machine Learning Studio-Integration (klassisch) in Stream Analytics (Vorschau)
Stream Analytics unterstützt benutzerdefinierte Funktionen, die Azure Machine Learning Studio-Endpunkte (klassisch) kontaktieren. Die REST-API-Unterstützung für dieses Feature ist in der [Stream Analytics-REST-API-Bibliothek](https://msdn.microsoft.com/library/azure/dn835031.aspx)ausführlich beschrieben. Dieser Artikel enthält zusätzliche Informationen, die für die erfolgreiche Implementierung dieser Funktion in Stream Analytics erforderlich sind. Es wurde ein Tutorial bereitgestellt, das [hier](stream-analytics-machine-learning-integration-tutorial.md)verfügbar ist.

## <a name="overview-azure-machine-learning-studio-classic-terminology"></a>Übersicht: Azure Machine Learning Studio-Terminologie (klassisch)
Microsoft Azure Machine Learning Studio (klassisch) ist ein Drag & Drop-Tool für die Zusammenarbeit, mit dem Sie Predictive Analytics-Lösungen für Ihre Daten erstellen, testen und bereitstellen können. Dieses Tool hat den Namen *Azure Machine Learning Studio (klassisch)* . Das Tool wird zum Interagieren mit den Machine Learning-Ressourcen und zum einfachen Erstellen, Testen und Durchlaufen Ihres Entwurfs verwendet. Unten sind diese Ressourcen und die dazugehörigen Definitionen angegeben.

* **Arbeitsbereich**: Der *Arbeitsbereich* ist ein Container, in dem alle anderen Machine Learning-Ressourcen zur Verwaltung und Steuerung zusammengefasst sind.
* **Experiment**: *Experimente* werden von Data Scientists erstellt, um Datasets zu nutzen und ein Machine Learning-Modell zu trainieren.
* **Endpunkt**: Bei *Endpunkten* handelt es sich um das Azure Machine Learning Studio-Objekt (klassisch), das zum Verwenden von Features als Eingabe, Anwenden eines angegebenen Machine Learning-Modells und Zurückgeben der erzielten Ausgabe eingesetzt wird.
* **Bewertungswebdienst**: Ein *Bewertungswebdienst* ist eine Sammlung von Endpunkten (wie oben beschrieben).

Jeder Endpunkt verfügt über APIs für die Batchausführung und synchrone Ausführung. Für Stream Analytics wird die synchrone Ausführung verwendet. Der entsprechende Dienst wird in Azure Machine Learning Studio (klassisch) als [Anforderung-Antwort-Dienst](../machine-learning/studio/consume-web-services.md) bezeichnet.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Für Stream Analytics-Aufträge erforderliche Machine Learning-Ressourcen
Bei der Verarbeitung von Stream Analytics-Aufträgen sind für eine erfolgreiche Ausführung ein Anforderung/Antwort-Endpunkt, ein [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md)und eine Swagger-Definition erforderlich. Stream Analytics verfügt über einen zusätzlichen Endpunkt, der die URL für den Swagger-Endpunkt erstellt, nach der Schnittstelle sucht und eine UDF-Standarddefinition für den Benutzer zurückgibt.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurieren einer Stream Analytics- und Machine Learning-UDF per REST-API
Mit REST-APIs können Sie Ihren Auftrag so konfigurieren, dass er Azure Machine Language-Funktionen aufruft. Die Schritte lauten wie folgt:

1. Erstellen eines Stream Analytics-Auftrags
2. Definieren einer Eingabe
3. Definieren einer Ausgabe
4. Erstellen einer benutzerdefinierten Funktion (UDF)
5. Schreiben einer Stream Analytics-Transformation zum Aufrufen der UDF
6. Starten des Auftrags

## <a name="creating-a-udf-with-basic-properties"></a>Erstellen einer UDF mit grundlegenden Eigenschaften
Beispielsweise wird im folgenden Beispielcode eine Skalar-UDF namens *newudf* erstellt, die an einen Azure Machine Learning Studio-Endpunkt (klassisch) gebunden ist. Beachten Sie, dass sich der *Endpunkt* (Dienst-URI) auf der API-Hilfeseite für den gewählten Dienst und der *apiKey* auf der Hauptseite der Dienste befindet.

```
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Beispiel für Anforderungstext:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
```

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Aufrufen des RetrieveDefaultDefinition-Endpunkts für standardmäßige UDF
Nachdem das UDF-Gerüst erstellt wurde, wird die vollständige Definition der UDF benötigt. Über den RetrieveDefaultDefinition-Endpunkt können Sie die Standarddefinition für eine Skalarfunktion erhalten, die an einen Azure Machine Learning Studio-Endpunkt (klassisch) gebunden ist. Für die unten angegebene Nutzlast ist es erforderlich, dass Sie die UDF-Standarddefinition für eine Skalarfunktion abrufen, die an einen Azure Machine Learning-Endpunkt gebunden ist. Hierbei wird der eigentliche Endpunkt nicht angegeben, da er bereits während der PUT-Anforderung bereitgestellt wurde. Stream Analytics ruft den in der Anforderung angegebenen Endpunkt auf, falls er explizit bereitgestellt wird. Andernfalls wird der ursprünglich angegebene Endpunkt verwendet. Hier verwendet die UDF einen einzelnen Zeichenfolgenparameter (einen Satz) und gibt eine Einzelausgabe einer Typzeichenfolge zurück, mit der die Bezeichnung „sentiment“ (Stimmung) für diesen Satz angegeben wird.

```
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
```

Beispiel für Anforderungstext:

```json
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
```

Eine Beispielausgabe sieht hierfür ungefähr wie folgt aus:

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="patch-udf-with-the-response"></a>Patchen der UDF mit der Antwort
Jetzt muss die UDF mit der vorherigen Antwort wie unten gezeigt gepatcht werden.

```
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
```

Anforderungstext (Ausgabe von „RetrieveDefaultDefinition“):

```json
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
```

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementieren der Stream Analytics-Transformation für den Aufruf der UDF
Fragen Sie die UDF (hier: scoreTweet) jetzt in Bezug auf alle Eingabeereignisse ab, und schreiben Sie eine Antwort für das Ereignis in eine Ausgabe.

```json
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
```


## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
