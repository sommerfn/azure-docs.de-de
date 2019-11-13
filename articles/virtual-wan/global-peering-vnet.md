---
title: Konfigurieren des globalen VNet-Peerings für Azure Virtual WAN | Microsoft-Dokumentation
description: Erstellen einer Verbindung für ein VNet in einer anderen Region zu Ihrem Virtual WAN-Hub.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 340472f84d2dd2c4f46d180992745a57e8ad1884
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587067"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>Konfigurieren des globalen VNet-Peerings (regionsübergreifendes VNet) für Virtual WAN

Sie können für ein VNet in einer anderen Region eine Verbindung zu Ihrem Virtual WAN-Hub erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass die folgenden Kriterien erfüllt sind:

* Das regionsübergreifende VNet (Spoke) ist nicht mit einem anderen Virtual WAN-Hub verbunden. Ein Spoke kann nur mit einem virtuellen Hub verbunden werden.
* Das VNet (Spoke) enthält kein VNet-Gateway (z. B. ein Azure VPN Gateway oder ein ExpressRoute-Gateway für virtuelle Netzwerke). Wenn das VNet ein Gateway für virtuelle Netzwerke enthält, müssen Sie das Gateway entfernen, bevor Sie eine Verbindung vom Spoke-VNet zum Hub erstellen.

## <a name="register"></a>Registrieren dieses Features

Sie können sich mit PowerShell für dieses Feature registrieren. Wenn Sie im folgenden Beispiel „Ausprobieren“ auswählen, wird Azure Cloud Shell geöffnet, sodass Sie die PowerShell-Cmdlets nicht lokal auf Ihrem Computer installieren müssen. Falls erforderlich, können Sie Abonnements mithilfe des Cmdlets „Select-AzSubscription -SubscriptionId <subid>“ ändern.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

## <a name="verify"></a>Überprüfen der Registrierung

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

## <a name="hub"></a>Verbinden eines VNet mit dem Hub

In diesem Schritt erstellen Sie die Peeringverbindung zwischen Ihrem Hub und dem regionsübergreifenden VNet. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Virtuelle Netzwerkverbindungen**.
2. Klicken Sie auf der Seite für die VNET-Verbindung auf **+Add connection** (+Verbindung hinzufügen).
3. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk kann nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein.
4. Klicken Sie auf **OK**, um die Peeringverbindung zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie in der [Übersicht über Virtual WAN](virtual-wan-about.md).