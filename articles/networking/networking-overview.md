---
title: Azure-Netzwerke | Microsoft-Dokumentation
description: Erfahren Sie mehr über Netzwerkdienste in Azure und deren Möglichkeiten.
services: networking
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2019
ms.author: kumud
ms.openlocfilehash: 857b38693ca85d6ab397cbe850f0cd530fefc88c
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598390"
---
# <a name="azure-networking"></a>Azure-Netzwerke

Die Netzwerkdienste in Azure bieten eine Vielzahl von Netzwerkfunktionen, die einzeln oder zusammen verwendet werden können. Klicken Sie auf eine der folgenden Schlüsselfunktionen, um weitere Informationen zu erhalten:
- [**Konnektivitätsdienste**](#connect): Verbinden Sie Azure-Ressourcen und lokale Ressourcen über einen oder eine Kombination dieser Netzwerkdienste in Azure: Virtual Network (VNet), Virtual WAN, ExpressRoute, VPN Gateway, Azure DNS oder Azure Bastion.
- [**Anwendungsschutzdienste**](#protect) Schützen Sie Ihre Anwendungen mit einem oder mehreren dieser Netzwerkdienste in Azure: DDoS Protection, Firewall, Netzwerksicherheitsgruppen, Web Application Firewall oder Virtual Network Endpoints.
- [**Anwendungsbereitstellungsdienste**](#deliver) Bereitstellen von Anwendungen im Azure-Netzwerk über einen oder eine Kombination dieser Netzwerkdienste in Azure: Content Delivery Network (CDN), Azure Front Door Service, Traffic Manager, Application Gateway oder Load Balancer.
- [**Netzwerküberwachung**](#monitor) – Überwachen Sie Ihre Netzwerkressourcen mit einem oder mehreren dieser Netzwerkdienste in Azure: Network Watcher, ExpressRoute Monitor, Azure Monitor oder VNet Terminal Access Point (TAP).

## <a name="connect"></a>Konnektivitätsdienste
 
Dieser Abschnitt beschreibt Dienste, die die Konnektivität zwischen Azure-Ressourcen, die Konnektivität von einem lokalen Netzwerk zu Azure-Ressourcen und die Konnektivität von Azure – Virtual Network, ExpressRoute, VPN Gateway, Virtual WAN, DNS und Azure Bastion bereitstellen.

|Dienst|Gründe, die für die Verwendung sprechen:|Szenarien|
|---|---|---|
|[Virtuelles Netzwerk](#vnet)|Ermöglicht es Azure-Ressourcen sicher untereinander sowie mit dem Internet und mit lokalen Netzwerken zu kommunizieren.| <p>[Erstellen von Netzwerksicherheitsgruppen über das Azure-Portal](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[Weiterleiten von Netzwerkdatenverkehr](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[Einschränken des Netzwerkzugriffs auf Ressourcen](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[Herstellen von Verbindungen zwischen virtuellen Netzwerken](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[ExpressRoute](#expressroute)|Dehnt Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Cloud von Microsoft aus.|<p>[Erstellen und Ändern einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[Konfiguration und Verwaltung von Routenfiltern für ExpressRoute-Verbindungen](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN Gateway](#vpngateway)|Sendet verschlüsselten Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem lokalen Standort über das öffentliche Internet.|<p>[Site-to-site-Verbindungen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[VNet-zu-VNet-Verbindungen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[Point-to-Site-Verbindungen](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[Virtual WAN](#virtualwan)|Optimiert und automatisiert die Branchkonnektivität zu und durch Azure. Azure-Regionen dienen als Hubs, die Sie auswählen können, um Branches mit ihnen zu verbinden.|<p>[Site-to-site-Verbindungen](../virtual-wan/virtual-wan-site-to-site-portal.md), [ExpressRoute-Verbindungen](../virtual-wan/virtual-wan-expressroute-portal.md)</p>|
|[Azure DNS](#dns)|Hostet die DNS-Domäne, die eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur ermöglicht.|<p>[Hosten Ihrer Domäne in Azure DNS](../dns/dns-delegate-domain-azure-dns.md)</p><p>[Erstellen von DNS-Einträgen für eine Web-App](../dns/dns-web-sites-custom-domain.md)</p> <p>[Erstellen eines Aliaseintrags für Traffic Manager](../dns/tutorial-alias-tm.md)</p> <p>[Erstellen Sie einen Aliaseintrag für die öffentliche IP-Adresse](../dns/tutorial-alias-pip.md)</p> <p>[Erstellen eines Aliaseintrags für Ressourceneinträge in der Zone](../dns/tutorial-alias-rr.md)</p>|
|[Azure Bastion (Vorschauversion)](#bastion)|Konfigurieren Sie sichere und nahtlose RDP-/SSH-Verbindungen mit Ihren virtuellen Computern über SSL direkt im Azure-Portal. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse.|<p>[Erstellen eines Azure Bastion-Hosts](../bastion/bastion-create-host-portal.md)</p><p>[Verbinden über SSH mit einer Linux-VM](../bastion/bastion-connect-vm-ssh.md)</p><p>[Verbindung über RDP mit einer Windows-VM](../bastion/bastion-connect-vm-rdp.md)</p>|
||||


### <a name="vnet"></a>Virtuelles Netzwerk

Azure Virtual Network (VNET) ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. Sie können ein VNets für folgende Zwecke verwenden:
- **Kommunikation zwischen Azure-Ressourcen**: Sie können virtuelle Computer und verschiedene andere Arten von Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, wie beispielsweise Azure App Service-Umgebungen, den Azure Kubernetes Service (AKS) und Azure Virtual Machine Scale Sets. Eine vollständige Liste mit Azure-Ressourcen, die Sie in einem virtuellen Netzwerk bereitstellen können, finden Sie unter [Integration virtueller Netzwerke für Azure-Dienste](../virtual-network/virtual-network-for-azure-services.md).
- **Kommunikation untereinander**: Sie können virtuelle Netzwerke mittels VNet-Peering miteinander verbinden und so die Kommunikation zwischen Ressourcen in beiden virtuellen Netzwerken ermöglichen. Die verbundenen virtuellen Netzwerke können sich in der gleichen Azure-Region oder in verschiedenen Azure-Regionen befinden. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../virtual-network/virtual-network-peering-overview.md).
- **Kommunikation mit dem Internet**: Alle Ressourcen in einem VNET können standardmäßig in ausgehender Richtung mit dem Internet kommunizieren. Zur Kommunikation in eingehender Richtung muss der entsprechenden Ressource eine öffentliche IP-Adresse oder eine öffentliche Load Balancer-Instanz zugewiesen werden. Sie können die [öffentliche IP-Adressen](../virtual-network/virtual-network-public-ip-address.md) bzw. die öffentliche [Load Balancer-Instanz](../load-balancer/load-balancer-overview.md) auch zum Verwalten Ihrer ausgehenden Verbindungen verwenden.
- **Kommunikation mit lokalen Netzwerken**: Sie können Ihre lokalen Computer und Netzwerke unter Verwendung von [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [ExpressRoute](../expressroute/expressroute-introduction.md) verbinden.

Weitere Informationen finden Sie unter [„Was ist Azure Virtual Network?“](../virtual-network/virtual-networks-overview.md).

### <a name="expressroute"></a>ExpressRoute
Mit ExpressRoute können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Microsoft Azure Cloud ausdehnen. Diese Verbindung ist privat. Der Datenverkehr wird nicht über das Internet übertragen. Mit ExpressRoute können Sie Verbindungen zu Microsoft-Clouddiensten herstellen, z.B. Microsoft Azure, Office 365 und Dynamics 365.  Weitere Informationen finden Sie unter [„Was ist ExpressRoute?“](../expressroute/expressroute-introduction.md).

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpngateway"></a>VPN Gateway
Mit VPN Gateway erstellen Sie verschlüsselte lokale Verbindungen mit Ihrem virtuellen Netzwerk von lokalen Standorten aus, oder erstellen Sie verschlüsselte Verbindungen zwischen VNETs. Für VPN Gateway-Verbindungen stehen verschiedene Konfigurationen zur Verfügung, wie z. B. Site-to-Site, Point-to-Point oder VNet-to-VNet.
Das folgende Diagramm veranschaulicht mehrere Site-to-Site-VPN-Verbindungen zum gleichen virtuellen Netzwerk.

![Site-to-Site Azure VPN Gateway-Verbindungen](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

Weitere Informationen zu den verschiedenen Arten von VPN-Verbindungen finden Sie unter [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="virtualwan"></a>Virtual WAN
Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Branch-Konnektivität mit und durch Azure. Azure-Regionen dienen als Hubs, die Sie auswählen können, um Branches mit ihnen zu verbinden. Sie können das Azure-Backbone nutzen, um auch Verbindungen mit Branches herzustellen und die Branch-to-VNET-Konnektivität zu nutzen. Azure Virtual WAN vereint zahlreiche Azure-Cloudkonnektivitätsdienste, z.B. Site-to-Site-VPN, ExpressRoute, Point-to-Site-Benutzer-VPN in einer einzigen Betriebsschnittstelle. Die Konnektivität mit Azure-VNets wird mithilfe virtueller Netzwerkverbindungen hergestellt. Weitere Informationen finden Sie unter [„Was ist das virtuelle Azure-WAN?“](../virtual-wan/virtual-wan-about.md).

![Virtual WAN-Diagramm](./media/networking-overview/virtualwan1.png)

### <a name="dns"></a>Azure DNS
Azure DNS ist ein Hostingdienst für DNS-Domänen, der eine Namensauflösung mittels Microsoft Azure-Infrastruktur bietet. Indem Sie Ihre Domänen in Azure hosten, können Sie Ihre DNS-Einträge unter Verwendung der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsabläufe wie bei Ihren anderen Azure-Diensten verwalten. Weitere Informationen finden Sie unter [„Was ist Azure DNS?“](../dns/dns-overview.md).

### <a name="bastion"></a>Azure Bastion (Vorschauversion)
Azure Bastion ist ein neuer vollständig verwalteter PaaS-Dienst, den Sie in Ihrem virtuellen Netzwerk bereitstellen können. Dieser Dienst bietet sichere und nahtlose RDP- und SSH-Verbindungen mit Ihren virtuellen Computern über SSL direkt im Azure-Portal. Beim Herstellen einer Verbindung über Azure Bastion benötigen Ihre virtuellen Computer keine öffentliche IP-Adresse. Weitere Informationen finden Sie unter [„Was ist Azure Bastion?“](../bastion/bastion-overview.md).

![Azure Bastion-Architektur](./media/networking-overview/architecture.png)


## <a name="protect"></a>Anwendungsschutzdienste

Dieser Abschnitt beschreibt Netzwerkdienste in Azure, die zum Schutz Ihrer Netzwerkressourcen beitragen: DDoS Protection, Web Application Firewall, Azure Firewall, Netzwerksicherheitsgruppen und Dienstendpunkte.

|Dienst|Gründe, die für die Verwendung sprechen:|Szenario|
|---|---|---|
|[DDoS Protection](#ddosprotection) |Hohe Verfügbarkeit für Ihre Anwendungen mit Schutz vor überhöhten IP-Datenverkehrsgebühren|[Verwalten von Azure DDoS Protection](../virtual-network/manage-ddos-protection.md)|
|[Web Application Firewall](#waf)|<p>[Azure WAF mit Application Gateway](../application-gateway/waf-overview.md) bietet regionalen Schutz für Unternehmen im öffentlichen und privaten Adressraum.</p><p>[Azure WAF mit Front Door](../frontdoor/waf-overview.md) bietet Schutz am Netzwerkedge zu öffentlichen Endpunkten.</p>|<p>[Konfigurieren von Bot-Schutzregeln](../frontdoor/waf-front-door-policy-configure-bot-protection.md)</p> <p>[Konfigurieren von benutzerdefiniertem Antwortcode](../frontdoor/waf-front-door-configure-custom-response-code.md)</p> <p>[Konfigurieren von IP-Einschränkungsregeln](../frontdoor/waf-front-door-configure-ip-restriction.md)</p> <p>[Konfigurieren von Ratenbegrenzungsregeln](../frontdoor/waf-front-door-rate-limit-powershell.md)</p> |
|[Azure Firewall](#firewall)|Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit.|<p>[Bereitstellung einer Azure Firewall in einem Vnet](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[- Bereitstellung einer Azure Firewall in einem Hybridnetzwerk](../firewall/tutorial-hybrid-ps.md)</p> <p>[Filtern von eingehendem Datenverkehr mit Azure Firewall DNAT](../firewall/tutorial-firewall-dnat.md)</p>|
|[Netzwerksicherheitsgruppen](#nsg)|Volle granulare verteilte Endknotensteuerung bei VM/Subnetz für alle Datenflüsse im Netzwerk|[Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/tutorial-filter-network-traffic.md)|
|[VNET-Dienstendpunkte](#serviceendpoints)|Ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken.|[Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
|||
### <a name="ddosprotection"></a>DDoS Protection 
[Azure DDoS Protection](../virtual-network/manage-ddos-protection.md) bietet Gegenmaßnahmen gegen raffinierte DDoS-Bedrohungen. Der Service umfasst erweiterte Funktionen zur Abwehr von DDoS-Angriffen für die in Ihren virtuellen Netzwerken bereitgestellten Anwendungen und Ressourcen. Darüber hinaus haben Kunden, die Azure DDoS Protection einsetzen, Zugang zum DDoS Rapid Response-Support, um DDoS-Experten während einer aktiven Bedrohung zu kontaktieren.

![DDoS Protection](./media/networking-overview/ddos-protection.png)

### <a name="waf"></a>Web Application Firewall

Die Azure Web Application Firewall (WAF) bietet Ihren Webanwendungen Schutz vor gängigen Sicherheitslücken im Web und Schwachstellen wie der Einschleusung von SQL-Befehlen und websiteübergreifenden Skripts. Azure WAF bietet vorkonfigurierten Schutz vor den Top-10-Schwachstellen von OWASP über verwaltete Regeln. Zusätzlich können Kunden auch benutzerdefinierte Regeln konfigurieren, bei denen es sich um von Kunden verwaltete Regeln handelt, die einen zusätzlichen Schutz basierend auf dem Quell-IP-Adressbereich bieten, und Attribute wie Header, Cookies, Formulardatenfelder oder Abzeichenfolge-Parameter anfordern.

Kunden können [Azure WAF mit Application Gateway](../application-gateway/waf-overview.md) bereitstellen, das regionalen Schutz für Unternehmen im öffentlichen und privaten Adressraum bietet. Kunden können sich auch für [Azure WAF mit Front Door](../frontdoor/waf-overview.md) entscheiden, das Schutz am Netzwerkedge zu öffentlichen Endpunkten bietet.

![Web Application Firewall](./media/networking-overview/waf-overview.png)


### <a name="firewall"></a>Azure Firewall
Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Mit Azure Firewall können Sie Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen, die es außenstehenden Firewalls ermöglicht, Datenverkehr aus Ihrem virtuellen Netzwerk zu identifizieren. 

Weitere Informationen zur Azure Firewall finden Sie in der [„Azure Firewall-Dokumentation“](../firewall/overview.md).

![Firewallübersicht](./media/networking-overview/firewall-threat.png)

### <a name="nsg"></a>Netzwerksicherheitsgruppen
Sie können Netzwerkdatenverkehr von und zu Azure-Ressourcen in einem virtuellen Azure-Netzwerk mithilfe einer Netzwerksicherheitsgruppe filtern. Weitere Informationen finden Sie unter [Sicherheitsübersicht](../virtual-network/security-overview.md).

### <a name="serviceendpoints"></a>Dienstendpunkte
Mit Dienstendpunkten von virtuellen Netzwerken (VNETs) werden der Bereich privater Adressen Ihres virtuellen Netzwerks und die Identität Ihres VNET über eine direkte Verbindung auf die Azure-Dienste erweitert. Endpunkte ermöglichen es Ihnen, Ihre kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. Der Datenverkehr aus Ihrem VNET an den Azure-Dienst verbleibt immer im Backbone-Netzwerk von Microsoft Azure. Weitere Informationen finden Sie unter [VNET-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).

![VNET-Dienstendpunkte](./media/networking-overview/vnet-service-endpoints-overview.png)

## <a name="deliver"></a>Dienste für die Anwendungsbereitstellung

Dieser Abschnitt beschreibt Netzwerkdienste in Azure, die bei der Bereitstellung von Anwendungen helfen: Content Delivery Network (CDN), Azure Front Door Service, Traffic Manager, Application Gateway und Load Balancer.

|Dienst|Gründe, die für die Verwendung sprechen:|Szenario|
|---|---|---|
|[Content Delivery Network](#cdn)|Bietet Benutzern Inhalte mit hoher Bandbreite. In CDNs werden zwischengespeicherte Inhalte auf Edgeservern an POP-Standorten (Point of Presence) gespeichert, die sich in der Nähe der Endbenutzer befinden, um die Wartezeit zu verringern.|<p>[Hinzufügen von CDN zu einer Web-App](../cdn/cdn-add-to-web-app.md)</p> <p>[Zugreifen auf Speicherblobs unter Verwendung einer benutzerdefinierten Azure CDN-Domäne über HTTPS](..//cdn/cdn-storage-custom-domain-https.md)</p> <p>[Hinzufügen einer benutzerdefinierten Domäne zum Azure CDN-Endpunkt](../cdn/cdn-map-content-to-custom-domain.md)</p> <p>[Konfigurieren von HTTPS in einer benutzerdefinierten Azure CDN-Domäne](../cdn/cdn-custom-ssl.md?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)</p>|
|[Azure Front Door Service](#frontdoor)|Damit können Sie das globale Routing für Ihren Webdatenverkehr definieren, verwalten und überwachen, indem Sie es für beste Leistung und sofortiges globales Failover bei Hochverfügbarkeit optimieren.|<p>[Hinzufügen einer benutzerdefinierten Domäne für Azure Front Door Service](../frontdoor/front-door-custom-domain.md)</p> <p>[Konfigurieren von HTTPS in einer benutzerdefinierten Front Door-Domäne](../frontdoor/front-door-custom-domain-https.md)</p><p>[Einrichten der Firewallrichtlinie für die Geofilterung von Webanwendungen](../frontdoor/front-door-tutorial-geo-filtering.md)|
|[Traffic Manager](#trafficmanager)|Verteilt den Datenverkehr basierend auf DNS auf Dienste in Azure-Regionen auf der ganzen Welt und sorgt gleichzeitig für Hochverfügbarkeit und Reaktionsfähigkeit.|<p> [Weiterleiten von Datenverkehr für geringe Latenz](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[Weiterleiten von Datenverkehr an einen Prioritätsendpunkt](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [Steuern des Datenverkehrs mit gewichteten Endpunkten](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[Weiterleiten von Datenverkehr basierend auf dem geografischen Standort des Endpunkts](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [Weiterleiten von Datenverkehr basierend auf dem Subnetz des Benutzers](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[Load Balancer](#loadbalancer)|Bietet regionalen Lastausgleich durch Weiterleitung des Datenverkehrs über Verfügbarkeitszonen und in Ihre VNetze. Bietet einen internen Lastenausgleich durch Weiterleitung des Datenverkehrs über und zwischen Ihren Ressourcen, um Ihre regionale Anwendung zu erstellen.|<p> [Vornehmen eines Lastausgleichs für Internetdatenverkehr virtueller Computer](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[Ausführen eines Lastenausgleichs des Datenverkehrs für mehrere VMs in einem virtuellen Netzwerk](../load-balancer/tutorial-load-balancer-basic-internal-portal.md)<p>[Portweiterleitung des Datenverkehrs an einen bestimmten Port auf bestimmten VMs](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [Konfigurieren von Lastenausgleichs- und Ausgangsregeln](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[Application Gateway](#applicationgateway)|Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können.|<p>[Weiterleiten von Webdatenverkehr per Azure Application Gateway](../application-gateway/quick-create-portal.md)</p><p>[Tutorial: Konfigurieren eines Anwendungsgateways mit SSL-Terminierung mithilfe des Azure-Portals](../application-gateway/create-ssl-portal.md)</p><p>[Erstellen eines Anwendungsgateways mit Umleitung auf URL-Pfadbasis](../application-gateway/create-url-route-portal.md) </p>|
|

### <a name="cdn"></a>Content Delivery Network
Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung zur schnellen Übermittlung von Inhalten mit hoher Bandbreite an Benutzer. Hierzu werden Inhalte auf physischen Knoten zwischengespeichert, die strategisch auf der ganzen Welt verteilt sind. Weitere Informationen zum Azure CDN finden Sie unter [„Azure Content Delivery Network“](../cdn/cdn-overview.md).

![Azure CDN](./media/networking-overview/cdn-overview.png)

### <a name="frontdoor"></a>Azure Front Door Service
Über Azure Front Door Service können Sie das globale Routing für Ihren Webdatenverkehr definieren, verwalten und überwachen, indem Sie es für beste Leistung und sofortiges globales Failover bei Hochverfügbarkeit optimieren. Mit Front Door können Sie Ihre globalen (mehrere Regionen) Consumer- und Unternehmensanwendungen in stabile und hochleistungsfähige personalisierte moderne Anwendungen, APIs und Inhalte transformieren, die eine globale Zielgruppe mit Azure erreichen. Weitere Informationen finden Sie unter [Azure Front Door](../frontdoor/front-door-overview.md).


### <a name="trafficmanager"></a>Traffic Manager

Azure Traffic Manager ist ein DNS-basierter Lastenausgleich für Datenverkehr, der es Ihnen ermöglicht, den Datenverkehr optimal auf Dienste in den globalen Azure-Regionen zu verteilen, und gleichzeitig Hochverfügbarkeit und Reaktionsfähigkeit sicherzustellen. Traffic Manager bietet eine Reihe von Routingmethoden für den Datenverkehr zur Verteilung des Datenverkehrs wie Priorität, Gewichtung, Leistung, Geographie, Mehrwertig oder Subnetz. Weitere Informationen zu Routingmethoden für Datenverkehr finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](../traffic-manager/traffic-manager-routing-methods.md).

Das folgende Diagramm zeigt das prioritätsbasierte Routing von Endpunkten mit dem Traffic Manager:

![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](./media/networking-overview/priority.png)

Weitere Informationen über Traffic Manager finden Sie unter [„Was ist Azure Traffic Manager?“](../traffic-manager/traffic-manager-overview.md).

### <a name="loadbalancer"></a>Load Balancer
Der Azure Load Balancer ist äußerst leistungsfähig und bietet Lastenausgleich der Ebene 4 mit niedriger Latenz für alle UDP- und TCP-Protokolle. Der Dienst verwaltet eingehende und ausgehende Verbindungen. Sie können öffentliche und interne Endpunkte mit Lastenausgleich konfigurieren. Sie können Regeln definieren, um eingehende Verbindungen anhand von TCP- und HTTP-Integritätstestoptionen zu Back-End-Pool-Zielen zuzuordnen, um die Dienstverfügbarkeit zu verwalten. Weitere Informationen zum Load Balancer finden Sie im Artikel mit der [Übersicht über den Load Balancer](../load-balancer/load-balancer-overview.md).

Die folgende Abbildung zeigt eine Multi-Tier-Anwendung mit Internetzugriff, die sowohl externen als auch internen Lastenausgleich nutzt:

![Beispiel für Azure Load Balancer](./media/networking-overview/IC744147.png)


### <a name="applicationgateway"></a>Application Gateway
Azure Application Gateway ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können. Es handelt sich um ein ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen auf Schicht 7 für Ihre Anwendung. Weitere Informationen finden Sie unter [„Was ist Azure Application Gateway?“](../application-gateway/overview.md).

Das folgende Diagramm zeigt das auf URL-Pfaden basierende Routing mit Application Gateway:

![Application Gateway-Beispiel](./media/networking-overview/figure1-720.png)

## <a name="monitor"></a>Netzwerküberwachungsdienste
Dieser Abschnitt beschreibt Netzwerkdienste in Azure, die Ihnen helfen, Ihre Netzwerkressourcen zu überwachen: Network Watcher, ExpressRoute Monitor, Azure Monitor und Virtual Network TAP.

|Dienst|Gründe, die für die Verwendung sprechen:|Szenario|
|---|---|---|
|[Network Watcher](#networkwatcher)|Hilft bei der Überwachung und Fehlerbehebung von Konnektivitätsproblemen, hilft bei der Diagnose von VPN-, NSG- und Routingproblemen, erfasst Pakete auf Ihrer VM, automatisiert die Auslösung von Diagnosetools mit Azure Functions und Logic Apps.|<p>[Diagnostizieren eines Problems mit dem Filter für VM-Datenverkehr](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[Diagnostizieren von VM-Routingproblemen](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[Überwachen der Kommunikation zwischen virtuellen Computern](../network-watcher/connection-monitor.md)</p><p>[Diagnostizieren von Problemen mit der Kommunikation zwischen Netzwerken](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[Protokollieren des ein- und ausgehenden Netzwerkdatenverkehrs eines virtuellen Computers](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[ExpressRoute-Monitor](#expressroutemonitor)|Bietet Echtzeitüberwachung der Netzwerkleistung, -verfügbarkeit und -nutzung, hilft bei der automatischen Ermittlung der Netzwerktopologie, bietet eine schnellere Fehlerisolierung, erkennt transiente Netzwerkprobleme, hilft bei der Analyse historischer Leistungsmerkmale des Netzwerks, unterstützt Mehrfachabonnements.|<p>[Konfigurieren des Netzwerkleistungsmonitors für ExpressRoute](../expressroute/how-to-npm.md)</p><p>[ExpressRoute-Überwachung, Metriken und Warnungen](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|[Azure Monitor](#azuremonitor)|Hilft Ihnen, die Leistung Ihrer Anwendungen zu verstehen, und erkennt proaktiv Probleme, die sich auf sie auswirken, und Ressourcen, von denen sie abhängen.|<p>[Traffic Manager-Metriken und -Warnungen](../traffic-manager/traffic-manager-metrics-alerts.md)</p><p>[Azure-Überwachungsdiganose für Load Balancer Standard](../load-balancer/load-balancer-standard-diagnostics.md)</p><p>[Überwachen von Azure Firewall-Protokollen und -Metriken](../firewall/tutorial-diagnostics.md)</p><p>[Überwachung und Protokollierung von Azure Web Application Firewall](../frontdoor/waf-front-door-monitor.md)</p>|
|[TAP eines virtuellen Netzwerks](#vnettap)|Bietet kontinuierliches Streaming des Netzwerkverkehrs virtueller Maschinen zum Paketsammler, ermöglicht Verwaltungslösungen für Netzwerk- und Anwendungsleistung und Sicherheitsanalysetools.|[Erstellen einer VNet TAP-Ressource](../virtual-network/tutorial-tap-virtual-network-cli.md)|
|

### <a name="networkwatcher"></a>Network Watcher
Mit Azure Network Watcher werden Tools für die Überwachung, Diagnose, Metrikanzeige und Aktivierung/Deaktivierung von Protokollen für Ressourcen in einem virtuellen Azure-Netzwerk bereitgestellt. Weitere Informationen finden Sie unter [„Was ist Network Watcher?“](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="expressroutemonitor"></a>ExpressRoute-Monitor
Informationen darüber, wie Sie die ExpressRoute-Metriken zu Leitungen, Diagnoseprotokolle und Warnmeldungen anzeigen können, finden Sie unter [„ExpressRoute-Überwachung, Metriken und Warnungen“](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="azuremonitor"></a>Azure Monitor
Azure Monitor maximiert die Verfügbarkeit und Leistung Ihrer Anwendungen durch die Bereitstellung einer umfassenden Lösung für das Sammeln, Analysieren und Reagieren auf Telemetriedaten aus Ihren cloudbasierten und lokalen Umgebungen. Diese Lösung hilft Ihnen, die Leistung Ihrer Anwendungen zu verstehen, und erkennt proaktiv Probleme, die sich auf sie auswirken, und Ressourcen, von denen sie abhängen. Weitere Informationen finden Sie unter [„Azure Monitor-Übersicht“](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="vnettap"></a>TAP eines virtuellen Netzwerks
Mit dem TAP (Terminal Access Point) für virtuelle Azure-Netzwerke können Sie Ihren VM-Netzwerkdatenverkehr kontinuierlich an einen Netzwerkpaketcollector oder ein Analysetool streamen. Der Collector oder das Analysetool wird von einem Partner für [virtuelle Netzwerkappliances](https://azure.microsoft.com/solutions/network-appliances/) bereitgestellt. 

In der folgenden Abbildung wird gezeigt, wie der TAP des virtuellen Netzwerks funktioniert. 

![Funktionsweise eines TAP eines virtuellen Netzwerks](./media/networking-overview/virtual-network-tap-architecture.png)

Weitere Informationen finden Sie unter [„Was ist der TAP eines virtuelles Netzwerks?“](../virtual-network/virtual-network-tap-overview.md).

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie Ihr erstes VNet, und verbinden Sie einige VMs damit, indem Sie die Schritte des Artikels [Erstellen Ihres ersten virtuellen Netzwerks](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) ausführen.
- Verbinden Sie Ihren Computer über eine Point-to-Site-Verbindung mit einem VNET, indem Sie die Schritte im Artikel [„Konfigurieren einer Point-to-Site-Verbindung“](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) ausführen.
- Führen Sie einen Lastenausgleich des Internetdatenverkehrs zu öffentlichen Servern durch, indem Sie die Schritte im Artikel [Erstellen eines Load Balancers mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) ausführen.
 
 
   
