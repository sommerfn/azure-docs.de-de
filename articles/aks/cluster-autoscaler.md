---
title: Verwenden der Autoskalierung für Cluster in Azure Kubernetes Service (AKS)
description: In diesem Artikel erfahren Sie, wie Sie Ihren Cluster mithilfe der Autoskalierung automatisch zur Erfüllung der Anwendungsanforderungen in einem Azure Kubernetes Service-Cluster (AKS) skalieren.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/18/2019
ms.author: mlearned
ms.openlocfilehash: 9d7a404b767d3975cefd55e1db8487fbb45042e2
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174183"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Vorschau: Automatisches Skalieren eines Clusters zur Erfüllung von Anwendungsanforderungen in Azure Kubernetes Service (AKS)

Um die Anwendungsanforderungen in Azure Kubernetes Service (AKS) zu erfüllen, müssen Sie möglicherweise die Anzahl von Knoten anpassen, die Ihre Workloads ausführen. Die Komponente für die automatische Clusterskalierung kann auf Pods in Ihrem Cluster überprüfen, die aufgrund von Ressourceneinschränkungen nicht geplant werden können. Sobald Probleme erkannt werden, wird die Anzahl von Knoten in einem Knotenpool erhöht, um den Anwendungsanforderungen gerecht zu werden. Die Knoten werden außerdem regelmäßig auf einen Mangel an ausgeführten Pods überprüft, und bei Bedarf wird die Anzahl von Knoten verringert. Diese Fähigkeit zur automatischen Erhöhung oder Verringerung der Knotenanzahl in Ihrem AKS-Cluster ermöglicht es Ihnen, einen effizienten, kostengünstigen Cluster zu betreiben.

In diesem Artikel wird gezeigt, wie Sie die Autoskalierung für Cluster in einem AKS-Cluster aktivieren und verwalten. Die automatische Clusterskalierung sollte nur in der Vorschauversion auf AKS-Clustern getestet werden.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.65 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Um die automatische Clusterskalierung verwenden zu können, benötigen Sie die CLI-Erweiterung *aks-preview* Version 0.4.12 oder höher. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Erstellung und Verwaltung von AKS-Clustern, die die automatische Clusterskalierung verwenden:

* Das Add-On für das HTTP-Anwendungsrouting kann nicht verwendet werden.

## <a name="about-the-cluster-autoscaler"></a>Grundlegendes zur Autoskalierung für Cluster

Für eine Anpassung an sich ändernde Anwendungsanforderungen, z.B. zwischen Arbeitstag und Abend oder an einem Wochenende, benötigen Cluster oft eine Möglichkeit zur automatischen Skalierung. AKS-Cluster können auf zwei Arten skaliert werden:

* Die **Autoskalierung für Cluster** überprüft auf Pods, die aufgrund von Ressourceneinschränkungen nicht auf Knoten geplant werden können. Der Cluster erhöht in diesem Fall automatisch die Anzahl von Knoten.
* Bei der **horizontalen automatischen Podskalierung** überwacht der Metrikserver in einem Kubernetes-Cluster die Ressourcenanforderungen der Pods. Wenn eine Anwendung mehr Ressourcen benötigt, wird die Anzahl von Pods automatisch erhöht, um den Bedarf zu decken.

![Die Autoskalierung für Cluster und die horizontale automatische Podskalierung arbeiten häufig zusammen, um die Anwendungsanforderungen zu erfüllen.](media/autoscaler/cluster-autoscaler.png)

Sowohl über die horizontale automatische Podskalierung als auch über die Autoskalierung für Cluster kann bei Bedarf die Anzahl von Pods und Knoten verringert werden. Die Autoskalierung für Cluster verringert die Anzahl von Knoten, wenn für einen längeren Zeitraum Kapazität ungenutzt bleibt. Pods auf einem Knoten, der durch die Autoskalierung für Cluster entfernt werden soll, werden sicher an anderer Stelle im Cluster geplant. Die Autoskalierung für Cluster kann Pods möglicherweise nicht zentral herunterskalieren, wenn ein Verschieben nicht möglich ist. Dies kann in folgenden Situationen der Fall sein:

* Ein Pod wird direkt erstellt und nicht von einem Controllerobjekt wie z.B. einer Bereitstellungs- oder Replikatgruppe unterstützt.
* Ein Budget für die Unterbrechung von Pods (Pod Disruption Budget, PDB) ist zu restriktiv und lässt nicht zu, dass die Anzahl von Pods unter einen bestimmten Schwellenwert fällt.
* Ein Pod verwendet Knotenselektoren oder Anti-Affinität, die nicht berücksichtigt werden können, wenn der Pod auf einem anderen Knoten geplant wird.

