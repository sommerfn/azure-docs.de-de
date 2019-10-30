---
title: Planen von Azure Machine Learning-Pipelines
titleSuffix: Azure Machine Learning
description: Planen von Azure Machine Learning-Pipelines mit dem Azure Machine Learning SDK für Python. Geplante Pipelines ermöglichen die Automatisierung routinemäßiger, zeitaufwändiger Aufgaben wie Datenverarbeitung, Training und Überwachung.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 10/15/2019
ms.openlocfilehash: 31c3cd944651b9ba4ca4fcaa275e5b0ccedd947c
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72559404"
---
# <a name="schedule-machine-learning-pipelines-with-azure-machine-learning-sdk-for-python"></a>Planen von Machine Learning-Pipelines mit dem Azure Machine Learning-SDK für Python

In diesem Artikel erfahren Sie, wie Sie programmgesteuert eine Pipeline für die Ausführung in Azure planen. Sie können wahlweise einen Plan erstellen, der auf verstrichener Zeit oder auf Änderungen im Dateisystem beruht. Zeitbasierte Pläne können zum Erledigen von Routineaufgaben (z.B. Datendriftüberwachung) verwendet werden. Pläne auf Basis von Änderungen können für die Reaktion auf unregelmäßige bzw. unvorhersehbare Änderungen (z.B. Hochladen neuer Daten oder Bearbeiten alter Daten) verwendet werden. Nachdem Sie gelernt haben, wie Pläne erstellt werden, erfahren Sie, wie Sie diese abrufen und deaktivieren können.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.

* Eine Python-Umgebung, in der das Azure Machine Learning SDK für Python installiert ist. Weitere Informationen finden Sie unter [Erstellen und Verwalten von wiederverwendbaren Umgebungen für Training und Bereitstellung mit Azure Machine Learning](how-to-use-environments.md).

* Ein Machine Learning-Arbeitsbereich mit einer veröffentlichten Pipeline. Sie können die Pipeline verwenden, die in [Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning SDK](how-to-create-your-first-pipeline.md) erstellt wurde.

## <a name="initialize-the-workspace--get-data"></a>Initialisieren des Arbeitsbereichs und Abrufen von Daten

Zum Planen einer Pipeline benötigen Sie einen Verweis auf Ihren Arbeitsbereich, den Bezeichner (ID) der veröffentlichten Pipeline und den Namen des Experiments, in dem Sie den Plan erstellen möchten. Diese Werte können Sie mithilfe des folgenden Codes abrufen:

```Python
import azureml.core
from azureml.core import Workspace
from azureml.pipeline.core import Pipeline, PublishedPipeline
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

experiments = Experiment.list(ws)
for experiment in experiments:
    print(experiment.name)

published_pipelines = PublishedPipeline.list(ws)
for published_pipeline in  published_pipelines:
    print(f"{published_pipeline.name},'{published_pipeline.id}'")

experiment_name = "MyExperiment" 
pipeline_id = "aaaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" 
```

## <a name="create-a-schedule"></a>Erstellen eines Zeitplans

Wenn eine Pipeline regelmäßig ausgeführt werden soll, erstellen Sie einen Plan. Ein `Schedule` verknüpft eine Pipeline, ein Experiment und einen Auslöser. Der Auslöser kann entweder vom Typ `ScheduleRecurrence` sein, der die Wartezeit zwischen den Ausführungen beschreibt, oder ein Datenspeicherpfad, der ein Verzeichnis angibt, das auf Änderungen überwacht werden soll. In beiden Fällen benötigen Sie die ID der Pipeline und den Namen des Experiments, in dem der Plan erstellt werden soll.

### <a name="create-a-time-based-schedule"></a>Erstellen eines zeitbasierten Plans

Der `ScheduleRecurrence`-Konstruktor verfügt über das erforderliche `frequency`-Argument, das einer der folgenden Zeichenfolgen entsprechen muss: „Minute“, „Stunde“, „Tag“, „Woche“ oder „Monat“. Außerdem wird ein ganzzahliges `interval`-Argument benötigt, das angibt, wie viele `frequency`-Einheiten zwischen den einzelnen Starts des Zeitplans verstreichen sollen. Optionale Argumente ermöglichen Ihnen eine genauere Angabe der Startzeiten, wie in der [Dokumentation zum ScheduleRecurrence SDK](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedulerecurrence?view=azure-ml-py) ausführlich erläutert.

Erstellen Sie einen `Schedule`, der alle 15 Minuten eine Ausführung startet:

```python
recurrence = ScheduleRecurrence(frequency="Minute", interval=15)
recurring_schedule = Schedule.create(ws, name="MyRecurringSchedule", 
                            description="Based on time",
                            pipeline_id=pipeline_id, 
                            experiment_name=experiment_name, 
                            recurrence=recurrence)
```

### <a name="create-a-change-based-schedule"></a>Erstellen eines änderungsbasierten Plans

Pipelines, die durch Dateiänderungen ausgelöst werden, sind möglicherweise effizienter als zeitbasierte Pläne. Vielleicht möchten Sie zum Beispiel einen Vorverarbeitungsschritt ausführen, wenn eine Datei geändert oder einem Datenverzeichnis eine neue Datei hinzugefügt wird. Sie können alle Änderungen an einem Datenspeicher oder Änderungen in einem bestimmten Verzeichnis des Datenspeichers überwachen. Wenn Sie ein bestimmtes Verzeichnis überwachen, lösen Änderungen in den Unterverzeichnissen dieses Verzeichnisses _keine_ Ausführung aus.

