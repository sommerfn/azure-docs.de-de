---
title: 'Tutorial: Ausführen von Bildvorgängen – Python'
titlesuffix: Azure Cognitive Services
description: Es wird beschrieben, wie Sie die Maschinelles Sehen-API mit Python verwenden, indem Sie Jupyter-Notebooks nutzen. Visualisieren Sie Ihre Ergebnisse mithilfe gängiger Bibliotheken.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b5333557355aa816245b5086836eac980d90540a
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341483"
---
# <a name="tutorial-computer-vision-api-python"></a>Tutorial: Maschinelles Sehen-API (Python)

In diesem Tutorial wird veranschaulicht, wie Sie die Maschinelles Sehen-API in Python verwenden und Ihre Ergebnisse mithilfe beliebter Bibliotheken visualisieren. Sie nutzen Jupyter, um das Tutorial durchzuarbeiten. Informationen zum Einstieg in interaktive Jupyter-Notebooks finden Sie in der [Jupyter-Dokumentation](https://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Voraussetzungen

- [Python 2.7+ oder 3.5+](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)-Tool
- [Jupyter Notebook](https://jupyter.org/install) muss installiert sein.

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Öffnen des Tutorial-Notebooks in Jupyter 

1. Navigieren Sie zum GitHub-Repository [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Klicken Sie auf die grüne Schaltfläche, um das Repository zu klonen oder herunterzuladen. 
3. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zum Ordner **Cognitive-Vision-Python\Jupyter Notebook**.
1. Vergewissern Sie sich, dass alle erforderlichen Bibliotheken vorhanden sind. Führen Sie dazu an der Eingabeaufforderung den Befehl `pip install requests opencv-python numpy matplotlib` aus.
1. Starten Sie Jupyter, indem Sie an der Eingabeaufforderung den Befehl `jupyter notebook` ausführen.
1. Klicken Sie im Jupyter-Fenster auf _Computer Vision API Example.ipynb_, um das Tutorial-Notebook zu öffnen.

## <a name="run-the-tutorial"></a>Ausführen des Tutorials

Um dieses Notizbuch verwenden zu können, benötigen Sie einen Abonnementschlüssel für die Maschinelles Sehen-API. Besuchen Sie zur Registrierung die [Abonnementseite](https://azure.microsoft.com/try/cognitive-services/). Melden Sie sich auf der Seite **Anmelden** mit Ihrem Microsoft-Konto an. Anschließend können Sie kostenlose Schlüssel abonnieren und abrufen. Fügen Sie nach Abschluss des Registrierungsvorgangs Ihren Schlüssel in den Abschnitt `Variables` des Notebooks ein (unten dargestellt). Entweder der Primär- oder der Sekundärschlüssel funktioniert. Achten Sie darauf, den Schlüssel in Anführungszeichen zu setzen, um ihn zu einer Zeichenfolge zu machen.

Sie müssen darüber hinaus sicherstellen, dass das Feld `_region` der Region Ihres Abonnements entspricht.

```python
# Variables
_region = 'westcentralus'  # Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(
    _region)
_key = None  # Here you have to paste your primary key
_maxNumRetries = 10
```

In diesem Tutorial können Sie zu analysierende Bilder hinzufügen – sowohl in Form einer URL als auch aus dem lokalen Speicher. Das Skript zeigt die Bilder- und Analyseinformationen im Notebook an.
