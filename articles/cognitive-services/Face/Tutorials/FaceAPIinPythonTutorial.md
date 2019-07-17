---
title: 'Schnellstart: Erkennen und Umranden von Gesichtern in einem Bild mit dem Python SDK'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart erstellen Sie ein Python-Skript, das mithilfe der Gesichtserkennungs-API Gesichter in einem Remotebild erkennt und umrandet.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/03/2018
ms.author: sbowles
ms.openlocfilehash: 741dd18a3b8da5e44d77c24d46adb8d550322281
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603294"
---
# <a name="quickstart-create-a-python-script-to-detect-and-frame-faces-in-an-image"></a>Schnellstart: Erstellen eines Python-Skripts zum Erkennen und Umranden von Gesichtern in einem Bild

In diesem Schnellstart erstellen Sie ein Python-Skript, das mithilfe der Azure-Gesichtserkennungs-API über das Python SDK menschliche Gesichter in einem Remotebild erkennt und umrandet. Die Anwendung zeigt ein ausgewähltes Bild und zeichnet einen Rahmen um jedes erkannte Gesicht.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- [Python 2.7+ oder 3.5+](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)-Tool

## <a name="get-the-face-sdk"></a>Abrufen des Face SDK

Installieren Sie das Face Python SDK, indem Sie die Eingabeaufforderung öffnen und den folgenden Befehl ausführen:

```shell
pip install cognitive_face
```

## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Erstellen Sie ein neues Python-Skript mit dem Namen _FaceQuickstart.py_, und fügen Sie den folgenden Code hinzu. Dieser Code behandelt die Kernfunktionen der Gesichtserkennung. Sie müssen `<Subscription Key>` durch den Wert Ihres Schlüssels ersetzen. Darüber hinaus müssen Sie unter Umständen den Wert von `BASE_URL` in die korrekte Regions-ID für Ihren Schlüssel ändern. (Eine Liste aller Regionsendpunkte finden Sie in den [Dokumenten zur Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).) Abonnementschlüssel für kostenlose Testversionen werden in der Region **westus** generiert. Legen Sie optional `img_url` auf die URL des Bilds fest, das Sie verwenden möchten.

Das Skript erkennt Gesichter durch Aufrufen der **cognitive_face.face.detect**-Methode, die die [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)-REST-API umschließt und eine Liste mit Gesichtern zurückgibt.

```python
import cognitive_face as CF

# Replace with a valid subscription key (keeping the quotes in place).
KEY = '<Subscription Key>'
CF.Key.set(KEY)

# Replace with your regional Base URL
BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

### <a name="try-the-app"></a>Testen der App

Führen Sie die App mit dem Befehl `python FaceQuickstart.py` aus. Im Konsolenfenster sollte ungefähr folgende Textantwort angezeigt werden:

```console
[{'faceId': '26d8face-9714-4f3e-bfa1-f19a7a7aa240', 'faceRectangle': {'top': 124, 'left': 459, 'width': 227, 'height': 227}}]
```

Die Ausgabe stellt eine Liste der erkannten Gesichtern dar. Jeder Eintrag in der Liste ist eine **dict**-Instanz, wobei `faceId` eine eindeutige ID für das erkannte Gesicht ist und `faceRectangle` die Position des erkannten Gesichts beschreibt. 

> [!NOTE]
> Gesichts-IDs laufen nach 24 Stunden ab. Gesichtserkennungsdaten müssen explizit gespeichert werden, wenn Sie sie langfristig aufbewahren möchten.

## <a name="draw-face-rectangles"></a>Zeichnen von Rechtecken um Gesichter

Mithilfe der Koordinaten, die Sie durch den vorherigen Befehl erhalten haben, können Sie Rechtecke in das Bild zeichnen und so die einzelnen Gesichter visuell darstellen. Sie müssen Pillow installieren (`pip install pillow`), um das Pillow-Bildmodul verwenden zu können. Fügen Sie am Anfang von *FaceQuickstart.py* den folgenden Code hinzu:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Fügen Sie anschließend am Ende des Skripts den folgenden Code hinzu. Dieser Code erstellt eine einfache Funktion zum Analysieren der Rechteckkoordinaten und verwendet Pillow zum Zeichnen von Rechtecken im ursprünglichen Bild. Anschließend wird das Bild in Ihrer standardmäßigen Bildanzeige angezeigt.

```python
# Convert width height to a point in a rectangle


def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))


# Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

# For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

# Display the image in the users default image browser.
img.show()
```

## <a name="run-the-app"></a>Ausführen der App

Unter Umständen werden Sie zuerst zum Auswählen einer Standardbildanzeige aufgefordert. Anschließend sollte ein Bild wie das folgende angezeigt werden: Die Rechteckdaten sollten außerdem im Konsolenfenster ausgegeben werden.

![Eine junge Frau mit einem roten Rechteck um das Gesicht](../images/face-rectangle-result.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde der grundlegende Prozess für die Verwendung des Python SDK der Gesichtserkennungs-API beschrieben und ein Skript zum Erkennen und Umranden von Gesichtern in einem Bild erstellt. Sehen Sie sich als Nächstes die Verwendung des Python SDK in einem komplexeren Beispiel an. Rufen Sie das Python-Beispiel für die Gesichtserkennung auf GitHub auf, klonen Sie es in Ihrem Projektordner, und befolgen Sie die Anweisungen in der Datei _README.md_.

> [!div class="nextstepaction"]
> [Python-Beispiel für die Gesichtserkennung](https://github.com/Microsoft/Cognitive-Face-Python)
