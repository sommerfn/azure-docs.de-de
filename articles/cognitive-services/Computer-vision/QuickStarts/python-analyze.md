---
title: 'Schnellstart: Analysieren eines Remotebilds – REST, Python'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung analysieren Sie ein Remotebild mit der Maschinelles Sehen-API und Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 065fdd2e95e4ae90a7da7798dd38fdc06b1b32f5
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177140"
---
# <a name="quickstart-analyze-a-remote-image-using-the-computer-vision-rest-api-and-python"></a>Schnellstart: Analysieren eines Remotebilds mit der Maschinelles Sehen-REST-API und Python

In dieser Schnellstartanleitung analysieren Sie mithilfe der REST-API von Maschinelles Sehen ein remote gespeichertes Bild, um visuelle Merkmale zu extrahieren. Mit der Methode [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) können Sie basierend auf dem Inhalt des Bilds visuelle Merkmale extrahieren.

Sie können diese Schnellstartanleitung Schritt für Schritt mit einem Jupyter-Notebook in [MyBinder](https://mybinder.org) ausführen. Klicken Sie zum Starten von Binder auf die folgende Schaltfläche:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie das Beispiel lokal ausführen möchten, muss [Python](https://www.python.org/downloads/) installiert sein.
- Sie benötigen einen Abonnementschlüssel für maschinelles Sehen. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Schlüssel für eine kostenlose Testversion abrufen. Alternativ gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten. [Erstellen Sie dann Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für die Schlüssel- und Dienstendpunkt-Zeichenfolge, und nennen Sie sie `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT`.
- Sie müssen die folgenden Python-Pakete installiert haben: Die Python-Pakete können mit [pip](https://packaging.python.org/tutorials/installing-packages/) installiert werden.
    - requests
    - [matplotlib](https://matplotlib.org/)
    - [pillow](https://python-pillow.org/)

## <a name="create-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Text-Editor.
1. Ersetzen Sie optional den Wert von `image_url` durch die URL eines anderen Bilds, das analysiert werden soll.
1. Speichern Sie den Code als Datei mit der Erweiterung `.py`. Beispiel: `analyze-image.py`.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python analyze-image.py`.

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
import matplotlib.pyplot as plt
import json
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
if 'COMPUTER_VISION_SUBSCRIPTION_KEY' in os.environ:
    subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
else:
    print("\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

if 'COMPUTER_VISION_ENDPOINT' in os.environ:
    endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

analyze_url = endpoint + "vision/v2.1/analyze"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/" + \
    "Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'visualFeatures': 'Categories,Description,Color'}
data = {'url': image_url}
response = requests.post(analyze_url, headers=headers,
                         params=params, json=data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(json.dumps(response.json()))
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(requests.get(image_url).content))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
plt.show()
```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort wird im JSON-Format zurückgegeben. Die Beispielwebseite analysiert eine Antwort und zeigt diese bei erfolgreicher Ausführung im Eingabeaufforderungsfenster an. Im Folgenden finden Sie ein Beispiel dafür:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie eine einfache Python-Anwendung kennen, die maschinelles Sehen verwendet, um eine optische Zeichenerkennung (Optical Character Recognition, OCR) durchzuführen, intelligent zugeschnittene Miniaturansichten zu erstellen sowie visuelle Merkmale (einschließlich Gesichter) in einem Bild zu erkennen, zu kategorisieren, zu markieren und zu beschreiben. Um schnell mit der Maschinelles Sehen-API zu experimentieren, probieren Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) aus.

> [!div class="nextstepaction"]
> [Tutorial zur Maschinelles Sehen-API in Python](../Tutorials/PythonTutorial.md)
