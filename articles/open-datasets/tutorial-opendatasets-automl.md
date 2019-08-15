---
title: 'Tutorial: Anreichern eines automatisierten Machine Learning-Modells'
titleSuffix: Azure Open Datasets
description: Erfahren Sie, wie Sie die Benutzerfreundlichkeit von öffentlichen Azure-Datasets und die Leistungsfähigkeit von Azure Machine Learning Service nutzen, um ein Regressionsmodell zum Vorhersagen von Standardtaxipreisen in New York zu erstellen.
services: open-datasets
ms.service: open-datasets
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 05/02/2019
ms.openlocfilehash: 6f72daa4a601df0e3592910645c2f9b35ab64431
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845821"
---
# <a name="tutorial-build-a-regression-model-with-automated-machine-learning-and-open-datasets"></a>Tutorial: Erstellen eines Regressionsmodells mit automatisiertem maschinellem Lernen und öffentlichen Datasets

In diesem Tutorial nutzen Sie die Benutzerfreundlichkeit von öffentlichen Azure-Datasets und die Leistungsfähigkeit von Azure Machine Learning Service, um ein Regressionsmodell zum Vorhersagen von Standardtaxipreisen in New York zu erstellen. Laden Sie öffentlich verfügbare Taxi-, Feiertags- und Wetterdaten einfach herunter, und konfigurieren Sie ein Experiment mit automatisiertem maschinellem Lernen über Azure Machine Learning Service. Dieser Prozess akzeptiert Trainingsdaten und Konfigurationseinstellungen und durchläuft automatisch Kombinationen der verschiedenen Methoden, Modelle und Hyperparametereinstellungen zur Featurenormalisierung/-standardisierung, um zum besten Modell zu gelangen.

In diesem Tutorial lernen Sie Folgendes:

- Konfigurieren eines Azure Machine Learning-Dienstarbeitsbereichs
- Einrichten einer lokalen Python-Umgebung
- Zugreifen auf Daten, Transformieren von Daten und Verknüpfen von Daten mit öffentlichen Azure-Datasets
- Trainieren eines Regressionsmodells mit automatisiertem maschinellem Lernen
- Berechnen der Modellgenauigkeit

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial gelten folgende Voraussetzungen:

* Azure Machine Learning Service-Arbeitsbereich
* Eine Python 3.6-Umgebung

### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Befolgen Sie die [Anweisungen](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-workspace) zum Erstellen eines Arbeitsbereichs über das Azure-Portal, sofern Sie noch nicht über einen verfügen. Notieren Sie sich nach der Erstellung Ihren Arbeitsbereichsnamen, den Ressourcengruppennamen und die Abonnement-ID.

### <a name="create-a-python-environment"></a>Erstellen einer Python-Umgebung

Dieses Beispiel verwendet eine Anaconda-Umgebung mit Jupyter Notebooks, Sie können diesen Code jedoch in jeder 3.6.x-Umgebung und mit jedem Text-Editor bzw. jeder IDE ausführen. Führen Sie die folgenden Schritte aus, um eine neue Entwicklungsumgebung zu erstellen.

