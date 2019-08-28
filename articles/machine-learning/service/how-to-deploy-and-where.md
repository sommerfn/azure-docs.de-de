---
title: Wie und wo Modelle bereitgestellt werden
titleSuffix: Azure Machine Learning service
description: 'Erfahren Sie, wie und wo Sie Ihre Azure Machine Learning Service-Modelle bereitstellen, wie z. B.: Azure Container Instances, Azure Kubernetes Service, Azure IoT Edge und Field Programmable Gate Arrays.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 08/06/2019
ms.custom: seoapril2019
ms.openlocfilehash: acb3717f0e71ca1e67f1ddec79a259935f6cc539
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897674"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst

Erfahren Sie, wie Sie Ihr Machine Learning-Modell als Webdienst in der Azure-Cloud oder auf IoT Edge-Geräten bereitstellen.

Der Workflow ähnelt sich unabhängig vom [Bereitstellungsort](#target) Ihres Modells:

1. Registrieren des Modells.
1. Vorbereiten der Bereitstellung (Ressourcen, Nutzung, Computeziel angeben).
1. Bereitstellen des Modells auf dem Computeziel.
1. Testen des bereitgestellten Modells (auch als „Webdienst“ bezeichnet).

Weitere Informationen zu den am Bereitstellungsworkflow beteiligten Konzepten finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen mit dem Azure Machine Learning-Dienst](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Dienstbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning Service-Arbeitsbereichs](how-to-manage-workspace.md).

- Ein Modell. Wenn Sie über kein trainiertes Modell verfügen, können Sie die Modell- und Abhängigkeitsdateien verwenden, die in [diesem Tutorial](https://aka.ms/azml-deploy-cloud) bereitgestellt werden.

- Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](how-to-vscode-tools.md).

## <a name="connect-to-your-workspace"></a>Herstellen einer Verbindung mit Ihrem Arbeitsbereich

Der folgende Code veranschaulicht, wie Sie mithilfe von Informationen, die in der lokalen Entwicklungsumgebung zwischengespeichert sind, eine Verbindung mit einem Azure Machine Learning-Dienstarbeitsbereich herstellen:

**Verwenden des SDK**

```python
from azureml.core import Workspace
ws = Workspace.from_config(path=".file-path/ws_config.json")
```

Weitere Informationen zur Verwendung des SDKs, um eine Verbindung mit einem Arbeitsbereich herzustellen, finden Sie im [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace).

**Verwenden der CLI**

Verwenden Sie bei Verwendung der CLI den Parameter `-w` oder `--workspace-name`, um den Arbeitsbereich für den Befehl anzugeben.

**Verwenden von VS Code**

