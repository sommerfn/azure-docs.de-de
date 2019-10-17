---
title: Starten, Überwachen und Abbrechen von Trainingsausführungen in Python
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Machine Learning-Experimente starten, deren Status festlegen, sie markieren und sie organisieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2019
ms.openlocfilehash: 7ebbc7575ad52bbf7a399babb048113bc505a7f8
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174535"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Starten, Überwachen und Abbrechen von Trainingsausführungen in Python

Das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) und die [Machine Learning-CLI](reference-azure-machine-learning-cli.md) bieten verschiedene Methoden zum Überwachen, Organisieren und Verwalten Ihrer Ausführungen für Training und Experimentieren.

In diesem Artikel finden Sie Beispiele für die folgenden Aufgaben:

* Überwachen der Ausführungsleistung
* Abbrechen oder Fehler von Ausführungen
* Erstellen untergeordneter Ausführungen
* Markieren und Suchen von Ausführungen

## <a name="prerequisites"></a>Voraussetzungen

Sie benötige folgende Elemente:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

* Das Azure Machine Learning SDK für Python (Version 1.0.21 oder höher). Wie Sie die neueste Version des SDK installieren oder auf diese aktualisieren, erfahren Sie unter [Installieren des Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Um Ihre Version des Azure Machine Learning SDK zu überprüfen, verwenden Sie den folgenden Code:

    ```python
    print(azureml.core.VERSION)
    ```

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) und die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Starten einer Ausführung und deren Protokollierungsprozess

### <a name="using-the-sdk"></a>Verwenden des SDK

Richten Sie Ihr Experiment durch Importieren der Klassen [Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) und [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) aus dem [azureml.core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py)-Paket ein.

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Starten Sie eine Ausführung und ihren Protokollierungsprozess mit der [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-)-Methode.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Verwenden der CLI

Führen Sie die folgenden Schritte aus, um eine Ausführung Ihres Experiments zu starten:

1. Verwenden Sie in einer Shell oder Eingabeaufforderung die Azure CLI, um sich bei Ihrem Azure-Abonnement zu authentifizieren:

    ```azurecli-interactive
    az login
    ```

1. Fügen Sie eine Arbeitsbereichskonfiguration an den Ordner an, der Ihr Trainingsskript enthält. Ersetzen Sie `myworkspace` durch Ihren Azure Machine Learning-Arbeitsbereich. Ersetzen Sie `myresourcegroup` durch die Azure-Ressourcengruppe, die Ihren Arbeitsbereich enthält:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Dieser Befehl erstellt ein `.azureml`-Unterverzeichnis, das RUNCONFIG-Beispieldateien und Conda-Umgebungsdateien enthält. Es enthält auch eine `config.json`-Datei, die für die Kommunikation mit dem Azure Machine Learning-Arbeitsbereich verwendet wird.

    Weitere Informationen finden Sie unter [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Verwenden Sie den folgenden Befehl zum Starten der Ausführung. Wenn Sie diesen Befehl verwenden, geben Sie den Namen der Runconfig-Datei (den Text vor „\*.runconfig“, wenn Sie auf Ihr Dateisystem schauen) für den -c-Parameter an.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Mit dem Befehl `az ml folder attach` wurde ein `.azureml`-Unterverzeichnis erstellt, das zwei RUNCONFIG-Beispieldateien enthält.
    >
    > Wenn Sie ein Python-Skript haben, das programmgesteuert ein Laufzeitkonfigurationsobjekt erstellt, können Sie es mit [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) als RUNCPNFIG-Datei speichern.
    >
    > Weitere RUNCONFIG-Beispieldateien finden Sie unter [https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml](https://github.com/MicrosoftDocs/pipelines-azureml/tree/master/.azureml).

    Weitere Informationen finden Sie unter [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

## <a name="monitor-the-status-of-a-run"></a>Überwachen des Status einer Ausführung

### <a name="using-the-sdk"></a>Verwenden des SDK

Rufen Sie den Status einer Ausführung mit der [ `get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--)-Methode ab.

```python
print(notebook_run.get_status())
```

Rufen Sie die Ausführungs-ID, Ausführungszeit und zusätzliche Details zur Ausführung mit der [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--)-Methode ab.

```python
print(notebook_run.get_details())
```

Wenn die Ausführung erfolgreich abgeschlossen wurde, verwenden Sie die [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-)-Methode, um sie als abgeschlossen zu markieren.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Wenn Sie das Python-Entwurfsmuster `with...as` verwenden, markiert sich die Ausführung automatisch selbst als abgeschlossen, wenn sie außerhalb des Bereichs liegt. Sie müssen die Ausführung nicht manuell als abgeschlossen markieren.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>Verwenden der CLI

1. Verwenden Sie den folgenden Befehl, um eine Liste der Ausführungen für Ihr Experiment anzuzeigen. Ersetzen Sie `experiment` durch den Namen Ihres Experiments:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    Dieser Befehl gibt ein JSON-Dokument zurück, in dem Informationen zu Ausführungen für dieses Experiment aufgelistet sind.

    Weitere Informationen finden Sie unter [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Verwenden Sie den folgenden Befehl, um Informationen zu einer bestimmten Ausführung anzuzeigen. Ersetzen Sie `runid` durch die ID der Ausführung:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    Dieser Befehl gibt ein JSON-Dokument zurück, in dem Informationen zur Ausführung aufgelistet sind.

    Weitere Informationen finden Sie unter [az ml run show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).

## <a name="cancel-or-fail-runs"></a>Abbrechen oder Fehler von Ausführungen

Wenn Sie einen Fehler bemerken oder die Ausführung zu lange dauert, können Sie die Ausführung abbrechen.

### <a name="using-the-sdk"></a>Verwenden des SDK

Verwenden Sie zum Abbrechen einer Ausführung mit dem SDK die [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--)-Methode:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Wenn Ihre Ausführung abgeschlossen ist, aber Fehler enthält (etwa weil ein falsches Trainingsskript verwendet wurde), können Sie sie mit der [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-)-Methode als fehlerhaft markieren.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Verwenden der CLI

Verwenden Sie zum Abbrechen einer Ausführung mit der CLI den folgenden Befehl. Ersetzen Sie `runid` durch die ID der Ausführung.

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Weitere Informationen finden Sie unter [az ml run cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

## <a name="create-child-runs"></a>Erstellen untergeordneter Ausführungen

Erstellen Sie untergeordnete Ausführungen, um verwandte Ausführungen zu gruppieren, wie etwa für unterschiedliche Iterationen zum Optimieren von Hyperparametern.

> [!NOTE]
> Untergeordnete Ausführungen können nur mit dem SDK erstellt werden.

Dieses Codebeispiel verwendet das `hello_with_children.py`-Skript, um einen Batch von fünf untergeordneten Ausführungen aus einer übermittelten Ausführung mithilfe der [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-)-Methode zu erstellen:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Sobald sie den gültigen Bereich verlassen haben, werden untergeordnete Ausführungen automatisch als abgeschlossen markiert.

Um viele untergeordnete Ausführungen effizient zu erstellen, verwenden Sie die [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-)-Methode. Weil jeder Erstellungsvorgang zu einem Netzwerkaufruf führt, ist das Erstellen eines Ausführungsbatches effizienter als ein jeweils einzelnes Erstellen der Ausführungen.

### <a name="submit-child-runs"></a>Senden untergeordneter Ausführungen

Untergeordnete Ausführungen können auch aus einer übergeordneten Ausführung gesendet werden. Dies ermöglicht es Ihnen, Hierarchien von übergeordneten und untergeordneten Ausführungen zu erstellen, von denen jede auf verschiedenen Computezielen ausgeführt wird und die über eine allgemeine übergeordnete Ausführungs-ID verbunden sind.

Verwenden Sie die ['submit_child()'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#submit-child-config--tags-none----kwargs-)-Methode, um eine untergeordnete Ausführung aus einer übergeordneten Ausführung zu senden. Ermitteln Sie dazu im Skript der übergeordneten Ausführung den Ausführungskontext, und senden Sie die untergeordnete Ausführung mit der ``submit_child``-Methode der Kontextinstanz.

```python
## In parent run script
parent_run = Run.get_context()
child_run_config = ScriptRunConfig(source_directory='.', script='child_script.py')
parent_run.submit_child(child_run_config)
```

In einer untergeordneten Ausführung können Sie die ID der übergeordneten Ausführung anzeigen:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Abfragen von untergeordneten Ausführungen

Um die untergeordneten Ausführungen eines bestimmten übergeordneten Elements abzufragen, verwenden Sie die [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-)-Methode. Über das Argument ``recursive = True`` können Sie eine geschachtelte Struktur aus untergeordneten Elementen und Enkelelementen abfragen.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Markieren und Suchen von Ausführungen

In Azure Machine Learning können Sie Eigenschaften und Tags zum Organisieren und Abfragen wichtiger Informationen Ihrer Ausführungen verwenden.

### <a name="add-properties-and-tags"></a>Hinzufügen von Eigenschaften und Tags

#### <a name="using-the-sdk"></a>Verwenden des SDK

Um Ihren Ausführungen durchsuchbare Metadaten hinzuzufügen, verwenden Sie die [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-)-Methode. Der folgende Code fügt der Ausführung z. B. die `"author"`-Eigenschaft hinzu:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Eigenschaften sind unveränderlich, sodass sie eine permanente Aufzeichnung zu Überwachungszwecken erstellen. Das folgende Codebeispiel führt zu einem Fehler, weil `"azureml-user"` im vorstehenden Code bereits als Wert der `"author"`-Eigenschaft hinzugefügt wurde:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

Im Gegensatz zu Eigenschaften können Tags geändert werden. Um durchsuchbare und aussagekräftige Informationen für Consumer Ihres Experiments hinzuzufügen, verwenden Sie die [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-)-Methode.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Sie können auch einfache Zeichenfolgentags hinzufügen. Wenn diese Tags in das Tag-Wörterbuch als Schlüssel aufgenommen wurden, haben sie den Wert `None`.

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Verwenden der CLI

> [!NOTE]
> Mit der CLI können Sie nur Tags hinzufügen oder aktualisieren.

Verwenden Sie den folgenden Befehl, um ein Tag hinzuzufügen oder zu aktualisieren:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Weitere Informationen finden Sie unter [az ml run update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Abfragen von Eigenschaften und Tags

Sie können Ausführungen in einem Experiment abfragen, um eine Liste der Ausführungen zurückzugeben, die mit bestimmten Eigenschaften und Tags übereinstimmen.

#### <a name="using-the-sdk"></a>Verwenden des SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Verwenden der CLI

Die Azure CLI unterstützt [JMESPath](http://jmespath.org)-Abfragen, mit denen Ausführungen nach Eigenschaften und Tags gefiltert werden können. Wenn Sie eine JMESPath-Abfrage mit der Azure CLI verwenden möchten, geben Sie diese mit dem Parameter `--query` an. Die folgenden Beispiele zeigen grundlegende Abfragen mithilfe von Eigenschaften und Tags:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Weitere Informationen zum Abfragen von Azure CLI-Ergebnissen finden Sie unter [Abfragen der Azure CLI-Befehlsausgabe](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

## <a name="example-notebooks"></a>Beispielnotebooks

Die folgenden Notebooks veranschaulichen die Konzepte in diesem Artikel:

* Weitere Informationen zu den Protokollierungs-APIs finden Sie im [Protokollierungs-API-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Weitere Informationen zum Verwalten von Ausführungen mit dem Azure Machine Learning-SDK finden Sie in dem Beitrag zum [Verwalten von Notebooks von Ausführungen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Protokollieren von Metriken für Ihre Experimente finden Sie unter [Protokollieren von Metriken während Trainingsausführungen](how-to-track-experiments.md).