Weitere Informationen dazu, warum ein zentrales Herunterskalieren über die Autoskalierung für Cluster möglicherweise nicht möglich ist, finden Sie unter [What types of pods can prevent the cluster autoscaler from removing a node?][autoscaler-scaledown] (Welche Podtypen können das Entfernen eines Knotens durch die Autoskalierung für Cluster verhindern?).

Die Autoskalierung für Cluster verwendet Startparameter, um z.B. Zeitintervalle zwischen Skalierungsereignissen und Ressourcenschwellenwerte zu steuern. Diese Parameter werden über die Azure-Plattform definiert und können aktuell nicht von Ihnen angepasst werden. Weitere Informationen zu den von der Autoskalierung für Cluster verwendeten Parametern finden Sie unter [What are the cluster autoscaler parameters?][autoscaler-parameters] (Welche Parameter verwendet die Autoskalierung für Cluster?).

Die automatische Clusterskalierung und die horizontale automatische Podskalierung können zusammenarbeiten und werden häufig beide in einem Cluster bereitgestellt. In Kombination konzentriert sich die horizontale automatische Podskalierung auf die Anzahl von Pods, die zum Erfüllen der Anwendungsanforderungen benötigt werden. Die Autoskalierung für Cluster konzentriert sich auf die Anzahl von Knoten, die zum Unterstützen der geplanten Pods erforderlich sind.

