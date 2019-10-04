---
title: Virtuelles Azure-Netzwerk | Microsoft-Dokumentation
description: Enthält Informationen zu den Konzepten und Features des virtuellen Azure-Netzwerks (Azure Virtual Network).
services: virtual-network
documentationcenter: na
author: anavinahar
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: anavin
ms.openlocfilehash: 22c1e3050915fc697a62862620ef492ef22f80b8
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542832"
---
# <a name="what-is-azure-virtual-network"></a>Was ist Azure Virtual Network?

Azure Virtual Network (VNET) ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. Mit VNET können zahlreiche Arten von Azure-Ressourcen (beispielsweise virtuelle Azure-Computer) sicher untereinander sowie mit dem Internet und mit lokalen Netzwerken kommunizieren. VNET ähnelt einem herkömmlichen Netzwerk, das Sie in Ihrem Rechenzentrum betreiben, bietet jedoch zusätzliche Vorteile der Infrastruktur von Azure, z. B. Skalierbarkeit, Verfügbarkeit und Isolation.

## <a name="vnet-concepts"></a>VNET-Konzepte

- **Adressraum:** Beim Erstellen eines VNET müssen Sie einen benutzerdefinierten privaten IP-Adressraum mit öffentlichen und privaten Adressen (RFC 1918) eingeben. Azure weist Ressourcen in einem virtuellen Netzwerk eine private IP-Adresse aus dem von Ihnen zugewiesenen Adressraum zu. Wenn Sie beispielsweise einen virtuellen Computer in einem VNET mit dem Adressraum 10.0.0.0/16 bereitstellen, wird er einer privaten IP-Adresse zugewiesen, z. B. 10.0.0.4.
- **Subnetze:** Mithilfe von Subnetzen können Sie das virtuelle Netzwerk in ein oder mehrere Subnetze segmentieren und jedem Subnetz einen Teil des Adressraums des virtuellen Netzwerks zuordnen. Dann können Sie Azure-Ressourcen in einem bestimmten Subnetz bereitstellen. Wie in einem herkömmlichen Netzwerk können Sie mit Subnetzen Ihren VNET-Adressraum in Segmente unterteilen, die für das interne Netzwerk des Unternehmens geeignet sind. Dadurch wird auch die Adresszuordnung optimiert. Sie können Ressourcen in Subnetzen mit Netzwerksicherheitsgruppen sichern. Weitere Informationen finden Sie unter [Sicherheitsgruppen](security-overview.md).
- **Regionen:** Ein VNET ist auf eine Region oder einen Standort begrenzt. Mehrere virtuelle Netzwerke aus verschiedenen Regionen können jedoch über Peering virtueller Netzwerke miteinander verbunden werden.
- **Abonnement:** Ein VNET ist auf ein Abonnement begrenzt. Sie können in jedem Azure-[Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) und in jeder Azure-[Region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) mehrere virtuelle Netzwerke implementieren.

## <a name="best-practices"></a>Bewährte Methoden

Beim Erstellen Ihres Netzwerks in Azure sind folgende allgemeine Entwurfsprinzipien zu beachten:

- Stellen Sie sicher, dass sich Adressräume nicht überschneiden. Stellen Sie sicher, dass der Adressraum Ihres VNET (CIDR-Block) sich nicht mit anderen Netzwerkbereichen Ihrer Organisation überschneidet.
- Die Subnetze sollten nicht den gesamten Adressraum des VNET ausmachen. Planen Sie voraus, und reservieren Sie Adressraum für die Zukunft.
- Es empfiehlt sich, einige große VNETs und nicht viele kleine VNETs zu erstellen. Dadurch verringert sich der Verwaltungsaufwand.
- Schützen Sie Ihr VNET mit Netzwerksicherheitsgruppen (NSGs).

## <a name="communicate-with-the-internet"></a>Kommunikation mit dem Internet

