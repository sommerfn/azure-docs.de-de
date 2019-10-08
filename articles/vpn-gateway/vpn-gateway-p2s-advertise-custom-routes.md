---
title: Ankündigen von benutzerdefinierten Routen für Point-to-Site-VPN-Clients | Azure | Microsoft-Dokumentation
description: Schritte zum Ankündigen von benutzerdefinierten Routen für Ihre Point-to-Site-Clients
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/26/2019
ms.author: cherylmc
ms.openlocfilehash: 38250d1cd9853013ba9721ece0201a8df6dd1b4a
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71336287"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>Ankündigen von benutzerdefinierten Routen für P2S-VPN-Clients

Möglicherweise möchten Sie benutzerdefinierte Routen für alle Ihre Point-to-Site-Clients ankündigen. Beispielsweise dann, wenn Sie Speicherendpunkte in Ihrem VNET aktiviert haben und möchten, dass die Remotebenutzer über die VPN-Verbindung auf diese Speicherkonten zugreifen können. Sie können die IP-Adresse des Speicherendpunkts allen Remotebenutzern ankündigen, damit der Datenverkehr zum Speicherkonto über den VPN-Tunnel und nicht über das öffentliche Internet geleitet wird.

![Beispiel für Multi-Site-Verbindung per Azure VPN Gateway](./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png)

## <a name="to-advertise-custom-routes"></a>So kündigen Sie benutzerdefinierte Routen an

Wenn Sie benutzerdefinierte Routen ankündigen möchten, verwenden Sie das `Set-AzVirtualNetworkGateway cmdlet`. Im folgenden Beispiel wird gezeigt, wie Sie die IP-Adresse für die [Contoso-Speicherkontotabellen](https://contoso.table.core.windows.net) ankündigen.

1. Pingen Sie *contoso.table.core.windows.net*, und notieren Sie sich die IP-Adresse. Beispiel:

    ```cmd
    C:\>ping contoso.table.core.windows.net
    Pinging table.by4prdstr05a.store.core.windows.net [13.88.144.250] with 32 bytes of data:
    ```

2. Führen Sie die folgenden PowerShell-Befehle aus:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 13.88.144.250/32
    ```

3. Um mehrere benutzerdefinierte Routen hinzuzufügen, verwenden Sie ein Komma und Leerzeichen, um die Adressen voneinander zu trennen. Beispiel:

    ```azurepowershell-interactive
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute x.x.x.x/xx , y.y.y.y/yy
    ```
## <a name="to-view-custom-routes"></a>So zeigen Sie benutzerdefinierte Routen an

Verwenden Sie das folgende Beispiel, um benutzerdefinierte Routen anzuzeigen:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  $gw.CustomRoutes | Format-List
  ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum P2S-Routing finden Sie unter [Informationen zum Point-to-Site-Routing](vpn-gateway-about-point-to-site-routing.md).
