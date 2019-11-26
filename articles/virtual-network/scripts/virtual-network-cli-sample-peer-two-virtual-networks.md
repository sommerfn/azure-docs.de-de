---
title: 'Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken: Azure CLI-Skriptbeispiel'
description: Azure CLI-Skriptbeispiel – Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 2dd5336d66872cc8c56fd372e89b67ce9c892f3a
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74083843"
---
# <a name="peer-two-virtual-networks-script-sample"></a>Skriptbeispiel für das Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken

Dieses Skriptbeispiel erstellt zwei virtuelle Netzwerke in der gleichen Region und stellt über das Azure-Netzwerk eine Verbindung zwischen ihnen her. Nach dem Ausführen des Skripts besteht ein Peering zwischen zwei virtuellen Netzwerken.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/bash) oder eine lokale Installation der Azure CLI ausführen. Wenn Sie die CLI lokal verwenden, muss für dieses Skript Version 2.0.28 oder höher ausgeführt werden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). Wenn Sie die CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet) | Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [az network vnet peering create](/cli/azure/network/vnet/peering) | Erstellt ein Peering zwischen virtuellen Netzwerken.  |
| [az group delete](/cli/azure/vm/extension) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Weitere CLI-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [Azure CLI-Beispiele für virtuelle Netzwerke](../cli-samples.md).
