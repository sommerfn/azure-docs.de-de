---
title: Verwenden eines Lastenausgleichs mit einer Standard-SKU in Azure Kubernetes Service (AKS)
description: Hier erfahren Sie, wie Sie einen Lastenausgleich mit einer Standard-SKU verwenden, um Ihre Dienste mit Azure Kubernetes Service (AKS) verfügbar zu machen.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 09/27/2019
ms.author: zarhoads
ms.openlocfilehash: 8ebd91f8f02ad7eacd8440b34a31b78f5cac5741
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472627"
---
# <a name="use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Verwenden eines Lastenausgleichs mit einer Standard-SKU in Azure Kubernetes Service (AKS)

Sie können eine Azure Load Balancer-Instanz erstellen und verwenden, um den Zugriff auf Ihre Anwendungen in Azure Kubernetes Service (AKS) zu ermöglichen. Ein in AKS ausgeführter Lastenausgleich kann als interner oder externer Lastenausgleich verwendet werden. Bei einem internen Lastenausgleich können nur Anwendungen, die im gleichen virtuellen Netzwerk ausgeführt werden wie der AKS-Cluster, auf einen Kubernetes-Dienst zugreifen. Ein externer Lastenausgleich erhält mindestens eine öffentliche IP-Adresse für eingehenden Datenverkehr, um externe Zugriffe auf einen Kubernetes-Dienst über die öffentlichen IP-Adressen zu ermöglichen.

Azure Load Balancer ist in zwei SKUs verfügbar: *Basic* und *Standard*. Beim Erstellen eines AKS-Clusters wird normalerweise die SKU vom Typ *Standard* verwendet. Bei Verwendung eines Lastenausgleichs mit einer SKU vom Typ *Standard* stehen zusätzliche Features und Funktionen zur Verfügung (etwa ein größerer Back-End-Pool und Verfügbarkeitszonen). Wichtig: Machen Sie sich mit den Unterschieden zwischen *Standard* und *Basic* vertraut, bevor Sie sich für einen Lastenausgleich entscheiden. Nach Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU für diesen Cluster nicht mehr geändert werden. Weitere Informationen zu den SKUs *Basic* und *Standard* finden Sie unter [Vergleich der Load Balancer-SKUs][azure-lb-comparison].

Für diesen Artikel werden Grundkenntnisse im Zusammenhang mit Kubernetes und Azure Load Balancer vorausgesetzt. Weitere Informationen finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][kubernetes-concepts] und [Was versteht man unter Azure Load Balancer?][azure-lb].

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Version 2.0.74 der Azure-Befehlszeilenschnittstelle verwenden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="before-you-begin"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über einen AKS-Cluster mit der Azure Load Balancer-SKU *Standard* verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Der AKS-Clusterdienstprinzipal benötigt auch die Berechtigung zum Verwalten von Netzwerkressourcen, wenn Sie ein bestehendes Subnetz oder eine vorhandene Ressourcengruppe verwenden. Im Allgemeinen weisen Sie die Rolle *Netzwerkmitwirkender* Ihrem Dienstprinzipal für die delegierten Ressourcen zu. Weitere Informationen zu Berechtigungen finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen][aks-sp].

### <a name="limitations"></a>Einschränkungen

Wenn Sie AKS-Cluster erstellen und verwalten, die einen Lastenausgleich mit der SKU *Standard* unterstützen, gelten folgende Einschränkungen:

