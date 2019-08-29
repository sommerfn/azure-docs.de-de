---
title: Erstellen von Datasets zum Zugreifen auf Daten mit „azureml-datasets“
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie Datasets aus verschiedenen Quellen erstellen und Datasets in Ihrem Arbeitsbereich registrieren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: 67dda1ab56c6a706a9fdbef45fabdae9167ffe2b
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69616344"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Erstellen von und Zugreifen auf Datasets (Vorschauversion) in Azure Machine Learning

In diesem Artikel wird beschrieben, wie Sie Azure Machine Learning-Datasets (Vorschauversion) erstellen und auf Daten in lokalen oder remotebasierten Experimenten zugreifen.

Azure Machine Learning-Datasets ermöglichen Folgendes: 

* **Aufbewahren einer einzelnen Datenkopie in Ihrem Speicher**, auf die durch Datasets verwiesen wird. 

* **Müheloses Zugreifen auf Daten während des Modelltrainings**, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssen.

* **Freigeben von Daten und Zusammenarbeiten** mit anderen Benutzern.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning Service-Arbeitsbereich](how-to-manage-workspace.md)

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!Note]
> Einige Dataset-Klassen (Vorschauversion) verfügen über Abhängigkeiten vom [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py)-Paket. Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="dataset-types"></a>Datasettypen
Datasets werden in verschiedene Typen eingeteilt, je nachdem, wie Benutzer Sie beim Training nutzen. Zurzeit unterstützen wir TabularDatasets, die Daten in einem Tabellenformat darstellen, indem sie die bereitgestellte Datei oder Liste der Dateien analysieren. Dadurch erhalten Sie die Möglichkeit, die Daten in einem pandas-DataFrame zu materialisieren. TabularDataset kann aus CSV, TSV, parquet-Dateien, SQL-Abfrageergebnissen usw. erstellt werden. Eine vollständige Liste finden Sie in unserer Dokumentation.

Weitere Informationen zu bevorstehenden API-Änderungen finden Sie unter [Was ist Azure Machine Learning Service?](https://aka.ms/tabular-dataset). 

## <a name="create-datasets"></a>Erstellen von Datasets 

Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Die Daten bleiben an ihrem Speicherort, sodass keine zusätzlichen Speicherkosten anfallen.

Damit Azure Machine Learning Service auf die Daten zugreifen kann, müssen Datasets aus Pfaden in [Azure-Datenspeichern](how-to-access-data.md) oder öffentlichen Web-URLs erstellt werden.

Gehen Sie wie folgt vor, um Datasets auf der Grundlage eines [Azure-Datenspeichers](how-to-access-data.md) zu erstellen:

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
### <a name="create-tabulardatasets"></a>Erstellen von TabularDatasets

Verwenden Sie die `from_delimited_files()`-Methode mit der `TabularDatasetFactory`-Klasse, um Dateien im CSV- oder TSV-Format zu lesen, und erstellen Sie ein nicht registriertes TabularDataset. Wenn Sie aus mehreren Dateien lesen, werden die Ergebnisse in einer Tabellendarstellung aggregiert.

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

## <a name="register-datasets"></a>Registrieren von Datasets

Registrieren Sie Ihre Datasets beim Arbeitsbereich, um den Erstellungsprozess abzuschließen:

Verwenden Sie die `register()`-Methode zum Registrieren von Datasets in Ihrem Arbeitsbereich, damit diese für andere Benutzer freigegeben und für unterschiedliche Experimente wiederverwendet werden können.

```Python
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'titanic training data')
```

## <a name="version-datasets"></a>Versionsverwaltung von Datasets

Sie können ein neues Dataset unter demselben Namen registrieren, indem Sie eine neue Version erstellen. Die Verwaltung von Datasetversionen ist eine Möglichkeit, den Zustand der Daten zu markieren, sodass Sie eine bestimmte Version des Datasets für Experimente oder zukünftige Reproduktion anwenden können. Typische Szenarien für die Versionsverwaltung: 
* Wenn neue Daten für ein erneutes Training verfügbar sind.
* Wenn Sie verschiedene Ansätze für Datenvorbereitung oder Merkmalsentwicklung anwenden.

```Python
# create a TabularDataset from new Titanic training data
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


## <a name="access-your-data-during-training"></a>Zugreifen auf Ihre Daten während des Trainings

Auf registrierte Datasets kann in Computeclustern wie Azure Machine Learning Compute lokal und remote zugegriffen werden. Verwenden Sie für den experimentübergreifenden Zugriff auf Ihr registriertes Dataset den folgenden Code, um Ihren Arbeitsbereich und das registrierte Dataset anhand des Namens abzurufen. Die `get_by_name`-Methode mit der `Dataset`-Klasse gibt standardmäßig die neueste Version des im Arbeitsbereich registrierten Datasets zurück.

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

* Verwenden Sie automatisiertes Machine Learning, um [mit TabularDatasets zu trainieren](https://aka.ms/automl-dataset).
* Weitere Beispiele zum Training mit Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
