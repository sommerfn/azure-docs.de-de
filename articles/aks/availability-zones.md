---
title: Verwendung von Verfügbarkeitszonen in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie in Azure Kubernetes Service (AKS) einen Cluster erstellen, der Knoten über Verfügbarkeitszonen verteilt.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 06/24/2019
ms.author: iainfou
ms.openlocfilehash: 0f99386aa9eeb75a990507e383c32412fb39eceb
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840458"
---
# <a name="preview---create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Vorschau – Erstellen eines Azure Kubernetes Service (AKS)-Clusters, der Verfügbarkeitszonen verwendet

Ein Azure Kubernetes Service (AKS)-Cluster verteilt Ressourcen wie Knoten und Speicher auf logische Abschnitte der zugrunde liegenden Azure-Computerinfrastruktur. Dieses Bereitstellungsmodell stellt sicher, dass die Knoten über getrennte Update- und Fehlerdomänen in einem einzigen Azure-Datencenter laufen. AKS-Cluster, die mit diesem Standardverhalten bereitgestellt werden, bieten ein hohes Maß an Verfügbarkeit zum Schutz vor Hardwareausfällen oder geplanten Wartungsereignissen.

Um eine höhere Verfügbarkeit Ihrer Anwendungen zu gewährleisten, können AKS-Cluster über Verfügbarkeitszonen verteilt werden. Diese Zonen sind physisch getrennte Datencenter innerhalb einer bestimmten Region. Wenn die Clusterkomponenten auf mehrere Zonen verteilt sind, kann Ihr AKS-Cluster einen Fehler in einer dieser Zonen tolerieren. Ihre Anwendungen und Verwaltungsabläufe sind auch bei Problemen in einem ganzen Datencenter weiterhin verfügbar.

