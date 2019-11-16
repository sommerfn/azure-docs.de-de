---
title: 'Azure ExpressRoute: Voraussetzungen'
description: Diese Seite enthält eine Liste der Anforderungen, die erfüllt sein müssen, bevor Sie eine Azure ExpressRoute-Verbindung anfordern können. Sie umfasst eine Checkliste.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: cherylmc
ms.openlocfilehash: a72eba9bde0745e66bdf8e7efd8eaec7d6a0b186
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74083353"
---
# <a name="expressroute-prerequisites--checklist"></a>Voraussetzungen und Checkliste für ExpressRoute
Zum Herstellen einer Verbindung mit den Microsoft-Clouddiensten über ExpressRoute müssen Sie sicherstellen, dass die unten in den Abschnitten genannten Voraussetzungen erfüllt sind.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-Konto
* Ein gültiges und aktives Microsoft Azure-Konto. Dieses Konto ist zum Einrichten der ExpressRoute-Verbindung erforderlich. Bei ExpressRoute-Verbindungen handelt es sich um Ressourcen in Azure-Abonnements. Ein Azure-Abonnement ist Voraussetzung, auch wenn die Konnektivität auf andere Microsoft-Clouddienste (nicht Azure) wie Office 365 beschränkt ist.
* Ein aktives Office 365-Abonnement (bei Nutzung von Office 365-Diensten). Weitere Informationen finden Sie im Abschnitt „Besondere Vorgaben für Office 365“ dieses Artikels.

## <a name="connectivity-provider"></a>Konnektivitätsanbieter

* Sie können mit einem [ExpressRoute-Konnektivitätspartner](expressroute-locations.md#partners) zusammenarbeiten, um eine Verbindung mit der Microsoft Cloud herzustellen. Zum Einrichten einer Verbindung zwischen Ihrem lokalen Netzwerk und Microsoft stehen Ihnen [drei Möglichkeiten](expressroute-introduction.md)zur Verfügung.
* Wenn Ihr Anbieter kein ExpressRoute-Konnektivitätspartner ist, können Sie die Verbindung zur Microsoft Cloud über einen [Cloud-Exchange-Anbieter](expressroute-locations.md#connectivity-through-exchange-providers)herstellen.

## <a name="network-requirements"></a>Netzwerkanforderungen
* **Redundanz an jedem Peeringstandort**: Für Microsoft ist es erforderlich, dass zwischen den Routern von Microsoft und den Peeringroutern redundante BGP-Sitzungen für jede ExpressRoute-Verbindung eingerichtet werden, (auch wenn [zum Cloud-Exchange nur eine physische Verbindung](expressroute-faqs.md#onep2plink) besteht).
* **Redundanz für die Notfallwiederherstellung**: Microsoft empfiehlt dringend, mindestens zwei ExpressRoute-Verbindungen an verschiedenen Peeringstandorten einzurichten, um einen Single Point of Failure zu vermeiden.
* **Routing**: Je nachdem, wie Sie die Verbindung mit Microsoft Cloud herstellen, müssen Sie oder Ihr Anbieter die BGP-Sitzungen für [Routingdomänen](expressroute-circuit-peerings.md)einrichten und verwalten. Einige Ethernet-Konnektivitätsanbieter oder Cloud-Exchange-Anbieter bieten BGP-Verwaltung als Dienst an, der einen Mehrwert schafft.
* **NAT**: Microsoft akzeptiert nur öffentliche IP-Adressen über Microsoft-Peering. Wenn Sie private IP-Adressen in Ihrem lokalen Netzwerk verwenden, müssen Sie oder Ihr Anbieter die privaten IP-Adressen [mithilfe der NAT](expressroute-nat.md)in öffentliche IP-Adressen übersetzen.
* **QoS**: Skype for Business umfasst verschiedene Dienste (z.B. Sprachanrufe, Videoanrufe, SMS) mit jeweils unterschiedlichen QoS-Anforderungen. Sie und Ihr Anbieter müssen daher die [QoS-Anforderungen](expressroute-qos.md)berücksichtigen.
* **Netzwerksicherheit**: Berücksichtigen Sie die [Netzwerksicherheit](../best-practices-network-security.md) beim Herstellen der Verbindung mit der Microsoft Cloud über ExpressRoute.

## <a name="office-365"></a>Office 365
Wenn Sie Office 365 auf ExpressRoute aktivieren möchten, helfen Ihnen die Informationen in den folgenden Dokumenten zu den Anforderungen für Office 365 weiter.

* [Overview of ExpressRoute for Office 365 (Überblick über ExpressRoute für Office 365)](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Verwalten von Office 365-Netzwerkdatenverkehr](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [Hochverfügbarkeit und Failover mit ExpressRoute](https://aka.ms/erhighavailability)
* [URLs und IP-Adressbereiche von Office 365](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Netzwerkplanung und Leistungsoptimierung für Office 365](https://support.office.com/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Network bandwidth calculators and tools](https://support.office.com/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132) (Netzwerk- und Migrationsplanung für Office 365)
* [Office 365-Integration in lokale Umgebungen](https://support.office.com/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [ExpressRoute unter Office 365 – Schulungsvideos für Fortgeschrittene](https://channel9.msdn.com/series/aer/)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen über ExpressRoute finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).
* Suchen Sie einen ExpressRoute-Konnektivitätsanbieter. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md), [NAT](expressroute-nat.md) und [QoS](expressroute-qos.md) an.
* Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  * [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-arm.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
