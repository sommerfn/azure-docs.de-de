---
title: 'Azure CLI-Skriptbeispiel: Erstellen einer VM mit einer VHD'
description: 'Azure-CLI-Skriptbeispiel: Erstellen einer VM mit einer virtuellen Festplatte.'
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
ms.date: 03/09/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 16f4e3e153cbc02f8626199d168d069add48e4b6
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74037669"
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Erstellen einer VM mit einer virtuellen Festplatte

In diesem Beispiel wird ein virtueller Computer mithilfe einer VHD erstellt.
Es wird eine Ressourcengruppe, ein Speicherkonto und ein Container erstellt. Anschließend wird eine VM erstellt, indem die VHD in den Container hochgeladen wird.
Der öffentliche SSH-Schlüssel wird durch Ihren öffentlichen Schlüssel ersetzt, damit Sie Zugriff auf die VM haben.

Sie benötigen eine startbare virtuelle Festplatte. Das Skript sucht nach `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account) | Listet Speicherkonten auf |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account) | Stellt sicher, dass ein Speicherkontoname gültig und nicht bereits vorhanden ist |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys) | Listet Schlüssel für die Speicherkonten auf |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob) | Überprüft, ob das Blob vorhanden ist |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container) | Erstellt einen Container in einem Speicherkonto |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob) | Erstellt ein Blob im Container durch Hochladen der VHD |
| [az vm list](https://docs.microsoft.com/cli/azure/vm) | Wird zusammen mit `--query` verwendet, um zu überprüfen, ob der Name der VM bereits verwendet wird | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set) | Erstellt die virtuellen Computer |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#az-vm-list-ip-addresses) | Ruft die IP-Adresse des virtuellen Computers ab, der erstellt wurde |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
