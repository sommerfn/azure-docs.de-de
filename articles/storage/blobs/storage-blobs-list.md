---
title: Auflisten von Blobs mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie Blobs in einem Container in Ihrem Azure Storage-Konto mithilfe der .NET-Clientbibliothek auflisten.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/04/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: bf9d2d59e993de3807a10a6c39f88b2063024bfc
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72599800"
---
# <a name="list-blobs-with-net"></a>Auflisten von Blobs mit .NET

Wenn Sie Blobs über Ihren Code auflisten, können Sie eine Reihe von Optionen angeben, um zu steuern, wie Ergebnisse von Azure Storage zurückgegeben werden. In diesem Artikel wird beschrieben, wie Blobs mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) aufgelistet werden.  

## <a name="understand-blob-listing-options"></a>Grundlegendes zu den Optionen für das Auflisten von Blobs

Rufen Sie zum Auflisten der Blobs in einem Speicherkonto eine der folgenden Methoden auf:

- [CloudBlobClient.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobs)
- [CloudBlobClient.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmented)
- [CloudBlobClient.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listblobssegmentedasync)

Rufen Sie zum Auflisten der Blobs in einem Container eine der folgenden Methoden auf:

- [CloudBlobContainer.ListBlobs](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs)
- [CloudBlobContainer.ListBlobsSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmented)
- [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)

Die Überladungen dieser Methoden bieten zusätzliche Optionen zum Steuern, wie Blobs durch den Auflistungsvorgang zurückgegeben werden. Diese Optionen sind in den folgenden Abschnitten beschrieben.

### <a name="manage-how-many-results-are-returned"></a>Festlegen der Anzahl der zurückgegebenen Ergebnisse

Standardmäßig werden durch einen einzelnen Auflistungsvorgang bis zu 5.000 Ergebnisse zurückgegeben. Wenn ein kleinerer Ergebnissatz zurückgegeben werden soll, geben Sie beim Aufrufen einer der **ListBlobs**-Methoden einen Wert ungleich NULL für den Parameter `maxresults` an.

Wenn ein Auflistungsvorgang mehr als 5.000 Blobs zurückgibt oder wenn Sie einen Wert für `maxresults` angegeben haben, aufgrund dessen dieser Vorgang eine Teilmenge der Container im Speicherkonto zurückgibt, gibt Azure Storage ein *Fortsetzungstoken* mit der Liste der Blobs zurück. Ein Fortsetzungstoken ist ein nicht transparenter Wert, den Sie verwenden können, um den nächsten Satz von Ergebnissen aus Azure Storage abzurufen.

Überprüfen Sie im Code den Wert des Fortsetzungstokens, um zu bestimmen, ob er NULL ist. Wenn das Fortsetzungstoken NULL ist, ist der Satz der Ergebnisse vollständig. Wenn das Fortsetzungstoken nicht NULL ist, rufen Sie den Auflistungsvorgang erneut auf, und übergeben Sie das Fortsetzungstoken, um den nächsten Ergebnissatz so oft abzurufen, bis das Fortsetzungstoken NULL ist.

### <a name="filter-results-with-a-prefix"></a>Filtern von Ergebnissen mit einem Präfix

Um die Liste der Container zu filtern, geben Sie für den `prefix`-Parameter eine Zeichenfolge an. Die Präfixzeichenfolge kann ein oder mehrere Zeichen enthalten. Dann gibt Azure Storage nur die Blobs zurück, deren Namen mit diesem Präfix beginnen.

### <a name="return-metadata"></a>Zurückgeben von Metadaten

Wenn mit den Ergebnissen Blobmetadaten zurückgegeben werden sollen, geben Sie für die Enumeration [BlobListingDetails](/dotnet/api/microsoft.azure.storage.blob.bloblistingdetails) den Wert **Metadata** an. Weil Azure Storage Metadaten für jedes zurückgegebene Blob enthält, müssen Sie in diesem Kontext keine der **FetchAttributes**-Methoden aufrufen, um die Blobmetadaten abzurufen.

### <a name="flat-listing-versus-hierarchical-listing"></a>Flache Auflistung und hierarchische Auflistung im Vergleich

Blobs in Azure Storage sind in einem flachen Paradigma organisiert statt in einem hierarchischen Paradigma (wie ein klassisches Dateisystem). Sie können Blobs jedoch in *virtuellen Verzeichnissen* organisieren, um ein hierarchisches Paradigma zu imitieren. Ein virtuelles Verzeichnis ist ein Teil des Blobnamens und durch das Trennzeichen gekennzeichnet.

Wenn Sie also Blobs in virtuellen Verzeichnissen organisieren möchten, verwenden Sie ein Trennzeichen im Blobnamen. Das Standardtrennzeichen ist ein Schrägstrich (/), doch können Sie ein beliebiges Zeichen als Trennzeichen angeben.

