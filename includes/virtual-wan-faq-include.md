---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0ba69715998f110d88015bdba2fbf340a6b64089
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491639"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpn-gateway"></a>Worin besteht der Unterschied zwischen einem virtuellen Azure-Netzwerkgateway (VPN-Gateway) und einem Azure Virtual WAN-VPN-Gateway?

Virtual WAN ermöglicht eine umfassende Site-to-Site-Konnektivität und ist auf Durchsatz, Skalierbarkeit und Benutzerfreundlichkeit ausgelegt. Wenn Sie einen Standort mit einem Virtual WAN-VPN-Gateway verbinden, unterscheidet sich dies von einem regulären Gateway für virtuelle Netzwerke, für das der Gatewaytyp „VPN“ verwendet wird. Wenn Sie eine ExpressRoute-Leitung mit einem virtuellen WAN-Hub verbinden, wird eine andere Ressource für das ExpressRoute-Gateway als für das reguläre Gateway für virtuelle Netzwerke mit dem Gatewaytyp „ExpressRoute“ verwendet. Beim virtuellen WAN wird sowohl für VPN als auch für ExpressRoute ein aggregierter Durchsatz von bis zu 20 GBit/s unterstützt. Das virtuelle WAN verfügt auch über eine Automatisierung in Bezug auf die Konnektivität mit einem Ökosystem aus CPE-Branchgerät-Partnern. CPE-Branchgeräte weisen eine integrierte Automatisierung auf, die automatisch bereitgestellt wird und für die eine Verbindung mit Azure Virtual WAN hergestellt wird. Diese Geräte sind über ein ständig wachsendes Ökosystem von SD-WAN- und VPN-Partnern erhältlich. Siehe die [Liste der bevorzugten Partner](../articles/virtual-wan/virtual-wan-locations-partners.md).

### <a name="how-is-virtual-wan-different-from-an-azure-virtual-network-gateway"></a>Inwiefern unterscheidet sich Virtual WAN von einem Azure-Gateway für virtuelle Netzwerke?

Das VPN des Gateways für virtuelle Netzwerke ist auf 30 Tunnel begrenzt. Für Verbindungen sollten Sie bei einem größeren VPN-Umfang Virtual WAN verwenden. Sie können bis zu 1.000 Branchverbindungen pro Region (virtueller Hub) mit einer Aggregierung von 20 GBit/s pro Hub verbinden. Eine Verbindung ist ein Aktiv-Aktiv-Tunnel vom lokalen VPN-Gerät zum virtuellen Hub. Sie können einen Hub pro Region nutzen, sodass Sie mehr als 1.000 Branches über Hubs verbinden können.

### <a name="which-device-providers-virtual-wan-partners-are-supported"></a>Welche Geräteanbieter (Virtual WAN-Partner) werden unterstützt?

Derzeit wird die vollständig automatisierte Virtual WAN-Umgebung von vielen Partnern unterstützt. Weitere Informationen finden Sie auf der Seite mit den Informationen zu [Virtual WAN-Partnern](../articles/virtual-wan/virtual-wan-locations-partners.md). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>Was sind die Automatisierungsschritte für Virtual WAN-Partner?