* Mindestens eine öffentliche IP-Adresse oder ein IP-Präfix ist erforderlich, um ausgehenden Datenverkehr aus dem AKS-Cluster zuzulassen. Darüber hinaus wird die öffentliche IP-Adresse oder das IP-Präfix benötigt, um die Konnektivität zwischen der Steuerungsebene und den Agent-Knoten sowie die Kompatibilität mit früheren Versionen von AKS zu gewährleisten. Sie haben die folgenden Optionen zum Angeben von öffentlichen IP-Adressen oder IP- Präfixen mit einem *Standard*-SKU-Lastenausgleichsmodul:
    * Geben Sie Ihre eigenen öffentlichen IP-Adressen an.
    * Geben Sie Ihre eigenen öffentlichen IP-Präfixe an.
    * Geben Sie eine Zahl bis 100 an, damit der AKS-Cluster so viele öffentliche *Standard*-SKU-IP-Adressen in derselben Ressourcengruppe erstellen kann, in der sich der AKS-Cluster befindet, in der Regel mit *MC_* am Anfang benannt. AKS weist die öffentliche IP-Adresse dem Lastenausgleich mit der SKU *Standard* zu. Standardmäßig wird automatisch eine öffentliche IP-Adresse in derselben Ressourcengruppe wie der AKS-Cluster erstellt, wenn weder eine öffentliche IP-Adresse noch ein öffentliches IP-Präfix oder eine Anzahl von IP-Adressen angegeben wird. Sie müssen auch öffentliche Adressen zulassen und dürfen keine Azure-Richtlinie erstellen, die die Erstellung von IP-Adressen unterbindet.
* Wenn Sie für einen Lastenausgleich die *Standard*-SKU verwenden, benötigen Sie mindestens die Kubernetes-Version 1.13.
* Das Definieren der Lastenausgleichs-SKU kann nur durchgeführt werden, wenn Sie einen AKS-Cluster erstellen. Nach der Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU nicht mehr geändert werden.
* Sie können pro Cluster nur eine einzelne Lastenausgleichs-SKU verwenden.

## <a name="configure-the-load-balancer-to-be-internal"></a>Konfigurieren des Load Balancers als internen Lastenausgleich

Sie können den Lastenausgleich auch als internen Lastenausgleich konfigurieren und keine öffentliche IP-Adresse verfügbar machen. Wenn Sie den Lastenausgleich als intern konfigurieren möchten, müssen Sie dem Dienst *LoadBalancer* die Anmerkung `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` hinzufügen. Ein exemplarisches YAML-Manifest sowie weitere Details zum internen Lastenausgleich finden Sie [hier][internal-lb-yaml].

## <a name="scale-the-number-of-managed-public-ips"></a>Skalieren der Anzahl der verwalteten öffentlichen IP-Adressen

Wenn Sie einen *Standard*-SKU-Lastenausgleich mit verwalteten ausgehenden öffentlichen IP-Adressen verwenden, die standardmäßig erstellt werden, können Sie die Anzahl verwalteter ausgehender öffentlicher IP-Adressen mit dem *load-balancer-managed-ip-count*-Parameter skalieren.

Zum Aktualisieren eines vorhandenen Clusters führen Sie den folgenden Befehl aus. Dieser Parameter kann auch zum Zeitpunkt der Clustererstellung festgelegt werden, um mehrere verwaltete ausgehende öffentliche IP-Adressen zu erhalten.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

Im obigen Beispiel wird die Anzahl der verwalteten ausgehenden öffentlichen IP-Adressen für den *myAKSCluster*-Cluster in *myResourceGroup* auf *2* festgelegt. 

Sie können auch den Parameter *load-balancer-managed-ip-count* verwenden, um die anfängliche Anzahl von verwalteten ausgehenden öffentlichen IP-Adressen beim Erstellen des Clusters festzulegen. Dazu fügen Sie den Parameter `--load-balancer-managed-outbound-ip-count` an und legen ihn auf den gewünschten Wert fest. Die Standardanzahl von verwalteten ausgehenden öffentlichen IP-Adressen beträgt 1.

## <a name="provide-your-own-public-ips-or-prefixes-for-egress"></a>Angeben Ihrer eigenen öffentlichen IP-Adressen oder Präfixe für ausgehenden Datenverkehr

Wenn Sie einen *Standard*-SKU-Lastenausgleich verwenden, erstellt der AKS-Cluster automatisch eine öffentliche IP-Adresse in derselben Ressourcengruppe, die für den AKS-Cluster erstellt wurde, und weist die öffentliche IP-Adresse dem *Standard*-SKU-Lastenausgleich zu. Alternativ können Sie Ihre eigene öffentliche IP-Adresse zum Zeitpunkt der Clustererstellung zuweisen, oder Sie können die Lastenausgleichseigenschaften eines vorhandenen Clusters aktualisieren.

