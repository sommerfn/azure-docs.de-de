---
title: Trainieren von Scikit-learn-Machine Learning-Modellen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Ihre Scikit-learn-Trainingsskripts auf Unternehmensebene mit der SKlearn-Estimator-Klasse von Azure Machine Learning ausführen. Das Beispielskript klassifiziert Irisbilder zum Erstellen eines Machine Learning-Modells auf der Grundlage des Iris-Datasets von Scikit-learn.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 08/02/2019
ms.custom: seodec18
ms.openlocfilehash: ea466486509c4b5dadc48ef830c9f05ec42ab5b3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73814862"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning"></a>Erstellen von Scikit-learn-Modellen nach Maß mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Ihre Scikit-learn-Trainingsskripts auf Unternehmensebene mit der [SKlearn-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)-Klasse von Azure Machine Learning ausführen. 

Mit den Beispielskripts in diesem Artikel werden Irisbilder zum Erstellen eines Machine Learning-Modells auf der Grundlage des [Iris-Datasets](https://archive.ics.uci.edu/ml/datasets/iris) von Scikit-learn klassifiziert.

Unabhängig davon, ob Sie ein Scikit-learn-Machine Learning-Modell von Grund auf trainieren oder ob Sie ein vorhandenes Modell in die Cloud verschieben, können Sie Azure Machine Learning zum Erweitern von Open-Source-Trainingsaufträgen mithilfe elastischer Cloudcomputeressourcen verwenden. Sie können produktionsgeeignete Modelle mit Azure Machine Learning erstellen, bereitstellen und überwachen sowie ihre Versionen verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie diesen Code in einer dieser Umgebungen aus:
 - Azure Machine Learning Notebook VM: keine Downloads oder Installationen erforderlich

    - Absolvieren Sie [Tutorial: Einrichten von Umgebung und Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md), um einen dedizierten Notebook-Server zu erstellen, auf dem das SDK und Beispielrepository vorinstalliert sind.
    - Im Beispieltrainingsordner auf dem Notebookserver finden Sie im folgenden Verzeichnis ein fertiges und erweitertes Notebook: **how-to-use-azureml > ml-frameworks > scikit-learn > training > train-hyperparameter-tune-deploy-with-sklearn**.

 - Ihr eigener Jupyter Notebook-Server

    - [Installieren Sie das Azure Machine Learning SDK.](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
    - [Erstellen einer Konfigurationsdatei für den Arbeitsbereich](how-to-configure-environment.md#workspace).
    - Herunterladen des Datasets und der Beispielskriptdatei 
        - [Iris-Dataset](https://archive.ics.uci.edu/ml/datasets/iris)
        - [`train_iris.py`](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn)
    - Auf der GitHub-Seite mit Beispielen finden Sie außerdem eine fertige [Jupyter Notebook-Version](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/scikit-learn/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) dieser Anleitung. Das Notebook enthält einen erweiterten Abschnitt, der intelligente Hyperparameteroptimierung und das Abrufen des besten Modells nach primären Metriken behandelt.

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

### <a name="create-a-machine-learning-experiment"></a>Erstellen eines Machine Learning-Experiments

Erstellen Sie ein Experiment und einen Ordner, in dem Ihre Trainingsskripts gespeichert werden. In diesem Beispiel erstellen Sie ein Experiment mit dem Namen „sklearn-iris“.

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="prepare-training-script"></a>Vorbereiten des Trainingsskripts

In diesem Tutorial wurde das Trainingsskript **train_iris.py** bereits bereitgestellt. In der Praxis sollten Sie benutzerdefinierte Trainingsskripts unverändert übernehmen und mit Azure ML ausführen können, ohne Ihren Code ändern zu müssen.

Wenn Sie die Nachverfolgungs- und Metrikfunktionen von Azure ML nutzen möchten, ergänzen Sie Ihr Trainingsskript mit Azure ML-Code.  Das Trainingsskript **train_iris.py** zeigt, wie Sie mit dem `Run`-Objekt innerhalb des Skripts einige Metriken zu Ihrer Azure ML-Ausführung protokollieren können.

Das bereitgestellte Trainingsskript verwendet Beispieldaten aus der `iris = datasets.load_iris()`-Funktion.  Für Ihre eigenen Daten müssen Sie unter Umständen einige Schritte (etwa [Hochladen von Dataset und Skripts](how-to-train-keras.md#data-upload)) ausführen, um Daten während des Trainings verfügbar zu machen.

Kopieren Sie das Trainingsskript **train_iris.py** in Ihr Projektverzeichnis.

```
import shutil
shutil.copy('./train_iris.py', project_folder)
```

## <a name="create-or-get-a-compute-target"></a>Erstellen oder Abrufen eines Computeziels

Erstellen Sie ein Computeziel, auf dem der Scikit-learn-Auftrag ausgeführt werden soll. Scikit-learn unterstützt nur Einzelknoten-CPU-Computing.

Der folgende Code erstellt eine von Azure Machine Learning verwaltete Computeumgebung (AmlCompute) für Ihre Remotetraining-Computeressource. Die Erstellung von AmlCompute dauert etwa fünf Minuten. Wenn sich eine AmlCompute-Ressource mit diesem Namen bereits in Ihrem Arbeitsbereich befindet, überspringt dieser Code den Erstellungsprozess.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Weitere Informationen zu Computezielen finden Sie im Artikel [Was ist ein Computeziel?](concept-compute-target.md).

## <a name="create-a-scikit-learn-estimator"></a>Erstellen eines Scikit-learn-Estimators

Der [Scikit-learn-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) bietet eine einfache Möglichkeit, einen Scikit-learn-Trainingsauftrag auf einem Computeziel zu starten. Es wird durch die [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)-Klasse implementiert, mit der jedes Einzelknoten-CPU-Training unterstützt werden kann.

Wenn für Ihr Trainingsskript zusätzliche Pip- oder Conda-Pakete ausgeführt werden müssen, können Sie die Pakete auf dem resultierenden Docker-Image installieren, indem Sie die Namen mit dem `pip_packages`-Argument und `conda_packages`-Argument übergeben.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```

## <a name="submit-a-run"></a>Initiieren einer Ausführung

Das [Run-Objekt](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) bildet die Schnittstelle zum Ausführungsverlauf, während der Auftrag ausgeführt wird und nachdem er abgeschlossen wurde.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Die Ausführung durchläuft die folgenden Phasen:

- **Vorbereitung**: Gemäß dem TensorFlow-Estimator wird ein Docker-Image erstellt. Das Image wird in die Containerregistrierung des Arbeitsbereichs hochgeladen und für spätere Ausführungen zwischengespeichert. Darüber hinaus werden Protokolle in den Ausführungsverlauf gestreamt, mit deren Hilfe der Status überwacht werden kann.

- **Skalierung**: Der Cluster versucht ein zentrales Hochskalieren, wenn der Batch KI-Cluster mehr Knoten zur Ausführung benötigt, als derzeit verfügbar sind.

- **Running**: Alle Skripts im Skriptordner werden auf das Computeziel hochgeladen, Datenspeicher werden bereitgestellt oder kopiert, und das „entry_script“ wird ausgeführt. Ausgaben aus „stdout“ und dem Ordner „./logs“ werden in den Ausführungsverlauf gestreamt und können zur Überwachung der Ausführung verwendet werden.

- **Nachbearbeitung**: Der Ordner „./outputs“ der Ausführung wird in den Ausführungsverlauf kopiert.

## <a name="save-and-register-the-model"></a>Speichern und Registrieren des Modells

Sobald Sie das Modell trainiert haben, können Sie es in Ihrem Arbeitsbereich speichern und registrieren. Die Modellregistrierung bietet die Möglichkeit, Ihre Modelle in Ihrem Arbeitsbereich zu speichern und zu versionieren, um die [Modellverwaltung und -bereitstellung](concept-model-management-and-deployment.md) zu vereinfachen.

Fügen Sie Ihrem Trainingsskript train_iris.py den folgenden Code hinzu, um das Modell zu speichern. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Registrieren Sie das Modell mit dem folgenden Code in Ihrem Arbeitsbereich.

```Python
model = run.register_model(model_name='sklearn-iris', model_path='model.joblib')
```

## <a name="next-steps"></a>Nächste Schritte


In diesem Artikel haben Sie ein Keras-Modell in Azure Machine Learning trainiert und registriert. Um zu erfahren, wie Sie ein Modell bereitstellen, fahren Sie mit unserem Artikel zur Modellbereitstellung fort.

> [!div class="nextstepaction"]
> [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md)
* [Erfassen einer Ausführungsmetrik während des Trainings](how-to-track-experiments.md)
* [Optimieren von Hyperparametern](how-to-tune-hyperparameters.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
* [Verteiltes Trainieren von Deep Learning-Modellen in Azure](/azure/architecture/reference-architectures/ai/training-deep-learning)
