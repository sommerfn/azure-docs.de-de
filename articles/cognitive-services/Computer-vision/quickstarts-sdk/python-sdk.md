---
title: 'Schnellstart: Clientbibliothek für maschinelles Sehen für Python | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek der Maschinelles Sehen-API für Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/10/2019
ms.author: pafarley
ms.openlocfilehash: 8d47ae84fd489b4841d8bcf7755da6c30cf6035d
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966995"
---
# <a name="quickstart-computer-vision-client-library-for-python"></a>Schnellstart: Clientbibliothek der Maschinelles Sehen-API für Python

Über den Dienst für maschinelles Sehen haben Entwickler Zugriff auf erweiterte Algorithmen für die Bildverarbeitung und die Rückgabe von Informationen. Algorithmen für maschinelles Sehen analysieren den Inhalt eines Bilds auf unterschiedliche Weise – je nachdem, für welche visuellen Merkmale Sie sich interessieren.

Verwenden Sie die Clientbibliothek der Maschinelles Sehen-API für Python für Folgendes:

* Analysieren eines Bilds auf Tags, Textbeschreibungen, Gesichter, nicht jugendfreie Inhalte usw.
* Erkennen von gedrucktem und handschriftlichem Text mit der Batch Read-API

> [!NOTE]
> In den Szenarien in diesem Schnellstart werden Remotebild-URLs verwendet. Beispielcode, der die gleichen Vorgänge für lokale Bilder durchführt, finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).

[Referenzdokumentation](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-computervision) | [Paket (PiPy)](https://pypi.org/project/azure-cognitiveservices-vision-computervision/) | [Beispiele](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Einrichten

### <a name="create-a-computer-vision-azure-resource"></a>Erstellen einer Azure-Ressource für maschinelles Sehen

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für maschinelles Sehen. Weitere Funktionen:

* Rufen Sie einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, der sieben Tage lang gültig ist. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und eine Endpunkt-URL mit den Namen `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT`.
 
### <a name="create-a-new-python-application"></a>Erstellen einer neuen Python-Anwendung

Erstellen Sie ein neues Python-Skript, etwa *quickstart-file.py*. Öffnen Sie es anschließend in Ihrem bevorzugten Editor bzw. Ihrer bevorzugten IDE, und importieren Sie die folgenden Bibliotheken:

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_imports)]

Erstellen Sie dann Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_vars)]

> [!NOTE]
> Wenn Sie die Umgebungsvariable nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Sie können die Clientbibliothek wie folgt installieren:

