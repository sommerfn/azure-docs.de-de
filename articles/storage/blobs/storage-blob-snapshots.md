---
title: Erstellen und Verwalten einer Blobmomentaufnahme in .NET – Azure Storage
description: Hier erfahren Sie, wie Sie eine schreibgeschützte Blobmomentaufnahme erstellen, um Blobdaten zu einem bestimmten Zeitpunkt zu sichern.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/06/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: fb2da8acb0aa4d105f23ab5d1ad42f08a6ae722c
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595247"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>Erstellen und Verwalten einer Blobmomentaufnahme in .NET

Eine Momentaufnahme ist eine schreibgeschützte Version eines Blobs, die zu einem bestimmten Zeitpunkt erstellt wird. Momentaufnahmen sind nützlich, um Blobs zu sichern. In diesem Artikel wird die Erstellung und Verwaltung von Blobmomentaufnahmen unter Verwendung der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) beschrieben.

## <a name="about-blob-snapshots"></a>Informationen zu Blobmomentaufnahmen

Eine Momentaufnahme eines Blobs ist mit dem dazugehörigen Basisblob bis auf die Ausnahme identisch, dass an den Blob-URI ein **DateTime**-Wert angefügt ist. Hiermit wird der Zeitpunkt angegeben, zu dem die Momentaufnahme erstellt wurde. Wenn der Seitenblob-URI `http://storagesample.core.blob.windows.net/mydrives/myvhd` ist, lautet der Momentaufnahmen-URI z.B. in etwa `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Für alle Momentaufnahmen wird der URI des Basisblobs verwendet. Der einzige Unterschied zwischen dem Basisblob und der Momentaufnahme ist der angefügte **DateTime** -Wert.
>

Für ein Blob kann eine beliebige Anzahl von Momentaufnahmen vorhanden sein. Momentaufnahmen bleiben bestehen, bis sie explizit gelöscht werden. Eine Momentaufnahme kann also nicht länger bestehen bleiben als das zugehörige Basisblob. Sie können alle einem Basisblob zugeordneten Momentaufnahmen auflisten, um die aktuell vorhandenen Momentaufnahmen nachzuverfolgen.

Wenn Sie eine Momentaufnahme eines Blobs erstellen, werden seine Systemeigenschaften mit denselben Werten in die Momentaufnahme kopiert. Die Metadaten des Basisblobs werden auch in die Momentaufnahme kopiert, sofern Sie beim Erstellen keine separaten Metadaten für die Momentaufnahme angeben. Nach dem Erstellen einer Momentaufnahme kann sie gelesen, kopiert oder gelöscht, aber nicht mehr geändert werden.

Dem Basis-Blob zugeordnete Leases wirken sich nicht auf die Momentaufnahme aus. Sie können für eine Momentaufnahme keine Lease abrufen.

Für das Speichern der aktuellen Informationen und des Status eines VM-Datenträgers wird eine VHD-Datei verwendet. Sie können einen Datenträger vom virtuellen Computer trennen oder die VM herunterfahren und dann eine Momentaufnahme der VHD-Datei erstellen. Sie können diese Momentaufnahmedatei später verwenden, um die VHD-Datei zu diesem Zeitpunkt abzurufen und den virtuellen Computer neu zu erstellen.

## <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme

Verwenden Sie eine der folgenden Methoden, um eine Momentaufnahme eines Blockblobs zu erstellen:

- [CreateSnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [CreateSnapshotAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

Das folgende Codebeispiel zeigt, wie Sie eine Momentaufnahme erstellen. In diesem Beispiel werden zusätzliche Metadaten für die Momentaufnahme angegeben, wenn sie erstellt wird.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="delete-snapshots"></a>Löschen von Momentaufnahmen

Wenn Sie ein Blob löschen möchten, müssen Sie zunächst alle Momentaufnahmen dieses Blobs löschen. Sie können eine Momentaufnahme einzeln löschen oder angeben, dass alle Momentaufnahmen gelöscht werden sollen, wenn das Quellblob gelöscht wird. Wenn Sie versuchen, ein Blob zu löschen, für das noch Momentaufnahmen vorhanden sind, tritt ein Fehler auf.

Verwenden Sie zum Löschen von Blobmomentaufnahmen eine der folgenden Löschmethoden für Blobs, und schließen Sie die Enumeration [DeleteSnapshotsOption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) ein:

- [Löschen](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Blob und die zugehörigen Momentaufnahmen in .NET löschen, wenn `blockBlob` ein Objekt vom Typ [CloudBlockBlob][dotnet_CloudBlockBlob] ist:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Zurückgeben des absoluten URI zu einer Momentaufnahme

Im folgenden Codebeispiel wird eine Momentaufnahme erstellt und der absolute URI für den primären Standort ausgegeben:

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Grundlegendes zur Ermittlung der Gebühren für Momentaufnahmen

Durch das Erstellen einer Momentaufnahme, die eine schreibgeschützte Kopie eines BLOBs darstellt, können auf Ihrem Konto zusätzliche Gebühren für die Datenspeicherung anfallen. Wenn Sie beim Entwurf Ihrer Anwendung berücksichtigen, wie diese Gebühren auflaufen, können Sie die Kosten minimieren.

### <a name="important-billing-considerations"></a>Wichtige Überlegungen zur Abrechnung

Die folgende Liste enthält wichtige Punkte, die beim Erstellen einer Momentaufnahme zu berücksichtigen sind.

- Für Ihr Speicherkonto fallen Gebühren für eindeutige Blöcke oder Seiten an, die im Blob oder in der Momentaufnahme enthalten sind. Ihrem Konto werden erst dann zusätzliche Gebühren für Momentaufnahmen angerechnet, die einem BLOB zugeordnet sind, wenn Sie das BLOB aktualisieren, auf dem sie basieren. Sobald Sie das Basisblob aktualisieren, entstehen Abweichungen von den zugeordneten Momentaufnahmen. In diesem Fall werden für alle eindeutigen Blöcke oder Seiten in jedem Blob bzw. einer Momentaufnahme Gebühren berechnet.
- Wenn Sie einen Block innerhalb eines Block-BLOBs ersetzen, wird dieser Block anschließend als eindeutiger Block berechnet. Dies gilt auch, wenn der Block dieselbe Block-ID und dieselben Daten enthält wie in der Momentaufnahme. Nachdem ein erneuter Commit für den Block ausgeführt wurde, weicht er von seinem Pendant in den Momentaufnahmen ab, und Ihnen werden die Daten des Blocks berechnet. Gleiches gilt für eine Seite in einem Seitenblob, die mit identischen Daten aktualisiert wird.
- Wenn Sie ein Blockblob durch einen Aufruf einer der Methoden [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] oder [UploadFromByteArray][dotnet_UploadFromByteArray] ersetzen, werden alle Blöcke im Blob ersetzt. Wenn dem Blob eine Momentaufnahme zugeordnet ist, weisen anschließend alle Blöcke im Basisblob und in der Momentaufnahme Abweichungen auf, und Ihnen werden Gebühren für alle Blöcke in beiden Blobs berechnet. Dies gilt auch, wenn die Daten im Basis-BLOB und in der Momentaufnahme identisch sind.
- Der Azure-Blob-Dienst kann nicht feststellen, ob zwei Blöcke identische Daten enthalten. Jeder hochgeladene Block, für den ein Commit ausgeführt wird, wird als eindeutig behandelt, selbst wenn die enthaltenen Daten und die Block-ID identisch sind. Da Gebühren jeweils für eindeutige Blöcke berechnet werden, ist zu berücksichtigen, dass beim Aktualisieren eines Blobs mit einer zugeordneten Momentaufnahme zusätzliche eindeutige Blöcke generiert werden, für die zusätzliche Gebühren entstehen.

### <a name="minimize-cost-with-snapshot-management"></a>Minimieren der Kosten durch Momentaufnahmenverwaltung

Es empfiehlt sich, Ihre Momentaufnahmen sorgfältig zu verwalten, um zusätzlich anfallende Gebühren zu vermeiden. Sie können die folgenden bewährten Methoden befolgen, um die beim Speichern von Momentaufnahmen anfallenden Kosten zu minimieren:

- Löschen und erstellen Sie zugehörige Momentaufnahmen für ein BLOB neu, wenn Sie das BLOB aktualisieren, selbst wenn Sie mit identischen Daten aktualisieren, es sei denn, der Anwendungsentwurf erfordert, dass die Momentaufnahmen beibehalten werden. Durch Löschen und Neuerstellen der Momentaufnahmen für ein Blob können Sie sicherstellen, dass das Blob und die Momentaufnahmen nicht voneinander abweichen.
- Wenn Sie Momentaufnahmen für ein Blob beibehalten, sollten Sie Aufrufe von [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] und [UploadFromByteArray][dotnet_UploadFromByteArray] zum Aktualisieren des Blobs vermeiden. Bei diesen Methoden werden alle Blöcke im Blob ersetzt, sodass Ihr Basisblob und seine Momentaufnahmen erheblich voneinander abweichen. Aktualisieren Sie stattdessen so wenig Blöcke wie möglich, indem Sie die [PutBlock][dotnet_PutBlock]-Methode oder die [PutBlockList][dotnet_PutBlockList]-Methode aufrufen.

### <a name="snapshot-billing-scenarios"></a>Abrechnungsszenarien für Momentaufnahmen

Die folgenden Szenarien veranschaulichen, wie Gebühren für ein Block-BLOB und zugehörige Momentaufnahmen berechnet werden.

#### <a name="scenario-1"></a>Szenario 1

In Szenario 1 wurde das Basis-Blob nicht aktualisiert, nachdem die Momentaufnahme erstellt wurde, sodass Gebühren nur für die eindeutigen Blöcke 1, 2 und 3 berechnet werden:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>Szenario 2:

In Szenario 2 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde aktualisiert. Obwohl er die gleichen Daten und dieselbe ID enthält, ist er nicht identisch mit dem Block 3 der Momentaufnahme. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>Szenario 3

In Szenario 3 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde im Basis-BLOB durch Block 4 ersetzt, die Momentaufnahme enthält aber immer noch den Block 3. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>Szenario 4

In Szenario 4 wurde das Basis-Blob vollständig aktualisiert und enthält keinen der ursprünglichen Blöcke. Daher wird das Konto für alle acht eindeutigen Blöcke belastet. Dieses Szenario kann auftreten, wenn Sie eine Updatemethode wie [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] oder [UploadFromByteArray][dotnet_UploadFromByteArray] verwenden, da diese Methoden sämtliche Inhalte eines Blobs ersetzen.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Datenträger-Momentaufnahmen für virtuelle Computer (VMs) finden Sie in [Sichern nicht verwalteter Azure-VM-Datenträger mithilfe inkrementeller Momentaufnahmen](../../virtual-machines/windows/incremental-snapshots.md)

- Weitere Codebeispiele zur Verwendung von Blob Storage finden Sie unter [Azure-Codebeispiele](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Sie können eine Beispielanwendung herunterladen und ausführen oder den Code auf GitHub durchsuchen.

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfromfile
[dotnet_UploadFromStream]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext
