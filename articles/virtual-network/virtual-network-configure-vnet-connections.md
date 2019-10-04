---
title: Konfigurieren und Überprüfen von Verbindungen virtueller Netzwerke oder VPN-Verbindungen
description: Schrittweise Anleitung zum Konfigurieren und Überprüfen verschiedener Azure-VPN-Bereitstellungen und Bereitstellungen virtueller Netzwerke
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
ms.openlocfilehash: dddf402455292e19bf0fcda3c50d9ce10d5888d2
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71099059"
---
# <a name="configure-and-validate-virtual-network-or-vpn-connections"></a>Konfigurieren und Überprüfen von Verbindungen virtueller Netzwerke oder VPN-Verbindungen

Diese exemplarische Vorgehensweise enthält eine schrittweise Anleitung zum Konfigurieren und Überprüfen verschiedener Azure-VPN-Bereitstellungen und Bereitstellungen virtueller Netzwerke. Zu den Szenarien gehören Transitrouting, Netzwerk-zu-Netzwerk-Verbindungen, Border Gateway Protocol (BGP), Multistandort-Verbindungen und Punkt-zu-Standort-Verbindungen (Point-to-Site-Verbindungen).

Azure-VPN-Gateways bieten die Flexibilität, nahezu jede Topologie für verbundene virtuelle Netzwerke in Azure einrichten zu können. Beispielsweise können Sie virtuelle Netzwerke auf folgende Arten verbinden:

- Über Regionen hinweg
- Zwischen virtuellen Netzwerken verschiedenen Typs (Azure Resource Manager gegenüber klassischem Typ)
- In Azure oder in einer lokalen Hybridumgebung
- In unterschiedlichen Abonnements 

## <a name="network-to-network-vpn-connection"></a>Netzwerk-zu-Netzwerk-VPN-Verbindung

Das Verbinden eines virtuellen Netzwerks mit einem anderen virtuellen Netzwerk (Netzwerk-zu-Netzwerk) über VPN ist dem Verbinden eines virtuellen Netzwerks mit einem lokalen Standort sehr ähnlich. Für beide Verbindungstypen wird ein VPN-Gateway verwendet, um einen sicheren Tunnel über IPsec und IKE bereitzustellen. Die virtuellen Netzwerke können sich in derselben oder in unterschiedlichen Regionen befinden und aus demselben oder unterschiedlichen Abonnements stammen.

