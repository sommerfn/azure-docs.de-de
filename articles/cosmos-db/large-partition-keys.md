---
title: Erstellen von Azure Cosmos DB-Containern mit großen Partitionsschlüsseln über das Azure-Portal und mithilfe verschiedener SDKs
description: Erfahren Sie, wie Sie einen Container in Azure Cosmos DB mit einem großen Partitionsschlüssel über das Azure-Portal und mithilfe verschiedener SDKs erstellen können.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: be639a67f70ab40f8d7dcc0f3793cbbd4a2ec4a3
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72436809"
---
# <a name="create-containers-with-large-partition-key"></a>Erstellen von Containern mit großen Partitionsschlüsseln

Azure Cosmos DB verwendet hashbasierte Partitionsschemas, um Daten horizontal skalieren zu können. Bei allen Azure Cosmos Containern, die vor dem 3. Mai 2019 erstellt wurden, kommt eine Hashfunktion zum Einsatz, die den Hash mit Hilfe der ersten 100 Bytes des Partitionsschlüssels ermittelt. Wenn es mehrere Partitionsschlüssel gibt, deren ersten 100 Byte übereinstimmen, werden diese logischen Partitionen vom Dienst als dieselbe logische Partition angesehen. Das kann zu Problemen führen. Z. B. kann es sein, dass das Partitionsgrößenkontingent nicht richtig ist oder dass eindeutige Indizes für alle Partitionsschlüssel angewendet werden. Zur Behebung dieses Problems werden große Partitionsschlüssel eingeführt. Azure Cosmos DB unterstützt jetzt große Partitionsschlüssel mit einer Größe von bis zu 2 KB.

Große Partitionsschlüssel werden durch eine Funktionalität einer verbesserten Version der Hashfunktion unterstützt, durch die ein eindeutiger Hash für große Partitionsschlüssel mit einer Größe von bis zu 2 KB generiert werden kann. Diese Hashversion wird auch für Szenarios mit hoher Partitionsschlüsselkardinalität unabhängig von der Größe des Partitionsschlüssels empfohlen. Die Partitionsschlüsselkardinalität wird als Anzahl der eindeutigen logischen Partitionen definiert, z. B. ~30.000 logische Partitionen in einem Container. In diesem Artikel wird beschrieben, wie Sie einen Container in Azure Cosmos DB mit einem großen Partitionsschlüssel über das Azure-Portal und mithilfe verschiedener SDKs erstellen können.

## <a name="create-a-large-partition-key-azure-portal"></a>Erstellen eines großen Partitionsschlüssels (Azure-Portal)

Aktivieren Sie beim Erstellen eines neuen Containers über das Azure-Portal die Option **Mein Partitionsschlüssel ist größer als 100 Byte**, um einen großen Partitionsschlüssel zu erstellen. Deaktivieren Sie das Kontrollkästchen, wenn Sie keine großen Partitionsschlüssel benötigen oder wenn Sie Anwendungen verwenden, die mit SDK-Versionen vor 1.18 ausgeführt werden.

![Erstellen eines großen Partitionsschlüssels über das Azure-Portal](./media/large-partition-keys/large-partition-key-with-portal.png)

## <a name="create-a-large-partition-key-powershell"></a>Erstellen eines großen Partitionsschlüssels (PowerShell)

Informationen zum Erstellen eines Containers mit Unterstützung für große Partitionsschlüssel finden Sie unter

* [Erstellen eines Azure Cosmos DB-Containers mit einem großen Partitionsschlüssel](manage-with-powershell.md##create-container-big-pk)

## <a name="create-a-large-partition-key-net-sdk"></a>Erstellen eines großen Partitionsschlüssels (.NET SDK)

Wenn Sie mithilfe des .NET SDK einen Container mit einem großen Partitionsschlüssel erstellen möchten, geben Sie die Eigenschaft `PartitionKeyDefinitionVersion.V2` an. Im folgenden Beispiel wird veranschaulicht, wie Sie die Versionseigenschaft im Objekt „PartitionKeyDefinition“ angeben und auf „PartitionKeyDefinitionVersion.V2“ festlegen:

### <a name="v3-net-sdk"></a>v3 .NET SDK

```csharp
await database.CreateContainerAsync(
    new ContainerProperties(collectionName, $"/longpartitionkey")
    {
        PartitionKeyDefinitionVersion = PartitionKeyDefinitionVersion.V2,
    })
```

### <a name="v2-net-sdk"></a>v2 .NET SDK

```csharp
DocumentCollection collection = await newClient.CreateDocumentCollectionAsync(
database,
     new DocumentCollection
        {
           Id = Guid.NewGuid().ToString(),
           PartitionKey = new PartitionKeyDefinition
           {
             Paths = new Collection<string> {"/longpartitionkey" },
             Version = PartitionKeyDefinitionVersion.V2
           }
         },
      new RequestOptions { OfferThroughput = 400 });
```

## <a name="supported-sdk-versions"></a>Unterstützte SDK-Versionen

Die Schlüssel für große Partitionen werden mit den folgenden Mindestversionen von SDKs unterstützt:

|SDK-Typ  | Mindestversion   |
|---------|---------|
|.Net     |    1.18     |
|Java sync     |   2.4.0      |
|Java Async   |  2.5.0        |
| REST-API | Version höher als `2017-05-03` unter Verwendung des Anforderungsheaders `x-ms-version`.|

Derzeit können Sie in Power BI und Azure Logic Apps keine Container mit großem Partitionsschlüssel verwenden. Sie können Container ohne große Partitionsschlüssel in diesen Anwendungen verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Partitioning in Azure Cosmos DB](partitioning-overview.md) (Partitionierung in Azure Cosmos DB)
* [Anforderungseinheiten in Azure Cosmos DB](request-units.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Arbeiten mit einem Azure Cosmos-Konto](account-overview.md)
