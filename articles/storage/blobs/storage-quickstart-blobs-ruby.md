---
title: 'Azure-Schnellstart: Erstellen eines Blobs im Objektspeicher mithilfe von Ruby | Microsoft-Dokumentation'
description: In diesem Schnellstart erstellen Sie ein Speicherkonto und einen Container im Objektspeicher (Blob). Anschließend verwenden Sie die Speicherclientbibliothek für Ruby, um ein Blob in Azure Storage hochzuladen, ein Blob herunterzuladen und die Blobs in einem Container aufzulisten.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/14/2018
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 8c24c5f043d17b5f0e54ca1c2c6cf41a0d3fe9bc
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726348"
---
# <a name="quickstart-upload-download-and-list-blobs-using-ruby"></a>Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit Ruby

In diesem Schnellstart erfahren Sie, wie Sie mit Ruby Blockblobs in einem Container in Azure Blob Storage hochladen, herunterladen und auflisten. 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Stellen Sie sicher, dass die folgenden zusätzlichen Komponenten installiert sind:

* [Ruby](https://www.ruby-lang.org/en/downloads/)
* [Azure Storage-Bibliothek für Ruby](https://docs.microsoft.com/azure/storage/blobs/storage-ruby-how-to-use-blob-storage) mit dem rubygem-Paket: 

    ```
    gem install azure-storage-blob
    ```
    
## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung
Die in diesem Schnellstart verwendete [Beispielanwendung](https://github.com/Azure-Samples/storage-blobs-ruby-quickstart.git) ist eine einfache Ruby-Anwendung.  

Verwenden Sie [Git](https://git-scm.com/), um eine Kopie der Anwendung in Ihre Entwicklungsumgebung herunterzuladen. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-ruby-quickstart.git 
```

Mit diesem Befehl wird das Repository in Ihren lokalen Git-Ordner geklont. Suchen Sie zum Öffnen der Ruby-Beispielanwendung nach dem Ordner „storage-blobs-ruby-quickstart“, und öffnen Sie die Datei „example.rb“.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge
In der Anwendung müssen Sie Ihren Speicherkontonamen sowie den Kontoschlüssel angeben, um die `BlobService`-Instanz für Ihre Anwendung zu erstellen. Öffnen Sie im Lösungsmappen-Explorer in Ihrer IDE die Datei `example.rb`. Ersetzen Sie die Werte **accountname** und **accountkey** mit Ihrem Kontonamen und dem Kontoschlüssel. 

```ruby 
blob_client = Azure::Storage::Blob::BlobService.create(
            storage_account_name: account_name,
            storage_access_key: account_key
          )
```

## <a name="run-the-sample"></a>Ausführen des Beispiels
Dieses Beispiel erstellt eine Testdatei im Ordner „Dokumente“. Das Beispielprogramm lädt die Testdatei in den Blobspeicher hoch, listet die Blobs im Container auf und lädt die Datei mit einem neuen Namen herunter. 

Führen Sie das Beispiel aus. Die folgende Ausgabe ist ein Beispiel der Ausgabe, die zurückgegeben wird, wenn die Anwendung ausgeführt wird.
  
```
Creating a container: quickstartblobs7b278be3-a0dd-438b-b9cc-473401f0c0e8

Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Wenn Sie eine beliebige Taste drücken, um den Vorgang fortzusetzen, löscht das Beispielprogramm den Speichercontainer und die Dateien. Überprüfen Sie Ihren Ordner „Dokumente“ nach den zwei Dateien, bevor Sie fortfahren. Sie können diese öffnen und prüfen, ob sie identisch sind.

Sie können zum Anzeigen der Dateien in Blob Storage auch ein Tool, z.B. den [Azure Storage-Explorer](https://storageexplorer.com), verwenden. Der Azure Storage-Explorer ist ein kostenloses plattformübergreifendes Tool, das Ihnen den Zugriff auf die Speicherkontoinformationen ermöglicht. 

Nachdem Sie die Dateien erfolgreich überprüft haben, drücken Sie eine beliebige Taste, um die Demo zu beenden und die Testdateien zu löschen. Da Sie jetzt wissen, welche Aktionen im Beispiel durchgeführt werden, können Sie die Datei „example.rb“ öffnen, um sich den Code anzusehen. 

## <a name="understand-the-sample-code"></a>Grundlagen des Beispielcodes

Als Nächstes gehen wir schrittweise durch den Beispielcode, damit Sie verstehen können, wie er funktioniert.

### <a name="get-references-to-the-storage-objects"></a>Abrufen von Verweisen auf die Speicherobjekte
Zunächst müssen die Verweise auf die Objekte erstellt werden, die zum Zugreifen auf und Verwalten von Blob Storage verwendet werden. Diese Objekte bauen aufeinander auf und jedes wird vom jeweils nächsten in der Liste verwendet.

* Erstellen Sie eine Instanz des **BlobService**-Elements von Azure Storage, um die Anmeldeinformationen für die Verbindung einzurichten. 
* Erstellen Sie das **Container**-Objekt, das den Container darstellt, auf den Sie zugreifen. Container werden zum Organisieren der Blobs verwendet, so wie Sie auf Ihrem Computer Ordner zum Organisieren von Dateien verwenden.

Sobald Sie über den Cloud-Blobcontainer verfügen, können Sie das **Block**-Blobobjekt erstellen, das auf den gewünschten Blob verweist, und Upload-, Download- oder Kopiervorgänge durchführen.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zu Container- und Blobnamen finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

In diesem Abschnitt richten Sie eine Instanz des Azure Storage-Clients ein, instanziieren das Blob-Dienstobjekt, erstellen einen neuen Container und legen dann Berechtigungen für den Container fest, damit die Blobs öffentlich gemacht werden. Der Name des Containers lautet **quickstartblobs**. 

```ruby 
# Create a BlobService object
blob_client = Azure::Storage::Blob::BlobService.create(
    storage_account_name: account_name,
    storage_access_key: account_key
    )

# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs' + SecureRandom.uuid
container = blob_client.create_container(container_name)   

# Set the permission so the blobs are public.
blob_client.set_container_acl(container_name, "container")
```

### <a name="upload-blobs-to-the-container"></a>Hochladen von Blobs in den Container

Blobspeicher unterstützt Block-, Anfüge- und Seitenblobs. Blockblobs werden am häufigsten und auch in diesem Schnellstart verwendet.  

Um eine Datei in einen Blob hochzuladen, rufen Sie den vollständigen Pfad der Datei ab, indem Sie den Verzeichnisnamen und den Dateinamen auf Ihrem lokalen Laufwerk verknüpfen. Sie können anschließend die Datei an den Speicherort unter dem angegebenen Pfad hochladen, indem Sie die **create\_block\_blob()** -Methode verwenden. 

Der Beispielcode erstellt eine lokale Datei für den Upload und Download. Dabei wird die hochzuladende Datei als **file\_path\_to\_file** und der Name des Blob in **local\_file\_name** gespeichert. Im folgenden Beispiel wird die Datei in einen Container mit dem Namen **quickstartblobs** hochgeladen.

```ruby
# Create a file in Documents to test the upload and download.
local_path=File.expand_path("~/Documents")
local_file_name ="QuickStart_" + SecureRandom.uuid + ".txt"
full_path_to_file =File.join(local_path, local_file_name)

# Write text to the file.
file = File.open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

puts "Temp file = " + full_path_to_file
puts "\nUploading to Blob storage as blob" + local_file_name

# Upload the created file, using local_file_name for the blob name
blob_client.create_block_blob(container.name, local_file_name, full_path_to_file)
```

Verwenden Sie die **create\_block\_list()** -Methode, um eine Teilaktualisierung für den Inhalt eines Blockblogs durchzuführen. Blockblobs können eine Größe von bis zu 4,7 TB haben und alle Arten von Dateien sein, von Excel-Arbeitsblättern bis zu großen Videodateien. Seitenblobs werden in erster Linie für die VHD-Dateien verwendet, die IaaS-VMs zugrunde liegen. Anfügeblobs dienen der Protokollierung und können z.B. verwendet werden, um beim Schreiben in eine Datei zusätzliche Daten hinzuzufügen. Das Anfügeblob sollte nur in einem Writer-Modell verwendet werden. Die meisten Objekte, die in Blob Storage gespeichert werden, sind allerdings Blockblobs.

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Sie können eine Liste mit Dateien des Containers abrufen, indem Sie die **list\_blobs()** -Methode verwenden. Der folgende Code ruft die Liste der Blobs ab und durchläuft sie, um die Namen der in einem Container gefundenen Blobs anzuzeigen.  

```ruby
# List the blobs in the container
nextMarker = nil
loop do
    blobs = blob_client.list_blobs(container_name, { marker: nextMarker })
    blobs.each do |blob|
        puts "\tBlob name #{blob.name}"
    end
    nextMarker = blobs.continuation_token
    break unless nextMarker && !nextMarker.empty?
end
```

### <a name="download-the-blobs"></a>Herunterladen der Blobs

Laden Sie Blobs auf Ihren lokalen Datenträger mithilfe der **get\_blob()** -Methode herunter. Durch den folgenden Code wird der in einem vorherigen Abschnitt hochgeladene Blob heruntergeladen. „_DOWNLOADED“ wird als Suffix an den Blobnamen hinzugefügt, damit Sie beide Dateien auf dem lokalen Datenträger sehen können. 

```ruby
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = File.join(local_path, local_file_name.gsub('.txt', '_DOWNLOADED.txt'))

puts "\n Downloading blob to " + full_path_to_file2
blob, content = blob_client.get_blob(container_name,local_file_name)
File.open(full_path_to_file2,"wb") {|f| f.write(content)}
```

### <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie die in diesem Schnellstart hochgeladenen Blobs nicht mehr benötigen, können Sie mit der **delete\_container()** -Methode den gesamten Container löschen. Wenn die erstellten Dateien nicht mehr benötigt werden, können Sie die **delete\_blob()** -Methode verwenden, um die Dateien zu löschen.

```ruby
# Clean up resources. This includes the container and the temp files
blob_client.delete_container(container_name)
File.delete(full_path_to_file)
File.delete(full_path_to_file2)    
```
## <a name="resources-for-developing-ruby-applications-with-blobs"></a>Ressourcen für die Entwicklung von Ruby-Anwendungen mit Blobs

Sehen Sie sich diese zusätzlichen Ressourcen zur Ruby-Entwicklung mit Blobspeicher an:

- Zeigen Sie den [Quellcode der Ruby-Clientbibliothek](https://github.com/Azure/azure-storage-ruby) für Azure Storage auf GitHub an, und laden Sie ihn herunter.
- Sehen Sie sich die [Beispiele für Blobspeicher](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=ruby&term=blob) an, die mit der Ruby-Clientbibliothek geschrieben wurden.

## <a name="next-steps"></a>Nächste Schritte
 
In diesem Schnellstart haben Sie gelernt, wie Sie mit Ruby Dateien zwischen einem lokalen Datenträger und Azure Blob Storage übertragen. Weitere Informationen zum Arbeiten mit Blob Storage finden Sie in der exemplarischen Vorgehensweise zu Blob Storage.

> [!div class="nextstepaction"]
> [Gewusst wie: Blob Storage-Vorgänge](./storage-ruby-how-to-use-blob-storage.md)


Weitere Informationen zum Storage-Explorer und Blobs finden Sie unter [Verwalten von Azure Blob Storage-Ressourcen mit dem Speicher-Explorer (Vorschau)](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
