---
title: 'Tutorial: Verwenden von Python und TensorFlow in Azure Functions für Machine Learning-Rückschlüsse | Microsoft-Dokumentation'
description: In diesem Tutorial wird veranschaulicht, wie Sie TensorFlow-Machine Learning-Modelle in Azure Functions anwenden.
services: functions
author: anthonychu
manager: gwallace
ms.service: azure-functions
ms.devlang: python
ms.topic: tutorial
ms.date: 07/29/2019
ms.author: antchu
ms.custom: mvc
ms.openlocfilehash: e243fd2f5c4a90e45f424ce39a97913df2332b2b
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677878"
---
# <a name="tutorial-apply-machine-learning-models-in-azure-functions-with-python-and-tensorflow"></a>Tutorial: Anwenden von Machine Learning-Modellen in Azure Functions mit Python und TensorFlow

In diesem Artikel wird veranschaulicht, wie Sie mit Azure Functions Python und TensorFlow zusammen mit einem Machine Learning-Modell nutzen können, um ein Bild basierend auf seinem Inhalt zu klassifizieren.

In diesem Tutorial lernen Sie Folgendes: 

> [!div class="checklist"]
> * Initialisieren einer lokalen Umgebung zum Entwickeln von Azure Functions in Python
> * Importieren eines benutzerdefinierten TensorFlow-Machine Learning-Modells in eine Funktions-App
> * Erstellen einer serverlosen HTTP-API zum Vorhersagen, ob ein Foto einen Hund oder eine Katze zeigt
> * Verwenden der API aus einer Webanwendung

