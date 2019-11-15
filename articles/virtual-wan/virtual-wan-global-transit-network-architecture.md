---
title: Architektur mit einem globalen Transitnetzwerk für Azure Virtual WAN | Microsoft-Dokumentation
description: Erfahren Sie mehr zur Architektur mit einem globalen Transitnetzwerk für Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 9b736e897278c4656df83c30388efc812ac6193d
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607366"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Architektur mit einem globalen Transitnetzwerk und Azure Virtual WAN

Moderne Unternehmen erfordern überall Verbindungen zwischen umfassend verteilten Anwendungen, Daten und Benutzern in der Cloud und lokal. Die Architektur mit einem globalen Transitnetzwerk wird von Unternehmen eingesetzt, um den cloudzentrischen IT-Fußabdruck moderner, globaler Unternehmen zu konsolidieren, zu vernetzen und zu steuern.

Die Architektur mit einem globalen Transitnetzwerk basiert auf einem klassischen Hub-and-Spoke-Konnektivitätsmodell, bei dem der in der Cloud gehostete Netzwerkhub Übertragungsverbindungen zwischen Endpunkten ermöglicht, die sich in unterschiedlichen Spokes befinden.

In diesem Modell kann eine Spoke Folgendes sein:
* ein virtuelles Netzwerk (VNET)
* eine physische Zweigstelle
* ein Remotebenutzer
* Internet

![Hub-and-Spoke](./media/virtual-wan-global-transit-network-architecture/figure1.png)

**Abbildung 1: Globales Hub-and-Spoke-Transitnetzwerk**

In Abbildung 1 sehen Sie den logischen Aufbau des globalen Transitnetzwerks, bei dem geografisch verteilte Benutzer, physische Standorte und virtuelle Netzwerke über einen in der Cloud gehosteten Netzwerkhub miteinander verbunden sind. Durch diese Architektur sind logische Transitverbindungen mit einem Hop zwischen den Netzwerkendpunkten möglich.

## <a name="globalnetworktransit"></a>Globales Transitnetzwerk mit Virtual WAN

Azure Virtual WAN ist ein von Microsoft verwalteter Cloudnetzwerkdienst. Alle Netzwerkkomponenten, aus denen dieser Dienst besteht, werden von Microsoft gehostet und verwaltet. Weitere Informationen zu Virtual WAN finden Sie im Artikel mit der [Übersicht über Virtual WAN](virtual-wan-about.md).

Durch Azure Virtual WAN wird eine Architektur mit einem globalen Transitnetzwerk ermöglicht, da überall N:M-Verbindungen zwischen global verteilten Sätzen von Cloudworkloads in VNets, Zweigstellen, SaaS- und PaaS-Anwendungen und Benutzern möglich sind.

![Azure Virtual WAN](./media/virtual-wan-global-transit-network-architecture/figure2.png)

**Abbildung 2: Globales Transitnetzwerk und Virtual WAN**

In der Azure Virtual WAN-Architektur werden virtuelle WAN-Hubs in Azure-Regionen bereitgestellt, an die Sie Zweigstellen, VNets und Remotebenutzer anbinden können. Die physischen Zweigstellen werden über Premium ExpressRoute oder Site-to-Site-VPNs mit dem Hub verbunden, VNets werden über VNet-Verbindungen mit dem Hub verbunden und Remotebenutzer können über ein Benutzer-VPN (Point-to-Site-VPN) direkt eine Verbindung mit dem Hub herstellen. Virtual WAN unterstützt auch regionsübergreifende VNet-Verbindungen, wobei ein VNet in einer Region mit einem Virtual WAN-Hub in einer anderen Region verbunden werden kann.

Sie können ein virtuelles WAN erstellen, indem Sie einen einzelnen Virtual WAN-Hub in der Region erstellen, in der sich die meisten Spokes (Zweigstellen, VNets, Benutzer) befinden. Stellen Sie anschließend eine Verbindung der Spokes, die sich in einer anderen Region befinden, zum Hub her. Dies ist eine gute Möglichkeit, wenn der Fußabdruck eines Unternehmens größtenteils in einer Region mit wenigen Remote-Spokes liegt.  
  
## <a name="hubtohub"></a>Hub-zu-Hub-Verbindungen

Der Cloudfußabdruck eines Unternehmens kann sich über mehrere Cloudregionen erstrecken. Diese Architektur ist optimal (in Bezug auf die Latenz) für den Cloudzugriff in einer Region geeignet, die möglichst nah am physischen Standort des Unternehmens und an den Benutzern liegt. Einer der wichtigsten Faktoren für eine Architektur mit einem globalen Transitnetzwerk ist die Möglichkeit regionsübergreifender Verbindungen zwischen allen Cloud- und lokalen Netzwerkendpunkten. Das bedeutet, dass der Datenverkehr einer Zweigstelle, die in einer bestimmten Region mit der Cloud verbunden ist, eine Zweigstelle oder ein VNet in einer anderen Region über eine Hub-zu-Hub-Konnektivität, die durch das [Azure Global Network](https://azure.microsoft.com/global-infrastructure/global-network/) ermöglicht wird, erreichen kann.

![regionsübergreifend](./media/virtual-wan-global-transit-network-architecture/figure3.png)

**Abbildung 3: Regionsübergreifende Virtual WAN-Verbindungen**

Wenn mehrere Hubs in einem einzelnen virtuellen WAN aktiviert sind, werden die Hubs automatisch über Hub-zu-Hub-Verbindungen miteinander verbunden, wodurch eine globale Konnektivität zwischen Zweigstellen und VNets ermöglicht wird, die über mehrere Regionen verteilt sind. 

Die Hubs sind darüber hinaus zwar alle Teil desselben virtuellen WAN, können aber unterschiedlichen regionalen Zugriffs- und Sicherheitsrichtlinien unterliegen. Weitere Informationen finden Sie weiter unten in diesem Artikel unter [Sicherheits- und Richtliniensteuerung](#security).

## <a name="anytoany"></a>N:n-Verbindungen

Durch die Architektur mit einem globalen Transitnetzwerk sind N:M-Verbindungen über Virtual WAN-Hubs möglich. Diese Architektur macht vollständig vermaschte oder teilweise vermaschte Verbindungen zwischen Spokes, deren Aufbau und Wartung ist komplexer ist, überflüssig bzw. minimiert deren Notwendigkeit. Zusätzlich kann die Routingsteuerung in Hub-and-Spoke-Modellen im Vergleich zu vermaschten Netzwerken einfacher konfiguriert und gewartet werden.

Durch N:M-Verbindungen (im Kontext einer globalen Architektur) können Unternehmen mit global verteilten Benutzern, Zweigstellen, Rechenzentren, VNets und Anwendungen über einen Transithub Verbindungen miteinander herstellen. Azure Virtual WAN fungiert als globales Transitsystem.

![N:M](./media/virtual-wan-global-transit-network-architecture/figure4.png)

**Abbildung 4: Datenverkehrspfade in Azure Virtual WAN**

Azure Virtual WAN unterstützt die folgenden globalen Transitverbindungsarten. Die Buchstaben in Klammern entsprechen den Buchstaben in Abbildung 4.

* Zweigstelle-zu-VNET (a)
* Zweigstelle-zu-Zweigstelle (b)
  * ExpressRoute Global Reach und Virtual WAN
* Remotebenutzer-zu-VNET (c)
* Remotebenutzer-zu-Zweigstelle (d)
* VNet-zu-VNet (e)
* Zweigstelle-zu-Hub-Hub-zu-Zweigstelle (f)
* Zweigstelle-zu-Hub-Hub-zu-VNet (g)
* VNet-zu-Hub-Hub-zu-VNet (h)

### <a name="branch-to-vnet-a-and-branch-to-vnet-cross-region-g"></a>Zweigstelle-zu-VNet (a) und Zweigstelle-zu-VNet, regionsübergreifend (g)

Pfade zwischen Zweigstellen und VNETs werden primär von Azure Virtual WAN unterstützt. Mit diesem Pfad können Sie Zweigstellen mit Azure-IaaS-Unternehmensworkloads vernetzen, die in Azure-VNETs bereitgestellt wurden. Zweigstellen können über ExpressRoute oder ein Site-to-Site-VPN mit einem virtuellen WAN vernetzt werden. Der Datenverkehr wird an VNets übertragen, die mit den Virtual WAN-Hubs über VNet-Verbindungen verbunden sind. Ein expliziter [Gatewaytransit](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) ist für Virtual WAN nicht erforderlich, da Virtual WAN den Gatewaytransit an Zweigstellen automatisch ermöglicht. Informationen zum Herstellen einer Verbindung zwischen einem SD-WAN-CPE und einem Virtual WAN finden Sie im Artikel [Virtual WAN-Partner](virtual-wan-configure-automation-providers.md).

### <a name="expressroute-global-reach-and-virtual-wan"></a>ExpressRoute Global Reach und Virtual WAN

ExpressRoute ist eine private und zuverlässige Möglichkeit, Ihre lokalen Netzwerke mit der Microsoft Cloud zu verbinden. Virtual WAN unterstützt ExpressRoute-Leitungsverbindungen. Zum Herstellen einer Verbindung zwischen einer Zweigstelle und Virtual WAN mit ExpressRoute ist 1. eine Premium-Leitung erforderlich und 2. muss sich die Leitung in einem Global Reach-fähigen Standort befinden.

ExpressRoute Global Reach ist ein Add-On-Feature für ExpressRoute. Mithilfe von Global Reach können Sie ExpressRoute-Verbindungen miteinander verbinden, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen. Zweigstellen, die mit Azure Virtual WAN über ExpressRoute verbunden sind, erfordern ExpressRoute Global Reach, um miteinander kommunizieren zu können.

In diesem Modell kann jede Zweigstelle, die mit ExpressRoute mit dem Virtual WAN-Hub verbunden ist, über den Pfad zwischen Zweigstelle und VNet eine Verbindung zu VNets herstellen. Datenverkehr zwischen Zweigstellen wird nicht an den Hub übertragen, weil ExpressRoute Global Reach den optimaleren Pfad über Azure Virtual WAN ermöglicht.

### <a name="branch-to-branch-b-and-branch-to-branch-cross-region-f"></a>Zweigstelle-zu-Zweigstelle (b) und Zweigstelle-zu-Zweigstelle, regionsübergreifend (f)

Zweigstellen können mit einem Azure Virtual WAN-Hub über ExpressRoute-Leitungen und/oder Site-to-Site-VPN-Verbindungen verbunden sein. Sie können die Zweigstellen mit einem Virtual WAN-Hub verbinden, der sich in einer Region befindet, die der Zweigstelle am nächsten ist.

Durch diese Option können Unternehmen den Azure-Backbone zum Verbinden von Zweigstellen verwenden. Diese Funktion ist zwar verfügbar, Sie sollten aber dennoch die Vorteile der Verbindung von Zweigstellen über Azure Virtual WAN mit den Vorteilen eines privaten WAN vergleichen.  

### <a name="remote-user-to-vnet-c"></a>Remotebenutzer-zu-VNET (c)

Sie können direkten, sicheren Remotezugriff auf Azure über eine Point-to-Site-Verbindung zwischen einem Remotebenutzerclient und einem virtuellen WAN ermöglichen. Remotebenutzer eines Unternehmens müssen so nicht mehr mühsam eine Verbindung über ein Unternehmens-VPN herstellen.

### <a name="remote-user-to-branch-d"></a>Remotebenutzer-zu-Zweigstelle (d)

Mit dem Remotebenutzer-zu-Zweigstelle-Pfad können Remotebenutzer, die über eine Point-to-Site-Verbindung mit Azure verbunden sind, auf lokale Workloads und Anwendungen über die Cloud zugreifen. Durch diesen Pfad können Remotebenutzer flexibel auf Workloads zugreifen, die in Azure oder lokal bereitgestellt wurden. Unternehmen können in Azure Virtual WAN einen zentralen, cloudbasierten, sicheren Remotezugriffsdienst verfügbar machen.

### <a name="vnet-to-vnet-transit-e-and-vnet-to-vnet-cross-region-h"></a>VNet-zu-VNet-Transit (e) und VNet-zu-VNet, regionsübergreifend (h)

Der VNet-zu-VNet-Transit ermöglicht Verbindungen zwischen VNets, um mehrschichtige Anwendungen zu verbinden, die in mehreren VNets implementiert wurden. Optional können Sie VNets über VNet Peering miteinander verbinden. Dies ist u. U. für einige Szenarien geeignet, in denen kein Transit über den VWAN-Hub erforderlich ist.

## <a name="security"></a>Sicherheits- und Richtliniensteuerung

Die Azure Virtual WAN-Hubs verbinden alle Netzwerkendpunkte über das hybride Netzwerk und können u. U. den gesamten Transitnetzwerkdatenverkehr sehen. Virtual WAN-Hubs können durch Bereitstellen der Azure Firewall in VWAN-Hubs in geschützte virtuelle Hubs umgewandelt werden, um eine cloudbasierte Sicherheits-, Zugriffs- und Richtliniensteuerung zu ermöglichen. Die Orchestrierung von Azure Firewalls in virtuellen WAN-Hubs kann mit Azure Firewall Manager durchgeführt werden.

[Azure Firewall Manager](https://go.microsoft.com/fwlink/?linkid=2107683) stellt Funktionen zum Verwalten und Skalieren der Sicherheit für globale Transitnetzwerke zur Verfügung. Azure Firewall Manager bietet die Möglichkeit, Routing, globale Richtlinienverwaltung und erweiterte Internetsicherheitsdienste über Drittanbieter zusammen mit der Azure Firewall zentral zu verwalten.

![geschützter virtueller Hub mit Azure Firewall](./media/virtual-wan-global-transit-network-architecture/figure5.png)

**Abbildung 5: Geschützter virtueller Hub mit Azure Firewall**

Die Azure Firewall zum Virtual WAN unterstützt die folgenden globalen geschützten Transitverbindungsarten. Die Buchstaben in Klammern entsprechen den Buchstaben in Abbildung 5.

* Geschützter VNet-zu-VNet-Transit (e)
* VNet-zu-Internet oder Sicherheitsdienst von Drittanbietern (i)
* Zweigstelle-zu-Internet oder Sicherheitsdienst von Drittanbietern (j)

### <a name="vnet-to-vnet-secured-transit-e"></a>Geschützter VNet-zu-VNet-Transit (e)

Der geschützte VNet-zu-VNet-Transit ermöglicht Verbindungen zwischen den VNets über die Azure Firewall im Virtual WAN-Hub.

### <a name="vnet-to-internet-or-third-party-security-service-i"></a>VNet-zu-Internet oder Sicherheitsdienst von Drittanbietern (i)

Der geschützte Transit vom VNet zum Internet oder einem Drittanbieter ermöglicht Verbindungen vom VNet zum Internet oder zu unterstützten Sicherheitsdiensten von Drittanbietern über die Azure Firewall im Virtual WAN-Hub.

### <a name="branch-to-internet-or-third-party-security-service-j"></a>Zweigstelle-zu-Internet oder Sicherheitsdienst von Drittanbietern (j)
Der geschützte Transit von Zweigstellen zum Internet oder einem Drittanbieter ermöglicht Verbindungen von der Zweigstelle zum Internet oder zu unterstützten Sicherheitsdiensten von Drittanbietern über die Azure Firewall im Virtual WAN-Hub.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine Verbindung mit Virtual WAN und stellen Sie die Azure Firewall in VWAN-Hubs bereit.

* [Erstellen einer Site-to-Site-Verbindung per Azure Virtual WAN](virtual-wan-site-to-site-portal.md)
* [Erstellen einer ExpressRoute-Zuordnung per Azure Virtual WAN](virtual-wan-expressroute-portal.md)
* [Azure Firewall Manager zum Bereitstellen von Azure Firewall im VWAN](https://go.microsoft.com/fwlink/?linkid=2107683)
