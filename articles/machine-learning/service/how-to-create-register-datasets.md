---
title: Erstellen von Datasets zum Zugreifen auf Daten mit „azureml-datasets“
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Datasets aus verschiedenen Quellen erstellen und Datasets in Ihrem Arbeitsbereich registrieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 10/10/2019
ms.openlocfilehash: 54f8a1248688a6d62192e4f34cf6b98a94086da8
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72274761"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Erstellen von und Zugreifen auf Datasets (Vorschauversion) in Azure Machine Learning

In diesem Artikel wird beschrieben, wie Sie Azure Machine Learning-Datasets (Vorschauversion) erstellen und auf Daten in lokalen oder remotebasierten Experimenten zugreifen.

Azure Machine Learning-Datasets ermöglichen Folgendes:

* **Aufbewahren einer einzelnen Datenkopie in Ihrem Speicher**, auf die durch Datasets verwiesen wird.

* **Müheloses Zugreifen auf Daten während des Modelltrainings** ohne Berücksichtigung von Verbindungszeichenfolgen oder Datenpfaden

* **Freigeben von Daten und Zusammenarbeiten** mit anderen Benutzern.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!Note]
> Einige Dataset-Klassen (Vorschauversion) verfügen über Abhängigkeiten vom [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py)-Paket. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="dataset-types"></a>Datasettypen

Datasets werden in zwei Typen eingeteilt, je nachdem, wie Benutzer sie im Training nutzen.

* [TabularDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) stellt Daten in einem tabellarischen Format dar, indem die bereitgestellte Datei oder Liste von Dateien analysiert wird. Dadurch erhalten Sie die Möglichkeit, die Daten in einem Pandas- oder Spark-Datenrahmen zu materialisieren. Ein `TabularDataset`-Objekt kann aus CSV-, TSV- oder parquet-Dateien, SQL-Abfrageergebnissen usw. erstellt werden. Eine vollständige Liste finden Sie in unserer [Dokumentation](https://aka.ms/tabulardataset-api-reference).

* [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) verweist auf eine einzelne Datei oder auf mehrere Dateien in Ihren Datenspeichern oder öffentlichen URLs. Auf diese Weise besteht die Möglichkeit, die Dateien herunterzuladen oder in Ihrer Computeinstanz bereitzustellen. Die Dateien können ein beliebiges Format aufweisen. Dies ermöglicht eine größere Bandbreite an Machine Learning-Szenarien einschließlich Deep Learning.

Weitere Informationen zu bevorstehenden API-Änderungen finden Sie [hier](https://aka.ms/tabular-dataset).

## <a name="create-datasets"></a>Erstellen von Datasets

Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Die Daten verbleiben an ihrem Speicherort, sodass keine zusätzlichen Speicherkosten anfallen. Sowohl TabularDatasets als auch FileDatasets können mithilfe des Python SDK oder der Landing Page für den Arbeitsbereich (Vorschauversion) erstellt werden. 

Damit Azure Machine Learning auf die Daten zugreifen kann, müssen Datasets aus Pfaden in [Azure-Datenspeichern](how-to-access-data.md) oder öffentlichen Web-URLs erstellt werden.

### <a name="using-the-sdk"></a>Verwenden des SDK

So erstellen Sie Datasets aus einem [Azure-Datenspeicher](how-to-access-data.md) mithilfe des Python SDK:

* Vergewissern Sie sich, dass Sie für den registrierten Azure-Datenspeicher über Zugriff vom Typ `contributor` oder `owner` verfügen.

* Erstellen Sie das Dataset, indem Sie auf einen Pfad im Datenspeicher verweisen.

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
#### <a name="create-tabulardatasets"></a>Erstellen von TabularDatasets

Verwenden Sie die [`from_delimited_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-delimited-files-path--validate-true--include-path-false--infer-column-types-true--set-column-types-none--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---partition-format-none-)-Methode mit der `TabularDatasetFactory`-Klasse, um Dateien im CSV- oder TSV-Format zu lesen, und erstellen Sie ein nicht registriertes TabularDataset. Wenn Sie aus mehreren Dateien lesen, werden die Ergebnisse in einer Tabellendarstellung aggregiert.

```Python
# create a TabularDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'weather/2018/11.csv'),
                  (datastore, 'weather/2018/12.csv'),
                  (datastore, 'weather/2019/*.csv')
                 ]
weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)

# create a TabularDataset from a delimited file behind a public web url
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path)

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

