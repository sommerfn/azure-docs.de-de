---
title: Trainieren mit „azureml-datasets“
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie Datasets beim Training einsetzen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 09/25/2019
ms.openlocfilehash: 9ccc5f5721d1ddc8459918913a4f3ce707766dea
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71316691"
---
# <a name="train-with-datasets-preview-in-azure-machine-learning"></a>Trainieren mit Datasets (Vorschauversion) in Azure Machine Learning

In diesem Artikel lernen Sie die beiden Möglichkeiten kennen, wie Sie [Azure Machine Learning-Datasets](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29?view=azure-ml-py) in Trainingsläufen von Remoteexperimenten nutzen können, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssen.

- Option 1: Wenn Sie über strukturierte Daten verfügen, erstellen Sie ein TabularDataset-Element, und verwenden Sie es direkt in Ihrem Trainingsskript.

- Option 2: Wenn Sie über unstrukturierte Daten verfügen, erstellen Sie ein FileDataset-Element, und binden Sie Dateien für das Training auf einem Remotecomputer ein oder laden Sie diese herunter.

Azure Machine Learning-Datasets bieten eine nahtlose Integration in Azure Machine Learning-Trainingsprodukte wie [ScriptRun](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrun?view=azure-ml-py), [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) und [HyperDrive](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive?view=azure-ml-py).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und für das Training zu nutzen:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning Service-Arbeitsbereich](how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!Note]
> Einige Dataset-Klassen (Vorschauversion) verfügen über Abhängigkeiten vom [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py)-Paket. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="option-1-use-datasets-directly-in-training-scripts"></a>Option 1: Direktes Verwenden von Datasets in Trainingsskripts

In diesem Beispiel erstellen Sie ein [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py)-Element und verwenden es zum Training als direkte Eingabe für das `estimator`-Objekt. 

### <a name="create-a-tabulardataset"></a>Erstellen eines TabularDataset-Elements