![Screenshot: Abgeschlossenes Projekt](media/functions-machine-learning-tensorflow/functions-machine-learning-tensorflow-screenshot.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen 

Zum Erstellen von Azure Functions in Python müssen Sie einige Tools installieren.

- [Python 3.6](https://www.python.org/downloads/release/python-360/)
- [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools)
- Ein Code-Editor wie [Visual Studio Code](https://code.visualstudio.com/)

## <a name="clone-the-tutorial-repository"></a>Klonen des Tutorial-Repositorys

Öffnen Sie zunächst ein Terminal, und klonen Sie das folgende Repository per Git:

```console
git clone https://github.com/Azure-Samples/functions-python-tensorflow-tutorial.git
cd functions-python-tensorflow-tutorial
```

Das Repository enthält einige Ordner.

- *start*:  Dies ist der Arbeitsordner für das Tutorial.
- *end*: Dies ist das Endergebnis und die vollständige Implementierung zu Referenzzwecken.
- *resources*: Enthält das Machine Learning-Modell und Hilfsbibliotheken.
- *frontend*: Eine Website, über die die Funktions-App aufgerufen wird.

## <a name="create-and-activate-a-python-virtual-environment"></a>Erstellen und Aktivieren einer virtuellen Python-Umgebung

Für Azure Functions wird Python 3.6.x benötigt. Sie erstellen eine virtuelle Umgebung, um sicherzustellen, dass Sie die erforderliche Python-Version verwenden.

Ändern Sie das aktuelle Arbeitsverzeichnis in den Ordner *start*. Erstellen und aktivieren Sie eine virtuelle Umgebung mit dem Namen *.venv*. Abhängig von Ihrer Python-Installation können die Befehle zum Erstellen einer virtuellen Python 3.6-Umgebung von der folgenden Anleitung abweichen.

#### <a name="linux-and-macos"></a>Linux und macOS:

```bash
cd start
python3.6 -m venv .venv
source .venv/bin/activate
```

#### <a name="windows"></a>Windows:

```powershell
cd start
py -3.6 -m venv .venv
.venv\scripts\activate
```

Die Eingabeaufforderung im Terminal enthält jetzt das Präfix `(.venv)`, um anzugeben, dass die Aktivierung der virtuellen Umgebung erfolgreich war. Vergewissern Sie sich, dass `python` in der virtuellen Umgebung wirklich die Version Python 3.6.x aufweist.

```console
python --version
```

> [!NOTE]
> Im restlichen Teil des Tutorials führen Sie Befehle in der virtuellen Umgebung aus. Wenn Sie die virtuelle Umgebung in einem Terminal erneut aktivieren möchten, müssen Sie den entsprechenden Aktivierungsbefehl für Ihr Betriebssystem ausführen.

## <a name="create-an-azure-functions-project"></a>Erstellen eines Azure Functions-Projekts

Verwenden Sie im Ordner *start* die Azure Functions Core Tools, um eine Python-Funktions-App zu initialisieren.

```console
func init --worker-runtime python
```

Eine Funktions-App kann eine oder mehrere Azure Functions enthalten. Öffnen Sie den Ordner *start* in einem Editor, und untersuchen Sie den Inhalt.

- [*local.settings.json*](functions-run-local.md#local-settings-file): Enthält Anwendungseinstellungen, die für die lokale Entwicklung verwendet werden.
- [*host.json*](functions-host-json.md): Enthält Einstellungen für den Azure Functions-Host und Erweiterungen.
- [*requirements.txt*](functions-reference-python.md#python-version-and-package-management): Enthält Python-Pakete, die für diese Anwendung erforderlich sind.

## <a name="create-an-http-function"></a>Erstellen einer HTTP-Funktion

Die Anwendung benötigt einen einzelnen HTTP-API-Endpunkt, für den eine Bild-URL als Eingabe verwendet wird und eine Vorhersage dazu zurückgegeben wird, ob das Bild einen Hund oder eine Katze zeigt.

Verwenden Sie im Terminal die Azure Functions Core Tools, um ein Gerüst für eine neue HTTP-Funktion mit dem Namen *classify* zu erstellen.

```console
func new --language python --template HttpTrigger --name classify
```

Ein neuer Ordner mit dem Namen *classify* und zwei Dateien wird erstellt.

- *\_\_init\_\_.py*: Eine Datei für die main-Funktion.
- *function.json*:  Eine Datei, mit der der Trigger der Funktion und die zugehörigen Ein- und Ausgabebindungen beschrieben werden.

### <a name="run-the-function"></a>Ausführen der Funktion

Starten Sie im Terminal mit aktivierter virtueller Python-Umgebung die Funktions-App.

```console
func start
```

Öffnen Sie einen Browser, und navigieren Sie zur folgenden URL: Die Funktion sollte ausgeführt werden, und die Rückgabe sollte *Hello Azure!* lauten.

```
http://localhost:7071/api/classify?name=Azure
```

Verwenden Sie `Ctrl-C`, um die Funktions-App zu beenden.

## <a name="import-the-tensorflow-model"></a>Importieren des TensorFlow-Modells

Sie verwenden ein vordefiniertes TensorFlow-Modell, das mit dem Dienst Azure Custom Vision trainiert und exportiert wurde.

> [!NOTE]
> Falls Sie über den Free-Tarif von Custom Vision ein eigenes Modell erstellen möchten, können Sie die [Anleitung im Repository des Beispielprojekts](https://github.com/Azure-Samples/functions-python-tensorflow-tutorial/blob/master/train-custom-vision-model.md) befolgen.

Das Modell besteht aus zwei Dateien im Ordner *<REPOSITORYSTAMM>/resources/model*: *model.pb* und *labels.txt*. Kopieren Sie diese in den Ordner der Funktion *classify*.

#### <a name="linux-and-macos"></a>Linux und macOS:

```bash
cp ../resources/model/* classify
```

#### <a name="windows"></a>Windows:

```powershell
copy ..\resources\model\* classify
```

Stellen Sie sicher, dass Sie in den obigen Befehl \* einfügen. Vergewissern Sie sich, dass unter *classify* jetzt die Dateien mit den Namen *model.pb* und *labels.txt* enthalten sind.

## <a name="add-the-helper-functions-and-dependencies"></a>Hinzufügen der Hilfsfunktionen und Abhängigkeiten

Einige Hilfsfunktionen zur Vorbereitung des Eingabebilds und zum Treffen einer Vorhersage mit TensorFlow sind in der Datei *predict.py* im Ordner *resources* enthalten. Kopieren Sie diese Datei in den Ordner der Funktion *classify*.

#### <a name="linux-and-macos"></a>Linux und macOS:

```bash
cp ../resources/predict.py classify
```

#### <a name="windows"></a>Windows:

```powershell
copy ..\resources\predict.py classify
```

Vergewissern Sie sich, dass *classify* jetzt eine Datei mit dem Namen *predict.py* enthält.

### <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Die Hilfsbibliothek muss über einige Abhängigkeiten verfügen, die installiert werden müssen. Öffnen Sie *start/requirements.txt* in Ihrem Editor, und fügen Sie der Datei die folgenden Abhängigkeiten hinzu.

```txt
tensorflow==1.15
Pillow
requests
```

Speichern Sie die Datei .

Führen Sie im Terminal mit aktivierter virtueller Umgebung den folgenden Befehl im Ordner *start* aus, um die Abhängigkeiten zu installieren. Es kann einige Minuten dauern, bis bestimmte Installationsschritte abgeschlossen sind.

```console
pip install --no-cache-dir -r requirements.txt
```

### <a name="caching-the-model-in-global-variables"></a>Zwischenspeichern des Modells in globalen Variablen

Öffnen Sie im Editor die Datei *predict.py*, und suchen Sie im oberen Bereich der Datei nach der Funktion `_initialize`. Beachten Sie, dass das TensorFlow-Modell bei der ersten Ausführung vom Datenträger geladen und in globalen Variablen gespeichert wird. Das Laden vom Datenträger wird bei den nachfolgenden Ausführungen der Funktion `_initialize` übersprungen. Durch das Zwischenspeichern des Modells im Arbeitsspeicher mit diesem Verfahren werden spätere Vorhersagen beschleunigt.

Weitere Informationen zu globalen Variablen finden Sie im [Python-Entwicklerhandbuch für Azure Functions](functions-reference-python.md#global-variables).

## <a name="update-function-to-run-predictions"></a>Aktualisieren der Funktion zum Durchführen von Vorhersagen

Öffnen Sie *classify/\_\_init\_\_.py* in Ihrem Editor. Importieren Sie die Bibliothek *predict*, die Sie demselben Ordner zuvor hinzugefügt haben. Fügen Sie unterhalb der anderen Importe, die bereits in der Datei enthalten sind, die folgenden `import`-Anweisungen hinzu.

```python
import json
from .predict import predict_image_from_url
```

Ersetzen Sie den Funktionsvorlagencode durch Folgendes.

```python
def main(req: func.HttpRequest) -> func.HttpResponse:
    image_url = req.params.get('img')
    results = predict_image_from_url(image_url)

    headers = {
        "Content-type": "application/json",
        "Access-Control-Allow-Origin": "*"
    }
    return func.HttpResponse(json.dumps(results), headers = headers)
```

Speichern Sie Ihre Änderungen.

Mit dieser Funktion wird eine Bild-URL in einem Abfragezeichenfolgen-Parameter mit dem Namen `img` empfangen. `predict_image_from_url` wird aus der Hilfsbibliothek aufgerufen, um das Bild herunterzuladen und mit dem TensorFlow-Modell eine Vorhersage zurückzugeben. Die Funktion gibt dann eine HTTP-Antwort mit den Ergebnissen zurück.

Da der HTTP-Endpunkt von einer Webseite aufgerufen wird, die in einer anderen Domäne gehostet wird, enthält die HTTP-Antwort einen `Access-Control-Allow-Origin`-Header, um die CORS-Anforderungen (Cross-Origin Resource Sharing, Ressourcenfreigabe zwischen verschiedenen Ursprüngen) des Browsers zu erfüllen.

> [!NOTE]
> Ändern Sie in einer Produktionsanwendung `*` in den spezifischen Ursprung der Webseite, um die Sicherheit zu erhöhen.

### <a name="run-the-function-app"></a>Ausführen der Funktions-App

Stellen Sie sicher, dass die virtuelle Python-Umgebung noch aktiviert ist, und starten Sie die Funktions-App mit dem folgenden Befehl.

```console
func start
```

Öffnen Sie diese URL in einem Browser. Ihre Funktion wird mit der URL eines Katzenfotos aufgerufen. Vergewissern Sie sich, dass ein gültiges Vorhersageergebnis zurückgegeben wird.

```
http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png
```

Behalten Sie die Ausführung der Funktions-App bei.

### <a name="run-the-web-app"></a>Ausführen der Web-App

Der Ordner *frontend* enthält eine einfache Web-App, von der die HTTP-API in der Funktions-App genutzt wird.

Öffnen Sie ein *separates* Terminal, und wechseln Sie in den Ordner *frontend*. Starten Sie einen HTTP-Server mit Python 3.6.

#### <a name="linux-and-macos"></a>Linux und macOS:

```bash
cd <FRONT_END_FOLDER>
python3.6 -m http.server
```

#### <a name="windows"></a>Windows:

```powershell
cd <FRONT_END_FOLDER>
py -3.6  -m http.server
```

Navigieren Sie in einem Browser zur URL des HTTP-Servers, die im Terminal angezeigt wird. Es sollte eine Web-App angezeigt werden. Geben Sie die folgenden Foto-URLs in das Textfeld ein. Sie können auch eine URL eines öffentlich zugänglichen Katzen- oder Hundefotos verwenden.

- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat2.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog1.png`
- `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog2.png`

Wenn Sie auf die Schaltfläche zum Absenden klicken, wird die Funktions-App aufgerufen, und auf der Seite wird ein Ergebnis angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Dieses Tutorial wird vollständig lokal auf Ihrem Computer ausgeführt, sodass keine Azure-Ressourcen oder Dienste bereinigt werden müssen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie eine HTTP-API mit Azure Functions erstellen und anpassen, um Vorhersagen mit einem TensorFlow-Modell zu erstellen. Es wurde auch beschrieben, wie Sie die API aus einer Webanwendung aufrufen.

Sie können die Verfahren dieses Tutorials verwenden, um APIs mit beliebiger Komplexität zu erstellen, während Sie das von Azure Functions bereitgestellte serverlose Computemodell nutzen.

Verwenden Sie zum Bereitstellen der Funktions-App in Azure die [Azure Functions Core Tools](./functions-run-local.md#publish) oder [Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-azure-functions).

> [!div class="nextstepaction"]
> [Python-Entwicklerhandbuch für Azure Functions](./functions-reference-python.md)
