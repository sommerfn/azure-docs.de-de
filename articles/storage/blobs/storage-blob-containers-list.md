---
title: Auflisten von Blobcontainern mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie einen Blobcontainer in Ihrem Azure Storage-Konto mithilfe der .NET-Clientbibliothek auflisten.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: b7e5b0725049fa5de95f435c848502c36a3a1726
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72598123"
---
# <a name="list-blob-containers-with-net"></a>Auflisten von Blobcontainern mit .NET

Wenn Sie die Container in einem Azure Storage Konto über den Code auflisten, können Sie eine Reihe von Optionen angeben, um zu steuern, wie Ergebnisse von Azure Storage zurückgegeben werden. In diesem Artikel wird beschrieben, wie Container mit der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) aufgelistet werden.  

## <a name="understand-container-listing-options"></a>Grundlegendes zu den Optionen für das Auflisten von Containern

Rufen Sie zum Auflisten von Containern in Ihrem Speicherkonto eine der folgenden Methoden auf:

- [ListContainersSegmented](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmented)
- [ListContainersSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.listcontainerssegmentedasync)

Die Überladungen dieser Methoden bieten zusätzliche Optionen, um zu steuern, wie Container durch den Auflistungsvorgang zurückgegeben werden. Diese Optionen sind in den folgenden Abschnitten beschrieben.

### <a name="manage-how-many-results-are-returned"></a>Festlegen der Anzahl der zurückgegebenen Ergebnisse

Standardmäßig werden durch einen einzelnen Auflistungsvorgang bis zu 5.000 Ergebnisse zurückgegeben. Um einen kleineren Satz von Ergebnissen zurückzugeben, geben Sie für den Parameter `maxresults` einen Wert ungleich null an, wenn Sie eine der **ListContainerSegmented**-Methoden aufrufen.

Wenn Ihr Speicherkonto mehr als 5.000 Container enthält oder wenn Sie einen Wert für `maxresults` angegeben haben, aufgrund dessen die Auflistung eine Teilmenge der Container im Speicherkonto zurückgibt, gibt Azure Storage ein *Fortsetzungstoken* mit der Liste der Container zurück. Ein Fortsetzungstoken ist ein nicht transparenter Wert, den Sie verwenden können, um den nächsten Satz von Ergebnissen aus Azure Storage abzurufen.

Überprüfen Sie im Code den Wert des Fortsetzungstokens, um zu bestimmen, ob er NULL ist. Wenn das Fortsetzungstoken NULL ist, ist der Satz der Ergebnisse vollständig. Wenn das Fortsetzungstoken nicht NULL ist, rufen Sie **ListContainersSegmented** oder **ListContainersSegmentedAsync** erneut auf, und übergeben Sie das Fortsetzungstoken, um den nächsten Satz von Ergebnissen abzurufen, bis das Fortsetzungstoken NULL ist.

### <a name="filter-results-with-a-prefix"></a>Filtern von Ergebnissen mit einem Präfix

Um die Liste der Container zu filtern, geben Sie für den `prefix`-Parameter eine Zeichenfolge an. Die Präfixzeichenfolge kann ein oder mehrere Zeichen enthalten. Azure Storage gibt dann nur die Container zurück, deren Namen mit diesem Präfix beginnen.

### <a name="return-metadata"></a>Zurückgeben von Metadaten

Um mit den Ergebnissen Containermetadaten zurückzugeben, geben Sie für die Enumeration [ContainerListingDetails](/dotnet/api/microsoft.azure.storage.blob.containerlistingdetails) den Wert **Metadata** an. Da Azure Storage für jeden zurückgegebenen Container Metadaten enthält, müssen Sie nicht zusätzlich eine der **FetchAttributes**-Methoden aufrufen, um die Containermetadaten abzurufen.

## <a name="example-list-containers"></a>Beispiel: Auflisten von Containern

Im folgenden Beispiel werden die Container in einem Speicherkonto, die mit einem angegebenen Präfix beginnen, asynchron aufgelistet. Im Beispiel werden Container in Inkrementen von jeweils 5 Ergebnissen aufgelistet, und zum Abrufen des nächsten Segments von Ergebnissen wird das Fortsetzungstoken verwendet. Im Beispiel werden mit den Ergebnissen auch Containermetadaten zurückgegeben.

```csharp
private static async Task ListContainersWithPrefixAsync(CloudBlobClient blobClient, string prefix)
{
    Console.WriteLine("List all containers beginning with prefix {0}, plus container metadata:", prefix);

    try
    {
        ContainerResultSegment resultSegment = null;
        BlobContinuationToken continuationToken = null;

        do
        {
            // List containers beginning with the specified prefix, returning segments of 5 results each.
            // Passing null for the maxResults parameter returns the max number of results (up to 5000).
            // Requesting the container's metadata with the listing operation populates the metadata,
            // so it's not necessary to also call FetchAttributes() to read the metadata.
            resultSegment = await blobClient.ListContainersSegmentedAsync(
                prefix, ContainerListingDetails.Metadata, 5, continuationToken, null, null);

            // Enumerate the containers returned.
            foreach (var container in resultSegment.Results)
            {
                Console.WriteLine("\tContainer:" + container.Name);

                // Write the container's metadata keys and values.
                foreach (var metadataItem in container.Metadata)
                {
                    Console.WriteLine("\t\tMetadata key: " + metadataItem.Key);
                    Console.WriteLine("\t\tMetadata value: " + metadataItem.Value);
                }
            }

            // Get the continuation token. If not null, get the next segment.
            continuationToken = resultSegment.ContinuationToken;

        } while (continuationToken != null);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0} : {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Weitere Informationen

[Auflisten von Containern](/rest/api/storageservices/list-containers2)
[Auflisten von Blobressourcen](/rest/api/storageservices/enumerating-blob-resources)
