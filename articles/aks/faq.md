---
title: Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)
description: Finden Sie Antworten auf einige der häufig gestellten Fragen zu Azure Kubernetes Service (AKS).
services: container-service
author: mlearned
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/08/2019
ms.author: mlearned
ms.openlocfilehash: 7aff0fe47d1586b63157d5df7882fc338637f714
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381964"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Häufig gestellte Fragen zu Azure Kubernetes Service (AKS)

Dieser Artikel behandelt häufig gestellte Fragen zu Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-currently-provide-aks"></a>In welchen Azure-Regionen wird AKS derzeit zur Verfügung gestellt?

Eine vollständige Liste der verfügbaren Regionen finden Sie unter [„AKS-Regionen und Verfügbarkeit“][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>Unterstützt AKS die automatische Skalierung von Knoten?

Ja. Die Möglichkeit, Agentknoten in AKS horizontal zu skalieren, existiert derzeit in der Vorschauversion. Weitere Informationen finden Sie unter [„Automatisches Skalieren eines Clusters zur Erfüllung von Anwendungsanforderungen in AKS“][aks-cluster-autoscaler] for instructions. AKS autoscaling is based on the [Kubernetes autoscaler][auto-scaler]. Kann ich AKS in meinem vorhandenen virtuellen Netzwerk bereitstellen?

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Ja, Sie können einen AKS-Cluster in einem vorhandenen virtuellen Netzwerk mithilfe des [erweiterten Netzwerkfeatures][aks-advanced-networking] bereitstellen.

Kann ich einschränken, wer Zugriff auf den Kubernetes-API-Server hat?

## <a name="can-i-limit-who-has-access-to-the-kubernetes-api-server"></a>Ja. Sie können den Zugriff auf den Kubernetes-API-Server mit [„Vom API-Server autorisierte IP-Bereiche“][api-server-authorized-ip-ranges] einschränken, wovon es derzeit eine Vorschauversion gibt.

Kann ich den Kubernetes-API-Server nur innerhalb meines virtuellen Netzwerks zugänglich machen?

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Noch nicht, aber dies ist schon in der Planung.

Der Fortschritt kann im [AKS GitHub-Repository][private-clusters-github-issue] nachverfolgt werden. Kann ich verschiedene VM-Größen in einem einzigen Cluster haben?

## <a name="can-i-have-different-vm-sizes-in-a-single-cluster"></a>Ja. Sie können verschiedene Größen für virtuelle Computer in Ihrem AKS-Cluster einsetzen, indem Sie [mehrere Knotenpools][multi-node-pools] einsetzen, wovon es derzeit eine Vorschauversion gibt.

Werden Sicherheitsupdates auf AKS-Agent-Knoten angewendet?

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Azure wendet automatisch Sicherheitspatches auf die Linux-Knoten in Ihrem Cluster an, und zwar gemäß einem nächtlichen Zeitplan.

Sie sind jedoch dafür verantwortlich, dass diese Linux-Knoten bei Bedarf neu gestartet werden. Sie haben mehrere Möglichkeiten für Neustarts von Knoten: Manuell über das Azure-Portal oder die Azure-CLI.

- Durch ein Upgrade des AKS-Clusters.
- Der Cluster führt ein Upgrade für [Absperren und Ausgleichen von Knoten][cordon-drain] durch automatically and then bring a new node online with the latest Ubuntu image and a new patch version or a minor Kubernetes version. For more information, see [Upgrade an AKS cluster][aks-upgrade]. Mit [Kured](https://github.com/weaveworks/kured), einem Open Source-Neustartdaemon für Kubernetes. Kured wird als [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) ausgeführt und überwacht jeden Knoten auf das Vorhandensein einer Datei, die angibt, dass ein Neustart erforderlich ist.
- Neustarts des Betriebssystems werden clusterübergreifend verwaltet, wobei derselbe [Vorgang des Absperrens und Ausgleichens][cordon-drain] wie bei einem Clusterupgrade angewandt wird. Weitere Informationen zur Verwendung von kured finden Sie unter [Apply security and kernel updates to nodes in Azure Kubernetes Service (AKS)][node-updates-kured] (Anwenden von Sicherheits- und Kernelupdates auf Knoten in Azure Kubernetes Service (AKS)). Windows Server-Knoten

Für Windows Server-Knoten (derzeit in der Vorschauversion in AKS) werden die neuesten Updates von Windows Update nicht automatisch ausgeführt und angewendet.

### <a name="windows-server-nodes"></a>Sie sollten in regelmäßigen Abständen ein Upgrade für die Windows Server-Knotenpools in Ihrem AKS-Cluster durchführen, passend zum Windows Update-Freigabezyklus und Ihrem eigenen Validierungsprozess.

Während dieses Upgrades werden Knoten erstellt, die das neueste Windows Server-Image und die neuesten Windows Server-Patches ausführen und die älteren Knoten entfernen. Weitere Informationen zu diesem Prozess finden Sie unter [Upgrade a node pool in AKS (Durchführen eines Upgrades für einen Knotenpool in AKS)][nodepool-upgrade]. Warum werden zwei Ressourcengruppen mit AKS erstellt? Jede AKS-Bereitstellung umfasst zwei Ressourcengruppen:

## <a name="why-are-two-resource-groups-created-with-aks"></a>Die erste Ressourcengruppe wird von Ihnen erstellt.

Diese Gruppe enthält nur die Kubernetes-Dienstressource.

1. Der AKS-Ressourcenanbieter erstellt während der Bereitstellung automatisch die zweite Ressourcengruppe. *MC_myResourceGroup_myAKSCluster_eastus* ist ein Beispiel für die zweite Ressourcengruppe. Informationen dazu, wie Sie den Namen dieser zweiten Ressourcengruppe angeben, finden Sie im nächsten Abschnitt. Die zweite Ressourcengruppe, als *Knotenressourcengruppe* bezeichnet, enthält alle Infrastrukturressourcen für den Cluster. Diese Ressourcen umfassen die virtuellen Computer des Kubernetes-Knotens, virtuelle Netzwerke und Speicher.
1. Standardmäßig lautet der Name der Knotenressourcengruppe z. B. *MC_myResourceGroup_myAKSCluster_eastus*. AKS löscht automatisch die Knotenressource, wenn der Cluster gelöscht wird. Sie sollte daher nur für Ressourcen verwendet werden, die den gleichen Lebenszyklus wie der Cluster haben. Kann ich einen eigenen Namen für die AKS-Knotenressourcengruppe angeben? Ja.

## <a name="can-i-provide-my-own-name-for-the-aks-node-resource-group"></a>In AKS lautet der Standardname der Ressourcengruppe *MC_clustername_resourcegroupname_location*, Sie können jedoch auch einen eigenen Namen angeben.

Installieren Sie die [aks-preview][aks-preview-cli]-Erweiterungsversion *0.3.2* oder höher für die Azure CLI, um Ihren eigenen Ressourcengruppennamen anzugeben. Verwenden Sie bei der Erstellung eines AKS-Clusters mit dem Befehl [az aks create][az-aks-create] den Parameter *--node-resource-group*, und geben Sie einen Namen für die Ressourcengruppe an.

Wenn Sie [eine Azure Resource Manager-Vorlage verwenden][aks-rm-template], um einen AKS-Cluster bereitzustellen, können Sie die *nodeResourceGroup*-Eigenschaft verwenden, um den Namen der Ressourcengruppe zu definieren. Die sekundäre Ressourcengruppe wird automatisch vom Azure-Ressourcenanbieter in Ihrem eigenen Abonnement erstellt. Sie können nur einen benutzerdefinierten Namen für die Ressourcengruppe angeben, wenn Sie den Cluster erstellen.

* Beachten Sie beim Verwenden der Knotenressourcengruppe, dass Folgendes nicht möglich ist:
* Angeben einer vorhandenen Ressourcengruppe als Knotenressourcengruppe

Angeben eines anderen Abonnements für die Knotenressourcengruppe

* Ändern des Namens der Knotenressourcengruppe, nachdem der Cluster erstellt wurde
* Angeben von Namen für die verwalteten Ressourcen innerhalb der Knotenressourcengruppe
* Ändern oder Löschen von Tags verwalteter Ressourcen innerhalb der Knotenressourcengruppe
* (Weitere Informationen finden Sie im nächsten Abschnitt.)
* Kann ich Tags und andere Eigenschaften der AKS-Ressourcen in der Knotenressourcengruppe ändern? Wenn Sie die in Azure erstellten Tags und andere Ressourceneigenschaften in der Knotenressourcengruppe ändern oder löschen, kann dies zu unerwarteten Ergebnissen wie Skalierungs- und Aktualisierungsfehlern führen.

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group"></a>In AKS können Sie benutzerdefinierte Tags erstellen und ändern.

Sie können benutzerdefinierte Tags erstellen oder ändern, um beispielsweise eine Geschäftseinheit oder eine Kostenstelle zuzuweisen. Durch Ändern der Ressourcen unter der Knotenressourcengruppe im AKS-Cluster wird das Servicelevelziel (SLO) unterbrochen. Weitere Informationen finden Sie unter [Bietet AKS eine Vereinbarung zum Servicelevel?](#does-aks-offer-a-service-level-agreement) Welche Kubernetes-Zugangssteuerungen werden von AKS unterstützt? Können Zulassungscontroller hinzugefügt oder entfernt werden?

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>AKS unterstützt die folgenden [Zugangssteuerungen][admission-controllers]: *NamespaceLifecycle*

*LimitRanger*

- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*
- Derzeit können Sie die Liste der Zugriffssteuerungen in AKS nicht ändern.
- Ist Azure Key Vault in AKS integriert?

AKS ist derzeit nicht nativ mit Azure Key Vault integriert.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Das Projekt [Azure Key Vault FlexVolume für Kubernetes][keyvault-flexvolume] ermöglicht jedoch die direkte Integration von Kubernetes-Pods in Key Vault-Geheimnisse.

Kann ich Windows Server-Container unter AKS ausführen? Ja, Windows Server-Container sind in der Vorschauversion verfügbar.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Erstellen Sie einen Knotenpool, der Windows Server als Gastbetriebssystem ausführt, um Windows Server-Container in AKS auszuführen.

Windows Server-Container können nur Windows Server 2019 verwenden. Informationen zu den ersten Schritten finden Sie unter [Erstellen eines AKS-Clusters mit einem Windows Server-Knotenpool][aks-windows-cli]. Die Windows Server-Unterstützung für Knotenpools umfasst einige Einschränkungen, die Teil des Upstreamprojekts „Windows Server in Kubernetes“ sind. Weitere Informationen zu diesen Einschränkungen finden Sie unter [Windows Server containers in AKS limitations (Einschränkungen für Windows Server-Container in AKS)][aks-windows-limitations].

Bietet AKS eine Vereinbarung zum Servicelevel? In einer Vereinbarung zum Servicelevel (Service Level Agreement, SLA) sichert der Anbieter zu, dem Kunden die Kosten des Diensts zu erstatten, falls das veröffentliche Servicelevel nicht erfüllt wird.

## <a name="does-aks-offer-a-service-level-agreement"></a>Da AKS kostenlos ist, können keine Kosten erstattet werden. Daher verfügt AKS über keine formale SLA.

In AKS wird jedoch angestrebt, für den Kubernetes-API-Server eine Verfügbarkeit von mindestens 99,5 % zu bieten. Warum kann ich für maxPods keinen Wert unter 30 festlegen? In AKS können Sie den Wert für `maxPods` bei der Erstellung des Clusters über die Azure-Befehlszeilenschnittstelle und über Azure Resource Manager-Vorlagen festlegen.

## <a name="why-cant-i-set-maxpods-below-30"></a>Für Kubenet und Azure CNI ist jedoch ein *Mindestwert* erforderlich (wird zum Erstellungszeitpunkt überprüft):

Netzwerk Minimum

| Maximum | Azure CNI | 30 |
| -- | :--: | :--: |
| 250 | Kubenet | 30 |
| 110 | Da es sich bei AKS um einen verwalteten Dienst handelt, werden Add-Ons und Pods als Teil des Clusters bereitgestellt und verwaltet. | In der Vergangenheit konnten Benutzer einen `maxPods`-Wert definieren, der kleiner war als der Wert, der für die Ausführung der verwalteten Pods erforderlich ist (z. B. 30). |

AKS berechnet jetzt die Mindestanzahl der Pods mit folgender Formel: ((maxPods oder (maxPods * VM_Anzahl)) > Mindestanzahl verwalteter Add-On-Pods. Benutzer können die minimale `maxPods`-Validierung nicht außer Kraft setzen. Kann ich Rabatte für Azure-Reservierungen auf meine AKS-Agent-Knoten anwenden?

AKS-Agent-Knoten werden als standardmäßige Azure-VMs abgerechnet. Wenn Sie also [Azure-Reservierungen][reservation-discounts] für die von Ihnen in AKS verwendete VM-Größe erworben haben, werden diese Rabatte automatisch angewendet.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>Kann ich meinen Cluster zwischen Azure-Mandanten verschieben/migrieren?

Der Befehl `az aks update-credentials` kann verwendet werden, um einen AKS-Cluster zwischen Azure-Mandanten zu verschieben.

## <a name="can-i-movemigrate-my-cluster-between-azure-tenants"></a>Folgen Sie den Anweisungen unter [Aktualisieren oder Erstellen eines Dienstprinzipals](https://docs.microsoft.com/azure/aks/update-credentials), und [aktualisieren Sie den AKS-Cluster mit neuen Anmeldeinformationen](https://docs.microsoft.com/azure/aks/update-credentials#update-aks-cluster-with-new-credentials).

Kann ich meinen Cluster zwischen Abonnements verschieben/migrieren? Das Verschieben von Clustern zwischen Abonnements wird derzeit nicht unterstützt.

## <a name="can-i-movemigrate-my-cluster-between-subscriptions"></a>Kann ich meine AKS-Cluster aus dem aktuellen Azure-Abonnement in ein anderes verschieben?

Das Verschieben Ihres AKS-Clusters und der zugehörigen Ressourcen zwischen Azure-Abonnements wird nicht unterstützt.

## <a name="can-i-move-my-aks-clusters-from-the-current-azure-subscription-to-another"></a>Warum dauert das Löschen meines Clusters so lange? 

Die meisten Cluster werden auf Benutzeranforderung gelöscht. In einigen Fällen kann der Vorgang zusätzliche Zeit in Anspruch nehmen oder fehlschlagen, insbesondere bei Kunden, die ihre eigene Ressourcengruppe verwenden, oder beim Löschen von RG-übergreifenden Aufgaben.

## <a name="why-is-my-cluster-delete-taking-so-long"></a>Wenn Sie ein Problem mit Löschvorgängen haben, vergewissern Sie sich, dass keine Sperren für die Ressourcengruppen vorliegen, dass alle Verknüpfungen mit Ressourcen außerhalb der Ressourcengruppe aufgehoben wurden usw. 

Wenn Pods/Bereitstellungen den Zustand „NodeLost“ oder „Unbekannt“ aufweisen, kann ich meinen Cluster trotzdem aktualisieren? Dies ist möglich, aber AKS empfiehlt dies nicht.

## <a name="if-i-have-pod--deployments-in-state-nodelost-or-unknown-can-i-still-upgrade-my-cluster"></a>Upgrades sollten idealerweise ausgeführt werden, wenn der Zustand des Clusters bekannt und fehlerfrei ist.

Kann ich ein Upgrade ausführen, wenn ein Cluster mit einem oder mehreren Knoten einen fehlerhaften Zustand aufweisen oder heruntergefahren wurden? Nein. Löschen/Entfernen Sie alle Knoten, die den Status „Fehler“ aufweisen oder anderweitig aus dem Cluster entfernt wurden, bevor Sie das Upgrade ausführen.

## <a name="if-i-have-a-cluster-with-one-or-more-nodes-in-an-unhealthy-state-or-shut-down-can-i-perform-an-upgrade"></a>Ich habe eine Clusterlöschung ausgeführt, aber der Fehler `[Errno 11001] getaddrinfo failed` wird angezeigt.

Dies wird in der Regel dadurch verursacht, dass eine oder mehrere Netzwerksicherheitsgruppen (NSGs) noch von Benutzern verwendet werden und dem Cluster zugeordnet sind.

## <a name="i-ran-a-cluster-delete-but-see-the-error-errno-11001-getaddrinfo-failed"></a>Entfernen Sie diese, und versuchen Sie erneut, den Löschvorgang auszuführen. 

Ich habe ein Upgrade ausgeführt, aber jetzt befinden sich die Pods in Absturzschleifen, und Bereitschaftstests schlagen fehl.  Vergewissern Sie sich, dass der Dienstprinzipal nicht abgelaufen ist.

## <a name="i-ran-an-upgrade-but-now-my-pods-are-in-crash-loops-and-readiness-probes-fail"></a>Informationen finden Sie unter: [AKS-Dienstprinzipal](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) und [Aktualisieren der AKS-Anmeldeinformationen](https://docs.microsoft.com/azure/aks/update-credentials).

Mein Cluster hat funktioniert, kann aber plötzlich keine LoadBalancer bereitstellen, keine PVCs mounten usw.  Vergewissern Sie sich, dass der Dienstprinzipal nicht abgelaufen ist.

## <a name="my-cluster-was-working-but-suddenly-can-not-provision-loadbalancers-mount-pvcs-etc"></a>Informationen finden Sie unter: [AKS-Dienstprinzipal](https://docs.microsoft.com/azure/aks/kubernetes-service-principal) und [Aktualisieren der AKS-Anmeldeinformationen](https://docs.microsoft.com/azure/aks/update-credentials). 

Kann ich die VM-Skalierungsgruppen-APIs für eine manuelle Skalierung verwenden?  Nein, Skalierungsvorgänge mithilfe der VM-Skalierungsgruppen-APIs werden nicht unterstützt.

## <a name="can-i-use-the-virtual-machine-scale-set-apis-to-scale-manually"></a>Verwenden Sie die AKS-APIs (`az aks scale`).

Kann ich VM-Skalierungsgruppen verwenden, um manuell auf 0 Knoten zu skalieren? Nein, Skalierungsvorgänge mithilfe der VM-Skalierungsgruppen-APIs werden nicht unterstützt.

## <a name="can-i-use-virtual-machine-scale-sets-to-manually-scale-to-0-nodes"></a>Kann ich alle meine VMs beenden oder deren Zuordnung aufheben?

AKS verfügt zwar über Resilienzmechanismen, um eine solche Konfiguration zu überstehen und eine Wiederherstellung daraus vorzunehmen, es ist aber keine empfohlene Konfiguration.

## <a name="can-i-stop-or-de-allocate-all-my-vms"></a>Kann ich benutzerdefinierte VM-Erweiterungen verwenden?

Nein. AKS ist ein verwalteter Dienst, und die Bearbeitung der IaaS-Ressourcen wird nicht unterstützt.

## <a name="can-i-use-custom-vm-extensions"></a>Nutzen Sie zum Installieren von benutzerdefinierten Komponenten usw.

die Kubernetes-APIs und -Mechanismen. Nutzen Sie beispielsweise DaemonSets, um erforderliche Komponenten zu installieren. please leverage the kubernetes APIs and mechanisms. For example, leverage DaemonSets to install required components.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /azure/templates/microsoft.containerservice/2019-06-01/managedclusters
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md
[api-server-authorized-ip-ranges]: ./api-server-authorized-ip-ranges.md
[multi-node-pools]: ./use-multiple-node-pools.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
[private-clusters-github-issue]: https://github.com/Azure/AKS/issues/948
