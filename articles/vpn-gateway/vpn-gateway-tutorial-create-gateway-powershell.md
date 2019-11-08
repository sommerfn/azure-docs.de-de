---
title: Erstellen und Verwalten des Azure-VPN-Gateways mit PowerShell | Microsoft-Dokumentation
description: 'Tutorial: Erstellen und Verwalten des VPN-Gateways mit dem Azure PowerShell-Modul'
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: bfec1493492fb1e8e9bd7394aae3db8983f4cff9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495662"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Tutorial: Erstellen und Verwalten eines VPN-Gateways mit PowerShell

Azure VPN-Gateways ermöglichen standortübergreifende Konnektivität zwischen lokalen Kundenumgebungen und Azure. In diesem Tutorial werden grundlegende Aufgaben der Azure VPN-Gatewaybereitstellung beschrieben, z.B. Erstellen und Verwalten eines VPN-Gateways. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines VPN-Gateways
> * Anzeigen der öffentlichen IP-Adresse
> * Ändern der Größe eines VPN-Gateways
> * Zurücksetzen eines VPN-Gateways

Im folgenden Diagramm sind das virtuelle Netzwerk und das VPN-Gateway dargestellt, die im Rahmen dieses Tutorials erstellt wurden.

![VNet und VPN-Gateway](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell und Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Allgemeine Netzwerkparameterwerte

Nachfolgend finden Sie die Parameterwerte, die für dieses Tutorial verwendet werden. In den Beispielen werden die Variablen wie folgt übersetzt:

```
#$RG1         = The name of the resource group
#$VNet1       = The name of the virtual network
#$Location1   = The location region
#$FESubnet1   = The name of the first subnet
#$BESubnet1   = The name of the second subnet
#$VNet1Prefix = The address range for the virtual network
#$FEPrefix1   = Addresses for the first subnet
#$BEPrefix1   = Addresses for the second subnet
#$GwPrefix1   = Addresses for the GatewaySubnet
#$VNet1ASN    = ASN for the virtual network
#$DNS1        = The IP address of the DNS server you want to use for name resolution
#$Gw1         = The name of the virtual network gateway
#$GwIP1       = The public IP address for the virtual network gateway
#$GwIPConf1   = The name of the IP configuration
```

Ändern Sie die unten angegebenen Werte auf der Grundlage Ihrer Umgebung und Netzwerkeinrichtung. Kopieren Sie anschließend die Variablen, und fügen Sie sie ein, um sie für dieses Tutorial festzulegen. Wenn für Ihre Cloud Shell-Sitzung ein Timeout auftritt oder Sie ein anderes PowerShell-Fenster verwenden müssen, kopieren Sie die Variablen, fügen Sie sie in der neuen Sitzung ein, und setzen Sie dann das Tutorial fort.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Zuerst muss eine Ressourcengruppe erstellt werden. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *TestRG1* in der Region *East US* (USA, Osten) erstellt:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Das Azure VPN-Gateway ermöglicht standortübergreifende Konnektivität und P2S-VPN-Serverfunktionalität für Ihr virtuelles Netzwerk. Fügen Sie das VPN-Gateway einem vorhandenen virtuellen Netzwerk hinzu, oder erstellen Sie ein neues virtuelles Netzwerk und das Gateway. Beachten Sie, dass im Beispiel der Name des Gatewaysubnetzes spezifisch angegeben wird. Sie müssen den Namen des Gatewaysubnetzes immer als „GatewaySubnet“ angeben, damit es ordnungsgemäß funktioniert. In diesem Beispiel wird ein neues virtuelles Netzwerk mit drei Subnetzen erstellt: Front-End, Back-End und Gatewaysubnetz mit [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) und [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -AddressPrefix $GwPrefix1
$vnet   = New-AzVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>Anfordern einer öffentlichen IP-Adresse für das VPN Gateway

Azure VPN-Gateways kommunizieren mit Ihren lokalen VPN-Geräten über das Internet, um die IKE-Aushandlung (Internet Key Exchange, Internetschlüsselaustausch) und IPsec-Tunnel einzurichten. Verwenden Sie wie unten dargestellt [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) und [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig), um eine öffentliche IP-Adresse zu erstellen und Ihrem VPN-Gateway zuzuweisen:

> [!IMPORTANT]
> Derzeit können Sie nur eine dynamische öffentliche IP-Adresse für das Gateway verwenden. Die statische IP-Adresse wird auf Azure VPN-Gateways nicht unterstützt.

```azurepowershell-interactive
$gwpip    = New-AzPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-a-vpn-gateway"></a>Erstellen eines VPN-Gateways

Das Erstellen eines VPN-Gateways kann 45 Minuten oder länger dauern. Nachdem die Erstellung des Gateways abgeschlossen wurde, können Sie eine Verbindung Ihres virtuellen Netzwerks mit einem anderen virtuellen Netzwerk herstellen. Alternativ können Sie eine Verbindung zwischen dem virtuellen Netzwerk und einem lokalen Standort herstellen. Erstellen Sie mit dem Cmdlet [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) ein VPN-Gateway.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Wichtige Parameterwerte:
* GatewayType: Verwenden Sie **Vpn** für Site-to-Site- und VNET-zu-VNET-Verbindungen.
* VpnType: Verwenden Sie **RouteBased**, um mit einem größeren Bereich von VPN-Geräten und mehr Routingfeatures zu interagieren.
* GatewaySku: Die Standardeinstellung ist **VpnGw1**. Ändern Sie diese in eine andere VpnGw-SKU, wenn Sie einen höheren Durchsatz oder mehr Verbindungen benötigen. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Wenn Sie „TryIt“ verwenden, kann ein Timeout für die Sitzung auftreten. Das ist kein Problem. Das Gateway wird trotzdem erstellt.

Nachdem die Erstellung des Gateways abgeschlossen ist, können Sie eine Verbindung zwischen Ihrem virtuellen Netzwerk und einem anderen VNet oder eine Verbindung zwischen Ihrem virtuellen Netzwerk und einem lokalen Speicherort herstellen. Außerdem können Sie von einem Clientcomputer aus eine P2S-Verbindung mit Ihrem VNet konfigurieren.

## <a name="view-the-gateway-public-ip-address"></a>Anzeigen der öffentlichen IP-Adresse des Gateways

Wenn Ihnen der Name der öffentlichen IP-Adresse bekannt ist, können Sie die öffentliche IP-Adresse, die dem Gateway zugewiesen ist, mit [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) anzeigen.

Falls Ihre Sitzung abgelaufen ist, kopieren Sie die allgemeinen Netzwerkparameter vom Anfang dieses Tutorials in die neue Sitzung, und fahren Sie anschließend fort.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Ändern der Größe eines Gateways

Sie können die VPN-Gateway-SKU ändern, nachdem das Gateway erstellt wurde. Unterschiedliche Gateway-SKUs unterstützen verschiedene Spezifikationen, z.B. Durchsätze, Verbindungsanzahl usw. Im folgenden Beispiel wird [Resize-AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) verwendet, um die Größe Ihres Gateways von „VpnGw1“ in „VpnGw2“ zu ändern. Weitere Informationen finden Sie unter [Gateway-SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Eine Änderung der VPN-Gatewaygröße dauert ca. 30 bis 45 Minuten, aber bei diesem Vorgang werden vorhandene Verbindungen und Konfigurationen **nicht** unterbrochen oder entfernt.

## <a name="reset-a-gateway"></a>Zurücksetzen eines Gateways

Im Rahmen der Schritte für die Problembehandlung können Sie Ihr Azure VPN-Gateway zurücksetzen, um für das VPN-Gateway die IPsec-/IKE-Tunnelkonfigurationen neu zu starten. Verwenden Sie [Reset-AzVirtualNetworkGateway](/powershell/module/az.network/Reset-azVirtualNetworkGateway), um Ihr Gateway zurückzusetzen.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Weitere Informationen finden Sie unter [Zurücksetzen eines VPN-Gateways](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die hier verwendeten Ressourcen sind eine Voraussetzung für das [nächste Tutorial](vpn-gateway-tutorial-vpnconnection-powershell.md). Wenn Sie mit diesem Tutorial fortfahren möchten, sollten Sie sie daher beibehalten.

Falls das Gateway jedoch Teil einer Prototyp-, Test- oder Proof of Concept-Bereitstellung ist, können Sie die Ressourcengruppe, das VPN-Gateway und alle zugehörigen Ressourcen mit dem Befehl [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) entfernen.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zur grundlegenden Erstellung und Verwaltung von VPN-Gateways erhalten, z.B.:

> [!div class="checklist"]
> * Erstellen eines VPN-Gateways
> * Anzeigen der öffentlichen IP-Adresse
> * Ändern der Größe eines VPN-Gateways
> * Zurücksetzen eines VPN-Gateways

Fahren Sie mit den folgenden Tutorials fort, um sich über S2S-, VNET-zu-VNET- und P2S-Verbindungen zu informieren.

> [!div class="nextstepaction"]
> * [Erstellen von S2S-Verbindungen](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Erstellen von VNet-zu-VNet-Verbindungen](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Erstellen von P2S-Verbindungen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
