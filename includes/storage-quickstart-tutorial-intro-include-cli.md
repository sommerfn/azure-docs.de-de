---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: 4ad977dc8cbaa85360092dbfd391a3c3b88f67bb
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747928"
---
## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group) eine Azure-Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az group create \
    --name <resource-group-name> \
    --location <location>
```

## <a name="create-a-storage-account"></a>Speicherkonto erstellen

Erstellen Sie mit dem Befehl [az storage account create](/cli/azure/storage/account) ein allgemeines Speicherkonto. Das allgemeine Speicherkonto kann für alle vier Dienste verwendet werden: Blobs, Files, Tables und Queues.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group <resource-group-name> \
    --location <location> \
    --sku Standard_ZRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Angeben der Anmeldeinformationen für das Speicherkonto

Für die meisten Befehle der Azure-Befehlszeilenschnittstelle, die in diesem Tutorial verwendet werden, werden die Anmeldeinformationen für das Speicherkonto benötigt. Diese können auf unterschiedliche Weise angegeben werden. Eine der einfachsten Methoden besteht jedoch darin, die Umgebungsvariablen `AZURE_STORAGE_ACCOUNT` und `AZURE_STORAGE_KEY` festzulegen.

> [!NOTE]
> In diesem Artikel wird gezeigt, wie Sie Umgebungsvariablen mithilfe der Bash festlegen. Andere Umgebungen erfordern möglicherweise Syntaxanpassungen.

Zeigen Sie zunächst mithilfe des Befehls [az storage account keys list](/cli/azure/storage/account/keys) Ihre Speicherkontoschlüssel an. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```azurecli-interactive
az storage account keys list \
    --account-name <account-name> \
    --resource-group <resource-group-name> \
    --output table
```

Legen Sie dann die Umgebungsvariablen `AZURE_STORAGE_ACCOUNT` und `AZURE_STORAGE_KEY` fest. Hierzu können Sie in der Bash-Shell den Befehl `export` verwenden. Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```bash
export AZURE_STORAGE_ACCOUNT="<account-name>"
export AZURE_STORAGE_KEY="<account-key>"
```

Weitere Informationen zum Abrufen der Kontozugriffsschlüssel über das Azure-Portal finden Sie unter **Zugriffsschlüssel** in [Verwalten von Speicherkontoeinstellungen im Azure-Portal](../articles/storage/common/storage-account-manage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#access-keys).