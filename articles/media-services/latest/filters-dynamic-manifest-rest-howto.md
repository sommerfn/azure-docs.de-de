---
title: Erstellen von Filtern mit der Azure Media Services-REST-API | Microsoft Docs
description: In diesem Thema wird erläutert, wie Sie Filter erstellen, mit denen Ihre Kunden bestimmte Abschnitte eines Streams streamen können. Media Services erstellt dynamische Manifeste, um dieses selektive Streaming zu erreichen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 76e6e1595cb8bf49dbbc82c3cae5de80ea718aeb
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786454"
---
# <a name="creating-filters-with-media-services-rest-api"></a>Erstellen von Filtern mit der Media Services-REST-API

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für das Standardmedienobjekt beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren. 

Eine ausführliche Beschreibung dieser Funktion und der Szenarien, in denen sie verwendet wird, finden Sie unter [Dynamische Manifeste](filters-dynamic-manifest-overview.md) und [Filter](filters-concept.md).

In diesem Thema wird beschrieben, wie Sie einen Filter für ein Video on Demand-Medienobjekt definieren und mithilfe von REST-APIs [Kontofilter](https://docs.microsoft.com/rest/api/media/accountfilters) und [Medienobjektfilter](https://docs.microsoft.com/rest/api/media/assetfilters) erstellen. 

> [!NOTE]
> Lesen Sie die Informationen zu [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Voraussetzungen 

Für die in diesem Thema beschriebenen Schritte ist Folgendes erforderlich:

- Lesen Sie [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).
- [Konfigurieren Sie Postman für Azure Media Services-REST-API-Aufrufe](media-rest-apis-with-postman.md).

    Befolgen Sie dabei unbedingt den letzten Schritt im Thema [Abrufen von Azure AD-Token](media-rest-apis-with-postman.md#get-azure-ad-token). 

## <a name="define-a-filter"></a>Definieren eines Filters  

Das folgende Beispiel für einen **Anforderungstext** definiert die Titelauswahlbedingungen, die dem Manifest hinzugefügt werden. Dieser Filter bezieht alle EC-3-Audiotitel und alle Videotitel mit Bitraten im Bereich von 0–1000000 ein.

```json
{
    "properties": {
        "tracks": [
          {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Audio",
                        "operation": "Equal"
                    },
                    {
                        "property": "FourCC",
                        "value": "EC-3",
                        "operation": "Equal"
                    }
                ]
            },
            {
                "trackSelections": [
                    {
                        "property": "Type",
                        "value": "Video",
                        "operation": "Equal"
                    },
                    {
                        "property": "Bitrate",
                        "value": "0-1000000",
                        "operation": "Equal"
                    }
                ]
            }
        ]
     }
}
```

## <a name="create-account-filters"></a>Erstellen von Kontofiltern

Wählen Sie in der heruntergeladenen Sammlung von Postman **Account Filters (Kontofilter)** ->**Create or update an Account Filter (Kontofilter erstellen oder aktualisieren** aus.

Die **PUT**-HTTP-Anforderungsmethode ähnelt der folgenden:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/accountFilters/{filterName}?api-version=2018-07-01
```

Wählen Sie die Registerkarte **Text** aus, und fügen Sie den JSON-Code ein, den Sie [zuvor definiert haben](#define-a-filter).

Wählen Sie **Senden** aus. 

Der Filter wurde erstellt.

Weitere Informationen finden Sie unter [Account Filters – Create Or Update](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate) (Erstellen oder Aktualisieren von Kontofiltern). Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/accountfilters/createorupdate#create-an-account-filter).

## <a name="create-asset-filters"></a>Erstellen von Medienobjektfiltern  

Wählen Sie in der heruntergeladenen Postman-Sammlung „Media Services v3“ **Medienobjekte**->**Medienobjektfilter erstellen oder aktualisieren** aus.

Die **PUT**-HTTP-Anforderungsmethode ähnelt der folgenden:

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{accountName}/assets/{assetName}/assetFilters/{filterName}?api-version=2018-07-01
```

Wählen Sie die Registerkarte **Text** aus, und fügen Sie den JSON-Code ein, den Sie [zuvor definiert haben](#define-a-filter).

Wählen Sie **Senden** aus. 

Der Medienobjektfilter wurde erstellt.

Ausführliche Informationen zum Erstellen oder Aktualisieren von Medienobjektfiltern finden Sie unter [Erstellen und Aktualisieren](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate). Siehe auch [JSON-Beispiele für Filter](https://docs.microsoft.com/rest/api/media/assetfilters/createorupdate#create-an-asset-filter). 

## <a name="associate-filters-with-streaming-locator"></a>Zuordnen von Filtern mit Streaminglocator

Sie können eine Liste von Medienobjekt- oder Kontenfiltern angeben, die für Ihren Streaminglocator gelten würden. Der [dynamische Paketerstellungs-Manager (Streamingendpunkt)](dynamic-packaging-overview.md) wendet diese Liste der Filter zusammen mit den Filtern an, die Ihr Client in der URL angibt. Diese Kombination generiert ein [dynamisches Manifest](filters-dynamic-manifest-overview.md), das auf Filtern in den URL und Filtern basiert, die Sie im Streaminglocator angeben. Es wird empfohlen, dieses Feature zu verwenden, wenn Sie Filter anwenden, aber nicht die Filternamen in der URL verfügbar machen möchten.

Um Filter mit einem Streaminglocator mit REST zu erstellen und zuzuordnen, verwenden Sie die API [Streaminglocators – Erstellen](https://docs.microsoft.com/rest/api/media/streaminglocators/create) und geben Sie `properties.filters` im [Anforderungstext](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body) an.
                                
## <a name="stream-using-filters"></a>Streamen unter Verwendung von Filtern

Sobald Sie Filter definiert haben, können Ihre Kunden diese in der Streaming-URL verwenden. Auf Streamingprotokolle mit adaptiver Bitrate können Filter angewandt werden: Apple HTTP Live Streaming (HLS), MPEG-DASH und Smooth Streaming.

Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protocol|Beispiel|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Nächste Schritte

[Streamen von Videos](stream-files-tutorial-with-rest.md) 
