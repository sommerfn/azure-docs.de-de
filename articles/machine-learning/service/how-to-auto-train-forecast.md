---
title: Automatisches Trainieren eines Modells für die Zeitreihenprognose
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mit Azure Machine Learning ein Regressionsmodell für die Zeitreihenprognose mit automatisiertem maschinellem Lernen trainieren.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 276e741a9462c19a3cba9ad1f9ac44e2da7ef1d3
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580709"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Automatisches Trainieren eines Modells für die Zeitreihenprognose
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie in Azure Machine Learning ein Regressionsmodell für die Zeitreihenprognose mit automatisiertem maschinellem Lernen trainieren. Das Konfigurieren eines Vorhersagemodells ähnelt zwar der Einrichtung eines Standard-Regressionsmodells mit automatisiertem maschinellem Lernen, die Verwendung von Zeitreihendaten erfordert jedoch bestimmte Konfigurationsoptionen und Vorverarbeitungsschritte. In den Beispielen dieses Artikels wird Folgendes gezeigt:

* Vorbereiten von Daten für die Zeitreihenmodellierung
* Konfigurieren spezifischer Zeitreihenparameter in einem Objekt vom Typ [`AutoMLConfig`](/python/api/azureml-train-automl/azureml.train.automl.automlconfig)
* Ausführen von Vorhersagen mit Zeitreihendaten

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GW]

Sie können automatisiertes Machine Learning verwenden, um verschiedene Techniken und Ansätze zu kombinieren. Außerdem erhalten Sie dabei eine beliebte und hochwertige Zeitreihenprognose. Automatisierte Zeitreihenexperimente werden als multivariate Regressionsprobleme behandelt. Zeitreihenwerte aus der Vergangenheit werden pivotiert und dienen so zusammen mit anderen Vorhersageelementen als zusätzliche Dimensionen für den Regressor.

Dieser Ansatz hat im Gegensatz zu klassischen Zeitreihenmethoden den Vorteil, dass mehrere kontextbezogene Variablen und deren Beziehungen zueinander beim Training auf natürliche Weise integriert werden. In der Praxis können bei Vorhersageanwendungen mehrere Faktoren die Vorhersage beeinflussen. Wenn z. B. Verkaufszahlen vorhergesagt werden sollen, wird das Ergebnis auf der Grundlage von Interaktionen von Trends aus der Vergangenheit, des Wechselkurses und des Preises berechnet. Ein weiterer Vorteil ist, dass alle aktuellen Innovationen bei Regressionsmodellen direkt auf die Vorhersagen angewendet werden.

