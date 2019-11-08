---
title: Azure PowerShell-Skriptbeispiel – Exportieren/Kopieren der VHD eines verwalteten Datenträgers in ein Speicherkonto in einer anderen Region | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Exportieren/Kopieren der VHD eines verwalteten Datenträgers in ein Speicherkonto in derselben oder einer anderen Region
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
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: 2b5eb3d691f5ffdf7e659f3f1e08d288101f4276
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748774"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell"></a>Exportieren/Kopieren der VHD eines verwalteten Datenträgers in ein Speicherkonto in einer anderen Region mit PowerShell

Mit diesem Skript wird die VHD eines verwalteten Datenträgers in ein Speicherkonto in einer anderen Region exportiert. Zuerst wird der SAS-URI des verwalteten Datenträgers generiert und anschließend verwendet, um die zugrunde liegende VHD in ein Speicherkonto in einer anderen Region zu kopieren. Verwenden Sie dieses Skript, um verwaltete Datenträger zur regionalen Erweiterung in eine andere Region zu kopieren.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Generieren des SAS-URI eines verwalteten Datenträgers und zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto mithilfe des SAS-URI. Jeder Befehl in der Tabelle verfügt über einen Link zur zugehörigen Dokumentation.

| Get-Help | Notizen |
|---|---|
| [Grant-AzDiskAccess](https://docs.microsoft.com/powershell/module/az.compute/grant-azdiskaccess) | Generiert einen SAS-URI für einen verwalteten Datenträger, der zum Kopieren der zugrunde liegenden VHD in ein Speicherkonto verwendet wird. |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | Erstellt einen Speicherkontokontext, der den Kontonamen und -schlüssel verwendet. Dieser Kontext kann verwendet werden, um Lese-/Schreibvorgänge im Speicherkonto auszuführen. |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Kopiert die zugrunde liegende VHD einer Momentaufnahme in ein Speicherkonto. |

## <a name="next-steps"></a>Nächste Schritte

[Erstellen verwalteter Datenträger aus einer VHD](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Erstellen eines virtuellen Computers aus einem verwalteten Datenträger](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche VM-PowerShell-Skriptbeispiele finden Sie in der [Dokumentation zu Windows-VMs in Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).