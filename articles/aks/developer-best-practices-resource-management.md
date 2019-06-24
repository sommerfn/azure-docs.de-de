---
title: Bewährte Entwicklermethoden – Ressourcenverwaltung in Azure Kubernetes Service (AKS)
description: Lernen Sie bewährte Anwendungsentwicklermethoden für die Ressourcenverwaltung in Azure Kubernetes Service (AKS) kennen.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: zarhoads
ms.openlocfilehash: 69f60036bd718264174bf1befe832305e250e77c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "65073949"
---
# <a name="best-practices-for-application-developers-to-manage-resources-in-azure-kubernetes-service-aks"></a>Bewährte Anwendungsentwicklermethoden zum Verwalten von Ressourcen in Azure Kubernetes Service (AKS)

Beim Entwickeln und Ausführen von Anwendungen in Azure Kubernetes Service (AKS) sind jedoch einige wichtige Aspekte zu berücksichtigen. Die Art, in der Sie Ihre Anwendungsbereitstellungen verwalten, kann die Benutzerfreundlichkeit der Dienste beeinträchtigen, die Sie bereitstellen. Um erfolgreich zu sein, beachten Sie einige bewährte Methoden, wenn Sie Anwendungen entwickeln und in AKS ausführen.

Dieser Artikel zu bewährten Methoden konzentriert sich darauf, wie Sie Ihre Cluster und Workloads aus der Perspektive des Anwendungsentwicklers ausführen. Weitere Informationen zu bewährten Verwaltungsmethoden finden Sie unter [Bewährte Methoden der Clusterisolierung in Azure Kubernetes Service (AKS)][operator-best-practices-isolation]. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Podressourcenanforderungen und -grenzwerte
> * Methoden zum Entwickeln und Bereitstellen von Anwendungen mit Dev Spaces und Visual Studio Code
> * Verwenden des `kube-advisor`-Tools, um nach Problemen mit Bereitstellungen zu suchen

## <a name="define-pod-resource-requests-and-limits"></a>Definieren von Podressourcenanforderungen und -grenzwerten

**Best Practice-Anleitung**: Legen Sie Podanforderungen und -grenzwerte für alle Pods in Ihren YAML-Manifesten fest. Wenn der AKS-Cluster *Ressourcenkontingente* verwendet, wird Ihre Bereitstellung möglicherweise abgelehnt, wenn Sie diese Werte nicht definieren.

Eine primäre Methode zum Verwalten der Computeressourcen in einem AKS-Cluster ist die Verwendung von Podanforderungen und -grenzwerten. Diese Anforderungen und Grenzwerte teilen dem Kubernetes-Planer mit, welche Computeressourcen einem Pod zugewiesen werden sollten.

* **Podanforderungen** definieren eine festgelegte CPU-Leistung und Arbeitsspeichermenge, die der Pod benötigt. Diese Anforderungen sollten dem Ausmaß an Computeressourcen entsprechen, die der Pod für eine akzeptable Leistung benötigt.
    * Wenn der Kubernetes-Planer versucht, einen Pod auf einem Knoten zu platzieren, wird anhand der Podanforderungen bestimmt, auf welchem Knoten genügend Ressourcen verfügbar wird.
    * Überwachen Sie die Leistung Ihrer Anwendung, um sie diesen Anforderungen anzupassen, damit Sie nicht weniger Ressourcen definieren, als für eine akzeptable Leistung erforderlich sind.
* **Podgrenzwerte** sind das Maximum an CPU-Leistung und Arbeitsspeichermenge, das ein Pod verwenden kann. Diese Grenzwerte verhindern, dass ein oder zwei Ausreißerpods dem Knoten zu viel CPU-Leistung und Arbeitsspeichermenge entziehen. Dieses Szenario würde die Leistung des Knotens und anderer Pods verringern, die darauf ausgeführt werden.
    * Legen Sie keinen Podgrenzwert fest, der höher ist, als Ihre Knoten unterstützen können. Jeder AKS-Knoten reserviert eine bestimmte Menge von CPU-Leistung und Arbeitsspeichermenge für die Kubernetes-Kernkomponenten. Ihre Anwendung versucht möglicherweise, zu viele Ressourcen auf dem Knoten für die erfolgreiche Ausführung anderer Pods zu beanspruchen.
    * Überwachen Sie auch hier die Leistung Ihrer Anwendung zu verschiedenen Zeitpunkten während des Tags oder der Woche. Bestimmen Sie die Spitzennachfrage, und passen Sie die Podgrenzwerte den Ressourcen an, die erforderlich sind, um die Anforderungen der Anwendung zu erfüllen.

