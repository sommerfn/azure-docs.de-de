---
title: Zugreifen auf Daten in Azure Storage-Dienste
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie mithilfe von Datenspeichern während des Trainings mit dem Azure Machine Learning Service auf Azure Storage-Dienste zugreifen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
ms.reviewer: nibaccam
ms.date: 08/2/2019
ms.custom: seodec18
ms.openlocfilehash: 4bc035ba061a65f6770136240d8867f82858e67e
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2019
ms.locfileid: "68772728"
---
# <a name="access-data-in-azure-storage-services"></a>Zugreifen auf Daten in Azure Storage-Diensten

 In diesem Artikel erfahren Sie, wie Sie auf einfache Weise über Azure Machine Learning-Datenspeicher auf Ihre Daten in Azure Storage-Diensten zugreifen können. Datenspeicher werden zum Speichern von Verbindungsinformationen wie Ihrer Abonnement-ID und der Tokenautorisierung verwendet, um auf Ihren Speicher zuzugreifen, ohne diese Informationen in Ihren Skripts hart codieren zu müssen.

In dieser Vorgehensweise finden Sie Beispiele für die folgenden Aufgaben:
* [Registrieren von Datenspeichern](#access)
* [Abrufen von Datenspeichern aus Arbeitsbereichen](#get)
* [Hochladen und Herunterladen von Daten mithilfe von Datenspeichern](#up-and-down)
* [Zugreifen auf Daten während des Trainings](#train)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie einen Datenspeicher verwenden möchten, benötigen Sie zunächst einen [Arbeitsbereich](concept-workspace.md).

Sie können entweder einen [neuen Arbeitsbereich erstellen](setup-create-workspace.md#sdk) oder einen vorhandenen Arbeitsbereich abrufen:

```Python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()
```

<a name="access"></a>

## <a name="register-datastores"></a>Registrieren von Datenspeichern

Alle Registriermethoden befinden sich in der [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py)-Klasse und weisen das Format „register_azure_*“ auf.

In den folgenden Beispielen wird gezeigt, wie Sie einen Azure-Blobcontainer oder eine Azure-Dateifreigabe als Datenspeicher registrieren können.

+ Verwenden Sie [`register_azure_blob-container()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) für einen **Azure-Blobcontainer-Datenspeicher**.

  ```Python
  datastore = Datastore.register_azure_blob_container(workspace=ws, 
                                                      datastore_name='your datastore name', 
                                                      container_name='your azure blob container name',
                                                      account_name='your storage account name', 
                                                      account_key='your storage account key',
                                                      create_if_not_exists=True)
  ```

+ Verwenden Sie [`register_azure_file_share()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#register-azure-file-share-workspace--datastore-name--file-share-name--account-name--sas-token-none--account-key-none--protocol-none--endpoint-none--overwrite-false--create-if-not-exists-false--skip-validation-false-) für einen **Azure-Dateifreigabe-Datenspeicher**. Beispiel: 
  ```Python
  datastore = Datastore.register_azure_file_share(workspace=ws, 
                                                  datastore_name='your datastore name', 
                                                  file_share_name='your file share name',
                                                  account_name='your storage account name', 
                                                  account_key='your storage account key',
                                                  create_if_not_exists=True)
  ```

####  <a name="storage-guidance"></a>Leitfaden für Speicher

Wir empfehlen Azure-Blob-Container. Für Blobs stehen sowohl der Standard- als auch der Premiumspeicher zur Verfügung. Der Premiumspeicher ist zwar teurer, aber dennoch die bevorzugte Option, da er höhere Durchsätze ermöglicht. Dies kann sich vor allem bei einem Training mit einem großen Dataset positiv auf die Ausführungsgeschwindigkeit auswirken. Weitere Informationen zu den Kosten von Speicherkonten stellt der [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=machine-learning-service) bereit.

<a name="get"></a>

## <a name="get-datastores-from-your-workspace"></a>Abrufen von Datenspeichern aus Ihrem Arbeitsbereich

Verwenden Sie die statische Methode [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#get-workspace--datastore-name-) mit der Datastore-Klasse, um einen bestimmten Datenspeicher im aktuellen Arbeitsbereich zu registrieren:

```Python
#get named datastore from current workspace
datastore = Datastore.get(ws, datastore_name='your datastore name')
```
Um die Liste der in einem bestimmten Arbeitsbereich registrierten Datenspeicher abzurufen, können Sie die `datastores`-Eigenschaft für ein Arbeitsbereichsobjekt verwenden:

```Python
#list all datastores registered in current workspace
datastores = ws.datastores
for name, datastore in datastores.items():
    print(name, datastore.datastore_type)
```

Wenn Sie einen Arbeitsbereich erstellen, werden ein Azure-Blob-Container und eine Azure-Dateifreigabe in dem Arbeitsbereich namens `workspaceblobstore` bzw. `workspacefilestore` registriert. Darin werden die Verbindungsinformationen des Blobcontainers und der Dateifreigabe, die im Speicherkonto bereitgestellt wird, das mit dem Arbeitsbereich verbunden ist, gespeichert. `workspaceblobstore` wird als Standarddatenspeicher festgelegt.

So rufen Sie den Standarddatenspeicher des Arbeitsbereichs ab:

```Python
datastore = ws.get_default_datastore()
```

Verwenden Sie die Methode [`set_default_datastore()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#set-default-datastore-name-) mit dem Arbeitsbereichsobjekt, um einen anderen Standarddatenspeicher für den aktuellen Arbeitsbereich zu definieren:

```Python
#define default datastore for current workspace
ws.set_default_datastore('your datastore name')
```

<a name="up-and-down"></a>
## <a name="upload--download-data"></a>Hoch- und herunterladen von Daten
Die in den folgenden Beispielen beschriebenen [`upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)- und [`download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py#download-target-path--prefix-none--overwrite-false--show-progress-true-)-Methoden sind für die [AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)- und [AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)-Klassen bestimmt und werden identisch ausgeführt.

### <a name="upload"></a>Hochladen

 Laden Sie entweder ein Verzeichnis oder einzelne Dateien mit dem Python SDK in den Datenspeicher hoch.

So laden Sie ein Verzeichnis in einen Datenspeicher `datastore` hoch:

```Python
import azureml.data
from azureml.data.azure_storage_datastore import AzureFileDatastore, AzureBlobDatastore

datastore.upload(src_dir='your source directory',
                 target_path='your target path',
                 overwrite=True,
                 show_progress=True)
```

Der Parameter `target_path` gibt den Speicherort in der Dateifreigabe (oder im Blobcontainer) für den Upload an. Standardmäßig lautet er `None`. In diesem Fall werden die Daten in den Stamm hochgeladen. Bei `overwrite=True` werden alle in `target_path` vorhandenen Daten überschrieben.

Alternativ können Sie eine Liste einzelner Dateien mithilfe der `upload_files()`-Methode in den Datenspeicher hochladen.

### <a name="download"></a>Download

Ebenso lassen sich Daten aus einem Datenspeicher in Ihr lokales Dateisystem herunterladen.

```Python
datastore.download(target_path='your target path',
                   prefix='your prefix',
                   show_progress=True)
```

Der Parameter `target_path` ist der Speicherort des lokalen Verzeichnisses, in das die Daten heruntergeladen werden sollen. Um einen Pfad zu dem Ordner in der Dateifreigabe (oder im Blobcontainer) für den Download anzugeben, geben Sie diesen Pfad mit `prefix` an. Wenn `prefix` gleich `None` ist, werden alle Inhalte Ihrer Dateifreigabe (oder Ihres Blobcontainers) heruntergeladen.

<a name="train"></a>
## <a name="access-your-data-during-training"></a>Zugreifen auf Ihre Daten während des Trainings

Wenn Sie während des Trainings auf Daten zugreifen möchten, können Sie Ihre Daten aus ihren Azure Storage-Diensten entweder auf das Computeziel herunterladen oder über Datenspeicher dort einbinden.

Die folgende Tabelle listet die Methoden auf, die dem Computeziel mitteilen, wie der Datenspeicher während der Ausführungen verwendet wird. 

Weg|Methode|BESCHREIBUNG|
----|-----|--------
Einbinden| [`as_mount()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-mount--)| Wird zum Einbinden des Datenspeichers auf dem Computeziel verwendet.
Download|[`as_download()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-download-path-on-compute-none-)|Wird zum Herunterladen der Inhalte Ihres Datenspeichers in den unter `path_on_compute` angegebenen Speicherort verwendet. <br> Dieser Download findet vor der Ausführung statt.
Hochladen|[`as_upload()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#as-upload-path-on-compute-none-)| Wird zum Hochladen einer Datei aus dem von `path_on_compute` angegebenen Speicherort in Ihren Datenspeicher verwendet. <br> Dieser Upload findet nach der Ausführung statt.

Um auf bestimmte Ordner oder Dateien in Ihrem Datenspeicher zu verweisen und sie im Computeziel verfügbar zu machen, verwenden Sie die Datenspeichermethode [`path()`](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.abstractazurestoragedatastore?view=azure-ml-py#path-path-none--data-reference-name-none-).

```Python
#to mount the full contents in your storage to the compute target
datastore.as_mount()

#to download the contents of the `./bar` directory in your storage to the compute target
datastore.path('./bar').as_download()
```
> [!NOTE]
> Jedes `datastore`- oder `datastore.path`-Objekt wird in den Namen einer Umgebungsvariablen des Formats `"$AZUREML_DATAREFERENCE_XXXX"` aufgelöst, deren Wert für den Einbindungs-/Downloadpfad im Zielcompute steht. Der Datenspeicherpfad auf dem Zielcompute stimmt möglicherweise nicht mit dem Ausführungspfad für das Trainingsskript überein.

### <a name="examples"></a>Beispiele 

Die folgenden Codebeispiele beziehen sich speziell auf die [`Estimator`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Klasse für den Zugriff auf Daten während des Trainings. 

`script_params` ist ein Wörterbuch, das Parameter für entry_script enthält. Sie können es verwenden, um einen Datenspeicher als Eingabe zu übergeben und zu beschreiben, wie Daten auf dem Computeziel verfügbar gemacht werden sollen. Weitere Informationen finden Sie in unserem End-to-End-[Tutorial](tutorial-train-models-with-aml.md).

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data_dir': datastore.path('/bar').as_mount()
}

est = Estimator(source_directory='your code directory',
                entry_script='train.py',
                script_params=script_params,
                compute_target=compute_target
                )
```

Sie können auch eine Liste von Datenspeichern an den Parameter `inputs` des Estimatorkonstruktors übergeben, um Daten in oder aus Ihrem/n Datenspeicher(n) einzubinden oder zu kopieren. Dieses Codebeispiel:
* Lädt alle Inhalte in `datastore1` auf das Computeziel herunter, bevor das Trainingsskript `train.py` ausgeführt wird.
* Lädt den Ordner `'./foo'` in `datastore2` auf das Computeziel herunter, bevor `train.py` ausgeführt wird.
* Lädt die Datei `'./bar.pkl'` aus dem Computeziel in den `datastore3` hoch, nachdem Ihr Skript ausgeführt wurde.

```Python
est = Estimator(source_directory='your code directory',
                compute_target=compute_target,
                entry_script='train.py',
                inputs=[datastore1.as_download(), datastore2.path('./foo').as_download(), datastore3.as_upload(path_on_compute='./bar.pkl')])
```
### <a name="compute-and-datastore-matrix"></a>Compute- und Datenspeichermatrix

Datenspeicher unterstützen derzeit das Speichern von Verbindungsinformationen in den Speicherdiensten, die in der folgenden Matrix aufgeführt sind. Diese Matrix zeigt die verfügbaren Datenzugriffsfunktionen für die verschiedenen Computeziele- und Datenspeicherszenarien. Weitere Informationen zu den [Computezielen für Azure Machine Learning](how-to-set-up-training-targets.md#compute-targets-for-training).

|Compute|[AzureBlobDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azureblobdatastore?view=azure-ml-py)                                       |[AzureFileDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_storage_datastore.azurefiledatastore?view=azure-ml-py)                                      |[AzureDataLakeDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakedatastore?view=azure-ml-py) |[AzureDataLakeGen2Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_data_lake_datastore.azuredatalakegen2datastore?view=azure-ml-py) [AzurePostgreSqlDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_postgre_sql_datastore.azurepostgresqldatastore?view=azure-ml-py) [AzureSqlDatabaseDatastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data.azure_sql_database_datastore.azuresqldatabasedatastore?view=azure-ml-py) |
|--------------------------------|----------------------------------------------------------|----------------------------------------------------------|------------------------|----------------------------------------------------------------------------------------|
| Lokal|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)|–         |–                                                                         |
| Azure Machine Learning Compute |[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;Pipelines](concept-ml-pipelines.md)|[as_mount()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-mount--), [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-), [ML&nbsp;Pipelines](concept-ml-pipelines.md)|–         |–                                                                         |
| Virtuelle Computer               |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-), [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                           | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |–         |–                                                                         |
| HDInsight                      |[as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            | [as_download()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-download-path-on-compute-none--overwrite-false-) [as_upload()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py#as-upload-path-on-compute-none--overwrite-false-)                            |–         |–                                                                         |
| Datenübertragung                  |[ML&nbsp;Pipelines](concept-ml-pipelines.md)                                               |–                                           |[ML&nbsp;Pipelines](concept-ml-pipelines.md)            |[ML&nbsp;Pipelines](concept-ml-pipelines.md)                                                                            |
| Databricks                     |[ML&nbsp;Pipelines](concept-ml-pipelines.md)                                              |–                                           |[ML&nbsp;Pipelines](concept-ml-pipelines.md)             |–                                                                         |
| Azure Batch                    |[ML&nbsp;Pipelines](concept-ml-pipelines.md)                                               |–                                           |–         |–                                                                         |
| Azure DataLake Analytics       |–                                           |–                                           |[ML&nbsp;Pipelines](concept-ml-pipelines.md)             |–                                                                         |

> [!NOTE]
> Es gibt möglicherweise Szenarios, in denen umfangreiche Datenverarbeitungsprozesse mit vielen Wiederholungen schneller mit `as_download()` anstelle von `as_mount()` ausgeführt werden. Dies können Sie in Experimenten überprüfen.

## <a name="access-data-during-scoring"></a>Zugreifen auf Daten während der Bewertung

Der Azure Machine Learning Service bietet mehrere Möglichkeiten, Ihre Modelle zur Bewertung zu verwenden. Einige dieser Methoden bieten keinen Zugriff auf Datenspeicher. Verwenden Sie die folgende Tabelle, um zu verstehen, welche Methoden Ihnen ermöglichen, während der Bewertung auf Datenspeicher zuzugreifen:

| Methode | Datenspeicherzugriff | BESCHREIBUNG |
| ----- | :-----: | ----- |
| [Batchvorhersage](how-to-run-batch-predictions.md) | ✔ | Treffen Sie asynchron Vorhersagen für große Datenmengen. |
| [Webdienst](how-to-deploy-and-where.md) | &nbsp; | Stellen Sie ein Modell bzw. Modelle als Webdienst bereit. |
| [IoT Edge-Modul](how-to-deploy-and-where.md) | &nbsp; | Stellen Sie ein Modell bzw. Modelle auf IoT Edge-Geräten bereit. |

Für Situationen, in denen das SDK keinen Zugriff auf Datenspeicher bietet, können Sie möglicherweise benutzerdefinierten Code mit dem entsprechenden Azure SDK erstellen, um auf die Daten zuzugreifen. Verwenden Sie beispielsweise das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python), um auf in Blobs gespeicherte Daten zuzugreifen.


## <a name="next-steps"></a>Nächste Schritte

* [Trainieren eines Modells](how-to-train-ml-models.md)

* [Bereitstellen eines Modells](how-to-deploy-and-where.md)