Alle Ressourcen in einem VNET können standardmäßig in ausgehender Richtung mit dem Internet kommunizieren. Zur Kommunikation in eingehender Richtung muss der entsprechenden Ressource eine öffentliche IP-Adresse oder eine öffentliche Load Balancer-Instanz zugewiesen werden. Die öffentliche IP-Adresse bzw. die öffentliche Load Balancer-Instanz kann auch zum Verwalten der ausgehenden Verbindungen verwendet werden.  Weitere Informationen zu ausgehenden Verbindungen in Azure finden Sie unter [Ausgehende Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md), [Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse](virtual-network-public-ip-address.md) und [Was versteht man unter Azure Load Balancer?](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>Wenn Sie nur eine interne Instanz von [Load Balancer Standard](../load-balancer/load-balancer-standard-overview.md) verwenden, ist ausgehende Konnektivität erst verfügbar, wenn Sie definieren, wie [ausgehende Verbindungen](../load-balancer/load-balancer-outbound-connections.md) mit einer öffentlichen IP auf Instanzebene oder einer öffentlichen Load Balancer-Instanz verwendet werden sollen.

## <a name="communicate-between-azure-resources"></a>Kommunikation zwischen Azure-Ressourcen

Azure-Ressourcen können auf eine der folgenden Arten sicher miteinander kommunizieren:

- **Über ein virtuelles Netzwerk**: Sie können virtuelle Computer und verschiedene andere Arten von Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, wie beispielsweise Azure App Service-Umgebungen, den Azure Kubernetes Service (AKS) und Azure Virtual Machine Scale Sets. Eine vollständige Liste mit Azure-Ressourcen, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie unter [Integration virtueller Netzwerke für Azure-Dienste](virtual-network-for-azure-services.md).
- **Über einen VNET-Dienstendpunkt**: Erweitern Sie den Bereich privater Adressen und die Identität Ihres virtuellen Netzwerks über eine direkte Verbindung auf Azure-Dienstressourcen wie Azure Storage-Konten und Azure SQL-Datenbank-Instanzen. Mithilfe von Dienstendpunkten können Sie Ihre kritischen Azure-Dienstressourcen auf ein virtuelles Netzwerk beschränken und so schützen. Weitere Informationen finden Sie in der Übersicht über [VNET-Dienstendpunkte](virtual-network-service-endpoints-overview.md).
- **Über VNET-Peering:** Sie können virtuelle Netzwerke mittels VNet-Peering miteinander verbinden und so die Kommunikation zwischen Ressourcen in beiden virtuellen Netzwerken ermöglichen. Die verbundenen virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen befinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](virtual-network-peering-overview.md).

## <a name="communicate-with-on-premises-resources"></a>Kommunikation mit lokalen Ressourcen

Sie können Ihre lokalen Computer und Netzwerke unter Verwendung einer beliebigen Kombination der folgenden Optionen mit einem virtuellen Netzwerk verbinden:

