---
title: Erstellen eines Azure Private Link-Diensts mit Azure PowerShell | Microsoft-Dokumentation
description: Informationen zum Erstellen eines Azure Private Link-Diensts mit Azure PowerShell
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: d8d40ed83d7f8234092ca6354642a76aaa83bc12
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73173069"
---
# <a name="create-a-private-link-service-using-azure-powershell"></a>Erstellen eines Private Link-Diensts mit Azure PowerShell
In diesem Artikel erfahren Sie, wie Sie mit Azure PowerShell einen Private Link-Dienst in Azure erstellen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die aktuelle Version des Azure PowerShell-Moduls verwenden. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bevor Sie Ihre Private Link-Instanz erstellen können, müssen Sie mit [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe erstellen. Im folgenden Beispiel wird am Speicherort *WestCentralUS* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```azurepowershell
$location = "westcentralus"
$rgName = "myResourceGroup"
New-AzResourceGroup `
  -ResourceGroupName $rgName `
  -Location $location
```
## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie mit [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) ein virtuelles Netzwerk für Ihre Private Link-Instanz. Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen *myvnet* mit Subnetz für Front-End (*frontendSubnet*), Back-End (*backendSubnet*), Private Link (*otherSubnet*) erstellt:

```azurepowershell
$virtualNetworkName = "myvnet"


# Create subnet config

$frontendSubnet = New-AzVirtualNetworkSubnetConfig `
-Name frontendSubnet `
-AddressPrefix "10.0.1.0/24"  

$backendSubnet = New-AzVirtualNetworkSubnetConfig `
-Name backendSubnet `
-AddressPrefix "10.0.2.0/24"  

$otherSubnet = New-AzVirtualNetworkSubnetConfig `
-Name otherSubnet `
-AddressPrefix "10.0.3.0/24" `
-PrivateLinkServiceNetworkPolicies "Disabled" 

# Create the virtual network
$vnet = New-AzVirtualNetwork `
-Name $virtualNetworkName `
-ResourceGroupName $rgName `
-Location $location `
-AddressPrefix "10.0.0.0/16" `
-Subnet $frontendSubnet,$backendSubnet,$otherSubnet 
```
## <a name="create-internal-load-balancer"></a>Erstellen eines internen Lastenausgleichs
Erstellen Sie mit dem Befehl [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) einen internen Load Balancer Standard-Lastenausgleich. Im folgenden Beispiel wird mithilfe von Front-End-IP-Konfiguration, Test, Regel und Back-End-Pool, die Sie in den vorherigen Schritten erstellt haben, ein interner Load Balancer Standard-Lastenausgleich erstellt:

```azurepowershell

$lbBackendName = "LB-backend" 
$lbFrontName = "LB-frontend" 
$lbName = "lb"
 
#Create Internal Load Balancer
$frontendIP = New-AzLoadBalancerFrontendIpConfig -Name $lbFrontName -PrivateIpAddress 10.0.1.5 -SubnetId $vnet.subnets[0].Id 
$beaddresspool= New-AzLoadBalancerBackendAddressPoolConfig -Name $lbBackendName 
$probe = New-AzLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
$rule = New-AzLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beaddresspool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
$NRPLB = New-AzLoadBalancer -ResourceGroupName $rgName -Name $lbName -Location $location -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $probe -LoadBalancingRule $rule -Sku Standard 
```
## <a name="create-a-private-link-service"></a>Erstellen eines Private Link-Diensts
Erstellen Sie einen Private Link-Dienst mit [New-AzPrivateLinkService](/powershell/module/az.network/new-azloadbalancer).  Bei diesem Beispiel wird ein Private Link-Dienst mit dem Namen *myPLS* unter Verwendung einer Load Balancer Standard-Instanz in der Ressourcengruppe mit dem Namen *myResourceGroup* erstellt. 
```azurepowershell

$plsIpConfigName = "PLS-ipconfig" 
$plsName = "pls"
$peName = "pe" 
  
$IPConfig = New-AzPrivateLinkServiceIpConfig `
-Name $plsIpConfigName `
-Subnet $vnet.subnets[2] `
-PrivateIpAddress 10.0.3.5 

$fe = Get-AzLoadBalancer -Name $lbName | Get-AzLoadBalancerFrontendIpConfig 

$privateLinkService = New-AzPrivateLinkService `
-ServiceName $plsName `
-ResourceGroupName $rgName `
-Location $location `
-LoadBalancerFrontendIpConfiguration $frontendIP `
-IpConfiguration $IPConfig 
```

### <a name="get-private-link-service"></a>Abrufen des Private Link-Diensts
Rufen Sie mit [New-AzPrivateLinkService](/powershell/module/az.network/get-azprivatelinkservice) wie folgt nähere Informationen zu Ihrem Private Link-Dienst ab:

```azurepowershell
$pls = Get-AzPrivateLinkService -Name $plsName -ResourceGroupName $rgName 
```

In dieser Phase ist Ihr Private Link-Dienst erfolgreich erstellt und bereit, den Datenverkehr zu empfangen. Beachten Sie, dass das obige Beispiel nur zur Veranschaulichung der Erstellung des Private Link-Diensts mit PowerShell dient.  Wir haben weder die Load Balancer-Back-End-Pools noch Anwendungen in den Back-End-Pools konfiguriert, um auf den Datenverkehr zu lauschen. Wenn Sie den gesamten Datenverkehrsfluss verfolgen möchten, sollten Sie Ihre Anwendung unbedingt hinter Ihrer Load Balancer Standard-Instanz konfigurieren. 

Als Nächstes veranschaulichen wir, wie dieser Dienst mit PowerShell einem privaten Endpunkt in einem anderen VNET zugeordnet wird. Wiederum ist das Beispiel auf die Erstellung des privaten Endpunkts und das Herstellen einer Verbindung mit dem zuvor erstellten Private Link-Dienst beschränkt. Sie können im virtuellen Netzwerk virtuelle Computer zum Senden/Empfangen von Datenverkehr an den privaten Endpunkt erstellen, um Ihr Szenario einzurichten. 

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts
### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie mit [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) ein virtuelles Netzwerk für Ihren privaten Endpunkt. In diesem Beispiel wird ein virtuelles Netzwerk mit dem Namen  *vnetPE* in der Ressourcengruppe *myResourceGroup* erstellt:
 
```azurepowershell
$virtualNetworkNamePE = "vnetPE"
 
# Create VNet for private endpoint
$peSubnet = New-AzVirtualNetworkSubnetConfig `
-Name peSubnet `
-AddressPrefix "11.0.1.0/24" `
-PrivateEndpointNetworkPolicies "Disabled" 

$vnetPE = New-AzVirtualNetwork `
-Name $virtualNetworkNamePE `
-ResourceGroupName $rgName `
-Location $location `
-AddressPrefix "11.0.0.0/16" `
-Subnet $peSubnet 
```

### <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts
Erstellen Sie einen privaten Endpunkt zur Nutzung des Private Link-Diensts, der zuvor in Ihrem virtuellen Netzwerk erstellt wurde:
 
```azurepowershell
 
$plsConnection= New-AzPrivateLinkServiceConnection `
-Name plsConnection `
-PrivateLinkServiceId  $privateLinkService.Id  

$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $rgName -Name $peName -Location $location -Subnet $vnetPE.subnets[0] -PrivateLinkServiceConnection $plsConnection -ByManualRequest 
```
 
### <a name="get-private-endpoint"></a>Abrufen privater Endpunkte
Rufen Sie die IP-Adresse des privaten Endpunkts mit `Get-AzPrivateEndpoint` wie folgt ab:

```azurepowershell
# Get Private Endpoint and its IP Address 
$pe =  Get-AzPrivateEndpoint `
-Name $peName `
-ResourceGroupName $rgName  `
-ExpandResource networkinterfaces

$pe.NetworkInterfaces[0].IpConfigurations[0].PrivateIpAddress 

```

### <a name="approve-the-private-endpoint-connection"></a>Genehmigen der Verbindung mit einem privaten Endpunkt
Genehmigen Sie das Herstellen der Verbindung des privaten Endpunkts mit dem Private Link-Dienst mit „Approve-AzPrivateEndpointConnection“.

```azurepowershell   

$pls = Get-AzPrivateLinkService `
-Name $plsName `
-ResourceGroupName $rgName 

Approve-AzPrivateEndpointConnection -ResourceId $pls.PrivateEndpointConnections[0].Id -Description "Approved" 

``` 

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure Private Link](private-link-overview.md).
 