Sie können u. a. [konfigurieren](#config), wie weit die Vorhersage in die Zukunft reichen soll (Vorhersagehorizont), und ob es Verzögerungen geben soll. Beim automatisierten Machine Learning wird ein zwar einfaches, aber häufig in interne Verzweigungen unterteiltes Modell für alle Elemente im Dataset und in den Vorhersagehorizonten erlernt. Dadurch sind mehr Daten verfügbar, um Modellparameter zu schätzen, und die Generalisierung von unbekannten Reihen wird möglich.

Features, die aus den Trainingsdaten extrahiert werden, spielen eine wichtige Rolle. Zudem werden beim automatisierten Machine Learning einige Standardschritte für die Vorverarbeitung durchgeführt, und es werden zusätzliche Zeitreihenfeatures generiert, um saisonale Auswirkungen zu erfassen und die Vorhersage so genau wie möglich zu gestalten.

## <a name="time-series-and-deep-learning-models"></a>Zeitreihen- und Deep Learning-Modelle


Automatisiertes maschinelles Lernen bietet Benutzern sowohl native Zeitreihen- als auch Deep Learning-Modelle als Teil des Empfehlungssystems. Zu diesen Lernmodellen gehören:
+ Prophet
+ Auto-ARIMA
+ ForecastTCN

Deep Learning mit automatisiertem maschinellem Lernen ermöglicht das Vorhersagen von ein- und mehrdimensionalen Zeitreihendaten.

Deep Learning-Modelle weisen drei intrinsische Funktionen auf:
1. Sie können von beliebigen Zuordnungen von Eingaben zu Ausgaben lernen.
1. Sie unterstützen mehrere Eingaben und Ausgaben.
1. Sie können automatisch Muster in Eingabedaten extrahieren, die lange Folgen umfassen.

Mit größeren Daten können Deep Learning-Modelle wie ForecastTCN von Microsofts die Scores des resultierenden Modells verbessern. 

Native Zeitreihenlernmodelle werden auch als Teil von automatisiertem maschinellem Lernen bereitgestellt. Prophet funktioniert am besten mit Zeitreihen, die starke saisonale Effekte aufweisen und viele Saisons von historischen Daten umfassen. Prophet ist schnell und genau, stabil gegenüber Ausreißern, fehlenden Daten und dramatischen Änderungen in den Zeitreihen. 

Der autoregressive integrierte gleitende Mittelwert (Autoregressive Integrated Moving Average, ARIMA) ist eine beliebte statistische Methode für Zeitreihenvorhersagen. Diese Vorhersagemethode wird häufig in kurzfristigen Vorhersageszenarien verwendet, in denen die Daten Hinweise auf Trends wie z. B. Zyklen enthalten. Solche Trends können unerwartet und schwierig zu modellieren oder vorherzusagen sein. Auto-ARIMA transformiert Ihre Daten in stationäre Daten, um konsistente, zuverlässige Ergebnisse zu erhalten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Arbeitsbereich. Informationen zum Erstellen des Arbeitsbereichs finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).
* In diesem Artikel werden Grundkenntnisse im Zusammenhang mit der Einrichtung eines Experiments mit automatisiertem maschinellem Lernen vorausgesetzt. Machen Sie sich anhand des [Tutorials](tutorial-auto-train-models.md) oder der [Anleitung](how-to-configure-auto-train.md) mit den grundlegenden Entwurfsmustern vertraut.

## <a name="preparing-data"></a>Vorbereiten der Daten

Der wichtigste Unterschied zwischen einem Regressionsaufgabentyp für die Vorhersage und einem regulären Regressionsaufgabentyp innerhalb des automatisierten maschinellen Lernens liegt in der Einbeziehung eines Features in Ihre Daten, das eine gültige Zeitreihe darstellt. Eine reguläre Zeitreihe besitzt ein klar definiertes und konsistentes Intervall sowie einen Wert an jedem Stichprobenpunkt in einem ununterbrochenen Zeitraum. Betrachten Sie die folgende Momentaufnahme der Datei `sample.csv`:

    day_datetime,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/4/2018,A,2300,36
    9/4/2018,B,550,36
    9/5/2018,A,2100,36
    9/5/2018,B,650,36
    9/6/2018,A,2400,36
    9/6/2018,B,700,36
    9/7/2018,A,2450,36
    9/7/2018,B,650,36

Dieses Dataset ist ein einfaches Beispiel für die täglichen Verkaufsdaten eines Unternehmens mit zwei Filialen: A und B. Darüber hinaus ist ein Feature für `week_of_year` vorhanden, das dem Modell die Erkennung der wöchentlichen Saisonalität ermöglicht. Das Feld `day_datetime` stellt eine bereinigte Zeitreihe mit täglichem Intervall dar. Das Feld `sales_quantity` ist die Zielspalte für laufende Vorhersagen. Lesen Sie die Daten in einen Pandas-Datenrahmen ein, und verwenden Sie anschließend die Funktion `to_datetime`, um sicherzustellen, dass eine Zeitreihe vom Typ `datetime` verwendet wird.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

