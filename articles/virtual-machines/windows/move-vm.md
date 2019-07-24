---
title: Verschieben einer Windows-VM-Ressource in Azure | Microsoft-Dokumentation
description: Verschieben Sie einen virtuellen Windows-Computer im Resource Manager-Bereitstellungsmodell in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/03/2019
ms.author: cynthn
ms.openlocfilehash: 6b189c4bfcc61084ed197649d376cae8fdf2eb56
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723093"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Verschieben eines virtuellen Windows-Computers in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe
In diesem Artikel wird beschrieben, wie Sie einen virtuellen Windows-Computer (VM) zwischen Ressourcengruppen oder Abonnements verschieben. Das Verschieben zwischen Abonnements kann hilfreich sein, wenn Sie einen virtuellen Computer ursprünglich in einem persönlichen Abonnement erstellt haben und ihn nun in das Abonnement Ihres Unternehmens verschieben möchten, um weiterarbeiten zu können. Sie müssen den virtuellen Computer nicht starten, um ihn zu verschieben, und er sollte auch während des Verschiebens weiter ausgeführt werden.

> [!IMPORTANT]
>Im Rahmen der Verschiebung werden neue Ressourcen-IDs erstellt. Nach dem Verschieben des virtuellen Computers müssen Sie Ihre Tools und Skripts aktualisieren, damit die neuen Ressourcen-IDs verwendet werden. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Verschieben eines virtuellen Computers mithilfe von PowerShell

Wenn Sie einen virtuellen Computer in eine andere Ressourcengruppe verschieben möchten, müssen Sie auch alle abhängigen Ressourcen verschieben. Um eine Liste mit der Ressourcen-ID jeder dieser Ressourcen zu erhalten, verwenden Sie das Cmdlet [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource).

```azurepowershell-interactive
 Get-AzResource -ResourceGroupName <sourceResourceGroupName> | Format-list -wrap -Property ResourceId 
```

Sie können die Ausgabe des vorherigen Befehls als durch Trennzeichen getrennte Liste von Ressourcen-IDs für [Move-AzResource](https://docs.microsoft.com/powershell/module/az.resources/move-azresource) verwenden, um jede Ressource an das Ziel zu verschieben. 

```azurepowershell-interactive
Move-AzResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Schließen Sie den Parameter **-DestinationSubscriptionId** ein, um Ressourcen in ein anderes Abonnement zu verschieben. 

```azurepowershell-interactive
Move-AzResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Wenn Sie aufgefordert werden, zu bestätigen, dass die angegebene Ressource verschoben werden soll, geben Sie **J** ein.

## <a name="next-steps"></a>Nächste Schritte
Sie können viele verschiedene Arten von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../resource-group-move-resources.md).    

