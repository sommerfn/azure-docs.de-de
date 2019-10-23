---
title: Implementieren der benutzerdefinierten Synchronisierung zur Optimierung der Verfügbarkeit und Leistung in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie zur Optimierung der Verfügbarkeit und Leistung in Azure Cosmos DB die benutzerdefinierte Synchronisierung implementieren.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 1fdd05f8a4757a49414a2a03c8f991a80186ed44
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755046"
---
# <a name="implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Implementieren der benutzerdefinierten Synchronisierung zur Optimierung der Verfügbarkeit und Leistung

In Azure Cosmos DB stehen [fünf klar definierte Konsistenzebenen](consistency-levels.md) zur Auswahl, mit denen Sie Konsistenz, Leistung und Verfügbarkeit optimal aufeinander abstimmen können. Mit einer hohen Konsistenz lässt sich sicherstellen, dass Daten synchron repliziert und dauerhaft in jeder Region gespeichert werden, in der das Azure Cosmos-Konto verfügbar ist. Diese Konfiguration bietet zwar ein Höchstmaß an Dauerhaftigkeit, sie geht aber zulasten der Leistung und der Verfügbarkeit. Soll Ihre Anwendung die Dauerhaftigkeit von Daten steuern oder lockern, um den Anforderungen der Anwendung zu genügen, ohne die Verfügbarkeit zu beeinträchtigen, können Sie *benutzerdefinierte Synchronisierung* auf der Anwendungsebene verwenden, um das von Ihnen gewünschte Maß an Dauerhaftigkeit zu erreichen.

Die folgende Abbildung veranschaulicht das Modell der benutzerdefinierten Synchronisierung:

![Benutzerdefinierte Synchronisierung](./media/how-to-custom-synchronization/custom-synchronization.png)

In diesem Szenario wird ein Azure Cosmos-Container global in mehreren Regionen auf mehreren Kontinenten repliziert. Die in diesem Szenario für alle Regionen verwendete hohe Konsistenz wirkt sich auf die Leistung aus. Um für die Daten ein höheres Maß an Dauerhaftigkeit zu gewährleisten, ohne die Wartezeit bei Schreibvorgängen zu erhöhen, kann die Anwendung zwei Clients mit demselben [Sitzungstoken](how-to-manage-consistency.md#utilize-session-tokens) verwenden.

Der erste Client kann Daten in der lokalen Region (beispielsweise „USA, Westen“) schreiben. Der zweite Client (beispielsweise in der Region „USA, Osten“) ist ein Leseclient, der dazu verwendet wird, Synchronisierung sicherzustellen. Durch die Weitergabe des Sitzungstokens von der Schreibantwort an den nachfolgenden Lesevorgang stellt der Lesevorgang die Synchronisierung von Schreibvorgängen in der Region „USA, Osten“ sicher. Azure Cosmos DB stellt sicher, dass Schreibvorgänge in mindestens einer Region registriert werden. Es ist garantiert, dass sie einen regionalen Ausfall überstehen, wenn die ursprüngliche Schreibregion ausfällt. In diesem Szenario wird jeder Schreibvorgang mit „USA, Osten“ synchronisiert, wodurch die Wartezeit für die Nutzung von hoher Konsistenz in allen Regionen verringert wird. In einem Multimasterszenario, in dem es Schreibvorgänge in jeder Region gibt, können Sie dieses Modell so erweitern, dass in mehreren Regionen parallel synchronisiert wird.

## <a name="configure-the-clients"></a>Konfigurieren der Clients

Im folgenden Beispiel ist eine Datenzugriffsschicht gezeigt, für die zwei Clients für die benutzerdefinierte Synchronisierung instanziiert werden:

### <a name="net-v2-sdk"></a>.NET V2 SDK
```csharp
class MyDataAccessLayer
{
    private DocumentClient writeClient;
    private DocumentClient readClient;

    public async Task InitializeAsync(Uri accountEndpoint, string key)
    {
        ConnectionPolicy writeConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp,
            UseMultipleWriteLocations = true
        };
        writeConnectionPolicy.SetCurrentLocation(LocationNames.WestUS);

        ConnectionPolicy readConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        };
        readConnectionPolicy.SetCurrentLocation(LocationNames.EastUS);

        writeClient = new DocumentClient(accountEndpoint, key, writeConnectionPolicy);
        readClient = new DocumentClient(accountEndpoint, key, readConnectionPolicy, ConsistencyLevel.Session);

        await Task.WhenAll(new Task[]
        {
            writeClient.OpenAsync(),
            readClient.OpenAsync()
        });
    }
}
```

### <a name="net-v3-sdk"></a>.NET V3 SDK
```csharp
class MyDataAccessLayer
{
    private CosmosClient writeClient;
    private CosmosClient readClient;

    public void InitializeAsync(string accountEndpoint, string key)
    {
        CosmosClientOptions writeConnectionOptions = new CosmosClientOptions();
        writeConnectionOptions.ConnectionMode = ConnectionMode.Direct;
        writeConnectionOptions.ApplicationRegion = "West US";

        CosmosClientOptions readConnectionOptions = new CosmosClientOptions();
        readConnectionOptions.ConnectionMode = ConnectionMode.Direct;
        readConnectionOptions.ApplicationRegion = "East US";


        writeClient = new CosmosClient(accountEndpoint, key, writeConnectionOptions);
        readClient = new CosmosClient(accountEndpoint, key, readConnectionOptions);
    }
}
```

## <a name="implement-custom-synchronization"></a>Implementieren der benutzerdefinierten Synchronisierung

Nachdem die Clients initialisiert wurden, kann die Anwendung Schreibvorgänge in der lokalen Region (USA, Westen) ausführen und wie folgt das Synchronisieren der Schreibvorgänge in der Region „USA, Osten“ erzwingen.

### <a name="net-v2-sdk"></a>.NET V2 SDK
```csharp
class MyDataAccessLayer
{
    public async Task CreateItem(string containerLink, Document document)
    {
        ResourceResponse<Document> response = await writeClient.CreateDocumentAsync(
            containerLink, document);

        await readClient.ReadDocumentAsync(response.Resource.SelfLink,
            new RequestOptions { SessionToken = response.SessionToken });
    }
}
```

### <a name="net-v3-sdk"></a>.NET V3 SDK
```csharp
class MyDataAccessLayer
{
     public async Task CreateItem(string databaseId, string containerId, dynamic item)
     {
        ItemResponse<dynamic> response = await writeClient.GetContainer("containerId", "databaseId")
            .CreateItemAsync<dynamic>(
                item,
                new PartitionKey("test"));

        await readClient.GetContainer("containerId", "databaseId").ReadItemAsync<dynamic>(
            response.Resource.id,
            new PartitionKey("test"),
            new ItemRequestOptions { SessionToken = response.Headers.Session, ConsistencyLevel = ConsistencyLevel.Session });
    }
}
```

Sie können das Modell so erweitern, dass parallel in mehreren Regionen synchronisiert wird.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel, um mehr über die globale Verteilung und Konsistenz in Azure Cosmos DB zu erfahren:

* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)
* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)
* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md)
* [Partitionierung und horizontale Skalierung in Azure Cosmos DB](partition-data.md)
