---
title: Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke | Microsoft-Dokumentation
description: Informieren Sie sich über Gateways für virtuelle Netzwerke für ExpressRoute. Dieser Artikel enthält Informationen zu Gateway-SKUs und -Typen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: ba03d643c8d3770da60d4225d6c2b84d2a07766f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72325531"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>ExpressRoute-Gateway für virtuelle Netzwerke und FastPath
Wenn Sie Ihr virtuelles Azure-Netzwerk und Ihr lokales Netzwerk über ExpressRoute verbinden möchten, müssen Sie ein virtuelles Netzwerkgateway erstellen. Ein Gateway für virtuelle Netzwerke dient zwei Zwecken: dem Austausch von IP-Routen zwischen den Netzwerken und der Weiterleitung des Netzwerkdatenverkehrs. In diesem Artikel werden Gatewaytypen, Gateway-SKUs und die geschätzte Leistung nach SKU erläutert. In diesem Artikel wird auch ExpressRoute [FastPath](#fastpath) erläutert, ein Feature, das es dem Netzwerkdatenverkehr aus Ihrem lokalen Netzwerk ermöglicht, das virtuelle Netzwerkgateway zu umgehen, um die Leistung zu verbessern.

## <a name="gateway-types"></a>Gatewaytypen

Beim Erstellen eines Gateways für virtuelle Netzwerke müssen mehrere Einstellungen angegeben werden. Eine dieser erforderlichen Einstellungen, „-GatewayType“, gibt an, ob das Gateway für ExpressRoute- oder für VPN-Datenverkehr verwendet wird. Die zwei Gatewaytypen sind:

* **Vpn** – Wenn verschlüsselter Netzwerkdatenverkehr über das öffentliche Internet gesendet wird, verwenden Sie den Gatewaytyp „Vpn“. Dies wird auch als VPN Gateway bezeichnet. Bei Site-to-Site-, Point-to-Site- und VNet-zu-VNet-Verbindungen wird jeweils VPN Gateway verwendet.

* **ExpressRoute** – Verwenden Sie den Gatewaytyp „ExpressRoute“, wenn Netzwerkdatenverkehr über eine private Verbindung gesendet wird. Dies wird auch als ExpressRoute-Gateway bezeichnet und ist der Gatewaytyp, der auch zum Konfigurieren von ExpressRoute verwendet wird.

Ein virtuelles Netzwerk kann pro Gatewaytyp immer nur über ein einzelnes virtuelles Netzwerkgateway verfügen. So können Sie beispielsweise ein virtuelles Netzwerkgateway mit „-GatewayType Vpn“ und ein virtuelles Netzwerkgateway mit „-GatewayType ExpressRoute“ verwenden.

## <a name="gwsku"></a>Gateway-SKUs
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Wenn Sie das Gateway auf eine leistungsfähigere Gateway-SKU aktualisieren möchten, können Sie in den meisten Fällen das PowerShell-Cmdlet „Resize-AzVirtualNetworkGateway“ verwenden. Mit diesem können Sie Upgrades von Standard- auf HighPerformance-SKUs durchführen. Für ein Upgrade auf die UltraPerformance-SKU müssen Sie jedoch das Gateway neu erstellen. Das Neuerstellen eines Gateways führt zu Ausfällen.

### <a name="aggthroughput"></a>Geschätzte Leistungen nach Gateway-SKU
In der folgenden Tabelle sind die Gatewaytypen und die geschätzten Leistungen angegeben. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> Die Anwendungsleistung hängt von mehreren Faktoren ab, z.B. der End-to-End-Latenz und der Anzahl der Datenflüsse, die die Anwendung öffnet. Die Zahlen in der Tabelle stellen die Obergrenze dar, die die Anwendung theoretisch in einer idealen Umgebung erzielen kann.
>
>

### <a name="zrgw"></a>SKUs für zonenredundante Gateways

Sie können VPN- und ExpressRoute-Gateways auch in Azure-Verfügbarkeitszonen bereitstellen. Dadurch werden sie physisch und logisch in verschiedene Verfügbarkeitszonen unterteilt, wodurch Ihre lokale Netzwerkverbindung zu Azure vor Ausfällen auf Zonenebene geschützt wird.

![Zonenredundante ExpressRoute-Gateway](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Zonenredundante Gateways verwenden bestimmte neue Gateway-SKUs für ExpressRoute-Gateway.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Die neuen Gateway-SKUs unterstützen zudem andere Bereitstellungsoptionen, die Ihre Anforderungen am besten zu erfüllen. Wenn Sie ein virtuelles Netzwerkgateway mit den neuen Gateway-SKUs erstellen, haben Sie auch die Möglichkeit, das Gateway in einer bestimmten Zone bereitzustellen. Dies wird als zonenbasiertes Gateway bezeichnet. Wenn Sie ein zonenbasiertes Gateway bereitstellen, werden alle Instanzen des Gateways in derselben Verfügbarkeitszone bereitgestellt.

## <a name="fastpath"></a>FastPath
Das virtuelle Netzwerkgateway ExpressRoute wurde entwickelt, um Netzwerkrouten auszutauschen und den Netzwerkdatenverkehr zu steuern. FastPath wurde entwickelt, um die Datenpfadleistung zwischen Ihrem lokalen und dem virtuellen Netzwerk zu verbessern. Wenn aktiviert, sendet FastPath den Netzwerkdatenverkehr direkt an virtuelle Computer im virtuellen Netzwerk und umgeht dabei das Gateway. 

FastPath ist unter allen ExpressRoute-Verbindungen verfügbar. Es erfordert weiterhin die Erstellung eines virtuellen Netzwerkgateways für den Austausch von Routen zwischen virtuellem und lokalem Netzwerk. Das virtuelle Netzwerkgateway muss entweder „Ultra Performance“ oder „ErGw3AZ“ sein.

FastPath unterstützt die folgenden Features nicht:
* UDR auf Gatewaysubnetz: Wenn Sie ein UDR auf das Gatewaysubnetz Ihres virtuellen Netzwerks anwenden, wird der Netzwerkdatenverkehr aus Ihrem lokalen Netzwerk weiterhin an das virtuelle Netzwerkgateway gesendet.
* VNet-Peering: Wenn andere virtuelle Netzwerke per Peering mit dem Netzwerk verbunden sind, das mit ExpressRoute verbunden ist, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den anderen virtuellen Netzwerken (d. h. den sogenannten virtuellen „Spoke“-Netzwerken) weiterhin an das virtuelle Netzwerkgateway gesendet. Die Problemumgehung besteht darin, alle virtuellen Netzwerke direkt mit der ExpressRoute-Verbindung zu verbinden.
* Basic-Load Balancer: Wenn Sie einen internen Basic-Load Balancer in Ihrem virtuellen Netzwerk bereitstellen oder der Azure-PaaS-Dienst, den Sie in Ihrem virtuellen Netzwerk bereitstellen, einen internen Basic-Load Balancer verwendet, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den virtuellen IP-Adressen, die auf dem Basic-Load Balancer gehostet werden, an das Gateway des virtuellen Netzwerks gesendet. Die Lösung besteht darin, den Basic-Load Balancer auf einen [Standard-Load Balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) zu aktualisieren.
* Private Link: Wenn Sie von Ihrem lokalen Netzwerk eine Verbindung mit einem [privaten Endpunkt](../private-link/private-link-overview.md) in Ihrem virtuellen Netzwerk herstellen, erfolgt die Verbindung über das Gateway für virtuelle Netzwerke.
 
## <a name="resources"></a>REST-APIs und PowerShell-Cmdlets
Zusätzliche technische Ressourcen und spezielle Syntaxanforderungen bei der Verwendung von REST-APIs und PowerShell-Cmdlets für Gatewaykonfigurationen für virtuelle Netzwerke finden Sie auf den folgenden Seiten:

| **Klassisch** | **Ressourcen-Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [REST-API](https://msdn.microsoft.com/library/jj154113.aspx) |[REST-API](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu verfügbaren Verbindungskonfigurationen finden Sie in der [ExpressRoute-Übersicht](expressroute-introduction.md) .

Weitere Informationen zum Erstellen von ExpressRoute-Gateways finden Sie unter [Erstellen eines Gateways für ein virtuelles Netzwerk für ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

Unter [Erstellen eines zonenredundanten Gateways für virtuelle Netzwerke](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) finden Sie weitere Informationen zur Konfiguration zonenredundanter Gateways.

Weitere Informationen zum Aktivieren von FastPath finden Sie unter [Verknüpfen von virtuellen Netzwerken mit ExpressRoute](expressroute-howto-linkvnet-arm.md). 
