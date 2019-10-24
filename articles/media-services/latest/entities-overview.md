---
title: Filtern und Sortieren von Azure Media Services-Entitäten sowie Einteilen der Entitäten in Seiten| Microsoft-Dokumentation
description: Dieser Artikel erörtert die Filterung, Sortierung und Paginierung von Azure Media Services-Entitäten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/11/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed509ac8fea43a9c011bbbf76c1dc433cd78d43c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298951"
---
# <a name="filtering-ordering-paging-of-media-services-entities"></a>Filterung, Sortierung und Paginierung von Media Services-Entitäten

In diesem Thema werden die OData-Abfrageoptionen und die Paginierungsunterstützung behandelt, die beim Auflisten von Azure Media Services v3-Entitäten verfügbar sind.

## <a name="considerations"></a>Überlegungen

* Eigenschaften von Entitäten vom Datetime-Typ liegen immer im UTC-Format vor.
* Leerraum in der Abfragezeichenfolge muss vor dem Senden einer Anforderung URL-codiert werden.

## <a name="comparison-operators"></a>Vergleichsoperatoren

Sie können die folgenden Operatoren verwenden, um ein Feld mit einem konstanten Wert zu vergleichen:

Gleichheitsoperatoren:

- `eq`: Testet, ob ein Feld **gleich** einem konstanten Wert ist.
- `ne`: Testet, ob ein Feld **nicht gleich** einem konstanten Wert ist.

Bereichsoperatoren:

- `gt`: Testet, ob ein Feld **größer als** ein konstanter Wert ist.
- `lt`: Testet, ob ein Feld **kleiner als** ein konstanter Wert ist.
- `ge`: Testet, ob ein Feld **größer oder gleich** einem konstanten Wert ist.
- `le`: Testet, ob ein Feld **kleiner oder gleich** einem konstanten Wert ist.

## <a name="filter"></a>Filter

**$filter**: Verwenden Sie Filter, um einen OData-Filterparameter anzugeben, um nur die Objekte zu finden, die Sie interessieren.

Im folgenden REST-Beispiel wird nach der alternateId einer Ressource gefiltert:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$filter=properties/alternateId%20eq%20'unique identifier'
```

Im folgenden C#-Beispiel wird nach dem Erstellungsdatum der Ressource gefiltert:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```    

## <a name="order-by"></a>ORDER BY

**$orderby**: Verwenden Sie diese Klausel, um die zurückgegebenen Objekte anhand des angegebenen Parameters zu sortieren. Beispiel:    

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01$orderby=properties/created%20gt%202018-05-11T17:39:08.387Z
```

Um die Ergebnisse in auf- oder absteigender Reihenfolge zu sortieren, fügen Sie entweder `asc` oder `desc` getrennt durch ein Leerzeichen an den Feldnamen an. Beispiel: `$orderby properties/created desc`.

## <a name="skip-token"></a>Skiptoken

**$skiptoken**: Wenn die Antwort auf eine Abfrage viele Elemente enthält, gibt der Dienst den Skiptokenwert `@odata.nextLink` zurück, mit dem Sie die nächste Seite der Ergebnisse abrufen können. Auf diese Weise kann das gesamte Resultset paginiert werden.

Sie können in Media Services v3 die Seitengröße nicht konfigurieren. Die Seitengröße unterscheidet sich nach der Art der Entität, bitte entnehmen Sie Details den folgenden Einzelabschnitten.

Wenn während der Paginierung der Sammlung Entitäten erstellt oder gelöscht werden, werden die Änderungen in den zurückgegebenen Ergebnissen reflektiert (sofern sich diese Änderungen in dem Teil der Sammlung befinden, der nicht heruntergeladen wurde). 

> [!TIP]
> Verwenden Sie stets `nextLink` zum Aufzählen der Sammlung und keine bestimmte Seitengröße als Referenz.
>
> `nextLink` ist nur vorhanden, wenn es mehr als eine Seite mit Entitäten gibt.

Betrachten Sie das folgende Beispiel für die Verwendung von „$skiptoken“. Stellen Sie sicher, dass Sie *amstestaccount* durch Ihren Kontonamen ersetzen und den Wert für *api-version* auf die neueste Version festlegen.

Fordern Sie folgendermaßen eine Liste von Medienobjekten an:

```
GET  https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Sie erhalten eine Antwort, die etwa wie folgt aussieht:

