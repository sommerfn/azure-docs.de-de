---
title: Verwenden von MLflow mit Azure Machine Learning Service
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie Metriken und Artefakte über die MLflow-Bibliothek für Azure Machine Learning Service protokollieren.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: d0bc4620d0c55d6e94a3d99c39ab405dab2743e5
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461652"
---
# <a name="use-mlflow-with-azure-machine-learning-service-preview"></a>Verwenden von MLflow mit Azure Machine Learning Service (Vorschauversion)

In diesem Artikel wird veranschaulicht, wie Sie den Tracking-URI und die Protokollierungs-API von MLflow (als „MLflow-Tracking“ bezeichnet) mit Azure Machine Learning Service nutzen, um Ihre Experimentmetriken und -artefakte in Ihrem [Azure Machine Learning Service-Arbeitsbereich](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) nachzuverfolgen und zu protokollieren. Falls Sie MLflow-Tracking bereits für Ihre Experimente verwenden, ist der Arbeitsbereich ein zentraler, sicherer und skalierbarer Ort zum Speichern Ihrer Trainingsmetriken und -modelle.

[MLflow](https://www.mlflow.org) ist eine Open-Source-Bibliothek zum Verwalten des Lebenszyklus Ihrer Machine Learning-Experimente. [MLFlow Tracking](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api) ist eine Komponente von MLflow, mit der Ihre Metriken und Modellartefakte von Trainingsausführungen protokolliert und nachverfolgt werden können – unabhängig davon, ob Ihre Experimente lokal, auf einem virtuellen Computer oder in einem Remotecomputecluster ausgeführt werden.
![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/mlflow-diagram.png)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Vergleich von MLflow mit Azure Machine Learning-Clients

 In der Tabelle unten sind die unterschiedlichen Clients, für die Azure Machine Learning Service genutzt werden kann, und die entsprechenden Funktionen zusammengefasst.

 MLflow-Tracking verfügt über Funktionen für Metrikprotokollierung und Artefaktspeicherung, die sonst nur über das [Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) zur Verfügung stehen.

| | MLflow-Tracking | Azure Machine Learning <br> Python SDK |  Azure Machine Learning <br> Befehlszeilenschnittstelle (CLI) | Azure-Portal|
|-|-|-|-|-|
| Verwalten des Arbeitsbereichs |   | ✓ |  ✓ | ✓  |
| Verwenden von Datenspeichern  |   | ✓ |  ✓ |    |
| Protokollieren von Metriken      | ✓ | ✓ |    |    |
| Hochladen von Artefakten | ✓ | ✓ |    |    |
| Anzeigen von Metriken     | ✓ | ✓ | ✓  | ✓ |
| Verwalten von Compute   |   | ✓ | ✓  | ✓ |
| Bereitstellen von Modellen    |   | ✓ |   ✓ | ✓ |

## <a name="prerequisites"></a>Voraussetzungen

* [Installieren Sie MLflow](https://mlflow.org/docs/latest/quickstart.html).
* [Installieren Sie das Azure Machine Learning-SDK für Python auf Ihrem lokalen Computer, und erstellen Sie einen Azure Machine Learning-Arbeitsbereich](setup-create-workspace.md#sdk). Das SDK stellt die Konnektivität für MLflow für den Zugriff auf Ihren Arbeitsbereich bereit.

## <a name="track-local-runs"></a>Nachverfolgen von lokalen Ausführungen

Installieren Sie das Paket `azureml-contrib-run`, um MLflow-Tracking mit Azure Machine Learning für Ihre Experimente zu nutzen, die lokal in einem Jupyter Notebook oder Code-Editor ausgeführt werden.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>Der Namespace „azureml.contrib“ ändert sich häufig, während wir an der Verbesserung des Diensts arbeiten. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.

Importieren Sie die Klassen `mlflow` und [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py), um auf den Tracking-URI von MLflow zuzugreifen und Ihren Arbeitsbereich zu konfigurieren.

Im folgenden Code wird mit der `get_mlflow_tracking_uri()`-Methode dem Arbeitsbereich `ws` eine eindeutige Tracking-URI-Adresse zugewiesen, und mit `set_tracking_uri()` wird für den Tracking-URI von MLflow auf diese Adresse verwiesen.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>Der Tracking-URI ist maximal eine Stunde lang gültig. Wenn Sie Ihr Skript nach einer Leerlaufzeit neu starten, sollten Sie die get_mlflow_tracking_uri-API verwenden, um einen neuen URI abzurufen.

Legen Sie den Namen des MLflow-Experiments mit `set_experiment()` fest, und starten Sie Ihre Trainingsausführung mit `start_run()`. Verwenden Sie anschließend `log_metric()`, um die API für die MLflow-Protokollierung zu aktivieren, und beginnen Sie mit dem Protokollieren Ihrer Metriken für die Trainingsausführungen.

```Python
experiment_name = "experiment_with_mlflow"
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Nachverfolgen von Remoteausführungen

Bei Remoteausführungen können Sie für das Trainieren Ihrer Modelle leistungsfähigere Computeumgebungen, z. B. GPU-fähige virtuelle Computer, oder Machine Learning Compute-Cluster verwenden. Informationen zu den verschiedenen Computeoptionen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

Konfigurieren Sie Ihre Umgebung für Computevorgänge und Trainingsausführungen mit der [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)-Klasse. Fügen Sie die PIP-Pakete `mlflow` und `azure-contrib-run` in den Abschnitt [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) der Umgebung ein. Erstellen Sie anschließend [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) mit Ihrer Remotecompute-Umgebung als Computeziel.

```Python

from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = "my_workspace",
                 name = "my_experiment")

mlflow_env = Environment(name="mlflow-env")

cd = CondaDependencies.create(pip_packages = ["mlflow", "azureml-contrib-run"])

mlflow_env.python.conda_dependencies=cd

src = ScriptRunConfig(source_directory="./my_script_location", script="my_training_script.py")

src.run_config.target = "my-remote-compute-compute"
src.run_config.environment = mlflow_env
```

Importieren Sie in Ihrem Trainingsskript `mlflow`, um die APIs für die MLflow-Protokollierung zu verwenden und mit dem Protokollieren Ihrer Ausführungsmetriken zu beginnen.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric("example", 1.23)
```

Verwenden Sie bei dieser Konfiguration für Computevorgänge und Trainingsausführungen die `Experiment.submit("train.py")`-Methode, um eine Ausführung zu übermitteln. Hiermit wird der MLflow Tracking-URI automatisch festgelegt und die Protokollierung von MLflow an Ihren Arbeitsbereich geleitet.

```Python
run = exp.submit(src)
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Anzeigen von Metriken und Artefakten in Ihrem Arbeitsbereich

Die Metriken und Artefakte aus der MLflow-Protokollierung werden im [Azure-Portal](https://portal.azure.com) in Ihrem Arbeitsbereich vorgehalten. Sie können sie jederzeit anzeigen, indem Sie zu Ihrem Arbeitsbereich navigieren und nach dem Namen des Experiments suchen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beachten Sie Folgendes, falls Sie nicht planen, die protokollierten Metriken und Artefakte in Ihrem Arbeitsbereich zu verwenden: Das Löschen einzelner Einträge ist derzeit nicht möglich. Löschen Sie stattdessen die Ressourcengruppe, die das Speicherkonto und den Arbeitsbereich enthält, damit hierfür keine Gebühren anfallen:

1. Wählen Sie ganz links im Azure-Portal **Ressourcengruppen** aus.

   ![Löschen im Azure-Portal](media/how-to-use-mlflow/delete-resources.png)

1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie den Ressourcengruppennamen ein. Wählen Sie anschließend die Option **Löschen**.

## <a name="example-notebooks"></a>Beispielnotebooks

Auf der Seite zum Thema [MLflow mit Azure ML-Notebooks](https://aka.ms/azureml-mlflow-examples) werden die Konzepte dieses Artikels veranschaulicht.

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen eines Modells](how-to-deploy-and-where.md)
