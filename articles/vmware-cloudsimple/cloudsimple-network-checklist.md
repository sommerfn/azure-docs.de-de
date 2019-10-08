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
ms.openlocfilehash: c31942b198697ddc913f3732fd41409334ff0ef6
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817393"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>Netzwerkvoraussetzungen für die Azure VMware-Lösung von CloudSimple

Die Azure VMware-Lösung von CloudSimple bietet eine private VMware-Cloudumgebung, auf die Benutzer und Anwendungen aus lokalen Umgebungen, über unternehmensverwaltete Geräte sowie aus Azure-Ressourcen zugreifen können. Die Konnektivität wird mithilfe von Netzwerkdiensten wie VPNs und ExpressRoute-Verbindungen bereitgestellt.  Einige der Netzwerkdienste erfordern die Angabe von Netzwerkadressbereichen zum Aktivieren dieser Dienste.  Die Tabellen in diesem Artikel beschreiben den Satz von Adressbereichen und die entsprechenden Dienste, die die angegebenen Adressen verwenden.  Einige der Adressen sind obligatorisch, andere hängen von den Diensten ab, die Sie bereitstellen möchten.  Diese Adressräume sollten sich nicht mit Ihren lokalen Subnetzen, Azure Virtual Network-Subnetzen oder geplanten CloudSimple-Workloadsubnetzen überschneiden.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Netzwerkadressbereiche, die zum Erstellen einer privaten Cloud erforderlich sind

Während der Erstellung des CloudSimple-Diensts und einer privaten Cloud ist der folgende CIDR-Netzwerkbereich erforderlich.

| Name/verwendet für     | BESCHREIBUNG                                                                                                                            | Adressbereich            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| Gateway-CIDR      | Erforderlich für Edgedienste (VPN-Gateways).  Dieser CIDR-Bereich ist während der CloudSimple-Diensterstellung erforderlich und muss aus dem RFC 1918-Adressraum stammen. | /28                      |
| vSphere-/vSAN-CIDR | Erforderlich für VMware-Verwaltungsnetzwerke. Dieser CIDR-Bereich muss während der Erstellung der privaten Cloud angegeben werden.                                    | /24 oder/23 oder/22 oder/21 |

## <a name="network-address-range-required-for-azure-network-connection-to-on-premises-network"></a>Der Netzwerkadressbereich, der für die Azure-Netzwerkverbindung mit dem lokalen Netzwerk erforderlich ist.

Beim Herstellen einer Verbindung aus dem [lokalen Netzwerk mit dem privaten Cloudnetzwerk mithilfe von ExpressRoute](on-premises-connection.md) wird eine Global Reach-Verbindung eingerichtet.  Durch die Verbindung werden Routen über BGP zwischen Ihrem lokalen Netzwerk, dem privaten Cloudnetzwerk und ihren Azure-Netzwerken ausgetauscht.

| Name/verwendet für             | BESCHREIBUNG                                                                                                                                                                             | Adressbereich |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| ExpressRoute-Peering-CIDR | Erforderlich, wenn ExpressRoute Global Reach für lokale Verbindungen verwendet wird. Dieser CIDR-Bereich muss bereitgestellt werden, wenn eine Global Reach-Verbindungsanforderung über ein Supportticket erfolgt. | /29           |

## <a name="network-address-range-required-for-using-site-to-site-vpn-connection-to-on-premises-network"></a>Der Netzwerkadressbereich, der für die Site-to-Site-VPN-Verbindung mit dem lokalen Netzwerk erforderlich ist.

Zum Herstellen einer Verbindung aus dem [lokalen Netzwerk mit dem privaten Cloudnetzwerk mithilfe von Site-to-Site-VPN](vpn-gateway.md) sind die folgenden IP-Adressen, das lokale Netzwerk und Bezeichner erforderlich. 

| Adresse/Adressbereich | BESCHREIBUNG                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Peer-IP-Adresse               | Öffentliche IP-Adresse des lokalen VPN-Gateways. Ist erforderlich, um eine Site-to-Site-VPN-Verbindung zwischen dem lokalen Rechenzentrum und der Region des CloudSimple-Diensts herzustellen. Diese IP-Adresse ist während der Erstellung des Site-to-Site-VPN-Gateways erforderlich.                                         |
| Peerbezeichner       | Der Peerbezeichner des lokalen VPN-Gateways. Dieser Bezeichner ist in der Regel mit der **Peer-IP-Adresse** identisch.  Wenn für Ihr lokales VPN-Gateway ein eindeutiger Bezeichner angegeben ist, muss der Bezeichner angegeben werden.  Die Peer-ID ist während der Erstellung des Site-to-Site-VPN-Gateways erforderlich.   |
| Lokale Netzwerke   | Lokale Präfixe, die auf CloudSimple-Netzwerke in der Region zugreifen müssen.  Schließen Sie alle Präfixe aus dem lokalen Netzwerk ein, die auf das CloudSimple-Netzwerk zugreifen, einschließlich des Clientnetzwerks, von dem aus Benutzer zugreifen.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Der Netzwerkadressbereich, der für Point-to-Site-VPN-Verbindungen erforderlich ist.

Point-to-Site-VPN-Verbindungen ermöglichen den Zugriff auf das CloudSimple-Netzwerk über einen Clientcomputer.  Beim [Einrichten eines Point-to-Site-VPN](vpn-gateway.md) müssen die folgenden Netzwerkadressbereiche angegeben werden.

| Adresse/Adressbereich | BESCHREIBUNG                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Clientsubnetz         | DHCP-Adressen werden aus dem Clientsubnetz angegeben, wenn Sie eine Point-to-Site-VPN-Verbindung herstellen. Dieses Subnetz ist beim Erstellen eines Point-to-Site-VPN-Gateways im CloudSimple-Portal erforderlich.  Das Netzwerk wird in zwei Subnetze aufgeteilt. Eines davon wird für UDP-Verbindungen, das andere für TCP-Verbindungen verwendet. |

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen des CloudSimple-Diensts](quickstart-create-cloudsimple-service.md)
* [quickstart-create-private-cloud](quickstart-create-private-cloud.md)
* Weitere Informationen zu [Azure-Netzwerkverbindungen](cloudsimple-azure-network-connection.md)
* Weitere Informationen zu [VPN Gateways](cloudsimple-vpn-gateways.md)
