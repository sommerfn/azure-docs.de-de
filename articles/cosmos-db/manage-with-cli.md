---
title: Verwalten von Azure Cosmos DB-Ressourcen mit der Azure CLI
description: Verwenden Sie die Azure CLI zum Verwalten Ihrer Konten, Datenbanken und Containern für Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: f9d8bf9161343e4b36a3c16209873962b69d8af5
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615212"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Verwalten von Azure Cosmos-Ressourcen mit der Azure CLI

Der folgende Leitfaden erläutert die gängigen Befehle zum Automatisieren der Verwaltung Ihrer Azure Cosmos DB-Konten und -Container mithilfe von Azure CLI. Referenzseiten für alle CLI-Befehle für Azure Cosmos DB sind in der [Referenz zur Azure CLI](https://docs.microsoft.com/cli/azure/cosmosdb) verfügbar. Weitere Beispiele finden Sie auch in [Azur CLI-Beispiele für Azure Cosmos DB](cli-samples.md), einschließlich der Erstellung und Verwaltung von Cosmos DB-Konten, Datenbanken und Containern für MongoDB, Gremlin, Cassandra und die Tabellen-API.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI in Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

Zum Erstellen eines Azure Cosmos DB-Kontos mit SQL-API und Sitzungskonsistenz für die Regionen „USA, Osten“ und „USA, Westen“ führen Sie den folgenden Befehl aus:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount # must be lowercase and < 31 characters \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=EastUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=WestUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> Der Name des Azure Cosmos-Kontos muss in Kleinbuchstaben geschrieben sein.

## <a name="create-a-database"></a>Erstellen einer Datenbank

Führen Sie den folgenden Befehl aus, um eine Cosmos-Datenbankinstanz zu erstellen:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Erstellen eines Containers

Um einen Cosmos-Container mit 400 RU/s und einem Partitionsschlüssel zu erstellen, führen Sie den folgenden Befehl aus:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>Ändern des Durchsatzes eines Containers

Um den Durchsatz eines Cosmos-Containers mit 1000 RU/s zu ändern, führen Sie den folgenden Befehl aus:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Auflisten von Kontoschlüsseln

Um die Schlüssel für Ihr Cosmos-Konto abzurufen, führen Sie den folgenden Befehl aus:

```azurecli-interactive
# List account keys
az cosmosdb keys list \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Auflisten von Verbindungszeichenfolgen

Um die Verbindungszeichenfolgen für Ihr Cosmos-Konto abzurufen, führen Sie den folgenden Befehl aus:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Erneutes Generieren eines Kontoschlüssels

Um einen Primärschlüssel für Ihr Cosmos-Konto erneut zu generieren, führen Sie den folgenden Befehl aus:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in:

- [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)
- [Azure-CLI-Referenz](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Zusätzliche Azure CLI-Beispiele für Azure Cosmos DB](cli-samples.md)