Durch Einbinden mehrerer IP-Adressen oder IP-Präfixe können Sie mehrere unterstützende Dienste definieren, wenn Sie die IP-Adresse hinter einem einzelnen Lastenausgleichsobjekt definieren. Der Endpunkt für ausgehenden Datenverkehr bestimmter Knoten hängt von dem Dienst ab, dem sie zugeordnet sind.

> [!IMPORTANT]
> Sie müssen die öffentlichen IP-Adressen der *Standard*-SKU für ausgehenden Datenverkehr mit Ihrem *Standard*-SKU-Lastenausgleich verwenden. Sie können die SKU Ihrer öffentlichen IP-Adressen mit dem Befehl [az network public-ip show][az-network-public-ip-show] überprüfen:
>
> ```azurecli-interactive
> az network public-ip show --resource-group myResourceGroup --name myPublicIP --query sku.name -o tsv
> ```

Verwenden Sie den Befehl [az network public-ip show][az-network-public-ip-show], um die IDs Ihrer öffentlichen IP-Adressen aufzulisten.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

Der obige Befehl zeigt die ID für die öffentliche IP-Adresse *myPublicIP* in der Ressourcengruppe *myResourceGroup* an.

Verwenden Sie den Befehl *az aks update* mit dem Parameter *load-balancer-outbound-ips*, um Ihren Cluster mit ihren öffentlichen IP-Adressen zu aktualisieren.

Im folgenden Beispiel wird der Parameter *load-balancer-outbound-ips* mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Sie können auch öffentliche IP-Präfixe für ausgehenden Datenverkehr mit Ihrem *Standard*-SKU-Lastenausgleich verwenden. Im folgenden Beispiel wird der Befehl [az network public-ip prefix show][az-network-public-ip-prefix-show] verwendet, um die IDs Ihrer öffentlichen IP-Präfixe aufzulisten:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

Der obige Befehl zeigt die ID für das öffentliche IP-Präfix *myPublicIPPrefix* in der Ressourcengruppe *myResourceGroup* an.

Im folgenden Beispiel wird der Parameter *load-balancer-outbound-ip-prefixes* mit den IDs aus dem vorherigen Befehl verwendet.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

> [!IMPORTANT]
> Die öffentlichen IP-Adressen und IP-Präfixe müssen sich in derselben Region wie Ihr AKS-Cluster befinden und Teil desselben Abonnements sein. 

### <a name="define-your-own-public-ip-or-prefixes-at-cluster-create-time"></a>Definieren eigener öffentlicher IP-Adressen oder IP-Präfixe zum Zeitpunkt der Clustererstellung

Möglicherweise möchten Sie zum Zeitpunkt der Clustererstellung Ihre eigenen IP-Adressen oder IP-Präfixe für den ausgehenden Datenverkehr einbinden, um Szenarien wie das Aufnehmen von Endpunkten für ausgehenden Datenverkehr in die Whitelists zu unterstützen. Fügen Sie die oben gezeigten Parameter im Schritt zur Clustererstellung an, um Ihre eigenen öffentlichen IP-Adressen und IP-Präfixe zu Beginn des Lebenszyklus eines Clusters zu definieren.

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ips*, um einen neuen Cluster mit Ihren öffentlichen IP-Adressen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Verwenden Sie den Befehl *az aks create* mit dem Parameter *load-balancer-outbound-ip-prefixes*, um einen neuen Cluster mit Ihren öffentlichen IP-Präfixen zu Beginn zu erstellen.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 1 \
    --load-balancer-sku standard \
    --generate-ssh-keys \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Kubernetes-Diensten finden Sie in der entsprechenden [Dokumentation][kubernetes-services].

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-network-public-ip-show]: /cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest#az-network-public-ip-prefix-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
