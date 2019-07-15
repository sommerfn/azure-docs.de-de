---
title: Bereitstellen eines Modells für das Ziehen von Rückschlüssen mit einer GPU
titleSuffix: Azure Machine Learning service
description: Dieser Artikel zeigt Ihnen, wie Sie den Azure Machine Learning Service verwenden können, um ein GPU-fähiges Tensorflow-Deep Learning Modell als web service.service und Score Inferenzanforderungen bereitzustellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: 8086d059913cc61bff0bca31681368bea6d76777
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543805"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Bereitstellen eines Deep Learning-Modells für das Ziehen von Rückschlüssen mit einer GPU

Dieser Artikel zeigt Ihnen, wie Sie den Azure Machine Learning Service verwenden können, um ein GPU-fähiges Tensorflow Deep Learning Modell als Webservice bereitzustellen.

Stellen Sie Ihr Modell in einem Azure Kubernetes Service (AKS)-Cluster bereit, um GPU-fähige Inferenzierungen durchzuführen. Inferenzierung oder Modellbewertung ist die Phase, in der das eingesetzte Modell für die Vorhersage verwendet wird. Die Verwendung von GPUs anstelle von CPUs bietet Leistungsvorteile bei hochparallelisierbaren Berechnungen.

Obwohl dieses Beispiel ein TensorFlow-Modell verwendet, können Sie die folgenden Schritte auf jedes Machine Learning-Framework anwenden, das GPUs unterstützt, indem Sie kleine Änderungen an der Bewertungsdatei und der Umgebungsdatei vornehmen. 

In diesem Artikel führen Sie die folgenden Schritte aus:

* Erstellen eines GPU-fähigen AKS-Clusters
* Bereitstellen eines TensorFlow-GPU-Modells
* Ausführen einer Beispielabfrage für Ihr bereitgestelltes Modell

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning Service-Arbeitsbereich.
* Eine Python-Distribution.
* Ein registriertes gespeichertes TensorFlow-Modell.
    * Informationen zum Registrieren von Modellen finden Sie unter [Bereitstellen von Modellen](../service/how-to-deploy-and-where.md#registermodel).

Sie können den ersten Teil dieser How-to-Serie, [Wie man ein TensorFlow-Modell trainiert](how-to-train-tensorflow.md), abschließen, um die notwendigen Voraussetzungen zu erfüllen.

## <a name="provision-an-aks-cluster-with-gpus"></a>Bereitstellen eines AKS-Clusters mit GPUs

Azure verfügt über viele verschiedene GPU-Optionen. Diese können Sie alle für das Ziehen von Rückschlüssen verwenden. In der [Liste mit den VMs der N-Serie](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) finden Sie eine vollständige Aufschlüsselung der Funktionen und Kosten.

Weitere Informationen zur Verwendung von AKS mit Azure Machine Learning Service finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    compute_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Azure berechnet Ihnen Gebühren, solange der AKS-Cluster bereitgestellt ist. Stellen Sie sicher, dass Sie Ihren AKS-Cluster löschen, wenn Sie ihn nicht mehr benötigen.

## <a name="write-the-entry-script"></a>Schreiben des Eingangsskripts

Speichern Sie den folgenden Code als `score.py` in Ihrem Arbeitsverzeichnis. Mit dieser Datei werden Bilder bewertet, wenn sie an Ihren Dienst gesendet werden. Sie lädt das gespeicherte TensorFlow-Modell und übergibt bei jeder POST-Anforderung dann das Eingabebild an die TensorFlow-Sitzung und gibt die sich ergebenden Bewertungen zurück. Für andere Frameworks zum Ziehen von Rückschlüssen sind andere Bewertungsdateien erforderlich.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Definieren der Conda-Umgebung

Erstellen Sie eine Conda-Umgebungsdatei namens `myenv.yml`, um die Abhängigkeiten für Ihren Dienst anzugeben. Es ist wichtig anzugeben, dass Sie `tensorflow-gpu` verwenden, um eine beschleunigte Leistung zu erzielen.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>Definieren der InferenceConfig-GPU-Klasse

Erstellen Sie ein `InferenceConfig`-Objekt, um die GPUs zu aktivieren und sicherzustellen, dass CUDA mit Ihrem Docker-Image installiert wird.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
```

Weitere Informationen finden Sie unter

- [InferenceConfig-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [AksServiceDeploymentConfiguration-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Stellen Sie dieses Modell in Ihrem AKS-Cluster bereit, und warten Sie, bis es den Dienst erstellt.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> Der Azure Machine Learning Service stellt kein Modell mit einem `InferenceConfig`-Objekt bereit, für das erwartet wird, dass die GPU für einen Cluster ohne GPU aktiviert ist.

Weitere Informationen finden Sie unter [Model class](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py) (Model-Klasse).

## <a name="issue-a-sample-query-to-your-model"></a>Ausführen einer Beispielabfrage an Ihr Modell

Senden Sie eine Testabfrage an das bereitgestellte Modell. Wenn Sie ein JPEG-Bild an das Modell senden, wird es bewertet. Das folgende Codebeispiel verwendet eine externe Hilfsfunktion zum Laden von Bildern. Den entsprechenden Code finden Sie unter pir [TensorFlow Beispiel auf GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py) . 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

> [!IMPORTANT]
> Stellen Sie sicher, dass sich Ihr Client in derselben Azure-Region wie der Endpunkt befindet, um die Latenz zu verringern und den Durchsatz zu optimieren. In diesem Beispiel werden die APIs in der Azure-Region „USA, Osten“ erstellt.

## <a name="clean-up-the-resources"></a>Bereinigen der Ressourcen

Löschen Sie Ihre Ressourcen, nachdem Sie dieses Beispiel abgeschlossen haben.

> [!IMPORTANT]
> Azure berechnet Ihnen Gebühren basierend darauf, wie lange der AKS-Cluster bereitgestellt wird. Achten Sie darauf, ihn zu bereinigen, wenn Sie ihn nicht mehr nutzen.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen eines Modells auf einem FPGA](../service/how-to-deploy-fpga-web-service.md)
* [Bereitstellen eines Modells mit ONNX](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Trainieren von TensorFlow-DNN-Modellen](../service/how-to-train-tensorflow.md)
