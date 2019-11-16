---
title: Analysieren und Überwachen von Datasets auf Datendrift (Vorschau)
titleSuffix: Azure Machine Learning
description: Erstellen Sie Monitore für Azure Machine Learning-Datasets (Vorschau), überwachen Sie die Datasets auf Datendrift, und richten Sie Warnungen ein.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
ms.author: copeters
author: lostmygithubaccount
ms.date: 11/04/2019
ms.openlocfilehash: 24b9b120240ffc6f7dd2252d12c9f8af2bcfafbc
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74049171"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Erkennen von Datendrift (Vorschau) in Datasets
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning-Datasetmonitore (Vorschau) erstellen, die Datasets auf Datendrift und statistische Änderungen überwachen und Warnungen einrichten.

Azure Machine Learning-Datasetmonitore ermöglichen Folgendes:
* **Analysieren der Drift in Ihren Daten**, um zu verstehen, wie diese sich im Lauf der Zeit ändern.
* **Überwachen von Modelldaten** auf Änderungen zwischen Trainings- und Nutzungsdatasets.
* **Überwachen von neuen Daten** auf Änderungen zwischen Baseline- und Zieldatasets.
* **Erstellen von Profilen für Features in Daten**, um nachzuverfolgen, wie sich statistische Eigenschaften im Lauf der Zeit ändern.
* **Einrichten von Warnungen zur Datendrift**, um bei potenziellen Problemen frühzeitig eine Benachrichtigung zu erhalten. 

Metriken und Einblicke sind über die [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)-Ressource verfügbar, die mit dem Azure Machine Learning Service-Arbeitsbereich verknüpft ist.

> [!Important]
> Beachten Sie, dass die Überwachung der Datendrift mit dem SDK in allen Editionen verfügbar ist, während dies per Studio im Web nur für die Enterprise-Edition gilt.

## <a name="prerequisites"></a>Voraussetzungen

Um Datasetmonitore zu erstellen und zu nutzen, benötigen Sie Folgendes:
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.
* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).
* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.
* Strukturierte (tabellarische) Daten mit einem Zeitstempel im Dateipfad, im Dateinamen oder in einer Spalte in den Daten.

## <a name="what-is-data-drift"></a>Was sind Datenabweichungen?

Im Zusammenhang mit maschinellem Lernen ist die Datenabweichung die Änderung der Modelleingabedaten, die zu einem Abfallen der Modellleistung führt. Datendrift ist einer der Hauptgründe für die Abnahme der Modellgenauigkeit im Lauf der Zeit, daher hilft das Überwachen auf Datendrift bei der Erkennung von Problemen bei der Leistung eines Modells.

Folgende Aspekte zählen zu den Gründen für Datendrift: 

- Vorgelagerte Prozessänderungen, z. B. durch einen ausgetauschten Sensor, der die Maßeinheiten von Zoll zu Zentimeter ändert. 
- Probleme mit der Datenqualität, z. B. durch einen defekten Sensor, der immer den Messwert 0 liefert.
- Natürliche Abweichungen in den Daten, z. B. durch Änderungen der mittleren Temperatur in den Jahreszeiten.
- Änderungen in der Beziehung zwischen Features oder Kovariantenabweichungen. 

Mit Azure Machine Learning-Datasetmonitoren können Sie Warnungen einrichten, die Ihnen beim Erkennen von Datendrift in Datasets im Lauf der Zeit helfen. 

### <a name="dataset-monitors"></a>Datasetmonitore 

Sie können einen Datasetmonitor erstellen, um Datendrift in neuen Daten in einem Dataset zu erkennen und eine entsprechende Warnung zu erhalten, Verlaufsdaten für die Drift zu analysieren und für neue Daten ein Profil im Zeitverlauf zu erstellen. Der Datendriftalgorithmus misst Änderungen von Daten allgemein und bietet Hinweise darauf, welche Features die Ursache sind, damit diese genauer untersucht werden können. Datasetmonitore generieren eine Reihe weiterer Metriken, indem für neue Daten im Dataset `timeseries` ein Profil erstellt wird. Über [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) können benutzerdefinierte Warnungen für alle vom Monitor generierten Metriken eingerichtet werden. Datasetmonitore können verwendet werden, um Datenprobleme schnell zu erfassen und den Zeitraum bis zum Beheben des Problems zu verkürzen, indem wahrscheinliche Ursachen ermittelt werden.  

