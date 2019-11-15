---
title: Beschreibung von ML-Pipelines
titleSuffix: Azure Machine Learning
description: In diesem Artikel lernen Sie die Vorteile von Machine Learning-Pipelines kennen, die Sie mit dem Azure Machine Learning SDK für Python erstellen können. Machine Learning-Pipelines werden von Data Scientists verwendet, um Workflows für das maschinelle Lernen zu erstellen, zu optimieren und zu verwalten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 11/04/2019
ms.openlocfilehash: f1b061e92ce0650da4d5b95643eb6e9df917e3b8
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73671550"
---
# <a name="what-are-azure-machine-learning-pipelines"></a>Beschreibung von Azure Machine Learning-Pipelines

Mit Azure Machine Learning Pipelines können Sie Workflows in ihren Machine Learning-Projekten erstellen. Diese Workflows bieten eine Reihe von Vorteilen: 

+ Einfachheit
+ Geschwindigkeit
+ Wiederholbarkeit
+ Flexibilität
+ Versionsverwaltung und Nachverfolgung
+ Modularität 
+ Qualitätssicherung
+ Kostenkontrolle

Diese Vorteile werden in dem Moment wichtig, da Ihr Machine Learning-Projekt das Feld des reinen Experiments verlässt und in die Weiterentwicklung eintritt. Sogar einfache Pipelines mit nur einem Schritt können nützlich sein. Machine Learning-Projekte befinden sich oftmals in einem komplexen Zustand, und es kann wirklich erholsam sein, aus der präzisen Erfüllung eines einzelnen Workflows einen trivialen Vorgang zu machen.

Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).

