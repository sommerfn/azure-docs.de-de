---
title: Verbinden von Azure mit öffentlichen Clouds | Microsoft-Dokumentation
description: Beschreibt verschiedene Möglichkeiten zum Verbinden von Azure mit anderen öffentlichen Clouds.
services: expressroute
author: osamazia
ms.service: expressroute
ms.topic: article
ms.date: 07/24/2019
ms.author: osamaz
ms.openlocfilehash: b8a454c2a104dfe8545cf734bf0b020b8f749bb1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73889628"
---
# <a name="connecting-azure-with-public-clouds"></a>Verbinden von Azure mit öffentlichen Clouds

Viele Unternehmen verfolgen aufgrund geschäftlicher und technischer Ziele eine Multi-Cloud-Strategie. Hierzu gehören Kosten, Flexibilität, Verfügbarkeit von Funktionen, Redundanz, Datenhoheit usw. Diese Strategie hilft Ihnen dabei, das Beste aus beiden Clouds zu nutzen. 

Dieser Ansatz stellt aber auch Herausforderungen an das Unternehmen im Hinblick auf Netzwerk- und Anwendungsarchitektur. Einige dieser Herausforderungen sind Latenz und Datendurchsatz. Um diese Herausforderungen zu meistern, versuchen Kunden direkt eine Verbindung mit mehreren Clouds herzustellen. Einige Dienstanbieter bieten eine Lösung, um für Kunden mehrere Cloudanbieter zu verbinden. In anderen Fällen können Kunden ihre eigenen Router bereitstellen, um mehrere öffentliche Clouds zu verbinden.
## <a name="connectivity-via-expressroute"></a>Konnektivität über ExpressRoute
Mit ExpressRoute können Kunden ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Cloud von Microsoft ausdehnen. Mit ExpressRoute können Kunden Verbindungen mit Microsoft-Clouddiensten herstellen.

Es gibt drei Möglichkeiten, um eine Verbindung über ExpressRoute herzustellen.

1. Layer3-Anbieter
2. Layer2-Anbieter
3. Direkte Verbindung

### <a name="layer3-provider"></a>Layer3-Anbieter

Layer3-Anbieter werden häufig als IP-VPN- oder MPLS VPN-Anbieter bezeichnet. Kunden nutzen diese Anbieter für Mehrpunktkonnektivität zwischen ihren Rechenzentren, Zweigstellen und der Cloud. Kunden stellen mit dem L3-Anbieter eine Verbindung über BGP oder über eine statische Standardroute her. Der Dienstanbieter kündigt Routen zwischen den Kundenstandorten, Rechenzentren und der öffentlichen Cloud an. 
 
Beim Verbinden über einen Layer3-Anbieter kündigt Microsoft VNET-Kundenrouten zum Dienstanbieter über BGP an. Der Anbieter kann über zwei verschiedene Implementierungen verfügen.

![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l3.png)

Der Anbieter kann für jeden Cloudanbieter eine Zielseite in einem separaten VRF-Dienst einrichten, wenn Datenverkehr von allen Cloudanbietern am Kundenrouter ankommt. Wenn der Kunde BGP mit dem Dienstanbieter betreibt, werden diese Routen standardmäßig anderen Cloudanbietern erneut angekündigt. 

Wenn der Dienstanbieter für alle Cloudanbieter im selben VRF-Dienst eine Zielseite einrichtet, werden Routen anderen Cloudanbietern direkt vom Dienstanbieter angekündigt. Dies setzt einen standardmäßiger BGP-Betrieb voraus, bei dem eBGP-Routen anderen eBGP-Nachbarn standardmäßig angekündigt werden.

Jede öffentliche Cloud besitzt ein anderes Präfixlimit, weshalb der Dienstanbieter während des Verteilens der Routen bei der Verteilung der Routen Vorsicht walten lassen sollte.

### <a name="layer2-provider-and-direct-connection"></a>Layer2-Anbieter und direkte Verbindung

Obwohl sich die physische Konnektivität in beiden Modellen unterscheidet, wird ein Layer3 BGP direkt zwischen MSEE und dem Kundenrouter eingerichtet. Für ExpressRoute Direct stellte der Kunde direkt eine Verbindung mit MSEE her. Währen der Dienstanbieter im Falle Layer2 das VLAN vom lokalen Kundenstandort in die Cloud erweitert. Kunden führen BGP zusätzlich zum Layer2-Netzwerk aus, um Ihre DCs mit der Cloud zu verbinden.
![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l2.png)
In beiden Fällen verfügen Kunden über Point-to-Point-Verbindungen mit jeder der öffentlichen Clouds. Kunden stellen eine separate BGP-Verbindung mit jeder öffentlichen Cloud her. Von einem Cloudanbieter empfangene Routen werden anderen Cloudanbietern standardmäßig angekündigt. Jeder Cloudanbieter hat ein anderes Präfixlimit, weshalb der Kunde beim Ankündigen der Routen diese Limits berücksichtigen sollte. Kunden können beim Ankündigen von Routen aus anderen öffentlichen Clouds gängige BGP-Knöpfe verwenden.

## <a name="direct-connection-with-expressroute"></a>Direkte Verbindung mit ExpressRoute

Kunden haben die Wahlmöglichkeit, ExpressRoute direkt mit dem Direktverbindungsangebot des Cloudanbieters zu verbinden. Zwei Cloudanbieter werden Back-to-Back verbunden, und BGP wird direkt zwischen ihren Routern eingerichtet. Dieser Typ von Verbindung ist heute mit Oracle verfügbar.

## <a name="site-to-site-vpn"></a>Standort-zu-Standort-VPN-Verbindung

Kunden können das Internet nutzen, um ihre Instanzen in Azure mit anderen öffentlichen Clouds zu verbinden. Fast alle Cloudanbieter bieten Site-to-Site-VPN-Funktionen. Es könnte jedoch zu Inkompatibilitäten kommen, weil bestimmte Varianten fehlen. Beispielsweise unterstützen einige Cloudanbieter nur IKEv1, sodass ein VPN-Terminierungsendpunkt in dieser Cloud erforderlich ist. Für solche Cloudanbieter, die IKEv2 unterstützen, kann ein direkter Tunnel zwischen den VPN-Gateways bei beiden Cloudanbietern eingerichtet werden.

Site-to-Site-VPN wird nicht als Lösung mit hohem Durchsatz und geringer Latenz betrachtet. Es kann jedoch als Sicherung für eine physische Verbindung verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
Antworten auf weitere Fragen zu ExpressRoute und zur virtuellen Netzwerkkonnektivität finden Sie unter [ExpressRoute – FAQ][ER-FAQ].

Informationen zur Konnektivität zwischen Azure und Oracle finden Sie unter [Einrichten der direkten Verbindung zwischen Azure und Oracle Cloud][ER-OCI].

<!--Link References-->
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[ER-OCI]: https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-azure-oci-networking



