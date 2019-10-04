---
title: Azure CLI-Beispiele für virtuelle Netzwerke | Microsoft-Dokumentation
description: Azure CLI-Beispiele für virtuelle Netzwerke.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: 4caa19ad22faa0a921fb6397e5b376b65a29e519
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249069"
---
# <a name="azure-cli-samples-for-virtual-network"></a>Azure CLI-Beispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Bashskripts mit Azure CLI-Befehlen:

| | |
|----|----|
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-cli-sample-multi-tier-application.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr an das Front-End-Subnetz ist auf HTTP und SSH beschränkt, während der Datenverkehr an das Back-End-Subnetz auf MySQL Port 3306 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-cli-sample-peer-two-virtual-networks.md) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-cli-sample-route-traffic-through-nva.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-cli-sample-filter-network-traffic.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP, HTTPS und SSH beschränkt. Aus dem Back-End-Subnetz an das Internet ausgehender Datenverkehr ist nicht zulässig. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6)mit Load Balancer im Tarif „Basic“](./scripts/virtual-network-cli-sample-ipv6-dual-stack.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 + IPv6) mit zwei virtuellen Computern und einem Azure Load Balancer Basic mit öffentlichen IPv4- und IPv6-Adressen bereit. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6) mit Load Balancer Standard](./scripts/virtual-network-cli-sample-ipv6-dual-stack-standard-load-balancer.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 und IPv6) mit zwei virtuellen Computern und einer Azure Load Balancer Standard-Instanz mit öffentlichen IPv4- und IPv6-Adressen bereit. |