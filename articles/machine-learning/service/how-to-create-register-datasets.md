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
ms.openlocfilehash: c5b423fca3e0ec116fceefb6867189f4f8413b96
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856083"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Erstellen von und Zugreifen auf Datasets (Vorschauversion) in Azure Machine Learning

In diesem Artikel wird beschrieben, wie Sie Azure Machine Learning-Datasets (Vorschauversion) erstellen und auf Daten in lokalen oder remotebasierten Experimenten zugreifen.

Azure Machine Learning-Datasets ermöglichen Folgendes: 

* **Aufbewahren einer einzelnen Datenkopie in Ihrem Speicher**, auf die durch Datasets verwiesen wird

* **Analysieren von Daten** mithilfe explorativer Datenanalysen 

* **Müheloses Zugreifen auf Daten während des Modelltrainings**, ohne sich Gedanken über Verbindungszeichenfolgen oder Datenpfade machen zu müssen

* **Freigeben von Daten und Zusammenarbeiten** mit anderen Benutzern

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um Datasets zu erstellen und zu nutzen:

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Ein [Azure Machine Learning Service-Arbeitsbereich](how-to-manage-workspace.md)

* Eine [Installation des Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), in dem das Paket „azureml-datasets“ enthalten ist.

> [!Note]
> Einige Dataset-Klassen (Vorschauversion) verfügen über Abhängigkeiten vom [azureml-dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py)-Paket (GA). Für Linux-Benutzer werden diese Klassen nur unter den folgenden Distributionen unterstützt:  Red Hat Enterprise Linux, Ubuntu, Fedora und CentOS.

## <a name="data-formats"></a>Datenformate

Folgende Formate können zur Erstellung eines Azure Machine Learning-Datasets verwendet werden:
+ [delimited](/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none--partition-format-none-)
+ [json](/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false--partition-format-none-)
+ [Excel](/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true--partition-format-none-)
+ [Parquet](/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false--partition-format-none-)
+ [pandas DataFrame](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-pandas-dataframe-dataframe--path-none--in-memory-false-)
+ [SQL query](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [binary](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)

## <a name="create-datasets"></a>Erstellen von Datasets 

Durch Erstellen eines Datasets erstellen Sie einen Verweis auf den Speicherort der Datenquelle sowie eine Kopie der zugehörigen Metadaten. Die Daten bleiben an ihrem Speicherort, sodass keine zusätzlichen Speicherkosten anfallen.

### <a name="create-from-local-files"></a>Erstellen aus lokalen Dateien

Um Dateien von Ihrem lokalen Computer zu laden, geben Sie den Pfad von Datei oder Ordner mit der [`auto_read_files()`](/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false--partition-format-none-)-Methode aus der `Dataset`-Klasse an.  Diese Methode führt die folgenden Schritte aus, ohne dass Sie den Dateityp angeben oder Argumente analysieren müssen:

* Ableiten und Festlegen von Trennzeichen.
* Überspringen leerer Datensätze am Anfang der Datei.
* Ableiten und Festlegen der Kopfzeile.
* Ableiten und Konvertieren von Spaltendatentypen.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Verwenden Sie alternativ die dateispezifischen Funktionen, um explizit die Analyse der Datei zu steuern. 


### <a name="create-from-azure-datastores"></a>Erstellen aus Azure-Datenspeichern

Gehen Sie wie folgt vor, um Datasets auf der Grundlage eines [Azure-Datenspeichers](how-to-access-data.md) zu erstellen:

* Vergewissern Sie sich, dass Sie für den registrierten Azure-Datenspeicher über Zugriff vom Typ `contributor` oder `owner` verfügen.

* Erstellen Sie das Dataset, indem Sie auf einen Pfad im Datenspeicher verweisen: 

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()

# retrieve an existing datastore in the workspace by name
dstore = Datastore.get(workspace, datastore_name)
```

Verwenden Sie die Methode `from_delimited_files()`, um durch Trennzeichen getrennte Dateien aus einer Klasse vom Typ [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py) zu lesen und ein nicht registriertes Dataset zu erstellen.

```Python
# create an in-memory Dataset on your local machine
dataset = Dataset.from_delimited_files(dstore.path('data/src/crime.csv'))

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Registrieren von Datasets

Registrieren Sie Ihre Datasets beim Arbeitsbereich, um den Erstellungsprozess abzuschließen:

Verwenden Sie die [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-)-Methode zum Registrieren von Datasets in Ihrem Arbeitsbereich, damit diese für andere Benutzer freigegeben und für unterschiedliche Experimente wiederverwendet werden können.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Es tritt ein Fehler auf, wenn `exist_ok = False` (Standardeinstellung) gilt und Sie versuchen, ein Dataset unter dem gleichen Namen wie für ein anderes Dataset zu registrieren. Legen Sie diese Option auf `True` fest, um das Überschreiben von vorhandenen Datasets zuzulassen.

## <a name="access-data-in-datasets"></a>Zugreifen auf Daten in Datasets

Auf registrierte Datasets kann in Computeclustern wie Azure Machine Learning Compute lokal und remote zugegriffen werden. Verwenden Sie für den experimentübergreifenden Zugriff auf Ihr registriertes Dataset den folgenden Code, um Ihren Arbeitsbereich und das registrierte Dataset anhand des Namens abzurufen.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
print(Dataset.list(workspace))

# Get dataset by name
dataset = Dataset.get(workspace, 'dataset_crime')

# Load data into pandas DataFrame
dataset.to_pandas_dataframe()
```

## <a name="next-steps"></a>Nächste Schritte

* [Untersuchen und Aufbereiten von Datasets](how-to-explore-prepare-data.md).
* Ein Beispiel für die Verwendung von Datasets finden Sie in den [Beispielnotebooks](https://aka.ms/dataset-tutorial).