> [!NOTE]
> Bei Verwendung der Autoskalierung für Cluster ist die manuelle Skalierung deaktiviert. Überlassen Sie der Autoskalierung für Cluster das Bestimmen der erforderlichen Anzahl von Knoten. Wenn Sie Ihren Cluster manuell skalieren möchten, [deaktivieren Sie die Autoskalierung für Cluster](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Erstellen eines AKS-Clusters und Aktivieren der Autoskalierung für Cluster

Wenn Sie einen AKS-Cluster erstellen müssen, verwenden Sie den Befehl [az aks create][az-aks-create]. Um die automatische Clusterskalierung im Knotenpool für den Cluster zu aktivieren und zu konfigurieren, verwenden Sie den Parameter *--enable-cluster-autoscaler* und geben einen *--min-count*- und einen *--max-count*-Wert für die Knoten an.

> [!IMPORTANT]
> Bei der automatischen Clusterskalierungsfunktion handelt es sich um eine Kubernetes-Komponente. Obwohl der AKS-Cluster eine VM-Skalierungsgruppe für die Knoten verwendet, sollten Sie die Einstellungen für die automatische Skalierung für die Skalierungsgruppe im Azure-Portal oder über die Azure CLI nicht manuell aktivieren oder bearbeiten. Lassen Sie die automatische Skalierungsfunktion für den Kubernetes-Cluster die erforderlichen Skalierungseinstellungen verwalten. Weitere Informationen finden Sie unter [Kann ich die AKS-Ressourcen in der Knotenressourcengruppe ändern?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-node-resource-group)

Im folgenden Beispiel wird ein AKS-Cluster mit einem einzigen Knotenpool erstellt, der von einer VM-Skalierungsgruppe unterstützt wird. Außerdem wird die automatische Clusterskalierung im Knotenpool für den Cluster aktiviert und die Mindestanzahl von Knoten auf *1* sowie die maximale Knotenanzahl auf *3* festgelegt:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

> [!NOTE]
> Wenn Sie beim Ausführen von `az aks create` einen Wert für *--kubernetes-version* angeben, muss diese Version der mindestens erforderlichen Version entsprechen oder höher sein, wie im vorstehenden Abschnitt [Voraussetzungen](#before-you-begin) beschrieben.

Die Erstellung des Clusters und die Konfiguration der Einstellungen für die Autoskalierung für Cluster dauert einige Minuten.

## <a name="change-the-cluster-autoscaler-settings"></a>Ändern der Einstellungen zur Autoskalierung für Cluster

> [!IMPORTANT]
> Wenn Sie das Feature *Mehrere Agent-Pools* in Ihrem Abonnement aktiviert haben, fahren Sie mit dem Abschnitt [Automatische Skalierung mit mehreren Agent-Pools](#use-the-cluster-autoscaler-with-multiple-node-pools-enabled) fort. Für Cluster, für die mehrere Agent-Pools aktiviert sind, ist die Verwendung des `az aks nodepool`-Befehlssatzes anstelle von `az aks` zum Ändern von Knotenpool-spezifischen Eigenschaften erforderlich. In den folgenden Anweisungen wird davon ausgegangen, dass Sie nicht mehrere Knotenpools aktiviert haben. Um zu überprüfen, ob diese aktiviert sind, führen Sie `az feature  list -o table` aus, und suchen Sie nach `Microsoft.ContainerService/multiagentpoolpreview`.

Im vorherigen Schritt zum Erstellen eines AKS-Clusters oder Aktualisieren eines vorhandenen Knotenpools wurde die Mindestanzahl von Knoten zur automatischen Clusterskalierung auf *1* und die maximale Knotenanzahl auf *3* festgelegt. Wenn sich die Anforderungen Ihrer Anwendung ändern, müssen Sie möglicherweise die Knotenanzahl für die Autoskalierung für Cluster anpassen.

Verwenden Sie den Befehl [az aks update][az-aks-update], um die Knotenanzahl zu ändern.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Im oben stehenden Beispiel wird die automatische Clusterskalierung für den einzelnen Knotenpool in *myAKSCluster* auf mindestens *1* und höchstens *5* Knoten aktualisiert.

> [!NOTE]
> Während der Vorschau können Sie keine höhere Mindestanzahl von Knoten einstellen, als aktuell für den Knotenpool festgelegt ist. Wenn beispielsweise die Mindestanzahl von Knoten aktuell auf *1* festgelegt ist, können Sie die Mindestanzahl nicht auf *3* aktualisieren.

Überwachen Sie die Leistung Ihrer Anwendungen und Dienste, und passen Sie die Anzahl von Knoten zur Autoskalierung für Cluster an die erforderliche Leistung an.

## <a name="disable-the-cluster-autoscaler"></a>Deaktivieren der Autoskalierung für Cluster

Wenn Sie die automatische Clusterskalierung nicht mehr verwenden möchten, können Sie sie mit dem Befehl [az aks update][az-aks-update] unter Angabe des Parameters *--disable-cluster-autoscaler* deaktivieren. Beim Deaktivieren der Autoskalierung für Cluster werden keine Knoten entfernt.

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Nachdem Sie die automatische Clusterskalierung deaktiviert haben, können Sie den Cluster mit dem Befehl [az aks scale][az-aks-scale] manuell skalieren. Wenn Sie die horizontale automatische Podskalierung verwenden, wird dieses Feature nach dem Deaktivieren der Autoskalierung für Cluster weiterhin ausgeführt, aber Pods können möglicherweise nicht geplant werden, wenn alle Knotenressourcen verwendet werden.

## <a name="re-enable-a-disabled-cluster-autoscaler"></a>Erneutes Aktivieren der deaktivierten automatischen Clusterskalierung

Wenn Sie die automatische Clusterskalierung für einen vorhandenen Cluster wieder aktivieren möchten, können Sie sie mit dem Befehl [az aks update][az-aks-update] unter Angabe des Parameters *--enable-cluster-autoscaler* wieder aktivieren.

## <a name="use-the-cluster-autoscaler-with-multiple-node-pools-enabled"></a>Verwenden der automatischen Clusterskalierung mit mehreren aktivierten Knotenpools

Die automatische Clusterskalierung kann zusammen mit aktivierter [Previewfunktion für mehrere Knotenpools](use-multiple-node-pools.md) verwendet werden. In diesem Dokument erfahren Sie, wie Sie mehrere Knotenpools aktivieren und einem vorhandenen Cluster zusätzliche Knotenpools hinzufügen. Wenn Sie beide Features gleichzeitig verwenden, aktivieren Sie die automatische Clusterskalierung für jeden einzelnen Knotenpool im Cluster und können jeweils eindeutige Regeln für die automatische Skalierung übergeben.

Der folgende Befehl setzt voraus, dass Sie die [ersten Anweisungen](#create-an-aks-cluster-and-enable-the-cluster-autoscaler) in diesem Dokument befolgt haben und für einen vorhandenen Knotenpool die maximale Anzahl von *3* auf *5* aktualisieren möchten. Verwenden Sie den Befehl [az aks nodepool update][az-aks-nodepool-update], um die Einstellungen eines vorhandenen Knotenpools zu aktualisieren.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name multipoolcluster \
  --name mynodepool \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

Die automatische Clusterskalierung kann mit [az aks nodepool update][az-aks-nodepool-update] unter Angabe des Parameters `--disable-cluster-autoscaler` deaktiviert werden.

```azurecli-interactive
az aks nodepool update \
  --resource-group myResourceGroup \
  --cluster-name multipoolcluster \
  --name mynodepool \
  --disable-cluster-autoscaler
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie die Anzahl von AKS-Knoten automatisch skalieren können. Außerdem können Sie mit der horizontalen automatischen Podskalierung automatisch die Anzahl von Pods anpassen, auf denen Ihre Anwendung ausgeführt wird. Die Schritte zum Verwenden der horizontalen automatischen Podskalierung finden Sie unter [Skalieren von Anwendungen in Azure Kubernetes Service (AKS)][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[az-aks-nodepool-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview#enable-cluster-auto-scaler-for-a-node-pool
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
