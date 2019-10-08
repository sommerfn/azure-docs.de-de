---
title: Überwachung, Metriken und Warnungen – Azure ExpressRoute | Microsoft-Dokumentation
description: Diese Seite enthält Informationen zur ExpressRoute-Überwachung.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 40e5561c9a55595340568ec660cbc6dd6e1eab51
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672133"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute-Überwachung, Metriken und Warnungen

Dieser Artikel macht Sie mit der ExpressRoute-Überwachung, Metriken und Warnungen in Azure Monitor vertraut. Azure Monitor ist die Zentrale für alle Metriken, Warnungen und Diagnoseprotokolle im gesamten Azure-System.
 
>[!NOTE]
>Die Verwendung **klassischer Metriken** wird nicht empfohlen.
>

## <a name="expressroute-metrics"></a>ExpressRoute-Metriken

Um **Metriken** anzuzeigen, navigieren Sie zur Seite *Azure Monitor*, und klicken Sie auf *Metriken*. Wenn Sie **ExpressRoute**-Metriken anzeigen möchten, filtern Sie nach dem Ressourcentyp *ExpressRoute-Leitungen*. Um **Global Reach**-Metriken anzuzeigen, filtern Sie nach dem Ressourcentyp *ExpressRoute-Leitungen*, und wählen Sie eine ExpressRoute-Leitungsressource aus, bei der Global Reach aktiviert ist. Um **ExpressRoute Direct**-Metriken anzuzeigen, filtern Sie nach dem Ressourcentyp *ExpressRoute-Ports*. 

Nach dem Auswählen einer Metrik wird die Standardaggregation angewendet. Optional können Sie die Teilung anwenden, wonach die Metrik mit anderen Dimensionen angezeigt wird.

### <a name="available-metrics"></a>Verfügbare Metriken
|**Metrik**|**Kategorie**|**Dimension(en)**|**Feature(s)**|
| --- | --- | --- | --- |
|ARP-Verfügbarkeit|Verfügbarkeit|<ui><li>Peer (Primärer/Sekundärer ExpressRoute-Router)</ui></li><ui><li> Peeringtyp (Privat/Öffentlich/Microsoft)</ui></li>|ExpressRoute|
|BGP-Verfügbarkeit|Verfügbarkeit|<ui><li> Peer (Primärer/Sekundärer ExpressRoute-Router)</ui></li><ui><li> Peeringtyp</ui></li>|ExpressRoute|
|BitsInPerSecond|Verkehr|<ui><li> Peeringtyp (ExpressRoute)</ui></li><ui><li>Link (ExpressRoute Direct)</ui></li>| <li> ExpressRoute</li><li>ExpressRoute Direct|
|BitsOutPerSecond|Verkehr| <ui><li>Peeringtyp (ExpressRoute)</ui></li><ui><li> Link (ExpressRoute Direct) | <ui><li>ExpressRoute<ui><li>ExpressRoute Direct</ui></li> |
|GlobalReachBitsInPerSecond|Verkehr|<ui><li>Dienstschlüssel der Peeringleitung</ui></li>|Global Reach|
|GlobalReachBitsOutPerSecond|Verkehr|<ui><li>Dienstschlüssel der Peeringleitung</ui></li>|Global Reach|
|AdminState|Physische Konnektivität|Link|ExpressRoute Direct|
|LineProtocol|Physische Konnektivität|Link|ExpressRoute Direct|
|RxLightLevel|Physische Konnektivität|<ui><li>Link</ui></li><ui><li>Bereich</ui></li>|ExpressRoute Direct|
|TxLightLevel|Physische Konnektivität|<ui><li>Link</ui></li><ui><li>Bereich</ui></li>|ExpressRoute Direct|
>[!NOTE]
>Die Verwendung von *GlobalGlobalReachBitsInPerSecond* und *GlobalGlobalReachBitsOutPerSecond* ist nur sichtbar, wenn mindestens eine Global Reach-Verbindung hergestellt wird.
>

## <a name="circuits-metrics"></a>Leitungsmetriken

### <a name="bits-in-and-out---metrics-across-all-peerings"></a>Ein- und ausgehende Bits – Metriken über alle Peerings

