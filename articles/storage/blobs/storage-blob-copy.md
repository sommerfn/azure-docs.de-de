---
title: Kopieren eines Blobs mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie ein Blob in Ihrem Azure Storage-Konto mithilfe der .NET-Clientbibliothek kopieren.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/20/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 9b3dba0041b38d9d59a10eaf80592bab91f65b98
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72599908"
---
# <a name="copy-a-blob-with-net"></a>Kopieren eines Blobs mit .NET

In diesem Artikel wird beschrieben, wie Sie ein Blob mit einem Azure Storage-Konto kopieren. Außerdem wird gezeigt, wie ein asynchroner Kopiervorgang abgebrochen wird. Im Beispielcode wird die [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) verwendet.

## <a name="about-copying-blobs"></a>Informationen zum Kopieren von Blobs

Wenn Sie ein Blob innerhalb desselben Speicherkontos kopieren, ist dies ein synchroner Vorgang. Wenn Sie zwischen Konten kopieren, ist es ein asynchroner Vorgang. Die Methoden [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet) und [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet) geben einen Wert für die Kopier-ID zurück, der zum Überprüfen des Status oder zum Abbrechen des Kopiervorgangs verwendet wird.

Das Quellblob für einen Kopiervorgang kann ein Blockblob, ein Anfügeblob, ein Seitenblob oder eine Momentaufnahme sein. Wenn das Zielblob bereits vorhanden ist, muss es von demselben Blobtyp wie das Quellblob sein. Ein eventuell vorhandenes Zielblob wird überschrieben. 

Das Zielblob kann während der Ausführung eines Kopiervorgangs nicht geändert werden. Bei einem Zielblob kann es nur einen ausstehenden Vorgang zum Kopieren des Blobs geben. Anders ausgedrückt: Ein Blob kann nicht das Ziel für mehrere ausstehende Kopiervorgänge sein.

Es wird immer das gesamte Quellblob oder die gesamte Quelldatei kopiert. Das Kopieren eines Bytebereichs oder einer Gruppe von Blöcken wird nicht unterstützt.

Wenn ein Blob kopiert wird, werden dessen Systemeigenschaften mit denselben Werten in das Zielblob kopiert.

Bei allen Blobtypen können Sie die Eigenschaft [CopyState.Status](/dotnet/api/microsoft.azure.storage.blob.copystate.status?view=azure-dotnet) im Zielblob überprüfen, um den Status des Kopiervorgangs zu erfahren. Das endgültige Blob wird bei Abschluss des Kopiervorgangs committet.

Ein Kopiervorgang kann eine der folgenden Formen haben:

  - Sie können ein Quellblob in ein Zielblob mit einem anderen Namen kopieren. Das Zielblob kann ein vorhandenes Blob desselben Blobtyps sein (Blockblob, Anfügeblob oder Seitenblob), oder es kann sich um ein neues Blob handeln, das durch den Kopiervorgang erstellt wurde.
  - Sie können ein Quellblob in ein Zielblob mit demselben Namen kopieren und so das Zielblob effektiv ersetzen. Ein solcher Kopiervorgang entfernt alle Blöcke ohne Commit und überschreibt die Metadaten des Zielblobs.
  - Sie können eine Quelldatei im Azure-Dateidienst in ein Zielblob kopieren. Das Zielblob kann ein vorhandenes Blockblob oder ein neues Blockblob sein, das durch den Kopiervorgang erstellt wurde. Das Kopieren von Dateien in Seitenblobs oder Anfügeblobs wird nicht unterstützt.
  - Sie können eine Momentaufnahme über das zugehörige Basis-Blob kopieren. Indem Sie eine Momentaufnahme zu einem Basis-Blob heraufstufen, können Sie eine frühere Version eines Blobs wiederherstellen.
  - Sie können eine Momentaufnahme in ein Ziel-Blob mit einem anderen Namen kopieren. Das resultierende Zielblob ist ein beschreibbares Blob und keine Momentaufnahme.

## <a name="copy-a-blob"></a>Kopieren eines Blobs

Zum Kopieren eines Blobs rufen Sie eine der folgenden Methoden auf:

 - [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet)
 - [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet)

