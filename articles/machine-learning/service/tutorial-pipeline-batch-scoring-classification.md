---
title: 'Tutorial: Azure ML-Pipelines für die Batchbewertung'
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie eine ML-Pipeline zum Ausführen einer Batchbewertung für ein Bildklassifizierungsmodell erstellen. Machine Learning-Pipelines optimieren Ihren Workflow für Geschwindigkeit, Portabilität und Wiederverwendung, sodass Sie sich auf Ihr Kenntnisse im maschinellen Lernen konzentrieren können, anstatt sich um Infrastruktur und Automatisierung kümmern zu müssen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 09/05/2019
ms.openlocfilehash: 15a11ba74262ec5a354f0cb3fe22c09167c8d5a6
ms.sourcegitcommit: d15b23e23328ce7502dd3d2846b49fd2d6d8209c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005395"
---
# <a name="use-azure-machine-learning-pipelines-for-batch-scoring"></a>Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung

In diesem Tutorial verwenden Sie Azure Machine Learning-Pipelines, um einen Batchbewertungsauftrag auszuführen. In diesem Beispiel wird das vorab trainierte TensorFlow-Modell [Inception-V3](https://arxiv.org/abs/1512.00567) eines künstlichen neuronalen Netzes (Convolutional Neural Network, CNN) verwendet, um Bilder ohne Bezeichnung zu klassifizieren. Nach dem Erstellen und Veröffentlichen einer Pipeline konfigurieren Sie einen REST-Endpunkt, um die Pipeline von einer beliebigen HTTP-Bibliothek auf einer beliebigen Plattform aus auslösen zu können.

Machine Learning-Pipelines optimieren Ihren Workflow für Geschwindigkeit, Portabilität und Wiederverwendung, sodass Sie sich auf Ihr Kenntnisse im maschinellen Lernen konzentrieren können, anstatt sich um Infrastruktur und Automatisierung kümmern zu müssen. Weitere Informationen zu ML-Pipelines finden Sie [hier](concept-ml-pipelines.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren eines Arbeitsbereichs und Herunterladen von Beispieldaten
> * Erstellen von Datenobjekten zum Abrufen und Ausgeben von Daten
> * Herunterladen, Vorbereiten und Registrieren des Modells in Ihrem Arbeitsbereich
> * Bereitstellen von Computezielen und Erstellen eines Bewertungsskripts
> * Erstellen, Ausführen und Veröffentlichen einer Pipeline
> * Aktivieren eines REST-Endpunkts für die Pipeline

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

## <a name="prerequisites"></a>Voraussetzungen

* Absolvieren Sie [Teil 1 des Einrichtungstutorials](tutorial-1st-experiment-sdk-setup.md), falls Sie noch nicht über einen Azure Machine Learning-Arbeitsbereich oder einen virtuellen Notebook-Computer verfügen.
* Öffnen Sie nach Abschluss des Einrichtungstutorials das Notebook **tutorials/tutorial-pipeline-batch-scoring-classification.ipynb** unter Verwendung des gleichen Notebook-Servers.

Dieses Tutorial ist auch auf [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) verfügbar, falls Sie es in Ihrer eigenen [lokalen Umgebung](how-to-configure-environment.md#local) ausführen möchten. Führen Sie `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-pipeline-steps pandas requests` aus, um die erforderlichen Pakete abzurufen.

## <a name="configure-workspace-and-create-datastore"></a>Konfigurieren eines Arbeitsbereichs und Erstellen eines Datenspeichers

Erstellen Sie ein Arbeitsbereichsobjekt auf der Grundlage des vorhandenen Azure Machine Learning-Arbeitsbereichs. 
+ Ein [Arbeitsbereich](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) ist eine Klasse, die Informationen zu Ihrem Azure-Abonnement und Ihren Azure-Ressourcen akzeptiert. Außerdem erstellt der Arbeitsbereich eine Cloudressource zur Überwachung und Nachverfolgung Ihrer Modellausführungen. 

+ `Workspace.from_config()` liest die Datei **config.json** und lädt die Authentifizierungsdetails in ein Objekt namens `ws`. `ws` wird im restlichen Code in diesem Tutorial verwendet.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

### <a name="create-a-datastore-for-sample-images"></a>Erstellen eines Datenspeichers für Beispielbilder

Rufen Sie aus dem öffentlichen Blobcontainer `sampledata` für das Konto `pipelinedata` das öffentliche ImageNet-Bewertungsdatenbeispiel ab. Durch Aufrufen von `register_azure_blob_container()` werden die Daten im Arbeitsbereich unter dem Namen `images_datastore` verfügbar gemacht. Geben Sie anschließend den Standarddatenspeicher des Arbeitsbereichs als Ausgabedatenspeicher an. Dieser wird für die Bewertung der Ausgabe in der Pipeline verwendet.

```python
from azureml.core.datastore import Datastore

batchscore_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="images_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata", 
                      overwrite=True)

def_data_store = ws.get_default_datastore()
```

## <a name="create-data-objects"></a>Erstellen von Datenobjekten

Bei der Pipelineerstellung werden Objekte vom Typ `DataReference` zum Lesen von Daten aus Arbeitsbereich-Datenspeichern und Objekte vom Typ `PipelineData` zum Übertragen von Zwischendaten zwischen Pipelineschritten verwendet.

> [!Important]
> Dieses Batchbewertungsbeispiel umfasst nur einen einzelnen Schritt. In Anwendungsfällen mit mehreren Schritten enthält der Ablauf jedoch in der Regel Folgendes:
>
> 1. Verwendung von Objekten vom Typ `DataReference` als **Eingaben** (zum Abrufen von Rohdaten), Ausführung von Transformationen sowie anschließende **Ausgabe** eines Objekts vom Typ `PipelineData`.
>
> 2. Verwenden Sie das **Ausgabeobjekt** `PipelineData` aus dem vorherigen Schritt wiederholt als *Eingabeobjekt* für nachfolgende Schritte.

In diesem Szenario werden Objekte vom Typ `DataReference` erstellt, die den Datenspeicherverzeichnissen für Eingabebilder und Klassifizierungsbezeichnungen (y-Testwerte) entsprechen. Außerdem wird ein Objekt vom Typ `PipelineData` für die Ausgabedaten der Batchbewertung erstellt.

```python
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download"
                            )

label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download"                          
                         )

output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

## <a name="download-and-register-the-model"></a>Herunterladen und Registrieren des Modells

Laden Sie das vortrainierte TensorFlow-Modell herunter, um es für die Batchbewertung in der Pipeline zu verwenden. Erstellen Sie zunächst ein lokales Verzeichnis zum Speichern des Modells. Laden Sie es anschließend herunter, und extrahieren Sie es.

```python
import os
import tarfile
import urllib.request

if not os.path.isdir("models"):
    os.mkdir("models")
    
response = urllib.request.urlretrieve("http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz", "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall("models")
```

Registrieren Sie als Nächstes das Modell bei Ihrem Arbeitsbereich, um es im Pipelineprozess problemlos abrufen zu können. In der statischen Funktion `register()` ist der Parameter `model_name` der Schlüssel, den Sie innerhalb des SDK verwenden, um Ihr Modell zu finden.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-remote-compute-target"></a>Erstellen und Anfügen eines Remotecomputeziels

Da ML-Pipelines nicht lokal ausgeführt werden können, müssen sie unter Verwendung von Cloudressourcen ausgeführt werden. Diese werden als Remotecomputeziele bezeichnet. Hierbei handelt es sich um wiederverwendbare virtuelle Compute-Umgebungen, in denen Sie Experimente und ML-Workflows ausführen können. Führen Sie den folgenden Code aus, um ein GPU-fähiges Ziel vom Typ [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) zu erstellen, und fügen Sie es an Ihren Arbeitsbereich an. Weitere Informationen zu Computezielen finden Sie in [diesem Konzeptartikel](https://docs.microsoft.com/azure/machine-learning/service/concept-compute-target).


```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.exceptions import ComputeTargetException
compute_name = "gpu-cluster"

# checks to see if compute target already exists in workspace, else create it
try:
    compute_target = ComputeTarget(workspace=ws, name=compute_name)
except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(vm_size="STANDARD_NC6",
                                                   vm_priority="lowpriority", 
                                                   min_nodes=0, 
                                                   max_nodes=1)

    compute_target = ComputeTarget.create(workspace=ws, name=compute_name, provisioning_configuration=config)
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="write-a-scoring-script"></a>Schreiben eines Bewertungsskripts

Für die Bewertung erstellen Sie ein Batchbewertungsskript namens `batch_scoring.py` und schreiben es in das aktuelle Verzeichnis. Das Skript nimmt Eingabebilder entgegen, wendet das Klassifizierungsmodell an und gibt die Vorhersagen in eine Ergebnisdatei aus.

Das Skript `batch_scoring.py` akzeptiert folgende Parameter. Sie werden im Rahmen des Pipelineschritts übergeben, den Sie später in diesem Tutorial erstellen:

- `--model_name`: Der Name des verwendeten Modells.
- `--label_dir`: Das Verzeichnis mit der Datei `labels.txt`. 
- `--dataset_path`: Das Verzeichnis mit den Eingabebildern.
- `--output_dir`: Das Skript führt das Modell für die Daten aus und gibt eine Ergebnisdatei (`results-label.txt`) in dieses Verzeichnis aus.
- `--batch_size`: Die beim Ausführen des Modells verwendete Batchgröße.

Die Pipelineinfrastruktur verwendet die Klasse `ArgumentParser`, um Parameter an Pipelineschritte zu übergeben. Im folgenden Code erhält beispielsweise das erste Argument (`--model_name`) den Eigenschaftsbezeichner `model_name`. In der Funktion `main()` wird auf diese Eigenschaft mithilfe von `Model.get_model_path(args.model_name)` zugegriffen.


```python
%%writefile batch_scoring.py

import os
import argparse
import datetime
import time
import tensorflow as tf
from math import ceil
import numpy as np
import shutil
from tensorflow.contrib.slim.python.slim.nets import inception_v3
from azureml.core.model import Model

slim = tf.contrib.slim

parser = argparse.ArgumentParser(description="Start a tensorflow model serving")
parser.add_argument('--model_name', dest="model_name", required=True)
parser.add_argument('--label_dir', dest="label_dir", required=True)
parser.add_argument('--dataset_path', dest="dataset_path", required=True)
parser.add_argument('--output_dir', dest="output_dir", required=True)
parser.add_argument('--batch_size', dest="batch_size", type=int, required=True)

args = parser.parse_args()

image_size = 299
num_channel = 3

# create output directory if it does not exist
os.makedirs(args.output_dir, exist_ok=True)


def get_class_label_dict(label_file):
    label = []
    proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
    for l in proto_as_ascii_lines:
        label.append(l.rstrip())
    return label


class DataIterator:
    def __init__(self, data_dir):
        self.file_paths = []
        image_list = os.listdir(data_dir)
        self.file_paths = [data_dir + '/' + file_name.rstrip() for file_name in image_list]
        self.labels = [1 for file_name in self.file_paths]

    @property
    def size(self):
        return len(self.labels)

    def input_pipeline(self, batch_size):
        images_tensor = tf.convert_to_tensor(self.file_paths, dtype=tf.string)
        labels_tensor = tf.convert_to_tensor(self.labels, dtype=tf.int64)
        input_queue = tf.train.slice_input_producer([images_tensor, labels_tensor], shuffle=False)
        labels = input_queue[1]
        images_content = tf.read_file(input_queue[0])

        image_reader = tf.image.decode_jpeg(images_content, channels=num_channel, name="jpeg_reader")
        float_caster = tf.cast(image_reader, tf.float32)
        new_size = tf.constant([image_size, image_size], dtype=tf.int32)
        images = tf.image.resize_images(float_caster, new_size)
        images = tf.divide(tf.subtract(images, [0]), [255])

        image_batch, label_batch = tf.train.batch([images, labels], batch_size=batch_size, capacity=5 * batch_size)
        return image_batch


def main(_):
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)
    count = 0

    # get model from model registry
    model_path = Model.get_model_path(args.model_name)

    with tf.Session() as sess:
        test_images = test_feeder.input_pipeline(batch_size=args.batch_size)
        with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
            input_images = tf.placeholder(tf.float32, [args.batch_size, image_size, image_size, num_channel])
            logits, _ = inception_v3.inception_v3(input_images,
                                                  num_classes=classes_num,
                                                  is_training=False)
            probabilities = tf.argmax(logits, 1)

        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(sess=sess, coord=coord)
        saver = tf.train.Saver()
        saver.restore(sess, model_path)
        out_filename = os.path.join(args.output_dir, "result-labels.txt")
        with open(out_filename, "w") as result_file:
            i = 0
            while count < total_size and not coord.should_stop():
                test_images_batch = sess.run(test_images)
                file_names_batch = test_feeder.file_paths[i * args.batch_size:
                                                          min(test_feeder.size, (i + 1) * args.batch_size)]
                results = sess.run(probabilities, feed_dict={input_images: test_images_batch})
                new_add = min(args.batch_size, total_size - count)
                count += new_add
                i += 1
                for j in range(new_add):
                    result_file.write(os.path.basename(file_names_batch[j]) + ": " + label_dict[results[j]] + "\n")
                result_file.flush()
            coord.request_stop()
            coord.join(threads)

        shutil.copy(out_filename, "./outputs/")

if __name__ == "__main__":
    tf.app.run()

```

> [!TIP]
> Die Pipeline in diesem Tutorial hat nur einen einzelnen Schritt und schreibt die Ausgabe in eine Datei. Bei Pipelines mit mehreren Schritten wird dagegen auch `ArgumentParser` verwendet, um ein Verzeichnis zum Schreiben von Ausgabedaten zu definieren, die in nachfolgenden Schritten als Eingabe verwendet werden. Im [Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb) finden Sie ein Beispiel für die Übergabe von Daten zwischen mehreren Pipelineschritten unter Verwendung des Entwurfsmusters `ArgumentParser`.

## <a name="build-and-run-the-pipeline"></a>Planen und Ausführen der Pipeline

Bevor Sie die Pipeline ausführen können, müssen Sie ein Objekt erstellen, das die Python-Umgebung und die erforderlichen Abhängigkeiten für Ihr Skript (`batch_scoring.py`) definiert. Die wichtigste erforderliche Abhängigkeit ist TensorFlow. Für Hintergrundprozesse des SDK muss allerdings auch `azureml-defaults` installiert werden. Erstellen Sie ein Objekt vom Typ `RunConfiguration` mit den Abhängigkeiten, und geben Sie auch Docker- und Docker-GPU-Unterstützung an.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE
from azureml.core.runconfig import CondaDependencies, RunConfiguration

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.13.1", "azureml-defaults"])

amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="parameterize-the-pipeline"></a>Parametrisieren der Pipeline

Definieren Sie zum Steuern der Batchgröße einen benutzerdefinierten Parameter für die Pipeline. Nachdem die Pipeline veröffentlicht und über einen REST-Endpunkt verfügbar gemacht wurde, werden alle konfigurierten Parameter ebenfalls verfügbar gemacht und können in der JSON-Nutzlast angegeben werden, wenn die Pipeline mit einer HTTP-Anforderung erneut ausgeführt wird.

Erstellen Sie ein Objekt vom Typ `PipelineParameter`, um dieses Verhalten zu ermöglichen, und definieren Sie einen Namen und einen Standardwert.

```python
from azureml.pipeline.core.graph import PipelineParameter
batch_size_param = PipelineParameter(name="param_batch_size", default_value=20)
```

### <a name="create-the-pipeline-step"></a>Erstellen des Pipelineschritts

Ein Pipelineschritt ist ein Objekt, das alles kapselt, was Sie zum Ausführen einer Pipeline benötigen:

* Umgebungs- und Abhängigkeitseinstellungen
* Die Computeressource für die Pipelineausführung
* Eingabe- und Ausgabedaten sowie ggf. benutzerdefinierte Parameter
* Verweis auf ein Skript oder eine SDK-Logik, die während des Schritts ausgeführt werden soll

Es gibt mehrere Klassen, die von der übergeordneten Klasse [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py) erben, um Sie bei der Erstellung eines Schritts unter Verwendung bestimmter Frameworks und Stapel zu unterstützen. In diesem Beispiel wird die Klasse [`PythonScriptStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) verwendet, um Ihre Schrittlogik mithilfe eines benutzerdefinierten Python-Skripts zu definieren. Hinweis: Wenn es sich bei einem Argument für Ihr Skript um eine Eingabe für den Schritt oder um eine Ausgabe des Schritts handelt, muss es **sowohl** im Array `arguments` **als auch** im Parameter `input` bzw. `output` definiert werden. 

Ein Objektverweis im Array `outputs` wird in Szenarien mit mehreren Schritten als **Eingabe** für einen nachfolgenden Pipelineschritt verfügbar.

```python
from azureml.pipeline.steps import PythonScriptStep

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_scoring.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config
)
```

Eine Liste mit allen Klassen für verschiedene Schritttypen finden Sie im [Schrittpaket](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py).

### <a name="run-the-pipeline"></a>Ausführen der Pipeline

Führen Sie nun die Pipeline aus. Erstellen Sie zunächst ein Objekt vom Typ `Pipeline` mit einem Verweis auf Ihren Arbeitsbereich sowie mit dem erstellten Pipelineschritt. Der Parameter `steps` ist ein Array mit Schritten. In diesem Fall ist allerdings nur ein einzelner Schritt für die Batchbewertung vorhanden. Wenn Sie Pipelines mit mehreren Schritten erstellen möchten, müssen Sie die Schritte in der gewünschten Reihenfolge in diesem Array platzieren.

Verwenden Sie als Nächstes die Funktion `Experiment.submit()`, um die Pipeline für die Ausführung zu übermitteln. Geben Sie außerdem den benutzerdefinierten Parameter `param_batch_size` an. Die Funktion `wait_for_completion` gibt während der Pipelineerstellung Protokolle aus, sodass Sie den aktuellen Status anzeigen können.

> [!IMPORTANT]
> Die erste Pipelineausführung dauert etwa **15 Minuten**, da alle Abhängigkeiten heruntergeladen werden müssen. Außerdem wird ein Docker-Image erstellt und die Python-Umgebung bereitgestellt/erstellt. Spätere Ausführungen sind dagegen deutlich schneller, da diese Ressourcen wiederverwendet werden. Die Gesamtlaufzeit hängt jedoch von der Arbeitsauslastung Ihrer Skripts sowie von den Prozessen ab, die in den einzelnen Pipelineschritten ausgeführt werden.

```python
from azureml.core import Experiment
from azureml.pipeline.core import Pipeline

pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_parameters={"param_batch_size": 20})
pipeline_run.wait_for_completion(show_output=True)
```

### <a name="download-and-review-output"></a>Herunterladen und Überprüfen der Ausgabe

Führen Sie den folgenden Code aus, um die durch das Skript `batch_scoring.py` erstellte Ausgabedatei herunterzuladen, und sehen Sie sich anschließend die Bewertungsergebnisse an:

```python
import pandas as pd

step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Dateiname</th>
      <th>Vorhersage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ILSVRC2012_val_00000102.JPEG</td>
      <td>Rhodesian Ridgeback</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ILSVRC2012_val_00000103.JPEG</td>
      <td>Stativ</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ILSVRC2012_val_00000104.JPEG</td>
      <td>Schreibmaschinentastatur</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ILSVRC2012_val_00000105.JPEG</td>
      <td>Silky Terrier</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ILSVRC2012_val_00000106.JPEG</td>
      <td>Windsorknoten</td>
    </tr>
    <tr>
      <td>5</td>
      <td>ILSVRC2012_val_00000107.JPEG</td>
      <td>Weberknecht</td>
    </tr>
    <tr>
      <td>6</td>
      <td>ILSVRC2012_val_00000108.JPEG</td>
      <td>Geige</td>
    </tr>
    <tr>
      <td>7</td>
      <td>ILSVRC2012_val_00000109.JPEG</td>
      <td>Lautsprecher</td>
    </tr>
    <tr>
      <td>8</td>
      <td>ILSVRC2012_val_00000110.JPEG</td>
      <td>Schürze</td>
    </tr>
    <tr>
      <td>9</td>
      <td>ILSVRC2012_val_00000111.JPEG</td>
      <td>Amerikanischer Hummer</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="publish-and-run-from-rest-endpoint"></a>Veröffentlichen und Ausführen über den REST-Endpunkt

Führen Sie den folgenden Code aus, um die Pipeline in Ihrem Arbeitsbereich zu veröffentlichen. In Ihrem Arbeitsbereich im Portal werden Metadaten für die Pipeline angezeigt. Hierzu zählen unter anderem Ausführungsverlauf und -dauer. Über das Portal kann die Pipeline auch manuell ausgeführt werden.

Darüber hinaus kann die veröffentlichte Pipeline durch einen REST-Endpunkt von einer beliebigen HTTP-Bibliothek auf einer beliebigen Plattform aus erneut ausgeführt werden.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

Um die Pipeline über den REST-Endpunkt ausführen zu können, benötigen Sie zunächst einen OAuth2-Bearer-Authentifizierungsheader. In diesem Beispiel wird zur Veranschaulichung eine interaktive Authentifizierung verwendet. In den meisten Produktionsszenarien, die eine automatisierte oder monitorlose Authentifizierung erfordern, wird dagegen eine Dienstprinzipalauthentifizierung verwendet, wie in [diesem Notebook](https://aka.ms/pl-restep-auth) beschrieben.

Für die Dienstprinzipalauthentifizierung muss eine **App-Registrierung** in **Azure Active Directory** erstellt, ein geheimer Clientschlüssel generiert und dem Dienstprinzipal anschließend **Rollenzugriff** auf Ihren Machine Learning-Arbeitsbereich gewährt werden. Zur Verwaltung des Authentifizierungsflusses wird die Klasse [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py) verwendet. 

`InteractiveLoginAuthentication` und `ServicePrincipalAuthentication` erben jeweils von `AbstractAuthentication`, und in beiden Fällen wird die Funktion `get_authentication_header()` auf die gleiche Weise verwendet, um den Header abzurufen.

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

Rufen Sie die REST-URL aus der Eigenschaft `endpoint` des veröffentlichten Pipelineobjekts ab. Die REST-URL finden Sie auch in Ihrem Arbeitsbereich im Portal. Erstellen Sie eine HTTP POST-Anforderung für den Endpunkt, und geben Sie Ihren Authentifizierungsheader an. Fügen Sie außerdem ein JSON-Nutzlastobjekt mit dem Namen des Experiments und dem Parameter für die Batchgröße hinzu. Zur Erinnerung: Der Parameter für die Batchgröße (`param_batch_size`) wird an Ihr Skript (`batch_scoring.py`) übergeben, da Sie ihn in der Schrittkonfiguration als Objekt vom Typ `PipelineParameter` definiert haben.

Übermitteln Sie die Anforderung, um die Ausführung auszulösen. Greifen Sie auf den Schlüssel `Id` aus dem Antwortwörterbuch zu, um den Wert der Ausführungs-ID zu erhalten.

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})
run_id = response.json()["Id"]
```

Verwenden Sie die Ausführungs-ID, um den Status der neuen Ausführung zu überwachen. Die Ausführung dauert ungefähr weitere zehn bis 15 Minuten und gleicht der vorherigen Pipelineausführung. Wenn Sie sich also keine weitere Pipelineausführung ansehen möchten, können Sie die Betrachtung der vollständigen Ausgabe überspringen.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Überspringen Sie diesen Abschnitt, wenn Sie weitere Azure Machine Learning-Tutorials absolvieren möchten.

### <a name="stop-the-notebook-vm"></a>Beenden der Notebook-VM

Sollten Sie einen cloudbasierten Notebook-Server verwendet haben, beenden Sie aus Kostengründen den virtuellen Computer, wenn Sie ihn nicht verwenden.

1. Wählen Sie in Ihrem Arbeitsbereich **Notebook-VMs** aus.
1. Wählen Sie die VM in der Liste aus.
1. Wählen Sie **Stop** (Beenden) aus.
1. Wenn Sie bereit sind, den Server erneut zu verwenden, wählen Sie **Starten** aus.

### <a name="delete-everything"></a>Alles löschen

Wenn Sie die erstellten Ressourcen nicht mehr benötigen, löschen Sie sie, damit Ihnen keine Kosten entstehen.

1. Wählen Sie ganz links im Azure-Portal **Ressourcengruppen** aus.
1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.
1. Wählen Sie die Option **Ressourcengruppe löschen**.
1. Geben Sie den Ressourcengruppennamen ein. Wählen Sie anschließend die Option **Löschen**.

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial zu Machine Learning-Pipelines haben Sie folgende Aufgaben ausgeführt:

> [!div class="checklist"]
> * Erstellen einer Pipeline mit Umgebungsabhängigkeiten, die auf einer Remote-GPU-Computeressource ausgeführt werden kann
> * Erstellen eines Bewertungsskripts zum Ausführen von Batchvorhersagen mit einem vortrainierten TensorFlow-Modell
> * Veröffentlichen einer Pipeline und Ermöglichen der Ausführung über einen REST-Endpunkt

Weitere Beispiele für die Pipelineerstellung mit dem Machine Learning SDK finden Sie im [Notebook-Repository](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines).
