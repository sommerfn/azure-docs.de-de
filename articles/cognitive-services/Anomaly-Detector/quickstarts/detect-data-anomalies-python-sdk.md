---
title: 'Schnellstart: Erkennen von Datenanomalien mithilfe der Anomalieerkennungsbibliothek und Python | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie die Anomalieerkennungs-API, um Anomalien in Ihren Datenreihen als Batch oder in Streamingdaten zu erkennen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 07/01/2019
ms.author: aahi
ms.openlocfilehash: 1d89ed8f40547142d41af9c587fc8fc000fa4dd9
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503663"
---
# <a name="quickstart-anomaly-detector-client-library-for-python"></a>Schnellstart: Anomalieerkennungs-Clientbibliothek für Python

Erfahren Sie etwas über die ersten Schritte mit der Anomalieerkennungs-Clientbibliothek für .NET. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Mit dem Anomalieerkennungsdienst können Sie Anomalien in Zeitreihendaten ermitteln, da unabhängig von der Branche, dem Szenario oder der Datenmenge automatisch die am besten geeigneten Modelle für Ihre Daten angewandt werden.

## <a name="key-concepts"></a>Wichtige Begriffe

Mit der Anomalieerkennungs-Clientbibliothek für Python ist Folgendes möglich:

* Erkennung von Anomalien in Ihrem gesamten Zeitreihen-Dataset als Batchanforderung
* Erkennen des Anomaliestatus des letzten Datenpunkts in Ihrer Zeitreihe

[Bibliotheksreferenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [Paket (PyPi)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/) | [Beispiele](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* Die [Pandas-Bibliothek für Datenanalyse](https://pandas.pydata.org/)
 
## <a name="setting-up"></a>Einrichten

### <a name="create-an-anomaly-detector-resource"></a>Erstellen einer Anomalieerkennungsressource

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Nach der Installation von Python, können Sie die Clientbibliothek mit Folgendem installieren:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Objektmodell

Der Anomalieerkennungsclient ist ein [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python)-Objekt, das sich mit Ihrem Schlüssel bei Azure authentifiziert. Der Client stellt zwei Methoden zur Anomalieerkennung bereit: Für ein gesamtes Dataset mithilfe von [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) und für den letzten Datenpunkt mithilfe von [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-). 

Zeitreihendaten werden als eine Reihe von [Points](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point(class)?view=azure-python) in einem [Request](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request(class)?view=azure-python)-Objekt gesendet. Das `Request`-Objekt enthält Eigenschaften zum Beschreiben der Daten (z.B. [Granularity](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python)) sowie Parameter für die Anomalieerkennung. 

Die Antwort der Anomalieerkennung ist je nach der verwendeten Methode ein [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)- oder [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)-Objekt. 

## <a name="getting-started"></a>Erste Schritte

Erstellen Sie eine neue Python-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Fügen Sie der Datei dann die folgenden Importdeklarationen hinzu. 

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

> [!NOTE]
> In diesem Schnellstart wird vorausgesetzt, dass Sie für den Schlüssel für die Anomalieerkennung mit dem Namen `ANOMALY_DETECTOR_KEY` [eine Umgebungsvariable erstellt haben](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication).

Erstellen Sie für Ihren Schlüssel Variablen wie eine Umgebungsvariable, den Pfad zu einer Zeitreihen-Datendatei und den Azure-Speicherort Ihres Abonnements. Beispiel: `westus2`. 

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

## <a name="code-examples"></a>Codebeispiele 

Diese Codeausschnitte veranschaulichen, wie folgende Vorgänge mit der Anomalieerkennungs-Clientbibliothek für .NET durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Laden eines Zeitreihendatasets aus einer Datei](#load-time-series-data-from-a-file)
* [Erkennen von Anomalien im gesamten Dataset](#detect-anomalies-in-the-entire-data-set) 
* [Erkennen des Anomaliestatus des letzten Datenpunkts](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="authenticate-the-client"></a>Authentifizieren des Clients

Fügen Sie Ihre Azure-Speicherortvariable dem Endpunkt hinzu, und authentifizieren Sie den Client mit Ihrem Schlüssel.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

### <a name="load-time-series-data-from-a-file"></a>Laden von Zeitreihendaten aus einer Datei

Laden Sie die Beispieldaten für diesen Schnellstart von [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv) herunter:
1. Klicken Sie im Browser mit der rechten Maustaste auf **Raw**.
2. Klicken Sie auf **Link speichern unter**.
3. Speichern Sie die Datei als CSV-Datei in Ihrem Anwendungsverzeichnis.

Die Zeitreihendaten werden als CSV-Datei formatiert und an die Anomalieerkennungs-API gesendet.

Laden Sie die Datendatei mit der `read_csv()`-Methode der Pandas-Bibliothek, und erstellen Sie eine leere Listenvariable zum Speichern der Datenreihe. Durchlaufen Sie die Datei, und fügen Sie die Daten als [Point](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point%28class%29?view=azure-python)-Objekt an. Dieses Objekt enthält den Zeitstempel und den numerischen Wert aus den Zeilen der CSV-Datendatei. 

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Erstellen Sie ein [Request](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request%28class%29?view=azure-python)-Objekt mit der Zeitreihe und der [Granularity](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (oder Periodizität) der zugehörigen Datenpunkte. Beispiel: `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Erkennen von Anomalien im gesamten Dataset 

Rufen Sie die API zum Erkennen von Anomalien in den gesamten Zeitreihendaten mit der [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)-Methode des Clients auf. Speichern Sie das zurückgegebene [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python)-Objekt. Durchlaufen Sie die `is_anomaly`-Liste der Antwort, und geben Sie den Index aller `true`-Werte aus. Diese Werte stimmen mit dem Index der anomalen Datenpunkte überein, sofern welche gefunden wurden.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Erkennen des Anomaliestatus des letzten Datenpunkts

Rufen Sie mithilfe der [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-)-Methode des Clients die Anomalieerkennungs-API auf, um festzustellen, ob Ihr letzter Datenpunkt eine Anomalie ist, und speichern Sie das zurückgegebene [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python)-Objekt. Der `is_anomaly`-Wert der Antwort ist ein Boolescher Wert, der den Anomaliestatus dieses Punktes angibt.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung in Ihrer IDE oder in der Befehlszeile mit dem `python`-Befehl und Ihrem Dateinamen aus.
 
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die der Ressourcengruppe zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Sie können außerdem den folgenden Cloud Shell-Befehl ausführen, um die Ressourcengruppe und die zugehörigen Ressourcen zu entfernen. Dies kann einige Minuten in Anspruch nehmen. 

```azurecli-interactive
az group delete --name example-anomaly-detector-resource-group
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Anomalieerkennung in Streamingdaten mit Azure Databricks](../tutorials/anomaly-detection-streaming-databricks.md)

* Was ist die [Anomalieerkennungs-API](../overview.md)?
* [Bewährte Methoden](../concepts/anomaly-detection-best-practices.md) bei der Verwendung der Anomalieerkennungs-API.
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs).
