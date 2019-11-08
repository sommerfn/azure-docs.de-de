---
title: Automatisierte ML-Remotecomputeziele
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Modelle mit automatisiertem maschinellem Lernen auf einem Azure Machine Learning-Remotecomputeziel mit Azure Machine Learning erstellen.
services: machine-learning
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 4276a713e62f96cc5340fc7be0e8391939d32342
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497318"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In Azure Machine Learning trainieren Sie Ihr Modell mit verschiedenen, von Ihnen verwalteten Arten von Computeressourcen. Beim Computeziel kann es sich um einen lokalen Computer oder eine Ressource in der Cloud handeln.

Sie können Ihr Machine Learning-Experiment bequem durch Hinzufügen zusätzlicher Computeziele wie Azure Machine Learning Compute (AmlCompute) vertikal oder horizontal hochskalieren. AmlCompute ist eine verwaltete Compute-Infrastruktur, mit der Sie ganz einfach ein Computeziel mit einem oder mehreren Knoten erstellen können.

In diesem Artikel erfahren Sie, wie Sie ein Modell mit automatisiertem Machine Learning mit AmlCompute erstellen.

## <a name="how-does-remote-differ-from-local"></a>Remote und lokal – wo liegen die Unterschiede?

Im Tutorial [Train a classification model with automated machine learning (Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellen Lernen)](tutorial-auto-train-models.md) erfahren Sie, wie Sie einen lokalen Computer zum Trainieren von Modellen mit automatisiertem maschinellen Lernen verwenden. Der Workflow zum lokalen Trainieren trifft ebenfalls auf Remoteziele zu. Bei Remotecomputezielen werden jedoch die Experimentiterationen mit automatisiertem maschinellem Lernen asynchron ausgeführt. Diese Funktion ermöglicht es, eine bestimmte Iteration abzubrechen, den Ausführungsstatus anzuzeigen oder im Jupyter Notebook an anderen Zellen weiterzuarbeiten. Damit Sie Ihr Modell remote trainieren können, müssen Sie zunächst ein Remotecomputeziel wie AmlCompute erstellen. Konfigurieren Sie dann die Remoteressource, und übermitteln Sie dort Ihren Code.

Dieser Artikel zeigt die weiteren erforderlichen Schritte, um ein Experiment mit automatisiertem Machine Learning auf einem AmlCompute-Remoteziel auszuführen. Das Arbeitsbereichsobjekt `ws` aus dem Tutorial wird hier im gesamten Code verwendet.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Erstellen von Ressourcen

Erstellen Sie das AmlCompute-Ziel in Ihrem Arbeitsbereich (`ws`), wenn es noch nicht vorhanden ist.

**Geschätzter Zeitaufwand**: Ein Erstellen des AmlCompute-Ziels dauert etwa fünf Minuten.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

amlcompute_cluster_name = "automlcl"  # Name your cluster
provisioning_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                            # for GPU, use "STANDARD_NC6"
                                                            # vm_priority = 'lowpriority', # optional
                                                            max_nodes=6)
compute_target = ComputeTarget.create(
    ws, amlcompute_cluster_name, provisioning_config)

# Can poll for a minimum number of nodes and for a specific timeout.
# If no min_node_count is provided, it will use the scale settings for the cluster.
compute_target.wait_for_completion(
    show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Sie können das `compute_target`-Objekt jetzt als Remotecomputeziel verwenden.

Zu den Einschränkungen für einen Clusternamen gehören:
+ Muss kürzer sein als 64 Zeichen.
+ Darf keines der folgenden Zeichen enthalten: `\` ~! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

## <a name="access-data-using-tabulardataset-function"></a>Zugreifen auf Daten mit der TabularDataset-Funktion

Definiert X und Y als `TabularDataset`s, die in der AutoMLConfig-Datei an Automated ML übergeben werden. `from_delimited_files` legt `infer_column_types` standardmäßig auf „true“ fest, wodurch der Spaltentypen automatisch abgeleitet wird. 

Wenn Sie die Spaltentypen manuell festlegen möchten, können Sie das `set_column_types`-Argument so festlegen, dass der Typ der einzelnen Spalten manuell festgelegt wird. Im folgenden Codebeispiel stammen die Daten aus dem sklearn-Paket.

```python
# Create a project_folder if it doesn't exist
if not os.path.isdir('data'):
    os.mkdir('data')
    
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

from sklearn import datasets
from azureml.core.dataset import Dataset
from scipy import sparse
import numpy as np
import pandas as pd

data_train = datasets.load_digits()

pd.DataFrame(data_train.data[100:,:]).to_csv("data/X_train.csv", index=False)
pd.DataFrame(data_train.target[100:]).to_csv("data/y_train.csv", index=False)

ds = ws.get_default_datastore()
ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)

X = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/X_train.csv'))
y = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/y_train.csv'))

```

## <a name="create-run-configuration"></a>Erstellen einer Laufzeitkonfiguration

Definieren Sie ein `RunConfiguration`-Objekt mit einem definierten `CondaDependencies`-Element, um Abhängigkeiten für das Skript get_data.py verfügbar zu machen. Verwenden Sie dieses Objekt für den `run_configuration`-Parameter in `AutoMLConfig`.

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config = RunConfiguration(framework="python")
run_config.target = compute_target
run_config.environment.docker.enabled = True
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

dependencies = CondaDependencies.create(
    pip_packages=["scikit-learn", "scipy", "numpy"])
run_config.environment.python.conda_dependencies = dependencies
```

Ein weiteres Beispiel für dieses Entwurfsmuster finden Sie in [diesem Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb).

