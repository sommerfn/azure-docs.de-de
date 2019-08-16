---
title: 'Abrufen des Typs und des SKU-Namens eines Speicherkontos mit .NET: Azure Storage'
description: Hier erfahren Sie, wie Sie den Typ und den SKU-Namen eines Azure Storage-Kontos mit der .NET-Clientbibliothek abrufen.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/06/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 21ecaae679ad4a5f21107ef53d3899cf03593c9d
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828878"
---
# <a name="get-storage-account-type-and-sku-name-with-net"></a>Abrufen des Typs und des SKU-Namens eines Speicherkontos mit .NET

In diesem Artikel wird gezeigt, wie Sie mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage/client) den Azure Storage-Kontotyp und -SKU-Namen für einen Blob abrufen.

Kontoinformationen sind für Dienstversionen verfügbar, die mit „2018-03-28“ beginnen.

## <a name="about-account-type-and-sku-name"></a>Abrufen von Kontotyp und SKU-Name

**Kontotyp:** Gültige Kontotypen sind `BlobStorage`, `BlockBlobStorage`, `FileStorage`, `Storage` und `StorageV2`. In der [Übersicht über Azure Storage-Konten](storage-account-overview.md) finden Sie weitere Informationen, u. a. Beschreibungen der verschiedenen Speicherkonten.

**SKU-Name:** Zu den gültigen SKU-Namen zählen `Premium_LRS`, `Premium_ZRS`, `Standard_GRS`, `Standard_GZRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_RAGZRS` und `Standard_ZRS`. Bei SKU-Namen muss die Groß-/Kleinschreibung beachtet werden, und es handelt sich dabei um Zeichenfolgenfelder in der [SkuName-Klasse](/dotnet/api/microsoft.azure.management.storage.models.skuname?view=azure-dotnet).

## <a name="retrieve-account-information"></a>Abrufen von Kontoinformationen

Rufen Sie zum Abrufen des Speicherkontotyps und -SKU-Namens eines Blobs die Methode [GetAccountProperties](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountproperties?view=azure-dotnet) oder [GetAccountPropertiesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountpropertiesasync?view=azure-dotnet) auf.

Im folgenden Codebeispiel werden die schreibgeschützten Kontoeigenschaften abgerufen und angezeigt.

```csharp
private static async Task GetAccountInfoAsync(CloudBlob blob)
{
    try
    {
        // Get the blob's storage account properties.
        AccountProperties acctProps = await blob.GetAccountPropertiesAsync();

        // Display the properties.
        Console.WriteLine("Account properties");
        Console.WriteLine("  AccountKind: {0}", acctProps.AccountKind);
        Console.WriteLine("      SkuName: {0}", acctProps.SkuName);
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

[!INCLUDE [storage-blob-dotnet-resources](../../../includes/storage-blob-dotnet-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über andere Vorgänge, die Sie über das [Azure-Portal](https://portal.azure.com) und die Azure-Rest-API für ein Speicherkonto ausführen können.

- [Verwalten eines Speicherkontos](storage-account-manage.md)
- [Aktualisieren eines Speicherkontos](storage-account-upgrade.md)
- [Abrufen von Kontoinformationen (REST)](/rest/api/storageservices/get-account-information)
