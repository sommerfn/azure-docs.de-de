---
title: Konfigurieren von Multimaster-Features in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie Multimaster in Ihren Anwendungen in Azure Cosmos DB konfigurieren.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: e86cacbd76a70c8b114d65a77ff013d32327a2d0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70093100"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Konfigurieren von Multimaster in Ihren Anwendungen, die Azure Cosmos DB verwenden

Sobald ein Konto mit mehreren aktivierten Schreibbereichen erstellt wurde, müssen Sie in ConnectionPolicy für DocumentClient zwei Änderungen vornehmen, um die Multimaster- und Multihomingfunktionen in Azure Cosmos DB zu aktivieren. Legen Sie in ConnectionPolicy das UseMultipleWriteLocations-Element auf TRUE fest, und übergeben Sie den Namen der Region, in der die Anwendung bereitgestellt wird, an SetCurrentLocation. Dadurch wird die PreferredLocations-Eigenschaft basierend auf der geografischen Nähe zum eingegebenen Standort aufgefüllt. Wenn dem Konto später eine neue Region hinzugefügt wird, muss die Anwendung nicht aktualisiert oder neu bereitgestellt werden. Sie erkennt automatisch die nähere Region und greift automatisch darauf zurück, wenn ein regionales Ereignis eintritt.

> [!Note]
> Cosmos-Konten, die ursprünglich mit einer einzelnen Schreibregion konfiguriert wurden, können für die Verwendung mehrerer Schreibregionen (also als Multimaster) ohne Ausfallzeiten konfiguriert werden. Weitere Informationen finden unter [Konfigurieren mehrerer Schreibregionen](how-to-manage-database-account.md#configure-multiple-write-regions).

## <a id="netv2"></a>.NET SDK v2

Um Multimaster in Ihrer Anwendung zu aktivieren, legen Sie `UseMultipleWriteLocations` auf `true` fest. Legen Sie außerdem `SetCurrentLocation` auf die Region fest, in der die Anwendung bereitgestellt wird und wo Azure Cosmos DB repliziert wird:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3

Um Multimaster in Ihrer Anwendung zu aktivieren, legen Sie `ApplicationRegion` auf die Region fest, in der die Anwendung bereitgestellt und wo Cosmos DB repliziert wird:

```csharp
CosmosClient cosmosClient = new CosmosClient(
    "<connection-string-from-portal>", 
    new CosmosClientOptions()
    {
        ApplicationRegion = Regions.WestUS2,
    });
```

Optional können Sie `CosmosClientBuilder` und `WithApplicationRegion` verwenden, um das gleiche Ergebnis zu erzielen:

```csharp
CosmosClientBuilder cosmosClientBuilder = new CosmosClientBuilder("<connection-string-from-portal>")
            .WithApplicationRegion(Regions.WestUS2);
CosmosClient client = cosmosClientBuilder.Build();
```

## <a id="java"></a>Java Async SDK

Um Multimaster in Ihrer Anwendung zu aktivieren, legen Sie `policy.setUsingMultipleWriteLocations(true)` fest, und legen Sie `policy.setPreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt und wo Cosmos DB repliziert wird:

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>Node.js, JavaScript und TypeScript SDK

Um Multimaster in Ihrer Anwendung zu aktivieren, legen Sie `connectionPolicy.UseMultipleWriteLocations` auf `true` fest. Legen Sie außerdem `connectionPolicy.PreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt wird und wo Cosmos DB repliziert wird:

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>Python SDK

Um Multimaster in Ihrer Anwendung zu aktivieren, legen Sie `connection_policy.UseMultipleWriteLocations` auf `true` fest. Legen Sie außerdem `connection_policy.PreferredLocations` auf die Region fest, in der die Anwendung bereitgestellt wird und wo Cosmos DB repliziert wird.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {
                                    'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel:

* [Verwalten von Konsistenzebenen in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Konflikttypen und Konfliktauflösungsrichtlinien](conflict-resolution-policies.md)
* [Hochverfügbarkeit mit Azure Cosmos DB](high-availability.md)
* [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)
* [Auswählen der richtigen Konsistenzebene](consistency-levels-choosing.md)
* [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md)
* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Globale Verteilung: Im Hintergrund](global-dist-under-the-hood.md)