## <a name="configure-experiment"></a>Konfigurieren des Experiments
Legen Sie die Einstellungen für `AutoMLConfig` fest.  (Siehe dazu eine [vollständige Liste der Parameter](how-to-configure-auto-train.md#configure-experiment) und ihrer möglichen Werte.)

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             X = X,
                             y = y,
                             **automl_settings,
                             )
```

### <a name="enable-model-explanations"></a>Aktivieren von Modellerklärungen

Legen Sie den optionalen `model_explainability`-Parameter im `AutoMLConfig` Konstruktor fest. Darüber hinaus muss ein Dataframe-Überprüfungsobjekt als Parameter `X_valid` übergeben werden, um das Modellerklärungsfeature verwenden zu können.

```python
automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             X = X,
                             y = y,
                             **automl_settings,
                             model_explainability=True,
                             X_valid=X_test
                             )
```

## <a name="submit-training-experiment"></a>Übermitteln des Trainingsexperiments

Übermitteln Sie nun die Konfiguration, um automatisch den Algorithmus und die Hyperparameter festzulegen und das Modell zu trainieren.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Es wird eine Ausgabe angezeigt, die in etwa wie im folgenden Beispiel aussieht:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************

     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Überprüfen der Ergebnisse

Sie können das gleiche [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) wie in diesem [Trainings-Tutorial](tutorial-auto-train-models.md#explore-the-results) verwenden, um ein Diagramm und eine Tabelle mit Ergebnissen anzuzeigen.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```

Hier ist ein statisches Bild des Widgets.  Klicken Sie im Notebook auf eine beliebige Zeile in der Tabelle, um die Ausführungseigenschaften und Ausgabeprotokolle für diese Ausführung anzuzeigen.   Sie können auch die Dropdownliste über dem Diagramm verwenden, um ein Diagramm der einzelnen verfügbaren Metriken für jede Iteration anzuzeigen.

![Widget-Tabelle](./media/how-to-auto-train-remote/table.png)
![Widget-Plot](./media/how-to-auto-train-remote/plot.png)

Das Widget zeigt eine URL an, die Sie zum Anzeigen und Überprüfen der Details der einzelnen Ausführungen verwenden können.  

Wenn Sie nicht in einem Jupyter-Notebook arbeiten, können Sie die URL aus der Ausführung selbst anzeigen.

```
remote_run.get_portal_url()
```

Die gleichen Informationen sind in Ihrem Arbeitsbereich verfügbar.  Weitere Informationen zu diesen Ergebnissen finden Sie unter [Grundlegendes zu den Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md).

### <a name="view-logs"></a>Anzeigen von Protokollen

Suchen Sie unter `/tmp/azureml_run/{iterationid}/azureml-logs` nach Protokollen zur DSVM.

## <a name="explain"></a> Erklärung für das beste Modell

Das Abrufen von Modellerklärungsdaten ermöglicht es Ihnen, detaillierte Informationen zu den Modellen anzuzeigen, um die Transparenz der Vorgänge zu erhöhen, die auf dem Back-End ausgeführt werden. In diesem Beispiel führen Sie Modellerklärungen nur für das am besten passende Modell aus. Eine Ausführung für alle Modelle in der Pipeline würde zu erheblichen Ausführungszeiten führen. Eine Modellerklärung enthält die folgenden Informationen:

* shap_values: Die von der shap-Bibliothek generierten Erklärungsinformationen.
* expected_values: Der erwartete Wert des Modells, der auf den Satz von X_train-Daten angewendet wird.
* overall_summary: Die Featuregewichtungswerte auf Modellebene, sortiert in absteigender Reihenfolge.
* overall_imp: Die Featurenamen, sortiert in derselben Reihenfolge wie in overall_summary.
* per_class_summary: Die Featuregewichtungswerte auf Klassenebene, sortiert in absteigender Reihenfolge. Nur für den Klassifizierungsfall verfügbar.
* per_class_imp: Die Featurenamen, sortiert in der gleichen Reihenfolge wie in per_class_summary. Nur für den Klassifizierungsfall verfügbar.

Verwenden Sie den folgenden Code, um die beste Pipeline aus Ihren Iterationen auszuwählen. Die `get_output`-Methode gibt die beste Ausführung und das angepasste Modell für den letzten passenden Aufruf zurück.

```python
best_run, fitted_model = remote_run.get_output()
```

Importieren Sie die `retrieve_model_explanation`-Funktion, und führen Sie sie für das beste Modell aus.

```python
from azureml.train.automl.automlexplainer import retrieve_model_explanation

shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
    retrieve_model_explanation(best_run)
```

Geben Sie die Ergebnisse für die `best_run`-Erklärungsvariablen aus, die Sie anzeigen möchten.

```python
print(overall_summary)
print(overall_imp)
print(per_class_summary)
print(per_class_imp)
```

Das Ausgeben der `best_run`-Erklärungszusammenfassungsvariablen führt zur folgenden Ausgabe.

![Konsolenausgabe der Modellerklärung](./media/how-to-auto-train-remote/expl-print.png)

Sie können die Featurerelevanz auch über die Widget-Benutzeroberfläche oder in Ihrem Arbeitsbereich in [Azure Machine Learning-Studio](https://ml.azure.com) visualisieren. 

![Benutzeroberfläche der Modellerklärung](./media/how-to-auto-train-remote/model-exp.png)

## <a name="example"></a>Beispiel

Das Notebook [how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) veranschaulicht die Konzepte in diesem Artikel.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, [wie Sie die Einstellungen für das automatische Training konfigurieren](how-to-configure-auto-train.md).
