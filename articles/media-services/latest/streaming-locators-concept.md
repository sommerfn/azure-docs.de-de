---
title: Streaminglocators in Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel erläutert, was Streaminglocators sind und wie sie in Azure Media Services verwendet werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/26/2019
ms.author: juliako
ms.openlocfilehash: 6d13ca5b3657f1deac9e6b4218decf8fe57eb1d9
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113739"
---
# <a name="streaming-locators"></a>Streaminglocators

Um Videos im Ausgabeobjekt für die Wiedergabe durch Clients verfügbar zu machen, müssen Sie einen [Streaminglocator](https://docs.microsoft.com/rest/api/media/streaminglocators) erstellen und dann die Streaming-URLs erstellen. Zum Erstellen einer URL müssen Sie den Hostnamen des Streamingendpunkts und den Pfad des Streaminglocators miteinander verketten. Informationen zum .NET-Beispiel finden Sie unter [Abrufen eines Streaminglocators](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Der Vorgang zum Erstellen eines **Streaminglocators** wird als „Veröffentlichen“ bezeichnet. Standardmäßig ist der **Streaminglocator** sofort nach dem Vornehmen der API-Aufrufe gültig und bleibt es auch, bis er gelöscht wird (es sei denn, Sie konfigurieren die optionalen Start- und Endzeiten). 

Beim Erstellen eines **Streaminglocators** müssen Sie einen **Medienobjekt**namen und einen Namen für die **Streamingrichtlinie** angeben. Weitere Informationen finden Sie in den folgenden Themen:

* [Medienobjekte](assets-concept.md)
* [Streamingrichtlinien](streaming-policy-concept.md)
* [Richtlinien für Inhaltsschlüssel](content-key-policy-concept.md)

Sie können außerdem die Start- und Endzeit in Ihrem Streaminglocator angeben, der Ihrem Benutzer nur die Wiedergabe des Inhalts zwischen diesen Zeitangaben gestattet (z. B. zwischen 1.5.2019 und 5.5.2019).  

## <a name="considerations"></a>Überlegungen

* **Streaminglocators** werden nicht aktualisiert. 
* Eigenschaften von **Streaminglocators** vom Datetime-Typ liegen immer im UTC-Format vor.
* Sie sollten eine begrenzte Sammlung von Richtlinien für Ihr Media Services-Konto erstellen und diese für Ihre Streaminglocators wiederverwenden, wenn dieselben Optionen benötigt werden. Weitere Informationen finden Sie unter [Kontingente und Einschränkungen](limits-quotas-constraints.md).

## <a name="create-streaming-locators"></a>Erstellen von Streaminglocators  

### <a name="not-encrypted"></a>Nicht verschlüsselt

Wenn Sie Ihre Datei im Klartext (nicht verschlüsselt) streamen möchten, legen Sie die vordefinierte Richtlinie für unverschlüsseltes Streaming fest: auf „Predefined_ClearStreamingOnly“ (in .NET können Sie die Enumeration „PredefinedStreamingPolicy.ClearStreamingOnly“ verwenden).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Verschlüsselt 

Wenn Sie Ihren Inhalt mit CENC-Verschlüsselung verschlüsseln müssen, legen Sie die Richtlinie auf „Predefined_MultiDrmCencStreaming“ fest. Die Widevine-Verschlüsselung wird auf einen DASH-Datenstrom und PlayReady auf Smooth angewendet. Der Schlüssel wird an einen Wiedergabeclient basierend auf den konfigurierten DRM-Lizenzen übermittelt.

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Wenn Sie auch Ihren HLS-Datenstrom mit CBCS (FairPlay) verschlüsseln möchten, verwenden Sie „Predefined_MultiDrmStreaming“.

## <a name="associate-filters-with-streaming-locators"></a>Zuordnen von Filtern mit Streaminglocators

Lesen Sie [Filter: Zuordnen mit Streaminglocators](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filtern, Sortieren, Paginieren von Streaminglocatorentitäten

Informationen finden Sie unter [Filterung, Sortierung, Paginierung von Media Services-Entitäten](entities-overview.md).

## <a name="list-streaming-locators-by-asset-name"></a>Auflisten von Streaminglocators nach Medienobjektname

Um Streaminglocators basierend auf dem zugeordneten Medienobjektnamen abzurufen, verwenden Sie die folgenden Vorgänge:

|Sprache|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|Befehlszeilenschnittstelle (CLI)|[az ams asset list-streaming-locators](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[ListStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[AssetStreamingLocator](https://docs.microsoft.com/java/api/com.microsoft.azure.management.mediaservices.v2018_07_01.assetstreaminglocator?view=azure-java-stable)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/@azure/arm-mediaservices/assets#liststreaminglocators-string--string--string--msrest-requestoptionsbase-)|

## <a name="also-see"></a>Siehe auch

* [Medienobjekte](assets-concept.md)
* [Streamingrichtlinien](streaming-policy-concept.md)
* [Richtlinien für Inhaltsschlüssel](content-key-policy-concept.md)

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Hochladen, Codieren und Streamen von Videos mithilfe von .NET](stream-files-tutorial-with-api.md)
