---
title: Ausführen von Batchrückschlüssen für große Datenmengen
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mithilfe von Batchrückschlüssen in Azure Machine Learning asynchron Rückschlüsse für große Datenmengen erhalten. Batchrückschlüsse bieten vorgefertigte Parallelverarbeitungsfunktionen sowie Optimierungen für Fire-and-Forget-Rückschlüsse mit hohem Durchsatz in Big Data-Szenarien.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: trbye, jmartens, larryfr
ms.author: tracych
author: tracych
ms.date: 11/04/2019
ms.custom: Ignite2019
ms.openlocfilehash: 615536fbba38279a23516352c69461c19f9972ed
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796711"
---
# <a name="run-batch-inference-on-large-amounts-of-data-by-using-azure-machine-learning"></a>Ausführen von Batchrückschlüssen für große Datenmengen mithilfe von Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In dieser Anleitung erfahren Sie, wie Sie mithilfe von Azure Machine Learning asynchron und parallel Rückschlüsse für große Datenmengen erhalten. Die hier beschriebene Funktion für Batchrückschlüsse befindet sich in der Public Preview-Phase. Sie ermöglicht die Generierung von Rückschlüssen sowie die Verarbeitung von Daten mit hoher Leistung und hohem Durchsatz. Darüber hinaus bietet sie vorgefertigte asynchrone Funktionen.

Mithilfe von Batchrückschlüssen lassen sich Offlinerückschlüsse für mehrere Terabytes an Produktionsdaten ganz einfach auf umfangreiche Computercluster skalieren, was zu Produktivitätssteigerungen und Kostenoptimierungen führt.

In dieser Anleitung lernen Sie Folgendes:

