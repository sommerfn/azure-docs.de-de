---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4842c55b2b1fd23f4d6b7996ccf02e7141504836
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495729"
---
Sie können das PowerShell-Cmdlet `Resize-AzVirtualNetworkGateway` zum Aktualisieren oder Herabstufen einer Generation1- oder Generation2-SKU verwenden (die Größe aller VpnGw-SKUs kann mit Ausnahme von Basic-SKUs geändert werden). Verwenden Sie bei Verwendung der Basic-Gateway-SKU stattdessen [diese Anweisungen](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize), um die Größe Ihres Gateways anzupassen.

Im folgenden PowerShell-Beispiel wird das Ändern der Größe einer Gateway-SKU in „VpnGw2“ veranschaulicht.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Sie können die Größe eines Gateways auch über das Azure-Portal anpassen. Wählen Sie hierzu auf der Seite **Konfiguration** für Ihr Gateway für virtuelle Netzwerke in der Dropdownliste eine andere SKU aus.