| |PassengerId|Survived|Pclass|NAME|Geschlecht|Alter|SibSp|Parch|Ticket|Fare|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|0|3|Braund, Mr. Owen Harris|male|22,0|1|0|A/5 21171|7.2500||S
1|2|1|1|Cumings, Mrs. John Bradley (Florence Briggs Th...|female|38,0|1|0|PC 17599|71.2833|C85|C
2|3|1|3|Heikkinen, Miss. Laina|female|26,0|0|0|STON/O2. 3101282|7.9250||S

Verwenden Sie die [`from_sql_query()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?view=azure-ml-py#from-sql-query-query--validate-true--set-column-types-none-)-Methode der `TabularDatasetFactory`-Klasse, um Daten aus Azure SQL-Datenbank zu lesen.

```Python

from azureml.core import Dataset, Datastore

# create tabular dataset from a SQL database in datastore
sql_datastore = Datastore.get(workspace, 'mssql')
sql_ds = Dataset.Tabular.from_sql_query((sql_datastore, 'SELECT * FROM my_table'))
```

In TabularDatasets kann ein Zeitstempel in einer Datenspalte oder in dem Pfadmuster angegeben werden, in dem Daten gespeichert werden, um ein Zeitreihenmerkmal zu aktivieren, was eine einfache und effiziente Filterung nach Zeit ermöglicht.

Verwenden Sie die [`with_timestamp_columns()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py#with-timestamp-columns-fine-grain-timestamp--coarse-grain-timestamp-none--validate-false-)-Methode für die `TabularDataset`-Klasse, um Ihre Zeitspalte anzugeben und die Filterung nach Zeit zu ermöglichen. Weitere Beispiele und Details finden Sie [hier](https://aka.ms/azureml-tsd-notebook).

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

#### <a name="create-filedatasets"></a>Erstellen von FileDatasets

Verwenden Sie die [`from_files()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-)-Methode für die `FileDatasetFactory`-Klasse, um Dateien in einem beliebigen Format zu laden, und erstellen Sie ein nicht registriertes FileDataset.

```Python
# create a FileDataset from multiple paths in datastore
datastore_paths = [
                  (datastore, 'animals/dog/1.jpg'),
                  (datastore, 'animals/dog/2.jpg'),
                  (datastore, 'animals/dog/*.jpg')
                 ]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = [
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
            'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz'
           ]
mnist_ds = Dataset.File.from_files(path=web_paths)
```

### <a name="using-the-workspace-landing-page"></a>Verwenden der Landing Page des Arbeitsbereichs

Melden Sie sich bei der [Landing Page des Arbeitsbereichs](https://ml.azure.com) an, um ein Dataset über die Webbenutzeroberfläche zu erstellen. Die Landing Page des Arbeitsbereichs unterstützt die Erstellung von TabularDatasets ebenso wie FileDatasets.

In der folgenden Animation wird gezeigt, wie ein Dataset auf der Landing Page des Arbeitsbereichs erstellt wird.

Wählen Sie zuerst **Datasets** im Abschnitt **Assets** im linken Bereich aus. Wählen Sie dann **+ Dataset erstellen** aus, um die Quelle Ihres Datasets auszuwählen. Hierfür kommen lokale Dateien, Datenspeicher oder öffentliche Web-URLs infrage. Wählen Sie den **Datasettyp** aus: *tabellarisch oder Datei. Die Formulare **Einstellungen und Vorschau** und **Schema** werden auf intelligente Weise auf dem Dateityp basierend aufgefüllt. Wählen Sie **Weiter** aus, um sie zu überprüfen oder das Dataset vor der Erstellung weiter zu konfigurieren. Wählen Sie **Fertig** aus, um die Erstellung des Datasets abzuschließen.

![Erstellen eines Datasets mithilfe der Benutzeroberfläche](media/how-to-create-register-datasets/create-dataset-ui.gif)

## <a name="register-datasets"></a>Registrieren von Datasets

Registrieren Sie Ihre Datasets bei einem Arbeitsbereich, um den Erstellungsprozess abzuschließen.

Verwenden Sie die [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-)-Methode, um Datasets für Ihren Arbeitsbereich zu registrieren, damit diese für andere Benutzer freigegeben und für unterschiedliche Experimente wiederverwendet werden können.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data')
```

>[!Note]
> Datasets, die über die Landing Page des Arbeitsbereichs erstellt werden, werden automatisch im Arbeitsbereich registriert.

## <a name="version-datasets"></a>Versionsverwaltung von Datasets

Sie können ein neues Dataset unter demselben Namen registrieren, indem Sie eine neue Version erstellen. Die Verwaltung von Datasetversionen ist eine Möglichkeit, den Zustand der Daten zu markieren, sodass Sie eine bestimmte Version des Datasets für Experimente oder zukünftige Reproduktion anwenden können. Typische Szenarien, in denen sich Versionsverwaltung anbietet, sind: 

* Wenn neue Daten für ein erneutes Training verfügbar sind.
* Wenn Sie verschiedene Ansätze für Datenvorbereitung oder Merkmalsentwicklung anwenden.

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

Auf registrierte Datasets kann in Computeclustern wie Azure Machine Learning Compute lokal und remote zugegriffen werden. Verwenden Sie für den experimentübergreifenden Zugriff auf Ihr registriertes Dataset den folgenden Code, um Ihren Arbeitsbereich und das registrierte Dataset anhand des Namens abzurufen. Die [`get_by_name()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#get-by-name-workspace--name--version--latest--)-Methode mit der `Dataset`-Klasse gibt standardmäßig die neueste Version des im Arbeitsbereich registrierten Datasets zurück.

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
* Weitere Beispiele zum Training mit Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
