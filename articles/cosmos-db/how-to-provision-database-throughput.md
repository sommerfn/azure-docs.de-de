---
title: Bereitstellen des Datenbankdurchsatzes in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie in Azure Cosmos DB Durchsatz auf Datenbankebene bereitstellen.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 93961b44f1c0d063774395ab384cb84b1aa05d99
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71812493"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos DB

In diesem Artikel wird erläutert, wie Sie Durchsatz für eine Datenbank in Azure Cosmos DB bereitstellen. Sie können Durchsatz für einen einzelnen [Container](how-to-provision-container-throughput.md) oder für eine Datenbank bereitstellen und den Durchsatz für die darin enthaltenen Container freigeben. Informationen dazu, wann Durchsatz auf Containerebene und wann auf Datenbankebene verwendet wird, finden Sie im Artikel [Anwendungsfälle für die Bereitstellung von Durchsatz für Container und Datenbanken](set-throughput.md). Durchsatz auf Datenbankebene kann über das Azure-Portal oder über Azure Cosmos DB SDKs bereitgestellt werden.

## <a name="provision-throughput-using-azure-portal"></a>Bereitstellen des Durchsatzes mithilfe des Azure-Portals

### <a id="portal-sql"></a>SQL-API (Core-API)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. [Erstellen Sie ein neues Azure Cosmos-Konto](create-sql-api-dotnet.md#create-account), oder wählen Sie ein bereits vorhandenes Azure Cosmos-Konto aus.

1. Öffnen Sie den Bereich **Daten-Explorer**, und wählen Sie **Neue Datenbank** aus. Geben Sie die folgenden Details an:

   * Geben Sie eine Datenbank-ID ein.
   * Aktivieren Sie das Kontrollkästchen **Durchsatz bereitstellen**.
   * Geben Sie einen Durchsatz ein (beispielsweise 1.000 RUs).
   * Klicken Sie auf **OK**.

    ![Screenshot: Dialogfeld „Neue Datenbank“](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Bereitstellen von Durchsatz über Azure CLI oder PowerShell

Weitere Informationen zum Erstellen einer Datenbank mit gemeinsamem Durchsatz finden Sie unter

* [Erstellen einer Datenbank über Azure CLI](manage-with-cli.md#create-a-database-with-shared-throughput)
* [Erstellen einer Datenbank über PowerShell](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>Bereitstellen des Durchsatzes mithilfe des .NET SDK

> [!Note]
> Sie können Cosmos SDKs für die SQL-API verwenden, um Durchsatz für alle APIs bereitzustellen. Sie können optional auch das folgende Beispiel für die Cassandra-API verwenden.

### <a id="dotnet-all"></a>Alle APIs

### <a name="net-v2-sdk"></a>.Net V2 SDK

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a name="net-v3-sdk"></a>.Net V3 SDK

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

### <a id="dotnet-cassandra"></a>Cassandra-API

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400);
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Bereitstellen des Durchsatzes in Azure Cosmos DB finden Sie in den folgenden Artikeln:

* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md)
* [Provision throughput for an Azure Cosmos DB container](how-to-provision-container-throughput.md) (Bereitstellen des Durchsatzes für einen Azure Cosmos DB-Container)
* [Durchsatz und Anforderungseinheiten in Azure Cosmos DB](request-units.md)
