---
title: Trainieren eines neuronalen Deep Learning-Netzes mit TensorFlow
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie TensorFlow-Trainingsskripts mithilfe von Azure Machine Learning bedarfsorientiert ausführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 08/20/2019
ms.custom: seodec18
ms.openlocfilehash: 11b16f91d600c20b48fbdc5887a4a0a4b538e916
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330656"
---
# <a name="build-a-tensorflow-deep-learning-model-at-scale-with-azure-machine-learning"></a>Bedarfsorientiertes Erstellen eines TensorFlow-Deep Learning-Modells mit Azure Machine Learning

In diesem Artikel wird gezeigt, wie Sie Ihre [TensorFlow](https://www.tensorflow.org/overview)-Trainingsskripts bedarfsorientiert mit der [TensorFlow-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)-Klasse von Azure Machine Learning ausführen. In diesem Beispiel wird ein TensorFlow-Modell trainiert und registriert, um handschriftliche Ziffern mithilfe eines Deep Neural Network (DNN) zu klassifizieren.

Unabhängig davon, ob Sie ein TensorFlow-Modell von Grund auf entwickeln oder ob Sie ein [vorhandenes Modell](how-to-deploy-existing-model.md) in die Cloud bringen, können Sie Azure Machine Learning zum Erweitern von Open-Source-Trainingsaufträgen verwenden, um für die Produktion geeignete Modelle zu erstellen, bereitzustellen und zu überwachen sowie ihre Versionen zu verwalten.

Weitere Informationen zu [Deep Learning im Vergleich zum maschinellen Lernen](concept-deep-learning-vs-machine-learning.md).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie diesen Code in einer dieser Umgebungen aus:

 - Azure Machine Learning Notebook VM: keine Downloads oder Installationen erforderlich

     - Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebookserver zu erstellen, auf dem das SDK und Beispielrepository vorinstalliert sind.
    - Suchen Sie im Deep Learning-Beispielordner auf dem Notebook-Server ein fertiges und erweitertes Notebook. Dazu navigieren Sie zum folgenden Verzeichnis: **how-to-use-azureml > ml-frameworks > tensorflow > deployment > train-hyperparameter-tune-deploy-with-tensorflow**. 
 
 - Ihr eigener Jupyter Notebook-Server

    - [Installieren Sie das Azure Machine Learning SDK.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Erstellen Sie eine Konfigurationsdatei für den Arbeitsbereich.](how-to-configure-environment.md#workspace)
    - [Laden Sie die Beispielskriptdateien](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` und `utils.py` herunter.
     
    Auf der GitHub-Seite mit Beispielen finden Sie außerdem eine fertige [Jupyter Notebook-Version](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/deployment/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) dieser Anleitung. Das Notebook umfasst erweiterte Abschnitte, in denen die intelligente Hyperparameteroptimierung, die Modellimplementierung und Notebook-Widgets behandelt werden.

## <a name="set-up-the-experiment"></a>Einrichten des Experiments

In diesem Abschnitt wird das Trainingsexperiment eingerichtet, indem die erforderlichen Python-Pakete geladen, ein Arbeitsbereich initialisiert, ein Experiment erstellt und die Trainingsdaten und -skripts hochgeladen werden.

### <a name="import-packages"></a>Importieren von Paketen

Importieren Sie zunächst die erforderlichen Python-Bibliotheken.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Initialisieren eines Arbeitsbereichs

Der [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für den Dienst. Er stellt den zentralen Ort für die Arbeit mit allen erstellten Artefakten dar. Im Python SDK können Sie auf die Arbeitsbereichsartefakte zugreifen, indem Sie ein [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py)-Objekt erstellen.

Erstellen Sie ein Arbeitsbereichsobjekt aus der Datei `config.json`, die im [Abschnitt „Voraussetzungen“](#prerequisites) erstellt wurde.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-deep-learning-experiment"></a>Erstellen eines Deep Learning-Experiments

Erstellen Sie ein Experiment und einen Ordner, in dem Ihre Trainingsskripts gespeichert werden. In diesem Beispiel erstellen Sie ein Experiment mit dem Namen „tf-mnist“.

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="create-a-file-dataset"></a>Erstellen eines Dateidatasets (FileDataset)

Ein `FileDataset`-Objekt verweist auf Dateien in Ihrem Arbeitsbereichsdatenspeicher oder in öffentlichen URLs. Die Dateien können ein beliebiges Format haben, und die Klasse ermöglicht es Ihnen, die Dateien in Ihre Computeinstanz herunterzuladen oder einzubinden. Durch Erstellen eines `FileDataset`-Objekts erstellen Sie einen Verweis auf den Speicherort der Datenquelle. Wenn Sie Transformationen auf das Dataset angewendet haben, werden diese ebenfalls im Dataset gespeichert. Die Daten verbleiben an ihrem Speicherort, sodass keine zusätzlichen Speicherkosten anfallen. Weitere Informationen finden Sie in der [Beschreibung](https://docs.microsoft.com/azure/machine-learning/service/how-to-create-register-datasets) des `Dataset`-Pakets.

```python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
dataset = Dataset.File.from_files(path=web_paths)
```

Verwenden Sie die `register()`-Methode, um das Dataset in Ihrem Arbeitsbereich zu registrieren, damit es für andere Benutzer freigegeben und für unterschiedliche Experimente wiederverwendet werden kann, und damit in Ihrem Trainingsskript über den Namen auf es verwiesen werden kann.

```python
dataset = dataset.register(workspace=ws,
                           name='mnist dataset',
                           description='training and test dataset',
                           create_new_version=True)

# list the files referenced by dataset
dataset.to_path()
```

## <a name="create-a-compute-target"></a>Erstellen eines Computeziels

Erstellen Sie ein Computeziel, auf dem der TensorFlow-Auftrag ausgeführt werden soll. In diesem Beispiel erstellen Sie einen GPU-fähigen Azure Machine Learning-Computercluster.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Weitere Informationen zu Computezielen finden Sie im Artikel [Was ist ein Computeziel?](concept-compute-target.md).

## <a name="create-a-tensorflow-estimator"></a>Erstellen eines TensorFlow-Estimators

Der [TensorFlow-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) bietet eine einfache Möglichkeit, einen TensorFlow-Trainingsauftrag auf einem Computeziel zu starten.

Der TensorFlow-Estimator wird durch die generische [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Klasse implementiert, die zur Unterstützung beliebiger Frameworks verwendet werden kann. Weitere Informationen zum Trainieren von Modellen mit dem generischen Estimator finden Sie unter [Trainieren von Azure Machine Learning-Modellen mit einem Estimator](how-to-train-ml-models.md).

Wenn für Ihr Trainingsskript zusätzliche Pip- oder Conda-Pakete ausgeführt werden müssen, können Sie die Pakete auf dem resultierenden Docker-Image installieren, indem Sie die Namen mit dem `pip_packages`-Argument und `conda_packages`-Argument übergeben.

```python
script_params = {
    '--data-folder': dataset.as_named_input('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Initiieren einer Ausführung

Das [Run-Objekt](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) bildet die Schnittstelle zum Ausführungsverlauf, während der Auftrag ausgeführt wird und nachdem er abgeschlossen wurde.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Die Ausführung durchläuft die folgenden Phasen:

- **Vorbereitung**: Gemäß dem TensorFlow-Estimator wird ein Docker-Image erstellt. Das Image wird in die Containerregistrierung des Arbeitsbereichs hochgeladen und für spätere Ausführungen zwischengespeichert. Darüber hinaus werden Protokolle in den Ausführungsverlauf gestreamt, mit deren Hilfe der Status überwacht werden kann.

- **Skalierung**: Der Cluster versucht ein zentrales Hochskalieren, wenn der Batch KI-Cluster mehr Knoten zur Ausführung benötigt, als derzeit verfügbar sind.

- **Running**: Alle Skripts im Skriptordner werden auf das Computeziel hochgeladen, Datenspeicher werden bereitgestellt oder kopiert, und das „entry_script“ wird ausgeführt. Ausgaben aus „stdout“ und dem Ordner „./logs“ werden in den Ausführungsverlauf gestreamt und können zur Überwachung der Ausführung verwendet werden.

- **Nachbearbeitung**: Der Ordner „./outputs“ der Ausführung wird in den Ausführungsverlauf kopiert.

## <a name="register-or-download-a-model"></a>Registrieren oder Herunterladen eines Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

Mit dem Run-Objekt können Sie auch eine lokale Kopie des Modells herunterladen. Im Trainingsskript `mnist-tf.py` wird das Modell durch ein saver-Objekt von TensorFlow persistent in einem lokalen Ordner (lokal für das Computeziel) gespeichert. Sie können das Run-Objekt verwenden, um eine Kopie herunterzuladen.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Verteiltes Training

Der [`TensorFlow`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)-Estimator unterstützt auch verteiltes Training für CPU- und GPU-Cluster. Sie können ganz einfach verteilte TensorFlow-Aufträge ausführen. Azure Machine Learning verwaltet die Orchestrierung für Sie.

Azure Machine Learning unterstützt zwei Methoden des verteilten Trainings in TensorFlow:

- Verteiltes Training auf Grundlage von [MPI](https://www.open-mpi.org/) mithilfe des [Horovod](https://github.com/uber/horovod)-Frameworks
- Natives [verteiltes TensorFlow-Training](https://www.tensorflow.org/deploy/distributed) über die Parameterservermethode

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) ist ein Open-Source-Framework, das von Uber für verteiltes Training entwickelt wurde. Es ermöglicht die einfache Erstellung von verteilten GPU-TensorFlow-Aufträgen.

Um Horovod zu verwenden, geben Sie im TensorFlow-Konstruktor ein [`MpiConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py)-Objekt für den`distributed_training`-Parameter an. Dieser Parameter stellt sicher, dass die Horovod-Bibliothek installiert ist, die Sie in Ihrem Trainingsskript verwenden.

```Python
from azureml.core.runconfig import MpiConfiguration
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Parameterserver

Sie können alternativ auch das [native verteilte TensorFlow-Training](https://www.tensorflow.org/deploy/distributed) ausführen, das auf das Parameterservermodell zurückgreift. Bei dieser Methode führen Sie das Training auf einem Cluster mit Parameterservern und Workern aus. Während des Trainings berechnen die Worker die Gradienten, und die Parameterserver aggregieren diese.

Um die Parameterservermethode zu verwenden, geben Sie im TensorFlow-Konstruktor ein [`TensorflowConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?view=azure-ml-py)-Objekt für den`distributed_training`-Parameter an.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tf_config"></a>Definieren von Clusterspezifikationen in „TF_CONFIG“

Sie benötigen für [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec) zusätzlich die Netzwerkadressen und Ports des Clusters, damit Azure Machine Learning die `TF_CONFIG`-Umgebungsvariable automatisch festlegt.

Die `TF_CONFIG`-Umgebungsvariable ist eine JSON-Zeichenfolge. Im folgenden Codeausschnitt finden Sie ein Beispiel für eine Parameterservervariable:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Wird die TensorFlow-High-Level-API [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) verwendet, analysiert TensorFlow die `TF_CONFIG`-Variable, und erstellt TensorFlow die Clusterspezifikation für Sie.

Bei Low-Level-APIs von TensorFlow für das Training müssen Sie die `TF_CONFIG`-Variable analysieren und `tf.train.ClusterSpec` in Ihrem Trainingscode erstellen.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein TensorFlow-Modell trainiert und registriert. Um zu erfahren, wie Sie ein Modell in einem GPU-fähigen Cluster bereitstellen, lesen Sie bitte unseren Artikel zur Implementierung von GPU-Modellen.

> [!div class="nextstepaction"]
> [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md)
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
* [Verteiltes Trainieren von Deep Learning-Modellen in Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