Informationen zu den Automatisierungsschritten für Partner finden Sie unter [Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Muss ich ein Gerät eines bevorzugten Partners nutzen?

Nein. Sie können ein beliebiges VPN-fähiges Gerät nutzen, das die Anforderungen von Azure für die IKEv2/IKEv1-IPsec-Unterstützung erfüllt.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Wie automatisieren Virtual WAN-Partner die Konnektivität mit Azure Virtual WAN?

Softwaredefinierte Konnektivitätslösungen verwalten ihre Branchgeräte normalerweise mithilfe eines Controllers oder über ein Center für die Gerätebereitstellung. Für den Controller können Azure-APIs verwendet werden, um die Konnektivität mit Azure Virtual WAN zu automatisieren. Weitere Informationen finden Sie unter „Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner“.

Softwaredefinierte Konnektivitätslösungen verwalten ihre Branchgeräte normalerweise mithilfe eines Controllers oder über ein Center für die Gerätebereitstellung. Für den Controller können Azure-APIs verwendet werden, um die Konnektivität mit Azure Virtual WAN zu automatisieren. Die Automatisierung umfasst das Hochladen von Branchinformationen, Herunterladen der Azure-Konfiguration, Einrichten von IPSec-Tunneln zu virtuellen Azure-Hubs und automatische Einrichten der Konnektivität vom Branchgerät zu Azure Virtual WAN. Wenn Sie über Hunderte von Branches verfügen, ist das Verbinden über Virtual WAN-CPE-Partner einfach, weil aufgrund des Onboardingverfahrens das aufwändige Einrichten, Konfigurieren und Verwalten der IPsec-Konnektivität entfällt. Weitere Informationen finden Sie unter [Konfigurieren von Virtual WAN-Automatisierung – für Virtual WAN-Partner (Vorschauversion)](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Muss ich ein Gerät eines bevorzugten Partners nutzen?

Nein. Sie können ein beliebiges VPN-fähiges Gerät nutzen, das die Anforderungen von Azure für die IKEv2/IKEv1-IPsec-Unterstützung erfüllt.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>Wie werden SD-WAN-Geräte durch Virtual WAN unterstützt?

Virtual WAN-Partner automatisieren die IPsec-Verbindung mit Azure-VPN-Endpunkten. Wenn es sich bei dem Virtual WAN-Partner um einen SD-WAN-Anbieter handelt, schließt dies ein, dass der SD-WAN-Controller die Automatisierung und IPsec-Verbindung mit Azure-VPN-Endpunkten verwaltet. Wenn das SD-WAN-Gerät anstelle des Azure-VPN einen eigenen Endpunkt für proprietäre SD-WAN-Funktionen erfordert, können Sie den SD-WAN-Endpunkt in einem Azure VNET bereitstellen, das neben Azure Virtual WAN vorhanden ist.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Ändern sich durch Virtual WAN vorhandene Konnektivitätsfeatures?

Es werden keine vorhandenen Azure-Konnektivitätsfeatures geändert.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sind neue Resource Manager-Ressourcen für Virtual WAN verfügbar?
  
Ja. Mit Virtual WAN werden neue Resource Manager-Ressourcen eingeführt. Weitere Informationen finden Sie in der entsprechenden [Übersicht](../articles/virtual-wan/virtual-wan-about.md).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Wie viele VPN-Geräte können sich mit einem einzelnen Hub verbinden?

Pro virtuellem Hub werden bis zu 1.000 Verbindungen unterstützt. Jede Verbindung besteht aus vier Verknüpfungen, und jede Verknüpfungsverbindung unterstützt zwei Tunnel mit einer Aktiv/Aktiv-Konfiguration. Die Tunnel enden in einem virtuellen Azure-Hub (vpngateway).

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Kann das lokale VPN-Gerät eine Verbindung mit mehreren Hubs herstellen?

Ja. Der Datenverkehr erfolgt zu Beginn vom lokalen Gerät zum nächsten Microsoft-Netzwerkedge und dann zum virtuellen Hub.

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Kann ich mein bevorzugtes virtuelles Netzwerkgerät (in einem NVA-VNET) mit Azure Virtual WAN bereitstellen und verwenden?

Ja. Sie können Ihr bevorzugte VNET des virtuellen Netzwerkgeräts mit Azure Virtual WAN verbinden. Verbinden Sie zunächst das VNET des virtuellen Netzwerkgeräts über eine Hub-VNET-Verbindung mit dem Hub. Erstellen Sie dann eine Route für den virtuellen Hub mit einem nächsten Hop, der auf das virtuelle Gerät zeigt. Sie können der Routingtabelle des virtuellen Hubs mehrere Routen zuweisen. Alle Spokes, die mit dem VNET des virtuellen Netzwerkgeräts verbunden sind, müssen außerdem mit dem virtuellen Hub verbunden sein. Dies stellt sicher, dass die Spoke-VNET-Routen an lokale Systeme weitergegeben werden.

### <a name="can-i-create-a-network-virtual-appliance-inside-the-virtual-hub"></a>Kann ich ein virtuelles Netzwerkgerät innerhalb des virtuellen Hubs erstellen?

Ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) kann nicht innerhalb eines virtuellen Hubs bereitgestellt werden. Dafür ist aber die Erstellung in einem Spoke-VNET möglich, das mit dem virtuellen Hub verbunden ist. Sie können im Hub eine Route aktivieren, um Datenverkehr über die NVA-IP-Adresse (der NIC) an das VNET-Ziel zu leiten.

### <a name="can-a-spoke-vnet-have-a-virtual-network-gateway"></a>Kann ein Spoke-VNET über ein Gateway für virtuelle Netzwerke verfügen?

Nein. Das Spoke-VNET kann nicht über ein Gateway für virtuelle Netzwerke verfügen, wenn es mit dem virtuellen Hub verbunden ist.

### <a name="is-there-support-for-bgp"></a>Ist Unterstützung für BGP vorhanden?

Ja. BGP wird unterstützt. Wenn Sie einen VPN-Standort erstellen, können Sie die BGP-Parameter darin angeben. Dies führt dazu, dass alle in Azure für diesen Standort erstellten Verbindungen für BGP aktiviert sind. Wenn Sie über ein VNET mit einem virtuellen Netzwerkgerät verfügen und das VNET des virtuellen Netzwerkgeräts an einen Virtual WAN-Hub angefügt ist, muss BGP für die Spokes, die an das VNET des virtuellen Netzwerkgeräts angefügt sind, deaktiviert werden, um sicherzustellen, dass Routen für ein VNET eines virtuellen Netzwerkgeräts richtig angekündigt werden. Verbinden Sie zusätzlich die Spoke-VNETs mit dem VNET des virtuellen Hubs, um sicherzustellen, dass die Spoke-VNET-Routen an lokale Systeme weitergegeben werden.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Kann ich Datenverkehr per UDR auf dem virtuellen Hub weiterleiten?

Ja. Sie können Datenverkehr mithilfe einer Routingtabelle des virtuellen Hubs an ein VNET weiterleiten. Auf diese Weise können Sie Routen für Ziel-VNETs in Azure über eine spezifische IP-Adresse (normalerweise der NVA-NIC) festlegen.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sind Informationen zu Lizenzen oder Preisen für Virtual WAN vorhanden?

Ja. Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-i-calculate-price-of-a-hub"></a>Wie wird der Preis eines Hubs berechnet?

* Sie zahlen für die Dienste im Hub. Angenommen, Sie verfügen über zehn Branches oder lokale Geräte, für die eine Verbindung mit Azure Virtual WAN erforderlich ist. Dies würde voraussetzen, dass eine Verbindung mit VPN-Endpunkten im Hub hergestellt wird. Bei einem VPN, bei dem 1 Skalierungseinheit 500 MBit/s entspricht, wird dies mit 0,361 USD/Stunde berechnet. Jede Verbindung wird mit 0,05 USD/Stunde berechnet. Für zehn Verbindungen belaufen sich die Gesamtkosten des Diensts auf 0,361 USD + 0,5 USD/Stunde. Zudem fallen Datengebühren für ausgehenden Datenverkehr von Azure an.

* Es fällt auch eine zusätzliche Hubgebühr an. Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/virtual-wan/).