Der folgende Code erstellt ein nicht registriertes TabularDataset-Element aus einer Web-URL. Sie können Datasets auch aus lokalen Dateien oder Pfaden in Datenspeichern erstellen. Erfahren Sie mehr über das [Erstellen von Datasets](https://aka.ms/azureml/howto/createdatasets).

```Python
from azureml.core.dataset import Dataset

web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)
```

### <a name="access-the-input-dataset-in-your-training-script"></a>Zugreifen auf das Eingabedataset im Trainingsskript

TabularDataset-Objekte bieten die Möglichkeit, die Daten in einen Pandas- oder Spark-Datenrahmen zu laden, damit Sie mit vertrauten Datenaufbereitungs- und Trainingsbibliotheken arbeiten können. Übergeben Sie ein TabularDataset-Element als Eingabe in Ihrer Trainingskonfiguration, und rufen Sie es anschließend in Ihrem Skript ab, um diese Funktion nutzen zu können.

Greifen Sie dazu über das [`Run`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py)-Objekt in Ihrem Trainingsskript auf das Eingabedataset zu, und verwenden Sie die Methode [`to_pandas_dataframe()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#to-pandas-dataframe--). 

```Python
%%writefile $script_folder/train_titanic.py

from azureml.core import Dataset, Run

run = Run.get_context()
# get the input dataset by name
dataset = run.input_datasets['titanic_ds']
# load the TabularDataset to pandas DataFrame
df = dataset.to_pandas_dataframe()
```

### <a name="configure-the-estimator"></a>Konfigurieren des Estimators

Ein [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Objekt wird verwendet, um die Experimentausführung zu übermitteln. Azure Machine Learning verfügt über vorkonfigurierte Estimators für gängige Frameworks für maschinelles Lernen sowie einen generischen Estimator.

Dieser Code erstellt ein generisches Estimator-Objekt (`est`) das Folgendes festlegt:

* Ein Skriptverzeichnis für Ihre Skripts. Alle Dateien in dieses Verzeichnis werden zur Ausführung in die Clusterknoten hochgeladen.
* Das Trainingsskript *train_titanic.py*
* Das Eingabedataset für das Training: `titanic`
* Das Computeziel für das Experiment
* Die Umgebungsdefinition für das Experiment

```Python
est = Estimator(source_directory=script_folder,
                entry_script='train_titanic.py',
                # pass dataset object as an input with name 'titanic'
                inputs=[titanic_ds.as_named_input('titanic')],
                compute_target=compute_target,
                environment_definition= conda_env)

# Submit the estimator as part of your experiment run
experiment_run = experiment.submit(est)
experiment_run.wait_for_completion(show_output=True)
```

## <a name="option-2--mount-files-to-a-remote-compute-target"></a>Option 2:  Einbinden von Dateien in einem Remotecomputeziel

Wenn Sie Ihre Datendateien auf dem Computeziel für das Training verfügbar machen möchten, nutzen Sie [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py), um Dateien einzubinden oder herunterzuladen, auf die damit verwiesen wird.

Wenn Sie ein Dateisystem einbinden, fügen Sie das Dateisystem an ein Verzeichnis (Bereitstellungspunkt) an, und stellen Sie es auf dem Computeziel zur Verfügung. Die Einbindung erfolgt sofort, da Dateien nur zum Zeitpunkt der Verarbeitung geladen werden. Die Einbindung wird für Linux-basierte Computeressourcen unterstützt und empfohlen, einschließlich Azure Machine Learning Compute, virtuelle Computer und HDInsight. Bei Computeressourcen, die nicht auf Linux basieren, wird nur das Herunterladen unterstützt.  

>[!WARNING]
> Wenn die Datengröße das Speicherlimit des Computeziels überschreitet, tritt beim Herunterladen ein Fehler auf.

### <a name="create-a-filedataset"></a>Erstellen eines FileDataset-Elements

Im folgenden Beispiel wird ein nicht registriertes FileDataset-Element aus Web-URLs erstellt. Erfahren Sie mehr über das [Erstellen von Datasets](https://aka.ms/azureml/howto/createdatasets) aus anderen Quellen.

```Python
from azureml.core.dataset import Dataset

web_paths = [
            'http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz',
            'http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz'
            ]
mnist_ds = Dataset.File.from_files(path = web_paths)
```

### <a name="configure-the-estimator"></a>Konfigurieren des Estimators

Anstatt das Dataset über den `inputs`-Parameter im Estimator zu übergeben, können Sie das Dataset auch über `script_params` übergeben und den Datenpfad (Bereitstellungspunkt) im Trainingsskript über Argumente abrufen. Auf diese Weise können Sie auf Ihre Daten zugreifen und ein vorhandenes Trainingsskript verwenden.

Ein [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py)-Estimatorobjekt wird verwendet, um die Scikit-learn-Experimente zu übermitteln. Weitere Informationen zum Training mit der [SKlearn-Schätzung](how-to-train-scikit-learn.md).

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    # mount the dataset on the remote compute and pass the mounted path as an argument to the training script
    '--data-folder': mnist_ds.as_named_input('mnist').as_mount(),
    '--regularization': 0.5
}

est = SKLearn(source_directory=script_folder,
              script_params=script_params,
              compute_target=compute_target,
              environment_definition=env,
              entry_script='train_mnist.py')

# Run the experiment
run = experiment.submit(est)
run.wait_for_completion(show_output=True)
```

### <a name="retrieve-the-data-in-your-training-script"></a>Abrufen der Daten im Trainingsskript

Nach dem Übermitteln der Ausführung werden Datendateien, auf die das `mnist`-Dataset verweist, auf dem Computeziel eingebunden. Der folgende Code zeigt, wie Sie die Daten in Ihrem Skript abrufen können.

```Python
%%writefile $script_folder/train_mnist.py

import argparse
import os
import numpy as np
import glob

from utils import load_data

# retrieve the 2 arguments configured through script_params in estimator
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = args.data_folder
print('Data folder:', data_folder)

# get the file paths on the compute
X_train_path = glob.glob(os.path.join(data_folder, '**/train-images-idx3-ubyte.gz'), recursive=True)[0]
X_test_path = glob.glob(os.path.join(data_folder, '**/t10k-images-idx3-ubyte.gz'), recursive=True)[0]
y_train_path = glob.glob(os.path.join(data_folder, '**/train-labels-idx1-ubyte.gz'), recursive=True)[0]
y_test = glob.glob(os.path.join(data_folder, '**/t10k-labels-idx1-ubyte.gz'), recursive=True)[0]

# load train and test set into numpy arrays
X_train = load_data(X_train_path, False) / 255.0
X_test = load_data(X_test_path, False) / 255.0
y_train = load_data(y_train_path, True).reshape(-1)
y_test = load_data(y_test, True).reshape(-1)
```

## <a name="notebook-examples"></a>Notebook-Beispiele

Die [Dataset-Notebooks](https://aka.ms/dataset-tutorial) veranschaulichen und erläutern die in diesem Artikel enthaltenen Konzepte. 

## <a name="next-steps"></a>Nächste Schritte

* [Automatisches Trainieren von Machine Learning-Modellen](how-to-auto-train-remote.md) mit TabularDatasets

* [Trainieren von Bildklassifizierungsmodellen](https://aka.ms/filedataset-samplenotebook) mit FileDatasets

