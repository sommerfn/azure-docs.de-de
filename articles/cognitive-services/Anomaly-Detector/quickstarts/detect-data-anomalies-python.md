---
title: 'Schnellstart: Erkennen von Anomalien als Batch mithilfe der Anomalieerkennungs-REST-API und Python'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Anomalieerkennungs-API, um Anomalien in Ihren Datenreihen als Batch oder in Streamingdaten zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 10/14/2019
ms.author: aahi
ms.openlocfilehash: 571626da0f3f43c8c2a2e33e1147418158c5473b
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754215"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Schnellstart: Erkennen von Anomalien in Zeitreihendaten mithilfe der Anomalieerkennungs-REST-API und Python

Dieser Schnellstart unterstützt Sie bei den ersten Schritten in den zwei Erkennungsmodi der Anomalieerkennungs-API, um Anomalien in Ihren Zeitreihendaten zu erkennen. Diese Python-Anwendung sendet zwei API-Anforderungen mit Zeitreihendaten im JSON-Format und erhält entsprechende Antworten.

| API-Anforderung                                        | Anwendungsausgabe                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Erkennen von Anomalien als Batch                        | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für jeden Datenpunkt in den Zeitreihendaten sowie die Positionen aller erkannten Anomalien. |
| Erkennen des Anomaliestatus des letzten Datenpunkts | Die JSON-Antwort enthält den Anomaliestatus (und weitere Daten) für den letzten Datenpunkt in den Zeitreihendaten.                                                                                                                                         |

 Diese Anwendung ist zwar in Python geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

## <a name="prerequisites"></a>Voraussetzungen

- [Python 2.x oder 3.x](https://www.python.org/downloads/)

- Die [Anforderungsbibliothek](https://pypi.org/project/requests/) für Python

- Eine JSON-Datei mit Datenpunkten auf einer Zeitreihe Die Beispieldaten für diesen Schnellstart finden Sie auf [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

### <a name="create-an-anomaly-detector-resource"></a>Erstellen einer Anomalieerkennungsressource

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]


## <a name="create-a-new-application"></a>Erstellen einer neuen Anwendung

1. Erstellen Sie eine neue Python-Datei, und fügen Sie die folgenden Importe hinzu:

    [!code-python[import statements](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=imports)]

2. Erstellen Sie Variablen für Ihren Abonnementschlüssel und Ihren Endpunkt. Unten sehen Sie die URIs, die Sie für die Anomalieerkennung verwenden können. Diese werden später dem Dienstendpunkt angefügt, um URLs für API-Anforderungen zu erstellen.

    |Erkennungsmethode  |URI  |
    |---------|---------|
    |Batcherkennung    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Erkennung am letzten Datenpunkt     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-python[initial endpoint and key variables](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=vars)]

3. Lesen Sie in der JSON-Datendatei, indem Sie sie öffnen und `json.load()` verwenden.

    [!code-python[Open JSON file and read in the data](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=fileLoad)]

## <a name="create-a-function-to-send-requests"></a>Erstellen einer Funktion zum Senden von Anforderungen

1. Erstellen Sie eine neuen Funktion namens `send_request()`, die die oben erstellten Variablen verwendet. Führen Sie anschließend die folgenden Schritte aus.

2. Erstellen Sie ein Wörterbuch für die Anforderungsheader. Setzen Sie `Content-Type` auf `application/json`, und fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu.

3. Senden Sie die Anforderung mit `requests.post()`. Kombinieren Sie den Endpunkt und die Anomalieerkennungs-URL zu einer vollständigen Anforderungs-URL, und schließen Sie die Header und JSON-Anforderungsdaten ein. Und geben Sie dann die Antwort zurück.

[!code-python[request method](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Erkennen von Anomalien als Batch

1. Erstellen Sie eine Methode namens `detect_batch()`, um Anomalien in allen Daten als Batch zu erkennen. Rufen Sie die oben erstellte Methode `send_request()` mit dem Endpunkt, der URL, dem Abonnementschlüssel und den JSON-Daten auf.

2. Rufen Sie für das Ergebnis `json.dumps()` auf, um es zu formatieren, und geben Sie es in der Konsole aus.

3. Wenn die Antwort das Feld `code` enthält, geben Sie den Fehlercode und die Fehlermeldung aus.

4. Andernfalls suchen Sie nach den Positionen der Anomalien im Dataset. Das Feld `isAnomaly` der Antwort enthält einen booleschen Wert, der angibt, ob es sich bei einem bestimmten Datenpunkt um eine Anomalie handelt. Gehen Sie die Liste durch, und geben Sie den Index aller `True`-Werte aus. Diese Werte stimmen mit dem Index der anomalen Datenpunkte überein, sofern welche gefunden wurden.

[!code-python[detection as a batch](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Erkennen des Anomaliestatus des letzten Datenpunkts

1. Erstellen Sie eine Methode namens `detect_latest()`, um zu ermitteln, ob der letzte Datenpunkt in der Zeitreihe eine Anomalie ist. Rufen Sie die oben erstellte Methode `send_request()` mit dem Endpunkt, der URL, dem Abonnementschlüssel und den JSON-Daten auf. 

2. Rufen Sie für das Ergebnis `json.dumps()` auf, um es zu formatieren, und geben Sie es in der Konsole aus.

[!code-python[Latest point detection](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectLatest)]

## <a name="send-the-request"></a>Senden der Anforderung

1. Rufen Sie die oben erstellten Methoden zur Anomalieerkennung auf.

[!code-python[Method calls](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=methodCalls)]

### <a name="example-response"></a>Beispielantwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Klicken Sie unten auf die Links, um die JSON-Antwort auf GitHub anzuzeigen:
* [Beispielantwort für die Batcherkennung](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Beispielantwort für die Erkennung des letzten Punkts](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Anomalieerkennung in Streamingdaten mit Azure Databricks](../tutorials/anomaly-detection-streaming-databricks.md)

* Was ist die [Anomalieerkennungs-API](../overview.md)?
* [Bewährte Methoden](../concepts/anomaly-detection-best-practices.md) bei der Verwendung der Anomalieerkennungs-API.
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs).
