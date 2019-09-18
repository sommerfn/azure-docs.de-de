---
title: Konfigurieren und Überprüfen von VNet- oder VPN-Verbindungen
description: Schrittweise Anleitung zum Konfigurieren und Überprüfen verschiedener Azure-VPN- und VNet-Bereitstellungen
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 0433a4f4-b5a0-476d-b398-1506c57eafa2
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: fc4b649ce8d082d8d854c4c19b617c088ff3141c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901434"
---
# <a name="configure-and-validate-vnet-or-vpn-connections"></a>Konfigurieren und Überprüfen von VNet- oder VPN-Verbindungen

Diese exemplarische Vorgehensweise enthält eine schrittweise Anleitung zum Konfigurieren und Überprüfen verschiedener Azure-VPN- und VNet-Bereitstellungen in Szenarios wie Transitrouting, VNet-zu-VNet-, BGP-, Multi-Standort-, Punkt-zu-Standort-Verbindung (Point-to-Site) usw.

Azure-VPN-Gateways bieten Flexibilität bei der Zusammenstellung fast jeder Art von Topologie für verbundene virtuelle Netzwerke (VNets) in Azure: Sie können VNets über Regionen hinweg, zwischen VNet-Typen (Azure Resource Manager bzw. Klassisch), in Azure oder mit lokaler Hybridumgebung, in unterschiedlichen Abonnements usw. verbinden. 

## <a name="scenario-1-vnet-to-vnet-vpn-connection"></a>Szenario 1: VNet-zu-VNet-VPN-Verbindung

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (VNet-zu-VNet) über VPN ist dem Verbinden eines VNet mit einem lokalen Standort sehr ähnlich. Beide Verbindungstypen verwenden ein VPN-Gateway, um einen sicheren Tunnel mit **IPsec/IKE** bereitzustellen. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen.

