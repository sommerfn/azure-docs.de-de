---
title: 'Azure CLI-Beispiel: Kopieren eines verwalteten Datenträgers in ein Speicherkonto'
description: 'Azure CLI-Beispiel: Exportieren oder Kopieren eines verwalteten Datenträgers in ein Speicherkonto'
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/09/2019
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: c5055860df007d0f009bd08c2b211ffeeaad711f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510873"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Exportieren/Kopieren eines verwalteten Datenträgers in ein Speicherkonto per Azure CLI

Mit diesem Skript wird die zugrunde liegende VHD eines verwalteten Datenträgers in ein Speicherkonto in derselben oder einer anderen Region exportiert. Zuerst wird der SAS-URI des verwalteten Datenträgers generiert und anschließend verwendet, um die VHD in ein Speicherkonto zu kopieren. Verwenden Sie dieses Skript, um verwaltete Datenträger zur regionalen Erweiterung in eine andere Region zu kopieren. Wenn Sie die VHD-Datei eines verwalteten Datenträgers im Azure Marketplace veröffentlichen möchten, können Sie die VHD-Datei mithilfe dieses Skripts in ein Speicherkonto kopieren und anschließend einen SAS-URI der kopierten VHD generieren, um ihn im Marketplace zu veröffentlichen.   


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für einen verwalteten Datenträger und zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto mithilfe des SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az disk grant-access](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Generiert eine schreibgeschützte Shared Access Signature (SAS), die verwendet wird, um die zugrunde liegende VHD-Datei in ein Speicherkonto zu kopieren oder lokal herunterzuladen  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy) | Kopiert ein Blob asynchron aus einem Speicherkonto in ein anderes |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche CLI-Skriptbeispiele für virtuelle Computer und verwaltete Datenträger finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
