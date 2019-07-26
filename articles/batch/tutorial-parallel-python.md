---
title: Ausführen einer parallelen Workload – Azure Batch Python
description: Tutorial – Paralleles Verarbeiten von Mediendateien mit ffmpeg in Azure Batch per Batch Python-Clientbibliothek
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.devlang: python
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: lahugh
ms.custom: mvc
ms.openlocfilehash: 92d8c6fb1bfa1689475774bbc4f62cd9ab38268f
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321831"
---
# <a name="tutorial-run-a-parallel-workload-with-azure-batch-using-the-python-api"></a>Tutorial: Ausführen einer parallelen Workload mit Azure Batch über die Python-API

Mithilfe von Azure Batch können Sie umfangreiche auf Parallelverarbeitung ausgelegte HPC-Batchaufträge (High Performance Computing) effizient in Azure ausführen. In diesem Tutorial wird Schritt für Schritt ein Python-Beispiel für die Ausführung einer parallelen Workload per Batch beschrieben. Sie erfahren, wie Sie einen gängigen Batch-Anwendungsworkflow durchführen und programmgesteuert mit Batch- und Storage-Ressourcen interagieren. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Authentifizieren mit Batch- und Storage-Konten
> * Hochladen von Eingabedateien in Storage
> * Erstellen eines Pools mit Computeknoten für die Ausführung einer Anwendung
> * Erstellen eines Auftrags und von Aufgaben zum Verarbeiten von Eingabedateien
> * Überwachen der Aufgabenausführung
> * Abrufen von Ausgabedateien

