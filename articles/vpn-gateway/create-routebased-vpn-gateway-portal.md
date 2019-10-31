---
title: 'Erstellen eines routenbasierten VPN-Gateways: Azure-Portal | Microsoft-Dokumentation'
description: Erstellen eines routenbasierten VPN-Gateways mit dem Azure-Portal
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/24/2019
ms.author: cherylmc
ms.openlocfilehash: 3ab662a4f06b2d73ab0dab52f562398fee23686c
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "71266516"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Erstellen eines routenbasierten VPN-Gateways mit dem Azure-Portal

In diesem Artikel erfahren Sie, wie Sie schnell ein routenbasiertes Azure-VPN-Gateway mit dem Azure-Portal erstellen.  Ein VPN-Gateway wird beim Herstellen einer VPN-Verbindung mit Ihrem lokalen Netzwerk verwendet. Sie können Verbindungen mit VNets auch mit einem VPN-Gateway herstellen. 

Mit den Schritten in diesem Artikel werden ein VNet, ein Subnetz, ein Gatewaysubnetz und ein routenbasiertes VPN-Gateway (Gateway des virtuellen Netzwerks) erstellt. Wenn die Gatewayerstellung abgeschlossen ist, können Sie Verbindungen herstellen. Diese Schritte erfordern ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="vnet"></a>Erstellen eines virtuellen Netzwerks

[!INCLUDE [create-gateway](../../includes/vpn-gateway-create-virtual-network-portal-include.md)]

## <a name="gwvalues"></a>Konfigurieren und Erstellen des Gateways

In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

>[!NOTE]
>Die Basic-Gateway-SKU unterstützt keine IKEv2- oder RADIUS-Authentifizierung. Wenn Sie planen, Verbindungen von Mac-Clients mit Ihrem virtuellen Netzwerk zuzulassen, sollten Sie nicht die Basic-SKU verwenden.

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="viewgw"></a>Anzeigen des VPN-Gateways

1. Nachdem das Gateway erstellt wurde, navigieren Sie im Portal zu VNet1. Das VPN-Gateway wird auf der Seite „Übersicht“ als verbundenes Gerät angezeigt.

   ![Verbundene Geräte](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Verbundene Geräte")

2. Klicken Sie in der Geräteliste auf **VNet1GW**, um weitere Informationen anzuzeigen.

   ![Anzeigen des VPN-Gateways](./media/create-routebased-vpn-gateway-portal/view-gateway.png "Anzeigen des VPN-Gateways")

## <a name="next-steps"></a>Nächste Schritte

Sobald das Gateway fertig gestellt ist, können Sie eine Verbindung Ihres virtuellen Netzwerks mit einem anderen VNet herstellen. Alternativ können Sie eine Verbindung zwischen dem virtuellen Netzwerk und einem lokalen Standort erstellen.

> [!div class="nextstepaction"]
> [Herstellen einer Site-to-Site-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Herstellen einer Point-to-Site-Verbindung](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Herstellen einer Verbindung mit einem anderen VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
