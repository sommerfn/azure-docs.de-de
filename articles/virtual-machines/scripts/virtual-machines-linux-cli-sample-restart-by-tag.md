---
title: Azure CLI-Skriptbeispiel – Neustarten von VMs
description: Azure CLI-Skriptbeispiel – Neustarten von VMs nach Tag und nach ID
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
ms.date: 03/01/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 5545fc07bd7b30225bcaf01eee8f10a141cd8d57
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040247"
---
# <a name="restart-vms"></a>Neustarten von VMs

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Dieses Beispiel zeigt verschiedene Möglichkeiten, einige VMs abzurufen und sie neu zu starten.

Bei der ersten werden alle virtuellen Computer in der Ressourcengruppe neu gestartet.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

Bei der zweiten werden die markierten VMs mit `az resource list` abgerufen, und es wird nach den Ressourcen gefiltert, die virtuelle Computer sind, und diese virtuellen Computer werden neu gestartet.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Installieren der Azure CLI 2.0 unter Windows](/cli/azure/install-azure-cli-windows).


## <a name="sample-script"></a>Beispielskript

Das Beispiel enthält drei Skripts.
Das erste stellt die virtuellen Computer bereit.
Es verwendet die „Nicht-warten“-Option, damit der Befehl nicht auf jeden bereitzustellenden virtuellen Computer wartet.
Das zweite wartet, bis die virtuellen Computer vollständig bereitgestellt sind.
Das dritte Skript startet alle virtuellen Computer neu, die bereitgestellt wurden, und dann einfach die markierten VMs.

### <a name="provision-the-vms"></a>Bereitstellen der VMs

Dieses Skript erstellt eine Ressourcengruppe und dann drei neu zu startende virtuelle Computer.
Zwei davon sind gekennzeichnet.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Warten

Dieses Skript überprüft alle 20 Sekunden den Bereitstellungsstatus, bis alle drei virtuellen Computer bereitgestellt sind, oder bei der Bereitstellung eines von ihnen ein Fehler auftritt.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>Neustarten der VMs

Dieses Skript startet alle virtuellen Computer in der Ressourcengruppe neu und startet dann nur die markierten virtuellen Computer neu.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppen, VMs und alle zugehörigen Ressourcen entfernt werden.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Erstellt die virtuellen Computer.  |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | Wird mit `--query` verwendet, um sicherzustellen, dass die virtuellen Computer vor ihrem Neustart bereitgestellt werden, und dann die IDs der virtuellen Computer abzurufen, um sie neu zu starten. |
| [az resource list](https://docs.microsoft.com/cli/azure/vm) | Wird mit `--query` verwendet, um die IDs der virtuellen Computer mit dem Tag abzurufen. |
| [az vm restart](https://docs.microsoft.com/cli/azure/vm) | Startet die VMs neu. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
