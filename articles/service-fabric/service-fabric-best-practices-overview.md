---
title: Bewährte Methoden für Azure Service Fabric-Anwendungen und -Cluster | Microsoft-Dokumentation
description: Bewährte Methoden für die Verwaltung von Service Fabric-Clustern und -Anwendungen.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/18/2019
ms.author: pepogors
ms.openlocfilehash: 5fdbd3f15b11e4c3975ca29627d5984382bcf049
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206805"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Bewährte Methoden für Azure Service Fabric-Anwendungen und -Cluster

In diesem Artikel finden Sie Links zu Best Practices für die Verwaltung von Azure Service Fabric-Anwendungen und -Clustern. Es wird dringend empfohlen, diese Best Practices umzusetzen, um die Zuverlässigkeit der Produktionsumgebung zu optimieren. Nutzen Sie für den Entwurf Ihrer Produktionslösung entweder eine der [Service Fabric-Clustervorlagen](https://github.com/Azure-Samples/service-fabric-cluster-templates), oder ergänzen Sie eine bestehende Vorlage um diese Methoden.

## <a name="security"></a>Sicherheit

* [Bewährte Methoden für die Sicherheit](service-fabric-best-practices-security.md)

## <a name="networking"></a>Netzwerk

* [Bewährte Methoden für Netzwerke](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Computeplanung und -skalierung

* [Bewährte Methoden für die Skalierung von Computeressourcen](service-fabric-best-practices-capacity-scaling.md)
* [Planen der Computekapazität](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infrastructure-as-Code

* [Bewährte Methoden für die Implementierung von Infrastructure-as-Code](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Überwachung und Diagnose

* [Bewährte Methoden für die Überwachung von Clustern und die Diagnose](service-fabric-best-practices-monitoring.md)

## <a name="application-design"></a>Anwendungsentwurf

* [Bewährte Methoden für en Anwendungsentwurf](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>Checkliste

Setzen Sie nach der Implementierung der oben aufgeführten Methoden alle Best Practices in der Checkliste für die Produktionsbereitschaft um:
* [Azure Service Fabric: Checkliste für die Produktionsbereitschaft](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Nächste Schritte

* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Problembehandlung für Service Fabric: [Leitfäden zur Problembehandlung](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)