- **Point-to-Site-VPN (virtuelles privates Netzwerk)** : Wird zwischen einem virtuellen Netzwerk und einem einzelnen Computer in Ihrem Netzwerk eingerichtet. Jeder Computer, der eine Verbindung mit einem virtuellen Netzwerk herstellen möchte, muss eine eigene Verbindung konfigurieren. Dieser Verbindungstyp ist gut geeignet, wenn Azure noch neu für Sie ist, oder wenn Sie ein Entwickler sind, da keine oder nur sehr geringe Änderungen Ihres vorhandenen Netzwerks erforderlich sind. Die Kommunikation zwischen Ihrem Computer und einem virtuellen Netzwerk wird durch einen verschlüsselten Tunnel über das Internet gesendet. Weitere Informationen finden Sie unter [Point-to-Site (VPN über IKEv2 oder SSTP)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Site-to-Site-VPN**: Wird zwischen Ihrem lokalen VPN-Gerät und einem Azure-VPN-Gateway eingerichtet, das in einem virtuellen Netzwerk bereitgestellt ist. Bei diesem Verbindungstyp können alle lokalen Ressourcen, die von Ihnen autorisiert werden, auf ein virtuelles Netzwerk zugreifen. Die Kommunikation zwischen Ihrem lokalen VPN-Gerät und einem Azure-VPN-Gateway wird durch einen verschlüsselten Tunnel über das Internet gesendet. Weitere Informationen finden Sie unter [Site-to-Site und Multi-Site (IPsec-/IKE-VPN-Tunnel)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute**: Wird zwischen Ihrem Netzwerk und Azure über einen ExpressRoute-Partner eingerichtet. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Weitere Informationen finden Sie unter [ExpressRoute (private Verbindung)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Filtern des Netzwerkdatenverkehrs

Sie können den Netzwerkdatenverkehr zwischen Subnetzen filtern, indem Sie eine oder beide folgenden Optionen verwenden:

- **Sicherheitsgruppen**: Netzwerksicherheitsgruppen und Anwendungssicherheitsgruppen können mehrere Sicherheitsregeln für die eingehende und ausgehende Richtung enthalten, mit denen Sie den Datenverkehr in beiden Richtungen nach Quell- und Ziel-IP-Adresse, Port und Protokoll filtern können. Weitere Informationen siehe [Netzwerksicherheitsgruppen](security-overview.md#network-security-groups) und [Anwendungssicherheitsgruppen](security-overview.md#application-security-groups).
- **Virtuelle Netzwerkgeräte**: Ein virtuelles Netzwerkgerät ist ein virtueller Computer, der eine Netzwerkfunktion (Firewall, WAN-Optimierung oder Ähnliches) übernimmt. Eine Liste mit verfügbaren virtuellen Netzwerkgeräten, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Weiterleiten von Netzwerkdatenverkehr

Azure leitet standardmäßig Datenverkehr zwischen Subnetzen, verbundenen virtuellen Netzwerken, lokalen Netzwerken und dem Internet weiter. Sie können eine oder beide der folgenden Optionen implementieren, um die von Azure erstellten Standardrouten außer Kraft zu setzen:

- **Routingtabellen**: Sie können benutzerdefinierte Routingtabellen mit Routen erstellen, über die gesteuert wird, wohin der Datenverkehr für die einzelnen Subnetze geleitet wird. Weitere Informationen zu Routingtabellen finden Sie [hier](virtual-networks-udr-overview.md#user-defined).
- **BGP-Routen (Border Gateway Protocol)** : Wenn Sie Ihr virtuelles Netzwerk mit Ihrem lokalen Netzwerk über ein Azure-VPN-Gateway oder eine ExpressRoute-Verbindung verbinden, können Sie Ihren virtuellen Netzwerken Ihre lokalen BGP-Routen weitergeben. Weitere Informationen zur Verwendung von BGP mit dem Azure-VPN-Gateway finden Sie [hier](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Informationen zur Verwendung mit ExpressRoute finden Sie [hier](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="azure-vnet-limits"></a>Einschränkungen von Azure VNET

Es gibt bestimmte Grenzwerte für die Anzahl der Azure-Ressourcen, die Sie bereitstellen können. Die meisten Grenzwerte für Azure-Netzwerke sind auf die Maximalwerte festgelegt. Sie können jedoch [bestimmte Grenzwerte für Netzwerke erhöhen](../azure-supportability/networking-quota-requests.md), wie unter [Grenzwerte für Netzwerke](../azure-subscription-service-limits.md#networking-limits) angegeben. 

## <a name="pricing"></a>Preise

Die Verwendung von Azure VNET ist kostenlos, es fallen keine Gebühren an. Für Ressourcen wie virtuelle Computer (VMs) und andere Produkte gelten Standardgebühren. Weitere Informationen finden Sie unter [Virtual Network – Preise](https://azure.microsoft.com/pricing/details/virtual-network/) und im [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/).

## <a name="next-steps"></a>Nächste Schritte

 Erstellen Sie als Nächstes ein virtuelles Netzwerk, stellen Sie einige virtuelle Computer darin bereit, und kommunizieren Sie zwischen den virtuellen Computern, um sich mit der Verwendung virtueller Netzwerke vertraut zu machen. Eine entsprechende Anleitung finden Sie unter [Erstellen eines virtuellen Netzwerks im Azure-Portal](quick-create-portal.md).
