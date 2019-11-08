---
title: Vom API-Server autorisierte IP-Adressbereiche in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Ihre Cluster durch Verwendung von IP-Adressbereichen für den Zugriff auf den API-Server in Azure Kubernetes Service (AKS) schützen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 8418499cc3e094162ac7483aaa6c71e74db95ae1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472962"
---
# <a name="secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Sicherer Zugriff auf den API-Server mit autorisierten IP-Adressbereichen in Azure Kubernetes Service (AKS)

In Kubernetes empfängt der API-Server Anforderungen zum Ausführen von Aktionen im Cluster wie z.B. das Erstellen von Ressourcen oder Skalieren der Anzahl der Knoten. Die API-Server ist die zentrale Möglichkeit zum Interagieren mit einem Cluster und seiner Verwaltung. Um die Clustersicherheit zu verbessern und Angriffe zu minimieren, sollte der Zugriff auf den API-Server nur über eine begrenzte Anzahl von IP-Adressbereichen möglich sein.

Dieser Artikel veranschaulicht die Verwendung der vom API-Server autorisierten IP-Adressbereiche. Diese werden zur Begrenzung der IP-Adressen und CIDRs verwendet, die Zugriff auf die Steuerungsebene erhalten.

> [!IMPORTANT]
> In neuen Clustern werden autorisierte IP-Adressbereiche für den API-Server nur auf dem *Standard* SKU Load Balancer unterstützt. Bestehende Cluster mit dem *Basic* SKU Load Balancer und den von API-Servern autorisierten IP-Adressbereichen bleiben weiter funktionsfähig. Diese bestehenden Cluster bleiben auch nach einem Upgrade funktionsfähig.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie mit Clustern arbeiten, die [kubenet][kubenet] verwenden.  Wenn Sie mit Clustern arbeiten, die auf [Azure Container Networking Interface (CNI)][cni-networking] basieren, verfügen Sie nicht über die erforderliche Routingtabelle, um den Zugriff schützen zu können.  Sie müssen die Routingtabelle manuell erstellen.  Weitere Informationen finden Sie unter [Verwalten von Routingtabellen](https://docs.microsoft.com/azure/virtual-network/manage-route-table).

Vom API-Server autorisierte IP-Adressbereiche funktionieren nur für neue AKS-Cluster, die Sie erstellen. In diesem Artikel wird beschrieben, wie Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle erstellen.

Azure CLI-Version 2.0.76 oder höher muss installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="limitations"></a>Einschränkungen

Wenn Sie vom API-Server autorisierte IP-Adressbereiche konfigurieren, gelten die folgenden Einschränkungen:

* Derzeit können Sie Azure Dev Spaces nicht verwenden, da die Kommunikation mit dem API-Server auch blockiert ist.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Übersicht über vom API-Server autorisierte IP-Adressbereiche

Die zugrunde liegenden Kubernetes-APIs werden auf dem Kubernetes-API-Server verfügbar gemacht. Diese Komponente ermöglicht die Interaktion für Verwaltungstools, z.B. `kubectl` oder das Kubernetes-Dashboard. AKS stellt einen Clustermaster mit Einzelmandant mit einem dedizierten API-Server bereit. Standardmäßig wird der API-Server einer öffentlichen IP-Adresse zugewiesen, und Sie sollten den Zugriff mithilfe der rollenbasierten Zugriffssteuerung (RBAC, Role-based Access Control) steuern.

Zum Absichern des Zugriffs auf AKS-Steuerungsebene/API-Server, die andernfalls öffentlich zugänglich sind, können Sie autorisierte IP-Adressbereiche aktivieren und verwenden. Diese autorisierten IP-Adressbereiche erlauben nur definierten IP-Adressbereichen die Kommunikation mit dem API-Server. Eine Anforderung einer IP-Adresse, die nicht Teil dieser autorisierten IP-Adressbereiche ist, an den API-Server wird blockiert. Sie sollten weiterhin RBAC verwenden, um anschließend Benutzer und die Aktionen, die sie anfordern, zu autorisieren.

Weitere Informationen zum API-Server und anderen Clusterkomponenten finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Vom API-Server autorisierte IP-Adressbereiche funktionieren nur für neue AKS-Cluster. Sie können autorisierte IP-Adressbereiche nicht im Rahmen der Clustererstellung aktivieren. Wenn Sie versuchen, autorisierte IP-Adressbereiche im Rahmen der Clustererstellung zu aktivieren, können die Clusterknoten nicht während der Bereitstellung auf den API-Server zugreifen, da die IP-Adresse für ausgehenden Datenverkehr zu diesem Zeitpunkt nicht definiert ist.

Erstellen Sie zuerst mit dem Befehl [az aks create][az-aks-create] einen Cluster. Im folgenden Beispiel wird ein Einzelknotencluster mit dem Namen *myAKSCluster* in der Ressourcengruppe mit dem Namen *myResourceGroup* erstellt.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="update-cluster-with-authorized-ip-ranges"></a>Aktualisieren eines Clusters mit autorisierten IP-Adressbereichen

Der Cluster verwendet standardmäßig den [Standard SKU Load Balancer][standard-sku-lb], den Sie zum Konfigurieren des ausgehenden Gateways verwenden können. Verwenden Sie [az network public-ip list][az-network-public-ip-list], und geben Sie die Ressourcengruppe des AKS-Clusters an, der in der Regel mit *MC_* beginnt. Dadurch wird die öffentliche IP-Adresse für den Cluster angezeigt, die Sie zulassen können. Legen Sie über den Befehl [az aks update][az-aks-update] den Wert des Parameters *--api-server-authorized-ip-ranges* fest, um die IP-Adresse des Clusters zuzulassen. Beispiel:

```azurecli-interactive
RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
SLB_PublicIP=$(az network public-ip list --resource-group $RG --query [].ipAddress -o tsv)
az aks update --api-server-authorized-ip-ranges $SLB_PublicIP --resource-group myResourceGroup --name myAKSCluster
```

Um autorisierte IP-Adressbereiche des API-Servers zu aktivieren, legen Sie über den Befehl [az aks update][az-aks-update] einen Wert für den Parameter *--api-server-authorized-ip-ranges* fest, um eine Liste der autorisierten IP-Adressbereiche bereitzustellen. Dabei handelt es sich in der Regel um IP-Adressbereiche, die von lokalen Netzwerken oder öffentlichen IP-Adressen verwendet werden. Wenn Sie einen CIDR-Bereich angeben, beginnen Sie mit der ersten IP-Adresse im Bereich. *137.117.106.90/29* ist z.B. ein gültiger Bereich, aber stellen Sie sicher, dass Sie die erste IP-Adresse im Bereich angeben, z.B. *137.117.106.88/29*.

Im folgenden Beispiel werden vom API-Server autorisierte IP-Adressbereiche für den Cluster namens *myAKSCluster* in der Ressourcengruppe *myResourceGroup* aktiviert. Die zu autorisierenden IP-Adressbereiche sind *172.0.0.0/16* (Pod/Nodes Address Range) und *168.10.0.0/18* (ServiceCidr):

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 172.0.0.0/16,168.10.0.0/18
```

> [!NOTE]
> Sie sollten diese Bereiche einer Zulassungsliste hinzufügen:
> - Öffentliche IP-Adresse der Firewall
> - Dienst-CIDR
> - Adressbereich für die Subnetze mit den Knoten und Pods
> - Alle Bereiche mit Netzwerken, von denen aus Sie den Cluster verwalten

## <a name="disable-authorized-ip-ranges"></a>Deaktivieren autorisierter IP-Adressbereiche

Um autorisierte IP-Adressbereiche zu deaktivieren, geben Sie mit [az aks update][az-aks-update] einen leeren Adressbereich an, um die autorisierten IP-Adressbereiche des API-Servers zu deaktivieren. Beispiel:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie vom API-Server autorisierte IP-Adressbereiche aktiviert. Dieser Ansatz ist ein Aspekt Ihrer sicheren Ausführung eines AKS-Clusters.

Weitere Informationen finden Sie unter [Sicherheitskonzepte für Anwendungen und Cluster in Azure Kubernetes Service (AKS)][concepts-security] und [Best Practices für Clustersicherheit und Upgrades in Azure Kubernetes Service (AKS)][operator-best-practices-cluster-security].

<!-- LINKS - external -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet

<!-- LINKS - internal -->
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[standard-sku-lb]: load-balancer-standard.md