In diesem Fall sind die Daten bereits aufsteigend nach dem Zeitfeld `day_datetime` sortiert. Bei der Einrichtung eines Experiments muss jedoch darauf geachtet werden, dass die gewünschte Zeitspalte in aufsteigender Reihenfolge sortiert wird, um eine gültige Zeitreihe zu erstellen. Nehmen Sie an, die Daten umfassen 1.000 Datensätze, und teilen Sie die Daten deterministisch auf, um Trainings- und Testdatasets zu erstellen. Bestimmen Sie den Spaltennamen der Bezeichnung, und legen Sie ihn auf die Bezeichnung fest. In diesem Beispiel lautet die Bezeichnung `sales_quantity`. Trennen Sie dann das Bezeichnungsfeld von `test_data`, um die `test_target`-Menge zu bilden.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!NOTE]
> Stellen Sie beim Trainieren eines Modells für die Vorhersage zukünftiger Werte sicher, dass alle während des Trainings verwendeten Features beim Ausführen von Vorhersagen für Ihren gewünschten Vorhersagehorizont verwendet werden können. Wenn Sie also beispielsweise eine Nachfrageprognose erstellen, lässt sich die Trainingsgenauigkeit durch die Einbeziehung eines Features für den aktuellen Aktienkurs erheblich verbessern. Wenn Sie bei Ihrer Vorhersage allerdings einen Vorhersagehorizont verwenden, der weit in der Zukunft liegt, lassen sich zukünftige Aktienkurse für zukünftige Zeitreihenpunkte ggf. nicht präzise vorhersagen, was sich nachteilig auf die Modellgenauigkeit auswirken kann.

<a name="config"></a>
## <a name="configure-and-run-experiment"></a>Konfigurieren und Ausführen des Experiments

Bei Vorhersageaufgaben nutzt das automatisierte maschinelle Lernen spezifische Vorverarbeitungs- und Schätzschritte für Zeitreihendaten. Folgende Vorverarbeitungsschritte werden ausgeführt:

* Erkennen des Intervalls der Zeitreihenstichprobe (etwa stündlich, täglich, wöchentlich) und Erstellen neuer Datensätze für fehlende Zeitpunkte, um eine ununterbrochene Reihe zu erhalten
* Imputieren fehlender Werte in der Zielspalte (mittels Forward-Fill) und der Featurespalte (mittels Median-Spaltenwerten)
* Erstellen granularitätsbasierter Features, um reihenübergreifend feste Effekte zu ermöglichen
* Erstellen zeitbasierter Features zur Ermittlung saisonaler Muster
* Codieren kategorischer Variablen zu numerischen Mengen

Das Objekt `AutoMLConfig` definiert die erforderlichen Einstellungen und Daten für eine Aufgabe mit automatisiertem maschinellem Lernen. Sie definieren Standardtrainingsparameter wie Aufgabentyp, Iterationsanzahl, Trainingsdaten und Anzahl von Kreuzvalidierungen (ähnlich wie bei einem Regressionsproblem). Bei Vorhersageaufgaben müssen allerdings noch weitere Parameter für das Experiment festgelegt werden. In der folgenden Tabelle werden die einzelnen Parameter sowie deren Verwendung erläutert:

| Parameter | BESCHREIBUNG | Erforderlich |
|-------|-------|-------|
|`time_column_name`|Dient zum Angeben der Datetime-Spalte in den Eingabedaten, die zum Erstellen der Zeitreihe sowie zum Ableiten des Intervalls verwendet wird.|✓|
|`grain_column_names`|Namen zum Definieren individueller Reihengruppen in den Eingabedaten. Ohne definierte Granularität wird bei dem Dataset von einer einzelnen Zeitreihe ausgegangen.||
|`max_horizon`|Definiert den maximal gewünschten Vorhersagehorizont in Einheiten von Zeitreihen. Die Einheiten basieren auf dem Zeitintervall Ihrer Trainingsdaten, z. B. monatlich oder wöchentlich, die vorhergesagt werden sollen.|✓|
|`target_lags`|Anzahl der Zeilen, um die die Zielwerte basierend auf der Häufigkeit der Daten verzögert werden sollen. Dies wird als eine Liste oder als einzelner Integer dargestellt. Die Verzögerung sollte verwendet werden, wenn die Beziehung zwischen den unabhängigen Variablen und der abhängigen Variable standardmäßig nicht übereinstimmt oder korreliert. Wenn Sie beispielsweise versuchen, die Nachfrage nach einem Produkt vorherzusagen, hängt die Nachfrage in einem Monat möglicherweise vom Preis für bestimmte Produkte vor 3 Monaten ab. In diesem Beispiel möchten Sie möglicherweise den Zielwert (Nachfrage) um 3 Monate negativ verzögern, sodass das Modell mit der richtigen Beziehung trainiert wird.||
|`target_rolling_window_size`|*n* Historische Zeiträume zum Generieren der vorhergesagten Werte, < = Größe Trainingsmenge. Wenn nicht angegeben, ist *n* die vollständige Trainingsmenge. Geben Sie diesen Parameter an, wenn Sie beim Trainieren des Modells nur eine bestimmte Menge des Verlaufs beachten möchten.||
|`enable_dnn`|Aktivieren Sie Vorhersage-DNNs.||

