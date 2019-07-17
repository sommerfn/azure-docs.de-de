---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/08/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8f8d366961049deb3eda193718ccb553aac930e3
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67666375"
---
Sie bezahlen für zwei Dinge: die stündlichen Computekosten für das Gateway des virtuellen Netzwerks und die Übertragung der Ausgangsdaten vom Gateway des virtuellen Netzwerks. Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) . Preise für ältere Gateway-SKUs finden Sie auf der [ExpressRoute-Preisseite](https://azure.microsoft.com/pricing/details/expressroute) im Abschnitt **Gateways für virtuelle Netzwerke**.

**Computekosten für virtuelles Netzwerkgateway**<br>Für jedes Gateway des virtuellen Netzwerks fallen stündliche Computekosten an. Der Preis basiert auf der Gateway-SKU, die Sie beim Erstellen des Gateways des virtuellen Netzwerks angeben. Die Kosten für das eigentliche Gateway fallen zusätzlich zur Datenübertragung an, die über das Gateway durchgeführt wird. Die Kosten eines aktiv-aktiven Setups sind gleich wie für aktiv-passiv.

**Datenübertragungskosten**<br>Datenübertragungskosten werden basierend auf ausgehendem Datenverkehr vom Gateway des virtuellen Quellnetzwerks berechnet.

* Wenn Sie Datenverkehr an Ihr lokales VPN-Gerät senden, wird dafür die Internetrate für die Übertragung von Ausgangsdaten berechnet.
* Wenn Sie Datenverkehr zwischen virtuellen Netzwerken in unterschiedlichen Regionen senden, richtet sich der Preis nach der Region.
* Wenn Sie Datenverkehr nur zwischen virtuellen Netzwerken senden, die sich in derselben Region befinden, fallen keine Kosten für die Datenübertragung an. Der Datenverkehr zwischen VNets in derselben Region ist kostenlos.
