---
title: 'Aktualisieren einer IPv4-Anwendung auf IPv6 in Azure Virtual Network: PowerShell'
titlesuffix: Azure Virtual Network
description: Dieser Artikel zeigt, wie IPv6-Adressen für eine vorhandene Anwendung in Azure Virtual Network mit Azure PowerShell bereitgestellt werden.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/21/2019
ms.author: kumud
ms.openlocfilehash: 47f73ca8ece8db5fad3f8a7709d8787db42626f4
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791200"
---
# <a name="upgrade-an-ipv4-application-to-ipv6-in-azure-virtual-network---powershell-preview"></a>Aktualisieren einer IPv4-Anwendung auf IPv6 in Azure Virtual Network: PowerShell (Vorschau)

Dieser Artikel zeigt Ihnen, wie Sie einer Anwendung IPv6-Adressen hinzufügen können, die öffentliche IPv4-Adresse in einem virtuellen Azure-Netzwerk für Load Balancer Standard verwendet. Das direkte Upgrade umfasst ein virtuelles Netzwerk und ein Subnetz, Load Balancer Standard mit IPv4- und IPV6-Front-End-Konfigurationen, VMs mit NICs mit IPv4- und IPv6-Konfigurationen, eine Netzwerksicherheitsgruppe und öffentliche IP-Adressen.

> [!Important]
> Die IPv6-Unterstützung für Azure Virtual Network ist derzeit als Public Preview verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 6.9.0 des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="register-the-service"></a>Registrieren des Diensts

Bevor Sie eine Anwendung mit dualem Stapel in Azure bereitstellen, müssen Sie Ihr Abonnement für diese Previewfunktion mit dem folgenden Azure PowerShell-Befehl konfigurieren:

Führen Sie die Registrierung wie folgt aus:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowIPv6CAOnStandardLB -ProviderNamespace Microsoft.Network
```
Es dauert bis zu 30 Minuten, bis die Featureregistrierung abgeschlossen ist. Sie können den Registrierungsstatus überprüfen, indem Sie den folgenden Azure PowerShell-Befehl ausführen: Überprüfen Sie die Registrierung wie folgt:
```azurepowershell
Get-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
Get-AzProviderFeature -FeatureName AllowIPv6CAOnStandardLB -ProviderNamespace Microsoft.Network
```
Führen Sie im Anschluss an die Registrierung den folgenden Befehl aus:

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

### <a name="create-a-standard-load-balancer"></a>Einrichten eines Load Balancers im Tarif „Standard“
In diesem Artikel wird davon ausgegangen, dass Sie eine Load Balancer Standard-Instanz wie unter [Schnellstart: Erstellen einer Load Balancer Standard-Instanz: Azure PowerShell](../load-balancer/quickstart-create-standard-load-balancer-powershell.md) beschrieben bereitgestellt haben.

## <a name="retrieve-the-resource-group"></a>Abrufen der Ressourcengruppe

Bevor Sie Ihr virtuelles Dual Stack-Netzwerk erstellen können, müssen Sie mit [Get-AzResourceGroup](/powershell/module/az.resources/get-azresourcegroup) eine Ressourcengruppe abrufen.

```azurepowershell
 $rg = Get-AzResourceGroup  -ResourceGroupName "myResourceGroupSLB"
```

## <a name="create-an-ipv6-ip-addresses"></a>Erstellen von IPv6-Adressen

Erstellen Sie mit [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) eine öffentliche IPv6-Adresse für Ihre Load Balancer Standard-Instanz. Im folgenden Beispiel wird in der Ressourcengruppe *myResourceGroupSLB* eine öffentliche IPv6-Adresse mit dem Namen *PublicIP_v6* erstellt:

```azurepowershell
  
  $PublicIP_v6 = New-AzPublicIpAddress `
  -Name "PublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Sku Standard  `
  -AllocationMethod Static `
  -IpAddressVersion IPv6
```

## <a name="configure-load-balancer-frontend"></a>Konfigurieren des Load Balancer-Front-Ends

Rufen Sie die vorhandene Load Balancer-Konfiguration ab, und konfigurieren Sie sie dann mit der neuen IPv6-Adresse, indem Sie [Add-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/Add-AzLoadBalancerFrontendIpConfig) wie folgt verwenden:

```azurepowershell
# Retrieve the load balancer configuration
$lb = Get-AzLoadBalancer -ResourceGroupName $rg.ResourceGroupName -Name "MyLoadBalancer"
# Add IPv6 components to the local copy of the load balancer configuration
$lb | Add-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6
#Update the running load balancer with the new frontend
$lb | Set-AzLoadBalancer
```

## <a name="configure-load-balancer-backend-pool"></a>Konfigurieren des Back-End-Pools für die Load Balancer-Instanz

Erstellen Sie den Back-End-Pool in der lokalen Kopie der Load Balancer-Konfiguration, und aktualisieren Sie die aktuell ausgeführte Load Balancer-Instanz mit der neuen Konfiguration des Back-End-Pools wie folgt:

