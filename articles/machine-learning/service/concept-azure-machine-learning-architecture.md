---
title: Architektur und wichtige Konzepte
titleSuffix: Azure Machine Learning
description: Enthält Informationen zu Architektur, Terminologie, Konzepten und Workflows von Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 10/16/2019
ms.custom: seodec18
ms.openlocfilehash: 36c496b77be5bfda83b3ed424a7fdf2b53101aa4
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580616"
---
# <a name="how-azure-machine-learning-works-architecture-and-concepts"></a>So funktioniert Azure Machine Learning: Architektur und Konzepte

Informationen zu Architektur, Konzepten und Workflows von Azure Machine Learning. Die wichtigsten Komponenten des Diensts und der allgemeine Workflow für seine Nutzung sind im folgenden Diagramm dargestellt:

![Azure Machine Learning – Architektur und Workflow](./media/concept-azure-machine-learning-architecture/workflow.png)

## <a name="workflow"></a>Workflow

Für den Workflow des Machine Learning-Modells werden in der Regel diese Schritte ausgeführt:

1. **Trainieren**
    + Entwickeln Sie Machine Learning-Trainingsskripts in **Python** oder mit dem Visual Designer.
    + Erstellen und Konfigurieren eines **Computeziels**
    + **Übermitteln der Skripts** an das konfigurierte Computeziel zur Ausführung in dieser Umgebung. Während des Trainings können die Skripts **Datenspeicher** lesen oder beschreiben. Die Datensätze zur Ausführung werden zudem als **Ausführungen** im **Arbeitsbereich** gespeichert und unter **Experimente** gruppiert.

1. **Paket**: Nachdem eine zufriedenstellende Ausführung gefunden wurde, können Sie das dauerhafte Modell in der **Modellregistrierung** registrieren.

1. **Überprüfen** - **Abfragen des Experiments** auf protokollierte Metriken aus den aktuellen und bereits erfolgten Ausführungen. Wenn die Metriken kein gewünschtes Ergebnis anzeigen, können Sie zu Schritt 1 zurückkehren und Ihre Skripts erneut durchlaufen.

1. **Bereitstellen**: Entwickeln Sie ein Bewertungsskript, von dem das Modell verwendet wird, und führen Sie die **Bereitstellung des Modells** als **Webdienst** in Azure oder auf einem **IoT Edge-Gerät** durch.

1. **Überwachen**: Führen Sie eine Überwachung auf **Datenabweichungen** zwischen dem Trainingsdataset und den Rückschlussdaten eines bereitgestellten Modells durch. Springen Sie bei Bedarf zurück zu Schritt 1, um das Modell mit neuen Trainingsdaten erneut zu trainieren.

## <a name="tools-for-azure-machine-learning"></a>Tools für Azure Machine Learning

Verwenden Sie diese Tools für Azure Machine Learning:

