---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, Python'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die Maschinelles Sehen-API mit Python verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: cf60e298782d7bdcf15b53474b2d002a3fd62bba
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341891"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-python-in-computer-vision"></a>Schnellstart: Generieren einer Miniaturansicht mit der REST-API und Python in der Maschinelles Sehen-API

In dieser Schnellstartanleitung generieren Sie eine Miniaturansicht eines Bilds unter Verwendung der REST-API für maschinelles Sehen. Mit der Methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) können Sie die gewünschte Höhe und Breite angeben. Maschinelles Sehen identifiziert dann mittels intelligentem Zuschneiden den gewünschten Bereich und generiert Zuschnittkoordinaten auf der Grundlage dieser Region.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/try/cognitive-services/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen einen Abonnementschlüssel für maschinelles Sehen. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Schlüssel für eine kostenlose Testversion abrufen. Oder gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten.
- Ein Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="create-and-run-the-sample"></a>Erstellen und Ausführen des Beispiels

Kopieren Sie zum Erstellen und Ausführen des Beispiels den folgenden Code in den Code-Editor. 

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the "westus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers,
                         params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

Gehen Sie anschließend wie folgt vor:
1. Ersetzen Sie den `subscription_key`-Wert durch Ihren Abonnementschlüssel.
1. Ersetzen Sie den Wert von `vision_base_url` durch die Endpunkt-URL für die Ressource „Maschinelles Sehen-API“ in der Azure-Region, in der Sie Ihre Abonnementschlüssel bezogen haben, falls erforderlich.
1. Ersetzen Sie den Wert von `image_url` optional durch die URL eines anderen Bilds, für das Sie eine Miniaturansicht generieren möchten.
1. Speichern Sie den Code als Datei mit der Erweiterung `.py`. Beispiel: `get-thumbnail.py`.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Verwenden Sie an der Eingabeaufforderung den Befehl `python`, um das Beispiel auszuführen. Beispiel: `python get-thumbnail.py`.

## <a name="examine-the-response"></a>Untersuchen der Antwort

Eine erfolgreiche Antwort wird in Form von Binärdaten zurückgegeben, die die Bilddaten für die Miniaturansicht darstellen. Im Beispiel sollte dieses Bild angezeigt werden. Ist die Anforderung nicht erfolgreich, wird die Antwort im Eingabeaufforderungsfenster angezeigt und sollte einen Fehlercode enthalten.

## <a name="run-in-jupyter-optional"></a>Ausführen in Jupyter (optional)

Sie können diese Schnellstartanleitung optional Schritt für Schritt mit einem Jupyter-Notebook in [MyBinder](https://mybinder.org) ausführen. Klicken Sie zum Starten von Binder auf die folgende Schaltfläche:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich als Nächstes ausführlicher über das Feature zum Generieren von Miniaturansichten.

> [!div class="nextstepaction"]
> [Generieren intelligent zugeschnittener Miniaturbilder mit maschinellem Sehen](../concept-generating-thumbnails.md)
