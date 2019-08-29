---
title: Verschieben von Azure-Netzwerkressourcen in ein neues Abonnement oder eine neue Ressourcengruppe | Microsoft-Dokumentation
description: Verwenden Sie Azure Resource Manager, um virtuelle Netzwerke und andere Netzwerkressourcen in eine neue Ressourcengruppe oder ein neues Abonnement zu verschieben.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: c41ab1c3309243fa3a96d907f620ffeff10376a1
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626119"
---
# <a name="move-guidance-for-networking-resources"></a>Leitfaden zum Verschieben von Netzwerkressourcen

In diesem Artikel wird beschrieben, wie Sie virtuelle Netzwerke und andere Netzwerkressourcen für bestimmte Szenarien verschieben.

## <a name="dependent-resources"></a>Abhängige Ressourcen

Wenn Sie ein virtuelles Netzwerk verschieben, müssen Sie auch dessen abhängige Ressourcen verschieben. Bei VPN-Gateways müssen Sie IP-Adressen, Gateways für virtuelle Netzwerke und alle zugehörigen Verbindungsressourcen verschieben. Gateways des lokalen Netzwerks können sich in einer anderen Ressourcengruppe befinden.

Um einen virtuellen Computer mit einer Netzwerkschnittstellenkarte zu verschieben, müssen Sie alle abhängigen Ressourcen verschieben. Sie verschieben das virtuelle Netzwerk für die Netzwerkschnittstellenkarte, alle anderen Netzwerkschnittstellenkarten für das virtuelle Netzwerk und die VPN-Gateways.

## <a name="state-of-dependent-resources"></a>Zustand von abhängigen Ressourcen

Wenn die Quell- oder Zielgruppenressource ein virtuelles Netzwerk enthält, werden während des Verschiebens die Zustände aller abhängigen Ressourcen des virtuellen Netzwerks überprüft. Wenn eine dieser Ressourcen den Zustand „Ausgefallen“ aufweist, wird die Verschiebung blockiert. Wenn beispielsweise ein virtueller Computer, der das virtuelle Netzwerk nutzt, ausgefallen ist, wird das Verschieben blockiert. Das Verschieben wird selbst dann blockiert, wenn der virtuelle Computer nicht zu den zu verschiebenden Ressourcen gehört und sich nicht in einer der Ressourcengruppen des Verschiebevorgangs befindet. Um dieses Problem zu vermeiden, verschieben Sie Ihre Ressourcen in eine Ressourcengruppe ohne virtuelles Netzwerk.

## <a name="peered-virtual-network"></a>Virtuelles Netzwerk mit Peering

Um ein mittels Peering verknüpftes virtuelles Netzwerk zu verschieben, müssen Sie zunächst das Peering des virtuellen Netzwerks deaktivieren. Nach der Deaktivierung können Sie das virtuelle Netzwerk verschieben. Aktivieren Sie nach der Verschiebung das Peering des virtuellen Netzwerks wieder.

## <a name="subnet-links"></a>Subnetzverknüpfungen

Ein virtuelles Netzwerk kann nicht in ein anderes Abonnement verschoben werden, wenn das virtuelle Netzwerk ein Subnetz mit Navigationslinks für Ressourcen enthält. Beispiel: Wenn eine Azure Cache for Redis-Ressource in einem Subnetz bereitgestellt wird, verfügt dieses Subnetz über einen Navigationslink für die Ressource.

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../resource-group-move-resources.md).