Aus konzeptioneller Sicht gibt es drei primäre Szenarien für die Einrichtung von Datasetmonitoren in Azure Machine Learning.

Szenario | BESCHREIBUNG
---|---
Überwachen der Nutzungsdaten eines Modells auf Drift von den Trainingsdaten des Modells | Angesichts der Tatsache, dass die Modellgenauigkeit abnimmt, wenn die Nutzungsdaten von den Trainingsdaten abweichen, können die Ergebnisse dieses Szenarios als Überwachung eines Indikators für die Modellgenauigkeit interpretiert werden.
Überwachen eines Zeitreihendatasets auf Drift von einem vorherigen Zeitraum | Dieses Szenario ist allgemeiner und kann zum Überwachen von Datasets verwendet werden, die an Prozessen vor oder nach der Modellerstellung beteiligt sind.  Das Zieldataset muss eine Zeitstempelspalte aufweisen, das Baselinedataset dagegen kann jedes tabellarische Dataset sein, das gemeinsame Features mit dem Zieldataset besitzt.
Analysieren von Daten der Vergangenheit | Dieses Szenario hilft dabei, frühere Daten zu verstehen und fundierte Entscheidungen für die Einstellungen von Datasetmonitoren zu treffen.

## <a name="how-dataset-can-monitor-data"></a>Überwachen von Daten im Dataset

Mit Azure Machine Learning lässt sich die Datendrift in Datasets überwachen. Zum Überwachen von Datenabweichungen wird ein Baseline-Dataset angegeben. Dies ist normalerweise das Trainingsdataset für ein Modell. Ein Zieldataset – in der Regel Modelleingabedaten – wird im Lauf der Zeit mit dem Baselinedataset verglichen. Das bedeutet, dass im Zieldataset eine Zeitstempelspalte angegeben sein muss.

### <a name="set-the-timeseries-trait-in-the-target-dataset"></a>Festlegen des `timeseries`-Merkmals im Zieldataset

Für das Zieldataset muss das `timeseries`-Merkmal festgelegt sein, indem die Zeitstempelspalte entweder anhand einer Spalte in den Daten oder einer aus dem Pfadmuster der Dateien abgeleiteten virtuellen Spalte angegeben wird. Dies kann über das Python SDK oder im Azure Machine Learning-Studio erfolgen. Zum Hinzufügen eines `timeseries`-Merkmals zum Dataset muss eine Spalte angegeben werden, die einen „differenzierten“ Zeitstempel repräsentiert. Wenn Ihre Daten in eine Ordnerstruktur mit Zeitinformationen partitioniert sind – z. B. „{yyyy/MM/dd}“, können Sie über die Einstellung für das Pfadmuster eine virtuelle Spalte erstellen und als „undifferenzierter“ Zeitstempel festlegen, um die Wichtigkeit der Zeitreihenfunktionalität zu erhöhen. 

#### <a name="python-sdk"></a>Python SDK

Die [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-)-Methode der [`Dataset`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-)-Klasse definiert die Zeitstempelspalte für das Dataset. 

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

