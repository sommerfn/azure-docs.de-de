---
title: Mandantenübergreifendes Freigeben von Katalogimages in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie VM-Images mithilfe von Katalogen mit freigegebenen Images über Azure-Mandanten hinweg freigeben.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: c26abe948fa415c780d543c615c34af2091cfbc7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709156"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Freigeben von Katalog-VM-Images über Azure-Mandanten hinweg

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


> [!IMPORTANT]
> Sie können das Portal nicht verwenden, um einen virtuellen Computer aus einem Image in einem anderen Azure-Mandanten bereitzustellen. Zum Erstellen eines virtuellen Computers aus einem von Mandanten gemeinsam verwendeten Image müssen Sie die Azure CLI oder [PowerShell](../linux/share-images-across-tenants.md) verwenden.

## <a name="create-a-vm-using-powershell"></a>Erstellen eines virtuellen Computers mithilfe von PowerShell


Melden Sie sich bei beiden Mandanten mit der Anwendungs-ID, dem Geheimnis und der Mandanten-ID an. 

```azurepowershell-interactive
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

Erstellen Sie die VM in der Ressourcengruppe, die über die Berechtigung für die App-Registrierung verfügt. Ersetzen Sie die Informationen in diesem Beispiel durch Ihre eigenen.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $image `
   -Location "South Central US" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch über das [Azure-Portal](shared-images-portal.md) Ressourcen in Katalogen mit freigegebenen Images erstellen.