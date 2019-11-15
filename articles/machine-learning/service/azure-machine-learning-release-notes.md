---
title: Neuerungen im Release
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über die neuesten Updates für Azure Machine Learning und Python SDKs für maschinelles Lernen und Datenaufbereitung.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jmartens
author: j-martens
ms.date: 08/19/2019
ms.custom: seodec18
ms.openlocfilehash: 587856be6ca1064e5d2487ba6740b51a8c645fee
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73716640"
---
# <a name="azure-machine-learning-release-notes"></a>Azure Machine Learning: Anmerkungen zu dieser Version

In diesem Artikel erhalten Sie Informationen zu Azure Machine Learning-Versionen.  Den vollständigen SDK-Referenzinhalt finden Sie auf der Hauptseite der Referenz zum [**Azure Machine Learning SDK für Python**](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

Sehen Sie die [Liste der bekannten Probleme](resource-known-issues.md) an, um mehr über bekannte Fehler und Problemumgehungen zu erfahren.


## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Weboberfläche 

Die Landing Page für den Zusammenarbeitsbereich unter [https://ml.azure.com](https://ml.azure.com) wurde erweitert und wird nun als Azure Machine Learning Studio (Vorschauversion) bezeichnet.

In Studio können Sie Azure Machine Learning-Ressoucen wie Datasets, Pipelines, Modelle, Endpunkte usw. trainieren, testen, bereitstellen und verwalten.  

Greifen Sie von Studio aus auf die folgenden webbasierten Erstellungstools zu:

| Webbasiertes Tool | BESCHREIBUNG | Edition |
|-|-|-|
| Notebook-VM (Vorschau) | Vollständig verwaltete, cloudbasierte Arbeitsstation | Basic und Enterprise |
| [Automatisiertes maschinelles Lernen](tutorial-first-experiment-automated-ml.md) (Vorschauversion) | Codefreie Umgebung für die Automatisierung der Machine Learning-Modellentwicklung | Enterprise |
| [Designer](ui-concept-visual-interface.md) (Vorschauversion) | Machine Learning-Modellierungstool mit Drag & Drop, zuvor als „der Designer“ bezeichnet | Enterprise |


### <a name="azure-machine-learning-designer-enhancements"></a>Verbesserungen am Azure Machine Learning-Designer 

+ Zuvor als grafische Benutzeroberfläche bezeichnet 
+   11 neue [Module](../algorithm-module-reference/module-reference.md) einschließlich Empfehlungs-, Klassifizierungs- und Trainings-Hilfsprogrammen für Funktionsentwicklung, Kreuzvalidierung, Datentransformation u. v. m.

### <a name="r-sdk"></a>R SDK 
 
Das [Azure Machine Learning-SDK für R](tutorial-1st-r-experiment.md) wird von Data Scientists und KI-Entwicklern zum Erstellen und Ausführen von Workflows zum maschinellen Lernen mit Azure Machine Learning verwendet.

Das Azure Machine Learning SDK für R verwendet das Paket `reticulate` zur Bindung an das Python-SDK. Durch die direkte Bindung an Python ermöglicht das SDK für R den Zugriff auf die im Python-SDK implementierten Kernobjekte und -methoden aus einer beliebigen R-Umgebung Ihrer Wahl.

Zu den Hauptfunktionen des SDK gehören:

+   Verwalten von Cloudressourcen zum Überwachen, Protokollieren und Organisieren von Experimenten mit maschinellem Lernen
+   Trainieren von Modellen mithilfe von Cloudressourcen, einschließlich GPU-beschleunigten Modelltrainings.
+   Bereitstellen Ihrer Modelle als Webdienste in Azure Container Instances (ACI) und Azure Kubernetes Service (AKS).

Eine vollständige Dokumentation finden Sie auf der [Website des Pakets](https://azure.github.io/azureml-sdk-for-r).

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>Azure Machine Learning SDK für Python v1.0.72

+ **Neue Features**
  + Datasetüberwachungen wurden über das Paket [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift) hinzugefügt, sodass Zeitreihendatasets auf Datendrift oder andere statistische Änderungen im Zeitverlauf überwacht werden können. Warnungen und Ereignisse können ausgelöst werden, wenn eine Drift erkannt wird oder andere Bedingungen für die Daten erfüllt werden. Ausführliche Informationen finden Sie in der [Dokumentation](https://aka.ms/datadrift). 
  + Für Azure Machine Learning werden zwei neue Editionen (auch als „SKU“ bezeichnet) bekannt gegeben. Mit diesem Release können Sie nun einen Basic- oder Enterprise-Arbeitsbereich für Azure Machine Learning erstellen. Alle vorhandenen Arbeitsbereiche werden standardmäßig auf die Basic-Edition bezogen, und Sie können den Arbeitsbereich jederzeit im Azure-Portal oder in Studio aktualisieren. Im Azure-Portal können Sie einen Basic- oder einen Enterprise-Arbeitsbereich erstellen. Lesen Sie [die Dokumentation](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-workspace), um mehr zu erfahren. Im SDK kann die Edition Ihres Arbeitsbereichs mithilfe der sku-Eigenschaft Ihres Arbeitsbereichsobjekts ermittelt werden.
  + Wir haben auch Verbesserungen an Azure Machine Learning Compute vorgenommen. Sie können nun neben Diagnoseprotokollen zum Debuggen auch Metriken für Ihre Cluster (z. B. Gesamtknoten, ausgeführte Knoten, Gesamtkernkontingent) in Azure Monitor anzeigen. Außerdem können Sie derzeit ausgeführte oder in der Warteschlange befindliche Ausführungen in Ihrem Cluster anzeigen, z. B. die IP-Adressen der verschiedenen Knoten im Cluster. Sie können diese im Portal oder mit den entsprechenden Funktionen im SDK oder in der Befehlszeilenschnittstelle anzeigen. 
  
  + **Vorschaufunktionen**
    + Wir veröffentlichen eine Vorschauunterstützung für die Datenträgerverschlüsselung Ihres lokalen SSD in Azure Machine Learning Compute. Eröffnen Sie ein technisches Supportticket, um Ihr Abonnement in die Whitelist zu bringen und dieses Feature verwenden zu können.
    + Public Preview von Azure Machine Learning-Batchrückschlüssen. Azure Machine Learning-Batchrückschlüsse sind für große Rückschlussaufträge vorgesehen, die nicht zeitempfindlich sind. Batchrückschlüsse bieten kosteneffiziente Computeskalierung für Rückschlüsse mit beispiellosem Durchsatz für asynchrone Anwendungen. Sie sind für Fire-and-Forget-Rückschlüsse mit hohem Durchsatz für große Datensammlungen optimiert.  
    + [**azureml-contrib-dataset**](https://docs.microsoft.com/python/api/azureml-contrib-dataset)
        + Aktivierte Funktionen für bezeichnete Datasets
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask
        
        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)
        
        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()
        
        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD) 
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)
        
        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **Fehlerbehebungen und Verbesserungen**
  + **azure-cli-ml**
    + Die Befehlszeilenschnittstelle unterstützt nun die Modellverpackung.
    + Dataset-Befehlszeilenschnittstelle wurde hinzugefügt. Weitere Informationen: `az ml dataset --help`
    + Es wurde Unterstützung für das Bereitstellen und Verpacken unterstützter Modelle (ONNX, scikit-learn und TensorFlow) ohne InferenceConfig-Instanz hinzugefügt.
    + Das Überschreibungsflag für die Dienstbereitstellung (ACI und AKS) wurde im SDK und der Befehlszeilenschnittstelle hinzugefügt. Wenn dies angegeben ist, wird der vorhandene Dienst überschrieben, wenn bereits ein Dienst mit dem Namen vorhanden ist. Ist der Dienst nicht vorhanden, wird ein neuer Dienst erstellt.
    + Modelle können mit zwei neuen Frameworks registriert werden, ONNX und TensorFlow. - Die Modellregistrierung akzeptiert Beispieleingabedaten, Beispielausgabedaten und eine Ressourcenkonfiguration für das Modell.
  + **azureml-automl-core**
    + Das Training einer Iteration wurde nur dann in einem untergeordneten Prozess ausgeführt, wenn Laufzeiteinschränkungen festgelegt wurden.
    + Es wurde eine Guardrail für Vorhersageaufgaben hinzugefügt, um zu überprüfen, ob ein angegebener Wert max_horizon auf dem angegebenen Computer ein Arbeitsspeicherproblem verursacht. In diesem Fall wird eine Guardrailmeldung angezeigt.
    + Es wurde Unterstützung für komplexe Häufigkeiten wie 2 Jahre und 1 Monat hinzugefügt. \- Es wurde eine verständliche Fehlermeldung hinzugefügt, wenn die Häufigkeit nicht bestimmt werden kann.
    + azureml-default wurde automatisch generierten Conda-Umgebungen hinzugefügt, um den Fehler bei der Modellbereitstellung zu beheben.
    + Zwischendaten in der Azure Machine Learning-Pipeline können nun in tabellarische Datasets konvertiert und in `AutoMLStep` verwendet werden.
    + Es wurde ein Spaltenzweckupdate für Streaming implementiert.
    + Es wurde ein Transformatorparameterupdate für Imputer und HashOneHotEncoder für das Streaming implementiert.
    + Die aktuelle Datengröße und die mindestens erforderliche Datengröße wurden den Validierungsfehlermeldungen hinzugefügt.
    + Die mindestens erforderliche Datengröße für die Kreuzvalidierung wurde aktualisiert, sodass mindestens zwei Stichproben in den einzelnen Validierungsfolds sicherstellt sind.
  + **azureml-cli-common**
    + Die Befehlszeilenschnittstelle unterstützt nun die Modellverpackung.
    + Modelle können mit zwei neuen Frameworks registriert werden, ONNX und TensorFlow.
    + Die Modellregistrierung akzeptiert Beispieleingabedaten, Beispielausgabedaten und eine Ressourcenkonfiguration für das Modell.
  + **azureml-contrib-gbdt**
    + Der Releasekanal für das Notebook wurde korrigiert.
    + Es wurde eine Warnung für andere Computeziele als AmlCompute hinzugefügt, die nicht unterstützt werden.
    + Dem Paket azureml-contrib-gbdt wurde eine LightGMB-Schätzung hinzugefügt.
  + [**azureml-core**](https://docs.microsoft.com/python/api/azureml-core)
    + Die Befehlszeilenschnittstelle unterstützt nun die Modellverpackung.
    + Es wurde eine Veraltungswarnung für veraltete Dataset-APIs hinzugefügt. Informationen finden Sie im Änderungshinweis zur Dataset-API unter https://aka.ms/tabular-dataset.
    + [`Dataset.get_by_id`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) wurde so geändert, dass beim Registrieren des Datasets der Registrierungsname und die Version zurückgegeben werden.
    + Ein Fehler wurde behoben, bei dem ScriptRunConfig nicht wiederholt mit einem Dataset als Argument zum Übermitteln der Experimentausführung verwendet werden konnte.
    + Während einer Ausführung abgerufene Datasets werden nachverfolgt und können auf der Seite mit den Ausführungsdetails oder durch Aufrufen von [`run.get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--) nach Abschluss der Ausführung angezeigt werden.
    + Zwischendaten in der Azure Machine Learning-Pipeline können nun in tabellarische Datasets konvertiert und in [`AutoMLStep`](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlstep) verwendet werden.
    + Es wurde Unterstützung für das Bereitstellen und Verpacken unterstützter Modelle (ONNX, scikit-learn und TensorFlow) ohne InferenceConfig-Instanz hinzugefügt.
    + Das Überschreibungsflag für die Dienstbereitstellung (ACI und AKS) wurde im SDK und der Befehlszeilenschnittstelle hinzugefügt. Wenn dies angegeben ist, wird der vorhandene Dienst überschrieben, wenn bereits ein Dienst mit dem Namen vorhanden ist. Ist der Dienst nicht vorhanden, wird ein neuer Dienst erstellt.
    +  Modelle können mit zwei neuen Frameworks registriert werden, ONNX und TensorFlow. Die Modellregistrierung akzeptiert Beispieleingabedaten, Beispielausgabedaten und eine Ressourcenkonfiguration für das Modell.
    + Es wurde ein neuer Datenspeicher für Azure Database for MySQL hinzugefügt. Ein Beispiel für die Verwendung von Azure Database for MySQL in DataTransferStep in Azure Machine Learning-Pipelines wurde hinzugefügt.
    + Es wurde zusätzliche Funktionalität zum Hinzufügen und Entfernen von Tags aus Experimenten hinzugefügt. Es wurde Funktionalität zum Entfernen von Tags aus Ausführungen hinzugefügt.
    + Das Überschreibungsflag für die Dienstbereitstellung (ACI und AKS) wurde im SDK und der Befehlszeilenschnittstelle hinzugefügt. Wenn dies angegeben ist, wird der vorhandene Dienst überschrieben, wenn bereits ein Dienst mit dem Namen vorhanden ist. Ist der Dienst nicht vorhanden, wird ein neuer Dienst erstellt.
  + [**azureml-datadrift**](https://docs.microsoft.com/python/api/azureml-datadrift)
    + Wurde von `azureml-contrib-datadrift` in `azureml-datadrift` verschoben
    + Es wurde Unterstützung für die Überwachung von Zeitreihendatasets auf Drift und andere statistische Measures hinzugefügt. 
    + Der [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector(class))-Klasse wurden die neuen Methoden `create_from_model()` und `create_from_dataset()` hinzugefügt. Die `create()`-Methode wird eingestellt. 
    + Anpassungen an den Visualisierungen in Python und der Benutzeroberfläche in Azure Machine Learning Studio.
    + Für Datasetmonitore werden jetzt auch wöchentliche und monatliche Überwachungszeitpläne zusätzlich zu den täglichen unterstützt.
    + Der Abgleich von Datenmonitormetriken zum Analysieren von Verlaufsdaten für Datasetmonitore wird jetzt unterstützt. 
    + Verschiedene Fehlerbehebungen 
  + [**azureml-pipeline-core**](https://docs.microsoft.com/python/api/azureml-pipeline-core)
    + azureml-dataprep ist nicht mehr erforderlich, um eine Azure Machine Learning-Pipelineausführung über die Datei `yaml` der Pipeline zu übermitteln.
  + [**azureml-train-automl**](https://docs.microsoft.com/python/api/azureml-train-automl)
    + azureml-default wurde automatisch generierten Conda-Umgebungen hinzugefügt, um den Fehler bei der Modellbereitstellung zu beheben.
    + Das AutoML-Remotetraining schließt jetzt azureml-defaults ein, um die Wiederverwendung von Trainingsumgebungen für Rückschlüsse zu ermöglichen.
  + **azureml-train-core**
    + In der [`PyTorch`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch)-Schätzung wurde Unterstützung für PyTorch 1.3 hinzugefügt.
  
## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Grafische Benutzeroberfläche (Vorschauversion)

+ Die grafische Benutzeroberfläche von Azure Machine Learning (Vorschauversion) wurde überarbeitet, damit sie in [Azure Machine Learning-Pipelines](concept-ml-pipelines.md) ausgeführt werden kann. Pipelines (früher als Experimente bezeichnet), die auf der grafischen Benutzeroberfläche erstellt werden, sind nun vollständig in die Kernoberfläche von Azure Machine Learning integriert.
  + Vereinheitlichte Verwaltungsoberfläche mit SDK-Ressourcen
  + Versionierung und Nachverfolgung von Modellen für grafische Benutzeroberflächen, Pipelines und Endpunkte 
  + Neu gestaltete Benutzeroberfläche
  + Batchrückschluss-Bereitstellung wurde hinzugefügt.
  + Unterstützung von Azure Kubernetes Service (AKS) für Rückschlüsse von Computezielen wurde hinzugefügt.
  + Neuer Workflow für die Erstellung von Python-Schrittpipelines
  + Neue [Landing Page](https://ml.azure.com) für visuelle Erstellungstools

+ **Neue Module**
  + Apply math operation
  + Apply SQL transformation
  + Clip values
  + Summarize data
  + Import from SQL database  

## <a name="2019-10-14"></a>14.10.2019

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>Azure Machine Learning SDK für Python v1.0.69

+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-automl-core**
    + Beschränkung der Modellerklärungen auf die beste Ausführung, anstatt die Erklärungen für jede Ausführung zu berechnen. Änderung dieses Verhaltens für die lokale Umgebung, Remoteumgebung und ADB.
    + Unterstützung für bedarfsgesteuerte Modellerklärungen für Benutzeroberfläche hinzugefügt
    + psutil wurde als Abhängigkeit von `automl` hinzugefügt, und psutil wurde als Conda-Abhängigkeit in amlcompute eingefügt.
    + Behebung eines Problems mit heuristischen Verzögerungen und Größen von rollierenden Zeitfenstern für die Vorhersagedatasets, von denen einige Serien zu Fehlern in Bezug auf die lineare Algebra führen können
      + Ausdruck für die heuristisch ermittelten Parameter in den Vorhersageläufen hinzugefügt.
  + **[azureml-contrib-datadrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift)**
    + Schutz für die Erstellung von Ausgabemetriken hinzugefügt, falls die Abweichung auf Datasetebene nicht im ersten Abschnitt enthalten ist.
  + **azureml-contrib-interpret**
    + Paket „azureml-contrib-explain-model“ wurde in „azureml-contrib-interpret“ umbenannt
  + **azureml-core**
    + API zum Aufheben der Registrierung von Datasets hinzugefügt. `dataset.unregister_all_versions()`
    + Paket „azureml-contrib-explain-model“ wurde in „azureml-contrib-interpret“ umbenannt.
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core)**
    + API zum Aufheben der Registrierung von Datasets hinzugefügt. dataset.[unregister_all_versions()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Dataset-API zum Überprüfen des Zeitpunkts von Datenänderungen hinzugefügt. `dataset.data_changed_time`.
    + Nutzung von `FileDataset` und `TabularDataset` als Eingaben für `PythonScriptStep`, `EstimatorStep` und `HyperDriveStep` in Azure Machine Learning-Pipeline
    + Leistung von `FileDataset.mount` für Ordner mit einer großen Zahl von Dateien wurde verbessert
    + Neue Möglichkeit, [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset) und [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset) als Eingaben für [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep) und [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) in Azure Machine Learning-Pipeline zu nutzen.
    + Die Leistung von FileDataset.[mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none-) für Ordner mit einer großen Anzahl von Dateien wurde verbessert.
    + URL wurde bekannten Fehlerempfehlungen in den Ausführungsdetails hinzugefügt.
    + Fehler in „run.get_metrics“ behoben, bei dem Anforderungen nicht erfolgreich waren, wenn eine Ausführung zu viele untergeordnete Elemente aufgewiesen hat
    + Ein Fehler in [run.get_metrics](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) wurde behoben, bei dem Anforderungen nicht erfolgreich waren, wenn eine Ausführung zu viele untergeordnete Elemente aufwies.
    + Unterstützung für die Authentifizierung im Arcadia-Cluster hinzugefügt.
    + Beim Erstellen eines Experiment-Objekts wird das Experiment im Azure Machine Learning-Arbeitsbereich für die Nachverfolgung des Ausführungsverlaufs abgerufen bzw. erstellt. Die Experiment-ID und die archivierte Zeit werden bei der Erstellung in das Experimentobjekt eingefügt. Beispiel: experiment = Experiment(workspace, "New Experiment") experiment_id = experiment.id archive() und reactivate() sind Funktionen, die für ein Experiment aufgerufen werden können, um es in der Benutzeroberfläche auszublenden und wiederherzustellen, oder die in einem Aufruf zum Auflisten von Experimenten standardmäßig zurückgegeben werden können. Wenn ein neues Experiment unter dem gleichen Namen als archiviertes Experiment erstellt wird, können Sie das archivierte Experiment bei der Reaktivierung umbenennen, indem Sie einen neuen Namen übergeben. Es kann nur jeweils ein aktives Experiment gleichen Namens vorhanden sein. Beispiel: experiment1 = Experiment(workspace, "Active Experiment") experiment1.archive() # Erstellung eines neuen aktiven Experiments mit dem gleichen Namen wie das archivierte Experiment. experiment2. = Experiment(workspace, "Active Experiment") experiment1.reactivate(new_name="Previous Active Experiment") Für die statische „list()“-Methode für „Experiment“ können ein name-Filter und ein ViewType-Filter verwendet werden. ViewType-Werte sind „ACTIVE_ONLY“, „ARCHIVED_ONLY“ und „ALL“. Beispiel: archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY") all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
    + Unterstützung der Nutzung einer Umgebung für die Modellbereitstellung und Dienstupdate
  + **azureml-datadrift**
    + Das „show“-Attribut der „DataDriftDector“-Klasse verfügt nicht mehr über die Unterstützung des optionalen Arguments „with_details“. Das „show“-Attribut stellt nur den Datendriftkoeffizienten und den Beitrag zur Datenabweichung der Featurespalten dar.
    + Änderungen des Verhaltens von „get_output“ für das DataDriftDetector-Attribut:
      + Eingabeparameter „start_time“ und „end_time“ sind nicht mehr obligatorisch, sondern optional.
      + Die Eingabe der spezifischen Angabe für start_time bzw. end_time mit einer spezifischen run_id im selben Aufruf führt zu einer Wertfehlerausnahme, da sich dies gegenseitig ausschließt. 
      + Wenn eine spezifische Eingabe für „start_time“ bzw. „end_time“ erfolgt, werden nur Ergebnisse von geplanten Ausführungen zurückgegeben. 
      + Der Parameter „daily_latest_only“ ist veraltet.
    + Unterstützung des Abrufs von Datendriftausgaben, die auf Datasets basieren.
  + **azureml-explain-model**
    + Benennt das Paket „AzureML-explain-model“ in „AzureML-interpret“ um. Das alte Paket wird vorerst aus Gründen der Abwärtskompatibilität beibehalten.
    + Der `automl`-Fehler wurde behoben, bei dem unformatierte Erklärungen beim Download von ExplanationClient standardmäßig nicht als Regression, sondern als Klassifizierungsaufgabe festgelegt wurden.
    + Unterstützung hinzugefügt, damit `ScoringExplainer` direkt per `MimicWrapper` erstellt werden kann.
  + **azureml-pipeline-core**
    + Verbesserte Leistung bei Erstellung einer großen Pipeline
  + **azureml-train-core**
    + Unterstützung für TensorFlow 2.0 in TensorFlow Estimator hinzugefügt
  + **azureml-train-automl**
    + Beim Erstellen eines [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment)-Objekts wird das Experiment im Azure Machine Learning-Arbeitsbereich für die Nachverfolgung des Ausführungsverlaufs abgerufen bzw. erstellt. Die Experiment-ID und die archivierte Zeit werden bei der Erstellung in das Experiment-Objekt eingefügt. Beispiel:

        ```py
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [archive()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#archive--) und [reactivate()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) sind Funktionen, die für ein Experiment aufgerufen werden können, um es in der Benutzeroberfläche auszublenden und wiederherzustellen, oder die in einem Aufruf zum Auflisten von Experimenten standardmäßig zurückgegeben werden können. Wenn ein neues Experiment unter dem gleichen Namen als archiviertes Experiment erstellt wird, können Sie das archivierte Experiment bei der Reaktivierung umbenennen, indem Sie einen neuen Namen übergeben. Es kann nur jeweils ein aktives Experiment gleichen Namens vorhanden sein. Beispiel: 
        
        ```py
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        Die statische [list()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly--)-Methode in einem Experiment kann einen Namensfilter und einen ViewType-Filter verwenden. Werte für ViewType sind „ACTIVE_ONLY“, „ARCHIVED_ONLY“ und „ALL“. Beispiel: 
        
        ```py
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Unterstützung der Nutzung einer Umgebung für die Modellbereitstellung und Dienstupdate.
  + **[azureml-datadrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift)**
    + Das show-Attribut der [DataDriftDetector](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector)-Klasse unterstützt nicht mehr das optionale Argument with_details. Das „show“-Attribut stellt nur den Datendriftkoeffizienten und den Beitrag zur Datenabweichung der Featurespalten dar.
    + Änderungen des Verhaltens von [get_output](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector#get-output-start-time--end-time--run-id-none--daily-latest-only-true-) für die DataDriftDetector-Funktion:
      + Eingabeparameter „start_time“ und „end_time“ sind nicht mehr obligatorisch, sondern optional.
      + Die Eingabe der spezifischen Angabe für start_time bzw. end_time mit einer spezifischen run_id im selben Aufruf führt zu einer Wertfehlerausnahme, da sich dies gegenseitig ausschließt. 
      + Wenn eine spezifische Eingabe für „start_time“ bzw. „end_time“ erfolgt, werden nur Ergebnisse von geplanten Ausführungen zurückgegeben. 
      + Der Parameter „daily_latest_only“ ist veraltet.
    + Unterstützung des Abrufs von Datendriftausgaben, die auf Datasets basieren.
  + **[azureml-explain-model](https://docs.microsoft.com/python/api/azureml-explain-model)**
    + Das Paket „AzureML-explain-model“ wurde in „AzureML-interpret“ umbenannt. Das alte Paket wird vorerst aus Gründen der Abwärtskompatibilität beibehalten.
    + Der automl-Fehler wurde behoben, bei dem unformatierte Erklärungen beim Download von ExplanationClient standardmäßig nicht als Regression, sondern als Klassifizierungsaufgabe festgelegt wurden.
    + Unterstützung für die direkte Erstellung von [ScoringExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.scoring.scoring_explainer.scoringexplainer) mit [MimicWrapper](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic_wrapper.mimicwrapper) wurde hinzugefügt.
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Die Leistung beim Erstellen einer großen Pipeline wurde verbessert.
  + **[azureml-train-core](https://docs.microsoft.com/python/api/azureml-train-core)**
    + Unterstützung für TensorFlow 2.0 in der [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow)-Schätzung wurde hinzugefügt.
  + **[azureml-train-automl](https://docs.microsoft.com/python/api/azureml-train-automl)**
    + Für die übergeordnete Ausführung tritt kein Fehler mehr auf, wenn die Setupiteration fehlschlägt, weil dies von der Orchestrierung übernommen wird.
    + Unterstützung von „local-docker“ und „local-conda“ für AutoML-Experimente hinzugefügt.
    + Unterstützung von local-docker und local-conda für AutoML-Experimente wurde hinzugefügt.


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Neue Weboberfläche (Vorschau) für Azure Machine Learning-Arbeitsbereiche

Die Registerkarte „Experiment“ im [Portal für neue Arbeitsbereiche](https://ml.azure.com) wurde aktualisiert, damit Datenanalysten Experimente auf leistungsfähigere Weise überwachen können. Sie können die folgenden Features erkunden:
+ Experimentmetadaten zum einfachen Filtern und Sortieren der Liste Ihrer Experimente
+ Vereinfachte und leistungsfähige Seiten für Experimentdetails, mit denen Sie Ihre Ausführungen visualisieren und vergleichen können
+ Neues Design zum Ausführen von Detailseiten zum Verstehen und Überwachen von Trainingsausführungen

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>Azure Machine Learning SDK für Python 1.0.65

  + **Neue Features**
    + Es wurden zusammengestellte Umgebungen hinzugefügt. Diese Umgebungen wurden mit Bibliotheken für gängige Aufgaben für maschinelles Lernen vorkonfiguriert und zur schnelleren Ausführung vorab als Docker-Images erstellt und zwischengespeichert. Sie werden standardmäßig in der Liste der Umgebungen des Arbeitsbereichs mit dem Präfix „AzureML“ angezeigt.
    + Es wurden zusammengestellte Umgebungen hinzugefügt. Diese Umgebungen wurden mit Bibliotheken für gängige Aufgaben für maschinelles Lernen vorkonfiguriert und zur schnelleren Ausführung vorab als Docker-Images erstellt und zwischengespeichert. Sie werden standardmäßig in der Liste der Umgebungen des [Arbeitsbereichs](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace%28class%29) mit dem Präfix „AzureML“ angezeigt.
  
  + **azureml-train-automl**
  + **[azureml-train-automl](https://docs.microsoft.com/python/api/azureml-train-automl)**
    + Unterstützung für die ONNX-Konvertierung für ADB und HDI wurde hinzugefügt.

+ **Vorschaufunktionen**  
  + **azureml-train-automl**
  + **[azureml-train-automl](https://docs.microsoft.com/python/api/azureml-train-automl)**
    + BERT und BiLSTM werden jetzt als Textfeaturanpassungen unterstützt (nur Vorschauversion).
    + Featureanpassung für Parameter für Spaltenzweck und Transformationsparameter wird jetzt unterstützt (nur Vorschauversion).
    + Unformatierte Erklärungen werden jetzt unterstützt, wenn der Benutzer während des Trainings die Modellerklärung aktiviert (nur Vorschauversion).
    + Prophet wurde für `timeseries`-Prognosen als trainierbare Pipeline hinzugefügt (nur Vorschauversion).
  
  + **azureml-contrib-datadrift**
    + Pakete wurden von azureml-contrib-datadrift nach azureml-datadrift verschoben. Das Paket `contrib` wird in einem zukünftigen Release entfernt. 

+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-automl-core**
    + FeaturizationConfig wurde in AutoMLConfig und AutoMLBaseSettings eingeführt.
    + FeaturizationConfig wurde in [AutoMLConfig](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig) und AutoMLBaseSettings eingeführt.
      + Spaltenzweck kann für Featureeinordnung mit angegebenen Spalten- und Featuretypen außer Kraft gesetzt werden.
      + Transformationsparameter können außer Kraft gesetzt werden.
    + Für explain_model() und retrieve_model_explanations() wurde ein Hinweis zur Veraltung hinzugefügt.
    + Prophet wurde als trainingsfähige Pipeline hinzugefügt (nur Vorschauversion).
    + Für explain_model() und retrieve_model_explanations() wurde ein Hinweis zur Veraltung hinzugefügt.
    + Prophet wurde als trainierbare Pipeline hinzugefügt (nur Vorschauversion).
    + Unterstützung für die automatische Erkennung von Zielverzögerungen, der Größe rollierender Fenster und des maximalen Horizonts wurde hinzugefügt. Wenn einer der Werte target_lags, target_rolling_window_size oder max_horizon auf automatisch festgelegt ist, wird die Heuristik zum Abschätzen des Werts des entsprechenden Parameters basierend auf den Trainingsdaten angewandt.
    + Die Vorhersage wurde für den Fall korrigiert, dass das Dataset eine Spalte vom Typ „Körnung“ enthält, diese einen numerischen Typ aufweist und eine Lücke zwischen dem Trainings- und dem Testsatz besteht.
    + Die Fehlermeldung über den duplizierten Index in der Remoteausführung in Prognoseaufgaben wurde korrigiert.
    + Vorhersagen wurden für den Fall korrigiert, dass das Dataset eine Körnungsspalte enthält, diese einen numerischen Typ aufweist und zwischen dem Trainings- und dem Testsatz eine Lücke besteht.
    + Die Fehlermeldung über den duplizierten Index in der Remoteausführung in Prognoseaufgaben wurde korrigiert.
    + Eine Guardrail wurde zur Überprüfung hinzugefügt, ob ein Dataset unausgeglichen ist. Wenn dies der Fall ist, wird eine Guardrailnachricht in die Konsole geschrieben.
  + **azureml-core**
    + Die Möglichkeit zum Abrufen der SAS-URL in das Modell im Speicher über das Modellobjekt wurde hinzugefügt. Beispiel: model.get_sas_url()
    + `run.get_details()['datasets']` wurde eingeführt, um mit der übermittelten Ausführung verknüpfte Datasets abzurufen.
    + Die API `Dataset.Tabular.from_json_lines_files` wurde hinzugefügt, um ein TabularDataset aus JSON-Lines-Dateien zu erstellen. Weitere Informationen zu diesen Tabellendaten in den JSON-Lines-Dateien für TabularDataset finden Sie in der Dokumentation unter https://aka.ms/azureml-data.
    + Der supported_vmsizes()-Funktion wurden zusätzliche VM-Größenfelder (Betriebssystemdatenträger, Anzahl von GPUs) hinzugefügt.
    + Der list_nodes()-Funktion wurden zusätzliche Felder hinzugefügt, um die Ausführung, die private und die öffentliche IP-Adresse, den Port usw. anzuzeigen.
    + Die Möglichkeit, bei der Clusterbereitstellung ein neues Feld über --remotelogin_port_public_access anzugeben, wurde hinzugefügt. Dieses kann je nachdem, ob der SSH-Port zum Zeitpunkt der Clustererstellung geöffnet oder geschlossen sein soll, aktiviert oder deaktiviert werden. Wenn keine Angabe erfolgt, öffnet oder schließt der Dienst den Port intelligent in Abhängigkeit davon, ob Sie den Cluster in einem VNET bereitstellen.
  + **azureml-explain-model**
  + **[azureml-core](https://docs.microsoft.com/python/api/azureml-core/azureml.core)**
    + Die Möglichkeit zum Abrufen der SAS-URL in das Modell im Speicher über das Modellobjekt wurde hinzugefügt. Beispiel: model.[get_sas_url()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + run.[get_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29#get-details--)['datasets'] wurde eingeführt, um mit der übermittelten Ausführung verknüpfte Datasets abzurufen.
    + Die API `Dataset.Tabular`.[from_json_lines_files()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) wurde hinzugefügt, um ein TabularDataset aus JSON-Lines-Dateien zu erstellen. Weitere Informationen zu diesen Tabellendaten in den JSON-Lines-Dateien für TabularDataset finden Sie in der Dokumentation unter https://aka.ms/azureml-data.
    + Der [supported_vmsizes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-)-Funktion wurden zusätzliche VM-Größenfelder (Betriebssystemdatenträger, Anzahl von GPUs) hinzugefügt.
    + Der [list_nodes()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--)-Funktion wurden zusätzliche Felder hinzugefügt, um die Ausführung, die private und die öffentliche IP-Adresse, den Port usw. anzuzeigen.
    + Die Möglichkeit, bei der [Clusterbereitstellung](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) das neue Feld `--remotelogin_port_public_access` anzugeben, wurde hinzugefügt. Dieses kann je nachdem, ob der SSH-Port zum Zeitpunkt der Clustererstellung geöffnet oder geschlossen sein soll, aktiviert oder deaktiviert werden. Wenn keine Angabe erfolgt, öffnet oder schließt der Dienst den Port intelligent in Abhängigkeit davon, ob Sie den Cluster in einem VNET bereitstellen.
  + **[azureml-explain-model](https://docs.microsoft.com/python/api/azureml-explain-model)**
    + Die Dokumentation für Erklärungsausgaben im Klassifizierungsszenario wurde verbessert.
    + Die Möglichkeit, die vorhergesagten y-Werte in der Erklärung für die Evaluierungsbeispiele hochzuladen, wurde hinzugefügt. Weitere nützliche Visualisierungen wurden entsperrt.
    + MimicWrapper wurde eine Erklärungseigenschaft hinzugefügt, um den zugrunde liegenden MimicExplainer-Wert abzurufen.
  + **azureml-pipeline-core**
    + Ein Notebook zum Beschreiben von Module, ModuleVersion und ModuleStep wurde hinzugefügt.
  + **azureml-pipeline-steps**
    + RScriptStep zur Unterstützung von R-Skript-Ausführungen über die AML-Pipeline wurde hinzugefügt.
    + Die Analyse von Metadatenparametern in AzureBatchStep wurde korrigiert. Dies war die Ursache der Fehlermeldung über eine nicht angegebene Zuweisung für den SubscriptionId-Parameter.
  + **azureml-train-automl**
    + training_data, validation_data, label_column_name und weight_column_name werden jetzt als Format für die Dateneingabe unterstützt.
    + Für explain_model() und retrieve_model_explanations() wurde ein Hinweis zur Veraltung hinzugefügt.
  + **[azureml-pipeline-core](https://docs.microsoft.com/python/api/azureml-pipeline-core)**
    + Ein [Notebook](https://aka.ms/pl-modulestep) zum Beschreiben von [Module](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.module(class)), [ModuleVersion](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.moduleversion) und [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep) wurde hinzugefügt.
  + **[azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps)**
    + [RScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) zur Unterstützung von R-Skript-Ausführungen über die AML-Pipeline wurde hinzugefügt.
    + Die Analyse von Metadatenparametern in [AzureBatchStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.azurebatchstep) wurde korrigiert. Dies war die Ursache der Fehlermeldung über eine nicht angegebene Zuweisung für den SubscriptionId-Parameter.
  + **[azureml-train-automl](https://docs.microsoft.com/python/api/azureml-train-automl)**
    + training_data, validation_data, label_column_name und weight_column_name werden jetzt als Format für die Dateneingabe unterstützt.
    + Für [explain_model()](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) und [retrieve_model_explanations()](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlexplainer#retrieve-model-explanation-child-run-) wurde ein Hinweis zur Veraltung hinzugefügt.

  
## <a name="2019-09-16"></a>16.09.2019

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>Azure Machine Learning SDK für Python v1.0.62

+ **Neue Features**
  + Das Merkmal `timeseries` für TabularDataset wurde eingeführt. Dieses Merkmal ermöglicht eine einfache Zeitstempelfilterung für Daten in einem TabularDataset, wie z. B. das Verwenden aller Daten aus einem Zeitbereich oder der neuesten Daten. Weitere Informationen zum Merkmal `timeseries` für TabularDataset erhalten Sie in der Dokumentation unter https://aka.ms/azureml-data oder im Beispielnotebook unter https://aka.ms/azureml-tsd-notebook. 
  + Training mit TabularDataset und FileDataset aktiviert. Unter https://aka.ms/dataset-tutorial finden Sie ein Beispielnotebook. 
  
  + **azureml-train-core**
    + In der PyTorch-Schätzung wurde die Unterstützung von `Nccl` und `Gloo` hinzugefügt.
  
+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-automl-core**
    + AutoML-Einstellung „lag_length“ und LaggingTransformer sind veraltet.
    + Validierung der Eingabedaten korrigiert, wenn sie in einem Dataflow-Format angegeben sind.
    + „it_pipeline.py“ so geändert, dass der JSON-Code für den Graphen generiert und in Artefakte hochgeladen wird. 
    + Das Diagramm unter `userrun` wurde mithilfe von `Cytoscape` gerendert.
  + **azureml-core**
    + Überprüfen Sie die Ausnahmebehandlung im ADB-Code, und nehmen Sie Änderungen gemäß der neuen Fehlerbehandlung vor.
    + Automatische MSI-Authentifizierung für Notebook-VMs hinzugefügt.
    + Fehler korrigiert, aufgrund dessen beschädigte oder leere Modelle aufgrund fehlgeschlagener Wiederholungen hochgeladen werden konnten.
    + Fehler korrigiert, bei dem sich der `DataReference`-Name ändert, wenn sich der `DataReference`-Modus ändert (z. B. beim Aufrufen von `as_upload`, `as_download` oder `as_mount`).
    + `mount_point` und `target_path` für `FileDataset.mount` und `FileDataset.download` als optional festgelegt.
    + Ausnahme bei Nichtauffindbarkeit der Zeitstempelspalte wird ausgelöst, wenn die auf Zeitreihen bezogene API ohne Zuweisung einer differenzierten Zeitstempelspalte aufgerufen wird oder die zugewiesenen Zeitstempelspalten gelöscht werden.
    + Zeitserienspalten müssen einer Spalte zugewiesen werden, deren Typ „Datum“ ist, sonst wird eine Ausnahme erwartet.
    + Zeitreihenspalten, die die API „with_timestamp_columns“ zuweisen, können den Wert „None“ bei einem differenzierten/undifferenzierten Namen einer Zeitstempelspalte annehmen, wodurch zuvor zugewiesene Zeitstempelspalten gelöscht werden.
    + Eine Ausnahme wird ausgelöst, wenn entweder eine differenzierte oder undifferenzierte Zeitstempelspalte mit dem Hinweis für den Benutzer gelöscht wird, dass das Löschen entweder nach dem Ausschließen der Zeitstempelspalte aus der Löschliste oder dem Aufruf von with_time_stamp mit dem Wert „None“ zur Freigabe von Zeitstempelspalten erfolgen kann.
    + Eine Ausnahme wird ausgelöst, wenn entweder eine differenzierte oder undifferenzierte Zeitstempelspalte mit dem Hinweis nicht der Liste zur Beibehaltung von Spalten hinzugefügt wird, dass das Beibehalten entweder nach dem Ausschließen der Zeitstempelspalte aus der Liste zur Beibehaltung von Spalten oder dem Aufruf von with_time_stamp mit dem Wert „None“ zur Freigabe von Zeitstempelspalten erfolgen kann.
    + Protokollierung für die Größe eines registrierten Modells hinzugefügt.
  + **azureml-explain-model**
    + In der Konsole ausgegebene Warnung korrigiert, wenn das Python-Paket „packaging“ nicht installiert ist: „Sie verwenden eine ältere als die unterstützte Version von lightgbm. Aktualisieren Sie auf eine Version höher als 2.2.1.“
    + Erklärung zum Downloadmodell mit Sharding für globale Erklärungen mit vielen Features korrigiert.
    + Fehlende Initialisierungsbeispiele zur Erläuterung der Ausgabe von Mimic Explainer korrigiert.
    + Unveränderlicher Fehler bei festgelegten Eigenschaften korrigiert, wenn beim Hochladen mit dem Erklärungsclient zwei verschiedene Arten von Modellen verwendet wurden.
    + get_raw-Parameter zu Bewertungs-Explainer .explain() hinzugefügt, sodass ein Bewertungs-Explainer sowohl bearbeitete als auch Rohwerte zurückgeben kann.
  + **azureml-train-automl**
    + Öffentliche APIs aus AutoML wurden zur Unterstützung von Erklärungen aus dem `automl` Explain SDK eingeführt. - Neuere Art der Unterstützung von AutoML-Erklärungen durch Entkopplung der AutoML-Featurisierung und Explain SDK. - Integrierte Roherklärungsunterstützung aus dem Azure ML Explain SDK für AutoML-Modelle.
    + azureml-defaults aus Remotetrainingsumgebungen entfernt.
    + Der Standardspeicherort des Caches von FileCacheStore wurde basierend auf AzureFileCacheStore geändert, um AutoML im Azure Databricks-Codepfad zu ermöglichen.
    + Validierung der Eingabedaten korrigiert, wenn sie in einem Dataflow-Format angegeben sind.
  + **azureml-train-core**
    + Veraltung von source_directory_data_store aufgehoben.
    + Möglichkeit zum Überschreiben installierter azureml-Paketversionen hinzugefügt. 
    + Dockerfile-Unterstützung im `environment_definition`-parameter in Estimators hinzugefügt.
    + Verteilte Trainingsparameter in Estimators vereinfacht.

         ```py 
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer 
        ```

## <a name="2019-09-09"></a>09.09.2019

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Neue Weboberfläche (Vorschau) für Azure Machine Learning-Arbeitsbereiche 
Die neue Weboberfläche ermöglicht Data Scientists und Datentechnikern, ihren End-to-End-Lebenszyklus für Machine Learning vom Vorbereiten und Visualisieren von Daten bis hin zu Trainings- und Bereitstellungsmodellen an einem einzigen Ort abzuschließen. 

![Benutzeroberfläche des Azure Machine Learning-Arbeitsbereichs (Vorschau)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Wichtige Features:**

Mit dieser neuen Azure Machine Learning-Benutzeroberfläche können Sie nun folgende Aktionen ausführen:
+ Verwalten Ihrer Notebooks oder Verknüpfen mit Jupyter
+ [Ausführen automatisierter ML-Experimente](tutorial-first-experiment-automated-ml.md)
+ [Erstellen von Datasets aus lokalen Dateien, Datenspeichern und Webdateien](how-to-create-register-datasets.md)
+ Untersuchen und Vorbereiten von Datasets für die Modellerstellung
+ Überwachen der Datenabweichung für Ihre Modelle 
+ Anzeigen aktueller Ressourcen aus einem Dashboard

Zum Zeitpunkt dieses Releases werden die folgenden Browser unterstützt: Chrome, Firefox, Safari und Microsoft Edge (Vorschau).

**Bekannte Probleme:**

1. Aktualisieren Sie Ihren Browser, wenn die Meldung „Leider ist ein Fehler aufgetreten. Fehler beim Laden von Blockdateien“ angezeigt wird, wenn die Bereitstellung ausgeführt wird.  

1. Die Datei kann in Notebooks und Dateien nicht gelöscht oder umbenannt werden. Während der Public Preview können Sie die Jupyter-Benutzeroberfläche oder das Terminal in der Notebook-VM verwenden, um Aktualisierungsdateivorgänge auszuführen. Da es sich um ein eingebundenes Netzwerkdateisystem handelt, spiegeln sich alle Änderungen, die Sie an der Notebook-VM vornehmen, sofort im Notebook-Arbeitsbereich wider. 

1. So stellen Sie eine SSH-Verbindung mit der Notebook-VM her:
   1. Suchen Sie die SSH-Schlüssel, die während der VM-Einrichtung erstellt wurden. Alternativ können Sie die Schlüssel im Azure ML-Azure-Portal suchen: Öffnen Sie die Registerkarte „Compute“, suchen Sie die Notebook-VM in der Liste, öffnen Sie ihre Eigenschaften, und kopieren Sie die Schlüssel aus dem Dialogfeld.
   1. Importieren Sie diese öffentlichen und privaten SSH-Schlüssel auf Ihren lokalen Computer.
   1. Verwenden Sie sie, um eine SSH-Verbindung mit der Notebook-VM herzustellen. 

## <a name="2019-09-03"></a>03.09.2019
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>Azure Machine Learning SDK für Python v1.0.60

+ **Neue Features**
  + FileDataset wurde eingeführt, das auf einzelne oder mehrere Dateien in Ihren Datenspeichern oder öffentlichen URLs verweist. Die Dateien können ein beliebiges Format aufweisen. Mit FileDataset haben Sie die Möglichkeit, die Dateien herunterzuladen oder in Ihrer Computeinstanz bereitzustellen. Weitere Informationen zu FileDataset finden Sie unter https://aka.ms/file-dataset.
  + Hinzugefügte Pipeline-YAML-Unterstützung für PythonScript Step, Adla Step, Databricks Step, DataTransferStep und AzureBatch Step

+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-automl-core**
    + AutoArima ist jetzt eine vorschlagbare Pipeline nur für die Vorschau.
    + Verbesserte Fehlerberichte für Vorhersagen.
    + Verbesserte Protokollierung durch Verwendung von benutzerdefinierten Ausnahmen anstelle von generischen Ausnahmen in den Vorhersagetasks.
    + Die Überprüfung wurde entfernt, dass max_concurrent_iterations kleiner als die Gesamtzahl von Iterationen ist.
    + AutoML-Modelle geben jetzt AutoMLExceptions zurück.
    + Dieses Release verbessert die Ausführungsleistung automatisierter lokaler Machine Learning-Ausführungen.
  + **azureml-core**
    + Dataset.get_all(workspace) wurde eingeführt und gibt ein Wörterbuch von `TabularDataset`- und `FileDataset`-Objekten zurück, die als Schlüssel ihren Registrierungsnamen verwenden. 
    
    ```py 
    workspace = Workspace.from_config() 
    all_datasets = Dataset.get_all(workspace) 
    mydata = all_datasets['my-data'] 
    ```
    
    + `parition_format` wurde als Argument für `Dataset.Tabular.from_delimited_files` und `Dataset.Tabular.from_parquet.files` eingeführt. Die Partitionsinformationen für die einzelnen Datenpfade werden basierend auf dem angegebenen Format in Spalten extrahiert. „{column_name}“ erstellt eine Zeichenfolgenspalte, und „{column_name:yyyyyy/MM/dd/HH/mm/ss}' erstellt eine datetime-Spalte, wobei „yyyy“, „MM“, „dd“, „HH“, „mm“ und „ss“ verwendet werden, um Jahr, Monat, Tag, Stunde, Minute und Sekunde für den datetime-Typ zu extrahieren. Das partition_format sollte an der Position des ersten Partitionsschlüssels beginnen und bis zum Ende des Dateipfads reichen. Beispielpfad: „../USA/2019/01/01/data.csv“. Dabei wird die Partition durch das Land und das Datum festgelegt, partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' erstellt die Zeichenfolgenspalte „Country“ mit dem Wert „USA“ und die datetime-Spalte „PartitionDate“ mit dem Wert „2019-01-01“.
        ```py 
        workspace = Workspace.from_config() 
        all_datasets = Dataset.get_all(workspace) 
        mydata = all_datasets['my-data'] 
        ```
        
    + `partition_format` wurde als Argument für `Dataset.Tabular.from_delimited_files` und `Dataset.Tabular.from_parquet.files` eingeführt. Die Partitionsinformationen für die einzelnen Datenpfade werden basierend auf dem angegebenen Format in Spalten extrahiert. „{column_name}“ erstellt eine Zeichenfolgenspalte, und „{column_name:yyyyyy/MM/dd/HH/mm/ss}' erstellt eine datetime-Spalte, wobei „yyyy“, „MM“, „dd“, „HH“, „mm“ und „ss“ verwendet werden, um Jahr, Monat, Tag, Stunde, Minute und Sekunde für den datetime-Typ zu extrahieren. Das partition_format sollte an der Position des ersten Partitionsschlüssels beginnen und bis zum Ende des Dateipfads reichen. Beispielpfad: „../USA/2019/01/01/data.csv“. Dabei wird die Partition durch das Land und das Datum festgelegt, partition_format='/{Country}/{PartitionDate:yyyy/MM/dd}/data.csv' erstellt die Zeichenfolgenspalte „Country“ mit dem Wert „USA“ und die datetime-Spalte „PartitionDate“ mit dem Wert „2019-01-01“.
    + Die Methoden `to_csv_files` und `to_parquet_files` wurden `TabularDataset` hinzugefügt. Diese Methoden ermöglichen die Konvertierung zwischen `TabularDataset` und `FileDataset`, indem die Daten in Dateien des angegebenen Formats konvertiert werden.
    + Automatische Anmeldung bei der Basisimageregistrierung beim Speichern der von Model.package() erstellten Dockerfile-Datei.
    + „gpu_support“ ist nicht mehr erforderlich. AzureML erkennt und verwendet nun automatisch die Nvidia-Docker-Erweiterung, wenn sie verfügbar ist. Dieses Element wird in einer späteren Version entfernt.
    + Unterstützung für das Erstellen, Aktualisieren und Verwenden von PipelineDrafts wurde hinzugefügt.
    + Dieses Release verbessert die Ausführungsleistung automatisierter lokaler Machine Learning-Ausführungen.
    + Benutzer können Metriken aus dem Ausführungsverlauf nach Namen abfragen.
    + Verbesserte Protokollierung durch Verwendung von benutzerdefinierten Ausnahmen anstelle von generischen Ausnahmen in den Vorhersagetasks.
  + **azureml-explain-model**
    + Dem neuen MimicWrapper wurde der feature_maps-Parameter hinzugefügt, sodass Benutzer unformatierte Merkmalserklärungen abrufen können.
    + Datasetuploads sind jetzt standardmäßig für den Erläuterungsupload deaktiviert und können mit upload_datasets=True erneut aktiviert werden.
    + Is_law-Filterparameter wurden der Erläuterungsliste und Downloadfunktionen hinzugefügt.
    + `get_raw_explanation(feature_maps)`-Methode zu globalen und lokalen Erläuterungsobjekten hinzugefügt.
    + Versionsüberprüfung zu lightgbm mit ausgegebener Warnung hinzugefügt, wenn die Version kleiner als die unterstützte Version ist.
    + Optimierte Speichernutzung bei der Batchverarbeitung von Erläuterungen
    + AutoML-Modelle geben jetzt AutoMLExceptions zurück.
  + **azureml-pipeline-core**
    + Unterstützung für das Erstellen, Aktualisieren und Verwenden von PipelineDrafts wurde hinzugefügt. Kann verwendet werden, um änderbare Pipelinedefinitionen zu verwalten und diese interaktiv für die Ausführung zu verwenden.
  + **azureml-train-automl**
    + Ein Feature zum Installieren spezifischer Versionen von GPU-fähigen PyTorch v1.1.0, :::no-loc text="cuda"::: Toolkit 9.0, pytorch-transformers wurde erstellt, das zum Aktivieren von BERT/XLNet in der Python-Remote-Laufzeitumgebung erforderlich ist.
  + **azureml-train-core**
    + Früher Ausfall einiger Hyperparameterraum-Definitionsfehler direkt im SDK anstelle auf der Serverseite.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>Azure Machine Learning Data Prep SDK, Version 1.1.14
+ **Fehlerbehebungen und Verbesserungen**
  + Das Schreiben in ADLS/ADLSGen2 wurde mit dem RAW-Pfad und den Anmeldeinformationen aktiviert.
  + Es wurde ein Fehler behoben, der dazu führte, dass `include_path=True` nicht für `read_parquet` funktionierte.
  + Ein `to_pandas_dataframe()`-Fehler aufgrund der Ausnahme „Ungültiger Eigenschaftswert: hostSecret“ wurde korrigiert.
  + Es wurde ein Fehler behoben, bei dem Dateien nicht im Spark-Modus auf DBFS gelesen werden konnten.
  
## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>Azure Machine Learning SDK für Python, Version 1.0.57
+ **Neue Features**
  + `TabularDataset` wurde aktiviert und kann mit AutomatedML genutzt werden. Weitere Informationen zu `TabularDataset` finden Sie unter https://aka.ms/azureml/howto/createdatasets.
  
+ **Fehlerbehebungen und Verbesserungen**
  + **automl-client-core-nativeclient**
    + Der Fehler wurde behoben, der auftrat, wenn Trainings- oder Validierungsbezeichnungen („y“ bzw. „y_valid“) in Form eines Pandas-Datenrahmens, jedoch nicht als NumPy-Array angegeben werden.
    + Die Schnittstelle zum Erstellen eines `RawDataContext`-Objekts wurde aktualisiert, sodass nur die Daten und das `AutoMLBaseSettings`-Objekt erforderlich sind.
    +  AutoML-Benutzer können nun Trainingsreihen löschen, die bei der Prognose nicht lang genug sind. – AutoML-Benutzer können nun Intervalle aus dem Testsatz löschen, die bei der Prognose nicht im Trainingssatz vorhanden sind.
  + **azure-cli-ml**
    + Sie können jetzt das SSL-Zertifikat für den im AKS-Cluster bereitgestellten Bewertungsendpunkt aktualisieren. Dies gilt sowohl für von Microsoft generierte Zertifikate als auch für Kundenzertifikate.
  + **azureml-automl-core**
    + Ein Problem in AutoML wurde behoben, bei dem Zeilen mit fehlenden Bezeichnungen nicht ordnungsgemäß entfernt wurden.
    + Die Fehlerprotokollierung in AutoML wurde verbessert: In die Protokolldatei werden nun immer vollständige Fehlermeldungen geschrieben.
    + Die Paketanheftung in AutoML wurde aktualisiert und umfasst nun `azureml-defaults`, `azureml-explain-model` und `azureml-dataprep`. In AutoML werden bei Paketkonflikten keine Warnungen mehr angezeigt (außer für das Paket `azureml-train-automl`).
    + Ein Problem in `timeseries` wurde behoben, bei dem Kreuzvalidierungsaufteilungen mit unterschiedlicher Größe zu Fehlern bei der binären Berechnung führten.
    + Wenn beim Ausführen der Ensembleiteration für den Trainingstyp der Kreuzvalidierung Fehler beim Herunterladen der Modelle auftraten, die für das gesamte Dataset trainiert wurden, bestand eine Inkonsistenz zwischen den Modellgewichtungen und den Modellen, die in das Abstimmungsensemble aufgenommen wurden.
    + Der Fehler wurde behoben, der auftrat, wenn Trainings- oder Validierungsbezeichnungen („y“ bzw. „y_valid“) in Form eines Pandas-Datenrahmens, jedoch nicht als NumPy-Array angegeben werden.
    + Das Problem wurde behoben, das bei den Vorhersageaufgaben auftrat, wenn in den booleschen Spalten von Eingabetabellen der Wert „Kein“ gefunden wurde.
    + AutoML-Benutzer können nun Trainingsreihen löschen, die bei der Prognose nicht lang genug sind. – AutoML-Benutzer können nun Intervalle aus dem Testsatz löschen, die bei der Prognose nicht im Trainingssatz vorhanden sind.
  + **azureml-core**
    + Ein Problem bei der Reihenfolge des Parameters „blob_cache_timeout“ wurde behoben.
    + Den Systemfehlern wurden externe Ausnahmetypen für die Anpassung und Transformation hinzugefügt.
    + Unterstützung für Key Vault-Geheimnisse für Remoteausführungen wurde hinzugefügt. Nun kann eine „azureml.core.keyvault.Keyvault“-Klasse hinzugefügt werden, um Geheimnisse aus dem mit Ihrem Arbeitsbereich verknüpften Schlüsseltresor hinzuzufügen, abzurufen und aufzulisten. Folgende Vorgänge werden unterstützt:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.keyvault.Keyvault.set_secret(name, value)
      + azureml.core.keyvault.Keyvault.set_secrets(secrets_dict)
      + azureml.core.keyvault.Keyvault.get_secret(name)
      + azureml.core.keyvault.Keyvault.get_secrets(secrets_list)
      + azureml.core.keyvault.Keyvault.list_secrets()
    + Weitere Methoden zum Abrufen des Standardschlüsseltresors und von Geheimnissen während der Remoteausführung:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.run.Run.get_secret(name)
      + azureml.core.run.Run.get_secrets(secrets_list)
    + Dem CLI-Befehl „submit-hyperdrive“ wurden weitere Überschreibungsparameter hinzugefügt.
    + Die Zuverlässigkeit von API-Aufrufen wurde durch Erweitern der Wiederholungsversuche auf häufige Ausnahmen von Bibliotheksanforderungen erhöht.
    + Unterstützung für das Übermitteln von Ausführungen über eine übermittelte Ausführung wurde hinzugefügt.
    + Ein Problem beim Ablauf des SAS-Tokens in FileWatcher wurde behoben, bei dem Dateien nicht mehr hochgeladen wurden, nachdem das ursprüngliche Token abgelaufen war.
    + Unterstützung für den Import von HTTP-CSV/TSV-Dateien in Datasets mit dem Python SDK wurde hinzugefügt.
    + Die Workspace.setup()-Methode wurde als veraltet markiert. In einer Warnmeldung wird Benutzern stattdessen die Verwendung von create() oder get()/from_config() empfohlen.
    + Die Environment.add_private_pip_wheel()-Methode wurde hinzugefügt, mit der private benutzerdefinierte Python-Pakete (`whl`) in den Arbeitsbereich hochgeladen und zum Erstellen und Materialisieren der Umgebung verwendet werden können.
    + Sie können jetzt das SSL-Zertifikat für den im AKS-Cluster bereitgestellten Bewertungsendpunkt aktualisieren. Dies gilt sowohl für von Microsoft generierte Zertifikate als auch für Kundenzertifikate.
  + **azureml-explain-model**
    + Ein Parameter wurde hinzugefügt, über den den Erläuterungen zum Hochladen eine Modell-ID hinzugefügt werden kann.
    + Den Erläuterungen im Speicher und beim Hochladen wurde `is_raw`-Tagging hinzugefügt.
    + PyTorch-Unterstützung und Tests für das Paket „azureml-explain-model“ wurden hinzugefügt.
  + **azureml-opendatasets**
    + Erkennung und Protokollierung der automatischen Testumgebung werden nun unterstützt.
    + Klassen zum Abrufen der US-amerikanischen Bevölkerung nach County und Postleitzahl wurden hinzugefügt.
  + **azureml-pipeline-core**
    + Den Definitionen für den Eingabe- und den Ausgabeport wurde die label-Eigenschaft hinzugefügt.
  + **azureml-telemetry**
    + Eine fehlerhafte Telemetriekonfiguration wurde korrigiert.
  + **azureml-train-automl**
    + Das Problem wurde behoben, bei dem bei Setupfehlern der Fehler nicht im Feld „errors“ für die Setupausführung protokolliert und daher nicht im Feld „errors“ der übergeordneten Ausführung gespeichert wurde.
    + Ein Problem in AutoML wurde behoben, bei dem Zeilen mit fehlenden Bezeichnungen nicht ordnungsgemäß entfernt wurden.
    + AutoML-Benutzer können nun Trainingsreihen löschen, die bei der Prognose nicht lang genug sind.
    + AutoML-Benutzer können nun Intervalle aus dem Trainingssatz löschen, die bei der Prognose im Trainingssatz nicht vorhanden sind.
    + AutoMLStep durchläuft nun die `automl`-Konfiguration bis zum Back-End, um Probleme bei Änderungen oder Ergänzungen neuer Konfigurationsparameter zu vermeiden.
    + AutoML Data GuardRail befindet sich jetzt in der öffentlichen Vorschau. Dem Benutzer wird nach dem Training ein Data GuardRail-Bericht (für Klassifizierungs-/Regressionsaufgaben) angezeigt, und er kann darauf auch über die SDK-API zugreifen.
  + **azureml-train-core**
    + In PyTorch wurde die Unterstützung von Torch 1.2 hinzugefügt.
  + **azureml-widgets**
    + Konfusionsmatrixdiagramme für das Klassifizierungstraining wurden verbessert.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>Azure Machine Learning Data Prep SDK, Version 1.1.12
+ **Neue Features**
  + Listen von Zeichenfolgen können nun als Eingabe an `read_*`-Methoden übergeben werden.

+ **Fehlerbehebungen und Verbesserungen**
  + Die Leistung von `read_parquet` bei der Ausführung in Spark wurde erheblich verbessert.
  + Ein Problem wurde behoben, bei dem `column_type_builder` bei einer einzelnen Spalte mit mehrdeutigen Datumsformaten nicht ausgeführt wurde.

### <a name="azure-portal"></a>Azure-Portal
+ **Previewfunktion**
  + Das Streaming von Protokoll- und Ausgabedateien ist nun für Seiten mit Ausführungsdetails verfügbar. Die Dateien streamen Updates in Echtzeit, wenn die Umschaltfläche für die Vorschau aktiviert ist.
  + Die Möglichkeit, ein Kontingent auf Arbeitsbereichsebene festzulegen, wurde als Vorschauversion veröffentlicht. AmlCompute-Kontingente werden auf Abonnementebene zugeordnet, aber wir ermöglichen Ihnen nun, dieses Kontingent für eine gerechte Freigabe und Governance zwischen Arbeitsbereichen zu verteilen und zuzuordnen. Klicken Sie in der linken Navigationsleiste Ihres Arbeitsbereichs auf das Blatt **Nutzung + Kontingente**, und wählen Sie die Registerkarte **Configure Quotas** (Kontingente konfigurieren) aus. Beachten Sie, dass Sie ein Abonnementadministrator sein müssen, um auf Arbeitsbereichsebene Kontingente festlegen zu können, da dies ein arbeitsbereichsübergreifender Vorgang ist.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>Azure Machine Learning SDK für Python v1.0.55

+ **Neue Features**
  + Die tokenbasierte Authentifizierung wird jetzt für die Aufrufe unterstützt, die an den in AKS bereitgestellten Bewertungsendpunkt ausgegeben werden. Wir setzen die Unterstützung der aktuellen, schlüsselbasierten Authentifizierung fort, und Benutzer können jeweils einen dieser Authentifizierungsmechanismen verwenden.
  + Möglichkeit zum Registrieren eines Blob-Speichers, der sich hinter dem virtuellen Netzwerk (VNet) als Datenspeicher befindet.
  
+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-automl-core**
    + Korrigiert einen Fehler, bei dem die Validierungsgröße für CV-Teilungen klein ist und zu ungültig vorhergesagten gegenüber echten Diagrammen für Regression und Prognosen führt.
    + Die Protokollierung von Vorhersageaufgaben bei Remoteausführungen wurde verbessert, und dem Benutzer wird nun eine umfassende Fehlermeldung bereitgestellt, wenn die Ausführung fehlschlägt.
    + Fehler von `Timeseries` wurden korrigiert, wenn das Vorverarbeitungsflag TRUE war.
    + Die Umsetzbarkeit einiger Fehlermeldungen zur Vorhersagedatenüberprüfung wurde verbessert.
    + Verringerte Arbeitsspeichernutzung bei AutoML-Läufen durch Löschen und/oder verzögertes Laden von Datasets, insbesondere zwischen der Erzeugung von Prozessen.
  + **azureml-contrib-explain-model**
    + Explainern wurde das Flag „model_task“ hinzugefügt, um Benutzern das Außerkraftsetzen der automatischen Standardrückschlusslogik für den Modelltyp zu gestatten.
    + Änderungen bei Widgets: Wird automatisch mit `contrib` installiert, keine Installation/Aktivierung von `nbextension` mehr. - Unterstützung der Erklärung durch rein globale Wichtigkeit der Funktion (z. B. permutativ).
    + Dashboardänderungen: - Boxplots und Violinplots zusätzlich zum `beeswarm`-Plot auf der Zusammenfassungsseite. - Viel schnelleres Rendern von `beeswarm`-Plots bei „Top-k“-Schieberegleränderung. - Hilfreiche Meldung, die erläutert, wie „Top-k“ berechnet wird. - Nützliche, anpassbare Meldungen anstelle von Diagrammen, wenn keine Daten bereitgestellt werden.
  + **azureml-core**
    + Die Model.package()-Methode wurde hinzugefügt, um Docker-Images und Dockerfiles zu erstellen, die Modelle und deren Abhängigkeiten einkapseln.
    + Aktualisierung der lokalen Webdienste, sodass sie InferenceConfigs akzeptieren, die Umgebungsobjekte enthalten.
    + Behoben wurde: Model.register() erzeugt ungültige Modelle, wenn "." (für das aktuelle Verzeichnis) als model_path-Parameter übergeben wird.
    + Run.submit_child wurde hinzugefügt, die Funktionalität spiegelt Experiment.submit, während gleichzeitig die Ausführung als übergeordnetes Element der übermittelten untergeordneten Ausführung angegeben wird.
    + Unterstützung von Konfigurationsoptionen aus Model.register in Run.register_model.
    + Möglichkeit zum Ausführen von JAR-Aufträgen in einem vorhandenen Cluster.
    + Jetzt Unterstützung der Parameter instance_pool_id und cluster_log_dbfs_path.
    + Unterstützung für die Verwendung eines Umgebungsobjekts beim Bereitstellen eines Modells in einem Webdienst wurde hinzugefügt. Das Umgebungsobjekt kann jetzt als Teil des InferenceConfig-Objekts bereitgestellt werden.
    + appinsifht-Zuordnung für neue Regionen wurde hinzugefügt: -centralus -westus -northcentralus
    + Dokumentation für alle Attribute in allen Datastore-Klassen wurde hinzugefügt.
    + Der blob_cache_timeout-Parameter wurde zu `Datastore.register_azure_blob_container` hinzugefügt.
    + Die Methoden save_to_directory und load_from_directory wurden zu „azureml.core.environment.Environment“ hinzugefügt.
    + Die Befehle „az ml environment download“ und „az ml environment register“ wurden der CLI hinzugefügt.
    + Die Methode Environment.add _private_pip_wheel wurde hinzugefügt.
  + **azureml-explain-model**
    + Die Nachverfolgung von Datasets wurde mithilfe des Dataset-Anbieters (Vorschau) zu Erläuterungen hinzugefügt.
    + Reduzierte Standardbatchgröße beim Streamen globaler Erläuterungen von 10.000 auf 100.
    + Explainern wurde das Flag „model_task“ hinzugefügt, um Benutzern das Außerkraftsetzen der automatischen Standardrückschlusslogik für den Modelltyp zu gestatten.
  + **azureml-mlflow**
    + Fehler in mlflow.azureml.build_image behoben, bei dem geschachtelte Verzeichnisse ignoriert werden.
  + **azureml-pipeline-steps**
    + Möglichkeit zum Ausführen von JAR-Aufträgen in einem vorhandenen Azure Databricks-Cluster wurde hinzugefügt.
    + Unterstützung der Parameter instance_pool_id und cluster_log_dbfs_path. für den DatabricksStep-Schritt hinzugefügt.
    + Unterstützung für Pipelineparameter im DatabricksStep-Schritt hinzugefügt.
  + **azureml-train-automl**
    + `docstrings` für die ensemblebezogenen Dateien wurden hinzugefügt.
    + Die Ausdrucksweise der Dokumente für `max_cores_per_iteration` und `max_concurrent_iterations` wurde angepasst.
    + Die Protokollierung von Vorhersageaufgaben bei Remoteausführungen wurde verbessert, und dem Benutzer wird nun eine umfassende Fehlermeldung bereitgestellt, wenn die Ausführung fehlschlägt.
    + get_data wurde aus dem Pipeline-Notebook `automlstep` entfernt.
    + `dataprep` wird jetzt in `automlstep` unterstützt.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>Azure Machine Learning Data Prep SDK v1.1.10

+ **Neue Features**
  + Sie können jetzt die Ausführung bestimmter Inspektoren (z. B. Histogramm, Punktdiagramm usw.) für bestimmte Spalten anfordern.
  + `append_columns`wurde ein prarallelisiertes Argument hinzugefügt. Wenn „True“, werden Daten in den Arbeitsspeicher geladen, aber die Ausführung wird parallel ausgeführt. Wenn „False“, wird die Ausführung gestreamt, aber singlethread.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>Azure Machine Learning SDK für Python v1.0.53

+ **Neue Features**
  + Für das automatisierte maschinelle Lernen wird jetzt das Trainieren von ONNX-Modellen auf dem Remotecomputeziel unterstützt.
  + In Azure Machine Learning ist es jetzt möglich, das Training von einer vorherigen Ausführung, einem Prüfpunkt oder für bestimmte Modelldateien fortzusetzen.
    + Informieren Sie sich, wie Sie [Estimators verwenden, um das Training für eine vorherige Ausführung fortzusetzen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb).

+ **Fehlerbehebungen und Verbesserungen**
  + **automl-client-core-nativeclient**
    + Der Fehler im Zusammenhang mit dem Verlust von Spaltentypen nach der Transformation wurde behoben (Link zum Fehler). 
    + „y_query“ wurde gestattet, einen Objekttyp anzunehmen, der „None(s)“ (Nichts) am Anfang enthält (#459519).
  + **azure-cli-ml**
    + Die CLI-Befehle „model deploy“ und „service update“ akzeptieren jetzt Parameter, Konfigurationsdateien oder eine Kombination daraus. Parameter haben Vorrang vor Attributen in Dateien.
    + Die Modellbeschreibung kann nach der Registrierung jetzt aktualisiert werden.
  + **azureml-automl-core**
    + Aktualisierung der NimbusML-Abhängigkeit auf Version 1.2.0 (derzeitige aktuelle Version).
    + Unterstützung für NimbusML-Estimators und -Pipelines für die Verwendung in AutoML-Estimators.
    + Es wurde ein Fehler im Vorgang bei der Ensembleauswahl behoben, bei dem das resultierende Ensemble unnötig vergrößert wurde, auch wenn die Ergebnisse konstant blieben.
    + Aktivierung der Wiederverwendung einiger Featurebereitstellungen für Vorhersageaufgaben über Kreuzvalidierungsaufteilungen hinweg. So wird die Laufzeit des Setups für aufwändige Featurebereitstellungen, z. B. Verzögerungen und gleitende Fenster, ungefähr um den Faktor „n_cross_validations“ beschleunigt.
    + Es wurde ein Problem behoben, das aufgetreten ist, wenn die Zeit außerhalb des von Pandas unterstützten Zeitbereichs gelegen hat. Nun wird ein DataException-Vorgang ausgelöst, falls die Zeit kleiner als „pd.Timestamp.min“ oder größer als „pd.Timestamp.max“ ist.
    + Bei der Vorhersage sind jetzt verschiedene Häufigkeiten in Trainings- und Testsätzen zulässig, falls sie aneinander ausgerichtet werden können. Dies ist beispielsweise für „Vierteljährlich ab Januar“ und „Vierteljährlich ab Oktober“ möglich.
    + Die „parameters“-Eigenschaft wurde dem „TimeSeriesTransformer“-Element hinzugefügt.
    + Entfernen Sie alte Ausnahmeklassen.
    + Bei Vorhersageaufgaben akzeptiert der Parameter `target_lags` jetzt einen einzelnen Integerwert oder eine Liste mit Integerwerten. Wenn der Integerwert angegeben wurde, wird nur eine Verzögerung erstellt. Bei Angabe einer Liste werden die eindeutigen Werte von Verzögerungen verwendet. Mit „target_lags=[1, 2, 2, 4]“ werden Verzögerungen mit einer, zwei und vier Perioden erstellt.
    + Der Fehler im Zusammenhang mit dem Verlust von Spaltentypen nach der Transformation wurde behoben (Link zum Fehler).
    + Bei `model.forecast(X, y_query)` wurde „y_query“ gestattet, einen Objekttyp anzunehmen, der „None(s)“ (Nichts) am Anfang enthält (#459519).
    + Der `automl`-Ausgabe wurden erwartete Werte hinzugefügt.
  + **azureml-contrib-datadrift**
    +  Verbesserungen am Beispielnotebook, z. B. Wechsel zu „azureml-opendatasets“ anstelle von „azureml-contrib-opendatasets“ und Leistungsverbesserungen bei der Anreicherung von Daten.
  + **azureml-contrib-explain-model**
    + Fehlerbehebung: „transformations“-Argument für LIME Explainer für unformatierte Featurepriorität im azureml-contrib-explain-model-Paket.
    + Bilderklärungen im Image Explainer für das AzureML-contrib-explain-model-Paket wurden Segmentierungen hinzugefügt.
    + SciPy Sparse-Unterstützung für Lime Explainer hinzugefügt.
    + Dem Mimic Explainer wurde `batch_size` hinzugefügt, wenn `include_local=False` für das Streaming globaler Erklärungen in Batches zutrifft, um die Ausführungszeit von DecisionTreeExplainableModel zu verbessern.
  + **azureml-contrib-featureengineering**
    + Fehlerbehebung für Aufruf von set_featurizer_timeseries_params(): Änderung des Wörterbuchwerttyps und Nullüberprüfung. - Notebook für `timeseries`-Featurizer wurde hinzugefügt.
    + Aktualisierung der NimbusML-Abhängigkeit auf Version 1.2.0 (derzeitige aktuelle Version).
  + **azureml-core**
    + Die Möglichkeit zum Anfügen von DBFS-Datenspeichern in der AzureML CLI wurde hinzugefügt. 
    + Der Fehler für den Datenspeicherupload, bei dem ein leerer Ordner erstellt wurde, wenn `target_path` mit `/` gestartet wurde, wurde behoben.
    + Ein Problem mit `deepcopy` in ServicePrincipalAuthentication wurde behoben.
    + Die Befehle „az ml environment show“ und „az ml environment list“ wurden der CLI hinzugefügt.
    + Umgebungen unterstützen jetzt das Angeben einer „base_dockerfile“ als Alternative zu einem bereits erstellten „base_image“.
    + Die nicht genutzte RunConfiguration-Einstellung „auto_prepare_environment“ wurde als veraltet gekennzeichnet.
    + Die Modellbeschreibung kann nach der Registrierung jetzt aktualisiert werden.
    + Fehlerbehebung: Beim Löschen von Modellen und Bildern werden jetzt mehr Informationen zum Abrufen von Upstreamobjekten bereitgestellt, die von ihnen abhängen, wenn der Löschvorgang aufgrund einer Upstreamabhängigkeit fehlschlägt.
    + Es wurde ein Fehler behoben, bei dem eine leere Dauer für Bereitstellungen ausgegeben wurde, die beim Erstellen eines Arbeitsbereichs für einige Umgebungen durchgeführt werden.
    + Verbesserung in Bezug auf Ausnahmen aufgrund eines Fehlers beim Erstellen von Arbeitsbereichen. Benutzern wird nicht eine Meldung der Art „Arbeitsbereich kann nicht erstellt werden. ... kann nicht gefunden werden.“ angezeigt, sondern stattdessen der tatsächliche Erstellungsfehler.
    + Unterstützung für die Tokenauthentifizierung in AKS-Webdiensten hinzugefügt. 
    + `Webservice`-Objekten wurde die `get_token()`-Methode hinzugefügt.
    + CLI-Unterstützung für die Verwaltung von Machine Learning-Datasets wurde hinzugefügt.
    + Für `Datastore.register_azure_blob_container` kann jetzt optional ein `blob_cache_timeout`-Wert (in Sekunden) verwendet werden, mit dem die blobfuse-Bereitstellungsparameter konfiguriert werden, um den Cacheablauf für diesen Datenspeicher zu aktivieren. In der Standardeinstellung wird kein Timeout genutzt. Wenn ein Blob gelesen wird, verbleibt er im lokalen Cache, bis der Auftrag abgeschlossen ist. Für die meisten Aufträge wird diese Einstellung bevorzugt, aber bei einigen Aufträgen müssen mehr Daten aus einem großen Dataset gelesen werden, als auf die Knoten passen. Bei diesen Aufträgen ist der Vorgang eher erfolgreich, wenn dieser Parameter optimiert wird. Gehen Sie beim Optimieren dieses Parameters mit Bedacht vor: Wenn Sie den Wert zu niedrig festlegen, kann dies zu einer schlechten Leistung führen, da die in einer Epoche verwendeten Daten ggf. ablaufen, bevor sie erneut verwendet werden. Dies bedeutet, dass alle Lesevorgänge aus dem Blobspeicher (also dem Netzwerk) erfolgen, anstatt aus dem lokalen Cache. Hieraus ergibt sich eine negative Auswirkung auf die Trainingszeiten.
    + Die Modellbeschreibung kann nach der Registrierung jetzt richtig aktualisiert werden.
    + Beim Löschen von Modellen und Images werden jetzt mehr Informationen zu Upstream-Objekten angegeben, die davon abhängig sind und dazu führen, dass der Löschvorgang nicht erfolgreich ist.
    + Die Ressourcenverwendung bei Remoteausführungen, die „azureml.mlflow“ nutzen, wurde verbessert.
  + **azureml-explain-model**
    + Fehlerbehebung: „transformations“-Argument für LIME Explainer für unformatierte Featurepriorität im azureml-contrib-explain-model-Paket.
    + SciPy Sparse-Unterstützung für Lime Explainer hinzugefügt.
    + Shape Linear Explainer-Wrapper wurde hinzugefügt, und Tabular Explainer wurde eine weitere Ebene zur Erklärung von linearen Modellen hinzugefügt.
    + Für Mimic Explainer in der Erklärungsmodellbibliothek wurde der Fehler behoben, der bei geringer Dateneingabe für „include_local=False“ aufgetreten ist.
    + Der `automl`-Ausgabe wurden erwartete Werte hinzugefügt.
    + Die Featurepriorität für die Permutation wurde für den Fall korrigiert, in dem das „transformations“-Argument bereitgestellt wurde, um die unformatierte Featurepriorität zu erhalten.
    + Dem Mimic Explainer wurde `batch_size` hinzugefügt, wenn `include_local=False` für das Streaming globaler Erklärungen in Batches zutrifft, um die Ausführungszeit von DecisionTreeExplainableModel zu verbessern.
    + Für die Modellerklärungsbibliothek wurden Fehler für Blackbox Explainer behoben, bei denen für die Vorhersage eine Pandas-Datenrahmeneingabe erforderlich ist.
    + Der Fehler, bei dem `explanation.expected_values` gelegentlich einen float-Wert anstelle einer Liste mit einem darin enthaltenen float-Wert zurückgegeben hat, wurde behoben.
  + **azureml-mlflow**
    + Leistung von „mlflow.set_experiment(experiment_name)“ wurde verbessert.
    + Ein Fehler bei der Nutzung von „InteractiveLoginAuthentication“ für „mlflow tracking_uri“ wurde behoben.
    + Die Ressourcenverwendung bei Remoteausführungen, die „azureml.mlflow“ nutzen, wurde verbessert.
    + Die Dokumentation des „azureml-mlflow“-Pakets wurde verbessert.
    + Es wurde ein Fehler gepatcht, bei dem „mlflow.log_artifacts("my_dir")“ Artefakte unter „my_dir/<artifact-paths>“ gespeichert hat, anstatt unter „<artifact-paths>“.
  + **azureml-opendatasets**
    + `pyarrow` von `opendatasets` wurde an alte Versionen (< 0.14.0) angeheftet, da dort ein Arbeitsspeicherproblem eingeführt wurde.
    +  „azureml-contrib-opendatasets“ wurde in „azureml-opendatasets“ verschoben. - Open Dataset-Klassen wurde gestattet, sich bei einem AML-Arbeitsbereich zu registrieren und nahtlos AML-Dataset-Funktionen zu nutzen. - Die NoaaIsdWeather-Erweiterungsleistung in Nicht-Spark-Versionen wurde erheblich verbessert.
  + **azureml-pipeline-steps**
    + DBFS Datastore wird in DatabricksStep jetzt für Ein- und Ausgaben unterstützt.
    + Aktualisierung der Dokumentation für Azure Batch-Schritt in Bezug auf Ein-/Ausgaben.
    + In AzureBatchStep wurde der Standardwert von *delete_batch_job_after_finish* in *true* geändert.
  + **azureml-telemetry**
    +  „azureml-contrib-opendatasets“ wurde in „azureml-opendatasets“ verschoben. - Open Dataset-Klassen wurde gestattet, sich bei einem AML-Arbeitsbereich zu registrieren und nahtlos AML-Dataset-Funktionen zu nutzen. - Die NoaaIsdWeather-Erweiterungsleistung in Nicht-Spark-Versionen wurde erheblich verbessert.
  + **azureml-train-automl**
    + Aktualisierung der Dokumentation zu „get_output“, damit der tatsächliche Rückgabetyp widergespiegelt wird, und zusätzliche Hinweise zum Abrufen wichtiger Eigenschaften.
    + Aktualisierung der NimbusML-Abhängigkeit auf Version 1.2.0 (derzeitige aktuelle Version).
    + Der `automl`-Ausgabe wurden erwartete Werte hinzugefügt.
  + **azureml-train-core**
    + Zeichenfolgen werden jetzt als Computeziel für die automatisierte Hyperparameteroptimierung akzeptiert.
    + Die nicht genutzte RunConfiguration-Einstellung „auto_prepare_environment“ wurde als veraltet gekennzeichnet.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>Azure Machine Learning Data Prep SDK, Version 1.1.9

+ **Neue Features**
  + Unterstützung für das direkte Lesen einer Datei aus einer HTTP- oder HTTPS-URL wurde hinzugefügt.

+ **Fehlerbehebungen und Verbesserungen**
  + Verbesserte Fehlermeldung beim Versuch, ein Parquet-Dataset aus einer Remotequelle zu lesen (dies wird derzeit nicht unterstützt).
  + Es wurde ein Fehler behoben, der beim Schreiben in Dateien im Parquet-Format in ADLS Gen 2 und beim Aktualisieren des ADLS Gen 2-Containernamens im Pfad auftrat.

## <a name="2019-07-09"></a>2019-07-09

### <a name="visual-interface"></a>Grafische Benutzeroberfläche
+ **Vorschaufunktionen**
  + Das Modul „Execute R Script“ wurde in der grafischen Benutzeroberfläche hinzugefügt.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>Azure Machine Learning SDK für Python, v1.0.48

+ **Neue Features**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** ist jetzt verfügbar als **azureml-opendatasets**. Das alte Paket kann weiterhin funktionieren, aber wir empfehlen Ihnen die Verwendung von **azureml-opendatasets**, um in den Genuss umfangreicherer Funktionen und Verbesserungen zu kommen.
    + Mit diesem neuen Paket können Sie geöffnete Datasets als Dataset im AML-Arbeitsbereich registrieren und jegliche Funktionen nutzen, die dieses Dataset bietet.
    + Es umfasst außerdem vorhandene Funktionen, wie das Nutzen geöffneter Datasets als Pandas/SPARK-Dataframes sowie Standortverknüpfungen für einige Datasets wie Wetter.

+ **Vorschaufunktionen**
    + HyperDriveConfig kann jetzt Pipelineobjekte als Parameter akzeptieren, um die Hyperparameteroptimierung mithilfe einer Pipeline zu unterstützen.

+ **Fehlerbehebungen und Verbesserungen**
  + **azureml-train-automl**
    + Der Fehler im Zusammenhang mit dem Verlust von Spaltentypen nach der Transformation wurde behoben.
    + Der Fehler, bei dem „y_query“ gestattet wurde, einen Objekttyp anzunehmen, der „None(s)“ (Nichts) am Anfang enthielt, wurde behoben. 
    + Das Problem im Vorgang bei der Ensembleauswahl, wodurch das resultierende Ensemble unnötig vergrößert wurde, auch wenn die Ergebnisse konstant blieben, wurde behoben.
    + Das Problem mit den Einstellungen „whitelist_models“ und „blacklist_models“ in AutoMLStep wurde behoben.
    + Das Problem, das die Verwendung der Vorverarbeitung verhindert hat, wenn AutoML im Kontext von Azure ML-Pipelines verwendet worden wäre, wurde behoben.
  + **azureml-opendatasets**
    + „azureml-contrib-opendatasets“ wurde in „azureml-opendatasets“ verschoben.
    + Open Dataset-Klassen wurde gestattet, sich bei einem AML-Arbeitsbereich zu registrieren und nahtlos AML-Dataset-Funktionen zu nutzen.
    + Die NoaaIsdWeather-Erweiterungsleistung in Nicht-Spark-Versionen wurde erheblich verbessert.
  + **azureml-explain-model**
    + Die Onlinedokumentation für Interpretierbarkeitsobjekte wurde aktualisiert.
    + Dem Mimic Explainer wurde `batch_size` hinzugefügt, wenn `include_local=False` für das Streaming globaler Erläuterungen in Batches zutrifft, um die Ausführungszeit von DecisionTreeExplainableModel für die Modellerklärbarkeitsbibliothek zu verbessern.
    + Das Problem, bei dem `explanation.expected_values` gelegentlich einen float-Wert anstelle einer Liste mit einem enthaltenen float-Wert zurückgab, wurde behoben.
    + Der `automl`-Ausgabe für den Mimic Explainer in der Erläuterungsmodellbibliothek wurden erwartete Werte hinzugefügt.
    + Die Wichtigkeit der Permutationsfunktion wurde korrigiert, wenn das „transformations“-Argument bereitgestellt wurde, um die unformatierte Funktionswichtigkeit zu erhalten.
  + **azureml-core**
    + Die Möglichkeit zum Anfügen von DBFS-Datenspeichern in der AzureML CLI wurde hinzugefügt.
    + Das Problem mit dem Datenspeicherupload, bei dem ein leerer Ordner erstellt wird, wenn `target_path` mit `/` gestartet wird, wurde behoben.
    + Der Vergleich von zwei Datasets wurde ermöglicht.
    + Beim Löschen von Modellen und Bildern werden jetzt mehr Informationen zum Abrufen von Upstreamobjekten bereitgestellt, die von ihnen abhängen, wenn der Löschvorgang aufgrund einer Upstreamabhängigkeit fehlschlägt.
    + Die nicht verwendete „RunConfiguration“-Einstellung in auto_prepare_environment wurde als veraltet markiert.
  + **azureml-mlflow**
    + Die Ressourcenverwendung bei Remoteausführungen, die „azureml.mlflow“ verwenden, wurde verbessert.
    + Die Dokumentation des „azureml-mlflow“-Pakets wurde verbessert.
    + Das Problem, bei dem „mlflow.log_artifacts("my_dir")“ Artefakte unter „my_dir/artifact-paths“ anstelle unter „artifact-paths“ speichern würde, wurde behoben.
  + **azureml-pipeline-core**
    + Der Parameter „hash_paths“ ist für alle Pipelineschritte veraltet und wird in Zukunft entfernt. Inhalte von „source_directory“ (mit Ausnahme der in „.amlignore“ oder „.gitignore“ aufgeführten Dateien) werden standardmäßig hashcodiert.
    + Module und ModuleStep wurden kontinuierlich verbessert, um computetypspezifische Module zur Vorbereitung auf die RunConfiguration-Integration und andere Änderungen zu unterstützen, damit deren Verwendung in Pipelines möglich wird.
  + **azureml-pipeline-steps**
    + AzureBatchStep: Verbesserte Dokumentation hinsichtlich Eingaben/Ausgaben.
    + AzureBatchStep: Der Standardwert von „delete_batch_job_after_finish“ wurde in „true“ geändert.
  + **azureml-train-core**
    + Zeichenfolgen werden jetzt als Computeziel für die automatisierte Hyperparameteroptimierung akzeptiert.
    + Die nicht verwendete „RunConfiguration“-Einstellung in auto_prepare_environment wurde als veraltet markiert.
    + Die Parameter `conda_dependencies_file_path` und `pip_requirements_file_path` wurden zugunsten von `conda_dependencies_file` bzw. `pip_requirements_file` als veraltet markiert.
  + **azureml-opendatasets**
    + Die NoaaIsdWeather-Erweiterungsleistung in Nicht-Spark-Versionen wurde erheblich verbessert.

### <a name="azure-machine-learning-data-prep-sdk-v118"></a>Azure Machine Learning Data Prep SDK v1.1.8

+ **Neue Features**
 + Dataflow-Objekte können jetzt durchlaufen werden, wodurch eine Abfolge von Datensätzen erzeugt wird. Siehe Dokumentation für `Dataflow.to_record_iterator`.
  + Dataflow-Objekte können jetzt durchlaufen werden, wodurch eine Abfolge von Datensätzen erzeugt wird. Siehe Dokumentation für `Dataflow.to_record_iterator`.

+ **Fehlerbehebungen und Verbesserungen**
 + Die Stabilität des DataPrep SDK wurde erhöht.
 + Verbesserte Handhabung von Pandas-DataFrames mit Spaltenindizes, die keine Zeichenfolgen sind.
 + Die Leistung von `to_pandas_dataframe` in Datasets wurde verbessert.
 + Ein Fehler wurde behoben, bei dem die Spark-Ausführung von Datasets fehlschlug, wenn sie in einer Umgebung mit mehreren Knoten durchgeführt wurde.
  + Die Stabilität des DataPrep SDK wurde erhöht.
  + Verbesserte Handhabung von Pandas-DataFrames mit Spaltenindizes, die keine Zeichenfolgen sind.
  + Die Leistung von `to_pandas_dataframe` in Datasets wurde verbessert.
  + Ein Fehler wurde behoben, bei dem die Spark-Ausführung von Datasets fehlschlug, wenn sie in einer Umgebung mit mehreren Knoten durchgeführt wurde.

## <a name="2019-07-01"></a>01.07.2019

### <a name="azure-machine-learning-data-prep-sdk-v117"></a>Azure Machine Learning Data Prep SDK, Version 1.1.7

Wir haben eine Änderung rückgängig gemacht, die zwar die Leistung verbessert, aber bei einigen Kunden, die Azure Databricks nutzen, zu Problemen geführt hat. Wenn ein Problem mit Azure Databricks aufgetreten ist, können Sie mit einer der folgenden Methoden ein Upgrade auf Version 1.1.7 vornehmen:
1. Führen Sie für das Upgrade dieses Skript aus: `%sh /home/ubuntu/databricks/python/bin/pip install azureml-dataprep==1.1.7`
2. Erstellen Sie den Cluster neu, wodurch die neueste Data Prep SDK-Version installiert wird.

## <a name="2019-06-25"></a>25.06.2019

### <a name="azure-machine-learning-sdk-for-python-v1045"></a>Azure Machine Learning SDK für Python, Version 1.0.45

+ **Neue Features**
  + Ersatzmodell für Entscheidungsstruktur hinzugefügt, um den Explainer im Paket „azureml-explain-model“ zu imitieren.
  + Möglichkeit der Angabe einer CUDA-Version, die für Rückschlussimages installiert werden kann. Unterstützung für CUDA 9.0, 9.1 und 10.0.
  + Informationen zu Basisimages für das Training von Azure Machine Learning sind ab sofort im [GitHub-Repository zu Azure ML-Containern](https://github.com/Azure/AzureML-Containers) und auf [DockerHub](https://hub.docker.com/_/microsoft-azureml) verfügbar.
  + CLI-Unterstützung für Pipelinezeitplan hinzugefügt. Führen Sie „az ml pipeline -h“ aus, um mehr zu erfahren.
  + Benutzerdefinierten Kubernetes-Namespaceparameter zur Webdienst-Bereitstellungskonfiguration und CLI von AKS hinzugefügt.
  + Parameter „hash_paths“ für alle Pipelineschritte als veraltet markiert
  + „Model.register“ unterstützt nun die Registrierung mehrerer einzelner Dateien als einzelnes Modell bei Verwendung des Parameters `child_paths`.
  
+ **Vorschaufunktionen**
    + Explainer für die Bewertung können nun optional Conda- und Pip-Informationen für eine zuverlässigere Serialisierung und Deserialisierung speichern.
    + Programmfehlerbehebung für automatische Featureauswahl.
    + „mlflow.azureml.build_image“ auf die neue API aktualisiert, durch die neue Implementierung verursachte Fehler behoben.

+ **Fehlerbehebungen und Verbesserungen**
  + Die Abhängigkeit von `paramiko` wurde aus azureml-core entfernt. Warnung zu Veraltung für ältere Anfügemethoden für Computeziele hinzugefügt.
  + Leistung von „run.create_children“ verbessert
  + Im Mimic Explainer mit binärem Klassifizierer wurde die Reihenfolge von Wahrscheinlichkeiten korrigiert, wenn die Wahrscheinlichkeit „Lehrer“ für die Skalierung von Shapewerten verwendet wird.
  + Verbesserte Fehlerbehandlung und Meldung für automatisiertes maschinelles Lernen. 
  + Problem durch Timeout bei Iteration für automatisiertes maschinelles Lernen behoben.
  + Leistung bei der Transformation von Zeitreihen für automatisiertes maschinelles Lernen verbessert.

## <a name="2019-06-24"></a>24.06.2019

### <a name="azure-machine-learning-data-prep-sdk-v116"></a>Azure Machine Learning Data Prep SDK, Version 1.1.6

+ **Neue Features**
  + Zusammenfassungsfunktionen für die obersten Werte (`SummaryFunction.TOPVALUES`) und untersten Werte (`SummaryFunction.BOTTOMVALUES`) hinzugefügt.

+ **Fehlerbehebungen und Verbesserungen**
  + Deutliche Verbesserung der Leistung von `read_pandas_dataframe`.
  + Fehler behoben, der dazu führen konnte, dass `get_profile()` bei einem Datenfluss, der auf Binärdateien zeigt, fehlschlug.
  + `set_diagnostics_collection()` verfügbar gemacht, um ein programmgesteuertes Aktivieren/Deaktivieren der Telemetriedatensammlung zu ermöglichen.
  + Das Verhalten von `get_profile()` geändert. NaN-Werte werden nun für Min, Mean, Std und Sum ignoriert, was sich am Verhalten von Pandas orientiert.


## <a name="2019-06-10"></a>10.06.2019

### <a name="azure-machine-learning-sdk-for-python-v1043"></a>Azure Machine Learning SDK für Python, Version 1.0.43

+ **Neue Features**
  + Azure Machine Learning bietet jetzt erstklassige Unterstützung für das beliebte Machine Learning- und Datenanalyse-Framework Scikit-learn. Mit dem [`SKLearn`-Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) können Benutzer Scikit-learn-Modelle einfach trainieren und bereitstellen.
    + Erfahren Sie mehr über das [Ausführen der Hyperparameteroptimierung mit Scikit-learn mithilfe von HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb).
  + Unterstützung für die Erstellung von ModuleStep in Pipelines zusammen mit den Klassen „Module“ und „ModuleVersion“ hinzugefügt, um wiederverwendbare Compute-Einheiten zu verwalten.
  + ACI-Webdienste unterstützen jetzt persistenten „scoring_uri“ über Updates. „scoring_uri“ ändert sich von IP-Adresse in FQDN. Die DNS-Namensbezeichnung für FQDN kann konfiguriert werden, indem „dns_name_label“ auf „deploy_configuration“ festgelegt wird. 
  + Automatisiertes maschinelles Lernen – Neue Features:
    + STL-Featurizer für Prognosen
    + k-Means-Clustering ist für Feature-Sweeping aktiviert
  + Schnellere Genehmigungen für AmlCompute-Kontingent! Wir haben jetzt den Prozess automatisiert, mit dem Ihre Kontingentanforderungen innerhalb eines Schwellenwerts genehmigt werden können. Weitere Informationen zur Funktionsweise von Kontingenten finden Sie unter [Verwalten von Kontingenten](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

+ **Vorschaufunktionen**
    + Integration in [MLflow](https://mlflow.org) 1.0.0-Nachverfolgung über „azureml-mlflow“-Paket ([Beispielnotebooks](https://aka.ms/azureml-mlflow-examples)).
    + Übermitteln Sie Jupyter-Notebook als Lauf. [API-Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-contrib-notebook/azureml.contrib.notebook?view=azure-ml-py)
    + Public Preview von [Datenvariationserkennung](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift?view=azure-ml-py) über „azureml-contrib-datadrift“-Paket ([Beispielnotebooks](https://aka.ms/azureml-datadrift-example)). Datenvariation ist einer der Hauptgründe, warum die Modellgenauigkeit mit der Zeit abnimmt. Sie tritt auf, wenn sich die Daten, die ein Modell in der Produktion erhält, von den Daten unterscheiden, mit denen das Modell trainiert wurde. Die AML-Erkennung von Datenvariation hilft Kunden beim Überwachen der Datenvariation und sendet Benachrichtigungen, wenn eine Variation erkannt wird. 

+ **Wichtige Änderungen**

+ **Fehlerbehebungen und Verbesserungen**
  + Das Laden und Speichern von RunConfiguration unterstützt die Angabe eines vollständigen Dateipfads mit voller Abwärtskompatibilität für vorheriges Verhalten.
  + Caching in ServicePrincipalAuthentication hinzugefügt, standardmäßig deaktiviert.
  + Protokollierung mehrerer Plots unter demselben Metriknamen aktiviert.
  + Modellklasse jetzt ordnungsgemäß aus azureml.core (`from azureml.core import Model`) importierbar.
  + In Pipelineschritten ist der Parameter `hash_path` jetzt veraltet. Neues Verhalten ist ein Hash des vollständigen „source_directory“, mit Ausnahme der in „.amlignore“ oder „.gitignore“ aufgeführten Dateien.
  + In Pipelinepaketen wurden verschiedene `get_all`- und `get_all_*`-Methoden zugunsten von `list` bzw. `list_*` als veraltet markiert.
  + „azureml.core.get_run“ erfordert nicht mehr das Importieren von Klassen, bevor der ursprüngliche Lauftyp zurückgegeben wird.
  + Problem behoben, bei dem einige Aufrufe von „WebService Update“ kein Update ausgelöst haben.
  + Das Zeitlimit bei der Bewertung in AKS-Webdiensten muss zwischen 5 ms und 300.000 ms betragen. Der maximale Wert von „scoring_timeout_ms“ für Bewertungsanforderungen wurde von 1 Min. auf 5 Min. erhöht.
  + LocalWebservice-Objekte haben nun die Eigenschaften `scoring_uri` und `swagger_uri`.
  + Das Erstellen und Hochladen des Verzeichnisses „outputs“ wurde aus dem Benutzerprozesses verschoben. SDK für den Ausführungsverlauf wurde in jedem Benutzerprozess aktiviert. Dies sollte einige Synchronisierungsprobleme beheben, die bei verteilten Trainingsläufen auftreten.
  + Der Name des „azureml“-Protokolls, der aus dem Namen des Benutzerprozesses stammt, enthält nun den Prozessnamen (nur für verteiltes Training) und die PID.

### <a name="azure-machine-learning-data-prep-sdk-v115"></a>Azure Machine Learning Data Prep SDK, Version 1.1.5

+ **Fehlerbehebungen und Verbesserungen**
  + Für interpretierte „datetime“-Werte, die ein zweistelliges Jahresformat haben, wurde der Bereich der gültigen Jahre aktualisiert, um mit dem Windows-Release vom Mai übereinzustimmen. Der Bereich wurde von 1930-2029 in 1950-2049 geändert.
  + Beim Lesen in einer Datei und Festlegung auf `handleQuotedLineBreaks=True` wird `\r` als neue Zeile behandelt.
  + Fehler behoben, der dazu führte, dass `read_pandas_dataframe` in einigen Fällen fehlschlug.
  + Verbesserte Leistung von `get_profile`.
  + Verbesserte Fehlermeldungen.

## <a name="2019-05-28"></a>28.05.2019

### <a name="azure-machine-learning-data-prep-sdk-v114"></a>Azure Machine Learning Data Prep SDK, Version 1.1.4

+ **Neue Features**
  + Sie können jetzt die folgenden Ausdruckssprachfunktionen verwenden, um datetime-Werte zu extrahieren und sie nach der Analyse neuen Spalten zuzuweisen.
    + `RegEx.extract_record()` extrahiert DateTime-Elemente in eine neue Spalte.
    + `create_datetime()` erstellt DateTime-Objekte aus separaten datetime-Elementen.
  + Beim Aufrufen von `get_profile()` können Sie jetzt sehen, dass Quantilspalten als „(est.)“ bezeichnet werden, um deutlich anzugeben, dass es sich um Näherungswerte handelt.
  + Sie können jetzt beim Lesen aus Azure Blob Storage ** als Platzhalter verwenden.
    + Beispiel: `dprep.read_csv(path='https://yourblob.blob.core.windows.net/yourcontainer/**/data/*.csv')`

+ **Fehlerbehebungen**
  + Korrektur eines Fehlers im Zusammenhang mit dem Lesen einer Parquet-Datei aus einer Remotequelle (Azure-Blob).

## <a name="2019-05-14"></a>2019-05-14

### <a name="azure-machine-learning-sdk-for-python-v1039"></a>Azure Machine Learning SDK für Python, Version 1.0.39
+ **Änderungen**
  + Die für ein Ausführen verwendete Konfigurationsoption „auto_prepare_environment“ ist veraltet, wobei automatisches Vorbereiten zum Standard wird.

## <a name="2019-05-08"></a>2019-05-08

### <a name="azure-machine-learning-data-prep-sdk-v113"></a>Azure Machine Learning Data Prep SDK, Version 1.1.3

+ **Neue Features**
  + Hinzugefügte Unterstützung für Lesen aus einer PostgreSQL-Datenbank, entweder durch Aufrufen von „read_postgresql“ oder durch Verwenden eines Datenspeichers.
    + Beispiele finden Sie in den Schrittanleitung:
      + [Data Ingestion notebook](https://aka.ms/aml-data-prep-ingestion-nb) (Datenerfassungsnotebook)
      + [Datastore notebook](https://aka.ms/aml-data-prep-datastore-nb) (Datenspeichernotebook)

+ **Fehlerbehebungen und Verbesserungen**
  + Behobene Probleme Spaltentypkonvertierung:
  + Eine boolesche oder numerische Spalte wird nun richtig in eine boolesche Spalte konvertiert.
  + Es gibt nun keinen Fehler mehr bei dem Versuch, eine Datumsspalte auf den Typ für Datum festzulegen.
  + Verbesserte JoinType-Typen und zugehörige Referenzdokumentation. Wenn Sie zwei Datenflüsse verknüpfen, können Sie jetzt einen der folgenden Verknüpfungstypen (Join-Typen) angeben:
    + NONE, MATCH, INNER, UNMATCHLEFT, LEFTANTI, LEFTOUTER, UNMATCHRIGHT, RIGHTANTI, RIGHTOUTER, FULLANTI, FULL.
  + Verbesserte Datentyprückschlüsse, um mehr Datumsformate zu erkennen.

## <a name="2019-05-06"></a>2019-05-06

### <a name="azure-portal"></a>Azure-Portal

Im Azure-Portal haben Sie jetzt folgende Möglichkeiten:
+ Erstellen und Ausführen automatisierter ML-Experimente 
+ Erstellen eines virtuellen Notebook-Computers, um selbst Jupyter-Notebooks zu testen
+ Brandneuer Erstellungsbereich (Vorschauversion) im Machine Learning Service-Arbeitsbereich, der automatisiertes maschinelles Lernen, eine grafische Benutzeroberfläche und gehostete virtuelle Notebook-Computer umfasst
    + Automatisches Erstellen eines Modells mithilfe von automatisiertem maschinellem Lernen 
    + Verwenden einer grafischen Benutzeroberfläche mit Drag & Drop-Funktion zum Ausführen von Experimenten
    + Erstellen eines virtuellen Notebook-Computers zum Untersuchen von Daten, Erstellen von Modellen und Bereitstellen von Diensten
+ Liveupdates von Diagrammen und Metriken auf Seiten mit Ausführungsberichten und -details
+ Aktualisierte Dateianzeige für Protokolle, Ausgaben und Snapshots auf den Detailseiten der Ausführung
+ Neue und verbesserte Berichterstellung auf der Registerkarte „Experimente“ 
+ Neue Möglichkeit zum Herunterladen der Datei „config.json“ von der Übersichtsseite des Azure Machine Learning-Arbeitsbereichs
+ Unterstützung der Erstellung von Machine Learning Service-Arbeitsbereichen über den Azure Databricks-Arbeitsbereich 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033"></a>Azure Machine Learning SDK für Python, Version 1.0.33
+ **Neue Features**
  + Die Methode _Workspace.create_ akzeptiert nun Standardclusterkonfigurationen für CPU- und GPU-Cluster.
  + Wenn die Erstellung des Arbeitsbereichs fehlschlägt, werden abhängige Ressourcen bereinigt.
  + Die standardmäßige Azure Container Registry SKU wurde auf „Basic“ umgestellt.
  + Die Azure Container Registry-Instanz wird verzögert erstellt, wenn sie für die Ausführung oder Bilderstellung benötigt wird.
  + Unterstützung von Umgebungen für Trainingsausführungen

### <a name="notebook-virtual-machine"></a>Virtueller Notebook-Computer 

Verwenden Sie einen virtuellen Notebook-Computer als sichere, unternehmensgerechte Hostingumgebung für Jupyter-Notebooks, in der Sie Machine Learning-Experimente programmieren, Modelle als Webendpunkte bereitstellen und alle anderen vom Azure Machine Learning SDK mit Python unterstützten Vorgänge durchführen können. Die Umgebung bietet mehrere Funktionen:
+ [Schnelles Starten eines vorkonfigurierten virtuellen Notebook-Computers](tutorial-1st-experiment-sdk-setup.md)  mit der aktuellen Version des Azure Machine Learning-SDK und zugehörigen Paketen.
+ Der Zugriff wird durch bewährte Technologien wie HTTPS, Azure Active Directory-Authentifizierung und Autorisierung gesichert.
+ Zuverlässige Cloudspeicherung von Notebooks und Code im Blob-Speicherkonto Ihres Azure Machine Learning-Arbeitsbereichs. Sie können Ihre virtuellen Notebook-Computer sicher löschen, ohne Ihre Arbeit zu verlieren.
+ Vorinstallierte Beispielnotebooks zum Erkunden und Experimentieren mit Azure Machine Learning-Features.
+ Umfassende Anpassungsfunktionen für virtuelle Azure-Computer, virtuelle Computer jedes Typs, alle Pakete, alle Treiber. 

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>Azure Machine Learning SDK für Python, Version 1.0.33 veröffentlicht.

+ Hardwarebeschleunigte Azure ML-Modelle in [FPGAs](concept-accelerate-with-fpgas.md) sind allgemein verfügbar.
  + Mit dem [Paket „azureml-accel-models“ ](how-to-deploy-fpga-web-service.md) haben Sie jetzt folgende Möglichkeiten:
    + Trainieren der Gewichtungen eines unterstützten Deep Neural Network (ResNet 50, ResNet 152, DenseNet-121, VGG-16 und SSD-VGG)
    + Verwenden von Transferlernen mit dem unterstützten DNN
    + Registrieren des Modells beim Modellverwaltungsdienst und Verpacken des Modells in Container
    + Bereitstellen des Modells auf einem virtuellen Azure-Computer mit einem FPGA in einem Azure Kubernetes Service (AKS)-Cluster
  + Bereitstellen des Containers auf einem [Azure Data Box-Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)-Servergerät
  + Bewerten Ihrer Daten mit dem gRPC-Endpunkt mit diesem [Beispiel](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

+ Featuresweeping, um das dynamische Hinzufügen von :::no-loc text="featurizers"::: zur Leistungsoptimierung zu ermöglichen. Neue :::no-loc text="featurizers":::: Einbettungen von Arbeit, Gewichtung von Beweisen, Zielcodierungen, Textzielcodierung, Clusterabstand
+ Intelligente Kreuzvalidierung zur Verarbeitung trainierter/gültiger Teilungen im Rahmen des automatisierten maschinellen Lernens
+ Einige Änderungen zur Speicheroptimierung und Verbesserung der Laufzeitleistung
+ Leistungssteigerung in Modellerklärung
+ ONNX-Modellwechsel für lokale Ausführung
+ Subsampling-Unterstützung hinzugefügt
+ Intelligentes Beenden, wenn keine Beendigungskriterien definiert sind
+ Gestapelte Ensembles

+ Zeitreihe und Vorhersage
  + Neue Prognosefunktion   
  + Sie können jetzt Kreuzvalidierung mit rollierendem Ursprung für Zeitreihendaten verwenden.
  + Neue Funktionalität zur Konfiguration von Zeitreihenverzögerungen hinzugefügt 
  + Neue Funktionalität zur Unterstützung von Aggregatfeatures für rollierende Zeitfenster hinzugefügt
  + Neue Feiertagserkennung und Featurizer, wenn der Ländercode in den Experiment-Einstellungen definiert ist

+ Azure Databricks
  + Funktion zur Vorhersage von Zeitreihen und Modellerklärbarkeit/-interpretierbarkeit aktiviert
  + Automatisierte ML-Experimente können nun abgebrochen und wieder aufgenommen (fortgesetzt) werden.
  + Unterstützung für Multi-Core-Verarbeitung hinzugefügt

### <a name="mlops"></a>MLOps
+ **Lokale Bereitstellung und Debuggen für die Bewertung von Containern**<br/> Sie können jetzt ein ML-Modell lokal bereitstellen und mit Ihrer Bewertungsdatei und Abhängigkeiten schnell durchlaufen, um sicherzustellen, dass sie sich wie erwartet verhalten.

+ **„InferenceConfig“ und „Model.deploy()“ eingeführt**<br/> Die Modellimplementierung unterstützt nun die Angabe eines Quellordners mit einem Eingabeskript, wie bei einer RUNCONFIG-Datei.  Darüber hinaus wurde Modellimplementierung zu einem einzelnen Befehl vereinfacht.

+ **Nachverfolgen von Git-Verweisen**<br/> Kunden fragen seit einiger Zeit nach grundlegenden Git-Integrationsmöglichkeiten, da diese für einen End-to-End-Überwachungspfad hilfreich sind. Wir haben Nachverfolgung für die wichtigsten Einheiten in Azure ML für Git-bezogene Metadaten (Repository, Commit, fehlerfreier Zustand) implementiert. Diese Informationen werden automatisch vom SDK und der CLI gesammelt.

+ **Modellprofilerstellungs- und Validierungsdienst**<br/> Kunden beklagen sich häufig über die Schwierigkeit der richtigen Dimensionierung der mit ihrem Rückschlussdienst verbundenen Computeleistung. Mit unserem Modellprofil-Erstellungsdienst kann der Kunde Beispieleingaben bereitstellen und wir erstellen Profile für 16 verschiedene CPU-/Arbeitsspeicherkonfigurationen, um die optimale Größe für die Bereitstellung zu ermitteln.

+ **Verwenden eines eigenen Basisimages für Rückschlüsse**<br/> Eine weitere häufige Beschwerde betraf die Schwierigkeit, bei der gemeinsamen Nutzung von Abhängigkeiten von der Experimentierung zu Rückschlüssen zu gelangen. Mit unserer neuen Funktion zur gemeinsamen Nutzung von Basisimages können Sie nun Ihre Experimentierbasisimages einschließlich Abhängigkeiten usw. für Rückschlüsse wiederverwenden. Dies sollte Bereitstellungen beschleunigen und den Abstand zwischen der inneren und äußeren Schleife verringern.

+ **Verbesserte Swagger-Schemagenerierung**<br/> Unsere bisherige Swagger-Generierungsmethode war fehleranfällig und konnte nicht automatisiert werden. Wir haben eine neue Inline-Methode zum Generieren von Swagger-Schemas mit einer beliebigen Python-Funktion über Decorator-Elemente. Wir haben diesen Code als Open Source freigegeben, und unser Schemagenerierungsprotokoll ist nicht an die Azure ML-Plattform gekoppelt.

+ **Azure ML CLI allgemein verfügbar (GA)**<br/> Modelle können jetzt mit einem einzigen CLI-Befehl bereitgestellt werden. Wir haben häufig das Kundenfeedback erhalten, dass niemand ein ML-Modell über ein Jupyter-Notebook bereitstellt. Die [**CLI-Referenzdokumentation**](https://aka.ms/azmlcli) wurde aktualisiert.


## <a name="2019-04-22"></a>2019-04-22

Azure Machine Learning SDK für Python, Version 1.0.30 veröffentlicht.

Das [`PipelineEndpoint`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint?view=azure-ml-py)-Element wurde eingeführt, um unter Beibehaltung desselben Endpunkts eine neue Version einer veröffentlichten Pipeline hinzuzufügen.

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning Data Prep SDK, Version 1.1.2

Hinweis: Die Pakete `numpy` und `pandas` des Data Prep Python SDK werden nicht mehr installiert. Weitere Informationen finden Sie in den [aktualisierten Installationsanweisungen](https://aka.ms/aml-data-prep-installation).

+ **Neue Features**
  + Sie können jetzt die Pivottransformation verwenden.
    + Schrittanleitung: [Pivotnotebook](https://aka.ms/aml-data-prep-pivot-nb)
  + Sie können jetzt reguläre Ausdrücke in nativen Funktionen verwenden.
    + Beispiele:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Sie können jetzt die Funktionen `to_upper` und `to_lower` in der Ausdruckssprache verwenden.
  + Sie können jetzt die Anzahl der eindeutigen Werte in den einzelnen Spalten in einem Datenprofil sehen.
  + Für einige der häufig verwendeten Leserschritte können Sie nun das Argument `infer_column_types` übergeben. Bei Festlegung auf `True` versucht die Datenvorbereitung, die Spaltentypen automatisch zu erkennen und zu konvertieren.
    + `inference_arguments` gilt jetzt als veraltet.
  + Sie können jetzt `Dataflow.shape` aufrufen.

+ **Fehlerbehebungen und Verbesserungen**
  + `keep_columns` akzeptiert jetzt das zusätzliche optionale Argument `validate_column_exists`, das überprüft, ob das Ergebnis von `keep_columns` Spalten enthält.
  + Alle Leserschritte (bei denen aus einer Datei gelesen wird) akzeptieren nun das zusätzliche optionale Argument `verify_exists`.
  + Verbesserte Leistung beim Lesen von Pandas-Datenrahmen und beim Abrufen von Datenprofilen.
  + Korrektur eines Fehlers, bei dem das Aufteilen eines einzelnen Schritts in Slices aus einem Dataflow zu einem Fehler mit einem Index geführt hat.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure-Portal
  + Sie können jetzt ein vorhandenes Skript, das auf einem vorhandenen Remotecomputecluster ausgeführt wird, erneut übermitteln. 
  + Sie können jetzt eine veröffentlichte Pipeline mit neuen Parametern auf der Registerkarte „Pipelines“ ausführen. 
  + Ausführungsdetails unterstützen jetzt eine neue Momentaufnahme-Dateianzeige. Sie können eine Momentaufnahme des Verzeichnisses zu dem Zeitpunkt anzeigen, zu dem Sie eine bestimmte Ausführung übermittelt haben. Sie können auch das Notebook herunterladen, das zum Starten der Ausführung übermittelt wurde.
  + Sie können jetzt übergeordnete Ausführungen über das Azure-Portal abbrechen.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine Learning SDK für Python, Version 1.0.23

+ **Neue Features**
  + Das Azure Machine Learning SDK unterstützt jetzt Python 3.7.
  + Azure Machine Learning-DNN-Kalkulatoren bieten jetzt integrierte Unterstützung mehrerer Versionen. Beispielsweise akzeptiert der Kalkulator `TensorFlow`  jetzt den Parameter `framework_version`, und Benutzer können als Version „1.10“ oder „1.12“ angeben. Eine Liste der von Ihrer aktuellen SDK-Version unterstützten Versionen erhalten Sie, indem Sie `get_supported_versions()` für die gewünschte Frameworkklasse aufrufen (z. B. `TensorFlow.get_supported_versions()`).
  Eine Liste der von der aktuellen SDK-Version unterstützten Versionen finden Sie in der [Dokumentation für DNN-Kalkulatoren](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning Data Prep SDK, Version 1.1.1

+ **Neue Features**
  + Sie können mehrere DataStore/DataPath/DataReference-Quellen mithilfe von Transformationen vom Typ „read_*“ lesen.
  + Sie können die folgenden Vorgänge für Spalten ausführen, um eine neue Spalte zu erstellen: division, floor, modulo, power, length.
  + Die Datenvorbereitung ist jetzt Teil der Azure Machine Learning-Diagnosesuite. Dabei werden standardmäßig die Diagnoseinformationen protokolliert.
    + Um dies zu deaktivieren, legen Sie diese Umgebungsvariable auf TRUE fest: DISABLE_DPREP_LOGGER

+ **Fehlerbehebungen und Verbesserungen**
  + Die Codedokumentation für häufig verwendete Klassen und Funktionen wurde verbessert.
  + Ein Fehler in auto_read_file, der Fehler beim Lesen von Excel-Dateien verursacht hat, wurde behoben.
  + Option zum Überschreiben des Ordners in read_pandas_dataframe wurde hinzugefügt.
  + Die Leistung der Installation der Abhängigkeit dotnetcore2 wurde verbessert, und Unterstützung für Fedora 27/28 und Ubuntu 18.04 wurde hinzugefügt.
  + Die Leistung beim Lesen aus Azure Blobs wurde verbessert.
  + Die Spaltentyperkennung unterstützt nun Spalten vom Typ Long.
  + Ein Fehler, durch den einige Datumswerte anstelle von Python-datetime-Objekten als Zeitstempel angezeigt wurden, wurde behoben.
  + Ein Fehler, durch den wurden einige Typenanzahlen als double-Werte anstelle von Integer-Werten angezeigt wurden, wurde behoben.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine Learning SDK für Python v1.0.21

+ **Neue Features**
  + Die Methode *azureml.core.Run.create_children* ermöglicht die Erstellung mit geringer Wartezeit von mehreren untergeordneten Ausführungen mit einem einzigen Aufruf.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning Data Prep SDK v1.1.0

+ **Wichtige Änderungen**
  + Das Konzept des Data Prep Package ist veraltet und wird nicht mehr unterstützt. Anstelle von mehreren Dataflows in einem Paket können Sie Dataflows auch einzeln beibehalten.
    + Schrittanleitung: [Öffnen und Speichern des Dataflows-Notebooks](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Neue Features**
  + Data Prep kann jetzt Spalten erkennen, die einem bestimmten semantischen Typ entsprechen, und diese entsprechend aufteilen. Zu den derzeit unterstützten Typen gehören: E-Mail-Adresse, geografische Koordinaten (Breitengrad und Längengrad), IPv4- und IPv6-Adressen, US-Telefonnummer und US-Postleitzahl.
    + Schrittanleitung: [Notebook für semantische Typen](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Data Prep unterstützt jetzt die folgenden Operationen, um eine Ergebnisspalte aus zwei numerischen Spalten zu generieren: Subtrahieren, Multiplizieren, Dividieren und Modulo.
  + Sie können `verify_has_data()` für einen Dataflow aufrufen, um zu prüfen, ob der Dataflow bei Ausführung Datensätze erzeugen würde.

+ **Fehlerbehebungen und Verbesserungen**
  + Sie können jetzt die Datengruppenanzahl angeben, die in einem Histogramm für numerische Spaltenprofile verwendet werden sollen.
  + Die `read_pandas_dataframe`-Transformation erfordert jetzt, dass der Datenrahmen Spaltennamen vom Typ Zeichenfolge oder Byte aufweist.
  + Es wurde ein Fehler in der `fill_nulls`-Transformation behoben, bei dem Werte nicht ordnungsgemäß ausgefüllt wurden, wenn die Spalte fehlte.

## <a name="2019-03-11"></a>11.03.2019

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine Learning SDK für Python v1.0.18

 + **Änderungen**
   + Das azureml-tensorboard-Paket ersetzt azureml-contrib-tensorboard.
   + Mit diesem Release können Sie direkt während der Erstellung Ihres verwalteten Computeclusters (amlcompute) ein Benutzerkonto darin einrichten. Dazu werden diese Eigenschaften in der Bereitstellungskonfiguration übergeben. Weitere Informationen finden Sie in der [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning Data Prep SDK v1.0.17

+ **Neue Features**
  + Unterstützt jetzt das Hinzufügen von zwei numerischen Spalten, um eine resultierende-Spalte mithilfe der Ausdruckssprache zu generieren.

+ **Fehlerbehebungen und Verbesserungen**
  + Die Dokumentation und Parameterüberprüfung für „random_split“ wurden verbessert.
  
## <a name="2019-02-27"></a>27.02.2019

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning Data Prep SDK v1.0.16

+ **Fehlerbehebung**
  + Ein Authentifizierungsproblem beim Dienstprinzipal wurde behoben, das durch eine API-Änderung hervorgerufen wurde.

## <a name="2019-02-25"></a>25.02.2019

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine Learning SDK für Python v1.0.17

+ **Neue Features**

  + Azure Machine Learning bietet jetzt erstklassige Unterstützung für das beliebte DNN-Framework Chainer. Mithilfe von [`Chainer`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) können Klassenbenutzer Chainer-Modelle auf einfache Weise trainieren und bereitstellen.
    + Erfahren Sie mehr über das [Ausführen von verteiltem Training mit ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Erfahren Sie mehr über die [Hyperparameteroptimierung mit Chainer mithilfe von HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Azure Machine Learning Pipelines haben neu die Funktionalität zum Auslösen einer Pipelineausführung auf der Grundlage von Änderungen im Datenspeicher erhalten. Die Pipeline [Notizbuch planen](https://aka.ms/pl-schedule) wurde aktualisiert, um diese Funktion ins rechte Licht zu rücken.

+ **Fehlerbehebungen und Verbesserungen**
  + Wir haben unterstützende Azure Machine Learning Pipelines zum Festlegen der source_directory_data_store-Eigenschaft auf einen gewünschten Datenspeicher (wie etwa einen Blob-Speicher) auf [RunConfigurations](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) hinzugefügt, die im [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) verfügbar sind. Standardmäßig wird in den Schritten der Azure File-Speicher als unterstützender Datenspeicher verwendet, bei dem es zu Drosselungsproblemen kommen kann, wenn eine große Anzahl von Schritten parallel abgearbeitet wird.

### <a name="azure-portal"></a>Azure-Portal

+ **Neue Features**
  + Neue Benutzeroberfläche mit Drag & Drop im Tabellen-Editor für Berichte. Benutzer können eine Spalte aus der Quelle auf den Tabellenbereich ziehen, wo eine Vorschau der Tabelle angezeigt wird. Die Spalten können neu angeordnet werden.
  + Neue Protokolldateianzeige
  + Links zu Experimentausführungen, Compute, Modellen, Images und Bereitstellungen auf der Registerkarte „Aktivitäten“

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning Data Prep SDK v1.0.15

+ **Neue Features**
  + Data Prep unterstützt jetzt das Schreiben von Datenströmen aus einem Datenfluss. Ferner besteht jetzt die Möglichkeit, die Namen von Dateiströmen zu ändern, um neue Dateinamen zu erstellen.
    + Schrittanleitung: [Arbeiten mit dem Dateistrom-Notizbuch](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Fehlerbehebungen und Verbesserungen**
  + Verbesserte Leistung von t-Digest bei großen Datasets.
  + Data Prep unterstützt jetzt das Lesen von Daten aus einem Datenpfad.
  + 1-aus-n-Codierung funktioniert jetzt in Booleschen und numerischen Spalten.
  + Verschiedene weitere Fehlerbehebungen

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine Learning SDK für Python v1.0.15

+ **Neue Features**
  + Für Azure Machine Learning-Pipelines wurden „AzureBatchStep“ ([Notebook](https://aka.ms/pl-azbatch)), „HyperDriveStep“ (Notebook) und Zeitplanungsfunktionalität ([Notebook](https://aka.ms/pl-schedule)) hinzugefügt.
  +  DataTranferStep wurde aktualisiert und funktioniert nun mit Azure SQL Server und Azure Database for PostgreSQL ([Notebook](https://aka.ms/pl-data-trans)).

+ **Änderungen**
  + `PublishedPipeline.get_published_pipeline` wurde durch `PublishedPipeline.get` ersetzt.
  + `Schedule.get_schedule` wurde durch `Schedule.get` ersetzt.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning Data Prep SDK v1.0.12

+ **Neue Features**
  + Data Prep unterstützt jetzt das Lesen aus einer Azure SQL-Datenbank mithilfe von Datenspeicher.
 
+ **Änderungen**
  + Die Leistung des Arbeitsspeichers für bestimmte Vorgänge mit großen Datenmengen wurde verbessert.
  + `temp_folder` erfordert nun, dass `read_pandas_dataframe()` angegeben ist.
  + Die `name`-Eigenschaft von `ColumnProfile` ist veraltet: Verwenden Sie stattdessen `column_name`.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine Learning SDK für Python v1.0.10

+ **Änderungen**: 
  + Das Azure ML SDK besitzt keine „azure-cli“-Pakete mehr als Abhängigkeit. Insbesondere wurden die Abhängigkeiten „azure-cli-core“ und „azure-cli-profile“ aus „azureml-core“ entfernt. Dies sind die Änderungen mit Auswirkungen auf Benutzer:
    + Wenn Sie „az login“ ausführen und dann „azureml-sdk“ verwenden, führt das SDK die Browser- oder Geräteanmeldung ein weiteres Mal durch. Es verwendet keinen von „az login“ erstellten Anmeldeinformationsstatus.
    + Verwenden Sie für die Azure CLI-Authentifizierung, z. B. wie bei der Verwendung von "az login", die _azureml.core.authentication.AzureCliAuthentication_-Klasse. Führen Sie für die Azure CLI-Authentifizierung _pip install azure-cli_ in der Python-Umgebung durch, in der Sie „azureml-sdk“ installiert haben.
    + Wenn Sie „az login“ unter Verwendung eines Dienstprinzipals für die Automatisierung durchführen, empfehlen wir, dass Sie die _azureml.core.authentication.ServicePrincipalAuthentication_-Klasse verwenden, weil „azureml-sdk“ keinen von der Azure CLI erstellen Anmeldeinformationszustand verwendet. 

+ **Programmfehlerbehebungen**: Dieses Release enthält hauptsächlich kleinere Programmfehlerbehebungen.

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning Data Prep SDK v1.0.8

+ **Fehlerbehebungen**
  + Die Leistung beim Abrufen von Datenprofilen wurde verbessert.
  + Kleinere Fehler im Zusammenhang mit der Fehlerberichterstattung wurden behoben.
  
### <a name="azure-portal-new-features"></a>Azure-Portal: Neue Features
+ Neue Drag & Drop-Diagrammerstellungserfahrung für Berichte. Benutzer können eine Spalte oder ein Attribut aus dem Bereich in den Diagrammbereich ziehen, wo das System automatisch einen geeigneten Diagrammtyp für den Benutzer auswählt, basierend auf dem Typ der Daten. Benutzer können den Diagrammtyp in andere anwendbare Typen zu ändern oder zusätzliche Attribute hinzufügen.

    Unterstützte Diagrammtypen:
    - Liniendiagramm
    - Histogramm
    - Gestapeltes Balkendiagramm
    - Boxplot
    - Punktdiagramm
    - Blasendiagramm
+ Das Portal generiert Berichte für Experimente nun dynamisch. Wenn ein Benutzer eine Ausführung an ein Experiment übermittelt, wird automatisch ein Bericht mit protokollierten Metriken und Diagrammen erstellt, um einen Vergleich zwischen verschiedenen Ausführungen zu ermöglichen. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine Learning SDK für Python v1.0.8

+ **Programmfehlerbehebungen**: Dieses Release enthält hauptsächlich kleinere Programmfehlerbehebungen.

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning Data Prep SDK v1.0.7

+ **Neue Features**
  + Datenspeicherverbesserungen (dokumentiert in der [Datenspeicher-Anleitung](https://aka.ms/aml-data-prep-datastore-nb))
    + Möglichkeit zum Auslesen von und Schreiben in eine Azure-Dateifreigabe sowie in ADLS-Datenspeicher beim Hochskalieren hinzugefügt.
    + Bei der Verwendung von Datenspeichern unterstützt die Datenvorbereitung jetzt die Verwendung von Dienstprinzipalauthentifizierung anstelle von interaktiver Authentifizierung.
    + Unterstützung für wasb- und wasbs-URLs hinzugefügt.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning Data Prep SDK v1.0.6

+ **Fehlerbehebungen**
  + Korrektur eines Fehlers beim Lesen von öffentlichen lesbaren Azure-Blobcontainern in Spark

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine Learning SDK für Python v1.0.6
+ **Programmfehlerbehebungen**: Dieses Release enthält hauptsächlich kleinere Programmfehlerbehebungen.

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning Data Prep SDK v1.0.4

+ **Neue Features**
  + Die `to_bool`-Funktion ermöglicht nun die Konvertierung nicht übereinstimmender Werte in Fehlerwerte. Dies ist das neue Standardverhalten bei Nichtübereinstimmung für `to_bool` und `set_column_types`, während das bisherige Standardverhalten darin bestand, nicht übereinstimmende Werte in FALSE zu konvertieren.
  + Beim Aufruf von `to_pandas_dataframe` gibt es eine neue Option, um NULL- bzw. fehlende Werte in numerischen Spalten als NaN zu interpretieren.
  + Die Möglichkeit, den Rückgabetyp einiger Ausdrücke zu überprüfen, um die Typkonsistenz zu gewährleisten und frühzeitig fehlzuschlagen, wurde hinzugefügt.
  + Sie können nun `parse_json` aufrufen, um Werte in einer Spalte als JSON-Objekte zu analysieren und in mehreren Spalten zu erweitern.

+ **Fehlerbehebungen**
  + Programmfehler korrigiert, der in Python 3.5.2 für den Absturz von `set_column_types` sorgte.
  + Programmfehler korrigiert, der beim Herstellen einer Verbindung mit dem Datenspeicher über ein AML-Bild einen Absturz verursachte.

+ **Updates**
  * [Beispiel-Notebooks](https://aka.ms/aml-data-prep-notebooks) mit Tutorials mit den ersten Schritten, Fallstudien und Leitfäden.

## <a name="2018-12-04-general-availability"></a>2018-12-04: Allgemeine Verfügbarkeit

Azure Machine Learning ist jetzt allgemein verfügbar.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning Compute
Mit diesem Release kündigen wir eine neue verwaltete Compute-Umgebung über [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute) an. Dieses Computeziel ersetzt das Azure Batch KI-Computeziel für Azure Machine Learning. 

Dieses Computeziel:
+ Wird für das Modelltraining und Batchrückschlüsse/-bewertungen verwendet
+ Ist ein Computeziel mit einem oder mehreren Knoten
+ Übernimmt die Clusterverwaltung und Auftragsplanung für den Benutzer
+ Wird standardmäßig automatische skaliert
+ Unterstützt CPU- und GPU-Ressourcen 
+ Ermöglicht zur Kostensenkung die Nutzung von VMs mit niedriger Priorität

Azure Machine Learning Compute kann unter Python, mit dem Azure-Portal oder mit der CLI erstellt werden. Die Erstellung muss in der Region Ihres Arbeitsbereichs erfolgen, und ein Anfügen an einen anderen Arbeitsbereich ist nicht möglich. Bei diesem Computeziel wird für Ihre Ausführung ein Docker-Container genutzt, und Ihre Abhängigkeiten werden gepackt, um die gleiche Umgebung auf allen Ihren Knoten zu replizieren.

> [!Warning]
> Es wird empfohlen, einen neuen Arbeitsbereich für die Nutzung von Azure Machine Learning Compute zu erstellen. Es besteht eine geringe Wahrscheinlichkeit, dass Benutzern, die Azure Machine Learning Compute aus einem vorhandenen Arbeitsbereich erstellen möchten, ein Fehler angezeigt wird. Ein vorhandener Computevorgang in Ihrem Arbeitsbereich sollte ohne Beeinträchtigungen weiterhin funktionieren.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine Learning SDK für Python v1.0.2
+ **Wichtige Änderungen**
  + Mit diesem Release entfällt die Unterstützung für die Erstellung einer VM über Azure Machine Learning. Sie können aber weiterhin eine vorhandene Cloud-VM oder einen entfernten lokalen Server anfügen. 
  + Darüber hinaus entfällt die Unterstützung für Batch AI, da alle Funktionen jetzt per Azure Machine Learning Compute unterstützt werden sollten.

+ **Neu**
  + Für Machine Learning-Pipelines:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Aktualisiert**
  + Für Machine Learning-Pipelines:
    + Für [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) wird jetzt „runconfig“ akzeptiert.
    + Für [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) kann jetzt in eine und aus einer SQL-Datenquelle kopiert werden.
    + Planen von Funktionalität im SDK zum Erstellen und Aktualisieren von Zeitplänen für die Ausführung von veröffentlichten Pipelines

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning Data Prep SDK v0.5.2
+ **Wichtige Änderungen** 
  * `SummaryFunction.N` wurde in `SummaryFunction.Count` umbenannt.
  
+ **Fehlerbehebungen**
  * Verwenden Sie das aktuelle AML-Ausführungstoken, wenn Sie bei Remoteausführungen aus Datenspeichern lesen oder in Datenspeicher schreiben. Wenn das AML-Ausführungstoken zuvor in Python aktualisiert wird, wird die Runtime für die Datenaufbereitung nicht mit dem aktualisierten AML-Ausführungstoken aktualisiert.
  * Zusätzliche verständlichere Fehlermeldungen
  * „to_spark_dataframe()“ stürzt nicht mehr ab, wenn für Spark die `Kryo`-Serialisierung verwendet wird.
  * Für den Inspektor für die Anzahl von Werten können jetzt mehr als 1.000 eindeutige Werte angezeigt werden.
  * Für die zufällige Aufteilung tritt kein Fehler mehr auf, wenn der ursprüngliche Dataflow keinen Namen hat.  

+ **Weitere Informationen**
  * [Azure Machine Learning – Datenaufbereitungs-SDK](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Dokumentation und Notebooks
+ ML-Pipelines
  + Neue und aktualisierte Notebooks als Einstieg in Pipelines, Batchbereichsdefinition und Beispiele für die Stilübertragung: https://aka.ms/aml-pipeline-notebooks.
  + Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).
  + Erfahren Sie, wie Sie [Batchvorhersagen mit Pipelines ausführen](how-to-run-batch-predictions.md).
+ Azure Machine Learning Compute-Ziel
  + [Beispielnotebooks](https://aka.ms/aml-notebooks) sind jetzt für die Verwendung des neuen verwalteten Computeziels aktualisiert.
  + [Informationen zu dieser Computeumgebung](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure-Portal: Neue Features
+ Erstellen und Verwalten von [Azure Machine Learning Compute](how-to-set-up-training-targets.md#amlcompute)-Typen im Portal
+ Überwachen der Kontingentnutzung und des [Anforderungskontingents](how-to-manage-quotas.md) für Azure Machine Learning Compute
+ Anzeigen des Status für den Azure Machine Learning Compute-Cluster in Echtzeit
+ Nutzen der hinzugefügten Unterstützung für virtuelle Netzwerke für Azure Machine Learning Compute und die Azure Kubernetes Service-Erstellung
+ Erneutes Ausführen Ihrer veröffentlichten Pipelines mit vorhandenen Parametern
+ Neue [automatisierte Machine Learning-Diagramme](how-to-understand-automated-ml.md) für Klassifizierungsmodelle (Prognosegüte-, Gewinn-, Kalibrierungs-, Featurewichtigkeitsdiagramm mit Modellerklärung) und Regressionsmodelle (Restdaten- und Featurewichtigkeitsdiagramm mit Modellerklärung) 
+ Pipelines können im Azure-Portal angezeigt werden




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine Learning SDK für Python v0.1.80

+ **Wichtige Änderungen** 
  * Der Namespace *azureml.train.widgets* wurde in *azureml.widgets* verschoben.
  * *azureml.core.compute.AmlCompute* führt zur Veraltung der folgenden Klassen: *azureml.core.compute.BatchAICompute* und *azureml.core.compute.DSVMCompute*. Die letztgenannte Klasse wird in künftigen Versionen nicht mehr enthalten sein. Die AmlCompute-Klasse verfügt jetzt über eine einfachere Definition und benötigt lediglich Angaben für „vm_size“ und „max_nodes“. Sie skaliert Ihren Cluster automatisch von 0 auf den Wert von „max_nodes“, wenn ein Auftrag übermittelt wird. Unsere [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) wurden mit diesen Informationen aktualisiert und enthalten Beispiele zur Verwendung. Wir hoffen, dass Ihnen diese Vereinfachung gefällt – genauso wie viele weitere interessante Features in den nachfolgenden Releases!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning Data Prep SDK v0.5.1 

Erfahren Sie mehr zum Data Prep SDK, indem Sie die [Referenzdokumente](https://aka.ms/data-prep-sdk) lesen.
+ **Neue Features**
   * Es wurde eine neue DataPrep CLI erstellt, mit der DataPrep-Pakete ausgeführt werden können und das Datenprofil für ein Dataset oder einen Dataflow angezeigt werden kann.
   * Die SetColumnType-API wurde neu gestaltet, um die Nutzbarkeit zu verbessern.
   * „smart_read_file“ wurde in „auto_read_file“ umbenannt.
   * Datenschiefe und Kurtosis sind jetzt im Datenprofil enthalten.
   * Stichprobenentnahme mit geschichteten Stichproben ist möglich.
   * Das Lesen aus ZIP-Dateien, die CSV-Dateien enthalten, ist möglich.
   * Datasets können zeilenweise per zufälliger Aufteilung geteilt werden (z. B. in Test-/Trainingssätze).
   * Alle Spaltendatentypen können aus einem Dataflow oder Datenprofil abgerufen werden, indem `.dtypes` aufgerufen wird.
   * Die Zeilenanzahl kann aus einem Dataflow oder Datenprofil abgerufen werden, indem `.row_count` aufgerufen wird.

+ **Fehlerbehebungen**
   * Fehler bei Konvertierung von „long“ in „double“ behoben 
   * Fehler bei Assert-Vorgang nach jedem Hinzufügen einer Spalte behoben 
   * FuzzyGrouping-Fehler behoben, bei dem in einigen Fällen Gruppen nicht erkannt wurden
   * Fehler bei Sortierfunktion behoben, damit die Sortierreihenfolge für mehrere Spalten beibehalten wird
   * Fehler mit Ausdrücken behoben bzw. an Verarbeitung durch `pandas` angepasst
   * Fehler beim Lesen aus dem dbfs-Pfad behoben
   * Verständlichkeit von Fehlermeldungen verbessert 
   * Fehler behoben, der beim Lesen auf dem Remotecomputeziel mit dem AML-Token aufgetreten ist
   * Fehler für Linux DSVM behoben
   * Kein Absturz mehr, wenn in Zeichenfolgenprädikaten andere Werte als Zeichenfolgen enthalten sind
   * Assertionsfehler werden bei Dataflow-Fehler jetzt richtig behandelt
   * Per dbutils bereitgestellte Speicherorte werden unter Azure Databricks jetzt unterstützt

## <a name="2018-11-05"></a>05.11.2018

### <a name="azure-portal"></a>Azure-Portal 
Im Azure-Portal für Azure Machine Learning wurden folgende Updates vorgenommen:
  * Neue Registerkarte namens **Pipelines** für veröffentlichte Pipelines
  * Unterstützung für das Anfügen eines vorhandenen HDInsight-Clusters als Computeziel hinzugefügt

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine Learning SDK für Python v0.1.74

+ **Wichtige Änderungen** 
  * *Workspace.compute_targets, datastores, experiments, images, models und *webservices* sind Eigenschaften, keine Methoden. Ersetzen Sie beispielsweise *Workspace.compute_targets()* durch *Workspace.compute_targets*.
  * *Run.get_context* ersetzt *Run.get_submitted_run*. Die letztgenannte Methode wird in künftigen Versionen entfernt.
  * Die Klasse *PipelineData* erwartet nun anstelle von „datastore_name“ ein datastore-Objekt als Parameter. Analog dazu akzeptiert *Pipeline* nun „default_datastore“ anstelle von „default_datastore_name“.

+ **Neue Features**
  * Das [Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) für Azure Machine Learning-Pipelines verwendet jetzt MPI-Schritte.
  * Das RunDetails-Widget für Jupyter-Notebooks wird aktualisiert, um eine Visualisierung der Pipeline anzuzeigen.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning Data Prep SDK v0.4.0 
 
+ **Neue Features**
  * Typanzahl zum Datenprofil hinzugefügt 
  * Wertanzahl und Histogramm nun verfügbar
  * Mehr Perzentile im Datenprofil
  * Median in der Zusammenfassung verfügbar
  * Python 3.7 nun unterstützt
  * Wenn Sie einen Dataflow mit Datenspeichern in einem Datenvorbereitungspaket speichern, werden die Datenspeicherinformationen als Teil des Datenvorbereitungspakets gespeichert.
  * Schreibvorgänge für den Datenspeicher nun unterstützt 
        
+ **Behobene Fehler**
  * 64-Bit-Ganzzahlüberläufe ohne Vorzeichen werden nun unter Linux ordnungsgemäß behandelt.
  * Falscher Text für Nur-Text-Dateien in „smart_read“ behoben
  * Zeichenfolgenspaltentyp wird nun in der Metrikansicht angezeigt
  * Typanzahl korrigiert, sodass nun Wertarten (ValueKinds) angezeigt werden, die einem einzelnen Feldtyp (FieldType) zugeordnet sind (anstatt einzelne)
  * Bei „Write_to_csv“ tritt kein Fehler mehr auf, wenn der Pfad als Zeichenfolge angegeben wird.
  * Wenn bei Verwendung von „Ersetzen“ das Suchfeld leer ist, tritt kein Fehler mehr auf. 

## <a name="2018-10-12"></a>12.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine Learning SDK für Python v0.1.68

+ **Neue Features**
  * Unterstützung für mehrere Mandanten beim Erstellen eines neuen Arbeitsbereichs.

+ **Behobene Fehler**
  * Sie müssen die Version der pynacl-Bibliothek beim Bereitstellen des Webdiensts nicht mehr anheften.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.3.0

+ **Neue Features**
  * Methode transform_partition_with_file(script_path) hinzugefügt, die es Benutzern ermöglicht, den Pfad einer auszuführenden Python-Datei zu übergeben

## <a name="2018-10-01"></a>01.10.2018

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine Learning SDK für Python v0.1.65
[Version 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) enthält neue Features, mehr Dokumentation, Fehlerbehebungen und weitere [Beispielnotebooks](https://aka.ms/aml-notebooks).

Sehen Sie die [Liste der bekannten Probleme](resource-known-issues.md) an, um mehr über bekannte Fehler und Problemumgehungen zu erfahren.

+ **Wichtige Änderungen**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services geben ein Wörterbuch zurück, bisher wurde eine Liste zurückgegeben. Weitere Informationen finden Sie in der [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py)-API-Dokumentation.

  * Der Normalized-Mean-Square-Fehler in den primären Metriken wurde mit automatisiertem maschinellem Lernen entfernt.

+ **HyperDrive**
  * Verschiedene HyperDrive-Fehlerbehebungen für Bayessche Wahrscheinlichkeit, verbesserte Leistung für Abrufaufrufe der Metriken. 
  * Tensorflow 1.10-Upgrade von 1.9 
  * Docker-Image-Optimierung für den Kaltstart. 
  * Aufträge melden jetzt den richtigen Status, selbst wenn sie mit einem anderen Fehlercode als 0 beendet werden. 
  * Überprüfung des RunConfig-Attributs in SDK. 
  * Das HyperDrive-Ausführungsobjekt unterstützt jetzt Abbrechen, ähnlich einer normalen Ausführung: Das Übergeben von Parametern ist nicht erforderlich. 
  * Verbesserte Widgets, um den Status von Dropdownwerten für verteilte Ausführungen und HyperDrive-Ausführungen beizubehalten. 
  * TensorBoard und andere Protokolldateien unterstützen „Fixed“ für den Parameter „Server“. 
  * Dienstseitige Intel(R) MPI-Unterstützung. 
  * Fehlerbehebung bei der Parameteroptimierung für verteilte Ausführungskorrekturen während der Überprüfung in BatchAI. 
  * Der Kontext-Manager identifiziert jetzt die primäre Instanz. 

+ **Azure-Portal-Benutzeroberfläche**
  * log_table() und log_row() werden in den Ausführungsdetails unterstützt. 
  * Erstellen Sie automatisch Diagramme für Tabellen und Zeilen mit 1, 2 oder 3 numerischen Spalten und einer optionalen Kategoriespalte.

+ **Automatisiertes maschinelles Lernen**
  * Verbesserte Fehlerbehandlung und Dokumentation 
  * Behobene Leistungsprobleme beim Abrufen der Ausführungseigenschaft. 
  * Behobenes Problem beim Fortsetzen der Ausführung. 
  * :::no-loc text="ensembling"::: Iterationsprobleme wurden behoben.
  * Behobener Fehler wegen fehlender Reaktion beim Training unter MAC OS.
  * Downsampling der durchschnittlichen PR/ROC-Kurve im benutzerdefinierten Überprüfungsszenario.
  * Zusätzliche Indexlogik wurde entfernt.
  * Filter aus der get_output-API entfernt.

+ **Pipelines**
  * Hinzugefügte Methode Pipeline.publish() zum direkten Veröffentlichen von Pipelines ohne vorhergehende Ausführung.   
  * Hinzugefügte Methode PipelineRun.get_pipeline_runs() zum Abfangen der Pipelineausführungen, die von einer veröffentlichten Pipeline generiert wurden.

+ **Project Brainwave**
  * Aktualisierte Unterstützung für neue KI-Modelle auf FPGAs verfügbar.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning – Datenaufbereitungs-SDK v0.2.0
[Version 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) umfasst folgende Features und Programmfehlerbehebungen:

+ **Neue Features**
  * Unterstützung für 1-aus-n-Codierung
  * Unterstützung für Quantiltransformation
   
+ **Behobener Fehler:**
  * Funktioniert mit jeder Tornado-Version, ein Downgrade Ihrer Tornado-Version ist nicht erforderlich
  * Der Wert gilt für alle Werte, nicht nur für die höchsten drei

## <a name="2018-09-public-preview-refresh"></a>09.2018 (Aktualisierung der öffentlichen Vorschau)

Neues, aktualisiertes Release von Azure Machine Learning: Weitere Informationen zu diesem Release: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Übersicht für [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
