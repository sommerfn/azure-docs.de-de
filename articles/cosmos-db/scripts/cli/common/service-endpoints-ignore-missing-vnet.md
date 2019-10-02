---
title: Verbinden eines vorhandenen Azure Cosmos-Kontos mit VNET-Dienstendpunkten
description: Verbinden eines vorhandenen Azure Cosmos-Kontos mit VNET-Dienstendpunkten
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 46e93e864034c451e1da1848a318ab176a292b6e
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275127"
---
# <a name="connect-an-existing-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Verbinden eines vorhandenen Azure Cosmos-Kontos mit VNET-Dienstendpunkten mithilfe der Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für dieses Thema erforderlich, die Azure CLI-Version 2.0.73 oder höher auszuführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel soll veranschaulichen, wie Sie ein vorhandenes Azure Cosmos-Konto mithilfe des `ignore-missing-vnet-service-endpoint`-Parameters mit einem vorhandenen neuen virtuellen Netzwerk verbinden, in dem das Subnetz noch nicht für Dienstendpunkte konfiguriert ist. Dadurch kann die Konfiguration für das Cosmos-Konto ohne Fehler abgeschlossen werden, bevor die Konfiguration des Subnetzes des virtuellen Netzwerks abgeschlossen ist. Sobald die Subnetzkonfiguration abgeschlossen ist, kann über das konfigurierte Subnetz auf das Cosmos-Konto zugegriffen werden.

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines SQL-API-Kontos (Core-API). Wenn Sie dieses Beispiel für andere APIs verwenden möchten, wenden Sie die Parameter `enable-virtual-network` und `virtual-network-rules` im folgenden Skript auf das API-spezifische Skript an.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints-ignore-missing-vnet.sh "Create an Azure Cosmos account with service endpoints.")]

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
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Erstellt ein virtuelles Azure-Netzwerk. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Erstellt ein Subnetz für ein virtuelles Azure-Netzwerk. |
| [az network vnet subnet show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Gibt ein Subnetz für ein virtuelles Azure-Netzwerk zurück. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellt ein Konto für Azure Cosmos DB. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) | Aktualisiert ein Subnetz für ein virtuelles Azure-Netzwerk. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB CLI finden Sie in der [Dokumentation zur Azure Cosmos DB CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
