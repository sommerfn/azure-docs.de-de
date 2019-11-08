---
title: Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)
description: Erfahren Sie, wie Sie ein Upgrade für einen AKS-Cluster (Azure Kubernetes Service) vornehmen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: 36bd05940644b62bc3e58b642bfe6c00238383fa
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472474"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Durchführen eines Upgrades für einen Azure Kubernetes Service-Cluster (AKS)

Im Verlauf des Lebenszyklus eines AKS-Clusters müssen Sie häufig ein Upgrade auf die neueste Kubernetes-Version vornehmen. Es ist wichtig, jeweils die aktuelle Kubernetes-Sicherheitsversion anzuwenden oder bei einem Upgrade die neuesten Features zu erhalten. In diesem Artikel wird veranschaulicht, wie Sie die Masterkomponenten oder einen einzelnen Standardknotenpool in einem AKS-Cluster aktualisieren.

Informationen zu AKS-Clustern, für die mehrere Knotenpools oder Windows Server-Knoten (in AKS jeweils in der Vorschauphase) verwendet werden, finden Sie unter [Durchführen eines Upgrades für einen Knotenpool in AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Voraussetzungen

Der Artikel setzt voraus, dass Sie mindestens Version 2.0.65 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

> [!WARNING]
> Durch ein AKS-Clusterupgrade werden Ihre Knoten als nicht planbar markiert und entleert (cordon/drain). Wenn Sie nur über ein geringes Computekontingent verfügen, kann das Upgrade möglicherweise nicht durchgeführt werden.  Weitere Informationen finden Sie unter [Anfordern einer Kontingenterhöhung](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request?branch=pr-en-us-83289).

## <a name="check-for-available-aks-cluster-upgrades"></a>Suchen nach verfügbaren AKS-Clusterupgrades

Überprüfen Sie mithilfe des Befehls [az aks get-upgrades][az-aks-get-upgrades], welche Kubernetes-Versionen für Ihren Cluster verfügbar sind. Im folgenden Beispiel werden für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* verfügbare Upgrades gesucht:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Beim Upgrade eines AKS-Clusters können Nebenversionen von Kubernetes nicht übersprungen werden. Beispielsweise sind Upgrades von *1.12.x* -> *1.13.x* oder *1.13.x* -> *1.14.x* zulässig, ein Upgrade von *1.12.x* -> *1.14.x* ist jedoch nicht möglich.
>
> Zum Durchführen eines Upgrades von *1.12.x* -> *1.14.x* müssen Sie zuerst ein Upgrade von *1.12.x* -> *1.13.x* und dann ein Upgrade von *1.13.x* -> *1.14.x* durchführen.

Die Ausgabe im folgenden Beispiel zeigt, dass der Cluster auf die Versionen *1.13.9* und *1.13.10* aktualisiert werden kann:

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Wenn kein Upgrade verfügbar ist, erhalten Sie Folgendes:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="upgrade-an-aks-cluster"></a>Aktualisieren eines AKS-Clusters

Wenn Sie die Liste der verfügbaren Versionen für Ihren AKS-Cluster angezeigt haben, verwenden Sie den Befehl [az aks upgrade][az-aks-upgrade], um den Cluster zu aktualisieren. Während des Upgradeprozesses fügt AKS dem Cluster einen neuen Knoten hinzu, auf dem die angegebene Kubernetes-Version ausgeführt wird. Danach sorgt AKS dafür, dass einer der alten Knoten [als unplanbar markiert und entleert wird][kubernetes-drain] (cordon/drain), um die Beeinträchtigung für ausgeführte Anwendungen möglichst gering zu halten. Wenn für den neuen Knoten die Ausführung von Anwendungspods bestätigt wird, wird der alte Knoten gelöscht. Dieser Prozess wird wiederholt, bis alle Knoten im Cluster aktualisiert wurden.

Im folgenden Beispiel wird ein Cluster auf Version *1.13.10* aktualisiert:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.13.10
```

Die Dauer des Clusterupgrades hängt von der Anzahl der vorhanden Knoten ab und kann einige Minuten in Anspruch nehmen. 

> [!NOTE]
> Für den Abschluss von Clusterupgrades besteht eine zulässige Gesamtzeit. Diese Zeit wird als das Produkt aus `10 minutes * total number of nodes in the cluster` berechnet. Beispielsweise müssen Upgradevorgänge in einem Cluster mit 20 Knoten in 200 Minuten erfolgreich ausgeführt werden. Andernfalls löst AKS einen Fehler aus, um einen nicht behebbaren Clusterstatus zu vermeiden. Um bei einem Upgradefehler eine Wiederherstellung auszuführen, wiederholen Sie den Upgradevorgang, nachdem das Timeout aufgetreten ist.

Überprüfen Sie nun mit dem Befehl [az aks show][az-aks-show], ob das Upgrade erfolgreich war:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Die Ausgabe im folgenden Beispiel zeigt, dass im Cluster nun Version *1.13.10* ausgeführt wird:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde gezeigt, wie Sie einen vorhandenen AKS-Cluster aktualisieren. In verschiedenen Tutorials können Sie mehr über die Bereitstellung und Verwaltung von AKS-Clustern erfahren.

> [!div class="nextstepaction"]
> [AKS-Tutorials][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