```
HTTP/1.1 200 OK

{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-00000000-0000-0000-0000-0000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-00000000-0000-0000-0000-000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

Danach fordern Sie die nächste Seite an, indem Sie eine get-Anforderung senden:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Das folgende C#-Beispiel zeigt, wie alle Streaminglocators im Konto aufgezählt werden.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

## <a name="using-logical-operators-to-combine-query-options"></a>Verwenden logischer Operatoren zum Kombinieren von Abfrageoptionen

Media Services v3 unterstützt die logischen Operatoren „or“ und „and“. 

Im folgenden REST-Beispiel wird der Status des Auftrags überprüft:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/qbtest/providers/Microsoft.Media/mediaServices/qbtest/transforms/VideoAnalyzerTransform/jobs?$filter=properties/state%20eq%20Microsoft.Media.JobState'Scheduled'%20or%20properties/state%20eq%20Microsoft.Media.JobState'Processing'&api-version=2018-07-01
```

In C# erstellen Sie die gleiche Abfrage wie folgt: 

```csharp
var odataQuery = new ODataQuery<Job>("properties/state eq Microsoft.Media.JobState'Scheduled' or properties/state eq Microsoft.Media.JobState'Processing'");
client.Jobs.List(config.ResourceGroup, config.AccountName, VideoAnalyzerTransformName, odataQuery);
```

## <a name="filtering-and-ordering-options-of-entities"></a>Filter- und Sortieroptionen für Entitäten

Die folgende Tabelle zeigt, wie die Filter- und Sortieroptionen auf verschiedene Entitäten angewendet werden können:

|Name der Entität|Eigenschaftenname|Filter|Reihenfolge|
|---|---|---|---|
|[Medienobjekte](https://docs.microsoft.com/rest/api/media/assets/)|name|`eq`, `gt`, `lt`, `ge`, `le`|`asc` und `desc`|
||properties.alternateId |`eq`||
||properties.assetId |`eq`||
||properties.created| `eq`, `gt`, `lt`| `asc` und `desc`|
|[Richtlinien für Inhaltsschlüssel](https://docs.microsoft.com/rest/api/media/contentkeypolicies)|name|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.description    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`||
||properties.lastModified|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.policyId|`eq`, `ne`||
|[Aufträge](https://docs.microsoft.com/rest/api/media/jobs)| name  | `eq`            | `asc` und `desc`|
||properties.state        | `eq`, `ne`        |                         |
||properties.created      | `gt`, `ge`, `lt`, `le`| `asc` und `desc`|
||properties.lastModified | `gt`, `ge`, `lt`, `le` | `asc` und `desc`| 
|[Streaminglocators](https://docs.microsoft.com/rest/api/media/streaminglocators)|name|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.endTime    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
|[Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies)|name|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
||properties.created    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` und `desc`|
|[Transformationen](https://docs.microsoft.com/rest/api/media/transforms)| name | `eq`            | `asc` und `desc`|
|| properties.created      | `gt`, `ge`, `lt`, `le`| `asc` und `desc`|
|| properties.lastModified | `gt`, `ge`, `lt`, `le`| `asc` und `desc`|

## <a name="next-steps"></a>Nächste Schritte

* [Auflisten von Medienobjekten](https://docs.microsoft.com/rest/api/media/assets/list)
* [Auflisten von Richtlinien für Inhaltsschlüssel](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)
* [Auflisten von Aufträgen](https://docs.microsoft.com/rest/api/media/jobs/list)
* [Auflisten von Streamingrichtlinien](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)
* [Auflisten von Streaminglocators](https://docs.microsoft.com/rest/api/media/streaminglocators/list)
* [Streamen einer Datei](stream-files-dotnet-quickstart.md)
* [Kontingente und Einschränkungen](limits-quotas-constraints.md)
