---
title: Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto
description: Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: b7b6be0ce781debcb19b5c0fb7b6a4b0123ef366
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275107"
---
# <a name="add-regions-change-failover-priority-trigger-failover-for-an-azure-cosmos-account-using-azure-cli"></a>Hinzufügen von Regionen, Anpassen der Failoverpriorität, Auslösen des Failovers für ein Azure Cosmos-Konto mithilfe der Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.0.73 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

Dieses Skript veranschaulicht drei Vorgänge.

- Hinzufügen einer Region zu einem vorhandenen Azure Cosmos-Konto
- Ändern der Priorität des regionalen Failovers (gilt für Konten mit automatischem Failover)
- Auslösen eines manuelles Failovers von der primären zur sekundären Region aus (gilt für Konten mit manuellem Failover).

> [!NOTE]
> Während andere Eigenschaften geändert werden, können in einem Cosmos-Konto keine Regionsvorgänge hinzugefügt oder entfernt werden.

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos (Core), aber diese Vorgänge sind für alle Datenbank-APIs in Cosmos DB identisch.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/regions.sh "Regional operations for Cosmos DB.")]

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
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellt ein Konto für Azure Cosmos DB |
| [az cosmosdb update](/cli/azure/cosmosdb#az-cosmosdb-update) | Aktualisiert ein Azure Cosmos DB-Konto (Hinzufügen oder Entfernen einer Region). |
| [az cosmosdb failover-priority-change](/cli/azure/cosmosdb#az-cosmosdb-failover-priority-change) | Aktualisiert die Failoverpriorität oder löst ein Failover für ein Azure Cosmos DB-Konto aus. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB CLI finden Sie in der [Dokumentation zur Azure Cosmos DB CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
