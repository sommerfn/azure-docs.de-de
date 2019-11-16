---
title: 'Azure ExpressRoute: QoS-Anforderungen'
description: Diese Seite enthält detaillierte Informationen zu den Anforderungen für das Konfigurieren und Verwalten von QoS. Skype for Business/Sprachdienste werden erläutert.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: cherylmc
ms.openlocfilehash: debc5d91478d0a5c3cc16c7b09f5713ba09b467e
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74080101"
---
# <a name="expressroute-qos-requirements"></a>QoS-Anforderungen für ExpressRoute
Skype for Business umfasst verschiedene Workloads mit jeweils unterschiedlichen QoS-Anforderungen. Wenn Sie Sprachdienste über ExpressRoute bereitstellen möchten, sollten die im Folgenden beschriebenen Anforderungen erfüllt sein.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Die QoS-Anforderungen gelten nur für das Microsoft-Peering. Die DSCP-Werte in Ihrem Netzwerkdatenverkehr, der über öffentliches und privates Azure-Peering empfangen wird, werden auf 0 zurückgesetzt. 
> 
> 

Die folgende Tabelle enthält eine Liste mit DSCP-Markierungen, die von Microsoft-Teams und Skype for Business verwendet werden. Weitere Informationen finden Sie unter [Verwalten der Dienstqualität für Skype for Business](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/managing-quality-of-service-QoS) .

| **Datenverkehrsklasse** | **Behandlung (DSCP-Markierung)** | **Microsoft Teams- und Skype for Business-Workloads** |
| --- | --- | --- |
| **Voice** |EF (46) |Skype/Microsoft Teams/Lync-Stimme |
| **Interactive** |AF41 (34) |Video, VBSS |
| |AF21 (18) |App-Freigabe | 
| **Standard** |AF11 (10) |Dateiübertragung |
| |CS0 (0) |Alles andere |

* Sie sollten die Workloads klassifizieren und die richtigen DSCP-Markierungen wählen. Folgen Sie [dieser Anleitung](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/configuring-port-ranges-for-your-skype-clients#configure-quality-of-service-policies-for-clients-running-on-windows-10) zum Festlegen von DSCP-Markierungen in Ihrem Netzwerk.
* Sie sollten mehrere QoS-Warteschlangen in Ihrem Netzwerk konfigurieren und bereitstellen. „Voice“ muss eine eigenständige Klasse sein und die in [RFC 3246](https://www.ietf.org/rfc/rfc3246.txt) definierte EF-Behandlung erhalten. 
* Sie können den gewünschten Warteschlangenmechanismus, die Richtlinie für die Auslastungserkennung und die Bandbreitenzuordnung pro Datenverkehrsklasse festlegen. Die DSCP-Markierung der Skype for Business-Workloads muss jedoch in jedem Fall erhalten bleiben. Wenn Sie andere als die oben aufgeführten DSCP-Markierungen verwenden, z. B. AF31 (26), muss dieser DSCP-Wert im Paket vor dem Senden an Microsoft auf 0 zurückgesetzt werden. Microsoft sendet nur Pakete mit dem in der obigen Tabelle aufgeführten DSCP-Wert. 

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md) und [NAT](expressroute-nat.md) an.
* Informationen zum Konfigurieren der ExpressRoute-Verbindung finden Sie über die folgenden Links.
  
  * [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)

