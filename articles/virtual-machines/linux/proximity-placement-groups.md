---
title: Verwenden von Näherungsplatzierungsgruppen für Linux-VMs
description: Erfahren Sie mehr über das Erstellen und Verwenden von Näherungsplatzierungsgruppen für Linux-VMs in Azure.
services: virtual-machines-linux
author: cynthn
manager: gwallace
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: 353a266b647c299515c15889c302ba4409aa511b
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73171058"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Bereitstellen von VMs für Näherungsplatzierungsgruppen mit Azure CLI

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer [Näherungsplatzierungsgruppe](co-location.md#proximity-placement-groups) bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Computeressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.


## <a name="create-the-proximity-placement-group"></a>Erstellen der Näherungsplatzierungsgruppe
Erstellen Sie eine Näherungsplatzierungsgruppe mit [`az ppg create`](/cli/azure/ppg#az-ppg-create). 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Auflisten von Näherungsplatzierungsgruppen

Mit [az ppg list](/cli/azure/ppg#az-ppg-list) können Sie alle Ihre Näherungsplatzierungsgruppen auflisten.

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Erstellen einer VM

Erstellen Sie einen virtuellen Computer innerhalb der Näherungsplatzierungsgruppe mit [new az vm](/cli/azure/vm#az-vm-create).

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Sie können die VM in der Näherungsplatzierungsgruppe mit [az ppg show](/cli/azure/ppg#az-ppg-show) anzeigen.

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Sie können auch eine Verfügbarkeitsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `--ppg`-Parameter mit [az vm availability-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create), um eine Verfügbarkeitsgruppe zu erstellen, und alle in der Verfügbarkeitsgruppe erstellten virtuellen Computer werden ebenfalls in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="scale-sets"></a>Skalierungsgruppen

Sie können auch eine Skalierungsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `--ppg`-Parameter mit [az vmss create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create), um eine Skalierungsgruppe zu erstellen, und alle Instanzen werden in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure CLI](/cli/azure/ppg)-Befehle für Näherungsplatzierungsgruppen.
