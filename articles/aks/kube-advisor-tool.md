---
title: Überprüfen Ihrer Kubernetes-Bereitstellungen in Azure für die Implementierung von Best Practices
description: Erfahren Sie, wie Sie Ihre Bereitstellungen von Azure Kubernetes Service mit kube-advisor auf die Implementierung von Best Practices überprüfen können.
services: container-service
author: seanmck
ms.service: container-service
ms.topic: troubleshooting
ms.date: 11/05/2018
ms.author: seanmck
ms.openlocfilehash: 03c5eb2e32a0a8ec51844511276d9efba5651068
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "65073775"
---
# <a name="checking-for-kubernetes-best-practices-in-your-cluster"></a>Überprüfen auf Best Practices von Kubernetes in Ihrem Cluster

Es gibt verschiedene Best Practices, die Sie in Ihren Kubernetes-Bereitstellungen nutzen sollten, um die beste Leistung und Stabilität in Ihren Anwendungen sicherzustellen. Sie können mit dem kube-advisor-Tool Bereitstellungen suchen, die diese Empfehlungen nicht befolgen.

## <a name="about-kube-advisor"></a>Informationen zu kube-advisor

Das [kube-advisor-Tool][kube-advisor-github] ist ein einzelner Container, der in Ihrem Cluster ausgeführt wird. Er fragt den Kubernetes-API-Server nach Informationen zu Ihren Bereitstellungen ab und gibt einen Satz von Verbesserungsvorschlägen zurück.

Das kube-advisor-Tool kann Berichte zur Ressourcenanforderung und zu Grenzwerten erstellen, die in PodSpecs für Windows- und Linux-Anwendungen fehlen, das kube-advisor-Tool selbst muss jedoch auf einem Linux-Pod geplant werden. Sie können einen Pod mit einem [Knoten-Selektor][ k8s-node-selector] in der Konfiguration des Pods so planen, dass er auf einem Knotenpool mit einem bestimmten Betriebssystem ausgeführt wird.

> [!NOTE]
> Das kube-advisor-Tool wird von Microsoft auf „Beste Leistung“-Basis unterstützt. Probleme und Vorschläge sollten auf GitHub eingereicht werden.

## <a name="running-kube-advisor"></a>Ausführung von kube-advisor

Um das Tool auf einem Cluster auszuführen, der für die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](azure-ad-integration.md) konfiguriert ist, verwenden Sie die folgenden Befehle. Der erste Befehl erstellt ein Kubernetes-Dienstkonto. Der zweite Befehl führt das Tool unter Verwendung dieses Dienstkontos in einem Pod aus und konfiguriert den Pod für die Löschung nach dem Beenden. 

```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml

kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never --overrides="{ \"apiVersion\": \"v1\", \"spec\": { \"serviceAccountName\": \"kube-advisor\" } }"
```

Wenn Sie keine rollenbasierte Zugriffssteuerung verwenden, können Sie den Befehl wie folgt ausführen:

```bash
kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never
```

Innerhalb weniger Sekunden sollte eine Tabelle mit der Beschreibung der potenzielle Verbesserungen für Ihre Bereitstellungen angezeigt werden.

![Kube-advisor-Ausgabe](media/kube-advisor-tool/kube-advisor-output.png)

## <a name="checks-performed"></a>Durchgeführte Prüfungen

Das Tool überprüft mehrere Best Practices von Kubernetes, jeweils mit eigenen Problemlösungsvorschlägen.

### <a name="resource-requests-and-limits"></a>Ressourcenanforderungen und Einschränkungen

Kubernetes unterstützt das Definieren von [Ressourcenanforderungen und Einschränkungen schränkt in Podspezifikationen][kube-cpumem]. Die Anforderung definiert die minimale CPU und den minimalen Arbeitsspeicher zum Ausführen des Containers. Die Einschränkung definiert die maximale CPU und den maximalen Arbeitsspeicher.

Standardmäßig werden keine Anforderungen oder Einschränkungen in den Podspezifikationen festgelegt. Dies kann zur Überlastung von Knoten und Blockierung von Containern führen. Das kube-advisor-Tool hebt Pods ohne Anforderungen und Einschränkungen hervor.

## <a name="cleaning-up"></a>Bereinigen

Wenn in Ihrem Cluster RBAC aktiviert ist, können Sie die `ClusterRoleBinding` nach dem Ausführen des Tools mit folgendem Befehl bereinigen:

```bash
kubectl delete -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml
```

Wenn Sie das Tool für einen Cluster ausführen, in dem RBAC nicht aktiviert ist, ist keine Bereinigung erforderlich.

## <a name="next-steps"></a>Nächste Schritte

- [Problembehandlung mit Azure Kubernetes Service](troubleshooting.md)

<!-- RESOURCES -->

[kube-cpumem]: https://github.com/Azure/azure-quickstart-templates
[kube-advisor-github]: https://github.com/azure/kube-advisor
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors