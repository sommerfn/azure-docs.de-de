---
title: Verwenden und Bereitstellen vorhandener Modelle
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie den Azure Machine Learning mit Modellen nutzen können, die außerhalb des Diensts trainiert wurden. Sie können Modelle registrieren, die außerhalb von Azure Machine Learning erstellt wurden, und diese dann als Webdienst oder Azure IoT Edge-Modul bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 11/06/2019
ms.openlocfilehash: 4e82d52428195797fb5cdc7a54e900fff7d37655
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73671443"
---
# <a name="use-an-existing-model-with-azure-machine-learning"></a>Verwenden eines vorhandenen Modells mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie, wie Sie ein vorhandenes Modell für maschinelles Lernen mit Azure Machine Learning nutzen können.

Wenn Sie ein Modell für maschinelles Lernen besitzen, das außerhalb von Azure Machine Learning trainiert wurde, können Sie den Dienst weiterhin verwenden, um das Modell als Webdienst oder auf einem IoT Edge-Gerät bereitzustellen. 

> [!TIP]
> Dieser Artikel enthält grundlegende Informationen zur Registrierung und Bereitstellung eines vorhandenen Modells. Nach der Bereitstellung stellt Azure Machine Learning die Überwachung Ihres Modells bereit. Es ermöglicht auch die Speicherung von Eingabedaten, die an die Bereitstellung gesendet werden, die für die Analyse von Datenabweichungen oder das Training neuer Versionen des Modells verwendet werden können.
>
> Weitere Informationen zu den hier verwendeten Konzepten und Begriffen finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen für maschinelles Lernen](concept-model-management-and-deployment.md).
>
> Allgemeine Informationen zum Bereitstellungsprozess finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Arbeitsbereichs](how-to-manage-workspace.md).

    > [!TIP]
    > Die Python-Beispiele in diesem Artikel gehen davon aus, dass die Variable `ws` auf Ihren Azure Machine Learning-Arbeitsbereich festgelegt ist.
    >
    > Die CLI-Beispiele verwenden einen Platzhalter von `myworkspace` und `myresourcegroup`. Ersetzen Sie diese durch den Namen Ihres Arbeitsbereichs und die Ressourcengruppe, die ihn enthält.

* Das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).  

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) und die [Machine Learning-CLI-Erweiterung](reference-azure-machine-learning-cli.md).

* Ein trainiertes Modell. Das Modell muss dauerhaft in mindestens eine Datei in Ihrer Entwicklungsumgebung gespeichert werden.

    > [!NOTE]
    > Um die Registrierung eines Modells zu veranschaulichen, das außerhalb des Azure Machine Learning trainiert wurde, verwenden die exemplarischen Codeausschnitte in diesem Artikel die Modelle, die vom Twitter-Stimmungsanalyseprojekt von Paolo Ripamonti erstellt wurden: [https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis](https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis).

## <a name="register-the-models"></a>Registrieren der Modelle

Die Registrierung eines Modells ermöglicht es Ihnen, Metadaten zu Modellen in Ihrem Arbeitsbereich zu speichern, zu versionieren und zu verfolgen. In den folgenden Python- und CLI-Beispielen enthält das Verzeichnis `models` die Dateien `model.h5`, `model.w2v`, `encoder.pkl` und `tokenizer.pkl`. In diesem Beispiel werden die im Verzeichnis `models` enthaltenen Dateien als neue Modellregistrierung mit dem Namen `sentiment` hochgeladen:

```python
from azureml.core.model import Model
# Tip: When model_path is set to a directory, you can use the child_paths parameter to include
#      only some of the files from the directory
model = Model.register(model_path = "./models",
                       model_name = "sentiment",
                       description = "Sentiment analysis model trained outside Azure Machine Learning",
                       workspace = ws)
```

Weitere Informationen finden Sie in der [Model.register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py#register-workspace--model-path--model-name--tags-none--properties-none--description-none--datasets-none--model-framework-none--model-framework-version-none--child-paths-none-)-Referenz.

```azurecli
az ml model register -p ./models -n sentiment -w myworkspace -g myresourcegroup
```

Weitere Informationen finden Sie in der [az ml model register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register)-Referenz.


Weitere Informationen zur Modellregistrierung im Allgemeinen finden Sie unter [Verwalten, Bereitstellen und Überwachen von Modellen für maschinelles Lernen](concept-model-management-and-deployment.md).


## <a name="define-inference-configuration"></a>Definieren der Rückschlusskonfiguration

Die Rückschlusskonfiguration definiert die Umgebung, in der das bereitgestellte Modell ausgeführt wird. Die Rückschlusskonfiguration verweist auf die folgenden Entitäten, die zum Ausführen des Modells verwendet werden, wenn es bereitgestellt wird:

* Ein Eingabeskript. Diese Datei (namens `score.py`) lädt das Modell, wenn der bereitgestellte Dienst gestartet wird. Sie ist auch dafür verantwortlich, Daten zu empfangen, sie an das Modell weiterzugeben und dann eine Antwort zurückzugeben.
* Eine Azure Machine Learning-[Umgebung](how-to-use-environments.md). Mit einer Umgebung werden die Softwareabhängigkeiten definiert, die zum Ausführen des Modells und Eingabeskripts benötigt werden.

Im folgenden Beispiel wird veranschaulicht, wie das SDK genutzt wird, um eine Umgebung zu erstellen und dann mit einer Rückschlusskonfiguration zu verwenden:

```python
from azureml.core.model import InferenceConfig
from azureml.core import Environment
from azureml.core.environment import CondaDependencies

# Create the environment
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Define the packages needed by the model and scripts
conda_dep.add_conda_package("tensorflow")
conda_dep.add_conda_package("numpy")
conda_dep.add_conda_package("scikit-learn")
conda_dep.add_pip_package("keras")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep

inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
```

Weitere Informationen finden Sie in den folgenden Artikeln:

+ [Erstellen und Verwalten von Umgebungen für Training und Bereitstellung](how-to-use-environments.md)
+ [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Referenz


Die CLI lädt die Rückschlusskonfiguration aus einer YAML-Datei:

```yaml
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "myenv.yml"
}
```

Mit der CLI wird die Conda-Umgebung in der Datei `myenv.yml` definiert, auf die in der Rückschlusskonfiguration verwiesen wird. Der folgende YAML-Code gibt den Inhalt dieser Datei wieder:

```yaml
name: inference_environment
dependencies:
- python=3.6.2
- tensorflow
- numpy
- scikit-learn
- pip:
    - azureml-defaults
    - keras
```

Weitere Informationen zur Rückschlusskonfiguration finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

### <a name="entry-script"></a>Eingabeskript

Das Eingabeskript weist nur zwei erforderliche Funktionen auf, `init()` und `run(data)`. Diese Funktionen werden verwendet, um den Dienst beim Start zu initialisieren und das Modell mit den von einem Client übergebenen Anforderungsdaten auszuführen. Der Rest des Skripts übernimmt das Laden und Ausführen der Modelle.

> [!IMPORTANT]
> Es gibt kein generisches Eingabeskript, das für alle Modelle funktioniert. Sie ist immer spezifisch für das verwendete Modell. Es muss verstehen, wie das Modell geladen wird, welches Datenformat das Modell erwartet und wie Daten mit dem Modell bewertet werden.

Der folgende Python-Code ist ein Beispiel für ein Eingabeskript (`score.py`):

```python
import os
import pickle
import json
import time
from keras.models import load_model
from keras.preprocessing.sequence import pad_sequences
from gensim.models.word2vec import Word2Vec

# SENTIMENT
POSITIVE = "POSITIVE"
NEGATIVE = "NEGATIVE"
NEUTRAL = "NEUTRAL"
SENTIMENT_THRESHOLDS = (0.4, 0.7)
SEQUENCE_LENGTH = 300

# Called when the deployed service starts
def init():
    global model
    global tokenizer
    global encoder
    global w2v_model

    # Get the path where the deployed model can be found.
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), './models')
    # load models
    model = load_model(model_path + '/model.h5')
    w2v_model = Word2Vec.load(model_path + '/model.w2v')

    with open(model_path + '/tokenizer.pkl','rb') as handle:
        tokenizer = pickle.load(handle)

    with open(model_path + '/encoder.pkl','rb') as handle:
        encoder = pickle.load(handle)

# Handle requests to the service
def run(data):
    try:
        # Pick out the text property of the JSON request.
        # This expects a request in the form of {"text": "some text to score for sentiment"}
        data = json.loads(data)
        prediction = predict(data['text'])
        #Return prediction
        return prediction
    except Exception as e:
        error = str(e)
        return error

# Determine sentiment from score
def decode_sentiment(score, include_neutral=True):
    if include_neutral:
        label = NEUTRAL
        if score <= SENTIMENT_THRESHOLDS[0]:
            label = NEGATIVE
        elif score >= SENTIMENT_THRESHOLDS[1]:
            label = POSITIVE
        return label
    else:
        return NEGATIVE if score < 0.5 else POSITIVE

# Predict sentiment using the model
def predict(text, include_neutral=True):
    start_at = time.time()
    # Tokenize text
    x_test = pad_sequences(tokenizer.texts_to_sequences([text]), maxlen=SEQUENCE_LENGTH)
    # Predict
    score = model.predict([x_test])[0]
    # Decode sentiment
    label = decode_sentiment(score, include_neutral=include_neutral)

    return {"label": label, "score": float(score),
       "elapsed_time": time.time()-start_at}  
```

Weitere Informationen zu Eingabeskripts finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="define-deployment"></a>Definieren der Bereitstellung

Das [Webdienst](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice?view=azure-ml-py)-Paket enthält die für die Bereitstellung verwendeten Klassen. Die von Ihnen verwendete Klasse bestimmt, wo das Modell bereitgestellt wird. Verwenden Sie z. B. zur Bereitstellung als Webdienst für Azure Kubernetes Service [AksWebService.deploy_configuration()](/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none--token-auth-enabled-none-), um die Bereitstellungskonfiguration zu erstellen.

Der folgende Python-Code definiert eine Bereitstellungskonfiguration für eine lokale Bereitstellung. Diese Konfiguration stellt das Modell als Webdienst auf Ihrem lokalen Computer bereit.

> [!IMPORTANT]
> Eine lokale Bereitstellung erfordert eine funktionierende Installation von [Docker](https://www.docker.com/) auf Ihrem lokalen Computer:

```python
from azureml.core.webservice import LocalWebservice

deployment_config = LocalWebservice.deploy_configuration()
```

Weitere Informationen finden Sie in der [LocalWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.localwebservice?view=azure-ml-py#deploy-configuration-port-none-)-Referenz.

Die CLI lädt die Bereitstellungskonfiguration aus einer YAML-Datei:

```YAML
{
    "computeType": "LOCAL"
}
```

Die Bereitstellung auf einem anderen Computeziel, z. B. Azure Kubernetes Service in der Azure Cloud, ist so einfach wie die Änderung der Bereitstellungskonfiguration. Weitere Informationen finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Das folgende Beispiel lädt Informationen über das registrierte Modell namens `sentiment` und stellt es dann als Dienst namens `sentiment` bereit. Während der Bereitstellung werden die Rückschluss- und die Bereitstellungskonfiguration verwendet, um die Dienstumgebung zu erstellen und zu konfigurieren:

```python
from azureml.core.model import Model

model = Model(ws, name='sentiment')
service = Model.deploy(ws, 'myservice', [model], inference_config, deployment_config)

service.wait_for_deployment(True)
print(service.state)
print("scoring URI: " + service.scoring_uri)
```

Weitere Informationen finden Sie in der [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)-Referenz.

Verwenden Sie den folgenden Befehl, um das Modell über die CLI bereitzustellen. Dieser Befehl stellt die Version 1 des registrierten Modells (`sentiment:1`) unter Verwendung der in den Dateien `inferenceConfig.json` und `deploymentConfig.json` gespeicherten Rückschluss- und Bereitstellungskonfiguration bereit:

```azurecli
az ml model deploy -n myservice -m sentiment:1 --ic inferenceConfig.json --dc deploymentConfig.json
```

Weitere Informationen finden Sie in der [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy)-Referenz.

Weitere Informationen zur Bereitstellung finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

## <a name="request-response-consumption"></a>Nutzung von Anforderung/Antwort

Nach der Bereitstellung wird der Bewertungs-URI angezeigt. Dieser URI kann von Clients verwendet werden, um Anfragen an den Dienst zu senden. Das folgende Beispiel ist ein einfacher Python-Client, der Daten an den Dienst übermittelt und die Antwort anzeigt:

```python
import requests
import json

scoring_uri = 'scoring uri for your service'
headers = {'Content-Type':'application/json'}

test_data = json.dumps({'text': 'Today is a great day!'})

response = requests.post(scoring_uri, data=test_data, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Weitere Informationen zur Verwendung des bereitgestellten Diensts finden Sie unter [Erstellen eines Clients](how-to-consume-web-service.md).

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
* [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md)
* [Erstellen eines Clients für ein bereitgestelltes Modell](how-to-consume-web-service.md)
