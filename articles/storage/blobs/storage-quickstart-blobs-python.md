---
title: 'Azure-Schnellstart: Erstellen eines Blobs im Objektspeicher mit Python | Microsoft-Dokumentation'
description: In diesem Schnellstart erstellen Sie ein Speicherkonto und einen Container im Objektspeicher (Blob). Anschließend verwenden Sie die Speicherclientbibliothek für Python, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.openlocfilehash: 5fe011d740b1c08ae3b9cf4e3ea67d2cdd4fee66
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360101"
---
# <a name="quickstart-upload-download-and-list-blobs-with-python"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit Python

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit Python Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. Bei Blobs handelt es sich lediglich um Objekte, die eine beliebige Menge an Text oder binären Daten (z. B. Bilder, Dokumente, Streamingmedien, Archivdaten usw.) enthalten und die in Azure Storage getrennt von Dateifreigaben, schemalosen Tabellen und Nachrichtenwarteschlangen vorliegen. (Weitere Informationen finden Sie unter [Einführung in Azure Storage](/azure/storage/common/storage-introduction).)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Stellen Sie sicher, dass die folgenden zusätzlichen Komponenten installiert sind:

* [Python](https://www.python.org/downloads/)
* [Azure Storage SDK für Python](https://github.com/Azure/azure-sdk-for-python)

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung
In dieser Schnellstartanleitung wird als [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) eine einfache Python-Anwendung verwendet.  

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Dieser Befehl klont das Repository *Azure-Samples/storage-blobs-python-quickstart* in Ihrem lokalen Git-Ordner. Öffnen Sie zum Ausführen des Python-Programms die Datei *example.py* im Stammverzeichnis des Repositorys.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
Geben Sie in der Anwendung Ihren Speicherkontonamen sowie den Kontoschlüssel an, um ein Objekt vom Typ `BlockBlobService` zu erstellen. Öffnen Sie im Projektmappen-Explorer in Ihrer IDE die Datei *example.py*. Ersetzen Sie die Werte `accountname` und `accountkey` durch Ihren Kontonamen und Schlüssel. 

```python
block_blob_service = BlockBlobService(
    account_name='accountname', account_key='accountkey')
```

## <a name="run-the-sample"></a>Ausführen des Beispiels
Dieses Beispiel erstellt eine Testdatei im Ordner *Documents*. Das Beispielprogramm lädt die Testdatei in den Blobspeicher hoch, listet die Blobs im Container auf und lädt die Datei mit einem neuen Namen herunter. 

Installieren Sie zuerst die Abhängigkeiten, indem Sie `pip install` ausführen:

```python
    pip install azure-storage-blob
```

Führen Sie als Nächstes das Beispiel aus. Daraufhin werden Nachrichten angezeigt, die in etwa wie in der folgenden Ausgabe aussehen:
  
```output
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Suchen Sie im Ordner *Documents* nach den beiden Dateien, bevor Sie fortfahren. Wenn Sie diese Dateien öffnen, sehen Sie, dass sie identisch sind.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](https://storageexplorer.com), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Drücken Sie nach erfolgreicher Überprüfung der Dateien eine beliebige Taste, um die Demo zu beenden und die Testdateien zu löschen. Nachdem Sie nun wissen, welche Aktionen das Beispiel ausführt, öffnen Sie die Datei *example.py*, und sehen Sie sich den Code an. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

In diesem Abschnitt gehen wir den Beispielcode Schritt für Schritt durch, damit Sie dessen Funktionsweise nachvollziehen können.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte
Als Erstes erstellen Sie die Verweise auf die Objekte, die zum Zugreifen auf und Verwalten von Blobspeicher verwendet werden. Diese Objekte bauen aufeinander auf und jedes wird vom jeweils nächsten in der Liste verwendet.

* Instanziieren Sie das **BlockBlobService**-Objekt, das auf den Blob-Dienst im Speicherkonto verweist. 

* Instanziieren Sie das **CloudBlobContainer**-Objekt, das den Container darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.

Wenn Sie über den Cloudblobcontainer verfügen, instanziieren Sie das **CloudBlockBlob**-Objekt, das auf das spezielle Blob verweist, das für Sie relevant ist. Anschließend können Sie das Blob nach Bedarf hochladen, herunterladen und kopieren.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

In diesem Abschnitt instanziieren Sie die Objekte, erstellen einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich sind. Der Name des Containers lautet **quickstartblobs**. 

```python
# Create the BlockBlockService that is used to call the Blob service for the storage account.
block_blob_service = BlockBlobService(
    account_name='accountname', account_key='accountkey')

# Create a container called 'quickstartblobs'.
container_name = 'quickstartblobs'
block_blob_service.create_container(container_name)

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(
    container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blob Storage unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten verwendet und auch in diesem Schnellstart verwendet.  

Um eine Datei in ein Blob hochzuladen, ermitteln Sie den vollständigen Dateipfad, indem Sie den Verzeichnisnamen mit dem Dateinamen auf Ihrem lokalen Laufwerk verknüpfen. Anschließend können Sie die Datei mithilfe der Methode `create_blob_from_path` in den angegebenen Pfad hochladen. 

Der Beispielcode erstellt eine lokale Datei für den Upload und Download. Dabei wird die hochzuladende Datei als *Vollständiger_Pfad_zur_Datei* und der Name des Blobs als *Name_der_lokalen_Datei* hochgeladen. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs** hochgeladen.

```python
# Create a file in Documents to test the upload and download.
local_path = os.path.expanduser("~\Documents")
local_file_name = "QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file = os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file, 'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name.
block_blob_service.create_blob_from_path(
    container_name, local_file_name, full_path_to_file)
```

Sie können für Blob Storage mehrere Uploadmethoden verwenden. Wenn Sie also beispielsweise über einen Speicherstream verfügen, können Sie anstelle von `create_blob_from_path` die Methode `create_blob_from_stream` verwenden. 

Blockblobs können eine Größe von bis zu 4,7 TB haben und alle Arten von Dateien sein, von Excel-Arbeitsblättern bis zu großen Videodateien. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Rufen Sie mithilfe der Methode `list_blobs` eine Liste mit den Dateien im Container ab. Diese Methode gibt einen Generator zurück. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die Namen der in einem Container gefundenen Blobs anzuzeigen.  

```python
# List the blobs in the container.
print("\nList blobs in the container")
generator = block_blob_service.list_blobs(container_name)
for blob in generator:
    print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>Herunterladen der Blobs

Laden Sie Blobs mithilfe der Methode `get_blob_to_path` auf Ihren lokalen Datenträger herunter. Durch den folgenden Code wird der in einem vorherigen Abschnitt hochgeladene Blob heruntergeladen. *_DOWNLOADED* wird als Suffix an den Blobnamen angefügt, sodass Sie beide Dateien auf dem lokalen Datenträger sehen können. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(
    local_file_name, '.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(
    container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die in dieser Schnellstartanleitung hochgeladenen Blobs nicht mehr benötigen, können Sie mithilfe der Methode `delete_container` den gesamten Container löschen. Falls Sie stattdessen einzelne Dateien löschen möchten, verwenden Sie die Methode `delete_blob`.

```python
# Clean up resources. This includes the container and the temp files.
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```
## <a name="resources-for-developing-python-applications-with-blobs"></a>Ressourcen für die Entwicklung von Python-Anwendungen mit Blobs

Weitere Informationen zur Python-Entwicklung mit Blobspeicher finden Sie in den folgenden Ressourcen:

### <a name="binaries-and-source-code"></a>Binärdateien und Quellcode

- Zeigen Sie den [Quellcode der Python-Clientbibliothek](https://github.com/Azure/azure-storage-python) für Azure Storage auf GitHub an, laden Sie ihn herunter, und installieren Sie ihn.

### <a name="client-library-reference-and-samples"></a>Clientbibliothek – Referenz und Beispiele

- Weitere Informationen zur Python-Clientbibliothek finden Sie in der [Python-API-Referenz](https://docs.microsoft.com/python/api/overview/azure/storage).
- Sehen Sie sich die [Beispiele für Blobspeicher](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=python&term=blob) an, die mit der Python-Clientbibliothek geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte
 
In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mit Python Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](./storage-python-how-to-use-blob-storage.md)
 
Weitere Informationen zum Storage-Explorer und Blobs finden Sie unter [Verwalten von Azure Blob Storage-Ressourcen mit dem Speicher-Explorer (Vorschau)](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