* Wenn Sie das ExpressRoute-Gateway nutzen, weil für ExpressRoute-Leitungen eine Verbindung mit einem virtuellen Hub hergestellt wird, zahlen Sie den Preis für Skalierungseinheiten. Jede Skalierungseinheit von ExpressRoute verfügt über 2 GBit/s, und jede Verbindungseinheit wird zu demselben Preis wie für die VPN-Verbindungseinheit berechnet.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>Wie erfolgt das Onboarding für neue Partner, die nicht in der Liste mit den Einführungspartnern aufgeführt sind?

Alle virtuellen WAN-APIs sind offene APIs. Informationen zur Bewertung der technischen Durchführbarkeit finden Sie in der Dokumentation. Senden Sie bei Fragen eine E-Mail an azurevirtualwan@microsoft.com. Ein idealer Partner verfügt über ein Gerät, das für IKEv1- oder IKEv2-IPSec-Konnektivität bereitgestellt werden kann.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>Was geschieht, wenn ein von mir verwendetes Gerät nicht in der Liste der Virtual WAN-Partner aufgeführt ist? Kann ich über dieses Gerät dennoch eine Verbindung mit dem Azure Virtual WAN-VPN herstellen?

Ja, solange das Gerät IPsec IKEv1 oder IKEv2 unterstützt. Virtual WAN-Partner automatisieren die Konnektivität vom Gerät zu Azure-VPN-Endpunkten. Dies umfasst die Automatisierung von Schritten wie beispielsweise „Hochladen von Branchinformationen“, „IPsec und Konfiguration“ und „Konnektivität“. Da Ihr Gerät nicht aus dem Ökosystem eines Virtual WAN-Partners stammt, müssen Sie die Azure-Konfiguration manuell übernehmen und Ihr Gerät aktualisieren, um eine IPsec-Verbindung einzurichten.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Ist es möglich, Azure Virtual WAN mit einer Resource Manager-Vorlage zu erstellen?

