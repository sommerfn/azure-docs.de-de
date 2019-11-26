---
title: Azure PowerShell-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in einer anderen Region
description: Azure PowerShell-Beispielskript – Exportieren/Kopieren einer Momentaufnahme als VHD in ein Speicherkonto in einer anderen Region
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: c1ad520aadf8b2bafa9fdd03333e50b47612fa49
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040202"
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>Exportieren/Kopieren verwalteter Momentaufnahmen als VHD in ein Speicherkonto in einer anderen Region mit PowerShell

Dieses Skript exportiert eine verwaltete Momentaufnahme in ein Speicherkonto in einer anderen Region. Zuerst generiert es den SAS-URI der Momentaufnahme, mit dem es diese anschließend in ein Speicherkonto in einer anderen Region kopiert. Verwenden Sie dieses Skript, um Sicherungen Ihrer verwalteten Datenträger für die Notfallwiederherstellung in einer anderen Region zu verwalten.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI für eine verwaltete Momentaufnahme und zum Kopieren der Momentaufnahme in ein Speicherkonto mithilfe dieses SAS-URI. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Grant-AzSnapshotAccess](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Generiert ein SAS-URI für eine Momentaufnahme zum Kopieren in ein anderes Speicherkonto. |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | Erstellt einen Speicherkontokontext, der den Kontonamen und -schlüssel verwendet. Dieser Kontext kann verwendet werden, um Lese-/Schreibvorgänge im Speicherkonto auszuführen. |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Kopiert die zugrunde liegende VHD einer Momentaufnahme in ein Speicherkonto. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).