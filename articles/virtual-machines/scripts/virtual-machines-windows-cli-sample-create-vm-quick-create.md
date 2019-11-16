---
title: Azure CLI-Skriptbeispiel – Schnelles Erstellen einer Windows Server 2016-VM
description: Azure CLI-Skriptbeispiel – Schnelles Erstellen einer Windows Server 2016-VM
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: a7be615584c4994619683a3670898d4c47c91d25
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74039886"
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a>Schnelles Erstellen einer VM mit der Azure CLI

Dieses Skript erstellt einen virtuellen Azure-Computer mit Windows Server 2016. Nach dem Ausführen des Skripts können Sie über eine Remotedesktopverbindung auf den virtuellen Computer zugreifen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-windows-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
