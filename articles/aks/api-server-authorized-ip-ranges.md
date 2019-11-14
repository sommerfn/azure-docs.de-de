---
title: Vom API-Server autorisierte IP-Adressbereiche in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Ihre Cluster durch Verwendung von IP-Adressbereichen für den Zugriff auf den API-Server in Azure Kubernetes Service (AKS) schützen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 11/05/2019
ms.author: mlearned
ms.openlocfilehash: aa0cf1ef3f758d7aba4639d779bde90249d039cb
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73815670"
---
# <a name="secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Sicherer Zugriff auf den API-Server mit autorisierten IP-Adressbereichen in Azure Kubernetes Service (AKS)

In Kubernetes empfängt der API-Server Anforderungen zum Ausführen von Aktionen im Cluster wie z.B. das Erstellen von Ressourcen oder Skalieren der Anzahl der Knoten. Die API-Server ist die zentrale Möglichkeit zum Interagieren mit einem Cluster und seiner Verwaltung. Um die Clustersicherheit zu verbessern und Angriffe zu minimieren, sollte der Zugriff auf den API-Server nur über eine begrenzte Anzahl von IP-Adressbereichen möglich sein.

Dieser Artikel veranschaulicht die Verwendung der vom API-Server autorisierten IP-Adressbereiche. Diese werden zur Begrenzung der IP-Adressen und CIDRs verwendet, die Zugriff auf die Steuerungsebene erhalten.

> [!IMPORTANT]
> In neuen Clustern werden autorisierte IP-Adressbereiche für den API-Server nur auf dem *Standard* SKU Load Balancer unterstützt. Bestehende Cluster mit dem *Basic* SKU Load Balancer und den von API-Servern autorisierten IP-Adressbereichen bleiben weiter funktionsfähig. Diese bestehenden Cluster bleiben auch nach einem Upgrade funktionsfähig.

## <a name="before-you-begin"></a>Voraussetzungen

Vom API-Server autorisierte IP-Adressbereiche funktionieren nur für neue AKS-Cluster, die Sie erstellen. In diesem Artikel wird beschrieben, wie Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle erstellen.

Azure CLI-Version 2.0.76 oder höher muss installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Übersicht über vom API-Server autorisierte IP-Adressbereiche

Die zugrunde liegenden Kubernetes-APIs werden auf dem Kubernetes-API-Server verfügbar gemacht. Diese Komponente ermöglicht die Interaktion für Verwaltungstools, z.B. `kubectl` oder das Kubernetes-Dashboard. AKS stellt einen Clustermaster mit Einzelmandant mit einem dedizierten API-Server bereit. Standardmäßig wird der API-Server einer öffentlichen IP-Adresse zugewiesen, und Sie sollten den Zugriff mithilfe der rollenbasierten Zugriffssteuerung (RBAC, Role-based Access Control) steuern.

Zum Absichern des Zugriffs auf AKS-Steuerungsebene/API-Server, die andernfalls öffentlich zugänglich sind, können Sie autorisierte IP-Adressbereiche aktivieren und verwenden. Diese autorisierten IP-Adressbereiche erlauben nur definierten IP-Adressbereichen die Kommunikation mit dem API-Server. Eine Anforderung einer IP-Adresse, die nicht Teil dieser autorisierten IP-Adressbereiche ist, an den API-Server wird blockiert. Verwenden Sie weiterhin die RBAC, um Benutzer und die von ihnen angeforderten Aktionen zu autorisieren.

Weitere Informationen zum API-Server und anderen Clusterkomponenten finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][concepts-clusters-workloads].

## <a name="create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled"></a>Erstellen eines AKS-Clusters mit aktivierten vom API-Server autorisierten IP-Adressbereichen

Vom API-Server autorisierte IP-Adressbereiche funktionieren nur für neue AKS-Cluster. Erstellen Sie mithilfe von [az aks create][az-aks-create] einen Cluster, und geben Sie den Parameter *--api-server-authorized-ip-ranges* an, um eine Liste mit autorisierten IP-Adressbereichen bereitzustellen. Dabei handelt es sich in der Regel um IP-Adressbereiche, die von lokalen Netzwerken oder öffentlichen IP-Adressen verwendet werden. Wenn Sie einen CIDR-Bereich angeben, beginnen Sie mit der ersten IP-Adresse im Bereich. *137.117.106.90/29* ist z.B. ein gültiger Bereich, aber stellen Sie sicher, dass Sie die erste IP-Adresse im Bereich angeben, z.B. *137.117.106.88/29*.

