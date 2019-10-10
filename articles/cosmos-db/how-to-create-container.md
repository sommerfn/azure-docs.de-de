---
title: Erstellen eines Containers in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie einen Container in Azure Cosmos DB erstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 911f56cef58e5d9c161a3ba947850bcfff073200
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242408"
---
# <a name="create-an-azure-cosmos-container"></a>Erstellen eines Azure Cosmos-Containers

In diesem Artikel werden die verschiedenen Möglichkeiten zur Erstellung eines Azure Cosmos-Containers (Sammlung, Tabelle oder Diagramm) erläutert. Sie können dazu das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder unterstützte SDKs verwenden. In diesem Artikel erfahren Sie, wie Sie einen Container erstellen, den Partitionsschlüssel angeben und den Durchsatz bereitstellen.

## <a name="create-a-container-using-azure-portal"></a>Erstellen eines Containers über das Azure-Portal

### <a id="portal-sql"></a>SQL-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neuer Container** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Container-ID ein.
   * Geben Sie einen Partitionsschlüssel ein.
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot des Bereichs „Daten-Explorer“ mit hervorgehobener Option „Neuer Container“](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB-API für MongoDB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-mongodb-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neuer Container** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Container-ID ein.
   * Geben Sie einen Shardschlüssel ein.
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot der Azure Cosmos DB-API für MongoDB, Dialogfeld „Container hinzufügen“](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-cassandra-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie einen neuen Keyspace erstellen oder einen vorhandenen Keyspace verwenden.
   * Geben Sie einen Tabellennamen ein.
   * Geben Sie die Eigenschaften ein, und geben Sie einen Primärschlüssel an.
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot der Cassandra-API, Dialogfeld „Tabelle hinzufügen“](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Bei der Cassandra-API wird der Primärschlüssel als Partitionsschlüssel verwendet.

### <a id="portal-gremlin"></a>Gremlin-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-graph-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **New Graph** (Neues Diagramm) aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie an, ob Sie eine neue Datenbank erstellen oder eine vorhandene Datenbank verwenden.
   * Geben Sie eine Diagramm-ID ein.
   * Wählen Sie für die Speicherkapazität die Option **Unbegrenzt** aus.
   * Geben Sie einen Partitionsschlüssel für Vertices ein.
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot der Gremlin-API, Dialogfeld „Diagramm hinzufügen“](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Tabellen-API

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-table-dotnet.md#create-a-database-account), oder wählen Sie ein bereits vorhandenes Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Tabelle** aus. Geben Sie anschließend die folgenden Details an:

   * Geben Sie eine Tabellen-ID ein.
   * Geben Sie den bereitzustellenden Durchsatz an (etwa 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot der Tabellen-API, Dialogfeld „Tabelle hinzufügen“](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Bei der Tabellen-API wird der Partitionsschlüssel jedes Mal angegeben, wenn Sie eine neue Zeile hinzufügen.

## Erstellen eines Containers mithilfe der Azure CLI<a id="cli-sql"></a><a id="cli-mongodb"></a><a id="cli-cassandra"></a><a id="cli-gremlin"></a><a id="cli-table"></a>

Über die folgenden Links erfahren Sie, wie Sie Containerressourcen für Azure Cosmos DB mithilfe der Azure CLI erstellen.

Eine Liste aller Azure CLI-Beispiele für alle Azure Cosmos DB-APIs finden Sie unter [SQL-API](cli-samples.md), [Cassandra-API](cli-samples-cassandra.md), [MongoDB-API](cli-samples-mongodb.md), [Gremlin-API](cli-samples-gremlin.md) und [Tabellen-API](cli-samples-table.md).

* [Erstellen eines Containers mit der Azure CLI](manage-with-cli.md#create-a-container)
* [Erstellen einer Sammlung für Azure Cosmos DB für die MongoDB-API mit der Azure CLI](./scripts/cli/mongodb/create.md)
* [Erstellen einer Cassandra-Tabelle mit der Azure CLI](./scripts/cli/cassandra/create.md)
* [Erstellen eines Gremlin-Graph mit der Azure CLI](./scripts/cli/gremlin/create.md)
* [Erstellen einer Tabellen-API mit der Azure CLI](./scripts/cli/table/create.md)

## Erstellen eines Containers mithilfe von PowerShell<a id="ps-sql"></a><a id="ps-mongodb"><a id="ps-cassandra"></a><a id="ps-gremlin"><a id="ps-table"></a>

Über die folgenden Links erfahren Sie, wie Sie Containerressourcen für Azure Cosmos DB mithilfe von PowerShell erstellen.

Eine Liste aller Azure CLI-Beispiele für alle Azure Cosmos DB-APIs finden Sie unter [SQL-API](powershell-samples-sql.md), [Cassandra-API](powershell-samples-cassandra.md), [MongoDB-API](powershell-samples-mongodb.md), [Gremlin-API](powershell-samples-gremlin.md) und [Tabellen-API](powershell-samples-table.md).

* [Erstellen eines Containers mit PowerShell](manage-with-powershell.md#create-container)
* [Erstellen einer Sammlung für Azure Cosmos DB für die MongoDB-API mit PowerShell](./scripts/powershell/mongodb/ps-mongodb-create.md)
* [Erstellen einer Cassandra-Tabelle mit PowerShell](./scripts/powershell/cassandra/ps-cassandra-create.md)
* [Erstellen eines Gremlin-Graph mit PowerShell](./scripts/powershell/gremlin/ps-gremlin-create.md)
* [Erstellen einer Tabellen-API-Tabelle mit PowerShell](./scripts/powershell/table/ps-table-create.md)

## <a name="create-a-container-using-net-sdk"></a>Erstellen eines Containers mithilfe des .NET SDK

### <a id="dotnet-sql-graph"></a>SQL-API und Gremlin-API

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>Azure Cosmos DB-API für MongoDB

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB Wire Protocol versteht das Konzept für [Anforderungseinheiten](request-units.md) nicht. Verwenden Sie zum Erstellen einer neuen Sammlung mit bereitgestelltem Durchsatz das Azure-Portal oder Cosmos DB SDKs für die SQL-API.

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Nächste Schritte

* [Partitioning in Azure Cosmos DB](partitioning-overview.md) (Partitionierung in Azure Cosmos DB)
* [Anforderungseinheiten in Azure Cosmos DB](request-units.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Arbeiten mit einem Azure Cosmos-Konto](account-overview.md)
