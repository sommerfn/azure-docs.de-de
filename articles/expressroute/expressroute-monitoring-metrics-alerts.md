---
title: Überwachung, Metriken und Warnungen – Azure ExpressRoute | Microsoft-Dokumentation
description: Diese Seite enthält Informationen zur ExpressRoute-Überwachung.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 92ec03e20fb6e681a0afd14048449ad004ebca0c
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991482"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute-Überwachung, Metriken und Warnungen

Dieser Artikel macht Sie mit der ExpressRoute-Überwachung, Metriken und Warnungen in Azure Monitor vertraut. Azure Monitor ist die Zentrale für alle Metriken, Warnungen und Diagnoseprotokolle im gesamten Azure-System.
 
>[!NOTE]
>Die Verwendung **klassischer Metriken** wird nicht empfohlen.
>

## <a name="circuit-metrics"></a>Metriken zu Leitungen

Navigieren Sie zu **Metriken**, und klicken Sie auf die Seite „ExpressRoute“ für die Leitung, die Sie überwachen möchten. Unter **Überwachung** können Sie die **Metriken** anzeigen. Wählen Sie eine der unten aufgeführten Metriken aus. Die Standardaggregation wird angewendet. Optional können Sie die Teilung anwenden, wonach die Metriken mit anderen Dimensionen angezeigt werden.

### <a name="metrics-available"></a>Verfügbare Metriken: 
* **Verfügbarkeit** 
    * ARP-Verfügbarkeit
      * Verfügbare Dimensionen:
        * Peer (Primärer/Sekundärer ExpressRoute-Router)
        * Peeringtyp (Privat/Öffentlich/Microsoft)
    * BGP-Verfügbarkeit
      * Verfügbare Dimensionen:
        * Peer (Primärer/Sekundärer ExpressRoute-Router)
        * Peeringtyp (Privat/Öffentlich/Microsoft)
* **Datenverkehr**
    * BitsInPerSecond
      * Verfügbare Dimensionen:
        * Peeringtyp (Privat/Öffentlich/Microsoft)
    * BitsOutPerSecond
      * Verfügbare Dimensionen:
        * Peeringtyp (Privat/Öffentlich/Microsoft)
    * GlobalReachBitsInPerSecond
      * Verfügbare Dimensionen:
        * Dienstschlüssel (Skey) der Peeringleitung (Skey)
    * GlobalReachBitsOutPerSecond
      * Verfügbare Dimensionen:
        * Dienstschlüssel (Skey) der Peeringleitung (Skey)

>[!NOTE]
>Die Verwendung von *GlobalGlobalReachBitsInPerSecond* und *GlobalGlobalReachBitsOutPerSecond* ist nur sichtbar, wenn mindestens eine Global Reach-Verbindung hergestellt wird.
>

## <a name="bits-in-and-out---metrics-across-all-peerings"></a>Ein- und ausgehende Bits – Metriken über alle Peerings

Sie können Metriken über alle Peerings hinweg für eine bestimmte ExpressRoute-Leitung anzeigen.

![Metriken zu Leitungen](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="bits-in-and-out---metrics-per-peering"></a>Ein- und ausgehende Bits – Metriken pro Peering

Sie können Metriken für privates, öffentliches und Microsoft-Peering in Bits pro Sekunde anzeigen.

![Peeringspezifische Metriken](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="bgp-availability---split-by-peer"></a>BGP-Verfügbarkeit – Aufgeteilt nach Peer  

Sie können die Verfügbarkeit von BGP nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die primäre BGP-Sitzung aufwärts für privates Peering und die zweite BGP-Sitzung abwärts für privates Peering an. 

![BGP-Verfügbarkeit pro Peer](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

## <a name="arp-availability---split-by-peering"></a>ARP-Verfügbarkeit – Aufgeteilt nach Peering  

Sie können die Verfügbarkeit von [ARP](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die ARP-Sitzung aufwärts für privates Peering auf beiden Peers an, aber vollständig abwärts für Microsoft-Peering über Peerings hinweg. Die Standardaggregation (Durchschnitt) wurde für beide Peers verwendet.  

![ARP-Verfügbarkeit pro Peer](./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute-Gatewayverbindungen in Bits pro Sekunde

![Gatewayverbindungen](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>Warnungen zu ExpressRoute-Gatewayverbindungen

1. Um Warnungen zu konfigurieren, navigieren Sie zu **Azure Monitor** und klicken dann auf **Warnungen**.

   ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Klicken Sie auf **+Ziel auswählen**, und wählen Sie die ExpressRoute-Gatewayverbindungsressource aus.

   ![target]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Definieren Sie die Warnungsdetails.

   ![Aktionsgruppe](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)

4. Definieren Sie die Aktionsgruppe, und fügen Sie sie hinzu.

   ![Hinzufügen einer Aktionsgruppe](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Warnungen basierend auf dem jeweiligen Peering

 ![was](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Konfigurieren von Warnungen für Aktivitätsprotokolle zu Leitungen

In **Warnungskriterien** können Sie **Aktivitätsprotokoll** als Signaltyp und dann das Signal auswählen.

  ![andere](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)
  
## <a name="next-steps"></a>Nächste Schritte

Konfigurieren Sie Ihre ExpressRoute-Verbindung.
  
  * [Erstellen und Ändern einer Verbindung](expressroute-howto-circuit-arm.md)
  * [Erstellen und Ändern einer Peeringkonfiguration](expressroute-howto-routing-arm.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
