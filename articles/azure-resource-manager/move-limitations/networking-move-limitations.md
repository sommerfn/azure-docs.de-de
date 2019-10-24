---
title: Verschieben von Azure-Netzwerkressourcen in ein neues Abonnement oder eine neue Ressourcengruppe | Microsoft-Dokumentation
description: Verwenden Sie Azure Resource Manager, um virtuelle Netzwerke und andere Netzwerkressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: tomfitz
ms.openlocfilehash: 14a7cb326a3017b1bdbcad21c8483eaaacd54674
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72437804"
---
# <a name="move-guidance-for-networking-resources"></a>Leitfaden zum Verschieben von Netzwerkressourcen

In diesem Artikel wird beschrieben, wie Sie virtuelle Netzwerke und andere Netzwerkressourcen für bestimmte Szenarien verschieben.

## <a name="dependent-resources"></a>Abhängige Ressourcen

Wenn Sie ein virtuelles Netzwerk verschieben, müssen Sie auch dessen abhängige Ressourcen verschieben. Bei VPN-Gateways müssen Sie IP-Adressen, Gateways für virtuelle Netzwerke und alle zugehörigen Verbindungsressourcen verschieben. Gateways des lokalen Netzwerks können sich in einer anderen Ressourcengruppe befinden.

Um einen virtuellen Computer mit einer Netzwerkschnittstellenkarte zu einem neuen Abonnement zu verschieben, müssen Sie alle abhängigen Ressourcen verschieben. Sie verschieben das virtuelle Netzwerk für die Netzwerkschnittstellenkarte, alle anderen Netzwerkschnittstellenkarten für das virtuelle Netzwerk und die VPN-Gateways.

Weitere Informationen finden Sie unter [Szenario für eine abonnementübergreifende Verschiebung](../resource-group-move-resources.md#scenario-for-move-across-subscriptions).

## <a name="peered-virtual-network"></a>Virtuelles Netzwerk mit Peering

Um ein mittels Peering verknüpftes virtuelles Netzwerk zu verschieben, müssen Sie zunächst das Peering des virtuellen Netzwerks deaktivieren. Nach der Deaktivierung können Sie das virtuelle Netzwerk verschieben. Aktivieren Sie nach der Verschiebung das Peering des virtuellen Netzwerks wieder.

## <a name="subnet-links"></a>Subnetzverknüpfungen

Ein virtuelles Netzwerk kann nicht in ein anderes Abonnement verschoben werden, wenn das virtuelle Netzwerk ein Subnetz mit Navigationslinks für Ressourcen enthält. Beispiel: Wenn eine Azure Cache for Redis-Ressource in einem Subnetz bereitgestellt wird, verfügt dieses Subnetz über einen Navigationslink für die Ressource.

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).
