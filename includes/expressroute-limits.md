---
title: Includedatei
description: Includedatei
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c025c431d826d3a2951a9eb5c09308695e172887
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170725"
---
| Resource | Standard/maximales Limit |
| --- | --- |
| ExpressRoute-Verbindungen pro Abonnement |10 |
| ExpressRoute-Verbindungen pro Region und Abonnement mit Azure Resource Manager |10 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Standard |4.000 |
| Maximale Anzahl von Routen für Private Peering in Azure mit ExpressRoute-Premium-Add-on |10.000 |
| Maximale Anzahl von Routen für privates Azure-Peering aus dem VNet-Adressraum für eine ExpressRoute-Verbindung |200 | 
| Maximale Anzahl von Routen für Microsoft Azure-Peering mit ExpressRoute-Standard |200 |
| Maximale Anzahl von Routen für Peering in Microsoft Azure mit ExpressRoute-Premium-Add-on |200 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk am selben Peeringstandort verknüpft sind |4 |
| Maximale Anzahl von ExpressRoute-Verbindungen, die mit dem gleichen virtuellen Netzwerk an unterschiedlichen Peeringstandorten verknüpft sind |4 |
| Anzahl der pro ExpressRoute-Leitung zulässigen virtuellen Netzwerkverbindungen |Siehe hierzu die folgende Tabelle. |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Anzahl der virtuellen Netzwerke pro ExpressRoute-Verbindung
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

*Nur ExpressRoute Direct
