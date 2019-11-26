---
title: 'Azure CLI-Skriptbeispiel: Erstellen von zwei VMs mit einer internen und externen NSG'
description: Azure CLI-Skriptbeispiel – Erstellen von zwei VMs mit interner und externer NSG
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 857a2862a7ddf515567f96b2e688e54a957cd3d9
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74039581"
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Sicherer Netzwerkdatenverkehr zwischen virtuellen Computern

Dieses Skript erstellt zwei virtuelle Computer und sichert den eingehenden Datenverkehr für beide VMs. Die erste VM ist über das Internet zugänglich und wird mit einer Netzwerksicherheitsgruppe (NSG) konfiguriert, um Datenverkehr über die Ports 22 und 80 zuzulassen. Die zweite VM ist nicht über das Internet zugänglich. Sie wird mit einer NSG konfiguriert, die nur Datenverkehr vom ersten virtuellen Computer zulässt.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet) | Erstellt ein Subnetz. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az network nsg rule list](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Gibt Informationen zu einer Netzwerksicherheitsgruppenregel zurück. In diesem Beispiel wird der Regelname in einer Variablen gespeichert, um eine spätere Verwendung im Skript zu ermöglichen. |
| [az network nsg rule update](https://docs.microsoft.com/cli/azure/network/nsg/rule) | Aktualisiert eine NSG-Regel. In diesem Beispiel wird die Back-End-Regel aktualisiert, um nur Datenverkehr vom Front-End-Subnetz zuzulassen. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
