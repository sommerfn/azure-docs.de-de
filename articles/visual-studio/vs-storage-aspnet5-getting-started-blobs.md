---
title: Erste Schritte mit Blobspeicher mit Visual Studio (ASP.NET Core)
description: Informieren Sie sich über die ersten Schritte mit Azure Blob Storage in einem ASP.NET Core-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten ein Speicherkonto erstellt haben.
services: storage
author: ghogen
manager: jillfra
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 81df41470c893f569fd17345e8bdf4b29641ec64
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298835"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Erste Schritte mit Azure Blob Storage und verbundenen Visual Studio-Diensten (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

Dieser Artikel beschreibt die ersten Schritte beim Verwenden von Azure Blob Storage in Visual Studio, nachdem Sie über das Visual Studio-Feature **Verbundene Dienste** in einem ASP.NET Core-Projekt ein Azure Storage-Konto erstellt oder darauf verwiesen haben. Beim Vorgang für **Verbundene Dienste** werden die entsprechenden NuGet-Pakete installiert, um auf Azure Storage in Ihrem Projekt zuzugreifen, und die Verbindungszeichenfolge für das Speicherkonto wird in Ihren Konfigurationsdateien des Projekts hinzugefügt. (Allgemeine Informationen zu Azure Storage finden Sie in der [Speicherdokumentation](https://azure.microsoft.com/documentation/services/storage/).)

Azure Blob Storage ist ein Dienst zur Speicherung großer Mengen unstrukturierter Daten, auf die von überall auf der Welt über HTTP oder HTTPS zugegriffen werden kann. Ein einzelner BLOB kann eine beliebige Größe aufweisen. BLOBs können Bilder, Audio- und Videodateien, Rohdaten und Dokumentdateien enthalten. In diesem Artikel werden die ersten Schritte mit dem Blob Storage beschrieben, nachdem Sie über das Visual Studio-Feature **Verbundene Dienste** ein Azure Storage-Konto in einem ASP.NET Core-Projekt erstellt haben.

Dateien werden in Ordnern gespeichert, Speicher-BLOBs analog dazu in Containern. Nachdem Sie ein Blob erstellt haben, erstellen Sie darin mindestens einen Container. Beispielsweise können Sie in einem Blob mit dem Namen „Notizbuch“ Container mit dem Namen „Bilder“ erstellen, um Bilder zu speichern, und einen anderen Container mit dem Namen „Audio“, um Audiodateien zu speichern. Nachdem Sie die Container erstellt haben, können Sie einzelne Dateien in die Container hochladen. Weitere Informationen finden Sie unter [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md). Hier finden Sie Informationen zum programmgesteuerten Bearbeiten von Blobs.

Einige Azure Storage-APIs sind asynchron, und im Code in diesem Artikel wird vorausgesetzt, dass asynchrone Methoden verwendet werden. Weitere Informationen finden Sie unter [Asynchrone Programmierung](https://docs.microsoft.com/dotnet/csharp/async).

## <a name="access-blob-containers-in-code"></a>Zugreifen auf BLOB-Container in Code

Für den programmgesteuerten Zugriff auf Blobs in ASP.NET Core-Projekten müssen Sie den folgenden Code hinzufügen, wenn er nicht bereits vorhanden ist:

1. Fügen Sie die erforderlichen `using`-Anweisungen hinzu:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Rufen Sie ein `CloudStorageAccount`-Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um Ihre Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Verwenden Sie ein `CloudBlobClient`-Objekt, um in Ihrem Speicherkonto einen `CloudBlobContainer`-Verweis auf einen vorhandenen Container zu erhalten:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Erstellen eines Containers in Code

Sie können den `CloudBlobClient` auch zum Erstellen eines Containers in Ihrem Speicherkonto verwenden, indem Sie `CreateIfNotExistsAsync` aufrufen:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Legen Sie für den Container den öffentlichen Zugriff fest, um die Dateien im Container für alle Benutzer verfügbar zu machen:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines Blobs in einen Container

Rufen Sie einen Containerverweis ab, und verwenden Sie diesen dann zum Abrufen eines BLOB-Verweises, um eine BLOB-Datei in einen Container hochzuladen. Laden Sie anschließend einen beliebigen Datenstrom auf diesen Verweis hoch, indem Sie die `UploadFromStreamAsync`-Methode aufrufen. Dieser Vorgang erstellt das Blob, wenn es noch nicht vorhanden ist, und überschreibt ein vorhandenes Blob. 

```cs
// Get a reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with the contents of a local file
// named "myfile".
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    await blockBlob.UploadFromStreamAsync(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Rufen Sie zum Auflisten von Blobs in einem Container zuerst einen Containerverweis ab, und rufen Sie anschließend die dazugehörige `ListBlobsSegmentedAsync`-Methode auf, um die darin enthaltenen Blobs bzw. Verzeichnisse abzurufen. Wenn Sie auf die umfassenden Eigenschaften und Methoden für ein zurückgegebenes `IListBlobItem` zugreifen möchten, müssen Sie es in ein `CloudBlockBlob`-, `CloudPageBlob`- oder `CloudBlobDirectory`-Objekt umwandeln. Falls der Blob-Typ unbekannt ist, können Sie eine Typüberprüfung verwenden, um zu bestimmen, in welchen Typ die Umwandlung erfolgen soll.

```cs
BlobContinuationToken token = null;
do
{
    BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
    token = resultSegment.ContinuationToken;

    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
        }

        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
        }

        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }
} while (token != null);
```

Weitere Informationen finden Sie unter [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container). Hier finden Sie Informationen zu weiteren Möglichkeiten zum Auflisten des Inhalts eines Blobcontainers.

## <a name="download-a-blob"></a>Herunterladen eines Blobs

Rufen Sie zum Herunterladen eines Blobs zuerst einen Verweis auf das Blob ab, und rufen Sie anschließend die `DownloadToStreamAsync`-Methode auf. Im folgenden Beispiel wird die `DownloadToStreamAsync`-Methode verwendet, um den Inhalt des Blobs in ein Datenstromobjekt zu übertragen, das anschließend als lokale Datei gespeichert werden kann.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Weitere Informationen finden Sie unter [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit .NET](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs). Hier sind weitere Möglichkeiten beschrieben, wie Blobs als Dateien gespeichert werden können.

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Rufen Sie zum Löschen eines Blobs zuerst einen Verweis auf das Blob ab, und rufen Sie anschließend die `DeleteAsync`-Methode auf:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
