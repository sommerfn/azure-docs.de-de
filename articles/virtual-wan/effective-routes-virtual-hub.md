---
title: 'Anzeigen der effektiven Routen für einen virtuellen Hub: Azure Virtual WAN | Microsoft-Dokumentation'
description: Anzeigen der effektiven Routen für einen virtuellen Hub in Azure Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 1173da81736661048d1e4e12d9919bc2aadf73ee
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511211"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>Anzeigen der effektiven Routen für einen virtuellen Hub

Sie können alle Routen Ihres Virtual WAN-Hubs im Azure-Portal anzeigen. Navigieren Sie zum Anzeigen der Routen zum virtuellen Hub und wählen Sie dann **Routing > Effektive Routen anzeigen** aus.

## <a name="understand"></a>Grundlegendes zu Routen

Anhand des folgenden Beispiels können Sie besser verstehen, wie das Virtual WAN-Routing erfolgt.

In diesem Beispiel gibt es ein virtuelles WAN mit drei Hubs. Der erste Hub befindet sich in der Region „USA, Osten“, der zweite Hub befindet sich in der Region „Europa, Westen“ und der dritte Hub in der Region „USA, Westen“. In einem Virtual WAN sind alle Hubs miteinander verbunden. In diesem Beispiel wird davon ausgegangen, dass die Hubs „USA, Osten“ und „Europa, Westen“ über Verbindungen zu lokalen Zweigstellen (Spokes) und virtuellen Azure-Netzwerken (Spokes) verfügen.

Ein Azure VNet-Spoke (10.4.0.0/16) mit einer Network Virtual Appliance (10.4.0.6) ist weiterhin mit einem VNet (10.5.0.0/16) gekoppelt. Weitere Informationen zur Hub-Routingtabelle finden Sie in den [zusätzlichen Informationen](#abouthubroute) weiter unten in diesem Artikel.

In diesem Beispiel wird auch davon ausgegangen, dass die Zweigstelle 1 in Westeuropa mit dem Hub „USA, Osten“ und dem Hub „Europa, Westen“ verbunden ist. Eine ExpressRoute-Verbindung in „USA, Osten“ verbindet Zweigstelle 2 mit dem Hub „USA, Osten“.

![Diagramm](./media/effective-routes-virtual-hub/diagram.png)

## <a name="view"></a>Anzeigen effektiver Routen

Wenn Sie im Portal die Option „Effektive Routen anzeigen“ auswählen, wird die in der [Hub-Routingtabelle](#routetable) angezeigte Ausgabe für den Hub „USA, Osten“ erstellt.

Genauer gesagt: die erste Zeile bedeutet, dass der Hub „USA, Osten“ die Route „10.20.1.0/24“ (Zweigstelle 1) durch die *Nächster Hop*-VPN-Verbindung ("Nächster Hop" VPN Gateway Instance0 IP 10.1.0.6, Instance1 IP 10.1.0.7) erlernt hat. *Routenursprung* verweist auf die Ressourcen-ID. *AS-Pfad* gibt den AS-Pfad für Zweigstelle 1 an.

### <a name="routetable"></a>Hub-Routingtabelle

Verwenden Sie die Scrollleiste am unteren Rand der Tabelle, um den „AS-Pfad“ anzuzeigen.

| **Präfix** |  **Typ des nächsten Hops** | **Nächster Hop** |  **Routenursprung** |**AS-Pfad** |
| ---        | ---                | ---          | ---               | ---         |
| 10.20.1.0/24|VPN |10.1.0.6, 10.1.0.7| /subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10, 10.1.0.11|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-eastus-gw|21000|
|10.23.1.0/24| VPN |10.1.0.6, 10.1.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw|23000|
|10.4.0.0/16|VNet-Verbindung| On-link |  |  |
|10.5.0.0/16| IP-Adresse| 10.4.0.6|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| IP-Adresse| `<Azure Firewall IP>` |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| Remote Hub|10.8.0.6, 10.8.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_| 4848-22000 |
|10.9.0.0/16| Remote Hub|  On-link |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_1| |

>[!NOTE]
> Wenn die Hubs „USA, Osten“ und „Europa, Westen“ in der Beispieltopologie nicht miteinander kommunizieren, gibt es die gelernte Route (10.9.0.0/16) nicht. Hubs kündigen nur Netzwerke an, die direkt mit Ihnen verbunden sind.
>

## <a name="additional"></a>Weitere Informationen

### <a name="abouthubroute"></a>Informationen zur Hub-Routingtabelle

Sie können eine virtuelle Hubroute erstellen und die Route der Routingtabelle des virtuellen Hubs zuweisen. Sie können der Routingtabelle des virtuellen Hubs mehrere Routen zuweisen. Auf diese Weise können Sie eine Route für das Ziel-VNet über eine IP-Adresse festlegen (normalerweise die Network Virtual Appliance (NVA) in einem Spoke-VNet). Weitere Informationen zu NVAs finden Sie unter [Weiterleiten von Datenverkehr von einem virtuellen Hub an eine NVA](virtual-wan-route-table-portal.md).

### <a name="aboutdefaultroute"></a>Informationen zur Standardroute (0.0.0.0/0)

Ein virtueller Hub kann eine erlernte Standardroute an eine VNet-, Site-to-Site-VPN- und ExpressRoute-Verbindung weitergeben, wenn das Flag für die Verbindung auf „Aktiviert“ festgelegt ist. Dieses Flag ist sichtbar, wenn Sie eine VNet-Verbindung, eine VPN-Verbindung oder eine ExpressRoute-Verbindung bearbeitet. „EnableInternetSecurity“ ist für VNet-, ExpressRoute- und VPN-Hubverbindungen standardmäßig immer „false“.

Der Ursprung der Standardroute liegt nicht im Virtual WAN-Hub. Die Standardroute wird weitergegeben, wenn sie dem Virtual WAN-Hub bereits bekannt ist, weil darin eine Firewall bereitgestellt wurde, oder wenn für eine andere verbundene Site die Tunnelerzwingung aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).