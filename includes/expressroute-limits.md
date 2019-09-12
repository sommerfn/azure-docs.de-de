---
title: include file
description: include file
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 07/25/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 45d34297bf37a6e46bc57e95ff49def49051e32e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "67622099"
---
| Resource | Standard/maximales Limit |
| --- | --- |
| ExpressRoute-Verbindungen pro Abonnement |10 |
| ExpressRoute-Verbindungen pro Region und Abonnement mit Azure Resource Manager |10 |
| Maximale Anzahl der für das private Azure-Peering angekündigten Routen mit ExpressRoute Standard |4\.000 |
| Maximale Anzahl der für das private Azure-Peering angekündigten Routen mit ExpressRoute Premium-Add-On |10.000 |
| Maximale Anzahl der vom privaten Azure-Peering angekündigten Routen aus dem VNET-Adressraum für eine ExpressRoute-Verbindung |200 |
| Maximale Anzahl der für das Microsoft-Peering angekündigten Routen mit ExpressRoute Standard |200 |
| Maximale Anzahl der für das Microsoft-Peering angekündigten Routen mit ExpressRoute Premium-Add-On |200 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk am selben Peeringstandort verknüpft sind |4 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk an unterschiedlichen Peeringstandorten verknüpft sind |4 |
| Anzahl der pro ExpressRoute-Leitung zulässigen virtuellen Netzwerkverbindungen |Siehe Tabelle [Anzahl von virtuellen Netzwerken pro ExpressRoute-Leitung](#vnetpercircuit).  |

#### <a name="vnetpercircuit"></a>Anzahl von virtuellen Netzwerken pro ExpressRoute-Leitung
| **Verbindungsgröße** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks für Standard** | **Anzahl der Verknüpfungen eines virtuellen Netzwerks mit Premium-Add-on** |
| --- | --- | --- |
| 50 MBit/s |10 |20 |
| 100 MBit/s |10 |25 |
| 200 MBit/s |10 |25 |
| 500 MBit/s |10 |40 |
| 1 GBit/s |10 |50 |
| 2 GBit/s |10 |60 |
| 5 GBit/s |10 |75 |
| 10 GBit/s |10 |100 |
| 40 GBit/s* |10 |100 |
| 100 GBit/s* |10 |100 |

**100 GBit/s nur bei ExpressRoute Direct*
