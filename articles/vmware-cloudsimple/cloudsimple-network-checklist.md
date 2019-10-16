---
title: 'Azure VMware-Lösung von CloudSimple: Netzwerkcheckliste'
description: Checkliste für die Zuordnung von CIDR-Netzwerkbereichen in der Azure VMware-Lösung von CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bfb170036293dc9f519259dc92737f30380aa84a
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244823"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>Netzwerkvoraussetzungen für die Azure VMware-Lösung von CloudSimple

Die Azure VMware-Lösung von CloudSimple bietet eine private VMware-Cloudumgebung, auf die Benutzer und Anwendungen aus lokalen Umgebungen, über unternehmensverwaltete Geräte und von Azure-Ressourcen aus zugreifen können. Die Konnektivität wird mithilfe von Netzwerkdiensten wie VPNs und Azure ExpressRoute-Verbindungen bereitgestellt. Einige dieser Netzwerkdienste erfordern die Angabe von Netzwerkadressbereichen zum Aktivieren der Dienste. 

Die Tabellen in diesem Artikel beschreiben den Satz von Adressbereichen und die entsprechenden Dienste, die die angegebenen Adressen verwenden. Einige der Adressen sind obligatorisch, andere sind von den bereitzustellenden Diensten abhängig. Diese Adressräume sollten sich nicht mit Ihren lokalen Subnetzen, Azure Virtual Network-Subnetzen oder geplanten CloudSimple-Workloadsubnetzen überschneiden.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Netzwerkadressbereiche, die zum Erstellen einer privaten Cloud erforderlich sind

Wenn Sie einen CloudSimple-Dienst und eine private Cloud erstellen, müssen Sie die angegebenen CIDR-Bereiche (Classless Inter-Domain Routing) im Netzwerk wie folgt einhalten.

| Name/Verwendung     | BESCHREIBUNG                                                                                                                            | Adressbereich            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| Gateway-CIDR      | Erforderlich für Edgedienste (VPN-Gateways).  Dieser CIDR-Bereich ist während der CloudSimple-Diensterstellung erforderlich und muss aus dem RFC 1918-Adressraum stammen. | /28                      |
| vSphere-/vSAN-CIDR | Erforderlich für VMware-Verwaltungsnetzwerke. Dieser CIDR-Bereich muss während der Erstellung der privaten Cloud angegeben werden.                                    | /24 oder/23 oder/22 oder/21 |

## <a name="network-address-range-required-for-azure-network-connection-to-an-on-premises-network"></a>Der Netzwerkadressbereich, der für die Azure-Netzwerkverbindung mit einem lokalen Netzwerk erforderlich ist

Beim Herstellen einer Verbindung aus dem [lokalen Netzwerk mit dem privaten Cloudnetzwerk über ExpressRoute](on-premises-connection.md) wird eine Global Reach-Verbindung eingerichtet.  Über diese Verbindung werden Routen mithilfe von BGP (Border Gateway Protocol) zwischen Ihrem lokalen Netzwerk, Ihrem privaten Cloudnetzwerk und Ihren Azure-Netzwerken ausgetauscht.

| Name/Verwendung             | BESCHREIBUNG                                                                                                                                                                             | Adressbereich |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| ExpressRoute-Peering-CIDR | Erforderlich, wenn ExpressRoute Global Reach für lokale Verbindungen verwendet wird. Dieser CIDR-Bereich muss bereitgestellt werden, wenn eine Global Reach-Verbindungsanforderung über ein Supportticket erfolgt. | /29           |

## <a name="network-address-range-required-for-using-a-site-to-site-vpn-connection-to-an-on-premises-network"></a>Der Netzwerkadressbereich, der für eine Site-to-Site-VPN-Verbindung mit einem lokalen Netzwerk erforderlich ist

Zum Herstellen einer Verbindung aus einem [lokalen Netzwerk mit dem privaten Cloudnetzwerk mithilfe von Site-to-Site-VPN](vpn-gateway.md) sind die folgenden IP-Adressen, das lokale Netzwerk und Bezeichner erforderlich. 

| Adresse/Adressbereich | BESCHREIBUNG                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Peer-IP-Adresse               | Öffentliche IP-Adresse des lokalen VPN-Gateways. Erforderlich, um eine Site-to-Site-VPN-Verbindung zwischen einem lokalen Rechenzentrum und der Region des CloudSimple-Diensts herzustellen. Diese IP-Adresse ist während der Erstellung des Site-to-Site-VPN-Gateways erforderlich.                                         |
| Peerbezeichner       | Der Peerbezeichner des lokalen VPN-Gateways. Dieser Bezeichner ist in der Regel mit der **Peer-IP-Adresse** identisch.  Wenn für Ihr lokales VPN-Gateway ein eindeutiger Bezeichner angegeben ist, muss der Bezeichner angegeben werden.  Die Peer-ID ist während der Erstellung des Site-to-Site-VPN-Gateways erforderlich.   |
| Lokale Netzwerke   | Lokale Präfixe, die auf CloudSimple-Netzwerke in der Region zugreifen müssen.  Schließen Sie alle Präfixe aus einem lokalen Netzwerk ein, die auf das CloudSimple-Netzwerk zugreifen sollen, einschließlich des Clientnetzwerks, von dem aus Benutzer auf das Netzwerk zugreifen.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Der Netzwerkadressbereich, der für Point-to-Site-VPN-Verbindungen erforderlich ist.

Eine Point-to-Site-VPN-Verbindung ermöglicht den Zugriff auf das CloudSimple-Netzwerk über einen Clientcomputer.  Zum [Einrichten des Point-to-Site-VPN](vpn-gateway.md) müssen Sie den folgenden Netzwerkadressbereich angeben.

| Adresse/Adressbereich | BESCHREIBUNG                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Clientsubnetz         | DHCP-Adressen werden aus dem Clientsubnetz bereitgestellt, wenn Sie eine Point-to-Site-VPN-Verbindung herstellen. Dieses Subnetz ist beim Erstellen eines Point-to-Site-VPN-Gateways in einem CloudSimple-Portal erforderlich.  Das Netzwerk wird in zwei Subnetze aufgeteilt: eines für UDP-Verbindungen, das andere für TCP-Verbindungen. |

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten der lokalen Firewall für den Zugriff auf Ihre private Cloud](on-premises-firewall-configuration.md)
* [Schnellstart: Erstellen eines CloudSimple-Diensts](quickstart-create-cloudsimple-service.md)
* [Schnellstart: Konfigurieren einer privaten Cloud](quickstart-create-private-cloud.md)
* Weitere Informationen zu [Azure-Netzwerkverbindungen](cloudsimple-azure-network-connection.md)
* Weitere Informationen zu [VPN Gateways](cloudsimple-vpn-gateways.md)
