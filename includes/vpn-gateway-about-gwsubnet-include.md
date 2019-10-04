---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/24/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5a734963ced9daefda2b7b6f4a52fd9ef437eddc
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71269277"
---
Das Gateway für virtuelle Netzwerke verwendet ein bestimmtes Subnetz, das als „Gatewaysubnetz“ bezeichnet wird. Das Gatewaysubnetz ist Teil des IP-Adressbereichs für das virtuelle Netzwerk, den Sie beim Konfigurieren Ihres virtuellen Netzwerks angeben. Es enthält die IP-Adressen, die von den Ressourcen und Diensten des Gateways für virtuelle Netzwerke verwendet werden. Das Subnetz muss „GatewaySubnet“ heißen, damit Azure die Gatewayressourcen bereitstellen kann. Für die Bereitstellung der Gatewayressourcen kann kein anderes Subnetz angegeben werden. Wenn Sie über kein Subnetz namens „GatewaySubnet“ verfügen, tritt beim Erstellen des VPN-Gateways ein Fehler auf.

Bei der Gatewayerstellung geben Sie die Anzahl der im Subnetz enthaltenen IP-Adressen an. Wie viele IP-Adressen erforderlich sind, hängt von der VPN-Gatewaykonfiguration ab, die Sie erstellen möchten. Einige Konfigurationen erfordern mehr IP-Adressen als andere. Es empfiehlt sich, ein Gatewaysubnetz mit /27 oder /28 zu erstellen.

Sollte ein Fehler mit dem Hinweis angezeigt werden, dass sich der Adressraum mit einem Subnetz überschneidet oder dass das Subnetz nicht im Adressraum für Ihr virtuelles Netzwerk enthalten ist, überprüfen Sie Ihren VNet-Adressbereich. Möglicherweise sind in dem Adressbereich, den Sie für Ihr virtuelles Netzwerk erstellt haben, nicht genügend IP-Adressen verfügbar. Wenn Ihr Standardsubnetz beispielsweise den gesamten Adressbereich umfasst, stehen für die Erstellung zusätzlicher Subnetze keine IP-Adressen mehr zur Verfügung. Sie können entweder Ihre Subnetze innerhalb des vorhandenen Adressraums anpassen, um IP-Adressen freizugeben, oder einen zusätzlichen Adressbereich angeben und das Gatewaysubnetz dort erstellen.