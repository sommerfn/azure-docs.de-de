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
ms.date: 07/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: 6b9ebb2f7ef46fd2900d036f178201863ecbc8d4
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358823"
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

- Ein Modell. Wenn Sie über kein trainiertes Modell verfügen, können Sie die Modell- und Abhängigkeitsdateien verwenden, die in [diesem Tutorial](https://aka.ms/azml-deploy-cloud) bereitgestellt werden.

- Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](https://aka.ms/aml-sdk) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registrieren Ihres Modells

Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell im Arbeitsbereich registrieren. Nach der Registrierung können Sie das registrierte Modell dann herunterladen oder bereitstellen und alle Dateien empfangen, die registriert wurden.

Machine Learning-Modelle werden in Ihrem Azure Machine Learning-Arbeitsbereich registriert. Das Modell kann aus Azure Machine Learning oder aus einer anderen Quelle stammen. Die folgenden Beispiele veranschaulichen das Registrieren eines Modells aus einer Datei.

### <a name="register-a-model-from-an-experiment-run"></a>Registrieren eines Modells aus einer Experimentausführung

+ **Scikit-Learn-Beispiel mit Verwendung des SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `model_path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden der CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `--asset-path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden von VS Code**

  Registrieren Sie Modelle, indem Sie beliebige Modelldateien oder -ordner mit der [VS Code](how-to-vscode-tools.md#deploy-and-manage-models)-Erweiterung verwenden.

### <a name="register-an-externally-created-model"></a>Registrieren eines extern erstellten Modells

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Sie können ein extern erstelltes Modell registrieren, indem Sie einen **lokalen Pfad** für das Modell bereitstellen. Sie können einen Ordner oder eine einzelne Datei angeben.

+ **ONNX-Beispiel mit dem Python SDK:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

  > [!TIP]
  > Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `model_path` auf das Verzeichnis ein, das die Dateien enthält.

+ **Verwenden der CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Um mehrere Dateien in die Modellregistrierung aufzunehmen, stellen Sie `-p` auf das Verzeichnis ein, das die Dateien enthält.

**Geschätzter Zeitaufwand**: Ungefähr 10 Sekunden.

Weitere Informationen finden Sie in der Referenzdokumentation zur [Modellklasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Weitere Informationen zur Arbeit mit Modellen, die außerhalb des Azure Machine Learning Service trainiert wurden, finden Sie unter [Verwenden eines vorhandenen Modells mit Azure Machine Learning Service](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Auswählen eines Computeziels

Die folgenden Computeziele bzw. Computeressourcen können verwendet werden, um Ihre Webdienstbereitstellung zu hosten. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Vorbereiten der Bereitstellung

Um die Bereitstellung als Webdienst durchzuführen, müssen Sie eine Rückschlusskonfiguration (`InferenceConfig`) und eine Bereitstellungskonfiguration erstellen. Rückschlüsse oder Modellbewertungen stellen die Phase dar, in der das bereitgestellte Modell für die Vorhersage verwendet wird (meist für Produktionsdaten). In der Rückschlusskonfiguration geben Sie die Skripts und Abhängigkeiten an, die für die Verarbeitung Ihres Modells erforderlich sind. In der Bereitstellungskonfiguration geben Sie Details zur Verarbeitung des Modells auf dem Computeziel an.

> [!IMPORTANT]
> Das Azure Machine Learning SDK bietet keinen Zugriff auf Ihren Datenspeicher oder Ihre Datasets durch Webdienste oder IoT Edge-Bereitstellungen. Wenn das bereitgestellte Modell auf Daten zugreifen muss, die außerhalb der Bereitstellung gespeichert sind – wie z.B. in einem Azure Storage-Konto –, müssen Sie mit dem entsprechenden SDK eine benutzerdefinierte Codelösung entwickeln. Ein Beispiel hierfür ist das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python).
>
> Eine andere Alternative, die in Ihrem Szenario funktionieren könnte, ist die [Batchvorhersage](how-to-run-batch-predictions.md), die beim Erstellen von Bewertungen Zugriff auf Datenspeicher bietet.

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

#### <a name="optional-automatic-swagger-schema-generation"></a>(Optional) Automatische Generierung des Swagger-Schemas

Um automatisch ein Schema für Ihren Webdienst zu generieren, stellen Sie ein Beispiel der Eingabe und/oder Ausgabe im Konstruktor für eines der definierten Typobjekte bereit, und der Typ und das Beispiel werden zur automatischen Erstellung des Schemas verwendet. Azure Machine Learning Service erstellt dann während der Bereitstellung eine [OpenAPI](https://swagger.io/docs/specification/about/)-Spezifikation (Swagger) für den Webdienst.

Folgende Typen werden derzeit unterstützt:

* `pandas`
* `numpy`
* `pyspark`
* Python-Standardobjekt

Um Schemagenerierung zu verwenden, schließen Sie das `inference-schema`-Paket in Ihre Conda-Umgebungsdatei ein. Das folgende Beispiel verwendet `[numpy-support]`, da das Eingangsskript einen NumPy-Parametertyp verwendet: 

#### <a name="example-dependencies-file"></a>Beispieldatei für Abhängigkeiten
Das folgende YAML-Beispiel zeigt eine Conda-Abhängigkeitsdatei für den Rückschluss.

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

> [!TIP]
> Verwenden Sie nach der Bereitstellung des Diensts die Eigenschaft `swagger_uri`, um das JSON-Schemadokument abzurufen.

#### <a name="example-entry-script"></a>Beispiel für ein Eingangsskript

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

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Beispielskript mit Wörterbucheingabe (Unterstützung der Nutzung von Daten aus Power BI)

Im folgende Beispiel wird veranschaulicht, wie Eingabedaten unter Verwendung von Datenrahmen als Wörterbuch des Typs <Schlüssel: Wert> definiert werden. Diese Methode wird für die Nutzung des von Power BI bereitgestellten Webdiensts unterstützt ([erfahren Sie mehr über den Webdienst von Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

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
* Bewertung anhand von Binärdaten: [Nutzen eines Webdiensts](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Definieren der Rückschlusskonfiguration

Die Rückschlusskonfiguration beschreibt, wie das Modell zum Treffen von Vorhersagen konfiguriert wird. Im folgenden Beispiel wird veranschaulicht, wie eine Rückschlusskonfiguration erstellt wird. Diese Konfiguration gibt die Runtime, das Eingabeskript und (optional) die Conda-Umgebungsdatei an:

```python
inference_config = InferenceConfig(runtime="python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Weitere Informationen finden Sie in der [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klassenreferenz.

Informationen zur Verwendung eines benutzerdefinierten Docker-Images mit Rückschlusskonfiguration finden Sie unter [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>CLI-Beispiel für InferenceConfig

[!INCLUDE [inferenceconfig](../../../includes/machine-learning-service-inference-config.md)]

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

Bevor Sie die Bereitstellung ausführen, müssen Sie die Bereitstellungskonfiguration definieren. Die Bereitstellungskonfiguration ist spezifisch für das Computeziel, das den Webdienst hosten wird. Beispielsweise müssen Sie bei einer lokalen Bereitstellung den Port angeben, an dem der Dienst Anforderungen akzeptiert.

Möglicherweise müssen Sie die Computeressourcen auch erstellen. Dies ist beispielsweise der Fall, wenn Sie noch keinen Azure Kubernetes Service für Ihren Arbeitsbereich eingerichtet haben.

Die folgende Tabelle enthält ein Beispiel für das Erstellen einer Bereitstellungskonfiguration für jedes Computeziel:

| Computeziel | Beispiel für eine Bereitstellungskonfiguration |
| ----- | ----- |
| Lokal | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instances | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Azure Kubernetes Service | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

Die folgenden Abschnitte zeigen, wie Sie die Bereitstellungskonfiguration erstellen und sie dann zum Bereitstellen des Webdiensts verwenden.

### <a name="optional-profile-your-model"></a>Optional: Erstellen eines Profils für Ihr Modell

Es empfiehlt sich gegebenenfalls, ein Profil für Ihr Modell zu erstellen, um die optimalen CPU- und Arbeitsspeicheranforderungen mithilfe des SDK oder der CLI zu bestimmen, bevor Sie Ihr Modell als Dienst bereitstellen.  Die Ergebnisse der Profilerstellung für das Modell werden als `Run`-Objekt ausgegeben. Die vollständigen Details des [Modellprofilschemas finden Sie in der API-Dokumentation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py).

Weitere Informationen zum [Erstellen eines Profils für das Modell mithilfe des SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-).

Verwenden Sie zum Erstellen eines Profils für das Modell mithilfe der CLI [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

## <a name="deploy-to-target"></a>Bereitstellen auf dem Ziel

### <a id="local"></a> Lokale Bereitstellung

Um eine lokale Bereitstellung durchzuführen, müssen Sie **Docker** auf Ihrem lokalen Computer installiert haben.

+ **Verwenden des SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Verwenden der CLI**

    Verwenden Sie für die Bereitstellung mit der CLI den folgenden Befehl. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells:

  ```azurecli-interactive
  az ml model deploy -m mymodel:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    [!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-local-deploy-config.md)]

### <a id="aci"></a> Azure Container Instances (DEVTEST)

Verwenden Sie Azure Container Instances für die Bereitstellung Ihrer Modelle als Webdienst, wenn mindestens eine der folgenden Bedingungen zutrifft:
- Sie müssen Ihr Modell schnell bereitstellen und überprüfen.
- Sie testen ein Modell in der Entwicklungsphase. 

Um Kontingente und die Regionsverfügbarkeit für ACI anzuzeigen, lesen Sie den Artikel [Kontingente und Regionsverfügbarkeit für Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas).

+ **Verwenden des SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Verwenden der CLI**

    Verwenden Sie für die Bereitstellung mit der CLI den folgenden Befehl. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells. Ersetzen Sie `myservice` durch den Namen, den dieser Dienst erhalten soll:

    ```azurecli-interactive
    az ml model deploy -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
    ```

    [!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aci-deploy-config.md)]

+ **Verwenden von VS Code**

  Zum [Bereitstellen Ihrer Modelle mit VS Code](how-to-vscode-tools.md#deploy-and-manage-models) müssen Sie für Tests nicht vorab einen ACI-Container erstellen, da ACI-Container dynamisch erstellt werden.

Weitere Informationen finden Sie in der Referenzdokumentation für die Klassen [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) und [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a>Azure Kubernetes Service (ENTWICKLUNG/TESTS und PRODUKTION)

Sie können einen vorhandenen AKS-Cluster verwenden oder mithilfe des Azure Machine Learning-SDK, der CLI oder des Azure-Portals einen neuen erstellen.

<a id="deploy-aks"></a>

Falls Sie bereits einen AKS-Cluster angefügt haben, können Sie dafür die Bereitstellung durchführen. Führen Sie die Schritte zum <a href="#create-attach-aks">Erstellen eines neuen AKS-Clusters</a> aus, wenn Sie noch keinen AKS-Cluster erstellt oder angefügt haben.

+ **Verwenden des SDK**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **Verwenden der CLI**

    Verwenden Sie für die Bereitstellung mit der CLI den folgenden Befehl. Ersetzen Sie `myaks` durch den Namen des AKS-Computeziels. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells. Ersetzen Sie `myservice` durch den Namen, den dieser Dienst erhalten soll:

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

    [!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

+ **Verwenden von VS Code**

  Sie können auch die [Bereitstellung für AKS über die VS Code-Erweiterung](how-to-vscode-tools.md#deploy-and-manage-models) wählen, aber in diesem Fall müssen Sie vorab AKS-Cluster konfigurieren.

Weitere Informationen zur AKS-Bereitstellung und zur automatischen Skalierung finden Sie im Referenzartikel zu [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice).

#### Erstellen eines neuen AKS-Clusters<a id="create-attach-aks"></a>
**Geschätzter Zeitaufwand**: Ca. 20 Minuten.

Das Erstellen oder Anfügen eines AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Vorgang. Sie können diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe löschen, die ihn enthält, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen. Sie können an Ihren Arbeitsbereich mehrere AKS-Cluster anfügen.

Falls Sie einen AKS-Cluster für Entwicklung, Validierung und Tests erstellen möchten, legen Sie `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` fest, wenn Sie [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) verwenden. Ein mit dieser Einstellung erstellter Cluster hat nur einen Knoten.

> [!IMPORTANT]
> Bei Festlegen von `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` wird ein AKS-Cluster erstellt, der nicht für die Verarbeitung von Produktionsdatenverkehr geeignet ist. Rückschlusszeiten können länger sein als bei einem Cluster, der für die Produktion angelegt wurde. Für Entwicklungs- und Testcluster wird zudem keine Fehlertoleranz garantiert.
>
> Wir empfehlen, dass Cluster, die für Entwicklung und Tests erstellt werden, mindestens zwei virtuelle CPUs aufweisen.

Das folgende Beispiel zeigt, wie ein neuer Azure Kubernetes Service-Cluster erstellt wird:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace=ws,
                                  name=aks_name,
                                  provisioning_configuration=prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output=True)
```

Weitere Informationen zum Erstellen eines AKS-Clusters außerhalb des Azure Machine Learning SDK finden Sie in den folgenden Artikeln:
* [az aks create](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Schnellstart: Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über das Azure-Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Weitere Informationen zum Parameter `cluster_purpose` finden Sie in der Referenz zu [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py).

> [!IMPORTANT]
> Wenn Sie für [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) benutzerdefinierte Werte für agent_count und vm_size auswählen, müssen Sie sicherstellen, dass agent_count multipliziert mit vm_size mindestens 12 virtuellen CPUs entspricht. Wenn Sie beispielsweise für vm_size „Standard_D3_v2“ verwenden, das 4 virtuelle CPUs hat, müssen Sie für agent_count einen Wert von mindestens 3 wählen.
>
> Das Azure Machine Learning-SDK bietet keine Unterstützung zur Skalierung eines AKS-Clusters. Die Knoten in Ihrem Cluster können Sie über die Benutzeroberfläche für Ihren AKS-Cluster im Azure-Portal skalieren. Sie können nur die Knotenanzahl ändern, nicht die VM-Größe des Clusters.

#### <a name="attach-an-existing-aks-cluster"></a>Anfügen eines vorhandenen AKS-Clusters
**Geschätzter Zeitaufwand:** Ca. fünf Minuten.

Wenn Sie in Ihrem Azure-Abonnement bereits über einen AKS-Cluster verfügen und dieser die Version 1.12.* hat, können Sie diesen für die Bereitstellung Ihres Image verwenden.

> [!WARNING]
> Wenn Sie einen AKS-Cluster an einen Arbeitsbereich anfügen, können Sie durch Festlegen des Parameters `cluster_purpose` bestimmen, wie Sie den Cluster verwenden möchten.
>
> Wenn Sie den Parameter `cluster_purpose` nicht bzw. `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD` festlegen, muss der Cluster über mindestens 12 virtuelle CPUs verfügen.
>
> Wenn Sie `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` festlegen, muss der Cluster nicht über 12 virtuelle CPUs verfügen. Ein Cluster, der für Entwicklung/Tests konfiguriert ist, eignet sich jedoch nicht für Produktionsdatenverkehr und kann Rückschlusszeiten erhöhen.

Der folgende Code veranschaulicht das Anfügen eines vorhandenen AKS-Clusters der Version 1.12.## an Ihren Arbeitsbereich:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group=resource_group,
                                                cluster_name=cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Weitere Informationen zu `attack_configuration()` finden Sie in der Referenz zu [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-).

Weitere Informationen zum Parameter `cluster_purpose` finden Sie in der Referenz zu [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py).

## <a name="consume-web-services"></a>Nutzen von Webdiensten

Jeder bereitgestellte Webdienst stellt eine REST-API zur Verfügung, sodass Sie Clientanwendungen in einer Reihe verschiedener Programmiersprachen erstellen können. Wenn Sie die Authentifizierung für Ihren Dienst aktiviert haben, müssen Sie einen Dienstschlüssel als Token in Ihrem Anforderungsheader angeben.

### <a name="request-response-consumption"></a>Nutzung von Anforderung/Antwort

Dies ist ein Beispiel, wie Sie Ihren Dienst in Python aufrufen können:
```python
import requests
import json

headers = {'Content-Type': 'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

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


### <a id="azuremlcompute"></a> Batchrückschluss
Azure Machine Learning Compute-Ziele werden von Azure Machine Learning Service erstellt und verwaltet. Sie können für Batchvorhersagen über Azure Machine Learning-Pipelines verwendet werden.

Eine exemplarische Vorgehensweise zu Batchrückschlüssen mit Azure Machine Learning Compute finden Sie im Artikel [Ausführen von Batchvorhersagen](how-to-run-batch-predictions.md).

### <a id="iotedge"></a> IoT Edge-Rückschluss
Unterstützung für die Bereitstellung auf Edge-Geräten befindet sich in der Vorschauphase. Weitere Informationen finden Sie im Artikel [Bereitstellen von Azure Machine Learning als IoT Edge-Modul](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a id="update"></a> Aktualisieren von Webdiensten

Wenn Sie ein neues Modell erstellen, müssen Sie jeden Dienst, der das neue Modell verwenden soll, manuell aktualisieren. Verwenden Sie die Methode `update`, um den Webdienst zu aktualisieren. Der folgende Code veranschaulicht, wie Sie den Webdienst so aktualisieren, dass er ein neues Modell verwendet:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path="outputs/sklearn_mnist_model.pkl",
                           model_name="sklearn_mnist",
                           tags={"key": "0.1"},
                           description="test",
                           workspace=ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name=service_name, workspace=ws)

# Update to new model(s)
service.update(models=[new_model])
print(service.state)
print(service.get_logs())
```

## <a name="continuous-model-deployment"></a>Fortlaufende Modellbereitstellung 

Sie können Modelle fortlaufend mit der Machine Learning-Erweiterung für [Azure DevOps](https://azure.microsoft.com/services/devops/) bereitstellen. Mit der Machine Learning-Erweiterung für Azure DevOps können Sie eine Bereitstellungspipeline auslösen, wenn ein neues Machine Learning-Modell im Azure Machine Learning Service-Arbeitsbereich registriert ist. 

1. Registrieren Sie sich für [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), was eine kontinuierliche Integration und Bereitstellung Ihrer Anwendung auf jeder Plattform/in jeder Cloud ermöglicht. Azure Pipelines [unterscheidet sich von ML-Pipelines](concept-ml-pipelines.md#compare). 

1. [Erstellen eines Azure DevOps-Projekts.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Installieren der [Machine Learning-Erweiterung für Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Richten Sie mit __Dienstverbindungen__ eine Dienstprinzipalverbindung mit Ihrem Azure Machine Learning Service-Arbeitsbereich ein, um auf alle Ihre Artefakte zuzugreifen. Wechseln Sie zu „Projekteinstellungen“, klicken Sie auf „Dienstverbindungen“, und wählen Sie „Azure Resource Manager“ aus.

    ![view-service-connection](media/how-to-deploy-and-where/view-service-connection.png) 

1. Definieren Sie AzureMLWorkspace als __Bereichsebene__, und geben Sie die folgenden Parameter ein.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Um Ihr Machine Learning-Modell mithilfe von Azure Pipelines kontinuierlich bereitzustellen, wählen Sie als Nächstes unter „Pipelines“ __Freigeben__ aus. Fügen Sie ein neues Artefakt hinzu, und wählen Sie „AzureML Model-Artefakt“ und die Dienstverbindung aus, die im vorherigen Schritt erstellt wurde. Wählen Sie das Modell und die Version aus, um eine Bereitstellung auszulösen. 

    ![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Aktivieren Sie den Modelltrigger auf Ihrem Modellartefakt. Durch Aktivieren des Triggers wird jedes Mal, wenn die angegebene Version (d.h. die neueste Version) dieses Modells in Ihrem Arbeitsbereich registriert wird, eine Azure DevOps-Releasepipeline ausgelöst. 

    ![enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)

Beispielprojekte und Beispiele finden Sie im [MLOps-Repository](https://github.com/Microsoft/MLOps)

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

