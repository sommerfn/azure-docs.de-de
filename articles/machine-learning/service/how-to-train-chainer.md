---
title: Trainieren von Deep Learning-Chainer-Modellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Ihre PyTorch-Trainingsskripts im Unternehmensumfang mit der Chainer-Estimator-Klasse von Azure Machine Learning ausführen.  Das Beispielskript klassifiziert Bilder von handschriftlichen Ziffern, um ein neuronales Deep Learning-Netz zu erzeugen, wobei die Python-Bibliothek von Chainer verwendet wird, die auf numpy ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: sdgilley
ms.date: 08/02/2019
ms.openlocfilehash: bfe44e552618dfbee13e8c85ef424f52d4b7ae5f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73814995"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning"></a>Trainieren und Registrieren von Chainer-Modellen in großem Umfang mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Ihre [Chainer](https://chainer.org/)-Trainingsskripts im Unternehmensumfang mit der [Chainer-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py)-Klasse von Azure Machine Learning ausführen. Das Beispieltrainingsskript in diesem Artikel verwendet das beliebte [MNIST-Dataset](http://yann.lecun.com/exdb/mnist/), um handschriftliche Ziffern mithilfe eines DNN (Deep Neural Network) zu klassifizieren, das mit der Python-Bibliothek von Chainer erstellt wurde, die auf [numpy](https://www.numpy.org/) ausgeführt wird.

Unabhängig davon, ob Sie ein Chainer-Deep Learning-Modell von Grund auf trainieren, oder ob Sie ein vorhandenes Modell in die Cloud bringen, können Sie Azure Machine Learning zum horizontalen Hochskalieren von Open Source-Trainingsaufträgen mithilfe elastischer Cloud-Computeressourcen verwenden. Sie können produktionsgeeignete Modelle mit Azure Machine Learning erstellen, bereitstellen, überwachen sowie die Versionen verwalten. 

Weitere Informationen zu [Deep Learning im Vergleich zum maschinellen Lernen](concept-deep-learning-vs-machine-learning.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie diesen Code in einer dieser Umgebungen aus:

- Azure Machine Learning Notebook VM: keine Downloads oder Installationen erforderlich

    - Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebookserver zu erstellen, auf dem das SDK und Beispielrepository vorinstalliert sind.
    - Im Ordner mit Deep Learning-Beispielen auf dem Notebookserver finden Sie ein vervollständigtes Notebook und Dateien im Ordner **how-to-use-azureml > ml-frameworks > chainer > deployment > train-hyperparameter-tune-deploy-with-chainer**.  Das Notebook umfasst erweiterte Abschnitte, in denen die intelligente Hyperparameteroptimierung, die Modellimplementierung und Notebook-Widgets behandelt werden.

- Ihr eigener Jupyter Notebook-Server

    - [Installieren Sie das Azure Machine Learning SDK.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Erstellen Sie eine Konfigurationsdatei für den Arbeitsbereich.](how-to-configure-environment.md#workspace)
    - Laden Sie die Beispielskriptdatei [chainer_mnist.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py) herunter.
     - Auf der GitHub-Seite mit Beispielen finden Sie außerdem eine vervollständigte [Jupyter Notebook-Version](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) dieser Anleitung. Das Notebook umfasst erweiterte Abschnitte, in denen die intelligente Hyperparameteroptimierung, die Modellimplementierung und Notebook-Widgets behandelt werden.

## <a name="set-up-the-experiment"></a>Einrichten des Experiments

In diesem Abschnitt wird das Trainingsexperiment eingerichtet, indem die erforderlichen Python-Pakete geladen, ein Arbeitsbereich initialisiert, ein Experiment erstellt und die Trainingsdaten und -skripts hochgeladen werden.

### <a name="import-packages"></a>Importieren von Paketen

Importieren Sie zunächst die Python-Bibliothek „azureml.core“, und zeigen Sie die Versionsnummer an.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Initialisieren eines Arbeitsbereichs

Der [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für den Dienst. Er stellt den zentralen Ort für die Arbeit mit allen erstellten Artefakten dar. Im Python SDK können Sie auf die Arbeitsbereichsartefakte zugreifen, indem Sie ein [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py)-Objekt erstellen.

Erstellen Sie ein Arbeitsbereichsobjekt mithilfe der Datei `config.json`, die im [Abschnitt Voraussetzungen](#prerequisites) erstellt wurde:

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Erstellen eines Projektverzeichnisses
Erstellen Sie ein Verzeichnis, das den gesamten erforderlichen Code auf Ihrem lokalen Computer enthält, auf den Sie in der Remoteressource zugreifen müssen. Dazu gehören das Trainingsskript und alle zusätzlichen Dateien, von denen Ihr Trainingsskript abhängt.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Vorbereiten des Trainingsskripts

In diesem Tutorial ist das Trainingsskript **chainer_mnist.py** bereits bereitgestellt. In der Praxis sollten Sie benutzerdefinierte Trainingsskripts unverändert übernehmen und mit Azure ML ausführen können, ohne Ihren Code ändern zu müssen.

Wenn Sie die Nachverfolgungs- und Metrikfunktionen von Azure ML nutzen möchten, ergänzen Sie Ihr Trainingsskript mit Azure ML-Code.  Das Trainingsskript **chainer_mnist.py** zeigt, wie Sie mit dem Objekt `Run` innerhalb des Skripts einige Metriken zu Ihrer Azure ML-Ausführung protokollieren können.

Das bereitgestellte Trainingsskript verwendet Beispieldaten aus der chainer-Funktion `datasets.mnist.get_mnist`.  Für Ihre eigenen Daten müssen Sie unter Umständen einige Schritte (etwa [Hochladen von Dataset und Skripts](how-to-train-keras.md#data-upload)) ausführen, um Daten während des Trainings verfügbar zu machen.

Kopieren Sie das Trainingsskript **chainer_mnist.py** in Ihr Projektverzeichnis.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-a-deep-learning-experiment"></a>Erstellen eines Deep Learning-Experiments

Erstellen Sie ein Experiment. In diesem Beispiel erstellen Sie ein Experiment mit dem Namen „chainer-mnist“.

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Erstellen oder Abrufen eines Computeziels

Sie benötigen zum Trainieren Ihres Modells ein [Computeziel](concept-compute-target.md). In diesem Beispiel verwenden Sie die verwaltete Azure ML-Computeressource (AmlCompute) als Computeressource für Ihr Remotetraining.

**Die Erstellung von AmlCompute dauert etwa fünf Minuten**. Wenn sich bereits eine AmlCompute-Ressource mit diesem Namen in Ihrem Arbeitsbereich befindet, überspringt dieser Code den Erstellungsprozess.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Weitere Informationen zu Computezielen finden Sie im Artikel [Was ist ein Computeziel?](concept-compute-target.md).

## <a name="create-a-chainer-estimator"></a>Erstellen eines Chainer-Estimators

Der [Chainer-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) bietet eine einfache Möglichkeit, Chainer-Trainingsaufträge für Ihr Computeziel zu starten.

Der Chainer-Estimator wird durch die generische [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Klasse implementiert, die zur Unterstützung beliebiger Frameworks verwendet werden kann. Weitere Informationen zum Trainieren von Modellen mit dem generischen Estimator finden Sie unter [Trainieren von Azure Machine Learning-Modellen mit einem Estimator](how-to-train-ml-models.md).

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Initiieren einer Ausführung

Das [Run-Objekt](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) bildet die Schnittstelle zum Ausführungsverlauf, während der Auftrag ausgeführt wird und nachdem er abgeschlossen wurde.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Die Ausführung durchläuft die folgenden Phasen:

- **Vorbereitung**: Gemäß dem Chainer-Estimator wird ein Docker-Image erstellt. Das Image wird in die Containerregistrierung des Arbeitsbereichs hochgeladen und für spätere Ausführungen zwischengespeichert. Darüber hinaus werden Protokolle in den Ausführungsverlauf gestreamt, mit deren Hilfe der Status überwacht werden kann.

- **Skalierung**: Der Cluster versucht ein zentrales Hochskalieren, wenn der Batch KI-Cluster mehr Knoten zur Ausführung benötigt, als derzeit verfügbar sind.

- **Running**: Alle Skripts im Skriptordner werden auf das Computeziel hochgeladen, Datenspeicher werden bereitgestellt oder kopiert, und das „entry_script“ wird ausgeführt. Ausgaben aus „stdout“ und dem Ordner „./logs“ werden in den Ausführungsverlauf gestreamt und können zur Überwachung der Ausführung verwendet werden.

- **Nachbearbeitung**: Der Ordner „./outputs“ der Ausführung wird in den Ausführungsverlauf kopiert.

## <a name="save-and-register-the-model"></a>Speichern und Registrieren des Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich speichern und registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.


Registrieren Sie nach Abschluss des Modelltrainings das Modell mit dem folgenden Code in Ihrem Arbeitsbereich:  

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```

> [!TIP]
> Wird ein Fehler mit der Meldung angezeigt, dass das Modell nicht gefunden wurde, warten Sie einen Moment, und wiederholen Sie dann den Vorgang.  Manchmal dauert es kurz, bis das Modell nach dem Ende der Trainingsausführung im Ausgabeverzeichnis verfügbar ist.

Sie können auch eine lokale Kopie des Modells herunterladen. Dies kann nützlich sein, um zusätzliche Arbeiten zur Modellüberprüfung lokal durchzuführen. Im Trainingsskript `chainer_mnist.py` wird das Modell durch ein saver-Objekt persistent in einem lokalen Ordner (lokal für das Computeziel) gespeichert. Mit dem Run-Objekt können Sie eine Kopie aus dem Datenspeicher herunterladen.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie ein neuronales Deep Learning-Netz mithilfe von Chainer in Azure Machine Learning trainiert und registriert. Um zu erfahren, wie Sie ein Modell implementieren, fahren Sie mit unserem Artikel zur [Modellimplementierung](how-to-deploy-and-where.md) fort.

* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)

* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)

* [Verteiltes Trainieren von Deep Learning-Modellen in Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