Weitere Informationen finden Sie in der [Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

Erstellen Sie die Zeitreiheneinstellungen als Wörterbuchobjekt. Legen Sie `time_column_name` auf das Feld `day_datetime` im Dataset fest. Definieren Sie den Parameter `grain_column_names`, um sicherzustellen, dass für die Daten **zwei separate Zeitreihengruppen** erstellt werden (jeweils eine für die Filialen A und B). Legen Sie abschließend `max_horizon` auf „50“ fest, um Vorhersagen für den gesamten Testsatz zu generieren. Legen Sie mit `target_rolling_window_size` ein Vorhersagefenster auf 10 Zeiträume fest, und geben Sie mit dem Parameter `target_lags` eine einzelne Verzögerung für das Ziel um 2 Zeiträume vorwärts an.

```python
time_series_settings = {
    "time_column_name": "day_datetime",
    "grain_column_names": ["store"],
    "max_horizon": 50,
    "target_lags": 2,
    "target_rolling_window_size": 10,
    "preprocess": True,
}
```

> [!NOTE]
> Die Schritte zur Vorverarbeitung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Vorverarbeitungsschritte automatisch auf Ihre Eingabedaten angewendet.

Durch die Definition von `grain_column_names` im obigen Codeausschnitt erstellt AutoML zwei separate Zeitreihengruppen, die auch als mehrfache Zeitreihen bezeichnet werden. Wenn keine Körnung definiert ist, geht AutoML davon aus, dass es sich bei dem Dataset um eine einzelne Zeitreihe handelt. Weitere Informationen zu einzelnen Zeitreihen finden Sie unter [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

Erstellen Sie als Nächstes ein Standardobjekt vom Typ `AutoMLConfig`, geben Sie den Aufgabentyp `forecasting` an, und übermitteln Sie das Experiment. Rufen Sie nach Fertigstellung des Modells die Iteration mit der besten Ausführung ab.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

Sehen Sie sich das Notebook zu Energiebedarf ([auto-ml-forecasting-energy-demand.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)) an. Dort finden Sie ausführliche Codebeispiele zu einer erweiterten Vorhersagekonfiguration, einschließlich:

* Feiertagserkennung und Erstellen zusätzlicher Merkmale (Featurization)
* Kreuzvalidierung mit rollierendem Ursprung (Rolling Origin Validation)
* Konfigurierbare Verzögerungen (Lags)
* Aggregierte Zeitfenstermerkmale (Rolling Window Features)

### <a name="configure-a-dnn-enable-forecasting-experiment"></a>Konfigurieren eines DNN-fähigen Vorhersageexperiments

> [!NOTE]
> Die DNN-Unterstützung für Vorhersagen beim automatisierten maschinellen Lernen befindet sich in der Vorschauphase.

Um DNNs für die Vorhersage zu nutzen, müssen Sie den Parameter `enable_dnn` in AutoMLConfig auf TRUE festlegen. 

Um DNNs verwenden zu können, wird die Verwendung eines AML-Computeclusters mit GPU-SKUs und mindestens zwei Knoten als Computeziel empfohlen. Weitere Informationen finden Sie in der [Dokumentation zu AML-Compute](how-to-set-up-training-targets.md#amlcompute). Weitere Informationen zu den VM-Größen mit GPUs finden Sie unter [Für GPU optimierte VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Es wird empfohlen, das Experimenttimeout auf mindestens einige Stunden festzulegen, damit ausreichend Zeit für das DNN-Training zur Verfügung steht.

### <a name="view-feature-engineering-summary"></a>Anzeigen der Zusammenfassung der Featureentwicklung

Für Aufgabentypen mit Zeitreihen beim automatisierten maschinellen Lernen können Sie Details aus dem Featureentwicklungsprozess anzeigen. Der folgende Code zeigt jedes Rohfeature sowie die folgenden Attribute:

* Name des Rohfeatures
* Anzahl der entwickelten Features, die aus diesem Rohfeature gebildet wurden
* Erkannter Typ
* Ob das Feature gelöscht wurde
* Liste der Featuretransformationen für dieses Rohfeature

```python
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
```

## <a name="forecasting-with-best-model"></a>Vorhersagen mit dem besten Modell

Verwenden Sie die beste Modelliteration, um Werte für das Testdataset vorherzusagen.

```python
predict_labels = fitted_model.predict(test_data)
actual_labels = test_labels.flatten()
```

Alternativ können Sie anstelle von `predict()` die Funktion `forecast()` verwenden, die die Festlegung ermöglicht, wann Vorhersagen beginnen sollen. Im folgenden Beispiel ersetzen Sie zunächst alle Werte in `y_pred` durch `NaN`. Der Ursprung der Vorhersage liegt in diesem Fall am Ende der Trainingsdaten, wie es normalerweise bei der Verwendung von `predict()` der Fall wäre. Wenn Sie jedoch nur die zweite Hälfte von `y_pred` durch `NaN` ersetzen, lässt die Funktion die numerischen Werte in der ersten Hälfte unverändert, sagt aber die `NaN`-Werte in der zweiten Hälfte voraus. Die Funktion gibt sowohl die vorhergesagten Werte als auch die angepassten Features zurück.

Sie können den Parameter `forecast_destination` in der Funktion `forecast()` auch verwenden, um Werte bis zu einem bestimmten Datum vorherzusagen.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_pipeline.forecast(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

Berechnen Sie den RMSE-Wert (Root Mean Squared Error) zwischen den tatsächlichen Werten (`actual_labels`) und den vorhergesagten Werten (`predict_labels`).

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_lables, predict_labels))
rmse
```

Nach der Ermittlung der allgemeinen Modellgenauigkeit besteht der nächste Schritt in der Regel darin, mithilfe des Modells unbekannte zukünftige Werte vorherzusagen. Stellen Sie einfach ein Dataset im gleichen Format wie das Testdataset `test_data`, aber mit zukünftigen Datums-/Uhrzeitwerten bereit, um einen Vorhersagesatz mit Vorhersagewerten für die einzelnen Zeitreihenschritte zu erhalten. Angenommen, die letzten Zeitreihendatensätze im Dataset waren für den 31.12.2018. Wenn Sie die Nachfrage für den Folgetag (oder für beliebig viele Vorhersagezeiträume < = `max_horizon`) vorhersagen möchten, erstellen Sie für jede Filiale einen einzelnen Zeitreihendatensatz für den 01.01.2019.

    day_datetime,store,week_of_year
    01/01/2019,A,1
    01/01/2019,A,1

Wiederholen Sie die erforderlichen Schritte, um diese zukünftigen Daten in einen Datenrahmen zu laden, und führen Sie anschließend `best_run.predict(test_data)` aus, um zukünftige Werte vorherzusagen.

> [!NOTE]
> Werte können nur für eine Anzahl von Zeiträumen vorhergesagt werden, die maximal dem Wert von `max_horizon` entspricht. Das Modell muss mit einem weiter in der Zukunft liegenden Vorhersagehorizont neu trainiert werden, um zukünftige Werte vorhersagen können, die über den aktuellen Vorhersagehorizont hinausgehen.

## <a name="next-steps"></a>Nächste Schritte

* Absolvieren Sie das [Tutorial](tutorial-auto-train-models.md), um zu erfahren, wie Sie Experimente mit automatisiertem maschinellem Lernen erstellen.
* Machen Sie sich mit der Referenzdokumentation [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) vertraut.