Dieser Artikel zeigt Ihnen, wie Sie einen AKS-Cluster erstellen und die Knotenkomponenten auf Verfügbarkeitszonen verteilen. Diese Funktion steht derzeit als Vorschau zur Verfügung.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Sie werden zum Sammeln von Feedback und Fehlern mithilfe unserer Community bereitgestellt. In der Vorschauversion sind diese Features nicht für den Einsatz in der Produktion vorgesehen. Features in der öffentlichen Vorschau unterliegen dem Prinzip des „bestmöglichen Supports“. Unterstützung durch die Teams für den technischen AKS-Support steht nur während der Geschäftszeiten in der Zeitzone „Pacific Standard Time“ (PST) zur Verfügung. Weitere Informationen hierzu finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.66 oder höher muss installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter [„Installieren der Azure-Befehlszeilenschnittstelle“][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Um AKS-Cluster zu erstellen, die Verfügbarkeitszonen verwenden, benötigen Sie die *aks-preview* CLI-Erweiterung Version 0.4.1 oder höher. Installieren Sie die *aks-preview* Azure CLI Erweiterung mit dem Befehl [az extension add][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flags-for-your-subscription"></a>Registrieren von Featureflags für Ihr Abonnement

Aktivieren Sie zunächst einige Featureflags in Ihrem Abonnement, um einen AKS-Cluster mit Verfügbarkeitszonen zu erstellen. Poolcluster verwenden eine VM-Skalierungsgruppe, um die Bereitstellung und Konfiguration der Kubernetes-Knoten zu verwalten. Die *Standard*-SKU des Azure Load Balancer ist auch erforderlich, um den Netzwerkkomponenten eine hohe Ausfallsicherheit zu bieten, wenn der Datenverkehr in Ihren Cluster geleitet wird. Registrieren Sie die Featureflags *„AvailabilityZonePreview“* , *„AKSAzureStandardLoadBalancer“* und *„VMSSPreview“* mithilfe des Befehls [„az feature register“][az-feature-register], wie im folgenden Beispiel gezeigt:

> [!CAUTION]
> Wenn Sie eine Funktion in einem Abonnement registrieren, können Sie die Registrierung dieser Funktion derzeit nicht aufheben. Nachdem Sie einige Previewfunktionen aktiviert haben, können Standardwerte für alle AKS-Cluster verwendet werden, die dann im Abonnement erstellt werden. Aktivieren Sie keine Previewfunktionen in Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Previewfunktionen zu testen und Feedback zu erhalten.

```azurecli-interactive
az feature register --name AvailabilityZonePreview --namespace Microsoft.ContainerService
az feature register --name AKSAzureStandardLoadBalancer --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mit dem Befehl [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AvailabilityZonePreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Wenn Sie bereit sind, aktualisieren Sie die Registrierung des *Microsoft.ContainerService* Ressourcenanbieters mit dem Befehl[ az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations-and-region-availability"></a>Einschränkungen und regionale Verfügbarkeit

AKS-Cluster können derzeit über Verfügbarkeitszonen in den folgenden Regionen erstellt werden:

* USA (Ost) 2
* Nordeuropa
* Asien, Südosten
* Europa, Westen
* USA, Westen 2

Die folgenden Einschränkungen gelten, wenn Sie einen AKS-Cluster mit Verfügbarkeitszonen erstellen:

* Verfügbarkeitszonen können Sie nur beim Anlegen des Clusters aktivieren.
* Die Einstellungen der Verfügbarkeitszone können nach dem Erstellen des Clusters nicht mehr aktualisiert werden. Außerdem können Sie einen bestehenden, nicht verfügbaren Zonencluster nicht aktualisieren, wenn Sie Verfügbarkeitszonen verwenden wollen.
* Sie können Verfügbarkeitszonen für einen AKS-Cluster auch nicht mehr deaktivieren, nachdem er erstellt wurde.
* Die gewählte Knotengröße (VM-SKU) muss in allen Verfügbarkeitszonen verfügbar sein.
* Cluster mit aktivierten Verfügbarkeitszonen erfordern den Einsatz von Azure Load Balancern Standard zur Verteilung über die Zonen.
* Sie müssen Kubernetes Version 1.13.5 oder höher verwenden, um Load Balancer Standard einsetzen zu können.

AKS-Cluster, die Verfügbarkeitszonen verwenden, müssen die Azure *Standard* Load Balancer-SKU verwenden. Die standardmäßige *Basis*-SKU des Azure Load Balancer unterstützt keine Verteilung über Verfügbarkeitszonen. Weitere Informationen und die Einschränkungen des Standardlastausgleichs finden Sie unter [„Einschränkungen der Vorschauversion von Azure Load Balancer Standard-SKU“][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Einschränkungen für Azure-Datenträger

Volumes, die von Azure verwaltete Datenträger verwenden, sind derzeit keine zonalen Ressourcen. Pods, die in einer anderen Zone als ihrer ursprünglichen Zone neu geplant wurden, können ihre vorherigen Datenträger nicht wieder anhängen. Es wird empfohlen, zustandslose Workloads auszuführen, die keinen persistenten Speicher erfordern, der auf zonale Probleme stoßen kann.

Wenn Sie zustandsbehaftete Workloads ausführen müssen, verwenden Sie Taints und Toleranzen in Ihren Pod-Spezifikationen, um den Kubernetes-Scheduler anzuweisen, Pods in der gleichen Zone wie Ihre Datenträger zu erstellen. Alternativ können Sie auch netzwerkbasierten Speicher wie Azure Files verwenden, die sich an Pods anhängen können, während sie zwischen den Zonen geplant sind.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>Übersicht über Verfügbarkeitszonen für AKS-Cluster

Verfügbarkeitszonen sind ein Hochverfügbarkeitsangebot, das Anwendungen und Daten vor Ausfällen von Rechenzentren schützt. Zonen sind eindeutige physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Zur Gewährleistung der Resilienz sind in allen aktivierten Regionen mindestens drei separate Zonen vorhanden. Die physische Trennung von Verfügbarkeitszonen innerhalb einer Region schützt Anwendungen und Daten vor Ausfällen von Rechenzentren. Zonenredundante Dienste replizieren Ihre Anwendungen und Daten zum Schutz vor einzelnen Fehlerquellen über Verfügbarkeitszonen hinweg.

Weitere Informationen finden Sie unter [„Was sind Verfügbarkeitszonen in Azure?“][az-overview].

AKS-Cluster, die über Verfügbarkeitszonen bereitgestellt werden, können Knoten über mehrere Zonen innerhalb einer einzelnen Region verteilen. So kann beispielsweise ein Cluster in der Region  *„USA, Osten 2“*   Knoten in allen drei Verfügbarkeitszonen in *„USA, Osten 2“* anlegen. Diese Verteilung der AKS-Clusterressourcen verbessert die Clusterverfügbarkeit, da sie gegen den Ausfall einer bestimmten Zone resistent sind.

![AKS-Knotenverteilung über Verfügbarkeitszonen hinweg](media/availability-zones/aks-availability-zones.png)

Bei einem Zonenausfall können die Knoten manuell oder über die automatische Clusterskalierung neu abgeglichen werden. Wenn eine einzelne Zone nicht verfügbar ist, werden Ihre Anwendungen weiterhin ausgeführt.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Erstellen eines AKS-Clusters über Verfügbarkeitszonen hinweg

Wenn Sie einen Cluster mit dem Befehl [„az aks create“][az-aks-create] erstellen, definiert der Parameter `--node-zones`, in welche Zonen Agentenknoten eingesetzt werden. Die AKS-Steuerungsebenenkomponenten für Ihren Cluster sind auch zonenübergreifend in der höchstmöglichen Konfiguration verteilt, wenn Sie einen Cluster erstellen, der den Parameter `--node-zones` angibt.

Wenn Sie beim Erstellen eines AKS-Clusters keine Zonen für den Standard-Agentpool definieren, verwenden die AKS-Steuerebenenkomponenten für Ihren Cluster keine Verfügbarkeitszonen. Sie können zusätzliche Knotenpools (derzeit in der Vorschauversion in AKS) mit dem Befehl [„az aks nodepool add“][az-aks-nodepool-add] hinzufügen und `--node-zones` für diese neuen Agentenknoten angeben. Jedoch bleiben die Komponenten der Steuerungsebene ohne Kenntnis der Verfügbarkeitszone. Sie können die Zonenwahrnehmung für einen Knotenpool oder die AKS-Steuerungsebenenkomponenten nach dem Einsatz nicht mehr ändern.

Das folgende Beispiel erstellt einen AKS-Cluster namens *„myAKSCluster“* in der Ressourcengruppe namens *„myResourceGroup“* . Es werden insgesamt *3* Knoten angelegt – ein Agent in Zone *1*, einer in *2* und dann ein weiterer in *3*. Die AKS-Steuerungsebenenkomponenten sind auch in der höchsten verfügbaren Konfiguration über Zonen verteilt, da sie als Teil des Cluster-Erstellungsprozesses definiert sind.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version 1.13.5 \
    --generate-ssh-keys \
    --enable-vmss \
    --load-balancer-sku standard \
    --node-count 3 \
    --node-zones 1 2 3
```

Die Erstellung des AKS-Clusters dauert einige Minuten.

## <a name="verify-node-distribution-across-zones"></a>Überprüfen der Verteilung der Knoten auf die Zonen

Wenn der Cluster bereit ist, listen Sie die Agentenknoten in der Skalierungsgruppe auf, um zu sehen, in welcher Verfügbarkeitszone sie bereitgestellt werden.

Rufen Sie zuerst die Anmeldeinformationen für den AKS-Cluster ab, indem Sie den Befehl [„az aks get-credentials“][az-aks-get-credentials] verwenden:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Verwenden Sie anschließend den Befehl [„kubectl describe“][kubectl-describe], um die Knoten im Cluster aufzulisten. Filtern Sie den Wert der *failure-domain.beta.kubernetes.io/zone*, wie im folgenden Beispiel gezeigt:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

Das folgende Beispiel zeigt die drei Knoten, die auf den angegebenen Bereich und die Verfügbarkeitszonen verteilt sind, wie z. B. *eastus2-1* für die erste Verfügbarkeitszone und *eastus2-2* für die zweite Verfügbarkeitszone:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

Wenn Sie zusätzliche Knoten zu einem Agentenpool hinzufügen, verteilt die Azure-Plattform die zugrunde liegenden VMs automatisch auf die angegebenen Verfügbarkeitszonen.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie einen AKS-Cluster erstellen, der Verfügbarkeitszonen verwendet. Weitere Informationen zu hochverfügbaren Clustern finden Sie unter [„Best Practices für Geschäftskontinuität und Notfallwiederherstellung in AKS“][best-practices-bc-dr].

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