![Netzwerk-zu-Netzwerk-Verbindung mit IPSec](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Wenn sich Ihre virtuellen Netzwerke in derselben Region befinden, bietet es sich an, sie über Peering virtueller Netzwerke zu verbinden. Bei Peering virtueller Netzwerke wird kein VPN-Gateway verwendet. Peering erhöht den Durchsatz und verringert die Wartezeit (Latenz). Wenn Sie eine Verbindung mit Peering virtueller Netzwerke konfigurieren möchten, wählen Sie **Configure and validate VNet Peering** (VNet-Peering konfigurieren und überprüfen) aus.

Wurden Ihre virtuellen Netzwerke über das Azure Resource Manager-Bereitstellungsmodell erstellt, wählen Sie **Configure and validate a Resource Manager VNet to a Resource Manager VNet connection** (Verbindung zwischen zwei Ressource Manager-VNets konfigurieren und überprüfen) aus, um eine VPN-Verbindung zu konfigurieren.

Wurde eines der virtuellen Netzwerke über das klassische Azure-Bereitstellungsmodell und das andere über Resource Manager erstellt, wählen Sie **Configure and validate a classic VNet to a Resource Manager VNet connection** (Verbindung zwischen klassischem VNet und Resource Manager-VNet konfigurieren und überprüfen) aus, um eine VPN-Verbindung zu konfigurieren.

### <a name="configure-virtual-network-peering-for-two-virtual-networks-in-the-same-region"></a>Konfigurieren des Peerings virtueller Netzwerke für zwei virtuelle Netzwerke in derselben Region

Bevor Sie mit dem Implementieren und Konfigurieren des Peerings virtueller Azure-Netzwerke beginnen, müssen Sie sich vergewissern, dass die folgenden Voraussetzungen erfüllt sind:

* Die beiden mittels Peering verknüpften virtuellen Netzwerke müssen sich in der gleichen Azure-Region befinden.
* Die über Peering verbundenen virtuellen Netzwerke müssen IP-Adressräume haben, die sich nicht überschneiden.
* Das VNET-Peering erfolgt zwischen zwei virtuellen Netzwerken. Es besteht keine abgeleitete transitive Beziehung zwischen Peerings. Wenn beispielsweise VNetA mittels Peering mit VNetB verknüpft und VNetB mittels Peering mit VNetC verknüpft ist, ist VNetA *nicht* mittels Peering mit VNetC verknüpft.

Sind diese Voraussetzungen erfüllt, können Sie die Schritte im [Tutorial: Herstellen von Verbindungen zwischen virtuellen Netzwerken durch Peerings für virtuelle Netzwerke mit dem Azure-Portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) ausführen, um das Peering zu erstellen und zu konfigurieren.

Verwenden Sie die folgenden Methoden, um die Peeringkonfiguration zu überprüfen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) über ein Konto an, dem die erforderlichen [Rollen und Berechtigungen](virtual-network-manage-peering.md#permissions) zugewiesen sind.
2. Geben Sie oben im Portal im Feld mit dem Text **Ressourcen suchen** die Zeichenfolge **Virtuelle Netzwerke** ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
3. Wählen Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** das virtuelle Netzwerk aus, für das Sie ein Peering erstellen möchten.
4. Klicken Sie in dem Bereich, der für das virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** auf **Peerings**.
5. Wählen Sie ein Peering aus, und sehen Sie sich die Konfigurationsergebnisse an.

![Auswahlmöglichkeiten zum Überprüfen der Konfiguration eines Peerings virtueller Netzwerke](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
Führen Sie mit Azure PowerShell den Befehl [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) aus, um das Peering virtueller Netzwerke abzurufen. Hier sehen Sie ein Beispiel:

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

### <a name="connect-a-resource-manager-virtual-network-to-another-resource-manager-virtual-network"></a>Verbinden eines virtuellen Resource Manager-Netzwerks mit einem anderen virtuellen Resource Manager-Netzwerk

Sie können eine Verbindung von einem virtuellen Resource Manager-Netzwerk mit einem anderen virtuellen Resource Manager-Netzwerk direkt konfigurieren. Alternativ können Sie die Verbindung über IPSec konfigurieren.

### <a name="configure-a-vpn-connection-between-resource-manager-virtual-networks"></a>Konfigurieren einer VPN-Verbindung zwischen virtuellen Resource Manager-Netzwerken

Informationen zum Konfigurieren einer Verbindung zwischen virtuellen Resource Manager-Netzwerken ohne IPSec finden Sie unter [Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal).

Wenn Sie eine Verbindung mit IPSec zwischen zwei virtuellen Resource Manager-Netzwerken konfigurieren möchten, führen Sie für jedes virtuelle Netzwerk die Schritte 1 bis 5 unter [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) aus.

> [!Note]
> Diese Schritte gelten nur für virtuelle Netzwerke im selben Abonnement. Wenn sich Ihre virtuellen Netzwerke in unterschiedlichen Abonnements befinden, müssen Sie PowerShell zum Herstellen der Verbindung verwenden. Informationen hierzu finden Sie im Artikel über [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps).

### <a name="validate-the-vpn-connection-between-resource-manager-virtual-networks"></a>Überprüfen der VPN-Verbindung zwischen virtuellen Resource Manager-Netzwerken

![Verbindung eines klassischen virtuellen Netzwerks mit einem virtuellen Azure Resource Manager-Netzwerk](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

Um zu überprüfen, ob Ihre VPN-Verbindung ordnungsgemäß konfiguriert ist, gehen Sie wie folgt vor.

> [!Note] 
> In den folgenden Schritten entspricht die Nummer hinter einer Komponente des virtuellen Netzwerks der jeweiligen Nummer im vorhergehenden Diagramm.

1. Vergewissern Sie sich, dass es in den verbundenen virtuellen Netzwerken keine überlappenden Adressräume gibt.
2. Vergewissern Sie sich, dass der Adressbereich für das virtuelle Azure Resource Manager-Netzwerk (1) in der **Verbindungsobjekt**-Instanz (4) ordnungsgemäß definiert ist.
3. Vergewissern Sie sich, dass der Adressbereich für das virtuelle Azure Resource Manager-Netzwerk (6) in der **Verbindungsobjekt**-Instanz (3) ordnungsgemäß definiert ist.
4. Überprüfen Sie, ob die vorinstallierten Schlüssel in den Verbindungsobjekten übereinstimmen.
5. Vergewissern Sie sich, dass die virtuelle IP-Adresse (VIP) des Gateways (2) des virtuellen Azure Resource Manager-Netzwerks in der **Verbindungsobjekt**-Instanz (4) ordnungsgemäß definiert ist.
6. Vergewissern Sie sich, dass die virtuelle IP-Adresse (VIP) des Gateways (5) des virtuellen Azure Resource Manager-Netzwerks in der **Verbindungsobjekt**-Instanz (3) ordnungsgemäß definiert ist.

### <a name="connect-a-classic-virtual-network-to-a-resource-manager-virtual-network"></a>Verbinden eines klassischen virtuellen Netzwerks mit einem virtuellen Resource Manager-Netzwerk

Sie können eine Verbindung zwischen virtuellen Netzwerken herstellen, die sich in unterschiedlichen Abonnements und in unterschiedlichen Regionen befinden. Es ist auch möglich, virtuelle Netzwerken zu verbinden, für die es bereits Verbindungen mit lokalen Netzwerken gibt. Einzige Voraussetzung dafür ist, dass Sie den Gatewaytyp als „Routenbasiert“ konfiguriert haben.

Informationen, wie eine Verbindung zwischen einem klassischen virtuellen Netzwerk und einem virtuellen Resource Manager-Netzwerk konfiguriert wird, finden Sie unter [Verbinden von virtuellen Netzwerken aus unterschiedlichen Bereitstellungsmodellen über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal).

![Verbindung eines klassischen virtuellen Netzwerks mit einem virtuellen Azure Resource Manager-Netzwerk](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Um die Konfiguration zu überprüfen, wenn Sie ein klassisches virtuelles Netzwerk mit einem virtuellen Azure Resource Manager-Netzwerk verbinden, gehen Sie wie folgt vor.

> [!Note] 
> In den folgenden Schritten entspricht die Nummer hinter einer Komponente des virtuellen Netzwerks der jeweiligen Nummer im vorhergehenden Diagramm. 

1. Vergewissern Sie sich, dass es in den verbundenen virtuellen Netzwerken keine überlappenden Adressräume gibt.
2. Vergewissern Sie sich, dass der Adressbereich für das virtuelle Azure Resource Manager-Netzwerk (6) in der Definition des klassischen lokalen virtuellen Netzwerks (3) ordnungsgemäß definiert ist.
3. Vergewissern Sie sich, dass der Adressbereich für das klassische virtuelle Netzwerk (1) in der Azure Resource Manager-**Verbindungsobjekt**-Instanz (4) ordnungsgemäß definiert ist.
4. Vergewissern Sie sich, dass die virtuelle IP-Adresse (VIP) des Gateways (2) des klassischen virtuellen Netzwerks in der Azure Resource Manager-**Verbindungsobjekt**-Instanz (4) ordnungsgemäß definiert ist.
5. Vergewissern Sie sich, dass das Gateway (5) des virtuellen Azure Resource Manager-Netzwerks in der Instanz (3) der **Definition des klassischen virtuellen Netzwerks** ordnungsgemäß definiert ist.
6. Überprüfen Sie, ob die vorinstallierten Schlüssel in beiden verbundenen virtuellen Netzwerken übereinstimmen:
   - Klassisches virtuelles Netzwerk: **Definition des lokalen Netzwerks** (3)
   - Virtuelles Azure Resource Manager-Netzwerk: **Verbindungsobjekt** (4)

## <a name="create-a-point-to-site-vpn-connection"></a>Erstellen einer Point-to-Site-VPN-Verbindung

Mit einer Punkt-zu-Standort-Konfiguration (Point-to-Site, *P2S* im folgenden Diagramm) können Sie von einem einzelnen Clientcomputer eine sichere Verbindung mit einem virtuellen Netzwerk herstellen. Point-to-Site-Verbindungen sind nützlich, wenn Sie von einem Remotestandort (beispielsweise von Zuhause oder einer Konferenz) eine Verbindung mit Ihrem virtuellen Netzwerk herstellen möchten. Sie sind ebenfalls nützlich, wenn Sie nur wenige Clients haben, die Verbindungen mit einem virtuellen Netzwerk herstellen müssen. 

Die Point-to-Site-VPN-Verbindung wird über den nativen Windows-VPN-Client vom Clientcomputer aus initiiert. Die Clients, die Verbindungen herstellen, verwenden Zertifikate zur Authentifizierung.

![Point-to-Site-Verbindung](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Für Point-to-Site-Verbindungen ist kein VPN-Gerät erforderlich. Mit ihnen wird die jeweilige VPN-Verbindung über Secure Socket Tunneling Protocol (SSTP) hergestellt. Sie können eine Point-to-Site-Verbindung (Punkt-zu-Standort-Verbindung) mit einem virtuellen Netzwerk über verschiedene Bereitstellungstools und Bereitstellungsmodelle konfigurieren:

* [Konfigurieren einer Point-to-Site-VPN-Verbindung mit einem VNET unter Verwendung der nativen Azure-Zertifikatauthentifizierung: Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Konfigurieren einer Point-to-Site-Verbindung unter Verwendung der Zertifikatauthentifizierung (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der nativen Azure-Zertifikatauthentifizierung: PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connection"></a>Überprüfen Ihrer Point-to-Site-Verbindungen

Im Artikel [Troubleshooting: Azure Point-to-Site-Verbindungsprobleme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) sind die üblichen Probleme aufgeführt, die es mit Punkt-zu-Standort-Verbindungen (Point-to-Site-Verbindungen) geben kann.

## <a name="create-a-multisite-vpn-connection"></a>Erstellen einer Multistandort-VPN-Verbindungen

Sie können einem virtuellen Netzwerk, das bereits eine Standort-zu-Standort-Verbindung (Site-to-Site-Verbindung), eine Punkt-zu-Standort-Verbindung oder eine Netzwerk-zu-Netzwerk-Verbindung hat, eine Standort-zu-Standort-Verbindung (*S2S* im folgenden Diagramm) hinzufügen. Diese Art von Verbindung wird häufig als *Multistandort*-Verbindung bezeichnet. 

![Multistandort-Verbindung](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

In Azure können derzeit zwei Bereitstellungsmodelle verwendet werden: Resource Manager-Bereitstellung und klassische Bereitstellung. Die beiden Modelle sind nicht vollständig miteinander kompatibel. Informationen zum Konfigurieren einer Multistandort-Verbindung mit unterschiedlichen Modellen finden Sie in den folgenden Artikeln:

* [Hinzufügen einer Site-to-Site-Verbindung (S2S) zu einem VNet mit einer vorhandenen VPN-Gatewayverbindung](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Hinzufügen einer Standort-zu-Standort-Verbindung mit einem VNet über eine vorhandene VPN-Gatewayverbindung (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Die Schritte in diesen Artikeln gelten nicht für Konfigurationen von parallel bestehenden Azure ExpressRoute- und Standort-zu-Standort-Verbindungen. Weitere Informationen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

## <a name="configure-transit-routing"></a>Konfigurieren von Transitrouting

Transitrouting ist ein bestimmtes Routingszenario, in dem Sie mehrere Netzwerke in einer Verkettungstopologie verbinden. Diese Art von Routing ermöglicht es Ressourcen in virtuellen Netzwerken an beiden Enden der Kette, über dazwischenliegende virtuelle Netzwerke zu kommunizieren. Ohne Transitrouting können Netzwerke oder Geräte, für die das Peering über einen Hub erfolgt, keine Verbindungen miteinander herstellen.

### <a name="configure-transit-routing-in-a-point-to-site-connection"></a>Konfigurieren von Transitrouting in einer Punkt-zu-Standort-Verbindung

Stellen Sie sich ein Szenario vor, in dem Sie eine Standort-zu-Standort-VPN-Verbindung zwischen VNetA und VNetB konfigurieren möchten. Außerdem möchten Sie eine Punkt-zu-Standort-VPN-Verbindung für den Client konfigurieren, um diesen mit dem Gateway von VNetA zu verbinden. Anschließend möchten Sie das Transitrouting für die Punkt-zu-Standort-Clients aktivieren, um eine Verbindung mit VNetB herzustellen, die durch VNetA führt. 

Dieses Szenario wird unterstützt, wenn BGP für die Standort-zu-Standort-VPN-Verbindung zwischen VNetA und VNetB aktiviert ist. Weitere Informationen hierzu finden Sie unter [Informationen zu Point-to-Site-VPN-Routing](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

### <a name="configure-transit-routing-in-an-expressroute-connection"></a>Konfigurieren von Transitrouting in einer ExpressRoute-Verbindung

Mit Azure ExpressRoute können Sie Ihre lokalen Netzwerke über eine dedizierte private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, in die Microsoft Cloud erweitern. Mit ExpressRoute können Sie Verbindungen zu Microsoft-Clouddiensten herstellen, z.B. Microsoft Azure, Office 365 und Dynamics 365. Weitere Informationen finden Sie unter [ExpressRoute-Übersicht](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Private ExpressRoute-Peering-Verbindung mit virtuellen Azure-Netzwerken](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

> [!Note]
> Wenn sich VNetA und VNetB in derselben geopolitischen Region befinden, empfiehlt es sich, [beide virtuellen Netzwerke mit der ExpressRoute-Leitung zu verbinden](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm), anstatt Transitrouting zu konfigurieren. Befinden sich Ihre virtuellen Netzwerke in unterschiedlichen geopolitischen Regionen, können Sie die Netzwerke auch direkt mit der Leitung verbinden, wenn Sie mit [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium) arbeiten. 

Verwenden Sie parallel bestehende ExpressRoute- und Standort-zu-Standort-Verbindungen, wird Transitrouting nicht unterstützt. Weitere Informationen finden Sie unter [Konfigurieren von parallel bestehenden ExpressRoute- und Standort-zu-Standort-Verbindungen mithilfe von PowerShell](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

Haben Sie über ExpressRoute Ihre lokalen Netzwerke mit einem virtuellen Azure-Netzwerk verbunden, können Sie Peering zwischen den virtuellen Netzwerken aktivieren, für die Sie Transitrouting benötigen. Damit Ihre lokalen Netzwerke eine Verbindung mit dem virtuellen Remotenetzwerk herstellen können, müssen Sie das [Peering virtueller Netzwerke](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity) konfigurieren. 

> [!Note]
> Peering virtueller Netzwerke ist nur für virtuelle Netzwerke in derselben Region verfügbar.

Gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie Transitrouting für Peering virtueller Netzwerke konfiguriert haben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) über ein Konto an, dem die erforderlichen [Rollen und Berechtigungen](virtual-network-manage-peering.md#permissions) zugewiesen sind.
2. [Erstellen Sie ein Peering zwischen VNetA und VNetB](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering), wie dies im vorherigen Diagramm dargestellt ist. 
3. Klicken Sie in dem Bereich, der für das virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** auf **Peerings**.
4. Wählen Sie das Peering aus, das angezeigt werden soll. Wählen Sie dann **Konfiguration** aus, um zu überprüfen, ob Sie **Gatewaytransit zulassen** für das VNetA-Netzwerk, das mit der ExpressRoute-Leitung verbunden ist, und **Remotegateways verwenden** für das Remote-VNetB-Netzwerk aktiviert haben, das nicht mit der ExpressRoute-Leitung verbunden ist.

### <a name="configure-transit-routing-in-a-virtual-network-peering-connection"></a>Konfigurieren von Transitrouting in einer Peeringverbindung virtueller Netzwerke

Wenn virtuelle Netzwerke mittels Peering verknüpft sind, können Sie auch das Gateway im mittels Peering verknüpften virtuellen Netzwerk als Transitpunkt für ein lokales Netzwerk konfigurieren. Informationen, wie eine Transitroute in Peering virtueller Netzwerke konfiguriert wird, finden Sie unter [Konfigurieren einer VNet-zu-VNet-VPN-Gatewayverbindung mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json).

> [!Note]
> Gatewaytransit wird in der Peeringbeziehung zwischen virtuellen Netzwerken, die mit unterschiedlichen Bereitstellungsmodellen erstellt wurden, nicht unterstützt. Sie müssen beide virtuellen Netzwerke in der Peeringbeziehung mit dem Resource Manager-Bereitstellungsmodell erstellen, um Gatewaytransit verwenden zu können.

Gehen Sie folgendermaßen vor, um zu überprüfen, ob Sie eine Transitroute für Peering virtueller Netzwerke konfiguriert haben:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) über ein Konto an, dem die erforderlichen [Rollen und Berechtigungen](virtual-network-manage-peering.md#permissions) zugewiesen sind.
2. Geben Sie oben im Portal im Feld mit dem Text **Ressourcen suchen** die Zeichenfolge **Virtuelle Netzwerke** ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
3. Wählen Sie auf dem angezeigten Blatt **Virtuelle Netzwerke** das virtuelle Netzwerk aus, für das Sie die Peeringeinstellung überprüfen möchten.
4. Wählen Sie in dem Bereich, der für das von Ihnen ausgewählte virtuelle Netzwerk angezeigt wird, im Abschnitt **Einstellungen** die Option **Peerings** aus.
5. Wählen Sie das Peering aus, das Sie anzeigen möchten. Vergewissern Sie sich, dass **Gatewaytransit zulassen** und **Remotegateways verwenden** unter **Konfiguration** aktiviert sind.

![Auswahlmöglichkeiten für die Überprüfung, ob Sie eine Transitroute für das Peering virtueller Netzwerke konfiguriert haben](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configure-transit-routing-in-a-network-to-network-connection"></a>Konfigurieren von Transitrouting in einer Netzwerk-zu-Netzwerk-Verbindung

Wenn Sie Transitrouting zwischen virtuellen Netzwerken konfigurieren möchten, müssen Sie BGP für alle dazwischenliegenden Netzwerk-zu-Netzwerk-Verbindungen aktivieren, indem Sie das Resource Manager-Bereitstellungsmodell und PowerShell verwenden. Anweisungen finden Sie unter [Konfigurieren von BGP für Azure-VPN-Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Transitdatenverkehr über ein Azure-VPN-Gateway ist mit dem klassischen Bereitstellungsmodell möglich, dieses basiert jedoch auf statisch definierten Adressräumen in der Netzwerkkonfigurationsdatei. BGP wird über das klassische Bereitstellungsmodell noch nicht für virtuelle Azure-Netzwerke und VPN-Gateways unterstützt. Ohne BGP ist ein manuelles Definieren von Transitadressräumen fehleranfällig und somit nicht empfehlenswert.

> [!Note]
> Sie können klassische Netzwerk-zu-Netzwerk-Verbindungen konfigurieren, indem Sie das klassische Azure-Portal oder eine Netzwerkkonfigurationsdatei im klassischen Portal verwenden. Sie können ein klassisches virtuelles Netzwerk nicht über das Azure Resource Manager-Bereitstellungsmodell oder das Azure-Portal erstellen oder ändern. Weitere Informationen zum Transitrouting für klassische virtuelle Netzwerke finden Sie im [Microsoft-Blog für Entwickler](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

### <a name="configure-transit-routing-in-a-site-to-site-connection"></a>Konfigurieren von Transitrouting in einer Standort-zu-Standort-Verbindung

Wenn Sie Transitrouting zwischen Ihrem lokalen Netzwerk und einem virtuellen Netzwerk mit einer Standort-zu-Standort-Verbindung konfigurieren möchten, müssen Sie BGP für alle dazwischenliegenden Standort-zu-Standort-Verbindungen aktivieren, indem Sie das Resource Manager-Bereitstellungsmodell und PowerShell verwenden. Anweisungen finden Sie unter [Konfigurieren von BGP für Azure-VPN-Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Transitdatenverkehr über ein Azure-VPN-Gateway ist mit dem klassischen Bereitstellungsmodell möglich, dieses basiert jedoch auf statisch definierten Adressräumen in der Netzwerkkonfigurationsdatei. BGP wird über das klassische Bereitstellungsmodell noch nicht für virtuelle Azure-Netzwerke und VPN-Gateways unterstützt. Ohne BGP ist ein manuelles Definieren von Transitadressräumen fehleranfällig und somit nicht empfehlenswert.

> [!Note]
> Sie können klassische Standort-zu-Standort-Verbindungen konfigurieren, indem Sie das klassische Azure-Portal oder eine Netzwerkkonfigurationsdatei im klassischen Portal verwenden. Sie können ein klassisches virtuelles Netzwerk nicht über das Azure Resource Manager-Bereitstellungsmodell oder das Azure-Portal erstellen oder ändern. Weitere Informationen zum Transitrouting für klassische virtuelle Netzwerke finden Sie im [Microsoft-Blog für Entwickler](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/).

## <a name="configure-bgp-for-a-vpn-gateway"></a>Konfigurieren von BGP für ein VPN-Gateway

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet Routing- und Erreichbarkeitsinformationen zwischen mehreren Netzwerken ausgetauscht werden. Wenn BGP im Kontext von virtuellen Azure-Netzwerken verwendet wird, aktiviert es die Azure-VPN-Gateways und ihre lokalen VPN-Geräte, die als BGP-Peers oder -Nachbarn bezeichnet werden. Sie tauschen „Routen“ aus, mit denen beide Gateways über die Verfügbarkeit und Erreichbarkeit für diese Präfixe informiert werden, um die beteiligten Gateways oder Router zu durchlaufen. 

BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben. Weitere Informationen hierzu finden Sie unter [Übersicht über BGP mit Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

### <a name="configure-bgp-for-a-vpn-connection"></a>Konfigurieren von BGP für eine VPN-Verbindung

Informationen zum Konfigurieren einer VPN-Verbindung, für die BGP verwendet wird, finden Sie unter [Konfigurieren von BGP für Azure VPN Gateways mithilfe von PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

Aktivieren Sie BGP für das Gateway eines virtuellen Netzwerks, indem Sie eine Autonomous System Number (AS-Nummer) für das Gateway erstellen. Gateways im Tarif „Basic“ unterstützen BGP nicht. Um den Tarif (SKU) des Gateways zu überprüfen, navigieren Sie im Azure-Portal auf dem Blatt **VPN Gateway** zum Abschnitt **Übersicht**. Arbeiten Sie im Tarif **Basic**, müssen Sie den Tarif (siehe [Ändern der Gatewaygröße](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)) in **VpnGw1** ändern. 

Ein Überprüfen des Tarifs führt zu einer Ausfallzeit von 20 bis 30 Minuten. Sobald das Gateway den richtigen Tarif (SKU) hat, können Sie die AS-Nummer hinzufügen, indem Sie das PowerShell-Cmdlet [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) verwenden. Nachdem Sie die AS-Nummer konfiguriert haben, wird automatisch eine BGP-Peer-IP-Adresse für das Gateway bereitgestellt.

Sie müssen `LocalNetworkGateway` manuell eine AS-Nummer und eine BGP-Peeradresse bereitstellen. Sie können die Werte für `ASN` und `-BgpPeeringAddress` mit dem PowerShell-Cmdlet [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) oder [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) festlegen. Einige AS-Nummern sind für Azure reserviert, und Sie können sie nicht so verwenden, wie dies unter [Übersicht über BGP mit Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md#faq) beschrieben ist.

Für die Verbindung (Connection-Objekt) muss BGP aktiviert werden. Sie können `-EnableBGP` mit [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) oder [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) auf `$True` festlegen.

### <a name="validate-the-bgp-configuration"></a>Überprüfen der BGP-Konfiguration

Um zu überprüfen, ob BGP ordnungsgemäß konfiguriert ist, können Sie die Cmdlets `get-AzureRmVirtualNetworkGateway` und `get-AzureRmLocalNetworkGateway` ausführen. Danach sehen Sie BGP-bezogene Ausgabe im `BgpSettingsText`-Teil. Beispiel:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="create-a-highly-available-activeactive-vpn-connection"></a>Erstellen einer hochverfügbaren Aktiv/Aktiv-VPN-Verbindung

Die wichtigsten Unterschiede zwischen Aktiv/Aktiv- und Aktiv/Standby-Gateways sind:

* Sie müssen zwei Gateway-IP-Konfigurationen mit zwei öffentlichen IP-Adressen erstellen.
* Sie müssen das **EnableActiveActiveFeature**-Flag festlegen.
* Der Gatewaytarif (-GatewaySku) muss **VpnGw1**, **VpnGw2** oder **VpnGw3** sein.

Um für standortübergreifende und Netzwerk-zu-Netzwerk-Verbindungen Hochverfügbarkeit zu erzielen, sollten Sie mehrere VPN-Gateways bereitstellen und mehrere parallele Verbindungen zwischen Ihren Netzwerken und Azure herstellen. Eine Übersicht über die Verbindungsoptionen und die Topologie finden Sie unter [Standortübergreifende Verbindungen und VNet-zu-VNet-Verbindungen mit hoher Verfügbarkeit](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable).

Um standortübergreifende und Netzwerk-zu-Netzwerk-Verbindungen als Aktiv/Aktiv-Verbindungen zu erstellen, führen Sie die in [Konfigurieren von Aktiv/Aktiv-S2S-VPN-Verbindungen mit Azure VPN Gateways](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) aufgeführten Schritte aus, um ein Azure-VPN-Gateway im Aktiv/Aktiv-Modus zu konfigurieren.

> [!Note]  
> * Wenn Sie dem Gateway des lokalen Netzwerks Adressen hinzufügen und das Gateway für BGP mit Aktiv/Aktiv-Modus aktiviert ist, dürfen Sie *nur die/32-Adressen der BGP-Peers hinzufügen*. Wenn Sie weitere Adressen hinzufügen, werden diese als statische Routen angesehen und haben Vorrang vor BGP-Routen.
> * Sie müssen unterschiedliche BGP-AS-Nummern für Ihre lokalen Netzwerke verwenden, die Verbindungen mit Azure herstellen. (Wenn diese identisch sind, müssen Sie die AS-Nummer Ihres virtuellen Netzwerks ändern, falls Ihr lokales VPN-Gerät die ASN bereits für eine Peerverbindung mit anderen BGP-Nachbarn verwendet.)

## <a name="change-an-azure-vpn-gateway-type-after-deployment"></a>Ändern eines Azure-VPN-Gatewaytyps nach der Bereitstellung

Sie können den Typ eines Gateways eines virtuellen Azure-Netzwerks nicht direkt von richtlinienbasiert in routenbasiert oder umgekehrt ändern. Sie müssen das Gateway zuerst löschen. Danach werden die IP-Adresse und der vorinstallierte Schlüssel nicht beibehalten. Im Anschluss daran können Sie ein neues Gateway des gewünschten Typs erstellen. 

Führen Sie die folgenden Schritte aus, um ein Gateway zu löschen und zu erstellen:

1. Löschen Sie alle Verbindungen, die dem zu ursprünglichen Gateway zugeordnet sind.
2. Löschen Sie das Gateway über das Azure-Portal, über Azure PowerShell oder über klassisches PowerShell: 
   * [Löschen eines Gateways für virtuelle Netzwerke über das Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [Löschen eines Gateways des virtuellen Netzwerks mit PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [Löschen eines Gateways des virtuellen Netzwerks mit PowerShell (klassisch)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. Führen Sie die Schritte unter [Erstellen des VPN-Gateways](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#VNetGateway) aus, um das neue Gateway des gewünschten Typs zu erstellen und die VPN-Konfiguration abzuschließen.

> [!Note]
> Dieser Vorgang dauert ca. 60 Minuten.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung bei Konnektivitätsproblemen zwischen Azure-VMs](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