1. Wenn Sie noch nicht über Anaconda verfügen [laden Sie es herunter](https://www.anaconda.com/distribution/), und installieren Sie es. Wählen Sie dabei die **Python-Version 3.7** aus.
1. Öffnen Sie eine Anaconda-Eingabeaufforderung, und erstellen Sie eine neue Umgebung. Es dauert einige Minuten, bis die Komponenten und Pakete heruntergeladen wurden und die Umgebung erstellt wurde.
    ```
    conda create -n tutorialenv python=3.6.5
    ```
1. Aktivieren Sie die Umgebung.
    ```
    conda activate tutorialenv
    ```
1. Aktivieren Sie umgebungsspezifische IPython-Kernel.
    ```
    conda install notebook ipykernel
    ```
1. Erstellen Sie den Kernel.
    ```
    ipython kernel install --user
    ```
1. Installieren Sie die Pakete, die Sie für dieses Tutorial benötigen. Diese Pakete sind umfangreich, und die Installation dauert 5 bis 10 Minuten.
    ```
    pip install azureml-sdk[automl] azureml-opendatasets
    ```
1. Starten Sie einen Notebook-Kernel aus Ihrer Umgebung.
    ```
    jupyter notebook
    ```

Wenn Sie diese Schritte abgeschlossen haben, klonen Sie das [Notebookrepository für öffentliche Datasets](https://github.com/Azure/OpenDatasetsNotebooks), und öffnen Sie das Notebook **tutorials/taxi-automl/01-tutorial-opendatasets-automl.ipynb**, um es auszuführen.

## <a name="download-and-prepare-data"></a>Herunterladen und Vorbereiten von Daten

Importieren Sie die erforderlichen Pakete. Das Paket mit öffentlichen Datasets enthält eine Klasse für jede Datenquelle (z. B. `NycTlcGreen`), damit Sie vor dem Herunterladen Datumsparameter ganz einfach filtern können.


```python
from azureml.opendatasets import NycTlcGreen
import pandas as pd
from datetime import datetime
from dateutil.relativedelta import relativedelta
```

Beginnen Sie, indem Sie einen Datenrahmen für die Taxidaten erstellen. Bei der Arbeit in einer Spark-fremden Umgebung ermöglichen öffentliche Datasets nur jeweils das Herunterladen der Daten eines Monats mit bestimmten Klassen, um `MemoryError` bei großen Datasets zu vermeiden. Um die Taxidaten eines Jahres herunterzuladen, rufen Sie iterativ jeweils einen Monat ab, und nehmen Sie nach dem Zufallsprinzip eine Stichprobe von 2.000 Einträgen aus jedem Monat, bevor Sie die Daten an `green_taxi_df` anhängen. So verhindern Sie, dass der Datenrahmen überfrachtet wird. Zeigen Sie dann eine Vorschau der Daten an.

>[!NOTE]
> Öffentliche Datasets enthalten gespiegelte Klassen für die Arbeit in Spark-Umgebungen, bei der Datengröße und Speicher nicht relevant sind.

```python
green_taxi_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016", "%m/%d/%Y")
end = datetime.strptime("1/31/2016", "%m/%d/%Y")

for sample_month in range(12):
    temp_df_green = NycTlcGreen(start + relativedelta(months=sample_month), end + relativedelta(months=sample_month)) \
        .to_pandas_dataframe()
    green_taxi_df = green_taxi_df.append(temp_df_green.sample(2000))

green_taxi_df.head(10)
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
      <th>vendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>...</th>
      <th>paymentType</th>
      <th>fareAmount</th>
      <th>extra</th>
      <th>mtaTax</th>
      <th>improvementSurcharge</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>ehailFee</th>
      <th>totalAmount</th>
      <th>tripType</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1</td>
      <td>0,98</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,921715</td>
      <td>40,766682</td>
      <td>–73,916908</td>
      <td>...</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>8,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1</td>
      <td>3,08</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,979973</td>
      <td>40,677071</td>
      <td>–73,934349</td>
      <td>...</td>
      <td>2.0</td>
      <td>11,5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1</td>
      <td>2.44</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,863045</td>
      <td>40,882923</td>
      <td>–73,839836</td>
      <td>...</td>
      <td>2.0</td>
      <td>12,5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1</td>
      <td>2,87</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,977730</td>
      <td>40,684647</td>
      <td>–73,931259</td>
      <td>...</td>
      <td>1.0</td>
      <td>12.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1</td>
      <td>0,50</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,942589</td>
      <td>40,841423</td>
      <td>–73,943672</td>
      <td>...</td>
      <td>2.0</td>
      <td>4,5</td>
      <td>0.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1</td>
      <td>2,25</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,830894</td>
      <td>40,759434</td>
      <td>–73,842422</td>
      <td>...</td>
      <td>2.0</td>
      <td>10,5</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1</td>
      <td>1,93</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,927109</td>
      <td>40,762848</td>
      <td>–73,909302</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.5</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>9,8</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0.80</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,881195</td>
      <td>40,741779</td>
      <td>–73,872086</td>
      <td>...</td>
      <td>2.0</td>
      <td>6,5</td>
      <td>0.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1</td>
      <td>1,04</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,954376</td>
      <td>40,805729</td>
      <td>–73,939117</td>
      <td>...</td>
      <td>1.0</td>
      <td>8.0</td>
      <td>1.0</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>11,3</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2.82</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,845200</td>
      <td>40,722134</td>
      <td>–73,810638</td>
      <td>...</td>
      <td>1.0</td>
      <td>13,0</td>
      <td>0,5</td>
      <td>0,5</td>
      <td>0,3</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>16,3</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
<p>10 Zeilen × 23 Spalten</p>
</div>



Nachdem die ersten Daten geladen wurden, definieren Sie eine Funktion zum Erstellen verschiedener zeitbasierter Features aus dem Feld für Startdatum und -uhrzeit. Dadurch werden neue Felder für die Monatsnummer, den Tag des Monats, den Wochentag und die Stunde des Tages erstellt. So kann das Modell zeitbasiert saisonale Besonderheiten berücksichtigen. Die Funktion fügt auch ein statisches Feature für den Ländercode hinzu, um Feiertagsdaten zusammenzuführen. Verwenden Sie die `apply()`-Funktion im Datenrahmen, um die `build_time_features()`-Funktion iterativ auf jede Zeile in den Taxidaten anzuwenden.


```python
def build_time_features(vector):
    pickup_datetime = vector[0]
    month_num = pickup_datetime.month
    day_of_month = pickup_datetime.day
    day_of_week = pickup_datetime.weekday()
    hour_of_day = pickup_datetime.hour
    country_code = "US"

    return pd.Series((month_num, day_of_month, day_of_week, hour_of_day, country_code))


green_taxi_df[["month_num", "day_of_month", "day_of_week", "hour_of_day", "country_code"]
              ] = green_taxi_df[["lpepPickupDatetime"]].apply(build_time_features, axis=1)
green_taxi_df.head(10)
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
      <th>vendorID</th>
      <th>lpepPickupDatetime</th>
      <th>lpepDropoffDatetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>puLocationId</th>
      <th>doLocationId</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>...</th>
      <th>tipAmount</th>
      <th>tollsAmount</th>
      <th>ehailFee</th>
      <th>totalAmount</th>
      <th>tripType</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20 17:38:28</td>
      <td>2016-01-20 17:46:33</td>
      <td>1</td>
      <td>0,98</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,921715</td>
      <td>40,766682</td>
      <td>–73,916908</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>8,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01 21:53:28</td>
      <td>2016-01-02 00:00:00</td>
      <td>1</td>
      <td>3,08</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,979973</td>
      <td>40,677071</td>
      <td>–73,934349</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01 00:50:23</td>
      <td>2016-01-01 01:05:37</td>
      <td>1</td>
      <td>2.44</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,863045</td>
      <td>40,882923</td>
      <td>–73,839836</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04 17:50:03</td>
      <td>2016-01-04 18:03:43</td>
      <td>1</td>
      <td>2,87</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,977730</td>
      <td>40,684647</td>
      <td>–73,931259</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>13,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>US</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13 08:48:20</td>
      <td>2016-01-13 08:52:16</td>
      <td>1</td>
      <td>0,50</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,942589</td>
      <td>40,841423</td>
      <td>–73,943672</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>5.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1049431</th>
      <td>2</td>
      <td>2016-01-29 17:16:18</td>
      <td>2016-01-29 17:27:52</td>
      <td>1</td>
      <td>2,25</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,830894</td>
      <td>40,759434</td>
      <td>–73,842422</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>12,3</td>
      <td>1.0</td>
      <td>1</td>
      <td>29</td>
      <td>4</td>
      <td>17</td>
      <td>US</td>
    </tr>
    <tr>
      <th>646563</th>
      <td>2</td>
      <td>2016-01-14 00:45:30</td>
      <td>2016-01-14 00:54:16</td>
      <td>1</td>
      <td>1,93</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,927109</td>
      <td>40,762848</td>
      <td>–73,909302</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>9,8</td>
      <td>1.0</td>
      <td>1</td>
      <td>14</td>
      <td>3</td>
      <td>0</td>
      <td>US</td>
    </tr>
    <tr>
      <th>438204</th>
      <td>1</td>
      <td>2016-01-09 14:25:02</td>
      <td>2016-01-09 14:32:48</td>
      <td>2</td>
      <td>0.80</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,881195</td>
      <td>40,741779</td>
      <td>–73,872086</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>7.3</td>
      <td>1.0</td>
      <td>1</td>
      <td>9</td>
      <td>5</td>
      <td>14</td>
      <td>US</td>
    </tr>
    <tr>
      <th>303784</th>
      <td>2</td>
      <td>2016-01-25 18:13:47</td>
      <td>2016-01-25 18:23:50</td>
      <td>1</td>
      <td>1,04</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,954376</td>
      <td>40,805729</td>
      <td>–73,939117</td>
      <td>...</td>
      <td>1.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>11,3</td>
      <td>1.0</td>
      <td>1</td>
      <td>25</td>
      <td>0</td>
      <td>18</td>
      <td>US</td>
    </tr>
    <tr>
      <th>269105</th>
      <td>2</td>
      <td>2016-01-24 20:46:50</td>
      <td>2016-01-24 21:04:03</td>
      <td>6</td>
      <td>2.82</td>
      <td>Keine</td>
      <td>Keine</td>
      <td>–73,845200</td>
      <td>40,722134</td>
      <td>–73,810638</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>16,3</td>
      <td>1.0</td>
      <td>1</td>
      <td>24</td>
      <td>6</td>
      <td>20</td>
      <td>US</td>
    </tr>
  </tbody>
</table>
<p>10 Zeilen × 28 Spalten</p>
</div>

Entfernen Sie einige der Spalten, die Sie für die Modellierung und das Erstellen zusätzlicher Feature nicht benötigen. Benennen Sie das Feld für die Startzeit um, und konvertieren Sie die Zeit außerdem mit `pandas.Series.dt.normalize` in Mitternacht. Dies führen Sie für alle Zeitfeatures aus, damit die Uhrzeitkomponente später beim Zusammenfügen von Datasets auf täglicher Granularitätsebene als Schlüssel verwendet werden kann.

```python
columns_to_remove = ["lpepDropoffDatetime", "puLocationId", "doLocationId", "extra", "mtaTax",
                     "improvementSurcharge", "tollsAmount", "ehailFee", "tripType", "rateCodeID",
                     "storeAndFwdFlag", "paymentType", "fareAmount", "tipAmount"
                     ]
for col in columns_to_remove:
    green_taxi_df.pop(col)

green_taxi_df = green_taxi_df.rename(
    columns={"lpepPickupDatetime": "datetime"})
green_taxi_df["datetime"] = green_taxi_df["datetime"].dt.normalize()
green_taxi_df.head(5)
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
      <th>vendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>117695</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1</td>
      <td>0,98</td>
      <td>–73,921715</td>
      <td>40,766682</td>
      <td>–73,916908</td>
      <td>40,761257</td>
      <td>8,8</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1221794</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>3,08</td>
      <td>–73,979973</td>
      <td>40,677071</td>
      <td>–73,934349</td>
      <td>40,671654</td>
      <td>12,8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1165078</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>2.44</td>
      <td>–73,863045</td>
      <td>40,882923</td>
      <td>–73,839836</td>
      <td>40,868336</td>
      <td>13,8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1345223</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1</td>
      <td>2,87</td>
      <td>–73,977730</td>
      <td>40,684647</td>
      <td>–73,931259</td>
      <td>40,694248</td>
      <td>13,8</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>US</td>
    </tr>
    <tr>
      <th>608125</th>
      <td>1</td>
      <td>2016-01-13</td>
      <td>1</td>
      <td>0,50</td>
      <td>–73,942589</td>
      <td>40,841423</td>
      <td>–73,943672</td>
      <td>40,834396</td>
      <td>5.3</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>US</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-holiday-data"></a>Anreichern mit Feiertagsdaten

Nachdem Sie nun die Taxidaten heruntergeladen und grob vorbereitet haben, fügen Sie Feiertagsdaten als zusätzliches Feature hinzu. Feiertagsspezifische Features tragen zur Genauigkeit des Modells bei, da große Feiertage Zeiten sind, in denen die Nachfrage nach Taxis stark steigt und die Versorgung eingeschränkt wird. Das Feiertagsdataset ist relativ klein, rufen Sie daher den vollständigen Satz mit dem Klassenkonstruktor `PublicHolidays` ohne Filterparameter ab. Zeigen Sie eine Vorschau der Daten an, um das Format zu überprüfen.

```python
from azureml.opendatasets import PublicHolidays
# call default constructor to download full dataset
holidays_df = PublicHolidays().to_pandas_dataframe()
holidays_df.head(5)
```

    ActivityStarted, to_pandas_dataframe
    Looking for parquet files...
    Reading them into Pandas dataframe...
    Reading Processed/part-00000-tid-1353805596865908763-9ee4e95b-0d55-4292-addd-a0e19d7c32cb-3559-c000.snappy.parquet under container holidaydatacontainer
    Done.
    ActivityCompleted: Activity=to_pandas_dataframe, HowEnded=Success, Duration=1799.89 [ms]

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
      <th>countryOrRegion</th>
      <th>holidayName</th>
      <th>isPaidTimeOff</th>
      <th>countryRegionCode</th>
      <th>normalizeHolidayName</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40688</th>
      <td>Albanien</td>
      <td>Neujahr</td>
      <td>Keine</td>
      <td>AL</td>
      <td>Neujahr</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40689</th>
      <td>Algerien</td>
      <td>Neujahr</td>
      <td>Keine</td>
      <td>DZ</td>
      <td>Neujahr</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40690</th>
      <td>Andorra</td>
      <td>Neujahr</td>
      <td>Keine</td>
      <td>AD</td>
      <td>Neujahr</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40691</th>
      <td>Angola</td>
      <td>Neujahr</td>
      <td>Keine</td>
      <td>AO</td>
      <td>Neujahr</td>
      <td>2008-01-01</td>
    </tr>
    <tr>
      <th>40692</th>
      <td>Argentinien</td>
      <td>Neujahr</td>
      <td>Keine</td>
      <td>AR</td>
      <td>Neujahr</td>
      <td>2008-01-01</td>
    </tr>
  </tbody>
</table>
</div>



Benennen Sie die Spalten `countryRegionCode` und `date` entsprechend den jeweiligen Feldnamen aus den Taxidaten um, und normalisieren Sie außerdem die Uhrzeit, damit sie als Schlüssel verwendet werden kann. Fügen Sie als Nächstes die Feiertagsdaten an die Taxidaten an, indem Sie mit der Pandas-Funktion `merge()` eine linksseitige Verknüpfung durchführen. Dabei werden alle Datensätze aus `green_taxi_df` beibehalten. Sofern aber für die entsprechenden Werte `datetime` und `country_code` (in diesem Fall immer `"US"`) Feiertagsdaten vorhanden sind, werden diese eingefügt. Zeigen Sie eine Vorschau der Daten an, um sicherzustellen, dass sie fehlerfrei zusammengeführt wurden.

```python
holidays_df = holidays_df.rename(
    columns={"countryRegionCode": "country_code", "date": "datetime"})
holidays_df["datetime"] = holidays_df["datetime"].dt.normalize()
holidays_df.pop("countryOrRegion")
holidays_df.pop("holidayName")

taxi_holidays_df = pd.merge(green_taxi_df, holidays_df, how="left", on=[
                            "datetime", "country_code"])
taxi_holidays_df.head(5)
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
      <th>vendorID</th>
      <th>datetime</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>country_code</th>
      <th>isPaidTimeOff</th>
      <th>normalizeHolidayName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>2016-01-20</td>
      <td>1</td>
      <td>0,98</td>
      <td>–73,921715</td>
      <td>40,766682</td>
      <td>–73,916908</td>
      <td>40,761257</td>
      <td>8,8</td>
      <td>1</td>
      <td>20</td>
      <td>2</td>
      <td>17</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>3,08</td>
      <td>–73,979973</td>
      <td>40,677071</td>
      <td>–73,934349</td>
      <td>40,671654</td>
      <td>12,8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21</td>
      <td>US</td>
      <td>True</td>
      <td>Neujahr</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>2016-01-01</td>
      <td>1</td>
      <td>2.44</td>
      <td>–73,863045</td>
      <td>40,882923</td>
      <td>–73,839836</td>
      <td>40,868336</td>
      <td>13,8</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>US</td>
      <td>True</td>
      <td>Neujahr</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>2016-01-04</td>
      <td>1</td>
      <td>2,87</td>
      <td>–73,977730</td>
      <td>40,684647</td>
      <td>–73,931259</td>
      <td>40,694248</td>
      <td>13,8</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>17</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>2016-01-13</td>
      <td>1</td>
      <td>0,50</td>
      <td>–73,942589</td>
      <td>40,841423</td>
      <td>–73,943672</td>
      <td>40,834396</td>
      <td>5.3</td>
      <td>1</td>
      <td>13</td>
      <td>2</td>
      <td>8</td>
      <td>US</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

### <a name="enrich-with-weather-data"></a>Anreichern mit Wetterdaten

Nun fügen Sie den Taxi- und Feiertagsdaten NOAA-Oberflächenwetterdaten hinzu. Gehen Sie ähnlich vor, um die Wetterdaten abzurufen, indem Sie iterativ jeweils einen Monat herunterladen. Geben Sie darüber hinaus den Parameter `cols` mit einem Array von Zeichenfolgen an, um die herunterzuladenden Spalten zu filtern. Dies ist ein sehr großes Dataset mit Oberflächenwetterdaten aus der ganzen Welt, daher sollten Sie vor dem Anfügen jedes Monats die Felder für Breiten- und Längengrad auf die Nähe von New York filtern, indem Sie die `query()`-Funktion auf den Dataframe anwenden. Dadurch wird sichergestellt, dass `weather_df` nicht zu groß wird.

```python
from azureml.opendatasets import NoaaIsdWeather

weather_df = pd.DataFrame([])
start = datetime.strptime("1/1/2016", "%m/%d/%Y")
end = datetime.strptime("1/31/2016", "%m/%d/%Y")

for sample_month in range(12):
    tmp_df = NoaaIsdWeather(cols=["temperature", "precipTime", "precipDepth", "snowDepth"], start_date=start + relativedelta(months=sample_month), end_date=end + relativedelta(months=sample_month))\
        .to_pandas_dataframe()
    print("--weather downloaded--")

    # filter out coordinates not in NYC to conserve memory
    tmp_df = tmp_df.query("latitude>=40.53 and latitude<=40.88")
    tmp_df = tmp_df.query("longitude>=-74.09 and longitude<=-73.72")
    print("--filtered coordinates--")
    weather_df = weather_df.append(tmp_df)

weather_df.head(10)
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
      <th>wban</th>
      <th>precipTime</th>
      <th>snowDepth</th>
      <th>Temperatur</th>
      <th>latitude</th>
      <th>precipDepth</th>
      <th>longitude</th>
      <th>datetime</th>
      <th>usaf</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1754979</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>7.2</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 00:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754980</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 01:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754981</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.7</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 02:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754982</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>6.1</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 03:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754983</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 04:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754984</th>
      <td>94741</td>
      <td>24,0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40,85</td>
      <td>5.0</td>
      <td>–74,061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754985</th>
      <td>94741</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>40,85</td>
      <td>NaN</td>
      <td>–74,061</td>
      <td>2016-01-01 04:59:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754986</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.6</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 05:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754987</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 06:51:00</td>
      <td>725025</td>
    </tr>
    <tr>
      <th>1754988</th>
      <td>94741</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>40,85</td>
      <td>0.0</td>
      <td>–74,061</td>
      <td>2016-01-01 07:51:00</td>
      <td>725025</td>
    </tr>
  </tbody>
</table>
</div>

Rufen Sie erneut `pandas.Series.dt.normalize` für das Feld `datetime` der Wetterdaten auf, damit der Zeitschlüssel dem in `taxi_holidays_df` entspricht. Löschen Sie die nicht benötigten Spalten, und filtern Sie Datensätze mit der Temperatur `NaN` heraus.

Gruppieren Sie als Nächstes die Wetterdaten, um tagesweise aggregierte Wetterwerte zu erhalten. Definieren Sie ein Wörterbuch `aggregations`, um zu definieren, wie jedes Feld auf Tagesebene aggregiert wird. Verwenden Sie für `snowDepth` und `temperature` den Mittelwert und für `precipTime` und `precipDepth` den täglichen Maximalwert. Verwenden Sie die `groupby()`-Funktion zusammen mit den Aggregationen, um die Daten zu gruppieren. Zeigen Sie eine Vorschau der Daten an, um sicherzustellen, dass ein Datensatz pro Tag vorhanden ist.

```python
weather_df["datetime"] = weather_df["datetime"].dt.normalize()
weather_df.pop("usaf")
weather_df.pop("wban")
weather_df.pop("longitude")
weather_df.pop("latitude")

# filter out NaN
weather_df = weather_df.query("temperature==temperature")

# group by datetime
aggregations = {"snowDepth": "mean", "precipTime": "max",
                "temperature": "mean", "precipDepth": "max"}
weather_df_grouped = weather_df.groupby("datetime").agg(aggregations)
weather_df_grouped.head(10)
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
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>Temperatur</th>
      <th>precipDepth</th>
    </tr>
    <tr>
      <th>datetime</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016-01-01</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>5,197345</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-02</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>2,567857</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-03</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>3,846429</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-04</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>0,123894</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-05</th>
      <td>NaN</td>
      <td>6,0</td>
      <td>–7,206250</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-06</th>
      <td>NaN</td>
      <td>6,0</td>
      <td>–0,896396</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-07</th>
      <td>NaN</td>
      <td>6,0</td>
      <td>3,180645</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-08</th>
      <td>NaN</td>
      <td>1.0</td>
      <td>4,384091</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2016-01-09</th>
      <td>NaN</td>
      <td>6,0</td>
      <td>6,710274</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2016-01-10</th>
      <td>NaN</td>
      <td>24,0</td>
      <td>10,943655</td>
      <td>254,0</td>
    </tr>
  </tbody>
</table>
</div>

> [!NOTE]
> Die Beispiele in diesem Tutorial führen Daten mithilfe von Pandas-Funktionen und benutzerdefinierten Aggregationen zusammen, das SDK für öffentliche Datasets enthält jedoch Klassen zum einfachen Zusammenführen und Anreichern von Datasets. Codebeispiele für diese Entwurfsmuster finden Sie im [Notebook](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb).

### <a name="cleanse-data"></a>Bereinigen der Daten

Führen Sie die Taxi- und Feiertagsdaten, die Sie vorbereitet haben, mit den neuen Wetterdaten zusammen. Dieses Mal benötigen Sie nur den Schlüssel `datetime`, und Sie führen erneut ein linksseitiges Verknüpfen der Daten durch. Führen Sie die `describe()`-Funktion für das neue Dataframe aus, um zusammenfassende Statistiken für jedes Feld anzuzeigen.

```python
taxi_holidays_weather_df = pd.merge(
    taxi_holidays_df, weather_df_grouped, how="left", on=["datetime"])
taxi_holidays_weather_df.describe()
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
      <th>vendorID</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>pickupLongitude</th>
      <th>pickupLatitude</th>
      <th>dropoffLongitude</th>
      <th>dropoffLatitude</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>Temperatur</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>1671.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
      <td>24000.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1,786583</td>
      <td>6,576208</td>
      <td>1,582588</td>
      <td>20,505491</td>
      <td>84,936413</td>
      <td>–36,232825</td>
      <td>21,723144</td>
      <td>7,863018</td>
      <td>6,500000</td>
      <td>15.113708</td>
      <td>3.240250</td>
      <td>13,664125</td>
      <td>11,764141</td>
      <td>13,258875</td>
      <td>13,903524</td>
      <td>1056.644458</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0,409728</td>
      <td>9,086857</td>
      <td>2,418177</td>
      <td>108,847821</td>
      <td>70,678506</td>
      <td>37.650276</td>
      <td>19,104384</td>
      <td>10,648766</td>
      <td>3,452124</td>
      <td>8,485155</td>
      <td>1,956895</td>
      <td>6.650676</td>
      <td>15,651884</td>
      <td>10,339720</td>
      <td>9,474396</td>
      <td>2\.815,592754</td>
    </tr>
    <tr>
      <th>Min</th>
      <td>1.000000</td>
      <td>–60,000000</td>
      <td>–1,000000</td>
      <td>–74,179482</td>
      <td>0.000000</td>
      <td>–74,190704</td>
      <td>0.000000</td>
      <td>–52,800000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3,000000</td>
      <td>1.000000</td>
      <td>–13,379464</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2,000000</td>
      <td>1.000000</td>
      <td>0,330000</td>
      <td>–73,946680</td>
      <td>40,717712</td>
      <td>–73,945429</td>
      <td>1,770000</td>
      <td>1.000000</td>
      <td>3,750000</td>
      <td>8,000000</td>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>3,000000</td>
      <td>1.000000</td>
      <td>6,620773</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2,000000</td>
      <td>4,000000</td>
      <td>0,830000</td>
      <td>1,500000</td>
      <td>40,814129</td>
      <td>0,500000</td>
      <td>21,495000</td>
      <td>2,000000</td>
      <td>6,500000</td>
      <td>15,000000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>4,428571</td>
      <td>6,000000</td>
      <td>13.090753</td>
      <td>10,000000</td>
    </tr>
    <tr>
      <th>75 %</th>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>1,870000</td>
      <td>89,000000</td>
      <td>129,000000</td>
      <td>1.000000</td>
      <td>40,746146</td>
      <td>11,300000</td>
      <td>9,250000</td>
      <td>22,000000</td>
      <td>5,000000</td>
      <td>19,000000</td>
      <td>12,722222</td>
      <td>24,000000</td>
      <td>22,944737</td>
      <td>132.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2,000000</td>
      <td>460.000000</td>
      <td>51,950000</td>
      <td>265,000000</td>
      <td>265,000000</td>
      <td>6,000000</td>
      <td>58,600000</td>
      <td>498.000000</td>
      <td>12,000000</td>
      <td>30,000000</td>
      <td>6,000000</td>
      <td>23,000000</td>
      <td>67,090909</td>
      <td>24,000000</td>
      <td>31,303665</td>
      <td>9\.999,000000</td>
    </tr>
  </tbody>
</table>
</div>

In den zusammenfassenden Statistiken sehen Sie, dass mehrere Felder Ausreißer oder Werte aufweisen, die die Modellgenauigkeit verringern. Filtern Sie zuerst die Felder für Breiten- und Längengrad, sodass sie innerhalb der gleichen Grenzen liegen, die Sie zum Filtern der Wetterdaten verwendet haben. Das Feld `tripDistance` weist einige ungültige Daten auf, da der Minimalwert negativ ist. Das Feld `passengerCount` weist ebenfalls ungültige Daten auf, da der Maximalwert 210 Passagiere beträgt. Das Feld `totalAmount` weist schließlich negative Werte auf, die im Kontext unseres Modells nicht sinnvoll sind.

Filtern Sie diese Anomalien mithilfe von Abfragefunktionen heraus, und entfernen Sie dann die letzten Spalten, die für das Training nicht erforderlich sind.

```python
final_df = taxi_holidays_weather_df.query(
    "pickupLatitude>=40.53 and pickupLatitude<=40.88")
final_df = final_df.query(
    "pickupLongitude>=-74.09 and pickupLongitude<=-73.72")
final_df = final_df.query("tripDistance>0 and tripDistance<75")
final_df = final_df.query("passengerCount>0 and passengerCount<100")
final_df = final_df.query("totalAmount>0")

columns_to_remove_for_training = ["datetime", "pickupLongitude",
                                  "pickupLatitude", "dropoffLongitude", "dropoffLatitude", "country_code"]
for col in columns_to_remove_for_training:
    final_df.pop(col)
```

Rufen Sie `describe()` erneut für die Daten auf, um sicherzustellen, dass die Bereinigung erwartungsgemäß verlaufen ist. Sie haben jetzt einen vorbereiteten und bereinigten Satz von Taxi-, Feiertags- und Wetterdaten, mit dem Sie das Machine Learning-Modell trainieren können.

```python
final_df.describe()
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
      <th>vendorID</th>
      <th>passengerCount</th>
      <th>tripDistance</th>
      <th>totalAmount</th>
      <th>month_num</th>
      <th>day_of_month</th>
      <th>day_of_week</th>
      <th>hour_of_day</th>
      <th>snowDepth</th>
      <th>precipTime</th>
      <th>Temperatur</th>
      <th>precipDepth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>1\.490,000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
      <td>11765.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1,786910</td>
      <td>1,343476</td>
      <td>2,848488</td>
      <td>14,689039</td>
      <td>3,499788</td>
      <td>14,948916</td>
      <td>3,234254</td>
      <td>13,647344</td>
      <td>12,508581</td>
      <td>11,855929</td>
      <td>10,301433</td>
      <td>208,432384</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0,409508</td>
      <td>1,001232</td>
      <td>2,895960</td>
      <td>10,289832</td>
      <td>1,707865</td>
      <td>8,442438</td>
      <td>1,958477</td>
      <td>6,640280</td>
      <td>16,203195</td>
      <td>10.125701</td>
      <td>8,553512</td>
      <td>1\.284,892832</td>
    </tr>
    <tr>
      <th>Min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0,010000</td>
      <td>3,300000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>3,000000</td>
      <td>1.000000</td>
      <td>–13,379464</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2,000000</td>
      <td>1.000000</td>
      <td>1,070000</td>
      <td>8.160000</td>
      <td>2,000000</td>
      <td>8,000000</td>
      <td>2,000000</td>
      <td>9,000000</td>
      <td>3,000000</td>
      <td>1.000000</td>
      <td>3,504580</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2,000000</td>
      <td>1.000000</td>
      <td>1,900000</td>
      <td>11,300000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>3,000000</td>
      <td>15,000000</td>
      <td>4,250000</td>
      <td>6,000000</td>
      <td>10,168182</td>
      <td>3,000000</td>
    </tr>
    <tr>
      <th>75 %</th>
      <td>2,000000</td>
      <td>1.000000</td>
      <td>3,550000</td>
      <td>17,800000</td>
      <td>5,000000</td>
      <td>22,000000</td>
      <td>5,000000</td>
      <td>19,000000</td>
      <td>15.647059</td>
      <td>24,000000</td>
      <td>16,966923</td>
      <td>41,000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2,000000</td>
      <td>6,000000</td>
      <td>51,950000</td>
      <td>150.300000</td>
      <td>6,000000</td>
      <td>30,000000</td>
      <td>6,000000</td>
      <td>23,000000</td>
      <td>67,090909</td>
      <td>24,000000</td>
      <td>26,524107</td>
      <td>9\.999,000000</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="train-a-model"></a>Trainieren eines Modells

Nun trainieren Sie mit den vorbereiteten Daten ein automatisiertes Machine Learning-Modell. Beginnen Sie mit der Aufteilung von `final_df` in Features (X-Werte) und Bezeichnungen (Y-Werte) – bei diesem Modell sind dies die Taxifahrtkosten.

```python
y_df = final_df.pop("totalAmount")
x_df = final_df
```

Als Nächstes teilen Sie die Daten mithilfe der Funktion `train_test_split()` in der Bibliothek `scikit-learn` in Trainings- und Testsätze auf. Der Parameter `test_size` bestimmt den Prozentsatz der Daten, die zu Testzwecken verwendet werden sollen. Der Parameter `random_state` legt einen Seed für den Zufallszahlengenerator fest, damit die Aufteilung zwischen Trainings- und Testdaten deterministisch ist.


```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    x_df, y_df, test_size=0.2, random_state=222)
```

### <a name="load-workspace-and-configure-experiment"></a>Laden des Arbeitsbereichs und Konfigurieren des Experiments

Laden Sie Ihren Azure Machine Learning Service-Arbeitsbereich über die `get()`-Funktion mit Ihrem Abonnement und den Informationen zu Ihrem Arbeitsbereich. Erstellen Sie in Ihrem Arbeitsbereich ein Experiment zum Speichern und Überwachen Ihrer Modellausführungen.


```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment

workspace = Workspace.get(subscription_id="<your-subscription-id>",
                          name="<your-workspace-name>", resource_group="<your-resource-group>")
experiment = Experiment(workspace, "opendatasets-ml")
```

Erstellen Sie mithilfe der `AutoMLConfig`-Klasse ein Konfigurationsobjekt für das Experiment. Sie fügen Ihre Trainingsdaten an und geben darüber hinaus Einstellungen und Parameter an, die den Trainingsvorgang steuern. Die Parameter haben folgenden Zweck:

* `task`: Typ des auszuführenden Experiments
* `X`: Trainingsfeatures
* `y`: Trainingsbezeichnungen
* `iterations`: Anzahl von zu durchlaufenden Iterationen. Jede Iteration probiert Kombinationen unterschiedlicher Methoden zur Featurenormalisierung/-standardisierung sowie verschiedene Modelle mithilfe mehrerer Hyperparametereinstellungen durch.
* `primary_metric`: primäre während des Modelltrainings zu optimierende Metrik. Anhand dieser Metrik wird das am besten geeignete Modell ausgewählt.
* `preprocess`: steuert, ob das Experiment die Eingabedaten vorverarbeiten kann, um beispielsweise fehlende Daten zu behandeln, Text in numerische Daten umzuwandeln usw.
* `n_cross_validations`: Anzahl von Kreuzvalidierungsaufteilungen, die ausgeführt werden sollen, wenn keine Überprüfungsdaten angegeben sind.


```python
from azureml.train.automl import AutoMLConfig

automl_config = AutoMLConfig(task="regression",
                             X=X_train.values,
                             y=y_train.values.flatten(),
                             iterations=20,
                             primary_metric="spearman_correlation",
                             preprocess=True,
                             n_cross_validations=5
                             )
```

### <a name="submit-experiment"></a>Senden eines Experiments

Übermitteln Sie das Experiment zum Training. Nach Übermittlung des Experiments durchläuft der Prozess verschiedene Machine Learning-Algorithmen und Hyperparametereinstellungen unter Berücksichtigung der von Ihnen definierten Einschränkungen. Er optimiert die definierte Genauigkeitsmetrik, um das am besten geeignete Modell auszuwählen. Übergeben Sie das `automl_config`-Objekt an das Experiment. Legen Sie die Ausgabe auf `True` fest, um den Fortschritt während des Experiments anzuzeigen.

Nach dem Übermitteln des Experiments wird eine Liveausgabe für das Training angezeigt. Für die einzelnen Iterationen werden jeweils der Typ und die Methode für die Featurenormalisierung/-standardisierung für das Modell, die Ausführungsdauer und die Trainingsgenauigkeit angezeigt. Im Feld `BEST` wird die beste Trainingsbewertung auf der Grundlage Ihres Metriktyps nachverfolgt.

```python
training_run = experiment.submit(automl_config, show_output=True)
```

    Running on local machine
    Parent Run ID: AutoML_5c35f2a7-e479-4e7f-a131-ed4cb51e29d1
    Current status: DatasetEvaluation. Gathering dataset statistics.
    Current status: FeaturesGeneration. Generating features for the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetCrossValidationSplit. Generating individually featurized CV splits.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: DatasetFeaturization. Featurizing the dataset.
    Current status: ModelSelection. Beginning model selection.

    ****************************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ****************************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler RandomForest                      0:00:07       0.9081    0.9081
             1   StandardScalerWrapper DecisionTree             0:00:05       0.9121    0.9121
             2   StandardScalerWrapper LightGBM                 0:00:04       0.9318    0.9318
             3   StandardScalerWrapper LightGBM                 0:00:07       0.9286    0.9318
             4   MaxAbsScaler LightGBM                          0:00:05       0.9246    0.9318
             5   MaxAbsScaler LightGBM                          0:00:05       0.9199    0.9318
             6   MaxAbsScaler RandomForest                      0:00:07       0.9327    0.9327
             7   StandardScalerWrapper ElasticNet               0:00:04       0.9371    0.9371
             8   MaxAbsScaler LightGBM                          0:00:05       0.9327    0.9371
             9   MaxAbsScaler SGD                               0:00:04       0.9077    0.9371
            10   MaxAbsScaler LightGBM                          0:00:04       0.9340    0.9371
            11   StandardScalerWrapper LightGBM                 0:00:04       0.8301    0.9371
            12   MaxAbsScaler DecisionTree                      0:00:05       0.9214    0.9371
            13   StandardScalerWrapper DecisionTree             0:00:04       0.9201    0.9371
            14   MaxAbsScaler DecisionTree                      0:00:05       0.9179    0.9371
            15   MaxAbsScaler ExtremeRandomTrees                0:00:05       0.9052    0.9371
            16   StandardScalerWrapper DecisionTree             0:00:04       0.9282    0.9371
            17   StandardScalerWrapper ElasticNet               0:00:04       0.9319    0.9371
            18   VotingEnsemble                                 0:00:16       0.9380    0.9380
            19   StackEnsemble                                  0:00:17       0.9376    0.9380

### <a name="retrieve-the-fitted-model"></a>Abrufen des angepassten Modells

Am Ende aller Trainingsiterationen erstellt der Prozess zum automatisierten maschinellen Lernen einen Ensemblealgorithmus aus allen Einzelausführungen mit Bootstrapaggregierung oder Stapelung. Rufen Sie das angepasste Ensemble in die Variable `fitted_model` und die beste Einzelausführung in die Variable `best_run` ab.

```python
best_run, fitted_model = training_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-model-accuracy"></a>Testen der Modellgenauigkeit

Verwenden Sie das beste Ensemblemodell, um Vorhersagen für das Testdataset auszuführen und Taxipreise vorherzusagen. Die `predict()`-Funktion verwendet das angepasste Modell und sagt die Werte von y (Taxifahrtkosten) aus dem Dataset `X_test` voraus.


```python
y_predict = fitted_model.predict(X_test.values)
```

Berechnen Sie den mittleren quadratischen Fehler der Ergebnisse. Verwenden Sie den Datenrahmen `y_test`, und konvertieren Sie ihn in eine Liste `y_actual`, um diese mit den prognostizierten Werten zu vergleichen. Die Funktion `mean_squared_error` akzeptiert zwei Wertarrays und berechnet den durchschnittlichen quadratischen Fehler zwischen den Arrays. Die Quadratwurzel des Ergebnisses ergibt einen Fehler in der gleichen Maßeinheit wie die y-Variable (Kosten). Dieser Fehler gibt Aufschluss darüber, wie weit die Vorhersagen des Taxipreises ungefähr vom tatsächlichen Tarif entfernt sind, wobei große Fehler stark gewichtet werden.


```python
from sklearn.metrics import mean_squared_error
from math import sqrt

y_actual = y_test.values.flatten().tolist()
rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```




    4.178568987067901



Führen Sie den folgenden Code aus, um den mittleren absoluten Fehler in Prozent (Mean Absolute Percent Error, MAPE) anhand der vollständigen Datasets `y_actual` und `y_predict` zu berechnen. Diese Metrik berechnet eine absolute Differenz zwischen jedem vorhergesagten und tatsächlichen Wert und addiert alle Differenzen. Die Summe wird dann als Prozentsatz der gesamten tatsächlichen Werte ausgedrückt.


```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.14923619644924357

    Model Accuracy:
    0.8507638035507564

Da Sie eine eher kleine Stichprobe der Daten relativ zum vollständigen Dataset verwendet haben (n = 11748), ist die Modellgenauigkeit mit 85 % bei einem RMSE um +/- 4,00 $ bei der Vorhersage des Taxipreises relativ hoch. Wechseln Sie als möglichen nächsten Schritt zum Verbessern der Genauigkeit zur zweiten Zelle dieses Notizbuchs zurück, erhöhen Sie die Stichprobengröße über 2.000 Datensätze pro Monat, und führen Sie das gesamte Experiment noch einmal aus, um das Modell mit einem größeren Datenumfang erneut zu trainieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die erstellten Ressourcen nicht mehr benötigen, löschen Sie sie, damit Ihnen keine Kosten entstehen.

1. Wählen Sie ganz links im Azure-Portal **Ressourcengruppen** aus.
1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.
1. Wählen Sie die Option **Ressourcengruppe löschen**.
1. Geben Sie den Ressourcengruppennamen ein. Wählen Sie anschließend die Option **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Codebeispiele finden Sie in den [Notebooks](https://github.com/Azure/OpenDatasetsNotebooks) für öffentliche Azure-Datasets.
* Befolgen Sie die [Anleitung](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train), um weitere Informationen zum automatisierten maschinellen Lernen in Azure Machine Learning Service zu erhalten.