Ein vollständiges Beispiel für die Verwendung des `timeseries`-Merkmals von Datasets finden Sie im [Beispiel-Notebook](https://aka.ms/azureml-tsd-notebook) oder in der [Dokumentation zum SDK für das Dataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-).

#### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku-inline.md)]

Wenn Sie Ihr Dataset mithilfe des Azure Machine Learning-Studios erstellen, stellen Sie sicher, dass der Pfad zu Ihren Daten Zeitstempelinformationen enthält, schließen Sie alle Unterordner mit Daten ein, und legen Sie das Partitionsformat fest. 

Im folgenden Beispiel werden alle Daten im Unterordner *NoaaIsdFlorida/2019* einbezogen, und das Partitionsformat gibt Jahr, Monat und Tag des Zeitstempels an. 

[![Partitionsformat](media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

Geben Sie in den Einstellungen für das **Schema** die Zeitstempelspalte anhand einer virtuellen oder realen Spalte im angegebenen Dataset an:

![Timestamp](media/how-to-monitor-datasets/timestamp.png)

## <a name="dataset-monitor-settings"></a>Einstellungen für den Datasetmonitor

Sobald Sie das Dataset mit den angegebenen Zeitstempeleinstellungen erstellt haben, können Sie den Datasetmonitor konfigurieren.

Die verschiedenen Einstellungen für einen Datasetmonitor lassen sich in drei Gruppen unterteilen: **Grundlegende Informationen, Monitoreinstellungen** und **Abgleichseinstellungen**.

### <a name="basic-info"></a>Grundlegende Informationen

Diese Tabelle enthält grundlegende Einstellungen, die für den Datasetmonitor verwendet werden.

| Einstellung | BESCHREIBUNG | Tipps | Veränderlich | 
| ------- | ----------- | ---- | ------- | 
| NAME | Name des Datasetmonitors. | | Nein |
| Baselinedataset | Tabellarisches Dataset, das als Baseline für den Vergleich mit dem Zieldataset im Lauf der Zeit verwendet wird. | Das Baselinedataset muss gemeinsame Features mit dem Zieldataset aufweisen. Im Allgemeinen sollte die Baseline auf ein Trainingsdataset des Modells oder auf einen Slice des Zieldatasets festgelegt werden. | Nein |
| Zieldataset | Tabellarisches Dataset mit angegebener Zeitstempelspalte, das im Hinblick auf Datendrift analysiert wird. | Das Zieldataset muss gemeinsame Features mit dem Baselinedataset aufweisen, und es sollte sich um ein `timeseries`-Dataset handeln, an das neue Daten angefügt werden. Es können Daten der Vergangenheit im Zieldataset analysiert oder neue Daten überwacht werden. | Nein | 
| Frequency | Dies ist die Häufigkeit, die zum Planen des Pipelineauftrags und zum Analysieren von Daten der Vergangenheit verwendet wird, wenn ein Abgleich durchgeführt wird. Verfügbare Optionen sind „täglich“, „wöchentlich“ oder „monatlich“. | Passen Sie diese Einstellung an, um eine vergleichbare Datenmenge in die Baseline einzubeziehen. | Nein | 
| Features | Liste der Features, die im Hinblick auf Datendrift im Lauf der Zeit analysiert werden. | Legen Sie diese Einstellung auf die Ausgabefeatures eines Modells fest, um eine konzeptionelle Drift zu messen. Schließen Sie keine Features ein, für die im Lauf der Zeit eine natürliche Drift auftritt (Monat, Jahr, Index usw.). Nach dem Anpassen der Featureliste können Sie einen Abgleich für alle vorhandenen Datendriftmonitore durchführen. | Ja | 
| Computeziel | Azure Machine Learning-Computeziel zur Ausführung der Aufträge des Datasetmonitors. | | Ja | 

### <a name="monitor-settings"></a>Monitoreinstellungen

Diese Einstellungen gelten für die geplante Datasetmonitor-Pipeline, die erstellt wird. 

| Einstellung | BESCHREIBUNG | Tipps | Veränderlich | 
| ------- | ----------- | ---- | ------- |
| Aktivieren | Aktivieren oder Deaktivieren des Zeitplans in der Pipeline des Datasetmonitors. | Deaktivieren Sie diese Einstellung, um Daten der Vergangenheit mithilfe der Abgleicheinstellung zu analysieren. Die Einstellung kann nach dem Erstellen des Datasetmonitors aktiviert werden. | Ja | 
| Latency | Der Zeitraum in Stunden, der verstreicht, bis Daten im Dataset eintreffen. Wenn es z. B. drei Tage dauert, bis Daten in der SQL-Datenbank eintreffen, die vom Dataset gekapselt wird, legen Sie die Latenz auf den Wert 72 fest. | Die Einstellung kann nach dem Erstellen des Datasetmonitors nicht geändert werden. | Nein | 
| E-Mail-Adressen | E-Mail-Adressen, an die Warnungen gesendet werden, wenn eine Verletzung des Schwellenwerts für den Prozentsatz der Datendrift ermittelt wird. | Das Senden der E-Mails erfolgt über Azure Monitor. | Ja | 
| Schwellenwert | Schwellenwert für den Prozentsatz der Datendrift für Warnungen per E-Mail. | Weitere Warnungen und Ereignisse können in vielen weiteren Metriken in der dem Arbeitsbereich zugeordneten Application Insights-Ressource festgelegt werden. | Ja | 

### <a name="backfill-settings"></a>Abgleichseinstellungen

Diese Einstellungen gelten für die Durchführung eines Abgleichs von Vergangenheitsdaten für Datendriftmetriken.

| Einstellung | BESCHREIBUNG | Tipps |
| ------- | ----------- | ---- |
| Startdatum | Startdatum des Abgleichsauftrags. | | 
| Enddatum | Enddatum des Abgleichsauftrags. | Dieses darf nicht mehr als 31 Zeiteinheiten für die Häufigkeit vom Startdatum abweichen. In einem vorhandenen Datasetmonitor kann ein Metrikabgleich durchgeführt werden, um Daten der Vergangenheit zu analysieren oder Metriken durch aktualisierte Einstellungen zu ersetzen. |

## <a name="create-dataset-monitors"></a>Erstellen von Datasetmonitoren 

Erstellen Sie Datasetmonitore, um mithilfe des Azure Machine Learning-Studios oder des Python SDKs Datendrift in einem neuen Dataset zu ermitteln und entsprechende Warnungen zu senden. 

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku-inline.md)]

Damit Sie Warnungen für Ihren Datasetmonitor festlegen können, muss der Arbeitsbereich, der das Dataset enthält, für das Sie einen Monitor erstellen möchten, über Funktionen der Enterprise-Edition verfügen. 

Wenn Sie sich davon überzeugt haben, dass der Arbeitsbereich über die notwendige Funktionalität verfügt, navigieren Sie zur Startseite des Studios, und wählen Sie auf der linken Seite die Registerkarte „Datasets“ aus. Wählen Sie „Datasetmonitore“ aus.

![Monitorliste](media/how-to-monitor-datasets/monitor-list.png)

Klicken Sie auf die Schaltfläche **+ Monitor erstellen**, und fahren Sie mit dem Assistenten fort, indem Sie auf **Weiter** klicken.

![Genie](media/how-to-monitor-datasets/wizard.png)

Der resultierende Datasetmonitor wird in der Liste angezeigt. Wählen Sie diesen Eintrag aus, um zur Detailseite für den Monitor zu gelangen.

### <a name="from-python-sdk"></a>Über das Python SDK

Vollständige Informationen finden Sie in der [Python SDK-Referenzdokumentation zur Datendrift](/python/api/azureml-datadrift/azureml.datadrift). 

Der folgende Code ist ein Beispiel für die Erstellung eines Datasetmonitors mit dem Python SDK.

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
dset = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

Ein vollständiges Beispiel für die Einrichtung eines `timeseries`-Datasets und einer Datendrifterkennung finden Sie in unserem [Beispiel-Notebook](https://aka.ms/datadrift-notebook).

## <a name="understanding-data-drift-results"></a>Verstehen der Datendriftergebnisse

Der Datenmonitor erzeugt zwei Gruppen von Ergebnissen: Driftübersicht und Featuredetails. Die folgende Animation zeigt die verfügbaren Diagramme für den Driftmonitor basierend auf dem ausgewählten Feature und der ausgewählten Metrik. 

![Demovideo](media/how-to-monitor-datasets/video.gif)

### <a name="drift-overview"></a>Driftübersicht

Der Abschnitt **Driftübersicht** enthält allgemeine Erkenntnisse zur Größenordnung der Datendrift und zu den Features, die genauer untersucht werden sollten. 

| Metrik | BESCHREIBUNG | Tipps | 
| ------ | ----------- | ---- | 
| Größenordnung der Datendrift | Wird als Prozentsatz zwischen Baseline- und Zieldataset im Lauf der Zeit angegeben. Werte liegen zwischen 0 und 100, wobei 0 auf identische Datasets hinweist und 100 bedeutet, dass die Datendriftfunktion von Azure Machine Learning die beiden Datasets vollkommen voneinander unterscheiden kann. | Aufgrund der Machine Learning-Techniken, die zum Generieren dieser Größenordnung verwenden werden, ist ein gewisses Maß an Ungenauigkeit beim gemessenen Prozentsatz zu erwarten. | 
| Driftbeitrag nach Feature | Der Beitrag jedes Features im Zieldataset zur gemessenen Größenordnung der Drift. |  Aufgrund von Kovariantenabweichungen muss sich die zugrunde liegende Verteilung eines Features nicht notwendigerweise ändern, um eine relativ hohe Featurerelevanz aufzuweisen. | 

Die folgende Abbildung zeigt ein Beispiel für Diagramme, die in den Ergebnissen in der **Driftübersicht** im Azure Machine Learning-Studio angezeigt werden und aus einem Abgleich des Datasets [NOAA Integrated Surface Data](https://azure.microsoft.com/services/open-datasets/catalog/noaa-integrated-surface-data/) stammen. Die Daten wurden für die Bedingung `stationName contains 'FLORIDA'` erfasst, wobei Januar 2019 als Baselinedataset und sämtliche Daten von 2019 als Zieldataset verwendet wurden.
 
![Driftübersicht](media/how-to-monitor-datasets/drift-overview.png)

### <a name="feature-details"></a>Featuredetails

Der Abschnitt **Featuredetails** enthält Erkenntnisse auf Featureebene zu Änderungen an der Verteilung des ausgewählten Features sowie weitere statistische Daten im Zeitverlauf. 

Für das Zieldataset wird auch ein Profil im Zeitverlauf erstellt. Der statistische Abstand zwischen der Baselineverteilung jedes Features wird mit dem zeitlichen Verlauf des Zieldatasets verglichen. Dies entspricht konzeptionell der Größenordnung der Datendrift, außer dass diese Berechnung für ein einzelnes Feature erfolgt. Mindest-, Maximal- und Mittelwerte sind ebenfalls verfügbar. 

Wenn Sie im Azure Machine Learning-Studio auf einen Datenpunkt im Diagramm klicken, wird die Verteilung des angezeigten Features entsprechend angepasst. Standardmäßig werden die Verteilung des Baselinedatasets und die Verteilung der letzten Ausführung desselben Features angezeigt. 

Diese Metriken können auch im Python SDK mithilfe der `get_metrics()`-Methode in einem `DataDriftDetector`-Objekt abgerufen werden. 

#### <a name="numeric-features"></a>Numerische Features 

Für numerische Features wird in jeder Ausführung des Datasetmonitors ein Profil erstellt. Folgendes wird im Azure Machine Learning-Studio verfügbar gemacht. Für die Verteilung wird die Wahrscheinlichkeitsdichte angezeigt.

| Metrik | BESCHREIBUNG |  
| ------ | ----------- |  
| Wasserstein-Distanz | Der Mindestarbeitsaufwand, der für die Transformation der Baselineverteilung in die Zielverteilung erforderlich ist. |
| Mittelwert | Durchschnittlicher Wert des Features. |
| Mindestwert | Minimaler Wert des Features. |
| Maximalwert | Maximaler Wert des Features. |

![Featuredetails, numerisch](media/how-to-monitor-datasets/feature-details.png)

#### <a name="categorical-features"></a>Kategorische Features 

Für numerische Features wird in jeder Ausführung des Datasetmonitors ein Profil erstellt. Folgendes wird im Azure Machine Learning-Studio verfügbar gemacht. Für die Verteilung wird ein Histogramm angezeigt.

| Metrik | BESCHREIBUNG |  
| ------ | ----------- |  
| Euklidischer Abstand | Geometrischer Abstand zwischen den Baseline- und Zielverteilungen. |
| Eindeutige Werte | Anzahl eindeutiger Werte (Kardinalität) des Features. |


![Featuredetails, kategorisch](media/how-to-monitor-datasets/feature-details2.png)

## <a name="metrics-alerts-and-events"></a>Metriken, Warnungen und Ereignisse

Metriken können in der [Azure Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)-Ressource abgefragt werden, die Ihrem Machine Learning-Arbeitsbereich zugeordnet ist. Hierdurch erhalten Sie Zugriff auf alle Features von Application Insights, z. B. auch zur Einrichtung von benutzerdefinierten Warnungsregeln und Aktionsgruppen zum Auslösen einer Aktion wie E-Mail/SMS/Pushübertragung/Sprachnachricht oder einer Azure-Funktion. Ausführlichere Informationen hierzu finden Sie in der vollständigen Application Insights-Dokumentation. 

Navigieren Sie zunächst zum Azure-Portal, und wählen Sie die Seite **Übersicht** Ihres Arbeitsbereichs aus.  Die zugeordnete Application Insights-Ressource befindet sich ganz rechts:

[![Azure-Portal – Übersicht](media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Wählen Sie im linken Bereich unter „Überwachung“ die Option „Protokolle (Analytics)“ aus:

![Application Insights-Übersicht](media/how-to-monitor-datasets/ai-overview.png)

Die Metriken der Datasetüberwachung werden als `customMetrics` gespeichert. Sie können eine einfache Abfrage schreiben und ausführen, nachdem Sie eine Überwachung per Datasetmonitor eingerichtet haben, um die Metriken anzuzeigen:

[![Log Analytics-Abfrage](media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Erstellen Sie eine neue Warnungsregel, nachdem Sie die Metriken für die Einrichtung von Warnungsregeln identifiziert haben:

![Neue Warnungsregel](media/how-to-monitor-datasets/alert-rule.png)

Sie können eine vorhandene oder eine neue Aktionsgruppe verwenden, um die Aktion zu definieren, die durchgeführt werden soll, wenn die festgelegten Bedingungen erfüllt sind:

![Neue Aktionsgruppe](media/how-to-monitor-datasets/action-group.png)

## <a name="troubleshooting"></a>Problembehandlung

Einschränkungen und bekannte Probleme:

* Der Zeitbereich von Abgleichsaufträgen ist auf 31 Intervalle der Häufigkeitseinstellung des Monitors beschränkt. 
* Es besteht eine Beschränkung auf 200 Features, es sei denn, es wurde keine Featureliste angegeben (alle Features werden verwendet).
* Die Computegröße muss ausreichend sein, um die Daten zu verarbeiten. 
* Stellen Sie sicher, dass Ihr Dataset über Daten innerhalb des Zeitraums verfügt, der durch das Start- und Enddatum für die jeweilige Ausführung der Überwachung festgelegt ist.

Spalten bzw. Features im Dataset werden basierend auf den Bedingungen in der unten angegebenen Tabelle als kategorisch oder numerisch klassifiziert. Wenn ein Feature diese Bedingungen nicht erfüllt – beispielsweise bei einer Spalte vom Typ „string“ mit mehr als 100 eindeutigen Werten –, wird es aus dem Datendriftalgorithmus entfernt. In die Profilerstellung wird es aber einbezogen. 

| Featuretyp | Datentyp | Bedingung | Einschränkungen | 
| ------------ | --------- | --------- | ----------- |
| Kategorisch | string, bool, int, float | Die Anzahl von eindeutigen Werten im Feature ist kleiner als 100 und geringer als 5 % der Anzahl von Zeilen. | NULL wird als eigene Kategorie behandelt. | 
| Numerisch | int, float | Weist einen numerischen Datentyp auf und erfüllt die Bedingungen für ein kategorisches Feature nicht. | Das Feature wird entfernt, wenn mehr als 15 % der Werte NULL sind. | 

## <a name="next-steps"></a>Nächste Schritte

* Fahren Sie mit dem [Azure Machine Learning-Studio](https://ml.azure.com) oder dem [Python-Notebook](https://aka.ms/datadrift-notebook) fort, um einen Datasetmonitor zu erstellen.
* Informieren Sie sich, wie Sie die Datendrift für [in Azure Kubernetes Service bereitgestellte Modelle](how-to-monitor-data-drift.md) einrichten.
* Richten Sie Datendriftüberwachungen mit [Ereignisraster](how-to-use-event-grid.md) ein. 