+  Interagieren Sie mit dem Dienst in einer beliebigen Python-Umgebung, indem Sie das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) verwenden.
+ Interagieren Sie mit dem Dienst in einer beliebigen R-Umgebung, indem Sie das [Azure Machine Learning SDK für R](https://azure.github.io/azureml-sdk-for-r/reference/index.html) verwenden.
+ Automatisieren Sie Ihre Machine Learning-Aktivitäten mit der [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli).
+ Schreiben Sie Code in Visual Studio Code mit der [VS Code-Erweiterung für Azure Machine Learning](how-to-vscode-tools.md).
+ Verwenden Sie den [Azure Machine Learning-Designer (Vorschauversion)](concept-designer.md), um die Workflowschritte auszuführen – ganz ohne Programmierung.


> [!NOTE]
> In diesem Artikel werden von Azure Machine Learning genutzte Begriffe und Konzepte definiert, aber keine Begriffe und Konzepte für Azure Platform. Weitere Informationen zur Azure Platform-Terminologie finden Sie im [Microsoft Azure-Glossar](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="glossary"></a>Glossar
+ <a href="#activities">Aktivität</a>
+ <a href="#compute-instance">Notebook-VM</a>
+ <a href="#compute-targets">Computeziele</a>
+ <a href="#datasets-and-datastores">Dataset und Datenspeicher</a>
+ <a href="#endpoints">Endpunkte</a>
+ <a href="#environments">Umgebungen</a>
+ [Schätzfunktionen](#estimators)
+ <a href="#experiments">Experimente</a>
+ <a href="#github-tracking-and-integration">Git-Tracking</a>
+ <a href="#iot-module-endpoints">IoT-Module</a>
+ <a href="#logging">Protokollierung</a>
+ <a href="#ml-pipelines">ML-Pipelines</a>
+ <a href="#models">Modelle</a>
+ <a href="#runs">Run</a>
+ <a href="#run-configurations">Laufzeitkonfiguration</a>
+ <a href="#snapshots">Momentaufnahme</a>
+ <a href="#training-scripts">Trainingsskript</a>
+ <a href="#web-service-endpoint">Webdienste</a>
+ <a href="#workspaces">Arbeitsbereich</a>

### <a name="activities"></a>activities

Eine Aktivität stellt einen Vorgang mit langer Ausführungsdauer dar. Die folgenden Vorgänge sind Beispiele für Aktivitäten:

* Erstellen oder Löschen eines Computeziels
* Ausführen eines Skripts auf einem Computeziel

Aktivitäten können Benachrichtigungen über das SDK oder eine Webbenutzeroberfläche bereitstellen, damit Sie den Status dieser Vorgänge auf einfache Weise überwachen können.

### <a name="compute-instance"></a>Notebook-VM

Eine **Azure Machine Learning-Notebook-VM** ist eine vollständig verwaltete, cloudbasierte Arbeitsstation, die mehrere für Machine Learning installierte Tools und Umgebungen umfasst. Notebook-VMs können als Computeziel für kleinere Trainings- und Rückschlussaufträge verwendet werden. Bei großen Aufgaben sind [Azure Machine Learning-Computecluster](how-to-set-up-training-targets.md#amlcompute) mit Skalierungsmöglichkeiten dank mehrerer Knoten die bessere Wahl eines Computeziels.

Erfahren Sie mehr über Notebook-VMs.

### <a name="compute-targets"></a>Computeziele

Mit einem [Computeziel](concept-compute-target.md) können Sie die Computeressource angeben, auf der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Hierbei kann es sich um Ihren lokalen Computer oder eine cloudbasierte Computeressource handeln.

Informieren Sie sich weiter über die [verfügbaren Computeziele für das Training und die Bereitstellung](concept-compute-target.md).

### <a name="datasets-and-datastores"></a>Datasets und Datenspeicher

**Azure Machine Learning-Datasets** (Vorschauversion) erleichtern Ihnen den Zugriff auf Ihre Daten und die Arbeit damit. Datasets verwalten Daten in verschiedenen Szenarien, z.B. Modelltraining und Pipelineerstellung. Mithilfe des Azure Machine Learning SDK können Sie auf zugrunde liegenden Speicher zugreifen, Daten untersuchen und den Lebenszyklus unterschiedlicher Datasetdefinitionen verwalten.

Datasets bieten Methoden zum Arbeiten mit Daten in gängigen Formaten, wie z.B. mit `from_delimited_files()` oder `to_pandas_dataframe()`.

Weitere Informationen finden Sie unter [Erstellen und Registrieren von Azure Machine Learning Datasets](how-to-create-register-datasets.md).  Weitere Beispiele für die Verwendung von Datasets finden Sie in den [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/datasets).

Ein **Datenspeicher** ist eine Speicherabstraktion eines Azure-Speicherkontos. Der Datenspeicher kann entweder einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Back-End-Speicher verwenden. Jeder Arbeitsbereich verfügt über einen Standarddatenspeicher, und Sie können auch zusätzliche Datenspeicher registrieren. Verwenden Sie die Python SDK-API oder die Azure Machine Learning-CLI, um Dateien zu speichern und aus dem Datenspeicher abzurufen.

### <a name="endpoints"></a>Endpunkte

Ein Endpunkt ist eine Instanziierung Ihres Modells in einem Webdienst, der in der Cloud gehostet werden kann, oder in einem IoT-Modul für Bereitstellungen von integrierten Diensten.

#### <a name="web-service-endpoint"></a>Webdienst-Endpunkt

Wenn Sie ein Modell als Webdienst bereitstellen, kann der Endpunkt auf Azure Container Instances, Azure Kubernetes Service oder FPGAs bereitgestellt werden. Sie erstellen den Dienst aus Ihrem Modell, dem Skript und zugeordneten Dateien. Diese werden in ein Basiscontainerimage eingefügt, das die Ausführungsumgebung für das Modell enthält. Das Image verfügt über einen HTTP-Endpunkt mit Lastenausgleich, der die an den Webdienst gesendeten Bewertungsanforderungen empfängt.

Azure dient Ihnen als Hilfe beim Überwachen Ihres Webdiensts, indem die Application Insights- bzw. Modelltelemetriedaten erfasst werden, wenn Sie dieses Feature aktiviert haben. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihren Application Insights- und Speicherkontoinstanzen gespeichert.

Wenn Sie die automatische Skalierung aktiviert haben, führt Azure für Ihre Bereitstellung automatisch eine Skalierung durch.

Ein Beispiel für die Bereitstellung eines Modells als Webdienst finden Sie unter [Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

#### <a name="iot-module-endpoints"></a>IoT-Modulendpunkte

Ein bereitgestellter IoT-Modulendpunkt ist ein Docker-Container, der Ihr Modell und das zugeordnete Skript oder die Anwendung sowie alle zusätzlichen Abhängigkeiten enthält. Sie stellen diese Module mit Azure IoT Edge auf Edge-Geräten bereit.

Wenn Sie die Überwachung aktiviert haben, erfasst Azure Telemetriedaten aus dem Modell im Azure IoT Edge-Modul. Die Telemetriedaten sind nur für Sie zugänglich und werden in Ihrer Speicherkontoinstanz gespeichert.

Azure IoT Edge stellt sicher, dass Ihr Modul ausgeführt wird, und überwacht das Gerät, auf dem es gehostet wird.

### <a name="environments"></a>Umgebungen

Azure ML-Umgebungen werden verwendet, um die Konfiguration (Docker/Python/Spark/usw.) anzugeben, die dazu verwendet wird, eine reproduzierbare Umgebung für Datenaufbereitung, Modelltraining und Modellspeisung zu erstellen. Dabei handelt es sich um verwaltete und versionsverwaltete Entitäten innerhalb Ihres Azure Machine Learning-Arbeitsbereichs, die reproduzierbare, überprüfbare und portierbare Machine Learning-Workflows über verschiedene Computeziele hinweg ermöglichen.

Sie können ein Umgebungsobjekt auf Ihrem lokalen Compute verwenden, um Ihr Trainingsskript zu entwickeln, dieselbe Umgebung auf Azure Machine Learning Compute für das skalierte Modelltraining wiederverwenden und Ihr Modell sogar mit dieser selben Umgebung bereitstellen. 

Erfahren Sie, wie Sie für Training und Rückschluss [eine wiederverwendbare ML-Umgebung erstellen und verwalten](how-to-use-environments.md).

### <a name="estimators"></a>Schätzfunktionen

Um das Training von Modellen mit beliebten Frameworks zu vereinfachen, können Sie mit der Klasse der Schätzfunktionen (Estimator) problemlos Laufzeitkonfigurationen erstellen. Sie können eine generische [Schätzfunktion](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) erstellen und verwenden, um Trainingsskripts zu übermitteln, die ein beliebiges, von Ihnen ausgewähltes Learning-Framework verwenden (z. B. scikit-learn).

Für PyTorch-, TensorFlow- und Chainer-Aufgaben bietet Azure Machine Learning außerdem die passenden [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py)-, [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)- und [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py)-Schätzfunktionen, um die Verwendung dieser Frameworks zu vereinfachen.

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Trainieren von ML-Modellen mit Schätzfunktionen](how-to-train-ml-models.md).
* [Trainieren von PyTorch-Deep Learning-Modellen in großem Umfang mit Azure Machine Learning](how-to-train-pytorch.md).
* [Trainieren und Registrieren von TensorFlow-Modellen in großem Umfang mit Azure Machine Learning](how-to-train-tensorflow.md).
* [Trainieren und Registrieren von Chainer-Modellen in großem Umfang mit Azure Machine Learning](how-to-train-chainer.md).

### <a name="experiments"></a>Experimente

Ein Experiment ist eine Gruppierung vieler Ausführungen aus einem bestimmten Skript. Es gehört immer zu einem Arbeitsbereich. Beim Übermitteln einer Ausführung geben Sie einen Experimentnamen an. Die Informationen für die Ausführung werden unter diesem Experiment gespeichert. Wenn Sie eine Ausführung übermitteln und einen nicht vorhandenen Experimentnamen angeben, wird automatisch ein neues Experiment mit diesem neu angegebenen Namen erstellt.

Ein Beispiel zum Verwenden eines Experiments finden Sie im [Tutorial: Trainieren Ihres ersten Modells](tutorial-1st-experiment-sdk-train.md).


### <a name="github-tracking-and-integration"></a>GitHub-Nachverfolgung und -Integration

Wenn Sie eine Trainingsausführung starten, bei der das Quellverzeichnis ein lokales Git-Repository ist, werden Informationen über das Repository im Ausführungsverlauf gespeichert. Dies funktioniert für Ausführungen, die über eine Schätzfunktion, ML-Pipeline oder Skriptausführung übermittelt wurden. Dies funktioniert auch für Ausführungen, die aus dem SDK oder der Machine Learning-CLI übermittelt wurden.

Weitere Informationen finden Sie unter [Git-Integration für Azure Machine Learning](concept-train-model-git-integration.md).

### <a name="logging"></a>Protokollierung

Verwenden Sie beim Entwickeln Ihrer Lösung das Azure Machine Learning Python SDK in Ihrem Python-Skript, um beliebige Metriken zu protokollieren. Fragen Sie nach der Ausführung die Metriken ab, um zu ermitteln, ob von der Ausführung das Modell erzeugt wurde, das Sie bereitstellen möchten.

### <a name="ml-pipelines"></a>ML-Pipelines

Machine Learning-Pipelines werden zum Erstellen und Verwalten von Workflows verwendet, die Machine Learning-Phasen zusammenfügen. Eine Pipeline kann beispielsweise eine Datenaufbereitungs-, eine Modelltrainings-, eine Modellimplementierungs- und eine Rückschluss-/Bewertungsphase enthalten. Jede Phase kann mehrere Schritte umfassen, von denen wiederum jeder Schritt auf verschiedenen Computezielen unbeaufsichtigt ausgeführt werden kann. 

Pipelineschritte sind wiederverwendbar und lassen sich ausführen, ohne dass nachfolgende Schritte erneut ausgeführt werden müssen, wenn sich die Ausgabe dieses Schritts nicht geändert hat. Beispielsweise können Sie ein Modell erneut trainieren, ohne teure Schritte zur Datenvorbereitung erneut auszuführen, wenn sich die Daten nicht geändert haben. Pipelines ermöglichen Data Scientists auch, bei der Arbeit an separaten Bereichen eines Machine Learning-Workflows zusammenzuarbeiten.

Weitere Informationen zu Machine Learning-Pipelines für diesen Dienst finden Sie unter [Pipelines und Azure Machine Learning](concept-ml-pipelines.md).

### <a name="models"></a>Modelle

Im einfachsten Fall ist ein Modell ein Codeabschnitt, für den eine Eingabe verwendet wird, um eine Ausgabe zu erzeugen. Die Erstellung eines Machine Learning-Modells umfasst die Auswahl eines Algorithmus, die Bereitstellung der zugehörigen Daten und die Optimierung von Hyperparametern. Das Training ist ein iterativer Prozess, bei dem ein trainiertes Modell mit den während des Trainingsprozesses erlernten Informationen erzeugt wird.

Ein Modell wird erzeugt, indem in Azure Machine Learning eine Ausführung erfolgt. Sie können auch ein Modell verwenden, das außerhalb von Azure Machine Learning trainiert wurde. Sie können ein Modell in einem Azure Machine Learning-Arbeitsbereich registrieren.

Azure Machine Learning ist „framework-agnostisch“. Wenn Sie ein Modell erstellen, können Sie alle gängigen Frameworks für maschinelles Lernen verwenden, wie Scikit-learn, XGBoost, PyTorch, TensorFlow und Chainer.

Ein Beispiel zum Trainieren eines Modells mittels Scikit-learn und einer Schätzfunktion finden Sie im [Tutorial: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).

Die **Modellregistrierung** verfolgt alle Modelle in Ihrem Azure Machine Learning-Arbeitsbereich.

Die Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, geht die Registrierung davon aus, dass es sich um eine neue Version handelt. Die Version wird inkrementiert, und das neue Modell wird unter demselben Namen registriert.

Bei der Registrierung des Modells können Sie zusätzliche Metadatentags bereitstellen und diese Tags dann beim Suchen nach Modellen verwenden.

> [!TIP]
> Ein registriertes Modell ist ein logischer Container für eine oder mehrere Dateien, aus denen Ihr Modell besteht. Wenn Sie beispielsweise ein Modell verwenden, das in mehreren Dateien gespeichert ist, können Sie diese als einzelnes Modell in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Nach der Registrierung können Sie das registrierte Modell dann herunterladen oder bereitstellen und alle Dateien empfangen, die registriert wurden.

Sie können kein registriertes Modell löschen, das von einer aktiven Bereitstellung verwendet wird.

Ein Beispiel für das Registrieren eines Modells finden Sie unter [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning Service](tutorial-train-models-with-aml.md).

### <a name="runs"></a>Ausführungen

Eine Ausführung ist eine einzelne Ausführung eines Trainingsskripts. Azure Machine Learning zeichnet alle Ausführungen auf und speichert die folgenden Informationen:

* Metadaten zur Ausführung (Zeitstempel, Dauer usw.)
* Metriken, die von Ihrem Skript protokolliert werden
* Ausgabedateien, die automatisch vom Experiment gesammelt oder explizit von Ihnen hochgeladen werden
* Eine Momentaufnahme des Verzeichnisses, in dem Ihre Skripts enthalten sind, vor der Ausführung

Eine Ausführung wird ausgelöst, wenn Sie ein Skript zum Trainieren eines Modells übermitteln. Eine Ausführung kann über null oder mehr untergeordnete Elemente verfügen. Die Ausführung der obersten Ebene weist also unter Umständen zwei untergeordnete Ausführungen auf, die beide jeweils selbst eine untergeordnete Ausführung aufweisen können.

### <a name="run-configurations"></a>Laufzeitkonfigurationen

Eine Laufzeitkonfiguration ist ein Satz mit Anweisungen, mit denen definiert wird, wie ein Skript auf einem bestimmten Computeziel ausgeführt werden sollte. Die Konfiguration umfasst einen weiten Bereich von Verhaltensdefinitionen, z.B. die Vorgabe, ob eine vorhandene Python-Umgebung oder eine aus einer Spezifikation erstellte Conda-Umgebung verwendet werden soll.

Eine Laufzeitkonfiguration kann in einer Datei in dem Verzeichnis beständig gespeichert werden, in dem Ihr Trainingsskript enthalten ist, oder sie kann als In-Memory-Objekt erstellt und zum Übermitteln einer Ausführung verwendet werden.

Beispiele für Laufzeitkonfigurationen finden Sie unter [Auswählen und Verwenden eines Computeziels zum Trainieren Ihres Modells](how-to-set-up-training-targets.md).
### <a name="snapshots"></a>Momentaufnahmen

Beim Übermitteln einer Ausführung komprimiert Azure Machine Learning das Verzeichnis, in dem das Skript als ZIP-Datei enthalten ist, und sendet es an das Computeziel. Die ZIP-Datei wird dann extrahiert, und das Skript wird ausgeführt. Azure Machine Learning speichert die ZIP-Datei im Rahmen der Ausführungsaufzeichnung zudem als Momentaufnahme. Alle Benutzer mit Zugriff auf den Arbeitsbereich können eine Ausführungsaufzeichnung durchsuchen und die Momentaufnahme herunterladen.

> [!NOTE]
> Um zu verhindern, dass nicht benötigte Dateien in die Momentaufnahme eingeschlossen werden, erstellen Sie eine Ignorierdatei (GITIGNORE oder AMLIGNORE). Platzieren Sie diese Datei im Snapshot-Verzeichnis, und fügen Sie die zu ignorierenden Dateinamen hinzu. Für die AMLIGNORE-Datei werden die gleiche [Syntax und die gleichen Muster wie für die GITIGNORE-Datei](https://git-scm.com/docs/gitignore) verwendet. Wenn beide Dateien vorhanden sind, hat die AMLIGNORE-Datei Vorrang.

### <a name="training-scripts"></a>Trainingsskripts

Zum Trainieren eines Modells geben Sie das Verzeichnis an, in dem das Trainingsskript und die zugehörigen Dateien enthalten sind. Außerdem geben Sie einen Experimentnamen an, der zum Speichern der während des Trainings gesammelten Informationen verwendet wird. Beim Training wird das gesamte Verzeichnis in die Trainingsumgebung (Computeziel) kopiert und das von der Laufzeitkonfiguration angegebene Skript gestartet. Darüber hinaus wird auch eine Momentaufnahme des Verzeichnisses unter dem Experiment im Arbeitsbereich gespeichert.

Ein Beispiel finden Sie unter [Tutorial: Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).

### <a name="workspaces"></a>Arbeitsbereiche

[Der Arbeitsbereich](concept-workspace.md) ist die Ressource der obersten Ebene für Azure Machine Learning. Er bietet einen zentralen Ort für die Arbeit mit allen Artefakten, die Sie bei der Verwendung von Azure Machine Learning erstellen. Sie können einen Arbeitsbereich mit anderen Benutzern teilen. Eine detaillierte Beschreibung von Arbeitsbereichen finden Sie unter [Was ist ein Azure Machine Learning-Arbeitsbereich?](concept-workspace.md).

### <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Azure Machine Learning finden Sie unter:

* [Was ist Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md)
* [Tutorial (Teil 1): Trainieren eines Modells](tutorial-train-models-with-aml.md)
