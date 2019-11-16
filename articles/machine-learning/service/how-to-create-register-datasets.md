---
title: Erstellen von Azure Machine Learning-Datasets für den Datenzugriff
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Azure Machine Learning-Datasets erstellen, um auf Ihre Daten für Machine Learning-Experimentausführungen zuzugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 11/04/2019
ms.openlocfilehash: b4439df5ced1f76b4cb69feadf862638ab223d56
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74123320"
---
# <a name="create-azure-machine-learning-datasets"></a>Erstellen von Azure Machine Learning-Datasets

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning-Datasets erstellen, um auf Daten für Ihre lokalen oder remotebasierten Experimente zuzugreifen.

Azure Machine Learning-Datasets ermöglichen Folgendes:

* Aufbewahren einer einzelnen Datenkopie in Ihrem Speicher, auf die durch Datasets verwiesen wird

* Nahtloses Zugreifen auf Daten während des Modelltrainings, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssen

* Freigeben von Daten und Zusammenarbeiten mit anderen Benutzern

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!NOTE]
> Einige Datasetklassen sind vom Paket [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) abhängig. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="dataset-types"></a>Datasettypen

Es gibt zwei Arten von Datasettypen – abhängig davon, wie Benutzer sie beim Trainieren verwenden:

* [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) stellt Daten in einem tabellarischen Format dar, indem die bereitgestellte Datei oder Liste von Dateien analysiert wird. Dadurch erhalten Sie die Möglichkeit, die Daten in einem Pandas- oder Spark-Datenrahmen (DataFrame) zu materialisieren. Ein Objekt vom Typ `TabularDataset` kann auf der Grundlage von CSV-, TSV-und Parquet-Dateien sowie auf der Grundlage von SQL-Abfrageergebnissen erstellt werden. Eine umfassende Liste finden Sie unter [TabularDatasetFactory-Klasse](https://aka.ms/tabulardataset-api-reference).

* Die Klasse [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) verweist auf eine einzelne Datei oder auf mehrere Dateien in Ihren Datenspeichern oder unter öffentlichen URLs. Mithilfe dieser Methode können Sie die Dateien als FileDataset-Objekt in Ihre Compute-Instanz herunterladen oder einbinden. Die Dateien können in einem beliebigen Format vorliegen. Dies ermöglicht eine größere Bandbreite an Machine Learning-Szenarien (einschließlich Deep Learning).

Weitere Informationen zu anstehenden API-Änderungen finden Sie im [Änderungshinweis für die Dataset-API](https://aka.ms/tabular-dataset).

## <a name="create-datasets"></a>Erstellen von Datasets

Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Da die Daten an ihrem Speicherort verbleiben, fallen keine zusätzlichen Speicherkosten an. Sowohl Dataset vom Typ `TabularDataset` als auch Datasets vom Typ `FileDataset` können mithilfe des Python SDK oder über die Landing Page des Arbeitsbereichs (Vorschauversion) erstellt werden.

Damit Azure Machine Learning auf die Daten zugreifen kann, müssen Datasets auf der Grundlage von Pfaden in [Azure-Datenspeichern](how-to-access-data.md) oder auf der Grundlage öffentlicher Web-URLs erstellt werden.

### <a name="use-the-sdk"></a>Verwenden des SDK

So erstellen Sie Datasets auf der Grundlage eines [Azure-Datenspeichers](how-to-access-data.md) unter Verwendung des Python SDK:

1. Vergewissern Sie sich, dass Sie für den registrierten Azure-Datenspeicher über Zugriff vom Typ `contributor` oder `owner` verfügen.

1. Erstellen Sie das Dataset, indem Sie auf einen Pfad im Datenspeicher verweisen:

    ```Python
    from azureml.core.workspace import Workspace
    from azureml.core.datastore import Datastore
    from azureml.core.dataset import Dataset
    
    datastore_name = 'your datastore name'
    
    # get existing workspace
    workspace = Workspace.from_config()
    
    # retrieve an existing datastore in the workspace by name
    datastore = Datastore.get(workspace, datastore_name)
    ```

#### <a name="create-a-tabulardataset"></a>Erstellen eines TabularDataset-Elements

TabularDataset-Objekte können über das SDK oder über Azure Machine Learning Studio erstellt werden. Sie können einen Zeitstempel auf der Grundlage einer Spalte in den Daten oder auf der Grundlage des Pfadmusters angeben, unter dem die Daten gespeichert sind, um ein Zeitreihenmerkmal zu aktivieren. Diese Angabe ermöglicht einfaches und effizientes Filtern nach Zeit.

Verwenden Sie die Methode [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header-true--partition-format-none-) für die Klasse `TabularDatasetFactory`, um Dateien im CSV- oder TSV-Format zu lesen und ein nicht registriertes TabularDataset-Objekt zu erstellen. Wenn Sie Daten aus mehreren Dateien lesen, werden die Ergebnisse in einer Tabellendarstellung aggregiert.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

Beim Erstellen eines TabularDataset-Objekts werden Spaltendatentypen standardmäßig automatisch abgeleitet. Sollten die abgeleiteten Typen nicht Ihren Erwartungen entsprechen, können Sie den folgenden Code verwenden, um Spaltentypen anzugeben. Weitere Informationen zu unterstützten Datentypen finden Sie [hier](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.datatype?view=azure-ml-py).

```Python
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|NAME|Geschlecht|Alter|SibSp|Parch|Ticket|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|male|22,0|1|0|A/5 21171|7.2500||S
1|2|True|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|female|38,0|1|0|PC 17599|71.2833|C85|C
2|3|True|3|Heikkinen, Miss. Laina|female|26,0|0|0|STON/O2. 3101282|7.9250||S

Verwenden Sie die Methode [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none-) für die Klasse `TabularDatasetFactory`, um Daten aus Azure SQL-Datenbank zu lesen:

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

In TabularDataset-Objekten können Sie einen Zeitstempel auf der Grundlage einer Spalte in den Daten oder auf der Grundlage des Orts angeben, an dem die Pfadmusterdaten gespeichert sind, um ein Zeitreihenmerkmal zu aktivieren. Diese Angabe ermöglicht einfaches und effizientes Filtern nach Zeit.

Verwenden Sie die Methode [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-) für die Klasse `TabularDataset`, um Ihre Zeitstempelspalte anzugeben und die Filterung nach Zeit zu ermöglichen. Weitere Informationen finden Sie unter [Demo einer tabellarischen, zeitreihenbezogenen API mit NOAA-Wetterdaten](https://aka.ms/azureml-tsd-notebook).

```Python
# create a TabularDataset with time series trait
datastore_paths = [(datastore, 'weather/*/*/*/data.parquet')]

# get a coarse timestamp column from the path pattern
dataset = Dataset.Tabular.from_parquet_files(path=datastore_path, partition_format='weather/{coarse_time:yyy/MM/dd}/data.parquet')

# set coarse timestamp to the virtual column created, and fine grain timestamp from a column in the data
dataset = dataset.with_timestamp_columns(fine_grain_timestamp='datetime', coarse_grain_timestamp='coarse_time')

# filter with time-series-trait-specific methods
data_slice = dataset.time_before(datetime(2019, 1, 1))
data_slice = dataset.time_after(datetime(2019, 1, 1))
data_slice = dataset.time_between(datetime(2019, 1, 1), datetime(2019, 2, 1))
data_slice = dataset.time_recent(timedelta(weeks=1, days=1))
```

#### <a name="create-a-filedataset"></a>Erstellen eines FileDataset-Elements

Verwenden Sie die Methode [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) für die Klasse `FileDatasetFactory`, um Dateien in einem beliebigen Format zu laden und ein nicht registriertes FileDataset-Objekt zu erstellen:

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [
                  (datastore, 'animals')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]
mnist_ds = Dataset.File.from_files(path=web_paths)
```

#### <a name="on-the-web"></a>Im Web 
In den folgenden Schritten und in der Animation wird gezeigt, wie ein Dataset in Azure Machine Learning Studio (https://ml.azure.com ) erstellt wird:

![Erstellen eines Datasets mithilfe der Benutzeroberfläche](media/how-to-create-register-datasets/create-dataset-ui.gif)

So erstellen Sie ein Dataset im Studio
1. Melden Sie sich bei https://ml.azure.com an.
1. Wählen Sie **Datasets** im Abschnitt **Assets** im linken Bereich aus. 
1. Wählen Sie **Dataset erstellen** aus, um die Quelle Ihres Datasets auszuwählen. Bei dieser Quelle kann es sich um lokale Dateien, um einen Datenspeicher oder um öffentliche URLs handeln.
1. Wählen Sie **Tabellarisch** oder **Datei** als Datasettyp aus.
1. Wählen Sie **Weiter** aus, um die Formulare **Einstellungen und Vorschau**, **Schema** und **Details bestätigen** zu überprüfen. Diese werden ausgehend vom Dateityp intelligent ausgefüllt. Verwenden Sie diese Formulare, um Ihre ausgewählten Optionen zu überprüfen und das Dataset vor der Erstellung weiter zu konfigurieren.  
1. Wählen Sie **Erstellen** aus, um die Erstellung des Datasets abzuschließen.

## <a name="register-datasets"></a>Registrieren von Datasets

Registrieren Sie Ihre Datasets bei einem Arbeitsbereich, um den Erstellungsprozess abzuschließen. Verwenden Sie die Methode [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-), um Datasets bei Ihrem Arbeitsbereich zu registrieren, damit sie für andere freigegeben und für unterschiedliche Experimente wiederverwendet werden können:

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

> [!Note]
> Datasets, die über Azure Machine Learning Studio erstellt werden, werden automatisch beim Arbeitsbereich registriert.

## <a name="create-datasets-with-azure-open-datasets"></a>Erstellen von Datasets mit Azure Open Datasets

[Öffentliche Azure-Datasets](https://azure.microsoft.com/services/open-datasets/) sind kuratierte öffentliche Datasets, mit denen Sie Lösungen mit maschinellem Lernen szenariospezifische Features hinzufügen können, um genauere Modelle zu erzielen. Die Datasets umfassen gemeinfreie Daten für Wetter, Volkszählungen, Feiertage, öffentliche Sicherheit und Orte, mit denen Sie Machine Learning-Modelle trainieren und Vorhersagelösungen anreichern können. Open Datasets befindet sich in der Cloud unter Microsoft Azure und ist sowohl im SDK als auch in der Benutzeroberfläche des Arbeitsbereichs enthalten.

### <a name="use-the-sdk"></a>Verwenden des SDK

Möchten Sie Datasets mit Azure Open Datasets aus dem SDK erstellen, muss das Paket mit `pip install azureml-opendatasets` installiert worden sein. Jedes einzelne Dataset wird durch seine eigene Klasse im SDK dargestellt, und bestimmte Klassen sind entweder als `TabularDataset`, als `FileDataset` oder als beides verfügbar. Eine vollständige Liste der Klassen finden Sie in der [Referenzdokumentation](https://docs.microsoft.com/python/api/azureml-opendatasets/azureml.opendatasets?view=azure-ml-py).

Die meisten Klassen erben von und geben eine Instanz von `TabularDataset` zurück. Beispiele für diese Klassen sind `PublicHolidays`, `BostonSafety` und `UsPopulationZip`. Um ein `TabularDataset`-Objekt aus diesen Klassen zu erstellen, verwenden Sie den Konstruktor ohne Argumente. Wenn Sie ein Dataset registrieren, das aus Open Datasets erstellt wurde, werden keine Daten sofort heruntergeladen, sondern die Daten werden später, wenn sie angefordert werden (z. B. während des Trainings), aus einem zentralen Speicherort abgerufen. 

```python
from azureml.opendatasets import UsPopulationZip

tabular_dataset = UsPopulationZip()
tabular_dataset = tabular_dataset.register(workspace=workspace, name="pop data", description="US population data by zip code")
```

Bestimmte Klassen können entweder als `TabularDataset` oder als `FileDataset` abgerufen werden, sodass Sie die Dateien direkt bearbeiten und/oder herunterladen können. Andere Klassen können ein Dataset nur über die Funktion `get_tabular_dataset()` oder `get_file_dataset()` abrufen. Das folgende Codebeispiel zeigt einige Beispiele für diese Art von Klassen:

```python
from azureml.opendatasets import MNIST

# MNIST class can return either TabularDataset or FileDataset
tabular_dataset = MNIST.get_tabular_dataset()
file_dataset = MNIST.get_file_dataset()

from azureml.opendatasets import Diabetes

# Diabetes class can return ONLY return TabularDataset and must be called from the static function
diabetes_tabular = Diabetes.get_tabular_dataset()
```

### <a name="use-the-ui"></a>Verwenden der Benutzeroberfläche

Sie können Datasets auch über die Benutzeroberfläche auf der Grundlage von Open Datasets-Klassen erstellen. Wählen Sie in Ihrem Arbeitsbereich unter **Assets** die Registerkarte **Datasets** aus. Wählen Sie im Dropdownmenü **Dataset erstellen** die Option **Aus Open Datasets** aus.

![Öffnen eines Datasets über die Benutzeroberfläche](media/how-to-create-register-datasets/open-datasets-1.png)

Wählen Sie ein Dataset aus, indem Sie die entsprechende Kachel auswählen. (Über die Suchleiste kann gefiltert werden.) Klicken Sie auf **Weiter**.

![Dataset auswählen](media/how-to-create-register-datasets/open-datasets-2.png)

Wählen Sie einen Namen, unter dem das Dataset registriert werden soll, und filtern Sie die Daten optional mithilfe der verfügbaren Filter. Filtern Sie in diesem Fall das Dataset der gesetzlichen Feiertage nach Zeitraum (ein Jahr) und Ländercode (nur USA). Klicken Sie auf **Erstellen**.

![Dataset-Parameter festlegen und Dataset erstellen](media/how-to-create-register-datasets/open-datasets-3.png)

Das Dataset ist nun in Ihrem Arbeitsbereich unter **Datasets** verfügbar. Sie können es auf die gleiche Weise verwenden wie andere Datasets, die Sie erstellt haben.

## <a name="version-datasets"></a>Versionsverwaltung von Datasets

Sie können ein neues Dataset unter demselben Namen registrieren, indem Sie eine neue Version erstellen. Eine Datasetversion ermöglicht es, den Zustand der Daten zu markieren, sodass Sie eine bestimmte Version des Datasets für Experimente oder zukünftige Reproduktion verwenden können. Weitere Informationen zu Datasetversionen finden Sie [hier](how-to-version-track-datasets.md).
```Python
# create a TabularDataset from Titanic training data
web_paths = [
            'https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
            'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv'
           ]
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```


## <a name="access-datasets-in-your-script"></a>Zugreifen auf Datasets in Ihrem Skript

Auf registrierte Datasets kann sowohl lokal als auch remote in Computeclustern wie Azure Machine Learning Compute zugegriffen werden. Verwenden Sie für den experimentübergreifenden Zugriff auf Ihr registriertes Dataset den folgenden Code, um anhand des Namens auf Ihren Arbeitsbereich und das registrierte Dataset zuzugreifen. Die Methode [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--) für die Klasse `Dataset` gibt standardmäßig die neueste Version des im Arbeitsbereich registrierten Datasets zurück.

```Python
%%writefile $script_folder/train.py

from azureml.core import Dataset, Run

run = Run.get_context()
workspace = run.experiment.workspace

dataset_name = 'titanic_ds'

# Get a dataset by name
titanic_ds = Dataset.get_by_name(workspace=workspace, name=dataset_name)

# Load a TabularDataset into pandas DataFrame
df = titanic_ds.to_pandas_dataframe()
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie mit Datasets trainiert wird](how-to-train-with-datasets.md).
* Verwenden Sie automatisiertes Machine Learning, um [mit TabularDatasets zu trainieren](https://aka.ms/automl-dataset).
* Weitere Beispiele zum Trainieren von Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