Wenn Sie VS Code verwenden, wird der Arbeitsbereich mithilfe einer grafischen Benutzeroberfläche ausgewählt. Weitere Informationen finden Sie unter [Bereitstellen und Verwalten von Modellen](how-to-vscode-tools.md#deploy-and-manage-models) in der Dokumentation zur VS Code-Erweiterung.

## <a id="registermodel"></a> Registrieren Ihres Modells

Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell im Arbeitsbereich registrieren. Nach der Registrierung können Sie das registrierte Modell dann herunterladen oder bereitstellen und alle Dateien empfangen, die registriert wurden.

> [!TIP]
> Wenn Sie ein Modell registrieren, geben Sie entweder einen Pfad zu einem Cloudspeicherort (aus einem Trainingslauf) oder ein lokales Verzeichnis an. Dieser Pfad dient lediglich zum Auffinden der Dateien für den Upload im Rahmen des Registrierungsprozesses. Er muss nicht mit dem im Eingangsskript verwendeten Pfad identisch sein. Weitere Informationen finden Sie unter [Was ist get_model_path?](#what-is-get_model_path).

Machine Learning-Modelle werden in Ihrem Azure Machine Learning-Arbeitsbereich registriert. Das Modell kann aus Azure Machine Learning oder aus einer anderen Quelle stammen. Die folgenden Beispiele veranschaulichen das Registrieren eines Modells:

### <a name="register-a-model-from-an-experiment-run"></a>Registrieren eines Modells aus einer Experimentausführung

Die Code Ausschnitte in diesem Abschnitt veranschaulichen das Registrieren eines Modells aus einem Trainingslauf:

> [!IMPORTANT]
> Bei diesen Codeausschnitten wird vorausgesetzt, dass Sie zuvor einen Trainingslauf ausgeführt haben und Zugriff auf das `run`-Objekt (SDK-Beispiel) oder den Ausführungs-ID-Wert haben (CLI-Beispiel). Weitere Informationen zum Trainieren von Modellen finden Sie unter [Einrichten von und Verwenden von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

+ **Verwenden des SDK**

  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  Der `model_path` bezieht sich auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad zu einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `model_path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden der CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment --run-id myrunid
  ```

  [!INCLUDE [install extension](../../../includes/machine-learning-service-install-extension.md)]

  Der `--asset-path` bezieht sich auf den Cloudspeicherort des Modells. In diesem Beispiel wird der Pfad zu einer einzelnen Datei verwendet. Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `--asset-path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden von VS Code**

  Registrieren Sie Modelle, indem Sie beliebige Modelldateien oder -ordner mit der [VS Code](how-to-vscode-tools.md#deploy-and-manage-models)-Erweiterung verwenden.

### <a name="register-a-model-from-a-local-file"></a>Registrieren eines Modells aus einer lokalen Datei

Sie können ein Modell registrieren, indem Sie einen **lokalen Pfad** für das Modell bereitstellen. Sie können einen Ordner oder eine einzelne Datei angeben. Mit dieser Methode können Sie sowohl Modelle registrieren, die mit dem Azure Machine Learning-Dienst trainiert und dann heruntergeladen wurden, oder Modelle, die außerhalb von Azure Machine Learning trainiert wurden.

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

+ **ONNX-Beispiel mit dem Python SDK:**

    ```python
    import os
    import urllib.request
    from azureml.core import Model
    # Download model
    onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
    urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
    os.system('tar xvzf mnist.tar.gz')
    # Register model
    model = Model.register(workspace = ws,
                            model_path ="mnist/model.onnx",
                            model_name = "onnx_mnist",
                            tags = {"onnx": "demo"},
                            description = "MNIST image classification CNN from ONNX Model Zoo",)
    ```

  Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `model_path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden der CLI**

  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `-p` auf das Verzeichnis ein, das die Dateien enthält.

**Geschätzter Zeitaufwand**: Ungefähr 10 Sekunden.

Weitere Informationen finden Sie in der Referenzdokumentation zur [Modellklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Weitere Informationen zur Arbeit mit Modellen, die außerhalb des Azure Machine Learning Service trainiert wurden, finden Sie unter [Verwenden eines vorhandenen Modells mit Azure Machine Learning Service](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Auswählen eines Computeziels

Die folgenden Computeziele bzw. Computeressourcen können verwendet werden, um Ihre Webdienstbereitstellung zu hosten. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Vorbereiten der Bereitstellung

Für die Bereitstellung des Modells müssen verschiedene Voraussetzungen erfüllt sein:

* Ein __Eingabeskript__. Dieses Skript akzeptiert Anforderungen, bewertet die Anforderung mithilfe des Modells und gibt die Ergebnisse zurück.

    > [!IMPORTANT]
    > Das Eingabeskript ist spezifisch für Ihr Modell. Es muss das Format der Daten der eingehenden Anforderung, das Format der im Modell erwarteten Daten und das Format der an Clients zurückgegebenen Daten erkennen.
    >
    > Wenn die Anforderungsdaten in einem Format vorliegen, das in Ihrem Modell nicht verwendet werden kann, können sie im Skript in ein akzeptables Format umgewandelt werden. Außerdem kann die Antwort umgewandelt werden, bevor sie an den Client zurückgegeben wird.

    > [!IMPORTANT]
    > Das Azure Machine Learning SDK bietet keinen Zugriff auf Ihren Datenspeicher oder Ihre Datasets durch Webdienste oder IoT Edge-Bereitstellungen. Wenn das bereitgestellte Modell auf Daten zugreifen muss, die außerhalb der Bereitstellung gespeichert sind – wie z.B. in einem Azure Storage-Konto –, müssen Sie mit dem entsprechenden SDK eine benutzerdefinierte Codelösung entwickeln. Ein Beispiel hierfür ist das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python).
    >
    > Eine andere Alternative, die in Ihrem Szenario funktionieren könnte, ist die [Batchvorhersage](how-to-run-batch-predictions.md), die beim Erstellen von Bewertungen Zugriff auf Datenspeicher bietet.

* **Abhängigkeiten**, z. B. Hilfsskripts oder Python/Conda-Pakete, die zum Ausführen des Eingabeskripts oder Modells erforderlich sind

* Die __Bereitstellungskonfiguration__ für das Computeziel, von dem das bereitgestellte Modell gehostet wird. Diese Konfiguration beschreibt Aspekte wie Arbeitsspeicher- und CPU-Anforderungen für die Ausführung des Modells.

Diese Entitäten werden in einer __Rückschlusskonfiguration__ und einer __Bereitstellungskonfiguration__ gekapselt. Die Rückschlusskonfiguration verweist auf das Eingabeskript und andere Abhängigkeiten. Diese Konfigurationen werden bei Verwendung des SDK programmgesteuert definiert. Erfolgt die Bereitstellung hingegen über die CLI, werden sie als JSON-Dateien definiert.

### <a id="script"></a> 1. Definieren des Eingangsskripts und der Abhängigkeiten

Das Eingangsskript empfängt an einen bereitgestellten Webdienst übermittelte Daten und übergibt sie an das Modell. Anschließend nimmt es die vom Modell zurückgegebene Antwort entgegen und gibt diese an den Client zurück. **Das Skript ist modellspezifisch.** Es muss die vom Modell erwarteten und zurückgegebenen Daten verstehen.

Das Skript enthält zwei Funktionen zum Laden und Ausführen des Modells:

* `init()`: Diese Funktion lädt üblicherweise das Modell in ein globales Objekt. Diese Funktion wird nur ein Mal ausgeführt, wenn der Docker-Container für Ihren Webdienst gestartet wird.

* `run(input_data)`: Diese Funktion verwendet das Modell zur Vorhersage eines Werts, der auf den Eingabedaten basiert. Ein- und Ausgaben an die/von der Ausführung verwenden in der Regel JSON für die Serialisierung und Deserialisierung. Sie können auch mit binären Rohdaten arbeiten. Sie können die Daten vor dem Senden an das Modell oder vor der Rückgabe an den Client transformieren.

#### <a name="what-is-get_model_path"></a>Was ist „get_model_path“?

Wenn Sie ein Modell registrieren, geben Sie einen Modellnamen ein. Dieser dient zur Verwaltung des Modells in der Registrierung. Der Name wird mit [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) verwendet, um den Pfad der Modelldatei(en) im lokalen Dateisystem abzurufen. Wenn Sie einen Ordner oder eine Sammlung von Dateien registrieren, gibt diese API den Pfad des Verzeichnisses zurück, das diese Dateien enthält.

Wenn Sie ein Modell registrieren, geben Sie ihm einen Namen, der dem Ort entspricht, an dem sich das Modell befindet (entweder lokal oder während der Dienstbereitstellung).

Im folgenden Beispiel wird ein Pfad zu einer einzelnen Datei namens `sklearn_mnist_model.pkl` zurückgegeben, die mit dem Namen `sklearn_mnist` registriert wurde:

```python
model_path = Model.get_model_path('sklearn_mnist')
```

<a id="schema"></a>

#### <a name="optional-automatic-schema-generation"></a>(Optional:) Automatische Schemagenerierung

Um automatisch ein Schema für Ihren Webdienst zu generieren, stellen Sie ein Beispiel der Eingabe und/oder Ausgabe im Konstruktor für eines der definierten Typobjekte bereit, und der Typ und das Beispiel werden zur automatischen Erstellung des Schemas verwendet. Azure Machine Learning Service erstellt dann während der Bereitstellung eine [OpenAPI](https://swagger.io/docs/specification/about/)-Spezifikation (Swagger) für den Webdienst.

Folgende Typen werden derzeit unterstützt:

* `pandas`
* `numpy`
* `pyspark`
* Python-Standardobjekt

Um Schemagenerierung zu verwenden, schließen Sie das `inference-schema`-Paket in Ihre Conda-Umgebungsdatei ein.

##### <a name="example-dependencies-file"></a>Beispieldatei für Abhängigkeiten

Das folgende YAML-Beispiel zeigt eine Conda-Abhängigkeitsdatei für den Rückschluss:

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Wenn Sie automatische Schemagenerierung verwenden möchten, **muss** Ihr Eingangsskript die `inference-schema`-Pakete importieren.

Definieren Sie in den Variablen `input_sample` und `output_sample` das Format für das Ein- und Ausgabebeispiel. (Die Variablen stellen das Anforderungs- und das Antwortformat für den Webdienst dar.) Verwenden Sie diese Beispiele in den Decorator-Elementen der Ein- und Ausgabefunktion für die Funktion `run()`. Das Scikit-learn-Beispiel unten verwendet Schemagenerierung.

##### <a name="example-entry-script"></a>Beispiel für ein Eingangsskript

Das folgende Beispiel zeigt, wie JSON-Daten akzeptiert und zurückgegeben werden:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType


def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = np.array([[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]])
output_sample = np.array([3726.995])


@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Im folgende Beispiel wird veranschaulicht, wie Eingabedaten unter Verwendung von Datenrahmen als Wörterbuch vom Typ `<key: value>` definiert werden. Diese Methode wird für die Nutzung des von Power BI bereitgestellten Webdiensts unterstützt ([Erfahren Sie mehr über den Webdienst von Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType


def init():
    global model
    # replace model_name with your actual model name, if needed
    model_path = Model.get_model_path('model_name')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)


input_sample = pd.DataFrame(data=[{
    # This is a decimal type sample. Use the data type that reflects this column in your data
    "input_name_1": 5.1,
    # This is a string type sample. Use the data type that reflects this column in your data
    "input_name_2": "value2",
    # This is a integer type sample. Use the data type that reflects this column in your data
    "input_name_3": 3
}])

# This is a integer type sample. Use the data type that reflects the expected result
output_sample = np.array([0])


@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

Weitere Beispielskripts finden Sie in den folgenden Beispielen:

* PyTorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)

<a id="binary"></a>

#### <a name="binary-data"></a>Binärdaten

Wenn Ihr Modell Binärdaten (beispielsweise ein Bild) akzeptiert, müssen Sie die für Ihre Bereitstellung verwendete Datei `score.py` so ändern, dass sie HTTP-Rohanforderungen akzeptiert. Um Rohdaten zu akzeptieren, verwenden Sie die `AMLRequest`-Klasse in Ihrem Eingangsskript und fügen der `run()`-Funktion den `@rawhttp`-Decorator hinzu.

Hier ist ein Beispiel für eine Datei `score.py`, die Binärdaten akzeptiert:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse


def init():
    print("This is init()")


@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real world solution, you would load the data from reqBody
        # and send to the model. Then return the response.

        # For demonstration purposes, this example just returns the posted data as the response.
        return AMLResponse(reqBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Die `AMLRequest`-Klasse befindet sich im `azureml.contrib`-Namespace. Dinge in diesem Namespace ändern sich häufig, während wir daran arbeiten, den Dienst zu verbessern. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.
>
> Wenn Sie dies in Ihrer lokalen Entwicklungsumgebung testen müssen, können Sie die Komponenten mit dem folgenden Befehl installieren:
>
> ```shell
> pip install azureml-contrib-services
> ```

<a id="cors"></a>

#### <a name="cross-origin-resource-sharing-cors"></a>Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)

Ressourcenfreigabe zwischen verschiedenen Ursprüngen ist eine Möglichkeit, um die Anforderung eingeschränkter Ressourcen auf einer Webseite aus anderen Domänen zuzulassen. CORS funktioniert auf Grundlage von HTTP-Headern, die mit der Clientanforderung gesendet und mit der Dienstantwort zurückgegeben werden. Weitere Informationen zu CORS und gültigen Headern finden Sie in Wikipedia unter [Cross-Origin Resource Sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Um Ihre Modellimplementierung für die Unterstützung von CORS zu konfigurieren, verwenden Sie die `AMLResponse`-Klasse in Ihrem Eingangsskript. Diese Klasse ermöglicht es Ihnen, die Header für das Antwortobjekt festzulegen.

Im folgenden Beispiel wird der `Access-Control-Allow-Origin`-Header für die Antwort aus dem Eingangsskript festgelegt:

```python
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real world solution, you would load the data from reqBody
        # and send to the model. Then return the response.

        # For demonstration purposes, this example
        # adds a header and returns the request body
        resp = AMLResponse(reqBody, 200)
        resp.headers['Access-Control-Allow-Origin'] = "http://www.example.com"
        return resp
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Die `AMLResponse`-Klasse befindet sich im `azureml.contrib`-Namespace. Dinge in diesem Namespace ändern sich häufig, während wir daran arbeiten, den Dienst zu verbessern. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.
>
> Wenn Sie dies in Ihrer lokalen Entwicklungsumgebung testen müssen, können Sie die Komponenten mit dem folgenden Befehl installieren:
>
> ```shell
> pip install azureml-contrib-services
> ```

### <a name="2-define-your-inferenceconfig"></a>2. Definieren der Rückschlusskonfiguration

Die Rückschlusskonfiguration beschreibt, wie das Modell zum Treffen von Vorhersagen konfiguriert wird. Diese Konfiguration ist kein Teil des Eingabeskripts; sie verweist auf das Eingabeskript, mit dem nach allen von der Bereitstellung benötigten Ressourcen gesucht wird. Sie wird später bei der eigentlichen Bereitstellung des Modells verwendet.

Im folgenden Beispiel wird veranschaulicht, wie eine Rückschlusskonfiguration erstellt wird. Diese Konfiguration gibt die Runtime, das Eingabeskript und (optional) die Conda-Umgebungsdatei an:

```python
from azureml.core.model import InferenceConfig

inference_config = InferenceConfig(runtime="python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Weitere Informationen finden Sie in der [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klassenreferenz.

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit Rückschlusskonfiguration finden Sie unter [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>CLI-Beispiel für InferenceConfig

[!INCLUDE [inference config](../../../includes/machine-learning-service-inference-config.md)]

Der folgende Befehl veranschaulicht, wie Sie ein Modell mithilfe der Befehlszeilenschnittstelle bereitstellen:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

In diesem Beispiel enthält die Konfiguration die folgenden Elemente:

* Die Angabe, dass für dieses Modell Python erforderlich ist.
* Das [Eingangsskript](#script), das zum Verarbeiten von an den bereitgestellten Dienst gesendeten Webanforderungen verwendet wird.
* Die Conda-Datei zur Beschreibung der Python-Pakete, die zum Ziehen von Rückschlüssen erforderlich sind.

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit Rückschlusskonfiguration finden Sie unter [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. Definieren der Bereitstellungskonfiguration

Bevor Sie die Bereitstellung ausführen, müssen Sie die Bereitstellungskonfiguration definieren. __Die Bereitstellungskonfiguration ist spezifisch für das Computeziel, das den Webdienst hosten wird.__ Beispielsweise müssen Sie bei einer lokalen Bereitstellung den Port angeben, an dem der Dienst Anforderungen akzeptiert. Die Bereitstellungskonfiguration ist kein Teil des Eingabeskripts. Damit werden die Merkmale des Computeziels definiert, von dem das Modell und das Eingabeskript gehostet werden.

Möglicherweise müssen Sie die Computeressourcen auch erstellen. Dies ist beispielsweise der Fall, wenn Sie noch keinen Azure Kubernetes Service für Ihren Arbeitsbereich eingerichtet haben.

Die folgende Tabelle enthält ein Beispiel für das Erstellen einer Bereitstellungskonfiguration für jedes Computeziel:

| Computeziel | Beispiel für eine Bereitstellungskonfiguration |
| ----- | ----- |
| Lokal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Jede dieser Klassen für lokale, ACI- und AKS-Webdienste kann aus `azureml.core.webservice` importiert werden:

```python
from azureml.core.webservice import AciWebservice, AksWebservice, LocalWebservice
```

> [!TIP]
> Es empfiehlt sich gegebenenfalls, ein Profil für Ihr Modell zu erstellen, um die optimalen CPU- und Arbeitsspeicheranforderungen zu bestimmen, bevor Sie Ihr Modell als Dienst bereitstellen. Sie können ein Profil Ihres Modells entweder mit dem SDK oder der CLI erstellen. Weitere Informationen finden Sie in der Referenz zu [profile()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-) und [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).
>
> Die Ergebnisse der Profilerstellung für das Modell werden als `Run`-Objekt ausgegeben. Weitere Informationen finden Sie in der Referenz zur [ModelProfile](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py)-Klasse.

## <a name="deploy-to-target"></a>Bereitstellen auf dem Ziel

Zum Bereitstellen der Modelle wird die Bereitstellungskonfiguration für die Rückschlusskonfiguration verwendet. Der Bereitstellungsprozess ist unabhängig vom Computeziel ähnlich. Die Bereitstellung von AKS ist geringfügig anders, da Sie eine Referenz zum AKS-Cluster bereitstellen müssen.

### <a id="local"></a> Lokale Bereitstellung

Um eine lokale Bereitstellung durchzuführen, müssen Sie Docker auf Ihrem lokalen Computer installiert haben.

#### <a name="using-the-sdk"></a>Verwenden des SDK

```python
from azureml.core.webservice import LocalWebservice, Webservice

deployment_config = LocalWebservice.deploy_configuration(port=8890)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
service.wait_for_deployment(show_output = True)
print(service.state)
```

Weitere Informationen finden Sie in der Referenzdokumentation zu [LocalWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py), [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

#### <a name="using-the-cli"></a>Verwenden der CLI

Verwenden Sie für die Bereitstellung mit der CLI den folgenden Befehl. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells:

```azurecli-interactive
az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [aml-local-deploy-config](../../../includes/machine-learning-service-local-deploy-config.md)]

Weitere Informationen finden Sie in der [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy)-Referenz.

### <a id="notebookvm"></a> NotebookVM-Webdienst (DEVTEST)

Siehe [Bereitstellen eines Modells in Notebook-VMs](how-to-deploy-local-container-notebook-vm.md).

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Lesen Sie dazu [Bereitstellen in Azure Container Instances](how-to-deploy-azure-container-instance.md).

### <a id="aks"></a>Azure Kubernetes Service (ENTWICKLUNG/TESTS und PRODUKTION)

Lesen Sie dazu [Bereitstellen in Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md).

## <a name="consume-web-services"></a>Nutzen von Webdiensten

Jeder bereitgestellte Webdienst stellt eine REST-API zur Verfügung, sodass Sie Clientanwendungen in einer Reihe verschiedener Programmiersprachen erstellen können. Wenn Sie die Schlüsselauthentifizierung für Ihren Dienst aktiviert haben, müssen Sie einen Dienstschlüssel als Token in Ihrem Anforderungsheader angeben.
Wenn Sie die Tokenauthentifizierung für Ihren Dienst aktiviert haben, müssen Sie ein Azure Machine Learning-JWT-Token als Bearertoken in Ihrem Anforderungsheader angeben.

> [!TIP]
> Sie können nach der Bereitstellung des Diensts das JSON-Schemadokument abrufen. Verwenden Sie die Eigenschaft [swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri) des bereitgestellten Webdiensts, z. B. `service.swagger_uri`, um den URI für die Swagger-Datei des lokalen Webdiensts abzurufen.

### <a name="request-response-consumption"></a>Nutzung von Anforderung/Antwort

Dies ist ein Beispiel, wie Sie Ihren Dienst in Python aufrufen können:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]
elif service.token_auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_token()[0]

print(headers)

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

response = requests.post(
    service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Weitere Informationen finden Sie unter [Erstellen von Clientanwendungen zur Nutzung von Webdiensten](how-to-consume-web-service.md).

### <a name="web-service-schema-openapi-specification"></a>Webdienst Schema (OpenAPI-Spezifikation)

Wenn Sie die automatische Schemagenerierung mit der Bereitstellung verwendet haben, können Sie die Adresse der OpenAPI-Spezifikation für den Dienst mithilfe der [swagger_uri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.local.localwebservice?view=azure-ml-py#swagger-uri)-Eigenschaft abrufen. Beispiel: `print(service.swagger_uri)`. Verwenden Sie eine GET-Anforderung (oder öffnen Sie den URI in einem Browser), um die Spezifikation abzurufen.

Das folgende JSON-Dokument ist ein Beispiel für ein Schema (OpenAPI-Spezifikation), das für eine Bereitstellung generiert wurde:

```json
{
    "swagger": "2.0",
    "info": {
        "title": "myservice",
        "description": "API specification for the Azure Machine Learning service myservice",
        "version": "1.0"
    },
    "schemes": [
        "https"
    ],
    "consumes": [
        "application/json"
    ],
    "produces": [
        "application/json"
    ],
    "securityDefinitions": {
        "Bearer": {
            "type": "apiKey",
            "name": "Authorization",
            "in": "header",
            "description": "For example: Bearer abc123"
        }
    },
    "paths": {
        "/": {
            "get": {
                "operationId": "ServiceHealthCheck",
                "description": "Simple health check endpoint to ensure the service is up at any given point.",
                "responses": {
                    "200": {
                        "description": "If service is up and running, this response will be returned with the content 'Healthy'",
                        "schema": {
                            "type": "string"
                        },
                        "examples": {
                            "application/json": "Healthy"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        },
        "/score": {
            "post": {
                "operationId": "RunMLService",
                "description": "Run web service's model and get the prediction output",
                "security": [
                    {
                        "Bearer": []
                    }
                ],
                "parameters": [
                    {
                        "name": "serviceInputPayload",
                        "in": "body",
                        "description": "The input payload for executing the real-time machine learning service.",
                        "schema": {
                            "$ref": "#/definitions/ServiceInput"
                        }
                    }
                ],
                "responses": {
                    "200": {
                        "description": "The service processed the input correctly and provided a result prediction, if applicable.",
                        "schema": {
                            "$ref": "#/definitions/ServiceOutput"
                        }
                    },
                    "default": {
                        "description": "The service failed to execute due to an error.",
                        "schema": {
                            "$ref": "#/definitions/ErrorResponse"
                        }
                    }
                }
            }
        }
    },
    "definitions": {
        "ServiceInput": {
            "type": "object",
            "properties": {
                "data": {
                    "type": "array",
                    "items": {
                        "type": "array",
                        "items": {
                            "type": "integer",
                            "format": "int64"
                        }
                    }
                }
            },
            "example": {
                "data": [
                    [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ]
                ]
            }
        },
        "ServiceOutput": {
            "type": "array",
            "items": {
                "type": "number",
                "format": "double"
            },
            "example": [
                3726.995
            ]
        },
        "ErrorResponse": {
            "type": "object",
            "properties": {
                "status_code": {
                    "type": "integer",
                    "format": "int32"
                },
                "message": {
                    "type": "string"
                }
            }
        }
    }
}
```

Weitere Informationen zur Spezifikation finden Sie in der [OpenAPI-Spezifikation](https://swagger.io/specification/).

Ein Hilfsprogramm, das Clientbibliotheken aus der Spezifikation erstellen kann, finden Sie unter [swagger-codegen](https://github.com/swagger-api/swagger-codegen).

### <a id="azuremlcompute"></a> Batchrückschluss
Azure Machine Learning Compute-Ziele werden von Azure Machine Learning Service erstellt und verwaltet. Sie können für Batchvorhersagen über Azure Machine Learning-Pipelines verwendet werden.

Eine exemplarische Vorgehensweise zu Batchrückschlüssen mit Azure Machine Learning Compute finden Sie im Artikel [Ausführen von Batchvorhersagen](how-to-run-batch-predictions.md).

### <a id="iotedge"></a> IoT Edge-Rückschluss
Unterstützung für die Bereitstellung auf Edge-Geräten befindet sich in der Vorschauphase. Weitere Informationen finden Sie im Artikel [Bereitstellen von Azure Machine Learning als IoT Edge-Modul](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a id="update"></a> Aktualisieren von Webdiensten

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="continuous-model-deployment"></a>Fortlaufende Modellbereitstellung 

Sie können Modelle fortlaufend mit der Machine Learning-Erweiterung für [Azure DevOps](https://azure.microsoft.com/services/devops/) bereitstellen. Mit der Machine Learning-Erweiterung für Azure DevOps können Sie eine Bereitstellungspipeline auslösen, wenn ein neues Machine Learning-Modell im Azure Machine Learning Service-Arbeitsbereich registriert ist. 

1. Registrieren Sie sich für [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), was eine kontinuierliche Integration und Bereitstellung Ihrer Anwendung auf jeder Plattform/in jeder Cloud ermöglicht. Azure Pipelines [unterscheidet sich von ML-Pipelines](concept-ml-pipelines.md#compare). 

1. [Erstellen eines Azure DevOps-Projekts.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Installieren der [Machine Learning-Erweiterung für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Richten Sie mit __Dienstverbindungen__ eine Dienstprinzipalverbindung mit Ihrem Azure Machine Learning Service-Arbeitsbereich ein, um auf alle Ihre Artefakte zuzugreifen. Wechseln Sie zu „Projekteinstellungen“, klicken Sie auf „Dienstverbindungen“, und wählen Sie „Azure Resource Manager“ aus.

    [![view-service-connection](media/how-to-deploy-and-where/view-service-connection.png)](media/how-to-deploy-and-where/view-service-connection-expanded.png) 

1. Definieren Sie AzureMLWorkspace als __Bereichsebene__, und geben Sie die folgenden Parameter ein.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Um Ihr Machine Learning-Modell mithilfe von Azure Pipelines kontinuierlich bereitzustellen, wählen Sie als Nächstes unter „Pipelines“ __Freigeben__ aus. Fügen Sie ein neues Artefakt hinzu, und wählen Sie „AzureML Model-Artefakt“ und die Dienstverbindung aus, die im vorherigen Schritt erstellt wurde. Wählen Sie das Modell und die Version aus, um eine Bereitstellung auszulösen. 

    [![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)](media/how-to-deploy-and-where/enable-modeltrigger-artifact-expanded.png)

1. Aktivieren Sie den Modelltrigger auf Ihrem Modellartefakt. Durch Aktivieren des Triggers wird jedes Mal, wenn die angegebene Version (d.h. die neueste Version) dieses Modells in Ihrem Arbeitsbereich registriert wird, eine Azure DevOps-Releasepipeline ausgelöst. 

    [![enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)](media/how-to-deploy-and-where/set-modeltrigger-expanded.png)

Weitere Beispielprojekte und Beispiele finden Sie in den folgenden Beispielrepositorys:

* [https://github.com/Microsoft/MLOps](https://github.com/Microsoft/MLOps)
* [https://github.com/Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.
Verwenden Sie zum Löschen eines registrierten Modells `model.delete()`.

Weitere Informationen finden Sie in der Referenzdokumentation zu [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--) und [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Nächste Schritte
* [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md)
* [Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI](how-to-troubleshoot-deployment.md)
* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)

