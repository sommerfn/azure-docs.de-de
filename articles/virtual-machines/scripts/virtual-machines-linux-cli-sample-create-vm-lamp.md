---
title: 'Azure CLI-Skriptbeispiel: Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich'
description: Verwenden Sie eine benutzerdefinierte Skripterweiterung zum Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
ms.author: cynthn
tags: azure-service-management
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.custom: mvc
ms.date: 04/05/2017
ms.openlocfilehash: 00aa744f09b7bcbbbc258b92a8fd5ab750e447c7
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74039114"
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich

Mit diesem Beispiel wird eine VM-Skalierungsgruppe erstellt und eine Erweiterung angewendet, mit der auf jedem virtuellen Computer in der Skalierungsgruppe ein benutzerdefiniertes Skript zum Bereitstellen des LAMP-Stapels ausgeführt wird.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Verbinden

Verwenden Sie folgenden Code zum Herstellen einer Verbindung mit den virtuellen Computern und der Skalierungsgruppe.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und die virtuellen Computer sowie alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss) | Erstellt eine VM-Skalierungsgruppe. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule) | Fügt einen Endpunkt mit Lastenausgleich hinzu. |
| [az vmss extension set](https://docs.microsoft.com/cli/azure/vmss/extension) | Erstellt die Erweiterung, mit der das benutzerdefinierte Skript für die Bereitstellung auf einem virtuellen Computer ausgeführt wird. |
| [az vmss update-instances](https://docs.microsoft.com/cli/azure/vmss) | Führt das benutzerdefinierte Skript auf den VM-Instanzen aus, die bereitgestellt wurden, bevor die Erweiterung auf die Skalierungsgruppe angewendet wurde. |
| [az vmss scale](https://docs.microsoft.com/cli/azure/vmss) | Skaliert die Skalierungsgruppe durch Hinzufügen weiterer VM-Instanzen zentral hoch. Das benutzerdefinierte Skript wird auf diesen Instanzen ausgeführt, wenn sie bereitgestellt werden. |
| [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip) | Ruft die IP-Adressen der Computer ab, die in diesem Beispiel erstellt wurden. |
| [az network lb show](https://docs.microsoft.com/cli/azure/network/lb) | Ruft die vom Load Balancer verwendeten Front-End- und Back-End-Ports ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
