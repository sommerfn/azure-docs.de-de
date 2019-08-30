---
title: Verwenden von Näherungsplatzierungsgruppen für Windows-VMS | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Erstellen und Verwenden von Näherungsplatzierungsgruppen für Windows-VMs in Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: af75b3f98232d6507fc8b0fda179bebc75828086
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70088835"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-powershell"></a>Vorschau: Bereitstellen von VMs für Näherungsplatzierungsgruppen mit PowerShell


Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer [Näherungsplatzierungsgruppe](co-location.md#preview-proximity-placement-groups) bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Computeressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.

> [!IMPORTANT]
> Näherungsplatzierungsgruppen sind zurzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Näherungsplatzierungsgruppen sind in der Vorschauversion in den folgenden Regionen nicht verfügbar: **Japan, Osten**, **Australien, Osten** und **Indien, Mitte**.


## <a name="create-a-proximity-placement-group"></a>Erstellen einer Näherungsplatzierungsgruppe
Erstellen Sie mithilfe des Cmdlets [New-AzProximityPlacementGroup ](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) eine Näherungsplatzierungsgruppe. 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Auflisten von Näherungsplatzierungsgruppen

Mit dem Cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) können Sie alle Näherungsplatzierungsgruppen auflisten.

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-vm"></a>Erstellen einer VM

Erstellen Sie mit `-ProximityPlacementGroup $ppg.Id` eine VM in der Näherungsplatzierungsgruppe, um auf die Näherungsplatzierungsgruppen-ID zu verweisen, wenn Sie mit [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) die VM erstellen.

```azurepowershell-interactive
$vmName = "myVM"

New-AzVm `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -OpenPorts 3389 `
  -ProximityPlacementGroup $ppg.Id
```

Sie können die VM in der Platzierungsgruppe mit [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) anzeigen.

```azurepowershell-interactive
Get-AzProximityPlacementGroup -ResourceId $ppg.Id |
    Format-Table -Property VirtualMachines -Wrap
```

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Sie können auch eine Verfügbarkeitsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `-ProximityPlacementGroup`-Parameter mit dem Cmdlet [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset), um eine Verfügbarkeitsgruppe zu erstellen, und alle in der Verfügbarkeitsgruppe erstellten virtuellen Computer werden ebenfalls in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="scale-sets"></a>Skalierungsgruppen

Sie können auch eine Skalierungsgruppe in ihrer Näherungsplatzierungsgruppe erstellen. Verwenden Sie den gleichen `-ProximityPlacementGroup`-Parameter mit [New-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss), um eine Skalierungsgruppe zu erstellen, und alle Instanzen werden in derselben Näherungsplatzierungsgruppe erstellt.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch die [Azure CLI](../linux/proximity-placement-groups.md) zum Erstellen von Näherungsplatzierungsgruppen verwenden.
