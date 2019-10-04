---
title: Verwenden von Kontoschlüsseln und Verbindungszeichenfolgen für ein Azure Cosmos-Konto
description: Verwenden von Kontoschlüsseln und Verbindungszeichenfolgen für ein Azure Cosmos-Konto
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: e766f52d729a4f916eefd2b148d926d929b4f540
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71275117"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>Verwenden von Kontoschlüsseln und Verbindungszeichenfolgen für ein Azure Cosmos-Konto mit der Azure-Befehlszeilenschnittstelle (Azure CLI)

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI Version 2.0.73 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

Dieses Skript veranschaulicht vier Vorgänge.

- Auflisten aller Kontoschlüssel
- Auflisten der schreibgeschützten Kontoschlüssel
- Auflisten von Verbindungszeichenfolgen
- Erneutes Generieren von Kontoschlüsseln

> [!NOTE]
> In diesem Beispiel wird die Verwendung eines SQL-API-Kontos (Core-API) veranschaulicht, die Vorgänge für Kontoschlüssel und Verbindungszeichenfolgen sind jedoch bei allen Datenbank-APIs in Cosmos DB gleich.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/keys.sh "Keys and connection string operations for Cosmos DB.")]

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
| [az cosmosdb keys list](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) | Listet die Schlüssel für ein Azure Cosmos DB-Konto auf. |
| [az cosmosdb list-read-only-keys](/cli/azure/cosmosdb#az-cosmosdb-list-read-only-keys) | Listet die schreibgeschützten Schlüssel für ein Azure Cosmos DB-Konto auf. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | Listet die Verbindungszeichenfolgen für ein Azure Cosmos DB-Konto auf. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | Generiert die Schlüssel für ein Azure Cosmos DB-Konto erneut. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