![Machine Learning-Pipelines in Azure Machine Learning](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Welche Azure-Pipelinetechnologie sollte ich verwenden?

Die Azure-Cloud bietet mehrere andere Pipelines, die jeweils einem anderen Zweck dienen. Die folgende Tabelle listet die verschiedenen Pipelines und ihren Verwendungszweck auf:

| Pipeline | Funktionsbeschreibung | Kanonische Pipe |
| ---- | ---- | ---- |
| Azure Machine Learning-Pipelines | Definiert wiederverwendbare Machine Learning-Workflows, die als Vorlage für Ihre Machine Learning-Szenarien verwendet werden können. | Daten -> Modell |
| [Azure Data Factory-Pipelines](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Gruppieren Datenverschiebungs-, Transformations- und Steuerungsaktivitäten, die für das Ausführen einer Aufgabe erforderlich sind.  | Daten -> Daten |
| [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) | Continuous Integration und Delivery Ihrer Anwendung auf jeder Plattform und in jeder Cloud  | Code -> App/Dienst |

## <a name="what-can-azure-ml-pipelines-do"></a>Wozu sind Azure ML-Pipelines imstande?

Eine Azure Machine Learning-Pipeline ist ein unabhängig ausführbarer Workflow einer vollständigen Machine Learning-Aufgabe. Teilaufgaben werden als eine Reihe von Schritten in der Pipeline gekapselt. Eine Azure Machine Learning-Pipeline kann ganz einfach sein und nur aus dem Aufruf eines Python-Skripts bestehen, also _kann_ sie fast alles. Pipelines _sollten_ aber den Schwerpunkt auf Machine Learning-Aufgaben legen, wie etwa:

+ Datenvorbereitung einschließlich Import, Validierung und Bereinigung, Verfremdung und Transformation, Normalisierung und Staging
+ Trainingskonfiguration einschließlich Parametrisierungsargumenten, Dateipfaden und Protokollierungs-/Berichtskonfigurationen
+ Effizientes und wiederholtes Trainieren und Validieren von Daten, einschließlich der Angabe spezifischer Datenteilmengen, verschiedener Hardware-Computeressourcen, verteilter Verarbeitung und Fortschrittsüberwachung
+ Bereitstellung, einschließlich Versionierung, Skalierung, Bereitstellung und Zugriffssteuerung 

Durch unabhängige Schritte können mehrere Datenanalysten gleichzeitig an derselben Pipeline arbeiten, ohne die Computeressourcen zu überlasten. Außerdem ist es bei einzelnen Schritten einfacher, für jeden Schritt verschiedene Computetypen/-größen zu verwenden.

Nachdem die Pipeline entworfen wurde, wird ihr Trainingsprozess in der Regel weiter optimiert. Wenn Sie eine Pipeline erneut ausführen, springt die Ausführung zu den Schritten, die wiederholt werden müssen, z.B. zu einem aktualisierten Trainingsskript. Schritte, die nicht erneut ausgeführt werden müssen, werden übersprungen. Das gleiche ergibt sich für unveränderte Skripts, die zum Ausführen des Schritts verwendet werden. Diese Wiederverwendungsfunktion hilft bei der Ausführung kostspieliger und zeitintensiver Schritte wie der Datenerfassung und -transformation, wenn die zugrunde liegenden Daten unverändert sind.

Mit Azure Machine Learning können Sie verschiedene Toolkits und Frameworks wie PyTorch oder TensorFlow für jeden Pipelineschritt verwenden. Azure koordiniert die verschiedenen [Computeziele](concept-azure-machine-learning-architecture.md), die Sie verwenden, sodass Ihre Zwischendaten für die nachgelagerten Computeziele freigegeben werden können.

Für das [Nachverfolgen der Metriken für Ihre Pipelineexperimente](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) haben Sie zwei Möglichkeiten: Nachverfolgung direkt im Azure-Portal oder über die [Landing Page Ihres Arbeitsbereichs (Vorschau)](https://ml.azure.com). Nach dem Veröffentlichen einer Pipeline können Sie einen REST-Endpunkt konfigurieren, mit dem Sie die Pipeline über eine beliebige Plattform bzw. einen beliebigen Stapel erneut ausführen können.

Kurz gesagt können alle komplexen Aufgaben des Machine Learning-Lebenszyklus mithilfe von Pipelines unterstützt werden. Andere Azure-Pipelinetechnologien weisen eigene Stärken auf, beispielsweise [Azure Data Factory-Pipelines](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) für das Arbeiten mit Daten und [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) für Continuous Integration und Deployment. Wenn Ihr Schwerpunkt jedoch auf Machine Learning liegt, sind Azure Machine Learning-Pipelines wahrscheinlich die beste Wahl für Ihre Workflowanforderungen. 

## <a name="what-are-azure-ml-pipelines"></a>Beschreibung von Azure ML-Pipelines

Eine Azure ML-Pipeline führt einen vollständigen logischen Workflow mit einer geordneten Abfolge von Schritten aus. Jeder Schritt stellt eine diskrete Verarbeitungsaktion dar. Pipelines werden im Kontext eines Azure Machine Learning-[Experiments](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) ausgeführt.

In den sehr frühen Phasen eines ML-Projekts ist nichts dagegen einzuwenden, dass ein einzelnes Jupyter-Notebook oder Python-Skript sämtliche Aufgaben der Azure-Arbeitsbereichs- und -Ressourcenkonfiguration durchführt und für Datenvorbereitung, Laufzeitkonfiguration, Training und Überprüfung zuständig ist. Aber genau wie Funktionen und Klassen schnell den Vorzug vor einem einzelnen, gebieterischen Codeblock erhalten, werden ML-Workflows schnell zur bevorzugten Option im Vergleich mit einem monolithischen Notebook oder Skript. 

Durch Modularisierung von ML-Aufgaben unterstützen Pipelines den Imperativ der Informatik, dass eine Komponente „(nur) eine Sache gut machen“ sollte. Modularität ist eindeutig kritisch für den Erfolg eines Projekts, wenn die Programmierung in Teams erfolgt, aber selbst bei der Arbeit ohne Mitstreiter umfassen auch kleine ML-Projekte verschiedene Aufgaben, von denen jede einen erheblichen Grad an Komplexität aufweist. Zu den Aufgaben gehören: Workspacekonfiguration und Datenzugriff, Datenvorbereitung, Modelldefinition und -konfiguration und Bereitstellung. Während die Ausgaben einer oder mehrerer Aufgaben jeweils die Eingaben weiterer Aufgaben bilden, stellen die Implementierungsdetails jeder einzelnen Aufgabe für die nächste bestenfalls eine irrelevante Ablenkung dar. Im schlimmsten Fall kann der Berechnungszustand einer Aufgabe einen Fehler in einer anderen verursachen. 

### <a name="analyzing-dependencies"></a>Analysieren von Abhängigkeiten

Viele Programmierökosysteme weisen Tools zum Orchestrieren von Ressourcen-, Bibliotheks- oder Kompilierungsabhängigkeiten auf. Im Allgemeinen verwenden diese Tools Dateizeitstempel zum Berechnen von Abhängigkeiten. Wenn eine Datei geändert wird, werden nur sie und ihre Abhängigkeiten aktualisiert (heruntergeladen, erneut kompiliert oder verpackt). Azure ML-Pipelines erweitern dieses Konzept erheblich. Wie herkömmliche Buildtools errechnen Pipelines Abhängigkeiten zwischen Schritten und wiederholen nur die erforderlichen Neuberechnungen. 

Die Abhängigkeitsanalyse in Azure ML-Pipelines ist jedoch anspruchsvoller als einfache Zeitstempel. Jeder Schritt kann in einer anderen Hardware- und Softwareumgebung ausgeführt werden. Die Datenaufbereitung ist möglicherweise ein zeitaufwändiger Vorgang, sie muss aber nicht auf Hardware mit leistungsstarken GPUs ausgeführt werden, für bestimmte Schritte ist vielleicht betriebssystemspezifische Software erforderlich, oder Sie entscheiden sich für verteiltes Training usw. Zwar können die Kosteneinsparungen durch das Optimieren von Ressourcen erheblich sein, es kann aber schlicht erdrückend sein, manuell durch all die verschiedenen Variationen in Hardware- und Softwareressoucen zu balancieren. Noch schwieriger ist es, all dies zu erreichen, ohne jemals einen Fehler bei der Übertragung der Daten zwischen den einzelnen Schritten zu machen. 

Pipelines lösen diese Probleme. Azure Machine Learning orchestriert automatisch alle Abhängigkeiten zwischen den Schritten der Pipeline. Diese Orchestrierung kann das Hoch- und Herunterfahren von Docker-Images, das Einbinden und Trennen von Computeressourcen und das automatisierte Verschieben der Daten zwischen den einzelnen Schritten in konsistenter Weise beinhalten.

### <a name="reusing-results"></a>Wiederverwendung von Ergebnissen

Darüber hinaus kann die Ausgabe eines Schritts auf Wunsch wiederverwendet werden. Wenn Sie die Wiederverwendung als eine Möglichkeit angeben und keine Abhängigkeiten im Upstream die Neuberechnung auslösen, verwendet der Pipelinedienst eine zwischengespeicherte Version der Ergebnisse des Schritts. Durch diese Art von Wiederverwendung kann die Entwicklungszeit erheblich verkürzt werden. Wenn Sie eine komplexe Datenaufbereitungsaufgabe bewältigen müssen, führen Sie sie wahrscheinlich öfter aus als unbedingt erforderlich. Pipelines nehmen Ihnen diese Sorge ab: Wenn es erforderlich ist, wird der Schritt ausgeführt, sonst nicht.

Diese gesamte Abhängigkeitsanalyse, -orchestrierung und -aktivierung werden von Azure Machine Learning ausgeführt, wenn Sie ein [Pipeline](https://docs.microsoft.com/api/azureml-pipeline-core/azureml.pipeline.core.pipeline(class)?view=azure-ml-py)-Objekt instanziieren, es an einen `Experiment` übergeben und `submit()` aufrufen. 

### <a name="coordinating-the-steps-involved"></a>Koordinieren der beteiligten Schritte

Beim Erstellen und Ausführen eines `Pipeline`-Objekts finden die folgenden Schritte auf einer allgemeinen Ebene statt:

+ Für jeden Schritt berechnet der Dienst die Anforderungen für:
    + Hardware-Computeressourcen
    + Betriebssystemressourcen (Docker-Image(s))
    + Softwareresources (Conda/virtualenv-Abhängigkeiten)
    + Dateneingaben 
+ Der Dienst bestimmt die Abhängigkeiten zwischen Schritten, was zu einem dynamischen Ausführungsdiagramm führt
+ Wenn jeder Knoten in einem Ausführungsdiagramm ausgeführt wird:
    + konfiguriert der Dienst die erforderlichen Hardware- und Softwareumgebungen (möglicherweise unter Wiederverwendung vorhandener Ressourcen)
    + Der Schritt wird ausgeführt und bietet Protokollierungs- und Überwachungsinformationen für das ihn enthaltende `Experiment`-Objekt
    + Wenn der Schritt abgeschlossen wird, werden seine Ausgaben als Eingaben für den nächsten Schritt vorbereitet und/oder in den Speicher geschrieben
    + Nicht mehr benötigte Ressourcen werden finalisiert und getrennt

![Pipelineschritte](media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

## <a name="building-pipelines-with-the-python-sdk"></a>Erstellen von Pipelines mit dem Python-SDK

Im [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) ist eine Pipeline ein Python-Objekt, das im `azureml.pipeline.core`-Modul definiert ist. Ein [Pipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py)-Objekt enthält eine geordnete Abfolge von einem oder mehreren [PipelineStep](https://docs.microsoft.com/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py)-Objekten. Die `PipelineStep`-Klasse ist abstrakt, und die eigentlichen Schritte gehören Unterklassen an, wie etwa [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep?view=azure-ml-py), [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.pythonscriptstep?view=azure-ml-py) oder [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py). Die [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep?view=azure-ml-py)-Klasse enthält eine wiederverwendbare Abfolge von Schritten, die von mehreren Pipelines gemeinsam verwendet werden können. Eine `Pipeline` wird als Teil einer `Experiment` ausgeführt.

Eine Azure ML-Pipeline ist einem Azure Machine Learning-Arbeitsbereich zugeordnet, und ein Pipelineschritt ist einem verfügbaren Computeziel in diesem Arbeitsbereich zugeordnet. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Azure Machine Learning-Arbeitsbereichen im Azure-Portal](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-workspace) oder [Was sind Computeziele in Azure Machine Learning?](https://docs.microsoft.com/azure/machine-learning/service/concept-compute-target).

Für Azure Machine Learning stellt ein Computeziel die Umgebung dar, in der eine ML-Phase stattfindet. Bei der Softwareumgebung kann es sich um eine Remote-VM, Azure Machine Learning Compute, Azure Databricks, Azure Batch usw. handeln. Die Hardwareumgebung kann ebenfalls eine erhebliche Streubreite aufweisen, abhängig von GPU-Unterstützung, Arbeitsspeicher, Speicherplatz usw. Sie können das Computeziel für jeden Schritt festlegen, was Ihnen eine feiner abgestufte Kostenkontrolle erlaubt. Sie können für die spezifische Aktion, das Datenvolumen und die Leistungsanforderungen Ihres Projekts mehr oder weniger leistungsstarke Ressourcen einsetzen. 

## <a name="building-pipelines-with-the-designer"></a>Erstellen von Pipelines mit dem Designer

Entwickler, die eine visuelle Entwurfsoberfläche bevorzugen, können Pipelines mit dem Azure Machine Learning-Designer erstellen. Sie können über die Auswahl **Designer** auf der Startseite Ihres Arbeitsbereichs auf dieses Tool zugreifen.  Der Designer ermöglicht Ihnen, Schritte per Drag & Drop auf die Entwurfsoberfläche zu verschieben. Zur schnellen Entwicklung können Sie vorhandene Module im gesamten Spektrum von ML-Aufgaben verwenden. Die vorhandenen Module decken alle Schritte von der Datentransformation über die Algorithmusauswahl bis hin zur Bereitstellung ab. Sie können auch eine vollständig benutzerdefinierte Pipeline erstellen, indem Sie Ihre eigenen in Python-Skripts definierten Schritte kombinieren.

Wenn Sie Pipelines visuell entwerfen, werden die Eingaben und Ausgaben eines Schritts sichtbar angezeigt. Für die Datenverbindungen wird Drag & Drop unterstützt, sodass Sie den Datenfluss Ihrer Pipeline schnell verstehen und ändern können.
 
![Azure Machine Learning-Designer: Beispiel](./media/concept-ml-pipelines/designer-drag-and-drop.gif)

### <a name="understanding-the-execution-graph"></a>Grundlegendes zum Ausführungsdiagramm

Die Schritte in einer Pipeline weisen möglicherweise Abhängigkeiten von anderen Schritten auf. Der Azure ML-Pipelinedienst übernimmt die Arbeit, diese Abhängigkeiten zu orchestrieren und zu analysieren. Die Knoten im resultierenden „Ausführungsdiagramm“ stellen Verarbeitungsschritte dar. Jeder Schritt kann das Erstellen oder Wiederverwenden einer bestimmten Kombination aus Hardware und Software, die Wiederverwendung zwischengespeicherter Ergebnisse usw. beinhalten. Die Orchestrierung und Optimierung dieses Ausführungsdiagramms kann eine ML-Phase erheblich beschleunigen und die Kosten verringern. 

Da die Schritte unabhängig ausgeführt werden, müssen die Objekte zur Aufnahme der Eingabe- und Ausgabedaten, die zwischen den verschiedenen Schritten fließen, extern definiert werden. Dies ist die Rolle von [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py), [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py) und der zugeordneten Klassen. Diese Datenobjekte sind mit einem [Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore%28class%29?view=azure-ml-py)-Objekt verknüpft, in dem ihre Speicherkonfiguration verkapselt ist. Die `PipelineStep`-Basisklasse wird immer mit einer `name`-Zeichenfolge, einer Liste von `inputs` und einer Liste von `outputs` erstellt. Normalerweise ist außerdem eine Liste `arguments` von und häufig auch eine Liste von `resource_inputs` vorhanden. Unterklassen verfügen in der Regel ebenfalls über zusätzliche Argumente (beispielsweise erfordert `PythonScriptStep`den Dateinamen und Pfad des auszuführenden Skripts). 

Das Ausführungsdiagramm ist azyklisch, Pipelines können aber planmäßig wiederholt werden und sind imstande, Python-Skripts ausführen, die Statusinformationen in das Dateisystem schreiben können, wodurch die Erstellung komplexer Profile möglich wird. Wenn Sie Ihre Pipeline so entwerfen, dass bestimmte Schritte parallel oder asynchron ausgeführt werden können, verarbeitet Azure Machine Learning die Abhängigkeitsanalyse und die Koordination von Auffächern und Zusammenführen (Fan-out, Fan-in) transparent. Im Allgemeinen brauchen Sie sich nicht mit den Details des Ausführungsdiagramms zu befassen, sie können aber über das [Pipeline.Graph](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline.pipeline?view=azure-ml-py#attributes) Attribut abgerufen werden. 


### <a name="a-simple-python-pipeline"></a>Eine einfache Python-Pipeline

Dieser Codeausschnitt zeigt die Objekte und Aufrufe, die zum Erstellen und Ausführen eines einfachen `Pipeline` erforderlich sind:

```python
ws = Workspace.from_config() 
blob_store = Datastore(ws, "workspaceblobstore")
compute_target = ws.compute_targets["STANDARD_NC6"]
experiment = Experiment(ws, 'MyExperiment') 

input_data = DataReference(
    datastore=Datastore(ws, blob_store),
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")

output_data = PipelineData(
    "output_data",
    datastore=blob_store,
    output_name="output_data1")

steps = [ PythonScriptStep(
    script_name="train.py",
    arguments=["--input", input_data, "--output", output_data],
    inputs=[input_data],
    outputs=[output_data],
    compute_target=compute_target,
    source_directory="myfolder"
) ]

pipeline = Pipeline(workspace=ws, steps=steps)

pipeline_run = experiment.submit(pipeline)
pipeline_run.wait_for_completion()
```

Dieser Codeausschnitt beginnt mit gebräuchlichen Azure Machine Learning-Objekten, einem `Workspace`, einem `Datastore`, einem [ComputeTarget](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py) und einem `Experiment`. Anschließend erstellt der Code die Objekte, die `input_data` und `output_data` aufnehmen. Das Array `steps` enthält ein einzelnen Element, ein `PythonScriptStep`, das die Datenobjekte verwendet und auf dem `compute_target` ausgeführt wird. Anschließend instanziiert der Code das eigentliche `Pipeline`-Objekt und übergibt den Workspace und das Array der Schritte. Mit dem Aufruf von `experiment.submit(pipeline)` beginnt die Ausführung der Azure ML-Pipeline. Der Aufruf von `wait_for_completion()` wird dann bis zum Abschluss der Pipeline gesperrt. 

## <a name="best-practices-when-using-pipelines"></a>Bewährte Methoden bei der Verwendung von Pipelines

Wie Sie sehen können, ist das Erstellen einer Azure ML-Pipeline etwas komplexer als das Starten eines Skripts. Für Pipelines müssen einige Python-Objekte konfiguriert und erstellt werden. 

Einige Situationen, in denen sich die Verwendung einer Pipeline anbietet:

* In einer Teamumgebung: Teilen Sie ML-Aufgaben in mehrere unabhängige Schritte auf, damit Entwickler unabhängig voneinander arbeiten und ihre Programme weiterentwickeln können. 

* Während oder kurz vor der Bereitstellung: Schreiben Sie die Konfiguration fest, und verwenden Sie geplante und ereignisgesteuerte Vorgänge, um trotz sich ändernder Daten den Überblick zu behalten.

* In den frühen Phasen eines ML-Projekts oder beim Arbeiten ohne Mitarbeiter: Verwenden Sie Pipelines, um den Build zu automatisieren. Vielleicht haben Sie sich schon einmal Gedanken über die Neuerstellung der Konfiguration und des Berechnungsstatus gemacht, bevor Sie eine neue Idee implementieren – das ist genau das Signal, dass Sie die Verwendung einer Pipeline zum Automatisieren des Workflows zumindest in Erwägung ziehen sollten. 

Angesichts der Wiederverwendung zwischengespeicherter Ergebnisse, der fein abgestuften Kontrolle der Berechnungskosten und der Prozessisolation ist es leicht, in Enthusiasmus zu verfallen, aber auch Pipelines haben ihren Preis. Zu den Antimustern gehören:

* Pipelines als einziges Mittel zur Trennung von Anliegen zu verwenden. Die integrierten Funktionen, Objekte und Module von Python helfen bei der Vermeidung verwirrender Programmzustände ein gutes Stück weiter! Ein Pipelineschritt ist viel aufwändiger als ein Funktionsaufruf.

* Enge Kopplung zwischen Schritten einer Pipeline. Wenn das Refactoring eines abhängigen Schritts häufig die Änderung der Ausgaben eines früheren Schritts erforderlich macht, ist es recht wahrscheinlich, dass die separaten Schritte zurzeit mehr kosten als sie nutzen. Ein weiterer Hinweis auf übermäßig enge Kopplung sind Argumente für einen Schritt, die nicht Daten darstellen sondern Flags zur Prozesssteuerung. 

* Vorzeitiges Optimieren von Computeressourcen. Beispielsweise besteht die Datenaufbereitung häufig aus verschiedenen Phasen, und man stößt häufig auf solche Stellen „Oh, an dieser Stelle könnte ich ein `MpiStep` für parallele Programmierung einsetzen, aber hier ist eine Stelle, wo ich `PythonScriptStep` mit einem weniger leistungsstarken Computeziel verwenden könnte“ usw. Und vielleicht erweist sich das Erstellen fein abgestufter Schritte auf lange Sicht wirklich als lohnend, insbesondere, wenn die Möglichkeit zur Verwendung zwischengespeicherter Ergebnisse besteht, statt immer neu zu berechnen. Pipelines sind aber nicht als Ersatz für das `multiprocessing`-Modul gedacht. 

Bis zu dem Punkt, dass ein Projekt groß wird oder sich der Bereitstellung nähert, sollten Ihre Pipelines eher grob statt fein abgestuft sein. Wenn Sie sich Ihr ML-Projekt in Form von _Phasen_ und eine Pipeline als Mittel zur Bereitstellung eines vollständigen Workflows vorstellen, der Sie durch eine bestimmte Phase bringt, sind Sie auf dem richtigen Weg. 

## <a name="key-advantages"></a>Hauptvorteile

Die wichtigsten Argumente für das Verwenden von Pipelines für Ihre Workflows mit maschinellem Lernen sind die folgenden:

|Vorteil|BESCHREIBUNG|
|:-------:|-----------|
|**Unbeaufsichtigte&nbsp;Ausführung**|Planen Sie Schritte, die zuverlässig und unbeaufsichtigt parallel oder nacheinander ausgeführt werden können. Die Datenvorbereitung und -modellierung kann Tage oder Wochen in Anspruch nehmen, und mit Pipelines können Sie sich auf andere Aufgaben konzentrieren, während der Prozess ausgeführt wird. |
|**Heterogenes Compute**|Verwenden Sie mehrere Pipelines, die zuverlässig über heterogene und skalierbare Computeressourcen und Speicherorte koordiniert werden. Um die verfügbaren Computeoptionen effizient zu nutzen, können einzelne Pipelineschritte auf verschiedenen Computezielen ausgeführt werden, z. B. HDInsight, Data Science-VMs mit GPU und Databricks.|
|**Wiederverwendbarkeit**|Erstellen Sie Pipelinevorlagen für bestimmte Szenarien wie erneutes Training und Batchbewertung. Lösen Sie veröffentlichte Pipelines von externen Systemen über einfache REST-Aufrufe aus.|
|**Nachverfolgung und Versionierung**|Statt Daten- und Ergebnispfade bei der Iteration manuell zu verfolgen, verwenden Sie das Pipelines SDK, um Ihre Datenquellen, Eingaben und Ausgaben explizit zu benennen und Versionen zu verwalten. Sie können Skripts und Daten auch separat verwalten, um die Produktivität zu steigern.|
| **Modularität** | Das Trennen von Bereichen mit verschiedenen Anliegen und das Isolieren von Änderungen ermöglicht die schnellere Entwicklung von Software mit höherer Qualität. | 
|**Kollaboration**|Pipelines ermöglichen Data Scientists, in allen Bereichen des Entwurfsprozesses für maschinelles Lernen zusammenzuarbeiten, während sie gleichzeitig an Pipelineschritten arbeiten können.|

## <a name="next-steps"></a>Nächste Schritte

Azure ML-Pipelines stellen ein leistungsfähiges Hilfsmittel dar, das schon in frühen Entwicklungsphasen Mehrwert produziert. Der Wert wächst mit der Größe von Team und Projekt. In diesem Artikel wurde erläutert, wie Pipelines mit dem Azure Machine Learning Python SDK festgelegt und in Azure orchestriert werden. Sie haben etwas einfachen Quellcode gesehen und haben einige der verfügbaren `PipelineStep`-Klassen kennengelernt. Sie sollten ein Gefühl dafür entwickeln können, wann Azure ML-Pipelines eingesetzt werden und wie Azure sie ausführt. 


+ Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).

+ Erfahren Sie, wie Sie [Batchvorhersagen für große Datenmengen ausführen](tutorial-pipeline-batch-scoring-classification.md ).

+ Weitere Informationen zum [Kern von Pipelines](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und den [Pipelineschritten](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) finden Sie in der SDK-Referenzdokumentation.

+ Testen Sie Beispiele für Jupyter Notebooks, die [Azure Machine Learning-Pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines) veranschaulichen. Erfahren Sie, wie Sie [Notebooks ausführen, um diesen Dienst zu untersuchen](samples-notebooks.md).