Eine einfache Konfiguration eines Virtual WAN mit einem Hub und einem VPN-Standort kann mit einer [Schnellstartvorlage](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network) erstellt werden. Virtual WAN ist in erster Linie ein von REST oder vom Portal gesteuerter Dienst.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Wird das globale VNET-Peering für Azure Virtual WAN unterstützt? 

Sie können für ein VNET eine Verbindung in einer anderen Region als der Ihres virtuellen WAN herstellen.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Können Spoke-VNETs, die über einen virtuellen Hub verbunden sind, miteinander kommunizieren?

Ja. Spoke-VNETs können direkt über das Peering virtueller Netzwerke kommunizieren. Weitere Informationen finden Sie unter [Peering in virtuellen Netzwerken](../articles/virtual-network/virtual-network-peering-overview.md). Wir unterstützen auch VNETs, die transitiv über den Hub kommunizieren.

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>Was ist eine Branchverbindung mit Azure Virtual WAN?

Für eine Verbindung von einem Branchgerät mit Azure Virtual WAN werden bis zu vier Verknüpfungen unterstützt. Eine Verknüpfung ist die physische Konnektivität am Branchstandort (z. B.: ATT, Verizon usw.). Jede Verknüpfungsverbindung besteht aus zwei IPsec-Tunneln vom Typ „Aktiv/Aktiv“.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Ist die Konnektivität zwischen Branches für Virtual WAN zulässig?

Ja. Die Konnektivität zwischen Zweigstellen ist in Virtual WAN für VPN und VPN zu ExpressRoute zulässig.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Verläuft der Datenverkehr zwischen den Branches über Azure Virtual WAN?

Ja.

### <a name="does-virtual-wan-require-expressroute-from-each-site"></a>Ist für die Virtual WAN-Instanz eine ExpressRoute-Verbindung mit jeder Site erforderlich?

Nein. Für diese Virtual WAN-Instanz ist keine ExpressRoute-Verbindung mit jeder Site erforderlich. Es wird IPsec-Site-to-Site-Standardkonnektivität über Internetlinks vom Gerät zu einem Azure Virtual WAN-Hub genutzt. Ihre Standorte können über eine ExpressRoute-Leitung mit einem Anbieternetzwerk verbunden sein. Standorte, die über ExpressRoute auf einem virtuellen Hub verbunden sind, können Zweigstelle-zu-Zweigstelle-Datenverkehr zwischen VPN und ExpressRoute aufweisen.

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Gibt es bei Verwendung von Azure Virtual WAN einen Grenzwert für den Netzwerkdurchsatz?

Die Anzahl von Branches ist auf 1000 Verbindungen pro Hub/Region und auf maximal 20 GBit/s im Hub begrenzt. Sie können einen Hub pro Region verwenden.

### <a name="how-many-vpn-connections-does-a-virtual-wan-hub-support"></a>Wie viele VPN-Verbindungen werden von einem Virtual WAN-Hub unterstützt?

Ein Azure Virtual WAN-Hub kann bis zu 1.000 Site-to-Site-Verbindungen, 10.000 Point-to-Site-Verbindungen und vier ExpressRoute-Verbindungen gleichzeitig unterstützen.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>Wie hoch ist der VPN-Gesamtdurchsatz eines VPN-Tunnels und einer Verbindung?

Der VPN-Gesamtdurchsatz eines Hubs beträgt basierend auf der ausgewählten Skalierungseinheit bis zu 20 GBit/s. Der Durchsatz wird von allen vorhandenen Verbindungen gemeinsam genutzt. Jeder Tunnel einer Verbindung kann bis zu 1 GBit/s unterstützen.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-the-portal-how-do-i-configure-that"></a>Die 20-Gbit/s-Einstellung für den virtuellen Hub wird im Portal nicht angezeigt. Wie konfiguriere ich dies?

Navigieren Sie im Portal zum VPN-Gateway innerhalb eines Hubs, und klicken Sie auf die Skalierungseinheit, um die entsprechende Einstellung festzulegen.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Lässt Virtual WAN für das lokale Gerät die parallele Nutzung mehrerer ISPs zu, oder wird immer nur ein VPN-Tunnel verwendet?