Sie können Metriken über alle Peerings hinweg für eine bestimmte ExpressRoute-Leitung anzeigen.

![Metriken zu Leitungen](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

### <a name="bits-in-and-out---metrics-per-peering"></a>Ein- und ausgehende Bits – Metriken pro Peering

Sie können Metriken für privates, öffentliches und Microsoft-Peering in Bits pro Sekunde anzeigen.

![Peeringspezifische Metriken](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

### <a name="bgp-availability---split-by-peer"></a>BGP-Verfügbarkeit – Aufgeteilt nach Peer  

Sie können die Verfügbarkeit von BGP nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die primäre BGP-Sitzung aufwärts für privates Peering und die zweite BGP-Sitzung abwärts für privates Peering an. 

![BGP-Verfügbarkeit pro Peer](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

### <a name="arp-availability---split-by-peering"></a>ARP-Verfügbarkeit – Aufgeteilt nach Peering  

Sie können die Verfügbarkeit von [ARP](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) nahezu in Echtzeit über Peerings und Peers (primäre und sekundäre ExpressRoute-Router) hinweg anzeigen. Dieses Dashboard zeigt die ARP-Sitzung aufwärts für privates Peering auf beiden Peers an, aber vollständig abwärts für Microsoft-Peering über Peerings hinweg. Die Standardaggregation (Durchschnitt) wurde für beide Peers verwendet.  

![ARP-Verfügbarkeit pro Peer](./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg) 

## <a name="expressroute-direct-metrics"></a>ExpressRoute Direct-Metriken

### <a name="admin-state---split-by-link"></a>Administratorstatus nach Link
Sie können den Administratorstatus für jeden Link des ExpressRoute Direct-Portpaars anzeigen.

![ExpressRoute Direct: Administratorstatus](./media/expressroute-monitoring-metrics-alerts/adminstate-per-link.jpg)

### <a name="bits-in-per-second---split-by-link"></a>Eingehende Bits pro Sekunde nach Link
Sie können die eingehenden Bits pro Sekunde über beide Links des ExpressRoute Direct-Portpaars anzeigen. 

![ExpressRoute Direct: eingehende Bits pro Sekunde](./media/expressroute-monitoring-metrics-alerts/bits-in-per-second-per-link.jpg)

### <a name="bits-out-per-second---split-by-link"></a>Ausgehende Bits pro Sekunde nach Link
Sie können auch die ausgehenden Bits pro Sekunde über beide Links des ExpressRoute Direct-Portpaars anzeigen. 

![ExpressRoute Direct: ausgehende Bits pro Sekunde](./media/expressroute-monitoring-metrics-alerts/bits-out-per-second-per-link.jpg)

### <a name="line-protocol---split-by-link"></a>Zeilenprotokoll nach Link
Sie können das Zeilenprotokoll für jeden Link des ExpressRoute Direct-Portpaars anzeigen.

![ExpressRoute Direct: Zeilenprotokoll](./media/expressroute-monitoring-metrics-alerts/line-protocol-per-link.jpg)

### <a name="rx-light-level---split-by-link"></a>Rx-Lichtpegel nach Link
Sie können den Rx-Lichtpegel (vom ExpressRoute Direct-Port **empfangene** Lichtintensität) für jeden Port anzeigen. Fehlerfreie Rx-Lichtpegel liegen in der Regel innerhalb eines Bereichs von -10 bis 0 dBm.

![ExpressRoute Direct: Rx-Lichtpegel](./media/expressroute-monitoring-metrics-alerts/rxlight-level-per-link.jpg)

### <a name="tx-light-level---split-by-link"></a>Tx-Lichtpegel nach Link
Sie können den Tx-Lichtpegel (vom ExpressRoute Direct-Port **übertragene** Lichtintensität) für jeden Port anzeigen. Fehlerfreie Tx-Lichtpegel liegen in der Regel innerhalb eines Bereichs von -10 bis 0 DBM.

![ExpressRoute Direct: Rx-Lichtpegel](./media/expressroute-monitoring-metrics-alerts/txlight-level-per-link.jpg)

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
