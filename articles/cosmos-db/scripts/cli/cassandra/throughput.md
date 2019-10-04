---
title: Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für einen Cassandra-Keyspace und eine Cassandra-Tabelle für Azure Cosmos DB
description: Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für einen Cassandra-Keyspace und eine Cassandra-Tabelle für Azure Cosmos DB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 3615157543d826050b8adf0e6ae59bf62ad9cb1b
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827298"
---
# <a name="update-rus-for-a-cassandra-keyspace-and-table-using-azure-cli"></a>Aktualisieren der Anforderungseinheiten pro Sekunde (RU/s) für einen Cassandra-Keyspace und eine Cassandra-Tabelle mit der Azure-Befehlszeilenschnittstelle (Azure CLI)

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.0.73 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt einen Cassandra-Keyspace mit freigegebenem Durchsatz sowie eine Cassandra-Tabelle mit dediziertem Durchsatz und aktualisiert dann den Durchsatz für den Keyspace und die Tabelle.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/throughput.sh "Update RU/s for Cassandra keyspace and table.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellt ein Konto für Azure Cosmos DB. |
| [az cosmosdb cassandra keyspace create](/cli/azure/cosmosdb/cassandra/keyspace#az-cosmosdb-cassandra-keyspace-create) | Erstellt einen Azure Cosmos-Cassandra-Keyspace. |
| [az cosmosdb cassandra table create](/cli/azure/cosmosdb/cassandra/table#az-cosmosdb-cassandra-table-create) | Erstellt eine Azure Cosmos-Cassandra-Tabelle. |
| [az cosmosdb cassandra keyspace throughput update](/cli/azure/cosmosdb/cassandra/keyspace/throughput?view=azure-cli-latest#az-cosmosdb-cassandra-keyspace-throughput-update) | Aktualisiert die Anforderungseinheiten pro Sekunde (RU/s) für einen Azure Cosmos-Cassandra-Keyspace. |
| [az cosmosdb cassandra table throughput update](/cli/azure/cosmosdb/cassandra/table/throughput#az-cosmosdb-cassandra-table-throughput-update) | Aktualisiert die Anforderungseinheiten pro Sekunde (RU/s) für eine Azure Cosmos-Cassandra-Tabelle. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
