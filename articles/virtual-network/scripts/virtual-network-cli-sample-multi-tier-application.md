---
title: Azure CLI-Skriptbeispiel – Erstellen eines Netzwerks für Anwendungen mit mehreren Ebenen | Microsoft-Dokumentation
description: Azure CLI-Skriptbeispiel – Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 9bd6fac54c4f6c86c4e1c624ffc26f626e938e49
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715699"
---
# <a name="create-a-network-for-multi-tier-applications-script-sample"></a>Skriptbeispiel für das Erstellen eines Netzwerks für Anwendungen mit mehreren Ebenen

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Der Datenverkehr am Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr zum Back-End-Subnetz auf MySQL und Port 3306 beschränkt ist. Nach dem Ausführen des Skripts haben Sie zwei virtuelle Computer – einen in jedem Subnetz – denen Sie Webserver- und MySQL-Software bereitstellen können.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/bash) oder eine lokale Installation der Azure CLI ausführen. Wenn Sie die CLI lokal verwenden, muss für dieses Skript Version 2.0.28 oder höher ausgeführt werden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). Wenn Sie die CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Erstellt ein Back-End-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic) | Erstellt virtuelle Netzwerkschnittstellen und fügt sie an das Front-End- und Back-End-Subnetz des virtuellen Netzwerks an. |
| [az network nsg create](/cli/azure/network/nsg) | Erstellt Netzwerksicherheitsgruppen (NSG), die dem Front-End- und Back-End-Subnetz zugeordnet sind. |
| [az network nsg rule create](/cli/azure/network/nsg/rule) |Erstellt NSG-Regeln, die bestimmte Ports bei bestimmten Subnetzen zulassen oder blockieren. |
| [az vm create](/cli/azure/vm) | Erstellt virtuelle Computer und fügt jedem virtuellen Computer eine NIC hinzu. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [Azure CLI-Beispiele für virtuelle Netzwerke](../cli-samples.md).