> [!IMPORTANT]
> Der Cluster verwendet standardmäßig den [Lastenausgleich mit einer Standard-SKU][standard-sku-lb], den Sie zum Konfigurieren des Ausgangsgateways verwenden können. Wenn Sie vom API-Server autorisierte IP-Adressbereiche bei der Clustererstellung aktivieren, wird neben den von Ihnen angegebenen Bereichen standardmäßig auch die öffentliche IP-Adresse für Ihren Cluster zugelassen. Wenn Sie *""* oder keinen Wert für *--api-server-authorized-ip-ranges* angeben, werden vom API-Server autorisierte IP-Adressbereiche deaktiviert.

Im folgenden Beispiel wird in der Ressourcengruppe *myResourceGroup* ein Cluster namens *myAKSCluster* mit einem einzelnen Knoten und mit aktivierten vom API-Server autorisierten IP-Adressbereichen erstellt. Dabei werden die IP-Adressbereiche *73.140.245.0/24* zugelassen:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 73.140.245.0/24 \
    --generate-ssh-keys
```

> [!NOTE]
> Sie sollten diese Bereiche einer Zulassungsliste hinzufügen:
> - Öffentliche IP-Adresse der Firewall
> - Alle Bereiche mit Netzwerken, von denen aus Sie den Cluster verwalten
> - Wenn Sie in Ihrem AKS-Cluster Azure Dev Spaces verwenden, müssen Sie [zusätzliche Bereiche (auf der Grundlage Ihrer Region)][dev-spaces-ranges] zulassen.

### <a name="specify-the-outbound-ips-for-the-standard-sku-load-balancer"></a>Angeben der ausgehenden IP-Adressen für den Lastenausgleich mit Standard-SKU

Wenn Sie beim Erstellen eines AKS-Clusters die ausgehenden IP-Adressen oder -Präfixe für den Cluster angeben, werden diese Adressen oder Präfixe ebenfalls zugelassen. Beispiel:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 73.140.245.0/24 \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2> \
    --generate-ssh-keys
```

Im obigen Beispiel werden alle IP-Adressen zugelassen, die im Parameter *--load-balancer-outbound-ip-prefixes* oder *--api-server-authorized-ip-ranges* angegeben sind.

Alternativ können Sie den Parameter *--load-balancer-outbound-ip-prefixes* angeben, um ausgehende IP-Präfixe des Lastenausgleichs zuzulassen.

### <a name="allow-only-the-outbound-public-ip-of-the-standard-sku-load-balancer"></a>Ausschließliches Zulassen der ausgehenden öffentlichen IP-Adresse des Lastenausgleichs mit Standard-SKU

Wenn Sie vom API-Server autorisierte IP-Adressbereiche bei der Clustererstellung aktivieren, wird neben den von Ihnen angegebenen Bereichen standardmäßig auch die ausgehende öffentliche IP-Adresse des Lastenausgleichs mit Standard-SKU für Ihren Cluster zugelassen. Soll nur die ausgehende öffentliche IP-Adresse des Lastenausgleichs mit Standard-SKU zugelassen werden, verwenden Sie *0.0.0.0/32*, wenn Sie den Parameter *--api-server-authorized-ip-ranges* angeben.

Im folgenden Beispiel wird nur die ausgehende öffentliche IP-Adresse des Lastenausgleichs mit Standard-SKU zugelassen, und auf den API-Server kann nur über die Knoten innerhalb des Clusters zugegriffen werden.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --api-server-authorized-ip-ranges 0.0.0.0/32 \
    --generate-ssh-keys
```

## <a name="update-a-clusters-api-server-authorized-ip-ranges"></a>Aktualisieren der vom API-Server autorisierten IP-Adressbereiche eines Clusters

Wenn Sie die vom API-Server autorisierten IP-Adressbereiche für einen vorhandenen Cluster aktualisieren möchten, verwenden Sie den Befehl [az aks update][az-aks-update] mit den Parametern *--api-server-authorized-ip-ranges*, *--load-balancer-outbound-ip-prefixes*, *--load-balancer-outbound-ips* oder *--load-balancer-outbound-ip-prefixes*.

Im folgenden Beispiel werden vom API-Server autorisierte IP-Adressbereiche für den Cluster namens *myAKSCluster* in der Ressourcengruppe *myResourceGroup* aktualisiert. Der zu autorisierende IP-Adressbereich lautet *73.140.245.0/24*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges  73.140.245.0/24
```

Sie können auch *0.0.0.0/32* verwenden, wenn Sie den Parameter *--api-server-authorized-ip-ranges* angeben, um nur die öffentliche IP-Adresse des Lastenausgleichs mit Standard-SKU zuzulassen.

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
[dev-spaces-ranges]: https://github.com/Azure/dev-spaces/tree/master/public-ips
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet

<!-- LINKS - internal -->
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[route-tables]: ../virtual-network/manage-route-table.md
[standard-sku-lb]: load-balancer-standard.md