Eine eingehende Verbindung in ein Virtual WAN-VPN ist immer ein Aktiv/Aktiv-Tunnel (zur Erzielung von Resilienz innerhalb desselben Hubs oder derselben Region), für den eine im Branch verfügbare Verbindung verwendet wird. Bei dieser Verbindung kann es sich um eine ISP-Verbindung im lokalen Branch handeln. Beim Virtual WAN „VPNSite“ können Sie dem Standort Verbindungsinformationen hinzufügen. Falls Sie im Branch über mehrere ISPs verfügen, und jeder ISP eine Verbindung bereitgestellt hat, können diese Informationen in Azure in den VPN-Standortinformationen eingerichtet werden. Die übergreifende Verwaltung von Failovers für die ISPs des Branchs ist ein rein branchzentrierter Routingvorgang.

### <a name="what-is-global-transit-architecture"></a>Was ist die Architektur für globale Übertragungen?

Informationen zur Architektur für globale Übertragungen finden Sie unter [Architektur mit einem globalen Transitnetzwerk und Azure Virtual WAN](../articles/virtual-wan/virtual-wan-global-transit-network-architecture.md).

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Wie wird Datenverkehr über den Azure-Backbone geleitet?

Für den Datenverkehr wird das folgende Muster verwendet: Branchgerät > ISP > Microsoft-Netzwerkedge > Microsoft-Domänencontroller (Hub-VNET) > Microsoft-Netzwerkedge > ISP > Branchgerät

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Was wird bei diesem Modell für jede Site benötigt? Nur eine Internetverbindung?

Ja. Eine Internetverbindung und ein physisches Gerät, das IPsec unterstützt – vorzugsweise von einem unserer integrierten [Virtual WAN-Partner](../articles/virtual-wan/virtual-wan-locations-partners.md). Optional können Sie die Konfiguration und Konnektivität mit Azure auf Ihrem bevorzugten Gerät manuell verwalten.

### <a name="how-do-i-enable-default-route-00000-in-a-connection-vpn-expressroute-or-virtual-network"></a>Vorgehensweise beim Aktivieren der Standardroute (0.0.0.0/0) für eine Verbindung (VPN, ExpressRoute oder Virtual Network):

Ein virtueller Hub kann eine erlernte Standardroute an eine Verbindung vom Typ „Virtuelles Netzwerk“, „Site-to-Site-VPN“ oder „ExpressRoute“ weitergeben, wenn das Flag für die Verbindung auf „Aktiviert“ festgelegt ist. Dieses Flag ist sichtbar, wenn der Benutzer eine VNET-Verbindung, eine VPN-Verbindung oder eine ExpressRoute-Verbindung bearbeitet. Das Flag ist standardmäßig deaktiviert, wenn für eine Site oder eine ExpressRoute-Leitung eine Verbindung mit einem Hub besteht. Es ist standardmäßig aktiviert, wenn eine VNET-Verbindung hinzugefügt wird, um ein VNET mit einem virtuellen Hub zu verbinden. Der Ursprung der Standardroute liegt nicht auf dem Virtual WAN-Hub. Sie wird weitergegeben, wenn sie dem Virtual WAN-Hub bereits bekannt ist, weil darin eine Firewall bereitgestellt wurde, oder wenn für eine andere verbundene Site die Tunnelerzwingung aktiviert ist.

### <a name="what-are-the-differences-between-the-virtual-wan-types-basic-and-standard"></a>Welche Unterschiede bestehen zwischen den Virtual WAN-Typen („Basic“ und „Standard“)?

Beim WAN-Typ „Basic“ können Sie einen einfachen Hub erstellen (SKU = Basic). Beim WAN-Typ „Standard“ können Sie einen Standard-Hub erstellen (SKU = Standard). „Basic“-Hubs sind auf Site-to-Site-VPN-Funktionen beschränkt. Bei „Standard“-Hubs können Sie darüber ExpressRoute, Benutzer-VPN (P2S), Full-Mesh-Hub und die VNET-zu-VNET-Übertragung nutzen. Sie zahlen eine Grundgebühr von 0,25 USD/Stunde für Standard-Hubs und eine Datenverarbeitungsgebühr für die Übertragung zwischen Hubs bei VNET-zu-VNET-Konnektivität sowie für die Datenverarbeitung für Datenverkehr von Hub zu Hub. Weitere Informationen finden Sie im Artikel zum Thema [Virtuelle WANs vom Typ „Basic“ und „Standard“](../articles/virtual-wan/virtual-wan-about.md#basicstandard). Informationen zu den Preisen finden Sie auf der Seite [Virtual WAN – Preise](https://azure.microsoft.com/pricing/details/virtual-wan/).