In Ihren Podspezifikationen sollten Sie diese Anforderungen und Grenzwerte definieren. Wenn Sie diese Werte nicht einbeziehen, versteht der Kubernetes-Planer nicht, welche Ressourcen erforderlich sind. Der Planer könnte den Pod auf einem Knoten ohne ausreichende Ressourcen für akzeptable Anwendungsleistung einplanen. Der Clusteradministrator kann *Ressourcenkontingente* auf einem Namespace festlegen, der von Ihnen das Festlegen von Ressourcenanforderungen und -grenzwerten anfordert. Weitere Informationen finden Sie unter [Ressourcenkontingente auf AKS-Clustern][resource-quotas].

Beim Definieren einer CPU-Anforderung oder eines Grenzwerts wird der Wert in CPU-Einheiten gemessen. *1.0* Die CPU entspricht einem zugrunde liegenden virtuellen CPU-Kern auf dem Knoten. Dieselbe Maßeinheit wird für GPUs verwendet. Sie können auch teilweise Anforderungen oder Grenzwerte definieren, in der Regel in Milli-CPU. Beispiel: *100m* ist *0,1* eines zugrunde liegenden virtuellen CPU-Kerns.

Im folgenden grundlegenden Beispiel für einen einzelnen NGINX-Pod fordert der Pod *100m* der CPU-Zeit und *128Mi* des Arbeitsspeichers an. Die Ressourcengrenzwerte für den Pod sind auf *250m* CPU und *256Mi* Arbeitsspeicher festgelegt:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
```

Weitere Informationen zu Ressourcenverwaltung und Zuweisungen finden Sie unter [Verwalten von Computeressourcen für Container][k8s-resource-limits].

## <a name="develop-and-debug-applications-against-an-aks-cluster"></a>Entwickeln und Debuggen von Anwendungen für einen AKS-Cluster

**Best Practice-Anleitung**: Entwicklungsteams sollten einen AKS-Cluster mithilfe von Dev Spaces bereitstellen und debuggen. Dieses Entwicklungsmodell stellt sicher, dass rollenbasierte Zugriffssteuerungen, Netzwerk- oder Speicheranforderungen implementiert werden, bevor die App in der Produktionsumgebung bereitgestellt wird.

Mit Azure Dev Spaces entwickeln, debuggen und testen Sie Anwendungen direkt mit einem AKS-Cluster. Entwickler in einem Team arbeiten zusammen, um während des gesamten Lebenszyklus der Anwendung zu erstellen und zu testen. Sie können weiterhin vorhandene Tools wie Visual Studio oder Visual Studio Code verwenden. Eine Erweiterung ist für Dev Spaces installiert, die eine Option zum Ausführen und Debuggen der Anwendung in einem AKS-Cluster bietet:

![Debuggen von Anwendungen in einem AKS-Cluster mit Dev Spaces](media/developer-best-practices-resource-management/dev-spaces-debug.png)

Dieser integrierte Entwicklungs- und Testprozess mit Dev Spaces verringert den Bedarf an lokalen Testumgebungen wie [minikube][minikube]. Stattdessen entwickeln und testen Sie anhand eines AKS-Clusters. Dieser Cluster kann gesichert und isoliert werden, wie im vorherigen Abschnitt zur Verwendung von Namespaces zum logischen Isolieren eines Clusters angemerkt. Wenn Ihre Apps für die Bereitstellung zur Produktion bereit sind, können Sie zuverlässig bereitstellen, da Ihre Entwicklung bereits mit einem echten AKS-Cluster ausgeführt wurde.

Azure Dev Spaces ist für die Verwendung mit auf Linux-Pods und -Knoten ausgeführten Anwendungen vorgesehen.

## <a name="use-the-visual-studio-code-extension-for-kubernetes"></a>Verwenden der Visual Studio Code-Erweiterung für Kubernetes

**Best Practice-Anleitung**: Installieren und verwenden Sie die VS Code-Erweiterung für Kubernetes beim Schreiben von YAML-Manifesten. Sie können die Erweiterung auch für integrierte Bereitstellungslösungen verwenden, was Besitzern von Anwendungen helfen kann, die nur selten mit dem AKS-Cluster interagieren.

Die [Visual Studio Code-Erweiterung für Kubernetes][vscode-kubernetes] hilft Ihnen, Anwendungen in AKS zu entwickeln und bereitzustellen. Die Erweiterung bietet Intellisense für Kubernetes-Ressourcen sowie Helm-Diagramme und Vorlagen. Sie können Kubernetes-Ressourcen auch in Visual Studio Code durchsuchen, bereitstellen und bearbeiten. Die Erweiterung bietet auch eine Intellisense-Überprüfung für Ressourcenanforderungen oder Grenzwerte, die in den Podspezifikationen festgelegt werden:

![VS Code-Erweiterung für Kubernetes-Warnung zu fehlenden Arbeitsspeicherlimits](media/developer-best-practices-resource-management/vs-code-kubernetes-extension.png)

## <a name="regularly-check-for-application-issues-with-kube-advisor"></a>Regelmäßiges Überprüfen auf Anwendungsprobleme mit dem Kube-Advisor

**Best Practices-Anleitung** – Führen Sie regelmäßig die neueste Version des Open-Source-Tools `kube-advisor` aus, um Probleme in Ihrem Cluster zu erkennen. Wenn Sie Ressourcenkontingente auf einen bestehenden AKS-Cluster anwenden, führen Sie zuerst `kube-advisor` aus, um Pods zu finden, die keine Ressourcenanforderungen und -grenzwerte definiert haben.

Das Tool [kube-advisor][kube-advisor] ist ein verwandtes Open-Source-Projekt für AKS, das einen Kubernetes-Cluster scannt und gefundene Probleme meldet. Eine nützliche Überprüfung ist die Identifizierung von Pods, bei denen keine Ressourcenanforderungen und -grenzwerte angegeben sind.

Das kube-advisor-Tool kann Berichte zur Ressourcenanforderung und zu Grenzwerten erstellen, die in PodSpecs für Windows- und Linux-Anwendungen fehlen, das kube-advisor-Tool selbst muss jedoch auf einem Linux-Pod geplant werden. Sie können einen Pod mit einem [Knoten-Selektor][ k8s-node-selector] in der Konfiguration des Pods so planen, dass er auf einem Knotenpool mit einem bestimmten Betriebssystem ausgeführt wird.

In einem AKS-Cluster, der mehrere Entwicklungsteams und Anwendungen hostet, kann es schwierig sein, Pods zu verfolgen, bei denen diese Ressourcenanforderungen und -grenzwerte nicht festgelegt wurden. Führen Sie als Best Practice regelmäßig `kube-advisor` in Ihren AKS-Clustern aus.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel zu bewährten Methoden konzentriert sich darauf, wie Sie Ihre Cluster und Workloads aus der Perspektive des Clusteroperators ausführen. Weitere Informationen zu bewährten Verwaltungsmethoden finden Sie unter [Bewährte Methoden der Clusterisolierung in Azure Kubernetes Service (AKS)][operator-best-practices-isolation].

Um einige dieser Best Practices zu implementieren, lesen Sie folgende Artikel:

* [Entwickeln mit Dev Spaces][dev-spaces]
* [Überprüfen auf Probleme mit dem Kube-Advisor][aks-kubeadvisor]

<!-- EXTERNAL LINKS -->
[k8s-resource-limits]: https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
[vscode-kubernetes]: https://github.com/Azure/vscode-kubernetes-tools
[kube-advisor]: https://github.com/Azure/kube-advisor
[minikube]: https://kubernetes.io/docs/setup/minikube/

<!-- INTERNAL LINKS -->
[aks-kubeadvisor]: kube-advisor-tool.md
[dev-spaces]: ../dev-spaces/get-started-netcore.md
[operator-best-practices-isolation]: operator-best-practices-cluster-isolation.md
[resource-quotas]: operator-best-practices-scheduler.md#enforce-resource-quotas
[k8s-node-selector]: concepts-clusters-workloads.md#node-selectors
