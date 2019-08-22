---
title: 'Schnellstart: Verwenden des Java Storage SDKs zum Erstellen eines Blobs'
description: In dieser Schnellstartanleitung verwenden Sie das Java Storage-SDK, um Container in einem Objektspeicher (Azure Blob) zu erstellen, Dateien hochzuladen, Objekte aufzulisten und Dateien herunterzuladen.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2018
ms.service: storage
ms.topic: quickstart
ms.custom: seo-java-july2019
ms.openlocfilehash: 633054d0e06a78124b8fdcefface665b2bf8c25d
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900441"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-the-java-storage-sdk-v10"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit dem Java Storage SDK v10

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Java Storage-SDK Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. Beim neuen Java-SDK wird das reaktive Programmierungsmodell mit RxJava zur Bereitstellung von asynchronen Vorgängen verwendet. Lesen Sie mehr über die [reaktiven Erweiterungen für den virtuellen Java-Computer](https://github.com/ReactiveX/RxJava) von RxJava. 

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Stellen Sie sicher, dass die folgenden zusätzlichen Komponenten installiert sind:

* [Maven](https://maven.apache.org/download.cgi) für die Verwendung über die Befehlszeile oder eine beliebige integrierte Entwicklungsumgebung für Java
* [JDK](https://aka.ms/azure-jdks)

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) ist eine einfache Konsolenanwendung. 

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont.

Nachdem der Import durch das Projekt abgeschlossen ist, öffnen Sie **Quickstart.java** (im Verzeichnis **src/main/java/quickstart**).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
Für diese Lösung müssen der Name und Schlüssel Ihres Speicherkontos sicher gespeichert werden. Speichern Sie sie in lokalen Umgebungsvariablen des Computers, auf dem das Beispiel ausgeführt wird. Befolgen Sie je nach Betriebssystem die Schritte für das Linux- oder das Windows-Beispiel, um die Umgebungsvariablen zu erstellen.

### <a name="linux-example"></a>Linux-Beispiel

```bash
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows-Beispiel

```CMD
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

In diesem Beispiel wird eine Testdatei im Standardverzeichnis (**AppData\Local\Temp**) für Windows-Benutzer erstellt. Anschließend werden Sie aufgefordert, die folgenden Schritte auszuführen:

1. Geben Sie Befehle zum Hochladen der Testdatei in Azure Blob Storage ein.
2. Listen Sie die Blobs im Container auf.
3. Laden Sie die hochgeladene Datei unter einem neuen Namen herunter, damit Sie die alte und die neue Datei vergleichen können. 

Wenn Sie das Beispiel mithilfe von Maven über die Befehlszeile ausführen möchten, öffnen Sie eine Shell, und navigieren Sie zu **storage-blobs-java-v10-quickstart** innerhalb Ihres geklonten Verzeichnisses. Geben Sie dann `mvn compile exec:java` ein.

In diesem Beispiel ist die Ausgabe gezeigt, wenn Sie die Anwendung unter Windows ausführen.

```Output
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Da Sie die Kontrolle über das Beispiel haben, geben Sie Befehle ein, damit der Code ausgeführt wird. Bei Eingaben muss die Groß-/Kleinschreibung beachtet werden.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Überprüfen Sie die Dateien. Drücken Sie anschließend **E** und die **EINGABETASTE**, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen mit dem Beispiel ausgeführt werden, öffnen Sie die Datei **Quickstart.java**, um den Code zu betrachten. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

In den nächsten Abschnitten gehen wir schrittweise durch den Beispielcode, damit Sie nachvollziehen können, wie er funktioniert.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte

Zunächst erstellen Sie die Verweise auf die Objekte, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf. Jedes wird vom jeweils nächsten Objekt in der Liste verwendet.

1. Erstellen Sie eine Instanz des Objekts **StorageURL**, das auf das Speicherkonto verweist.

    * Das Objekt [StorageURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-stable) ist eine Darstellung Ihres Speicherkontos. Sie generieren damit eine neue Pipeline. 
    * Bei einer Pipeline handelt es sich um eine Reihe von Richtlinien, die zum Bearbeiten von Anforderungen und Antworten mit Autorisierungs-, Protokollierungs- und Wiederholungsmechanismen verwendet werden. Weitere Informationen finden Sie unter [Azure Storage Java V10 Overview](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline) (Übersicht über Azure Storage Java v10).  
    * Erstellen Sie mithilfe der Pipeline eine Instanz des [ServiceURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-stable)-Objekts.
    * Erstellen Sie mithilfe des **ServiceURL**-Objekts eine Instanz von [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable).
    * Das **ContainerURL**-Element ist zum Ausführen von Vorgängen für Blobcontainer erforderlich.

2. Erstellen Sie eine Instanz des **ContainerURL**-Objekts, das den Container darstellt, auf den Sie zugreifen. Container organisieren Ihre Blobs, so wie auf Ihrem Computer Ordner zum Organisieren von Dateien verwendet werden.

    * Das **ContainerURL**-Objekt stellt einen Zugriffspunkt für den Containerdienst bereit. 
    * Erstellen Sie mithilfe des [ServiceURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-stable)-Objekts eine Instanz des [BlobURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable)-Objekts.
    * **BlobURL** ist zum Erstellen von Blobs erforderlich.

3. Erstellen Sie eine Instanz des **BlobURL**-Objekts, das auf das Blob verweist, das für Sie von Interesse ist. 

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

### <a name="create-a-container"></a>Erstellen eines Containers 

In diesem Abschnitt erstellen Sie eine Instanz des **ContainerURL**-Objekts. Dabei erstellen Sie auch einen neuen Container. Der Name des Containers in diesem Beispiel lautet **quickstart**. 

In diesem Beispiel wird [ContainerURL.create](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-stable#com_microsoft_azure_storage_blob__container_u_r_l_create_Metadata_PublicAccessType_Context_) verwendet, damit bei jedem Ausführen des Beispiels ein neuer Container erstellt werden kann. Sie können den Container auch im Voraus erstellen, damit er nicht im Code erstellt werden muss.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten verwendet. Sie werden in diesem Schnellstart verwendet. 

1. Um eine Datei in ein Blob hochzuladen, rufen Sie einen Verweis auf das Blob im Zielcontainer ab. 
2. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe einer der folgenden APIs eine Datei in das Blob hochladen:

   * Low-Level-APIs. Beispiele umfassen [BlockBlobURL.upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-stable#com_microsoft_azure_storage_blob__block_blob_u_r_l_upload_Flowable_ByteBuffer__long_BlobHTTPHeaders_Metadata_BlobAccessConditions_Context_) (auch als „PutBlob“ bezeichnet) und [BlockBlobURL.stageBlock](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-stable) (auch als „PutBLock“ bezeichnet) in der Instanz von **BlockBlobURL**. 

   * In der [TransferManager-Klasse](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-stable) bereitgestellte High-Level-APIs. Ein Beispiel ist die [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-stable)-Methode. 

     Dabei wird das Blob erstellt, falls es noch nicht vorhanden ist. Ist es bereits vorhanden, wird es überschrieben.

Der Beispielcode erstellt eine lokale Datei zur Verwendung für den Upload und Download. Die hochzuladende Datei wird als **sourceFile** und die URL des Blobs in **blob** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstart** hochgeladen.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {

    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());

    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Blockblobs können jede Art von Text oder Binärdateien darstellen. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs werden zum Anfügen von Daten an das Ende und häufig zum Protokollieren verwendet. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit [ContainerURL.listBlobsFlatSegment](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-stable) können Sie eine Liste mit Objekten in einem Container abrufen. Mit dieser Methode werden bis zu 5.000 Objekte auf einmal sowie eine Fortsetzungsmarkierung (next-Markierung) zurückgegeben, falls weitere Objekte im Container enthalten sind. Erstellen Sie eine Hilfsfunktion, die sich selbst wiederholt aufruft, wenn in der vorherigen **listBlobsFlatSegment**-Antwort eine next-Markierung vorhanden ist.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs,
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }

    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */

        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */

        return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
            .flatMap(containersListBlobFlatSegmentResponse ->
                listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie mithilfe von [BlobURL.download](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-stable) Blobs auf Ihren lokalen Datenträger herunter.

Durch den folgenden Code wird der in einem vorherigen Abschnitt hochgeladene Blob heruntergeladen. **_DOWNLOADED** wird als Suffix an den Blobnamen angefügt, damit Sie beide Dateien auf dem lokalen Datenträger sehen können. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit [ContainerURL.delete](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-stable) den gesamten Container löschen. Mit dieser Methode werden auch die Dateien im Container gelöscht.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie mit Java Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. 

> [!div class="nextstepaction"]
> [Storage SDK v10 für Java-Quellcode](https://github.com/Azure/azure-storage-java/)
> [API-Referenz](https://docs.microsoft.com/java/api/overview/azure/storage)
> [Weitere Informationen zu RxJava](https://github.com/ReactiveX/RxJava)
