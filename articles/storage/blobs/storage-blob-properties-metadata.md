---
title: Verwalten von Eigenschaften und Metadaten für ein Blob mit .NET – Azure Storage
description: Hier erfahren Sie, wie Sie mithilfe der .NET-Clientbibliothek Systemeigenschaften festlegen und abrufen sowie benutzerdefinierte Metadaten für Blobs in Ihrem Azure Storage-Konto speichern.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 03171e395bb23da05ee5420f60abca9a16ffb774
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72599912"
---
# <a name="manage-blob-properties-and-metadata-with-net"></a>Verwalten von Blobeigenschaften und Metadaten mit .NET

Blobs unterstützen neben den darin enthaltenen Daten auch Systemeigenschaften und benutzerdefinierte Metadaten. In diesem Artikel erfahren Sie, wie Sie Systemeigenschaften und benutzerdefinierte Metadaten mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) verwalten.

## <a name="about-properties-and-metadata"></a>Informationen zu Eigenschaften und Metadaten

- **Systemeigenschaften**: Systemeigenschaften sind in jeder Blobspeicherressource vorhanden. Einige davon können gelesen oder festgelegt werden, während andere schreibgeschützt sind. Darüber hinaus entsprechen einige Systemeigenschaften bestimmten HTTP-Standardheadern. Die Azure Storage-Clientbibliothek für .NET verwaltet diese Eigenschaften für Sie.

- **Benutzerdefinierte Metadaten**: Benutzerdefinierte Metadaten bestehen aus mindestens einem Name-Wert-Paar, das Sie für eine Blobspeicherressource angeben. Metadaten können verwendet werden, um zusätzliche Werte mit der Ressource zu speichern. Metadatenwerte sind nur für Ihre eigenen Zwecke bestimmt und wirken sich nicht auf das Verhalten der Ressource aus.

Das Abrufen von Metadaten- und Eigenschaftswerten für eine Blobspeicherressource ist ein zweistufiger Prozess. Bevor Sie diese Werte lesen können, müssen Sie sie explizit durch Aufrufen der Methode `FetchAttributes` oder `FetchAttributesAsync` abrufen. Eine Ausnahme von dieser Regel besteht darin, dass die Methoden `Exists` und `ExistsAsync` die entsprechende `FetchAttributes`-Methode im Hintergrund aufrufen. Wenn Sie eine dieser Methoden aufrufen, müssen Sie `FetchAttributes` nicht zusätzlich aufrufen.

> [!IMPORTANT]
> Sollten Sie feststellen, dass die Eigenschafts- oder Metadatenwerte für eine Speicherressource nicht aufgefüllt wurden, überprüfen Sie, ob Ihr Code die Methode `FetchAttributes` oder `FetchAttributesAsync` aufruft.

## <a name="set-and-retrieve-properties"></a>Festlegen und Abrufen von Eigenschaften

Im folgenden Codebeispiel werden die Systemeigenschaften `ContentType` und `ContentLanguage` für ein Blob festgelegt.

```csharp
public static async Task SetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        Console.WriteLine("Setting blob properties.");

        // You must explicitly set the MIME ContentType every time
        // the properties are updated or the field will be cleared.
        blob.Properties.ContentType = "text/plain";
        blob.Properties.ContentLanguage = "en-us";

        // Set the blob's properties.
        await blob.SetPropertiesAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Rufen Sie zum Abrufen von Blob-Eigenschaften die Methode `FetchAttributes` oder `FetchAttributesAsync` für Ihr Blob auf, um die Eigenschaft `Properties` aufzufüllen. Im folgenden Codebeispiel werden die Systemeigenschaften eines Blobs abgerufen und einige der Werte angezeigt:

```csharp
private static async Task GetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        // Fetch the blob properties.
        await blob.FetchAttributesAsync();

        // Display some of the blob's property values.
        Console.WriteLine(" ContentLanguage: {0}", blob.Properties.ContentLanguage);
        Console.WriteLine(" ContentType: {0}", blob.Properties.ContentType);
        Console.WriteLine(" Created: {0}", blob.Properties.Created);
        Console.WriteLine(" LastModified: {0}", blob.Properties.LastModified);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

## <a name="set-and-retrieve-metadata"></a>Festlegen und Abrufen von Metadaten

Sie können Metadaten als ein oder mehrere Name-Wert-Paare für eine Blob- oder Containerressource angeben. Fügen Sie der Auflistung `Metadata` für die Ressource Name/Wert-Paare hinzu, um Metadaten festzulegen. Anschließend können Sie eine der folgenden Methoden zum Schreiben der Werte aufrufen:

- [SetMetadata](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadataasync)

Name/Wert-Paare für Metadaten sind gültige HTTP-Header und sollten allen Einschränkungen für HTTP-Header entsprechen. Metadatennamen müssen gültige HTTP-Headernamen und gültige C#-Bezeichner sein, dürfen nur ASCII-Zeichen enthalten und sollten als „Keine Beachtung von Groß-/Kleinschreibung“ behandelt werden. Metadatenwerte [Base64-encode](https://docs.microsoft.com/dotnet/api/system.convert.tobase64string) oder [URL-encode](https://docs.microsoft.com/dotnet/api/system.web.httputility.urlencode), die Nicht-ASCII-Zeichen enthalten.

Der Name der Metadaten muss den Benennungskonventionen für C#-Bezeichner entsprechen. Metadatennamen behalten die Groß-/Kleinschreibung bei, mit der sie erstellt wurden. Beim Festlegen oder Lesen wird die Groß-/Kleinschreibung allerdings ignoriert. Wenn für eine Ressource mehrere Metadatenheader mit demselben Namen übermittelt werden, gibt Azure Blob Storage den HTTP-Fehlercode 400 („Ungültige Anforderung“) zurück.

Das folgende Codebeispiel legt die Metadaten für ein Blob fest. Mit der Methode `Add` der Sammlung wird ein Wert festgelegt. Der andere Wert wird mit der impliziten Schlüssel-Wert-Syntax festgelegt.

```csharp
public static async Task AddBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Add metadata to the blob by calling the Add method.
        blob.Metadata.Add("docType", "textDocuments");

        // Add metadata to the blob by using key/value syntax.
        blob.Metadata["category"] = "guidance";

        // Set the blob's metadata.
        await blob.SetMetadataAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Zum Abrufen von Metadaten rufen Sie die Methode `FetchAttributes` oder `FetchAttributesAsync` für das Blob oder den Container auf, um die Auflistung `Metadata` aufzufüllen, und lesen anschließend die Werte, wie im Beispiel unten gezeigt wird.

```csharp
public static async Task ReadBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Fetch blob attributes in order to populate 
        // the blob's properties and metadata.
        await blob.FetchAttributesAsync();

        Console.WriteLine("Blob metadata:");

        // Enumerate the blob's metadata.
        foreach (var metadataItem in blob.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Weitere Informationen

- [Vorgang zum Festlegen von Blob-Eigenschaften](/rest/api/storageservices/set-blob-properties)
- [Vorgang zum Abrufen von Blob-Eigenschaften](/rest/api/storageservices/get-blob-properties)
- [Vorgang zum Festlegen von Blobmetadaten](/rest/api/storageservices/set-blob-metadata)
- [Vorgang zum Abrufen von Blobmetadaten](/rest/api/storageservices/set-blob-metadata)