Um einen `Schedule` zu erstellen, der auf Dateiänderungen reagiert, müssen Sie den `datastore`-Parameter im Aufruf auf [Schedule.create](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.schedule.schedule?view=azure-ml-py#create-workspace--name--pipeline-id--experiment-name--recurrence-none--description-none--pipeline-parameters-none--wait-for-provisioning-false--wait-timeout-3600--datastore-none--polling-interval-5--data-path-parameter-name-none--continue-on-step-failure-none--path-on-datastore-none---workflow-provider-none---service-endpoint-none-) festlegen. Zum Überwachen eines Ordners legen Sie das `path_on_datastore`-Argument fest.

Mit dem `polling_interval`-Argument können Sie das Intervall, in dem der Datenspeicher auf Änderungen geprüft wird, in Minuten angeben.

Wenn die Pipeline mit [DataPath](https://docs.microsoft.com/python/api/azureml-core/azureml.data.datapath.datapath?view=azure-ml-py) [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelineparameter?view=azure-ml-py) erstellt wurde, können Sie durch Festlegen des `data_path_parameter_name`-Arguments diese Variable auf den Namen der geänderten Datei einstellen.

```python
datastore = Datastore(workspace=ws, name="workspaceblobstore")

reactive_schedule = Schedule.create(ws, name="MyReactiveSchedule", description="Based on time",
                            pipeline_id=pipeline_id, experiment_name=experiment_name, datastore=datastore, data_path_parameter_name="input_data")
```

### <a name="optional-arguments-when-creating-a-schedule"></a>Optionale Argumente beim Erstellen eines Plans

Neben den zuvor beschriebenen Argumenten können Sie das `status`-Argument auf `"Disabled"` einstellen, um einen inaktiven Plan zu erstellen. Schließlich können Sie noch mit `continue_on_step_failure` einen booleschen Wert übergeben, der das standardmäßige Fehlerverhalten der Pipeline überschreibt.

## <a name="view-your-scheduled-pipelines"></a>Anzeigen der geplanten Pipelines

Navigieren Sie in Ihrem Webbrowser zum Arbeitsbereich des Machine Learning Service. Wählen Sie im Navigationsbereich im Bereich **Ressourcen** den Eintrag **Pipelines** aus. Über diesen Link gelangen Sie zu einer Liste der im Arbeitsbereich veröffentlichten Pipelines.

![Seite „Pipelines“ im Arbeitsbereich](media/how-to-schedule-a-pipeline/pipelines-list.png)

Auf dieser Seite können Sie Übersichtsinformationen wie Name, Beschreibung, Status usw. zu allen Pipelines im Arbeitsbereich anzeigen. Durch Klicken auf eine Pipeline erhalten Sie ausführlichere Informationen. Auf der Seite, die dann geöffnet wird, finden Sie weitere Details zu Ihrer Pipeline und können einzelne Ausführungen genauer betrachten.

## <a name="deactivate-the-pipeline"></a>Deaktivieren der Pipeline

Wenn Sie über eine `Pipeline` verfügen, die zwar veröffentlicht, aber noch nicht geplant wurde, können Sie diese wie folgt deaktivieren:

```python
pipeline = PublishedPipeline.get(ws, id=pipeline_id)
pipeline.disable()
```

Wenn die Pipeline bereits geplant ist, müssen Sie zuerst den Plan abbrechen. Rufen Sie die ID des Plans aus dem Portal ab, oder führen Sie folgenden Befehl aus:

```python
ss = Schedule.list(ws)
for s in ss:
    print(s)
```

Sobald Sie über die `schedule_id` des zu deaktivierenden Plans verfügen, führen Sie Folgendes aus:

```python
def stop_by_schedule_id(ws, schedule_id):
    s = next(s for s in Schedule.list(ws) if s.id == schedule_id)
    s.disable()
    return s

stop_by_schedule(ws, schedule_id)
```

Wenn Sie dann `Schedule.list(ws)` erneut ausführen, sollte eine leere Liste angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie das Azure Machine Learning SDK für Python verwendet, um eine Pipeline auf zwei verschiedene Arten zu planen. Ein Plan wiederholt sich auf Basis der verstrichenen Uhrzeit. Der andere Plan wird ausgeführt, wenn eine Datei in einem angegebenen `Datastore` oder in einem Verzeichnis in diesem Speicher geändert wird. Sie haben erfahren, wie Sie das Portal zum Untersuchen von Pipelines und einzelnen Ausführungen verwenden können. Abschließend haben Sie gelernt, wie Sie einen Plan deaktivieren, damit die Pipeline nicht mehr ausgeführt wird.

Weitere Informationen finden Sie unter

> [!div class="nextstepaction"]
> [Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md)

* Weitere Informationen zu [Pipelines](concept-ml-pipelines.md)
* Erfahren Sie mehr über das [Erkunden von Azure Machine Learning mit Jupyter](samples-notebooks.md)
