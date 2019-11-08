---
title: Azure Virtual WAN – Standorte von Partnern | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Liste der Standorte von Azure Virtual WAN-Partnern und -Hubs.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 985823e5aa806244596277c982cc129554d890f2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489035"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Standorte von Virtual WAN-Partnern und -Hubs

Dieser Artikel enthält Informationen über die von Virtual WAN unterstützten Regionen und Partner für Konnektivität mit Virtual Hub.

Der Netzwerkdienst Azure Virtual WAN bietet optimierte und automatisierte Konnektivität zwischen Branches über Azure. Per Virtual WAN können Sie Branchgeräte für die Kommunikation mit Azure verbinden und konfigurieren. Dies ist entweder manuell oder durch die Nutzung von Geräten eines Anbieters über einen Virtual WAN-Partner möglich. Die Nutzung von Geräten eines Partners ermöglicht eine einfache Verwendung, die Vereinfachung der Verbindungsherstellung und die Konfigurationsverwaltung.

Konnektivität auf dem lokalen Gerät wird auf automatisierte Weise mit dem virtuellen Hub hergestellt. Ein virtueller Hub ist ein von Microsoft verwaltetes virtuelles Netzwerk. Der Hub enthält verschiedene Dienstendpunkte zum Aktivieren der Konnektivität über Ihr lokales Netzwerk (vpnsite). Pro Region ist nur ein Hub möglich.

## <a name="automation"></a>Automatisierung von Konnektivitätspartnern

Geräte, die sich mit Azure Virtual WAN verbinden, verfügen für die Verbindungsherstellung über eine integrierte Automatisierung. Diese wird typischerweise auf der Geräteverwaltungsoberfläche (o.ä.) eingerichtet. Dadurch wird die Verbindungs- und Konfigurationsverwaltung zwischen dem VPN-Zweigstellengerät und einem Azure Virtual Hub VPN-Endpunkt (VPN Gateway) eingerichtet.

Die folgende allgemeine Automatisierung wird in der Gerätekonsole bzw. im Verwaltungscenter eingerichtet:

* Entsprechende Berechtigungen für das Gerät, um auf die Azure Virtual WAN-Ressourcengruppe zugreifen zu können
* Hochladen des Zweigstellengeräts in Azure Virtual WAN
* Automatischer Download der Azure-Konnektivitätsinformationen
* Konfiguration des lokalen Zweigstellengeräts 

Einige Konnektivitätspartner können die Automatisierung erweitern, indem sie das Azure Virtual Hub-VNET und das VPN-Gateway erstellen. Wenn Sie mehr über Automatisierung erfahren möchten, siehe [Konfigurieren der Automatisierung – WAN-Partner](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>Konnektivität über Partner

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

Die folgenden Partner sind auf unsere Roadmap in naher Zukunft vorgesehen: Arista, F5 Networks, Velocloud.

## <a name="locations"></a>Standorte

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Virtual WAN finden Sie unter [Virtual WAN – Häufig gestellte Fragen](virtual-wan-faq.md).

* Weitere Informationen zur Automatisierung der Konnektivität mit Azure Virtual WAN finden Sie unter [Virtual WAN-Automatisierung durch Partner](virtual-wan-configure-automation-providers.md).
