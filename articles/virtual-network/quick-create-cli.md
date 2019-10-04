---
title: 'Erstellen eines virtuellen Netzwerks: Schnellstart – Azure CLI'
titlesuffix: Azure Virtual Network
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über Azure CLI ein virtuelles Netzwerk erstellen. In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer privat miteinander und mit dem Internet kommunizieren.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/12/2018
ms.author: kumud
ms.openlocfilehash: 6306d893f491f93cc31b7e478afe5632e997285c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692644"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Schnellstart: Erstellen eines virtuellen Netzwerks über die Azure-Befehlszeilenschnittstelle

In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer (VMs) privat miteinander und mit dem Internet kommunizieren. In dieser Schnellstartanleitung erfahren Sie, wie Sie ein virtuelles Netzwerk erstellen. Nach dem Erstellen eines virtuellen Netzwerks stellen Sie zwei virtuelle Computer im virtuellen Netzwerk bereit. Anschließend stellen Sie über das Internet eine Verbindung mit den VMs her und kommunizieren privat über das neue virtuelle Netzwerk.

Wenn Sie kein Azure-Abonnement besitzen, können Sie jetzt ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie stattdessen entscheiden, Azure CLI lokal zu installieren und zu verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group-and-a-virtual-network"></a>Erstellen einer Ressourcengruppe und eines virtuellen Netzwerks

Bevor Sie ein virtuelles Netzwerk erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [az group create](/cli/azure/group) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet) ein virtuelles Netzwerk. In diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *default* erstellt:

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk.

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit [az vm create](/cli/azure/vm) einen virtuellen Computer. Wenn SSH-Schlüssel nicht bereits an einem Standardschlüsselspeicherort vorhanden sind, werden sie durch den Befehl erstellt. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`. Mit der Option `--no-wait` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können. In diesem Beispiel wird ein virtueller Computer namens *myVm1* erstellt:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

Da Sie im vorherigen Schritt die Option `--no-wait` verwendet haben, können Sie fortfahren und den zweiten virtuellen Computer mit dem Namen *myvm2* erstellen.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="azure-cli-output-message"></a>Azure CLI-Ausgabenachricht

Die Erstellung der VMs kann einige Minuten dauern. Nachdem Azure die virtuellen Computer erstellt hat, gibt Azure CLI etwa folgende Ausgabe zurück:

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
  "zones": ""
}
```

Notieren Sie sich **publicIpAddress**. Sie verwenden diese Adresse im nächsten Schritt zum Herstellen einer Verbindung mit dem virtuellen Computer über das Internet.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Ersetzen Sie in diesem Befehl `<publicIpAddress>` durch die öffentliche IP-Adresse Ihres virtuellen Computers *myVm2*:

```bash
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

Wenn Sie die private Kommunikation zwischen den virtuellen Computern *myVm2* und *myVm1* überprüfen möchten, geben Sie diesen Befehl ein:

```bash
ping myVm1 -c 4
```

Sie erhalten vier Antworten von *10.0.0.4*.

Beenden Sie die SSH-Sitzung mit dem virtuellen Computer *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit [az group delete](/cli/azure/group) entfernen:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und privat zwischen den beiden virtuellen Computern kommuniziert. Weitere Informationen zu den Einstellungen des virtuellen Netzwerks finden Sie unter [Create, change, or delete a virtual network](manage-virtual-network.md) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks).

Azure erlaubt uneingeschränkte private Kommunikation zwischen virtuellen Computern. Standardmäßig ermöglicht Azure nur eingehende Remotedesktopverbindungen mit Windows-VMs über das Internet. Weitere Informationen zum Konfigurieren der verschiedenen Typen der VM-Netzwerkkommunikation finden Sie im Tutorial [Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](tutorial-filter-network-traffic.md).