Wenn Sie Ihre Blobs mithilfe eines Trennzeichens benennen, können Sie sie hierarchisch auflisten. Bei einem hierarchischen Auflistungsvorgang gibt Azure Storage alle virtuellen Verzeichnisse und Blobs unter dem übergeordneten Objekt zurück. Sie können den Auflistungsvorgang rekursiv aufrufen, um die Hierarchie zu durchlaufen – ähnlich wie beim programmgesteuerten Durchlaufen eines klassischen Dateisystems.

## <a name="use-a-flat-listing"></a>Verwenden einer flachen Auflistung

Ein Auflistungsvorgang gibt Blobs standardmäßig in einer flachen Auflistung zurück. In einer flachen Auflistung werden Blobs nicht nach virtuellem Verzeichnis organisiert.

Im folgenden Beispiel werden die Blobs im angegebenen Container mithilfe einer flachen Auflistung aufgeführt, wobei eine optionale Segmentgröße angegeben und der Blobname in ein Konsolenfenster geschrieben wird.

```csharp
private static async Task ListBlobsFlatListingAsync(CloudBlobContainer container, int? segmentSize)
{
    BlobContinuationToken continuationToken = null;
    CloudBlob blob;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned
        // and execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(string.Empty,
                true, BlobListingDetails.Metadata, segmentSize, continuationToken, null, null);

            foreach (var blobItem in resultSegment.Results)
            {
                // A flat listing operation returns only blobs, not virtual directories.
                blob = (CloudBlob)blobItem;

                // Write out some blob properties.
                Console.WriteLine("Blob name: {0}", blob.Name);
            }

            Console.WriteLine();

           // Get the continuation token and loop until it is null.
           continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Die Beispielausgabe sieht ähnlich wie hier aus:

```
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

## <a name="use-a-hierarchical-listing"></a>Verwenden einer hierarchischen Auflistung

Wenn Sie einen Auflistungsvorgang hierarchisch aufrufen, gibt Azure Storage die virtuellen Verzeichnisse und Blobs auf der ersten Hiearchieebene zurück. Die Eigenschaft [Prefix](/dotnet/api/microsoft.azure.storage.blob.cloudblobdirectory.prefix) der einzelnen virtuellen Verzeichnisse ist so festgelegt, dass Sie das Präfix in einem rekursiven Aufruf übergeben können, um das nächste Verzeichnis abzurufen.

Wenn Blobs hierarchisch aufgelistet werden sollen, legen Sie den Parameter `useFlatBlobListing` der Auflistungsmethode auf **false** fest.

Im folgenden Beispiel werden die Blobs im angegebenen Container mithilfe einer flachen Auflistung aufgeführt, wobei eine optionale Segmentgröße angegeben und der Blobname in das Konsolenfenster geschrieben wird.

```csharp
private static async Task ListBlobsHierarchicalListingAsync(CloudBlobContainer container, string prefix)
{
    CloudBlobDirectory dir;
    CloudBlob blob;
    BlobContinuationToken continuationToken;

    try
    {
        // Call the listing operation and enumerate the result segment.
        // When the continuation token is null, the last segment has been returned and 
        // execution can exit the loop.
        do
        {
            BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(prefix,
                false, BlobListingDetails.Metadata, null, null, null, null);
            foreach (var blobItem in resultSegment.Results)
            {
                // A hierarchical listing may return both virtual directories and blobs.
                if (blobItem is CloudBlobDirectory)
                {
                    dir = (CloudBlobDirectory)blobItem;

                    // Write out the prefix of the virtual directory.
                    Console.WriteLine("Virtual directory prefix: {0}", dir.Prefix);

                    // Call recursively with the prefix to traverse the virtual directory.
                    await ListBlobsHierarchicalListingAsync(container, dir.Prefix);
                }
                else
                {
                    // Write out the name of the blob.
                    blob = (CloudBlob)blobItem;

                    Console.WriteLine("Blob name: {0}", blob.Name);
                }
                Console.WriteLine();
            }

            // Get the continuation token and loop until it is null.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

Die Beispielausgabe sieht ähnlich wie hier aus:

```
Virtual directory prefix: FolderA/
Blob name: FolderA/blob1.txt
Blob name: FolderA/blob2.txt
Blob name: FolderA/blob3.txt

Virtual directory prefix: FolderA/FolderB/
Blob name: FolderA/FolderB/blob1.txt
Blob name: FolderA/FolderB/blob2.txt
Blob name: FolderA/FolderB/blob3.txt

Virtual directory prefix: FolderA/FolderB/FolderC/
Blob name: FolderA/FolderB/FolderC/blob1.txt
Blob name: FolderA/FolderB/FolderC/blob2.txt
Blob name: FolderA/FolderB/FolderC/blob3.txt
```

> [!NOTE]
> Blobmomentaufnahmen können nicht in einem hierarchischen Auflistungsvorgang aufgeführt werden.

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Auflisten von Blobs](/rest/api/storageservices/list-blobs)
- [Aufzählen von Blobressourcen](/rest/api/storageservices/enumerating-blob-resources)
