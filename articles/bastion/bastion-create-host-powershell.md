---
title: Erstellen eines Bastionhosts über Azure Powershell | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie einen Azure Bastion-Host erstellen.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 6cae6d258da2ddf0c3bfaade65ae74f1201b67b7
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74121072"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Erstellen eines Azure Bastion-Hosts über Azure PowerShell

In diesem Artikel wird gezeigt, wie Sie einen Azure Bastion-Host erstellen. Wenn Sie den Azure Bastion-Dienst in Ihrem virtuellen Netzwerk bereitgestellt haben, steht die nahtlose RDP-/SSH-Funktion für alle virtuellen Computer im gleichen virtuellen Netzwerk zur Verfügung. Diese Bereitstellung erfolgt pro virtuellem Netzwerk und nicht pro Abonnement/Konto oder virtuellem Computer.

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie sicher, dass Sie über ein Azure-Abonnement verfügen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial) registrieren.

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="createhost"></a>Erstellen eines Bastion-Hosts

In diesem Abschnitt erfahren Sie, wie Sie eine neue Azure Bastion-Ressource über Azure PowerShell erstellen.

1. Erstellen Sie ein virtuelles Netzwerk und ein Azure Bastion-Subnetz. Sie müssen ein Azure Bastion-Subnetz mit dem Namen **AzureBastionSubnet** erstellen. Dadurch weiß Azure, in welchem Subnetz die Bastion-Ressourcen bereitgestellt werden sollen. Dieses Subnetz unterscheidet sich von einem Gatewaysubnetz. Verwenden Sie mindestens ein Subnetz mit „/27“ oder ein größeres Subnetz („/27“, „/26“ usw.). Erstellen Sie **AzureBastionSubnet** ohne Routentabellen oder Delegierungen. Weitere Informationen zum Verwenden von Netzwerksicherheitsgruppen in **AzureBastionSubnet** finden Sie unter [Arbeiten mit Netzwerksicherheitsgruppen](bastion-nsg.md).

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName " myBastionRG " -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Erstellen Sie eine öffentliche IP-Adresse für Azure Bastion. Die öffentliche IP-Adresse ist die öffentliche IP-Adresse der Bastion-Ressource für den RDP-/SSH-Zugriff (über den Port 443). Die öffentliche IP-Adresse muss sich in der gleichen Region befinden wie die Bastion-Ressource, die Sie erstellen.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Erstellen Sie eine neue Azure Bastion-Ressource im AzureBastionSubnet des virtuellen Netzwerks. Die Erstellung und Bereitstellung der Bastion-Ressource dauert etwa fünf Minuten.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName " myBastionRG " -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