![IMAGE](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Abbildung 1: VNet-zu-VNet mit IPSec

Wenn sich Ihre VNets in derselben Region befinden, bietet es sich an, diese über ein VNet-Peering zu verbinden, für das kein VPN-Gateway verwendet und mit dem der Durchsatz erhöht und die Wartezeit (Latenz) verringert wird. Wählen Sie **Configure and validate VNet Peering** (VNet-Peering konfigurieren und überprüfen) aus, wenn Sie eine VNet-Peeringverbindung konfigurieren möchten.

Wurden Ihre beiden VNets mit dem Azure Resource Manager-Bereitstellungsmodell erstellt, wählen Sie **Configure and validate a Resource Manager VNet to a Resource Manager VNet connection** (Verbindung zwischen zwei Ressource Manager-VNets konfigurieren und überprüfen) aus, um eine VPN-Verbindung zu konfigurieren.

Wurde eines der VNets mit dem klassischen Azure-Bereitstellungsmodell, das andere mit Resource Manager erstellt, wählen Sie **Configure and validate a classic VNet to a Resource Manager VNet connection** (Verbindung zwischen klassischem VNet und Resource Manager-VNet konfigurieren und überprüfen) aus, um eine VPN-Verbindung zu konfigurieren.

### <a name="configuration-1-configure-vnet-peering-for-two-vnets-in-the-same-region"></a>Konfiguration 1: Konfigurieren von VNet-Peering für zwei VNets in derselben Region

Bevor Sie mit der Implementierung von Azure-VNet-Peering beginnen, sollten Sie sich vergewissern, dass die folgenden Voraussetzungen für ein Konfigurieren von VNet-Peering erfüllt sind:

* Die beiden mittels Peering verknüpften virtuellen Netzwerke müssen sich in der gleichen Azure-Region befinden.
* Die mittels Peering verknüpften virtuellen Netzwerke dürfen keine überlappenden IP-Adressräume haben.
* Das VNET-Peering erfolgt zwischen zwei virtuellen Netzwerken. Es besteht keine abgeleitete transitive Beziehung zwischen Peerings. Wenn beispielsweise VNetA mittels Peering mit VNetB verknüpft ist und VNetB mittels Peering mit VNetC verknüpft ist, ist VNetA *nicht* mittels Peering mit VNetC verknüpft.

Sind diese Anforderungen erfüllt, können Sie die Schritte im [Tutorial: Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke mit dem Azure-Portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) ausführen, um das VNet-Peering zu erstellen und zu konfigurieren.

Verwenden Sie die folgenden Methoden, um die Konfiguration des VNet-Peerings zu überprüfen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Konto an, dem die erforderlichen [Rollen und Berechtigungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) zugewiesen sind.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie ein Peering erstellen möchten.
4. Klicken Sie im Bereich, der für das von Ihnen ausgewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** auf **Peerings**.
5. Klicken Sie auf das Peering, dessen Konfiguration Sie überprüfen möchten.

![IMAGE](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
Führen Sie mit Azure PowerShell den Befehl [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) aus, um das Peering virtueller Netzwerke abzurufen. Beispiel:

```
PS C:\Users\User1> Get-AzureRmVirtualNetworkPeering -VirtualNetworkName Vnet10-01 -ResourceGroupName dev-vnets
Name                             : LinkToVNET10-02
Id                               : /subscriptions/GUID/resourceGroups/dev-vnets/providers/Microsoft.Network/virtualNetworks/VNET10-01/virtualNetworkPeerings/LinkToVNET10-0
2
Etag                             : W/"GUID"
ResourceGroupName                : dev-vnets
VirtualNetworkName               : vnet10-01
ProvisioningState                : Succeeded
RemoteVirtualNetwork             : {
                                  "Id": "/subscriptions/GUID/resourceGroups/DEV-VNET
                                   s/providers/Microsoft.Network/virtualNetworks/VNET10-02"
                                   }
AllowVirtualNetworkAccess        : True
AllowForwardedTraffic            : False
AllowGatewayTransit              : False
UseRemoteGateways                : False
RemoteGateways                   : null
RemoteVirtualNetworkAddressSpace : null
```

### <a name="connection-type-1-connect-a-resource-manager-vnet-to-anther-resource-manager-vnet-azure-resource-manager-to-azure-resource-manager"></a>Verbindungstyp 1: Verbinden eines Resource Manager-VNet mit einem anderen Ressource Manager-VNet (Azure Resource Manager zu Azure Resource Manager)

Sie können eine Verbindung von einem Resource Manager-VNet mit einem anderen Ressource Manager-VNet direkt konfigurieren, oder Sie können die Verbindung mit IPSec konfigurieren.

### <a name="configuration-2-configure-vpn-connection-between-resource-manager-vnets"></a>Konfiguration 2: Konfigurieren einer VPN-Verbindung zwischen Resource Manager-VNets

Informationen zum Konfigurieren einer Verbindung zwischen Resource Manager-VNets ohne IPSec finden Sie unter [Konfigurieren einer VNet-zu-VNet-VPN-Gatewayverbindung über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal).

Wenn Sie eine Verbindung mit IPSec zwischen zwei Resource Manager-VNets konfigurieren möchten, führen Sie die Schritte 1 bis 5 unter [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) aus.

> [!Note]
> Diese Schritte gelten nur für VNets in demselben Abonnement. Wenn sich Ihre VNets in unterschiedlichen Abonnements befinden, müssen Sie PowerShell zum Herstellen der Verbindung verwenden. Informationen hierzu finden Sie im Artikel über [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps).

### <a name="validate-vpn-connection-between-resource-manager-vnets"></a>Überprüfen einer VPN-Verbindung zwischen Resource Manager-VNets

![IMAGE](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

Abbildung 4: Verbindung zwischen klassischem VNet und Azure Resource Manager-VNet

Um zu überprüfen, ob Ihre VPN-Verbindung ordnungsgemäß konfiguriert ist, gehen Sie wie folgt vor:

> [!Note]
> In den folgenden Schritten entspricht die Nummer hinter einer Komponente des virtuellen Netzwerks, etwa „VNet 1“, der jeweiligen Nummer in Abbildung 4.

1. Überprüfen Sie auf überlappende Adressräume in den verbundenen VNets.

   > [!Note]
   > In VNet 1 und VNet 6 dürfen keine überlappenden Adressräume vorhanden sein. 

2. Überprüfen Sie, ob der Adressbereich von Azure Resource Manager-VNet 1 im **Verbindungsobjekt** 4 ordnungsgemäß definiert ist.
3. Überprüfen Sie, ob der Adressbereich von Azure Resource Manager-VNet 6 im **Verbindungsobjekt** 3 ordnungsgemäß definiert ist.
4. Überprüfen Sie, ob die vorinstallierten Schlüssel in den beiden Verbindungsobjekten 3 und 4 übereinstimmen.
5. Überprüfen Sie, ob die virtuelle IP-Adresse (VIP) von Gateway 2 des Azure Resource Manager-VNet im **Verbindungsobjekt** 4 ordnungsgemäß definiert ist.
6. Überprüfen Sie, ob die virtuelle IP-Adresse (VIP) von Gateway 5 des Azure Resource Manager-VNet im **Verbindungsobjekt** 3 ordnungsgemäß definiert ist.

### <a name="connection-type-2-connect-a-classic-vnet-to-a-resource-manager-vnet"></a>Verbindungstyp 2: Verbinden eines klassischen VNet mit einem Resource Manager-VNet

Sie können auch eine Verbindung zwischen VNets in unterschiedlichen Abonnements und Regionen erstellen. Es ist auch möglich, VNets zu verbinden, für die es bereits Verbindungen mit lokalen Netzwerken gibt. Einzige Voraussetzung dafür ist, dass Sie den Gatewaytyp als „Routenbasiert“ konfiguriert haben.

Informationen, wie eine Verbindung zwischen einem klassischen VNet und einem Resource Manager-VNet konfiguriert wird, finden Sie unter [Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal).

![IMAGE](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Abbildung 5: Verbindung zwischen klassischem VNet und Azure Resource Manager-VNet

Um die Konfiguration zu überprüfen, wenn Sie ein klassisches VNet mit einem Azure Resource Manager-VNet verbinden, gehen Sie wie folgt vor:

> [!Note]
> In den folgenden Schritten entspricht die Nummer hinter einer Komponente des virtuellen Netzwerks, beispielsweise „VNet 1“, der jeweiligen Nummer in Abbildung 5.

1. Überprüfen Sie auf überlappende Adressräume in den verbundenen VNets.

   > [!Note]
   > In VNet 1 und VNet 6 dürfen keine überlappenden Adressräume vorhanden sein.

2. Überprüfen Sie, ob der Adressbereich von Azure Resource Manager-VNet 6 in der Definition des klassischen lokalen Netzwerks 3 ordnungsgemäß definiert ist.
3. Überprüfen Sie, ob der Adressbereich des klassischen VNet 1 im Azure Resource Manager-**Verbindungsobjekt** 4 ordnungsgemäß definiert ist.
4. Überprüfen Sie, ob die VIP von Gateway 2 des klassischen VNet im Azure Resource Manager-**Verbindungsobjekt** 4 ordnungsgemäß definiert ist.
5. Überprüfen Sie, ob die VIP von Gateway 5 des Azure Resource Manager-VNet in der Definition des klassischen lokalen Netzwerks 3 (**Local Network Definition** 3) ordnungsgemäß definiert ist.
6. Überprüfen Sie, ob die vorinstallierten Schlüssel in beiden verbundenen VNets übereinstimmen:
   1. Klassisches VNet: Definition des lokalen Netzwerks 3
   2. Azure Resource Manager-VNet: Verbindungsobjekt 4

## <a name="scenario-2-point-to-site-vpn-connection"></a>Szenario 2: Punkt-zu-Standort-VPN-Verbindung

Mit einer P2S-Konfiguration (Point-to-Site) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. Point-to-Site-Verbindungen sind nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder bei einer Konferenz, eine Verbindung mit Ihrem VNet herstellen möchten. Diese Methode eignet sich auch, wenn Sie nur wenige Clients besitzen, die mit einem virtuellen Netzwerk verbunden werden müssen. Die P2S-VPN-Verbindung wird über den nativen Windows-VPN-Client vom Clientcomputer aus initiiert. Die Clients, die Verbindungen herstellen, verwenden Zertifikate zur Authentifizierung.

![IMAGE](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Abbildung 2: Punkt-zu-Standort-Verbindung (Point-to-Site-Verbindung)

Für Punkt-zu-Standort-Verbindungen ist kein VPN-Gerät erforderlich. P2S erstellt die VPN-Verbindung über SSTP (Secure Socket Tunneling Protocol). Sie können eine Punkt-zu-Standort-Verbindung (Point-to-Site-Verbindung) mit einem VNet mit verschiedenen Bereitstellungstools und Bereitstellungsmodellen konfigurieren:

* [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der Zertifikatauthentifizierung (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connections"></a>Überprüfen Ihrer Punkt-zu-Standort-Verbindungen

Im Artikel [Troubleshooting: Azure Point-to-Site-Verbindungsprobleme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) sind die üblichen Probleme aufgeführt, die es mit Punkt-zu-Standort-Verbindungen (Point-to-Site-Verbindungen) geben kann.

## <a name="scenario-3-multi-site-vpn-connection"></a>Szenario 3: Multi-Standort-VPN-Verbindung

Sie können eine Standort-zu-Standort-Verbindung (Site-to-Site-Verbindung, S2S) zu einem VNet hinzufügen, für das es bereits eine S2S-Verbindung, eine Punkt-zu-Standort-Verbindung oder eine VNet-zu-VNet-Verbindung gibt. Diese Art von Verbindung wird häufig als **Multi-Standort**-Konfiguration bezeichnet. 

![IMAGE](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

Abbildung 3: Multi-Standort-Verbindung

In Azure können derzeit zwei Bereitstellungsmodelle verwendet werden: Resource Manager-Bereitstellung und klassische Bereitstellung. Die beiden Modelle sind nicht vollständig miteinander kompatibel. Informationen zum Konfigurieren einer **Multi-Standort**-Verbindung mit unterschiedlichen Modellen finden Sie in den folgenden Artikeln:

* [Hinzufügen einer Site-to-Site-Verbindung (S2S) zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Hinzufügen einer Standort-zu-Standort-Verbindung mit einem VNet über eine vorhandene VPN-Gatewayverbindung (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Diese Schritte gelten nicht für Konfigurationen von parallel bestehenden ExpressRoute- und S2S-Verbindungen. Weitere Informationen zu parallel bestehenden Verbindungen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

## <a name="scenario-4-configure-transit-routing"></a>Szenario 4: Konfigurieren von Transitrouting

Transitrouting ist ein bestimmtes Routingszenario, in dem Sie mehrere Netzwerke in einer Verkettungstopologie verbinden. Diese Art von Routing ermöglicht es Ressourcen in VNets an beiden Enden der Kette, über dazwischenliegende VNets zu kommunizieren. Ohne Transitrouting können Netzwerke oder Geräte, für die das Peering über einen Hub erfolgt, keine Verbindungen miteinander herstellen.

### <a name="configuration-1-configure-transit-routing-in-a-point-to-site-connection"></a>Konfiguration 1: Konfigurieren von Transitrouting in einer Punkt-zu-Standort-Verbindung

In diesem Szenario konfigurieren Sie eine Standort-zu-Standort-VPN-Verbindung zwischen VNetA und VNetB, und Sie konfigurieren eine Punkt-zu-Standort-VPN-Verbindung für einen Client, um eine Verbindung mit dem Gateway von VNetA herzustellen. Anschließend möchten Sie das Transitrouting für die Punkt-zu-Standort-Clients aktivieren, um eine Verbindung mit VNetB herzustellen, wobei die Verbindung durch VNetA verläuft. Dieses Szenario wird unterstützt, wenn BGP für die Standort-zu-Standort-VPN-Verbindung zwischen VNetA und VNetB aktiviert ist. Weitere Informationen hierzu finden Sie unter [Informationen zu Point-to-Site-VPN-Routing](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

### <a name="configuration-2-configure-transit-routing-in-an-expressroute-connection"></a>Konfiguration 2: Konfigurieren von Transitrouting in einer ExpressRoute-Verbindung

Mit Microsoft Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine dedizierte private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Mit ExpressRoute können Sie Verbindungen zu Microsoft-Clouddiensten herstellen, z.B. Microsoft Azure, Office 365 und Dynamics 365.  Weitere Informationen finden Sie unter [ExpressRoute-Übersicht](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![IMAGE](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

Abbildung 6: ExpressRoute-Verbindung mit Azure-VNets über „Privates Peering“

> [!Note]
> Wenn sich VNetA und VNetB in derselben geopolitischen Region befinden, empfiehlt es sich, [beide VNets mit der ExpressRoute-Leitung zu verbinden](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm), anstatt Transitrouting zu konfigurieren. Befinden sich Ihre VNets in unterschiedlichen geopolitischen Regionen, können Sie die VNets auch direkt mit der Leitung verbinden, wenn Sie mit [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium) arbeiten. 

Verwenden Sie parallel bestehende ExpressRoute- und Standort-zu-Standort-Verbindungen, wird Transitrouting nicht unterstützt. Weitere Informationen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

Haben Sie über ExpressRoute Ihre lokalen Netzwerke mit einem virtuellen Azure-Netzwerk verbunden, können Sie VNet-Peering zwischen den VNets aktivieren, für die Sie Transitrouting benötigen. Damit Ihre lokalen Netzwerke eine Verbindung mit dem Remote-VNet herstellen können, müssen Sie das [Peering virtueller Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity) konfigurieren. 

> [!Note]
> VNet-Peering ist nur für VNets in derselben Region verfügbar.

Gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie Transitrouting für VNet-Peering konfiguriert haben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Konto an, dem die erforderlichen [Rollen und Berechtigungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) zugewiesen sind.
2. [Erstellen Sie ein Peering zwischen VNetA und VNetB](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) wie im Diagramm weiter oben (Abbildung 6). 
3. Klicken Sie in dem Bereich, der für das von Ihnen ausgewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** auf **Peerings**.
4. Klicken Sie auf das Peering, das Sie anzeigen möchten, und klicken Sie auf **Konfiguration**, um zu überprüfen, ob Sie **Gatewaytransit zulassen** für das VNetA, das mit der ExpressRoute-Leitung verbunden ist, und **Remotegateways verwenden** für das Remote-VNetB aktiviert haben, das nicht mit der ExpressRoute-Leitung verbunden ist.

### <a name="configuration-3-configure-transit-routing-in-a-vnet-peering-connection"></a>Konfiguration 3: Konfigurieren von Transitrouting in einer VNet-Peeringverbindung

Wenn virtuelle Netzwerke mittels Peering verknüpft sind, können Sie auch das Gateway im mittels Peering verknüpften virtuellen Netzwerk als Transitpunkt für ein lokales Netzwerk konfigurieren. Informationen, wie Transitrouting in VNet-Peering konfiguriert wird, finden Sie unter [Konfigurieren einer VNet-zu-VNet-VPN-Gatewayverbindung mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json).

> [!Note]
> Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die mit unterschiedlichen Bereitstellungsmodellen erstellt wurden, nicht unterstützt. Sie müssen beide virtuellen Netzwerke in der Peeringbeziehung mit dem Resource Manager-Bereitstellungsmodell erstellen, um den Gatewaytransit verwenden zu können.

Gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie Transitrouting für VNet-Peering konfiguriert haben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Konto an, dem die erforderlichen [Rollen und Berechtigungen](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) zugewiesen sind.
2. Geben Sie oben im Azure-Portal im Feld mit dem Text „Ressourcen“ suchen die Zeichenfolge *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** auf das virtuelle Netzwerk, für das Sie die Peeringeinstellung überprüfen möchten.
4. Klicken Sie in dem Bereich, der für das von Ihnen ausgewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** auf **Peerings**.
5. Klicken Sie auf das Peering, das Sie anzeigen möchten, und vergewissern Sie sich, dass **Gatewaytransit zulassen** und **Remotegateways verwenden** unter **Konfiguration** aktiviert sind.

![IMAGE](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configuration-4-configure-transit-routing-in-a-vnet-to-vnet-connection"></a>Konfiguration 4: Konfigurieren von Transitrouting in einer VNet-zu-VNet-Verbindung

Wenn Sie Transitrouting zwischen VNets konfigurieren möchten, müssen Sie BGP für alle dazwischenliegenden VNet-zu-VNet-Verbindungen aktivieren, indem Sie das Resource Manager-Bereitstellungsmodell und PowerShell verwenden. Anweisungen finden Sie unter [Konfigurieren von BGP für Azure-VPN-Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Transitdatenverkehr über ein Azure-VPN-Gateway ist möglich, indem das klassische Bereitstellungsmodells verwendet wird, basiert jedoch auf statisch definierten Adressräumen aus der Netzwerkkonfigurationsdatei. BGP wird noch nicht für virtuelle Azure-Netzwerke und VPN-Gateways unterstützt, wenn das klassische Bereitstellungsmodell verwendet wird. Ohne BGP ist ein manuelles Definieren von Transitadressräumen fehleranfällig und nicht zu empfehlen.

> [!Note]
> Klassische VNet-zu-VNet-Verbindungen werden über das Azure-Portal (klassisch) oder über eine Netzwerkkonfigurationsdatei im klassischen Portal konfiguriert. Sie können ein klassisches virtuelles Netzwerk nicht über das Azure Resource Manager-Bereitstellungsmodell oder das Azure-Portal erstellen oder ändern. Weitere Informationen zum Transitrouting für klassische VNets finden Sie unter [Hub&Spoke, Daisy-Chain and Full-Mesh VNET topologies in Azure ARM using VPN (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

### <a name="configuration-5-configure-transit-routing-in-a-site-to-site-connection"></a>Konfiguration 5: Konfigurieren von Transitrouting in einer Standort-zu-Standort-Verbindung

Wenn Sie das Transitrouting zwischen Ihrem lokalen Netzwerk und einem VNet mit einer Standort-zu-Standort-Verbindung konfigurieren möchten, müssen Sie BGP für alle dazwischenliegenden Standort-zu-Standort-Verbindungen aktivieren, indem Sie das Resource Manager-Bereitstellungsmodell und PowerShell verwenden. Weitere Informationen hierzu finden Sie unter [Konfigurieren von BGP für Azure VPN Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Transitdatenverkehr über ein Azure-VPN-Gateway ist möglich, indem das klassische Bereitstellungsmodells verwendet wird, basiert jedoch auf statisch definierten Adressräumen aus der Netzwerkkonfigurationsdatei. BGP wird noch nicht für virtuelle Azure-Netzwerke und VPN-Gateways unterstützt, wenn das klassische Bereitstellungsmodell verwendet wird. Ohne BGP ist ein manuelles Definieren von Transitadressräumen fehleranfällig und nicht zu empfehlen.

> [!Note]
> Klassische Standort-zu-Standort-Verbindungen werden über das Azure-Portal (klassisch) oder über eine Netzwerkkonfigurationsdatei im klassischen Portal konfiguriert. Sie können ein klassisches virtuelles Netzwerk nicht über das Azure Resource Manager-Bereitstellungsmodell oder das Azure-Portal erstellen oder ändern. Weitere Informationen zum Transitrouting für klassische VNets finden Sie unter [Hub&Spoke, Daisy-Chain and Full-Mesh VNET topologies in Azure ARM using VPN (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

## <a name="scenario-5-configure-bgp-for-a-vpn-gateway"></a>Szenario 5: Konfigurieren von BGP für ein VPN-Gateway

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet Routing- und Erreichbarkeitsinformationen zwischen mehreren Netzwerken ausgetauscht werden. Wenn BGP im Kontext von virtuellen Azure-Netzwerken verwendet wird, aktiviert BGP die Azure-VPN-Gateways und ihre lokalen VPN-Geräte, die als BGP-Peers oder Nachbarn bezeichnet werden. Sie tauschen „Routen“ aus, mit denen beide Gateways über die Verfügbarkeit und Erreichbarkeit für diese Präfixe informiert werden, um die beteiligten Gateways oder Router zu durchlaufen. BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben. Weitere Informationen hierzu finden Sie unter [Übersicht über BGP mit Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

### <a name="configure-bgp-for-a-vpn-connection"></a>Konfigurieren von BGP für eine VPN-Verbindung

Informationen zum Konfigurieren einer VPN-Verbindung, für die BGP verwendet wird, finden Sie unter [Konfigurieren von BGP für Azure VPN Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

> [!Note]
> 1. Aktivieren Sie BGP für das Gateway eines virtuellen Netzwerks, indem Sie eine AS-Nummer für das Gateway erstellen. Gateways im Tarif „Basic“ unterstützen BGP nicht. Um den Tarif (SKU) des Gateways zu überprüfen, navigieren Sie im Azure-Portal auf dem Blatt „VPN Gateway“ zum Abschnitt „Übersicht“. Arbeiten Sie im Tarif **Basic**, müssen Sie den Tarif ([Ändern der Gatewaygröße](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)) in den Tarif **VpnGw1** ändern. Ein Überprüfen des Tarifs führt zu einer Ausfallzeit von 20 bis 30 Minuten. Sobald das Gateway den richtigen Tarif (SKU) hat, kann die AS-Nummer über das PowerShell-Cmdlet [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) hinzugefügt werden. Nachdem Sie die AS-Nummer konfiguriert haben, wird automatisch eine BGP-Peer-IP-Adresse für das Gateway bereitgestellt.
> 2. Dem Gateway des lokalen Netzwerks (LocalNetworkGateway) müssen die AS-Nummer (ASN) und die BGP-Peeradresse **manuell** zugewiesen werden. Sie können die Werte für **-Asn** und **-BgpPeeringAddress** mit dem PowerShell-Cmdlet [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) oder [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) festlegen. Einige AS-Nummern sind für Azure reserviert, und Sie können sie nicht so verwenden, wie dies unter [Übersicht über BGP mit Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview#bgp-faq) beschrieben ist.
> 3. Schließlich muss für die Verbindung (Connection-Objekt) BGP aktiviert werden. Sie können **-EnableBGP** mit [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) oder [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) auf `$True` festlegen.

### <a name="validate-the-bgp-configuration"></a>Überprüfen der BGP-Konfiguration

Wenn Sie überprüfen möchten, ob BGP ordnungsgemäß konfiguriert ist, führen Sie die Cmdlets `get-AzureRmVirtualNetworkGateway` und `get-AzureRmLocalNetworkGateway` aus. Die zu BGP gehörende Ausgabe wird dann im Bestandteil „BgpSettingsText“ angezeigt. Beispiel:  BgpSettingsText:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="scenario-6-highly-available-active-active-vpn-connection"></a>Szenario 6: Hochverfügbare Aktiv/Aktiv-VPN-Verbindung

Die wichtigsten Unterschiede zwischen Aktiv/Aktiv- und Aktiv/Standby-Gateways sind:

* Sie müssen zwei Gateway-IP-Konfigurationen mit zwei öffentlichen IP-Adressen erstellen.
* Sie müssen das *EnableActiveActiveFeature*-Flag festlegen.
* Der Gatewaytarif (-GatewaySku) muss VpnGw1, VpnGw2 oder VpnGw3 sein.

Um für lokale und VNET-zu-VNET-Verbindungen Hochverfügbarkeit zu erzielen, sollten Sie mehrere VPN-Gateways bereitstellen und mehrere parallele Verbindungen zwischen Ihren Netzwerken und Azure herstellen. Eine Übersicht über die Verbindungsoptionen und die Topologie finden Sie unter [Standortübergreifende Verbindungen und VNet-zu-VNet-Verbindungen mit hoher Verfügbarkeit](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable).

Um standortübergreifende Aktiv/Aktiv-Verbindungen und VNet-zu-VNet-Verbindungen zu erstellen, führen Sie die in [Konfigurieren von Aktiv/Aktiv-S2S-VPN-Verbindungen mit Azure VPN Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) aufgeführten Schritte aus, um das Azure-VPN-Gateway im Aktiv/Aktiv-Modus zu konfigurieren.

> [!Note]  
> 1. Wenn Sie dem Gateway des lokalen Netzwerks Adressen hinzufügen und das Gateway für BGP mit Aktiv/Aktiv-Modus aktiviert ist, dürfen Sie *nur die/32-Adressen der BGP-Peers* hinzufügen. Wenn Sie weitere Adressen hinzufügen, werden diese als statische Routen angesehen und haben Vorrang vor BGP-Routen.
> 2. Sie müssen unterschiedliche BGP-ASNs für Ihre lokalen Netzwerke verwenden, die eine Verbindung mit Azure herstellen. (Wenn sie identisch sind, müssen Sie Ihre VNet-ASN ändern, falls Ihr lokales VPN-Gerät bereits die ASN für eine Peerverbindung mit anderen BGP-Nachbarn verwendet.)

## <a name="scenario-7-change-an-azure-vpn-gateway-type-after-deployment"></a>Szenario 7: Ändern eines Azure-VPN-Gatewaytyps nach der Bereitstellung

Sie können den Typ eines Azure-VNet-Gateways nicht direkt von richtlinienbasiert in routenbasiert oder umgekehrt ändern. Sie müssen das Gateway löschen. Danach werden die IP-Adresse und der vorinstallierte Schlüssel (Pre-Shared Key, PSK) nicht beibehalten. Danach können Sie ein neues Gateway des gewünschten Typs erstellen. Führen Sie die folgenden Schritte aus, um ein Gateway zu löschen und zu erstellen:

1. Löschen Sie alle Verbindungen, die dem zu ursprünglichen Gateway zugeordnet sind.
2. Löschen Sie das Gateway über das Azure-Portal, über PowerShell oder über klassisches PowerShell: 
   * [Löschen eines Gateways für virtuelle Netzwerke über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [Löschen eines Gateways des virtuellen Netzwerks mit PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [Löschen eines Gateways des virtuellen Netzwerks mit PowerShell (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. Führen Sie die Schritte unter [Erstellen des VPN-Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#a-namevnetgatewaya4-create-the-vpn-gateway) aus, um das neue Gateway des gewünschten Typs zu erstellen und die VPN-Konfiguration abzuschließen.

> [!Note]
> Dieser Vorgang dauert ca. 60 Minuten.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung bei Konnektivitätsproblemen zwischen Azure-VMs](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