> * Erstellen einer Remotecomputeressource
> * Schreiben eines benutzerdefinierten Rückschlussskripts
> * Erstellen einer [Machine Learning-Pipeline](concept-ml-pipelines.md), um ein vortrainiertes Bildklassifizierungsmodell basierend auf dem Dataset [MNIST](https://publicdataset.azurewebsites.net/dataDetail/mnist/) zu registrieren 
> * Verwenden des Modells, um Batchrückschlüsse für Beispielbilder aus Ihrem Azure Blob Storage-Konto auszuführen 

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Absolvieren Sie das [Einrichtungstutorial](tutorial-1st-experiment-sdk-setup.md), falls Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich oder über einen virtuellen Notebook-Computer verfügen. 

* Informationen zur Verwaltung Ihrer eigenen Umgebung und Abhängigkeiten finden Sie in der [Schrittanleitung](how-to-configure-environment.md) zum Konfigurieren Ihrer eigenen Umgebung. Führen Sie `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-contrib-pipeline-steps` in Ihrer Umgebung aus, um die erforderlichen Abhängigkeiten herunterzuladen.

## <a name="set-up-machine-learning-resources"></a>Einrichten von Machine Learning-Ressourcen

Die folgenden Aktionen dienen zum Einrichten der Ressourcen, die zum Ausführen einer Batch-Rückschlusspipeline benötigt werden:

- Erstellen Sie einen Datenspeicher, der auf einen Blobcontainer mit Bildern für Rückschlüsse verweist.
- Richten Sie Datenverweise als Ein- und Ausgaben für den Batchrückschluss-Pipelineschritt ein.
- Richten Sie einen Computecluster für die Ausführung des Batchrückschlussschritts ein.

### <a name="create-a-datastore-with-sample-images"></a>Erstellen eines Datenspeichers mit Beispielbildern

Laden Sie den MNIST-Auswertungssatz aus dem öffentlichen Blobcontainer `sampledata` in ein Konto namens `pipelinedata` herunter. Erstellen Sie einen Datenspeicher mit dem Namen `mnist_datastore`, der auf diesen Container verweist. Durch Festlegen des Flags `overwrite` auf `True` im folgenden Aufruf von `register_azure_blob_container` wird jeglicher Datenspeicher überschrieben, der zuvor mit diesem Namen erstellt wurde. 

Sie können diesen Schritt ändern, um auf Ihren Blobcontainer zu verweisen, indem Sie eigene Werte für `datastore_name`, `container_name` und `account_name` angeben.

```python
from azureml.core import Datastore
from azureml.core import Workspace

# Load workspace authorization details from config.json
ws = Workspace.from_config()

mnist_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="mnist_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata",
                      overwrite=True)
```

Geben Sie als Nächstes den Standarddatenspeicher des Arbeitsbereichs als Ausgabedatenspeicher an. Er wird für die Rückschlussausgabe verwendet.

Wenn Sie Ihren Arbeitsbereich erstellen, werden standardmäßig [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) und [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)  an den Arbeitsbereich angefügt. Azure Files ist der Standarddatenspeicher für einen Arbeitsbereich, aber Sie können auch Blob Storage als Datenspeicher verwenden. Weitere Informationen finden Sie unter [Azure-Speicheroptionen](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-inputs-and-outputs"></a>Konfigurieren von Dateneingaben und -ausgaben

Als Nächstes müssen Dateneingaben und -ausgaben konfiguriert werden. Hierzu zählen:

- Das Verzeichnis, das die Eingabebilder enthält
- Das Verzeichnis, in dem das vortrainierte Modell gespeichert ist
- Das Verzeichnis, das die Bezeichnungen enthält
- Das Verzeichnis für die Ausgabe

`Dataset` ist eine Klasse für die Untersuchung, Transformierung und Verwaltung von Daten in Azure Machine Learning. Diese Klasse verfügt über zwei Typen: `TabularDataset` und `FileDataset`. In diesem Beispiel wird `FileDataset` als Eingabe für den Batchrückschluss-Pipelineschritt verwendet. 

> [!NOTE] 
> Die Unterstützung von `FileDataset` in Batchrückschlüssen ist vorerst auf Azure Blob Storage beschränkt. 

In Ihrem benutzerdefinierten Rückschlussskript können Sie auch auf andere Datasets verweisen. So können Sie damit beispielsweise auf Bezeichnungen in Ihrem Skript zugreifen, um Bilder mithilfe von `Dataset.register` und `Dataset.get_by_name` mit Bezeichnungen zu versehen.

Weitere Informationen zu Azure Machine Learning-Datasets finden Sie unter [Erstellen von und Zugreifen auf Datasets (Vorschauversion) in Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-create-register-datasets).

Objekte vom Typ `PipelineData` dienen zur Übertragung von Zwischendaten zwischen Pipelineschritten. In diesem Beispiel wird es für Rückschlussausgaben verwendet.

```python
from azureml.core.dataset import Dataset

mnist_ds_name = 'mnist_sample_data'

path_on_datastore = mnist_blob.path('mnist/')
input_mnist_ds = Dataset.File.from_files(path=path_on_datastore, validate=False)
registered_mnist_ds = input_mnist_ds.register(ws, mnist_ds_name, create_new_version=True)
named_mnist_ds = registered_mnist_ds.as_named_input(mnist_ds_name)

output_dir = PipelineData(name="inferences", 
                          datastore=def_data_store, 
                          output_path_on_compute="mnist/results")
```

### <a name="set-up-a-compute-target"></a>Einrichten von Computezielen

In Azure Machine Learning bezieht sich *Compute* (oder *Computeziel*) auf die Computer oder Cluster, die die Berechnungsschritte in Ihrer Machine Learning-Pipeline durchführen. Führen Sie den folgenden Code aus, um ein CPU-basiertes Ziel vom Typ [AmlCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) zu erstellen:

```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
compute_name = os.environ.get("AML_COMPUTE_CLUSTER_NAME", "cpu-cluster")
compute_min_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("AML_COMPUTE_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("AML_COMPUTE_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(compute_target.get_status().serialize())
```

## <a name="prepare-the-model"></a>Vorbereiten des Modells

[Laden Sie das vortrainierte Bildklassifizierungsmodell herunter](https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz), und extrahieren Sie es im Verzeichnis `models`.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="https://pipelinedata.blob.core.windows.net/mnist-model/mnist-tf.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

Registrieren Sie das Modell anschließend bei Ihrem Arbeitsbereich, damit es für Ihre Remotecomputeressource verfügbar ist.

```python
from azureml.core.model import Model

# Register the downloaded model 
model = Model.register(model_path="models/",
                       model_name="mnist",
                       tags={'pretrained': "mnist"},
                       description="Mnist trained tensorflow model",
                       workspace=ws)
```

## <a name="write-your-inference-script"></a>Schreiben Ihres Rückschlussskripts

>[!Warning]
>Bei dem folgenden Code handelt es sich lediglich um ein Beispiel des [Beispielnotebooks](https://aka.ms/batch-inference-notebooks). Sie müssen ein eigenes Skript für Ihr Szenario erstellen.

Das Skript *muss zwei Funktionen enthalten*:
- `init()`: Verwenden Sie diese Funktion für alle aufwendigen oder allgemeinen Vorbereitungsmaßnahmen für den späteren Rückschluss. Ein Beispiel wäre etwa das Laden des Modells in ein globales Objekt.
-  `run(mini_batch)`: Diese Funktion wird für jede Instanz vom Typ `mini_batch` ausgeführt.
    -  `mini_batch`: Der Batchrückschluss ruft die Run-Methode auf und übergibt entweder eine Liste oder einen Pandas-Datenrahmen als Argument an die Methode. Jeder Eintrag in „min_batch“ ist entweder ein Dateipfad (bei Eingaben vom Typ „FileDataset“) oder ein Pandas-Datenrahmen (bei Eingaben vom Typ „TabularDataset“).
    -  `response`: Die Run-Methode muss einen Pandas-Datenrahmen oder ein Array zurückgeben. Bei Verwendung von „append_row output_action“ werden diese zurückgegebenen Elemente am Ende der allgemeinen Ausgabedatei hinzugefügt. Bei Verwendung von „summary_only“ wird der Inhalt der Elemente ignoriert. Bei allen Ausgabeaktionen geben die zurückgegebenen Ausgabeelemente jeweils einen erfolgreichen Rückschluss für ein Eingabeelement aus dem Eingabe-Minibatch an. Der Benutzer muss sicherstellen, dass das Rückschlussergebnis genügend Daten für eine Zuordnung zwischen Eingabe und Rückschluss enthält. Die Rückschlussausgabe wird in die Ausgabedatei geschrieben. Da hierbei nicht unbedingt die Reihenfolge eingehalten wird, muss der Benutzer einen Schlüssel in der Ausgabe verwenden, um sie der Eingabe zuzuordnen.

```python
# Snippets from a sample script.
# Refer to the accompanying digit_identification.py
# (https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/digit_identification.py)
# for the implementation script.

import os
import numpy as np
import tensorflow as tf
from PIL import Image
from azureml.core import Model


def init():
    global g_tf_sess

    # Pull down the model from the workspace
    model_path = Model.get_model_path("mnist")

    # Construct a graph to execute
    tf.reset_default_graph()
    saver = tf.train.import_meta_graph(os.path.join(model_path, 'mnist-tf.model.meta'))
    g_tf_sess = tf.Session()
    saver.restore(g_tf_sess, os.path.join(model_path, 'mnist-tf.model'))


def run(mini_batch):
    print(f'run method start: {__file__}, run({mini_batch})')
    resultList = []
    in_tensor = g_tf_sess.graph.get_tensor_by_name("network/X:0")
    output = g_tf_sess.graph.get_tensor_by_name("network/output/MatMul:0")

    for image in mini_batch:
        # Prepare each image
        data = Image.open(image)
        np_im = np.array(data).reshape((1, 784))
        # Perform inference
        inference_result = output.eval(feed_dict={in_tensor: np_im}, session=g_tf_sess)
        # Find the best probability, and add it to the result list
        best_result = np.argmax(inference_result)
        resultList.append("{}: {}".format(os.path.basename(image), best_result))

    return resultList
```

## <a name="build-and-run-the-batch-inference-pipeline"></a>Erstellen und Ausführen der Batch-Rückschlusspipeline

Sie verfügen nun über alles, was Sie zum Erstellen der Pipeline benötigen.

### <a name="prepare-the-run-environment"></a>Vorbereiten der Ausführungsumgebung

Geben Sie zunächst die Abhängigkeiten für Ihr Skript an. Dieses Objekt wird später bei der Erstellung des Pipelineschritts verwendet.

```python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

batch_conda_deps = CondaDependencies.create(pip_packages=["tensorflow==1.13.1", "pillow"])

batch_env = Environment(name="batch_environment")
batch_env.python.conda_dependencies = batch_conda_deps
batch_env.docker.enabled = True
batch_env.docker.base_image = DEFAULT_CPU_IMAGE
batch_env.spark.precache_packages = False
```

### <a name="specify-the-parameters-for-your-batch-inference-pipeline-step"></a>Angeben der Parameter für Ihren Batchrückschluss-Pipelineschritt

`ParallelRunConfig` ist die Hauptkonfiguration für die neu eingeführte Batchrückschlussinstanz vom Typ `ParallelRunStep` innerhalb der Azure Machine Learning-Pipeline. Sie wird verwendet, um Ihr Skript zu umschließen und die erforderlichen Parameter zu konfigurieren:
- `entry_script`: Ein Benutzerskript als lokaler Dateipfad, das parallel auf mehreren Knoten ausgeführt wird. Ist `source_directly` vorhanden, verwenden Sie einen relativen Pfad. Verwenden Sie andernfalls einen beliebigen Pfad, auf den auf dem Computer zugegriffen werden kann.
- `mini_batch_size`: Die Größe des Minibatchs, der an einen einzelnen Aufruf von `run()` übergeben wird (optional; Standardwert: `1`).
    - Bei `FileDataset` handelt es sich um die Anzahl von Dateien (Mindestwert: `1`). Mehrere Dateien können zu einem Minibatch zusammengefasst werden.
    - Bei `TabularDataset` handelt es sich um die Datengröße. Beispielwerte: `1024`, `1024KB`, `10MB`, `1GB`. Empfohlener Wert: `1MB`. Beachten Sie, dass der Minibatch auf der Grundlage von `TabularDataset` nie über Dateigrenzen hinausgeht. Ein Beispiel: Angenommen, Sie verfügen über unterschiedlich große CSV-Dateien. Die kleinste Datei hat eine Größe von 100 KB, die größte Datei ist 10 MB groß. Wenn Sie nun `mini_batch_size = 1MB` festlegen, werden Dateien mit einer Größe von weniger als 1 MB als einzelner Minibatch behandelt. Dateien mit einer Größe von mehr als 1 MB werden dagegen in mehrere Minibatches aufgeteilt.
- `error_threshold`: Die Anzahl von Datensatzfehlern für `TabularDataset` bzw. Dateifehlern für `FileDataset`, die während der Verarbeitung ignoriert werden sollen. Übersteigt die Fehleranzahl für die gesamte Eingabe diesen Wert, wird der Auftrag beendet. Der Fehlerschwellenwert gilt für die gesamte Eingabe und nicht für einzelne Minibatches, die an die Methode `run()` gesendet werden. Zulässiger Bereich: `[-1, int.max]`. Der Teil `-1` gibt an, dass bei der Verarbeitung alle Fehler ignoriert werden sollen.
- `output_action`: Gibt an, wie die Ausgabe strukturiert werden soll:
    - `summary_only`: Das Benutzerskript speichert die Ausgabe. `ParallelRunStep` verwendet die Ausgabe nur zur Berechnung des Fehlerschwellenwerts.
    - `append_row`: Für alle Eingabedateien wird lediglich eine einzelne Datei im Ausgabeordner erstellt, in der alle Ausgaben angefügt werden (getrennt durch eine Zeile). Der Dateiname lautet „parallel_run_step.txt“.
- `source_directory`: Pfade zu Ordnern mit allen Dateien, die am Computeziel ausgeführt werden sollen (optional).
- `compute_target`: Nur `AmlCompute` wird unterstützt.
- `node_count`: Die Anzahl von Computeknoten, die zum Ausführen des Benutzerskripts verwendet werden sollen.
- `process_count_per_node`: Die Anzahl von Prozessen pro Knoten.
- `environment`: Die Python-Umgebungsdefinition. Sie kann für die Verwendung einer vorhandenen Python-Umgebung oder für die Einrichtung einer temporären Umgebung für das Experiment konfiguriert werden. Die Definition kann auch zum Festlegen der erforderlichen Anwendungsabhängigkeiten verwendet werden (optional).
- `logging_level`: Die Ausführlichkeit des Protokolls. Mögliche Werte (mit zunehmender Ausführlichkeit): `WARNING`, `INFO`, `DEBUG`. Standardwert: `INFO` (optional).
- `run_invocation_timeout`: Das Timeout für den Aufruf der Methode `run()` in Sekunden. Standardwert: `60`.

```python
from azureml.contrib.pipeline.steps import ParallelRunConfig

parallel_run_config = ParallelRunConfig(
    source_directory=scripts_folder,
    entry_script="digit_identification.py",
    mini_batch_size="5",
    error_threshold=10,
    output_action="append_row",
    environment=batch_env,
    compute_target=compute_target,
    node_count=4)
```

### <a name="create-the-pipeline-step"></a>Erstellen des Pipelineschritts

Erstellen Sie den Pipelineschritt mithilfe des Skripts, der Umgebungskonfiguration und der Parameter. Geben Sie das Computeziel an, das Sie bereits als Ausführungsziel für das Skript an Ihren Arbeitsbereich angefügt haben. Verwenden Sie `ParallelRunStep`, um den Batchrückschluss-Pipelineschritt mit folgenden Parametern zu erstellen:
- `name`: Der Name des Schritts. Benennungseinschränkungen: eindeutig, 3–32 Zeichen und regulärer Ausdruck ^\[a-z\]([-a-z0-9]*[a-z0-9])?$.
- `models`: Null oder mehr Modellnamen, die bereits in der Azure Machine Learning-Modellregistrierung registriert sind.
- `parallel_run_config`: Ein Objekt vom Typ `ParallelRunConfig`, wie zuvor definiert.
- `inputs`: Mindestens ein Azure Machine Learning-Datasets eines einzelnen Typs.
- `output`: Ein Objekt vom Typ `PipelineData`, das dem Ausgabeverzeichnis entspricht.
- `arguments`: Eine Liste von Argumenten, die an das Benutzerskript übergeben werden (optional).
- `allow_reuse`: Angabe, ob bei dem Schritt vorherige Ergebnisse wiederverwendet werden sollen, wenn er mit den gleichen Einstellungen/Eingaben ausgeführt wird. Ist der Parameter auf `False` festgelegt, wird für diesen Schritt bei der Pipelineausführung immer eine neue Ausführung generiert. (optional; Standardwert: `True`).

```python
from azureml.contrib.pipeline.steps import ParallelRunStep

parallelrun_step = ParallelRunStep(
    name="batch-mnist",
    models=[model],
    parallel_run_config=parallel_run_config,
    inputs=[named_mnist_ds],
    output=output_dir,
    arguments=[],
    allow_reuse=True
)
```

### <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.

Führen Sie die Pipeline nun aus. Erstellen Sie zunächst ein Objekt vom Typ `Pipeline` mit einem Verweis auf Ihren Arbeitsbereich sowie mit dem erstellten Pipelineschritt. Der Parameter `steps` ist ein Array mit Schritten. In diesem Fall ist allerdings nur ein Schritt für die Batchbewertung vorhanden. Wenn Sie Pipelines mit mehreren Schritten erstellen möchten, müssen Sie die Schritte in der gewünschten Reihenfolge in diesem Array platzieren.

Verwenden Sie als Nächstes die Funktion `Experiment.submit()`, um die Pipeline für die Ausführung zu übermitteln.

```python
from azureml.pipeline.core import Pipeline
from azureml.core.experiment import Experiment

pipeline = Pipeline(workspace=ws, steps=[parallelrun_step])
pipeline_run = Experiment(ws, 'digit_identification').submit(pipeline)
```

## <a name="monitor-the-batch-inference-job"></a>Überwachen des Batchrückschlussauftrags

Ein Batchrückschlussauftrag kann sehr lange dauern. In diesem Beispiel wird der Fortschritt mithilfe eines Jupyter-Widgets überwacht. Weitere Möglichkeiten zu Verwaltung des Auftragsfortschritts:

* Azure Machine Learning Studio 
* Konsolenausgabe des Objekts [`PipelineRun`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.run.pipelinerun?view=azure-ml-py)

```python
from azureml.widgets import RunDetails
RunDetails(pipeline_run).show()

pipeline_run.wait_for_completion(show_output=True)
```

## <a name="next-steps"></a>Nächste Schritte

Den gesamten Ablauf dieses Prozesses können Sie sich anhand des [Notebooks für Batchrückschlüsse](https://aka.ms/batch-inference-notebooks) ansehen. 

Informationen zum Debugging und zur Problembehandlung für Pipelines finden Sie in der [Schrittanleitung](how-to-debug-pipelines.md).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