```console
pip install --upgrade azure-cognitiveservices-Computer Vision
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen des Python SDK für maschinelles Sehen.

|NAME|BESCHREIBUNG|
|---|---|
|[ComputerVisionClientOperationsMixin](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.operations.computervisionclientoperationsmixin?view=azure-python)| Diese Klasse verarbeitet direkt alle Bildvorgänge, etwa Bildanalyse, Texterkennung und Erstellung von Miniaturansichten.|
| [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python) | Diese Klasse wird für alle Funktionen der Maschinelles Sehen-API benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen. Sie implementiert **ComputerVisionClientOperationsMixin**.|
|[VisualFeatureTypes](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python)| Diese Enumeration definiert die verschiedenen Typen der Bildanalyse, die bei einem standardmäßigen Analysevorgang ausgeführt werden können. Sie geben abhängig von Ihren Anforderungen verschiedene **VisualFeatureTypes**-Werte an. |

## <a name="code-examples"></a>Codebeispiele

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Clientbibliothek der Maschinelles Sehen-API für Python ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Analysieren eines Bilds](#analyze-an-image)
* [Lesen von gedrucktem und handschriftlichem Text](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE]
> In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie für den Maschinelles Sehen-Schlüssel namens `COMPUTER_VISION_SUBSCRIPTION_KEY` [eine Umgebungsvariable erstellt haben](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication).

Instanziieren Sie einen Client mit Ihrem Endpunkt und Schlüssel. Erstellen Sie ein [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python)-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [ComputerVisionClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python)-Objekt zu erstellen.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_client)]

## <a name="analyze-an-image"></a>Analysieren von Bildern

Speichern Sie einen Verweis auf die URL eines Bilds, das Sie analysieren möchten.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_remoteimage)]

### <a name="get-image-description"></a>Abrufen der Bildbeschreibung

Mit dem folgenden Code wird die Liste der generierten Beschriftungen für das Bild abgerufen. Weitere Informationen finden Sie unter [Beschreiben von Bildern in lesbarer Sprache](../concept-describing-images.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_describe)]

### <a name="get-image-category"></a>Abrufen der Bildkategorie

Mit dem folgenden Code wird die erkannte Kategorie des Bilds abgerufen. Weitere Informationen finden Sie unter [Kategorisieren von Bildern nach Schlüsselinhalt](../concept-categorizing-images.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_categorize)]

### <a name="get-image-tags"></a>Abrufen von Bildtags

Mit dem folgenden Code werden die erkannten Tags im Bild abgerufen. Weitere Informationen finden Sie unter [Inhaltstags](../concept-tagging-images.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_tags)]

### <a name="detect-objects"></a>Erkennen von Objekten

Der folgende Code erkennt alltägliche Objekte im Bild und druckt sie in der Konsole. Weitere Informationen finden Sie unter [Objekterkennung](../concept-object-detection.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-brands"></a>Erkennen von Marken

Der folgende Code erkennt Marken und Firmenlogos im Bild und druckt sie in der Konsole. Weitere Informationen finden Sie unter [Markenerkennung](../concept-brand-detection.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_objects)]

### <a name="detect-faces"></a>Erkennen von Gesichtern

Mit dem folgenden Code werden die erkannten Gesichter im Bild mit den Rechteckkoordinaten und ausgewählten Gesichtsattributen zurückgegeben. Weitere Informationen finden Sie unter [Gesichtserkennung mit maschinellem Sehen](../concept-detecting-faces.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_faces)]

### <a name="detect-adult-or-racy-content"></a>Erkennen von nicht jugendfreien oder freizügigen Inhalten

Der folgende Code gibt an, ob nicht jugendfreie oder freizügige Inhalte im Bild erkannt wurden. Weitere Informationen finden Sie unter [Erkennen von nicht jugendfreien und freizügigen Inhalten](../concept-detecting-adult-content.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_adult)]

### <a name="get-image-color-scheme"></a>Abrufen des Farbschemas für ein Bild

Mit dem folgenden Code werden die die erkannten Farbattribute im Bild ausgegeben, etwa die vorherrschenden Farben und die Akzentfarbe. Weitere Informationen finden Sie unter [Erkennen von Farbschemas auf Bildern](../concept-detecting-color-schemes.md).

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_color)]

### <a name="get-domain-specific-content"></a>Abrufen des domänenspezifischen Inhalts

Maschinelles Sehen kann spezialisierte Modelle zur weiteren Analyse von Bildern verwenden. Weitere Informationen finden Sie unter [Erkennen domänenspezifischer Inhalte](../concept-detecting-domain-content.md). 

Der folgende Code analysiert Daten zu erkannten Prominenten im Bild:

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_celebs)]

Der folgende Code analysiert Daten zu erkannten Sehenswürdigkeiten im Bild:

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_landmarks)]

### <a name="get-the-image-type"></a>Abrufen des Bildtyps

Der folgende Code gibt Informationen über den Typ des Bilds aus, und zwar unabhängig davon, ob es sich um ClipArt oder eine linienbasierte Zeichnung handelt.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_type)]

## <a name="read-printed-and-handwritten-text"></a>Lesen von gedrucktem und handschriftlichem Text

Maschinelles Sehen kann sichtbaren Text in einem Bild lesen und in eine Zeichenfolge konvertieren. Dies erfolgt in zwei Teilen.

### <a name="call-the-read-api"></a>Aufrufen der Lese-API

Verwenden Sie als Erstes den folgenden Code, um die Methode **batch_read_file** für das entsprechende Bild aufzurufen. Dies gibt eine Vorgangs-ID zurück und startet einen asynchronen Prozess, um den Inhalt des Bilds zu lesen.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_call)]

### <a name="get-read-results"></a>Abrufen der Leseergebnisse

Rufen Sie als Nächstes die Vorgangs-ID ab, die vom **batch_read_file**-Aufruf zurückgegeben wurde, und verwenden Sie sie zum Abfragen des Diensts auf Vorgangsergebnisse. Der folgende Code überprüft den Vorgang in Intervallen von einer Sekunde, bis die Ergebnisse zurückgegeben werden. Anschließend werden die extrahierten Textdaten in der Konsole ausgegeben.

[!code-python[](~/cognitive-services-quickstart-code/python/ComputerVision/ComputerVisionQuickstart.py?name=snippet_read_response)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `python` für die Schnellstartdatei aus.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Bibliothek für maschinelles Sehen für Python für allgemeine Aufgaben verwenden. In der Referenzdokumentation finden Sie weitere Informationen zur Bibliothek.


> [!div class="nextstepaction"]
>[Referenz zur Maschinelles Sehen-API (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)

* [Was ist die Maschinelles Sehen-API?](../Home.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/ComputerVision/ComputerVisionQuickstart.py).