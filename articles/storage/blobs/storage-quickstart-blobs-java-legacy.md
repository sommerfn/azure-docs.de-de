---
title: 'Schnellstart: Azure Blob Storage-Clientbibliothek v8 für Java'
description: Erstellen Sie ein Speicherkonto und einen Container im Objektspeicher (Blob). Anschließend verwenden Sie die Azure Storage-Clientbibliothek v8 für Java, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 10/05/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: a6c4380e44d705e551bc96746a809c57aa02ac5b
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825369"
---
# <a name="quickstart-azure-blob-storage-client-library-v8-for-java"></a>Schnellstart: Azure Blob Storage-Clientbibliothek v8 für Java

Erfahren Sie etwas über die ersten Schritte mit der Azure Blob Storage-Clientbibliothek v8 für Java. Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Führen Sie die Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Blobspeicher ist für die Speicherung großer Mengen unstrukturierter Daten optimiert.

Mit der Azure Blob Storage-Clientbibliothek für Java ist Folgendes möglich:

* Erstellen eines Containers
* Festlegen von Berechtigungen für einen Container
* Erstellen eines Blobs in Azure Storage
* Herunterladen des Blobs auf den lokalen Computer
* Auflisten aller Blobs in einem Container
* Löschen eines Containers

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Azure Storage-Konto – [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Eine IDE mit Maven-Integration.
* Alternativ können Sie Maven installieren und so konfigurieren, dass es über die Befehlszeile verwendet werden kann.

In dieser Anleitung wird [Eclipse](https://www.eclipse.org/downloads/) mit der Konfiguration „Eclipse-IDE für Java-Entwickler“ verwendet.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Die [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-java-quickstart) ist eine einfache Konsolenanwendung.

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Um das Projekt zu öffnen, starten Sie Eclipse, und schließen Sie die Willkommensseite. Wählen Sie **File** (Datei) und anschließend **Open Projects from File System** (Projekte aus Dateisystem öffnen) aus. Stellen Sie sicher, dass **Detect and configure project natures** (Projektarten erkennen und konfigurieren) aktiviert ist. Wählen Sie **Directory** (Verzeichnis) aus, und navigieren Sie zum Speicherort des geklonten Repositorys. Wählen Sie innerhalb des geklonten Repositorys den Ordner **blobAzureApp** aus. Vergewissern Sie sich, dass das Projekt **blobAzureApp** als Eclipse-Projekt angezeigt wird, und wählen Sie anschließend **Finish** (Fertig stellen) aus.

Nachdem das Importieren des Projekts abgeschlossen ist, öffnen Sie **AzureApp.java** (unter **blobQuickstart.blobAzureApp** in **src/main/java**), und ersetzen Sie `accountname` und `accountkey` in der `storageConnectionString`-Zeichenfolge. Führen Sie die Anwendung dann aus. Spezifische Anweisungen für diese Aufgaben finden Sie in den folgenden Abschnitten.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Sie müssen in der Anwendung die Verbindungszeichenfolge für das Speicherkonto angeben. Öffnen Sie die Datei **AzureApp.Java**. Suchen Sie die Variable `storageConnectionString`, und fügen Sie den Wert der Verbindungszeichenfolge ein, den Sie im vorherigen Abschnitt kopiert haben. Die Variable `storageConnectionString` sollte in etwa wie im folgenden Codebeispiel aussehen:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Ausführen des Beispiels

Diese Beispielanwendung erstellt eine Testdatei in Ihrem Standardverzeichnis (*C:\Benutzer\<Benutzer>\AppData\Local\Temp* für Windows-Benutzer), lädt diese in Blob Storage hoch, listet die Blobs im Container auf und lädt die Datei dann mit einem neuen Namen herunter, sodass Sie die alte und die neue Datei vergleichen können.

Führen Sie das Beispiel mithilfe von Maven über die Befehlszeile aus. Öffnen Sie eine Shell, und navigieren Sie in Ihrem geklonten Verzeichnis zu **blobAzureApp**. Geben Sie dann `mvn compile exec:java` ein.

Das folgende Beispiel zeigt die Ausgabe für Benutzer, die die Anwendung unter Windows ausführen.

```
Azure Blob storage quick start sample
Creating container: quickstartcontainer
Creating a sample file at: C:\Users\<user>\AppData\Local\Temp\sampleFile514658495642546986.txt
Uploading the sample file
URI of blob is: https://myexamplesacct.blob.core.windows.net/quickstartcontainer/sampleFile514658495642546986.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.

Deleting the container
Deleting the source, and downloaded files
```

Überprüfen Sie, ob die Beispieldatei in Ihrem Standardverzeichnis (*C:\Benutzer\<Benutzer>\AppData\Local\Temp* für Windows-Benutzer) vorhanden ist, bevor Sie fortfahren. Kopieren Sie die URL für das Blob aus dem Konsolenfenster, und fügen Sie sie in einem Browser ein, um den Inhalt der Datei in Blob Storage anzuzeigen. Wenn Sie die Beispieldatei in Ihrem Verzeichnis mit dem in Blob Storage gespeicherten Inhalt vergleichen, sehen Sie, dass sie identisch sind.

  >[!NOTE]
  >Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht.

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie die**EINGABETASTE**, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei **AzureApp.java**, um den Code zu betrachten.

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Als Nächstes gehen wir schrittweise durch den Beispielcode, damit Sie verstehen können, wie er funktioniert.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte

Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf – jedes wird vom jeweils nächsten in der Liste verwendet.

* Erstellen Sie eine Instanz des [CloudStorageAccount](/java/api/com.microsoft.azure.management.storage.storageaccount)-Objekts, das auf das Speicherkonto verweist.

    Das **CloudStorageAccount**-Objekt ist eine Darstellung Ihres Speicherobjekts, und es ermöglicht den programmgesteuerten Zugriff und das programmgesteuerte Festlegen von Eigenschaften des Speicherkontos. Mithilfe des **CloudStorageAccount**-Objekts können Sie eine Instanz von **CloudBlobClient** erstellen, was für den Zugriff auf den Blob-Dienst erforderlich ist.

* Erstellen Sie eine Instanz des **CloudBlobClient**-Objekts, das auf den [Blob-Dienst](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) im Speicherkonto verweist.

    Der Cloudblobclient (**CloudBlobClient**) stellt einen Zugriffspunkt für den Blob-Dienst zur Verfügung, mit dem Sie programmgesteuert auf Blob-Speichereigenschaften zugreifen und sie festlegen können. Mithilfe von **CloudBlobClient** können Sie eine Instanz des **CloudBlobClient**-Objekts erstellen, was zum Erstellen von Containern erforderlich ist.

* Erstellen Sie eine Instanz des [CloudBlobContainer](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container)-Objekts, das den Container darstellt, auf den Sie zugreifen. Container dienen dazu, Ihre Blobs zu strukturieren – genau wie Ordner, die Sie auf Ihrem Computer zum Strukturieren Ihrer Dateien verwenden.

    Wenn Sie über den Cloudblobcontainer (**CloudBlobContainer**) verfügen, können Sie eine Instanz des Objekts [CloudBlockBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) erstellen, das auf das gewünschte Blob verweist, und beispielsweise einen Upload-, Download- oder Kopiervorgang ausführen.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Containern finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

### <a name="create-a-container"></a>Erstellen eines Containers

In diesem Abschnitt erstellen Sie eine Instanz der Objekte und einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich sind und einfach mit einer URL auf sie zugegriffen werden kann. Der Name des Containers lautet **quickstartcontainer**.

In diesem Beispiel wird [CreateIfNotExists](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.createifnotexists) verwendet, da bei jedem Ausführen des Beispiels ein neuer Container erstellt werden soll. In einer Produktionsumgebung, in der Sie in der gesamten Anwendung den gleichen Container verwenden, empfiehlt es sich hingegen, **CreateIfNotExists** nur einmal aufzurufen. Alternativ können Sie den Container im Voraus erstellen, damit er nicht im Code erstellt werden muss.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Um eine Datei in ein Blockblob hochzuladen, rufen Sie einen Verweis auf das Blob im Zielcontainer ab. Nachdem Sie den Blobverweis abgerufen haben, können Sie mithilfe von [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload) Daten in das Blob hochladen. Dieser Vorgang erstellt das Blob, sofern noch keines vorhanden ist, oder überschreibt das vorhandene Blob.

Der Beispielcode erstellt eine lokale Datei für den Upload und Download. Dabei wird die hochzuladende Datei als **Quelle** und der Name des Blob in **Blob** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartcontainer** hochgeladen.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Es gibt mehrere `upload`-Methoden (unter anderem [upload](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload), [uploadBlock](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadblock), [uploadFullBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadfullblob), [uploadStandardBlobTier](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadstandardblobtier) und [uploadText](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadtext)), die Sie mit Blob Storage verwenden können. Für eine Zeichenfolge können Sie beispielsweise anstelle der `Upload`-Methode die `UploadText`-Methode verwenden.

Blockblobs können jede Art von Text oder Binärdateien darstellen. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen zur Protokollierung und werden beispielsweise verwendet, wenn Sie etwas in eine Datei schreiben und ihr nach und nach weitere Informationen hinzufügen möchten. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Mit [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs) können Sie eine Liste von Dateien im Container abrufen. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die URIs der gefundenen Blobs anzuzeigen. Sie können den URI aus dem Befehlsfenster kopieren und in einen Browser einfügen, um die Datei anzuzeigen.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie mit [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile) Blobs auf den lokalen Datenträger herunter.

Der folgende Code lädt das im vorherigen Abschnitt hochgeladene Blob herunter und fügt dem Blobnamen das Suffix „_DOWNLOADED“ hinzu, damit auf dem lokalen Datenträger beide Dateien angezeigt werden.

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and
// haven't changed the blob we're interested in, so we can reuse it.
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die hochgeladenen Blobs nicht mehr benötigen, können Sie mit [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists) den gesamten Container löschen. Mit dieser Methode werden auch die Dateien im Container gelöscht.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();

if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Sie mit Java Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Java finden Sie in unserem Quellcoderepository auf GitHub.

> [!div class="nextstepaction"]
> [Java-API-Referenz](https://docs.microsoft.com/java/api/overview/azure/storage?view=azure-java-legacy)
> [Codebeispiele für Java](../common/storage-samples-java.md)