```azurepowershell
$lb | Add-AzLoadBalancerBackendAddressPoolConfig -Name "LbBackEndPool_v6"
# Update the running load balancer with the new backend pool
$lb | Set-AzLoadBalancer
```

## <a name="configure-load-balancer-rules"></a>Konfigurieren von Load Balancer-Regeln
Rufen Sie die vorhandene Konfiguration des Load Balancer-Front-Ends und des Back-End-Pools ab, und fügen Sie dann mithilfe von [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/Add-AzLoadBalancerRuleConfig) neue Lastenausgleichsregeln hinzu.

```azurepowershell
# Retrieve the updated (live) versions of the frontend and backend pool
$frontendIPv6 = Get-AzLoadBalancerFrontendIpConfig -Name "dsLbFrontEnd_v6" -LoadBalancer $lb
$backendPoolv6 = Get-AzLoadBalancerBackendAddressPoolConfig -Name "LbBackEndPool_v6" -LoadBalancer $lb
# Create new LB rule with the frontend and backend
$lb | Add-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
#Finalize all the load balancer updates on the running load balancer
$lb | Set-AzLoadBalancer
```
## <a name="add-ipv6-address-ranges"></a>Hinzufügen von IPv6-Adressbereichen

Fügen Sie dem virtuellen Netzwerk und Subnetz, die die Load Balancer-Instanz hosten, IPv6-Adressbereiche wie folgt hinzu:

```azurepowershell
#Add IPv6 ranges to the VNET and subnet
#Retreive the VNET object
$vnet = Get-AzVirtualNetwork  -ResourceGroupName $rg.ResourceGroupName -Name "myVnet" 
#Add IPv6 prefix to the VNET
$vnet.addressspace.addressprefixes.add("ace:cab:deca::/48")
#Update the running VNET
$vnet |  Set-AzVirtualNetwork

#Retrieve the subnet object from the local copy of the VNET
$subnet= $vnet.subnets[0]
#Add IPv6 prefix to the Subnet (subnet of the VNET prefix, of course)
$subnet.addressprefix.add("ace:cab:deca::/64")
#Update the running VNET with the new subnet configuration
$vnet |  Set-AzVirtualNetwork

```
## <a name="add-ipv6-configuration-to-nic"></a>Hinzufügen der IPv6-Konfiguration zur NIC

Konfigurieren Sie die beiden VM-NICs mit einer IPv6-Adresse, indem Sie [Add-AzNetworkInterfaceIpConfig](/powershell/module/az.network/Add-AzNetworkInterfaceIpConfig) wie folgt verwenden:

```azurepowershell

#Retrieve the NIC objects
$NIC_1 = Get-AzNetworkInterface -Name "myNic1" -ResourceGroupName $rg.ResourceGroupName
$NIC_2 = Get-AzNetworkInterface -Name "myNic2" -ResourceGroupName $rg.ResourceGroupName
$NIC_3 = Get-AzNetworkInterface -Name "myNic3" -ResourceGroupName $rg.ResourceGroupName
#Add an IPv6 IPconfig to NIC_1 and update the NIC on the running VM
$NIC_1 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_1 | Set-AzNetworkInterface
#Add an IPv6 IPconfig to NIC_2 and update the NIC on the running VM
$NIC_2 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_2 | Set-AzNetworkInterface
#Add an IPv6 IPconfig to NIC_3 and update the NIC on the running VM
$NIC_3 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_3 | Set-AzNetworkInterface

```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Anzeigen des virtuellen IPv6-Dual Stack-Netzwerks im Azure-Portal
Sie können das virtuelle IPv6-Dual Stack-Netzwerk im Azure-Portal wie folgt anzeigen:
1. Geben Sie in der Suchleiste des Portals *myVnet* ein.
2. Wenn **myVnet** in den Suchergebnissen angezeigt wird, wählen Sie diesen Eintrag aus. Dadurch wird die Seite **Übersicht** des virtuellen Dual Stack-Netzwerks namens *myVNet* gestartet. Das virtuelle Dual Stack-Netzwerk zeigt die drei NICs mit IPv4- und IPv6-Konfigurationen im Dual Stack-Subnetz namens *mySubnet* an.

  ![Virtuelles IPv6-Dual Stack-Netzwerk in Azure](./media/ipv6-add-to-existing-vnet-powershell/ipv6-dual-stack-vnet.png)

> [!NOTE]
> Das virtuelle IPv6-Netzwerk für Azure ist im Azure-Portal schreibgeschützt für diese Vorschauversion verfügbar.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyAzureResourceGroupSLB
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine vorhandene Load Balancer Standard-Instanz mit einer IPv4-Front-End-Konfiguration in eine Dual Stack-Konfiguration (IPv4 und IPv6) aktualisiert. Außerdem haben Sie den NICs der VMs im Back-End-Pool IPv6-Konfigurationen hinzugefügt. Weitere Informationen zur IPv6-Unterstützung in virtuellen Azure-Netzwerken finden Sie unter [Was ist IPv6 für Azure Virtual Network?](ipv6-overview.md)