In diesem Tutorial konvertieren Sie MP4-Mediendateien parallel in das MP3-Format, indem Sie das Open-Source-Tool [ffmpeg](https://ffmpeg.org/) verwenden. 

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Python-Version 2.7 oder 3.3 oder höher](https://www.python.org/downloads/)

* [pip](https://pip.pypa.io/en/stable/installing/)-Paket-Manager

* Ein Azure Batch-Konto und ein verknüpftes Azure Storage-Konto. Informationen zur Erstellung dieser Konten finden Sie in den Batch-Schnellstartanleitungen zum [Azure-Portal](quick-create-portal.md) und zur [Azure CLI](quick-create-cli.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

[!INCLUDE [batch-common-credentials](../../includes/batch-common-credentials.md)] 

## <a name="download-and-run-the-sample"></a>Herunterladen und Ausführen des Beispiels

### <a name="download-the-sample"></a>Herunterladen des Beispiels

[Laden Sie die Beispiel-App von GitHub herunter, oder klonen Sie sie](https://github.com/Azure-Samples/batch-python-ffmpeg-tutorial). Verwenden Sie den folgenden Befehl, um das Beispiel-App-Repository mit einem Git-Client zu klonen:

```
git clone https://github.com/Azure-Samples/batch-python-ffmpeg-tutorial.git
```

Navigieren Sie zum Verzeichnis mit der Datei `batch_python_tutorial_ffmpeg.py`.

Installieren Sie in Ihrer Python-Umgebung die erforderlichen Pakete mit `pip`.

```bash
pip install -r requirements.txt
```

Öffnen Sie die Datei `config.py`. Aktualisieren Sie Zeichenfolgen mit den Anmeldeinformationen für das Batch- und Storage-Konto mit den Werten Ihrer Konten. Beispiel:


```Python
_BATCH_ACCOUNT_NAME = 'mybatchaccount'
_BATCH_ACCOUNT_KEY = 'xxxxxxxxxxxxxxxxE+yXrRvJAqT9BlXwwo1CwF+SwAYOxxxxxxxxxxxxxxxx43pXi/gdiATkvbpLRl3x14pcEQ=='
_BATCH_ACCOUNT_URL = 'https://mybatchaccount.mybatchregion.batch.azure.com'
_STORAGE_ACCOUNT_NAME = 'mystorageaccount'
_STORAGE_ACCOUNT_KEY = 'xxxxxxxxxxxxxxxxy4/xxxxxxxxxxxxxxxxfwpbIC5aAWA8wDu+AFXZB827Mt9lybZB1nUcQbQiUrkPtilK5BQ=='
```

### <a name="run-the-app"></a>Ausführen der App

So führen Sie das Skript aus

```
python batch_python_tutorial_ffmpeg.py
```

Beim Ausführen der Beispielanwendung sieht die Konsolenausgabe in etwa wie folgt aus: Bei der Ausführung kommt es bei `Monitoring all tasks for 'Completed' state, timeout in 00:30:00...` zu einer Pause, während die Computeknoten des Pools gestartet werden. 
   
```
Sample start: 11/28/2018 3:20:21 PM

Container [input] created.
Container [output] created.
Uploading file LowPriVMs-1.mp4 to container [input]...
Uploading file LowPriVMs-2.mp4 to container [input]...
Uploading file LowPriVMs-3.mp4 to container [input]...
Uploading file LowPriVMs-4.mp4 to container [input]...
Uploading file LowPriVMs-5.mp4 to container [input]...
Creating pool [LinuxFFmpegPool]...
Creating job [LinuxFFmpegJob]...
Adding 5 tasks to job [LinuxFFmpegJob]...
Monitoring all tasks for 'Completed' state, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Deleting container [input]....

Sample end: 11/28/2018 3:29:36 PM
Elapsed time: 00:09:14.3418742
```

Navigieren Sie im Azure-Portal zu Ihrem Batch-Konto, um den Pool, die Computeknoten, den Auftrag und die Aufgaben zu überwachen. Klicken Sie beispielsweise auf **Pools** > *LinuxFFmpegPool*, um ein Wärmebild mit den Computeknoten Ihres Pools anzuzeigen.

Bei der Ausführung von Aufgaben sieht das Wärmebild in etwa wie folgt aus:

![Wärmebild für Pool](./media/tutorial-parallel-python/pool.png)

Die normale Ausführungsdauer beträgt ca. **fünf Minuten**, wenn die Anwendung in der Standardkonfiguration ausgeführt wird. Die meiste Zeit wird für die Poolerstellung benötigt. 

[!INCLUDE [batch-common-tutorial-download](../../includes/batch-common-tutorial-download.md)]

## <a name="review-the-code"></a>Überprüfen des Codes

In den folgenden Abschnitten ist die Beispielanwendung in die Schritte unterteilt, die ausgeführt werden, um eine Workload im Batch-Dienst zu verarbeiten. Ziehen Sie beim Lesen des restlichen Teils dieses Artikels den Python-Code zurate, da nicht jede Codezeile im Beispiel beschrieben wird.

### <a name="authenticate-blob-and-batch-clients"></a>Authentifizieren des Blobs und der Batch-Clients

Für die Interaktion mit einem Speicherkonto verwendet die App das Paket [azure-storage-blob](https://pypi.python.org/pypi/azure-storage-blob) zum Erstellen eines [BlockBlobService](/python/api/azure.storage.blob.blockblobservice.blockblobservice)-Objekts.

```python
blob_client = azureblob.BlockBlobService(
    account_name=_STORAGE_ACCOUNT_NAME,
    account_key=_STORAGE_ACCOUNT_KEY)
```

Die App erstellt ein [BatchServiceClient](/python/api/azure.batch.batchserviceclient)-Objekt zum Erstellen und Verwalten von Pools, Aufträgen und Aufgaben im Batch-Dienst. Für den Batch-Client im Beispiel wird die Authentifizierung mit gemeinsam verwendetem Schlüssel genutzt. Batch unterstützt auch die Authentifizierung per [Azure Active Directory](batch-aad-auth.md), um einzelne Benutzer oder eine unbeaufsichtigte Anwendung zu authentifizieren.

```python
credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                             _BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=_BATCH_ACCOUNT_URL)
```

### <a name="upload-input-files"></a>Hochladen von Eingabedateien

Die App verwendet den `blob_client`-Verweis, um einen Speichercontainer für die MP4-Eingabedateien und einen Container für die Aufgabenausgabe zu erstellen. Anschließend wird die Funktion `upload_file_to_container` aufgerufen, um MP4-Dateien aus dem lokalen Verzeichnis `InputFiles` in den Container hochzuladen. Die Dateien im Speicher werden als Batch-[ResourceFile](/python/api/azure.batch.models.resourcefile)-Objekte definiert, die von Batch später auf Computeknoten heruntergeladen werden können.

```python
blob_client.create_container(input_container_name, fail_on_exist=False)
blob_client.create_container(output_container_name, fail_on_exist=False)
input_file_paths = []

for folder, subs, files in os.walk(os.path.join(sys.path[0], './InputFiles/')):
    for filename in files:
        if filename.endswith(".mp4"):
            input_file_paths.append(os.path.abspath(
                os.path.join(folder, filename)))

# Upload the input files. This is the collection of files that are to be processed by the tasks.
input_files = [
    upload_file_to_container(blob_client, input_container_name, file_path)
    for file_path in input_file_paths]
```

### <a name="create-a-pool-of-compute-nodes"></a>Erstellen eines Pools mit Computeknoten

Als Nächstes erstellt das Beispiel im Batch-Konto durch Aufrufen von `create_pool` einen Pool mit Computeknoten. Für diese definierte Funktion wird die Batch-Klasse [PoolAddParameter](/python/api/azure.batch.models.pooladdparameter) verwendet, um die Anzahl von Knoten, die VM-Größe und eine Poolkonfiguration festzulegen. Hier gibt ein [VirtualMachineConfiguration](/python/api/azure.batch.models.virtualmachineconfiguration)-Objekt einen [ImageReference](/python/api/azure.batch.models.imagereference)-Verweis auf ein Ubuntu Server 18.04 LTS-Image an, das im Azure Marketplace veröffentlicht wurde. Batch unterstützt viele verschiedene VM-Images im Azure Marketplace und auch benutzerdefinierte VM-Images.

Die Anzahl von Knoten und die VM-Größe werden mit definierten Konstanten festgelegt. Batch unterstützt dedizierte Knoten und [Knoten mit niedriger Priorität](batch-low-pri-vms.md), und Sie können eine oder beide Arten von Knoten in Ihren Pools verwenden. Dedizierte Knoten sind für Ihren Pool reserviert. Knoten mit niedriger Priorität werden zu einem reduzierten Preis basierend auf überschüssiger VM-Kapazität in Azure angeboten. Knoten mit niedriger Priorität sind nicht mehr verfügbar, wenn in Azure nicht genügend Kapazität vorhanden ist. Im Beispiel wird standardmäßig ein Pool mit nur fünf Knoten mit niedriger Priorität und der Größe *Standard_A1_v2* erstellt. 

Zusätzlich zu den Eigenschaften des physischen Knotens enthält diese Poolkonfiguration ein [StartTask](/python/api/azure.batch.models.starttask)-Objekt. Die StartTask wird auf jedem Knoten ausgeführt, wenn dieser dem Pool hinzugefügt wird, sowie bei jedem Neustart eines Knotens. In diesem Beispiel führt die StartTask Bash-Shellbefehle aus, um das ffmpeg-Paket und Abhängigkeiten auf den Knoten zu installieren.

Mit der [pool.add](/python/api/azure.batch.operations.pooloperations)-Methode wird der Pool an den Batch-Dienst übermittelt.

```python
new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="18.04-LTS",
            version="latest"
        ),
        node_agent_sku_id="batch.node.ubuntu 18.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_DEDICATED_POOL_NODE_COUNT,
    target_low_priority_nodes=_LOW_PRIORITY_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install -y ffmpeg\"",
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(new_pool)
```

### <a name="create-a-job"></a>Erstellen eines Auftrags

Für einen Batch-Auftrag werden ein Pool zum Ausführen von Aufgaben und optionale Einstellungen wie eine Priorität und ein Zeitplan für die Arbeitsschritte angegeben. Im Beispiel wird ein Auftrag mit einem Aufruf von `create_job` erstellt. Bei dieser definierten Funktion wird die [JobAddParameter](/python/api/azure.batch.models.jobaddparameter)-Klasse verwendet, um einen Auftrag in Ihrem Pool zu erstellen. Mit der [job.add](/python/api/azure.batch.operations.joboperations)-Methode wird der Pool an den Batch-Dienst übermittelt. Der Auftrag enthält ursprünglich keine Aufgaben.

```python
job = batch.models.JobAddParameter(
    id=job_id,
    pool_info=batch.models.PoolInformation(pool_id=pool_id))

batch_service_client.job.add(job)
```

### <a name="create-tasks"></a>Erstellen von Aufgaben

Die App erstellt Aufgaben im Auftrag per Aufruf von `add_tasks`. Diese benutzerdefinierte Funktion erstellt eine Liste mit Aufgabenobjekten, indem die [TaskAddParameter](/python/api/azure.batch.models.taskaddparameter)-Klasse verwendet wird. Für jede Aufgabe wird ffmpeg ausgeführt, um ein `resource_files`-Eingabeobjekt mit dem Parameter `command_line` zu verarbeiten. ffmpeg wurde zuvor bei der Erstellung des Pools auf jedem Knoten installiert. Hier wird in der Befehlszeile ffmpeg ausgeführt, um jede MP4-Eingabedatei (Video) in eine MP3-Datei (Audio) zu konvertieren.

Im Beispiel wird nach der Ausführung über die Befehlszeile ein [OutputFile](/python/api/azure.batch.models.outputfile)-Objekt für die MP3-Datei erstellt. Die Ausgabedateien (in diesem Fall eine Datei) jeder Aufgabe werden in einen Container im verknüpften Speicherkonto hochgeladen, indem die `output_files`-Eigenschaft der Aufgabe verwendet wird.

Anschließend werden dem Auftrag von der App mit der [task.add_collection](/python/api/azure.batch.operations.taskoperations)-Methode Aufgaben hinzugefügt und für die Ausführung auf den Computeknoten in die Warteschlange eingereiht. 

```python
tasks = list()

for idx, input_file in enumerate(input_files):
    input_file_path = input_file.file_path
    output_file_path = "".join((input_file_path).split('.')[:-1]) + '.mp3'
    command = "/bin/bash -c \"ffmpeg -i {} {} \"".format(
        input_file_path, output_file_path)
    tasks.append(batch.models.TaskAddParameter(
        id='Task{}'.format(idx),
        command_line=command,
        resource_files=[input_file],
        output_files=[batchmodels.OutputFile(
            file_pattern=output_file_path,
            destination=batchmodels.OutputFileDestination(
                container=batchmodels.OutputFileBlobContainerDestination(
                    container_url=output_container_sas_url)),
            upload_options=batchmodels.OutputFileUploadOptions(
                upload_condition=batchmodels.OutputFileUploadCondition.task_success))]
    )
    )
batch_service_client.task.add_collection(job_id, tasks)
```

### <a name="monitor-tasks"></a>Überwachen von Aufgaben

Wenn Aufgaben einem Auftrag hinzugefügt werden, werden sie von Batch automatisch in die Warteschlange eingereiht und für die Ausführung auf Computeknoten im zugeordneten Pool eingeplant. Basierend auf den Einstellungen, die Sie angeben, führt Batch das Einreihen, Planen und erneute Ausführen sowie andere Schritte der Aufgabenverwaltung aus. 

Es gibt viele Ansätze für die Überwachung der Aufgabenausführung. In der Funktion `wait_for_tasks_to_complete` dieses Beispiels wird das [TaskState](/python/api/azure.batch.models.taskstate)-Objekt zum Überwachen von Aufgaben für einen bestimmten Status innerhalb eines Zeitlimits verwendet. In diesem Fall ist dies der Status „Abgeschlossen“.

```python
while datetime.datetime.now() < timeout_expiration:
    print('.', end='')
    sys.stdout.flush()
    tasks = batch_service_client.task.list(job_id)

    incomplete_tasks = [task for task in tasks if
                        task.state != batchmodels.TaskState.completed]
    if not incomplete_tasks:
        print()
        return True
    else:
        time.sleep(1)
...
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach dem Ausführen der Aufgaben löscht die App den erstellten Eingabespeichercontainer automatisch und ermöglicht Ihnen das Löschen des Batch-Pools und -Auftrags. Die Klassen [JobOperations](/python/api/azure.batch.operations.joboperations) und [PoolOperations](/python/api/azure.batch.operations.pooloperations) von BatchClient verfügen jeweils über delete-Methoden, die aufgerufen werden, wenn Sie das Löschen bestätigen. Für die Aufträge und Aufgaben fallen zwar keine Kosten an, für Computeknoten dagegen schon. Daher empfehlen wir Ihnen, Pools nur bei Bedarf zuzuordnen. Beim Löschen des Pools werden alle Aufgabenausgaben auf den Knoten gelöscht. Die Eingabe- und Ausgabedateien verbleiben aber im Speicherkonto.

Löschen Sie die Ressourcengruppe, das Batch-Konto und das Speicherkonto, wenn diese Elemente nicht mehr benötigt werden. Wählen Sie hierzu im Azure-Portal die Ressourcengruppe für das Batch-Konto aus, und klicken Sie auf **Ressourcengruppe löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Authentifizieren mit Batch- und Storage-Konten
> * Hochladen von Eingabedateien in Storage
> * Erstellen eines Pools mit Computeknoten für die Ausführung einer Anwendung
> * Erstellen eines Auftrags und von Aufgaben zum Verarbeiten von Eingabedateien
> * Überwachen der Aufgabenausführung
> * Abrufen von Ausgabedateien

Weitere Beispiele zur Verwendung der Python-API zum Planen und Verarbeiten von Batch-Workloads finden Sie in den Beispielen auf GitHub.

> [!div class="nextstepaction"]
> [Python-Beispiele für Batch](https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch)