Im folgenden Codebeispiel wird ein Verweis auf ein zuvor erstelltes Blob abgerufen und in ein neues Blob in demselben Container kopiert:

```csharp
private static async Task CopyBlockBlobAsync(CloudBlobContainer container)
{
    CloudBlockBlob sourceBlob = null;
    CloudBlockBlob destBlob = null;
    string leaseId = null;

    try
    {
        // Get a block blob from the container to use as the source.
        sourceBlob = container.ListBlobs().OfType<CloudBlockBlob>().FirstOrDefault();

        // Lease the source blob for the copy operation to prevent another client from modifying it.
        // Specifying null for the lease interval creates an infinite lease.
        leaseId = await sourceBlob.AcquireLeaseAsync(null);

        // Get a reference to a destination blob (in this case, a new blob).
        destBlob = container.GetBlockBlobReference("copy of " + sourceBlob.Name);

        // Ensure that the source blob exists.
        if (await sourceBlob.ExistsAsync())
        {
            // Get the ID of the copy operation.
            string copyId = await destBlob.StartCopyAsync(sourceBlob);

            // Fetch the destination blob's properties before checking the copy state.
            await destBlob.FetchAttributesAsync();

            Console.WriteLine("Status of copy operation: {0}", destBlob.CopyState.Status);
            Console.WriteLine("Completion time: {0}", destBlob.CopyState.CompletionTime);
            Console.WriteLine("Bytes copied: {0}", destBlob.CopyState.BytesCopied.ToString());
            Console.WriteLine("Total bytes: {0}", destBlob.CopyState.TotalBytes.ToString());
            Console.WriteLine();
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
    finally
    {
        // Break the lease on the source blob.
        if (sourceBlob != null)
        {
            await sourceBlob.FetchAttributesAsync();

            if (sourceBlob.Properties.LeaseState != LeaseState.Available)
            {
                await sourceBlob.BreakLeaseAsync(new TimeSpan(0));
            }
        }
    }
}
```

## <a name="abort-a-blob-copy-operation"></a>Abbrechen eines Vorgangs zum Kopieren des Blobs

Das Abbrechen eines Kopiervorgangs führt zu einem Zielblob der Länge 0 (null) für Blockblobs, Anfügeblobs und Seitenblobs. Die Metadaten für das Zielblob haben jedoch die neuen Werte, die aus dem Quellblob kopiert oder im Aufruf [StartCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopy?view=azure-dotnet) oder [StartCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.startcopyasync?view=azure-dotnet) explizit festgelegt wurden. Zur Beibehaltung der ursprünglichen Metadaten von vor dem Kopiervorgang erstellen Sie eine Momentaufnahme des Zielblobs, bevor Sie `StartCopy` oder `StartCopyAsync`aufrufen.

Wenn Sie einen Vorgang zum Kopieren des Blobs abbrechen, wird der Wert für [CopyState.Status](/dotnet/api/microsoft.azure.storage.blob.copystate.status?view=azure-dotnet#Microsoft_Azure_Storage_Blob_CopyState_Status) des Zielblobs auf [CopyStatus.Aborted](/dotnet/api/microsoft.azure.storage.blob.copystatus?view=azure-dotnet) festgelegt.

Die Methoden [AbortCopy](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopy?view=azure-dotnet) und [AbortCopyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.abortcopyasync?view=azure-dotnet) brechen einen laufenden Vorgang zum Kopieren des Blobs ab und behalten ein Zielblob mit der Länge 0 (null) und vollständigen Metadaten bei.

```csharp
// Fetch the destination blob's properties before checking the copy state.
await destBlob.FetchAttributesAsync();

// Check the copy status. If it is still pending, abort the copy operation.
if (destBlob.CopyState.Status == CopyStatus.Pending)
{
    await destBlob.AbortCopyAsync(copyId);
    Console.WriteLine("Copy operation {0} has been aborted.", copyId);
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Themen enthalten Informationen zum Kopieren von Blobs und zum Abbrechen laufender Kopiervorgänge mithilfe der Azure-REST-APIs.

- [Copy Blob](/rest/api/storageservices/copy-blob)
- [Abort Copy Blob](/rest/api/storageservices/abort-copy-blob)
