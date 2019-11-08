---
title: Aktualisieren eines Azure Virtual WAN von Basic auf Standard – Azure-Portal | Microsoft-Dokumentation
description: Sie können den Virtual WAN-Typ aktualisieren, um die Funktionalität zu erhöhen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 83fd5bafb5496908403c50dc0e000fd33a836c95
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511199"
---
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>Upgrade eines Virtual WAN von Basic auf Standard

Dieser Artikel unterstützt Sie beim Upgrade eines Basic WAN auf ein Standard WAN. Bei einem WAN vom Typ „Basic“ werden alle darin enthaltenen Hubs als Basic-SKU-Hubs erstellt. Basic-Hubs sind auf Site-to-Site-VPN-Funktionen beschränkt. Bei einem WAN vom Typ „Standard“ werden alle darin enthaltenen Hubs als Standard-SKU-Hubs erstellt. Wenn Sie Standard-Hubs verwenden, können Sie eine Übertragung über ExpressRoute, Benutzer-VPN (Point-to-Site-VPN), einen vollständig vermaschten Hub und zwischen VNets über Azure-Hubs aktivieren.

Die folgende Tabelle zeigt die verfügbaren Konfigurationen für die beiden WAN-Typen:

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>So ändern Sie den Virtual WAN-Typ

1. Wählen Sie auf der Seite für das Virtual WAN **Konfiguration** aus, um die Konfigurationsseite zu öffnen.

   ![Virtual WAN-Diagramm](./media/upgrade-virtual-wan/1.png)
2. Wählen Sie in der Dropdown-Liste **Standard** als Virtual WAN-Typ aus.

   ![Virtual WAN-Diagramm](./media/upgrade-virtual-wan/2.png)
3. Wenn Sie ein Upgrade auf ein Standard-Virtual WAN durchführen, können Sie nicht mehr zu einem Basic-Virtual WAN zurückkehren. Wählen Sie **Bestätigen** aus, wenn Sie ein Upgrade durchführen möchten.

   ![Virtual WAN-Diagramm](./media/upgrade-virtual-wan/4.png)
4. Nachdem die Änderung gespeichert wurde, sieht die Virtual WAN-Seite in etwa wie in diesem Beispiel aus.

   ![Virtual WAN-Diagramm](./media/upgrade-virtual-wan/5.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).