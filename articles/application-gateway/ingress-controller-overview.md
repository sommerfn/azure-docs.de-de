---
title: Was ist der Azure Application Gateway-Eingangscontroller?
description: Dieser Artikel bietet eine Einführung in die Funktionsweise des Application Gateway-Eingangscontrollers.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 9635798720667e38a767f26fc2e5f5374e420059
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795547"
---
# <a name="what-is-application-gateway-ingress-controller"></a>Was ist der Application Gateway-Eingangscontroller?
Der Application Gateway-Eingangscontroller (Application Gateway Ingress Controller, AGIC) ist eine Kubernetes-Anwendung, die es Kunden von [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) ermöglicht, den nativen L7-Lastenausgleich von [Application Gateway](https://azure.microsoft.com/services/application-gateway/) zu nutzen, um Cloudsoftware im Internet bereitzustellen. AGIC überwacht den Kubernetes-Cluster, auf dem er gehostet wird, und aktualisiert fortlaufend eine Application Gateway-Instanz, sodass ausgewählte Dienste im Internet bereitgestellt werden.

Der Eingangscontroller wird in einem eigenen Pod im AKS des Kunden ausgeführt. AGIC überwacht eine Teilmenge der Kubernetes-Ressourcen auf Änderungen. Der Status des AKS-Clusters wird in Application Gateway-spezifische Konfiguration übersetzt und auf [Azure Resource Manager (ARM)](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) angewendet.

## <a name="benefits-of-application-gateway-ingress-controller"></a>Vorteile des Application Gateway-Eingangscontrollers
AGIC ermöglicht der Bereitstellung, mehrere AKS-Cluster mit einem einzelnen Application Gateway-Eingangscontroller zu steuern. AGIC hilft auch, die Notwendigkeit zu vermeiden, einen weiteren Lastenausgleich bzw. eine öffentliche IP-Adresse vor dem AKS-Cluster einzusetzen, und vermeidet mehrere Hops in Ihrem Datenpfad, bevor Anforderungen den AKS-Cluster erreichen. Application Gateway kommuniziert direkt mit Pods über ihre private IP-Adresse und benötigt keine NodePort- oder KubeProxy-Dienste. Dies führt auch zu einer besseren Leistung für Ihre Bereitstellungen.

Eingangscontroller werden exklusiv von den Standard_v2- und WAF_v2-SKUs unterstützt, wodurch auch Vorteile der automatischen Skalierung erzielt werden. Application Gateway kann als Reaktion auf eine Erhöhung oder Verringerung der Datenverkehrslast reagieren und eine entsprechende Skalierung ausführen, ohne Ressourcen aus Ihrem AKS-Cluster zu verbrauchen.

Die Verwendung von Application Gateway zusätzlich zu AGIC schützt auch Ihren AKS-Cluster durch die Bereitstellung von TLS-Richtlinien und WAF-Funktionen (Web Application Firewall).

![Azure Application Gateway und AKS](./media/application-gateway-ingress-controller-overview/architecture.png)

AGIC wird über die Kubernetes-[Eingangsressource](http://kubernetes.io/docs/user-guide/ingress/) sowie über Dienste und Bereitstellungen/Pods konfiguriert. Er bietet eine Reihe von Features und nutzt dabei den nativen L7-Lastenausgleich von Application Gateway in Azure. Beispiele für solche Features:
  - URL-Routing
  - Cookiebasierte Affinität
  - SSL-Terminierung
  - End-to-End-SSL
  - Unterstützung für öffentliche, private und hybride Websites
  - Integrierte Web Application Firewall

AGIC kann mehrere Namespaces verarbeiten und verfügt über ProhibitedTargets. Dies bedeutet, dass AGIC das Application Gateway speziell für AKS-Cluster konfigurieren kann, ohne dass sich dies auf andere vorhandene Back-Ends auswirkt. 

## <a name="next-steps"></a>Nächste Schritte

- [**Greenfield-Bereitstellung**](ingress-controller-install-new.md): Anleitungen zum Installieren von AGIC, AKS und Application Gateway in einer leeren Infrastruktur.
- [**Brownfield-Bereitstellung**](ingress-controller-install-existing.md): Installieren von AGIC in einer vorhandenen AKS- und Application Gateway-Umgebung.

