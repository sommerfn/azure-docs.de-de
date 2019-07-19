---
title: 'Vorschauversion: Verwenden eines Lastenausgleichs mit einer Standard-SKU in Azure Kubernetes Service (AKS)'
description: Hier erfahren Sie, wie Sie einen Lastenausgleich mit einer Standard-SKU verwenden, um Ihre Dienste mit Azure Kubernetes Service (AKS) verfügbar zu machen.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/25/2019
ms.author: zarhoads
ms.openlocfilehash: a9cf3db3a15fab5a2f067a146950e02923a20379
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476809"
---
# <a name="preview---use-a-standard-sku-load-balancer-in-azure-kubernetes-service-aks"></a>Vorschauversion: Verwenden eines Lastenausgleichs mit einer Standard-SKU in Azure Kubernetes Service (AKS)

Sie können eine Azure Load Balancer-Instanz erstellen und verwenden, um den Zugriff auf Ihre Anwendungen in Azure Kubernetes Service (AKS) zu ermöglichen. Ein in AKS ausgeführter Lastenausgleich kann als interner oder externer Lastenausgleich verwendet werden. Bei einem internen Lastenausgleich können nur Anwendungen, die im gleichen virtuellen Netzwerk ausgeführt werden wie der AKS-Cluster, auf einen Kubernetes-Dienst zugreifen. Ein externer Lastenausgleich erhält mindestens eine öffentliche IP-Adresse für eingehenden Datenverkehr, um externe Zugriffe auf einen Kubernetes-Dienst über die öffentlichen IP-Adressen zu ermöglichen.

Azure Load Balancer ist in zwei SKUs verfügbar: *Basic* und *Standard*. Standardmäßig wird die SKU *Basic* verwendet, wenn in AKS ein Lastenausgleich unter Verwendung eines Dienstmanifests erstellt wird. Bei Verwendung eines Lastenausgleichs mit einer SKU vom Typ *Standard* stehen zusätzliche Features und Funktionen zur Verfügung (etwa ein größerer Back-End-Pool und Verfügbarkeitszonen). Wichtig: Machen Sie sich mit den Unterschieden zwischen *Standard* und *Basic* vertraut, bevor Sie sich für einen Lastenausgleich entscheiden. Nach Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU für diesen Cluster nicht mehr geändert werden. Weitere Informationen zu den SKUs *Basic* und *Standard* finden Sie unter [Vergleich der Load Balancer-SKUs][azure-lb-comparison].

In diesem Artikel erfahren Sie, wie Sie eine Azure Load Balancer-Instanz mit der SKU *Standard* mit Azure Kubernetes Service (AKS) erstellen und verwenden.

Für diesen Artikel werden Grundkenntnisse im Zusammenhang mit Kubernetes und Azure Load Balancer vorausgesetzt. Weitere Informationen finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][kubernetes-concepts] sowie unter „Was versteht man unter Azure Load Balancer?“.

Diese Funktion steht derzeit als Vorschau zur Verfügung.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens die Version 2.0.59 der Azure-Befehlszeilenschnittstelle verwenden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="before-you-begin"></a>Voraussetzungen

Der AKS-Clusterdienstprinzipal benötigt die Berechtigung zum Verwalten von Netzwerkressourcen, wenn Sie ein bestehendes Subnetz oder eine vorhandene Ressourcengruppe verwenden. Im Allgemeinen weisen Sie die Rolle *Netzwerkmitwirkender* Ihrem Dienstprinzipal für die delegierten Ressourcen zu. Weitere Informationen zu Berechtigungen finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen][aks-sp].

Sie müssen einen AKS-Cluster erstellen, der die SKU für den Lastenausgleich auf *Standard* festlegt anstatt auf den Standardwert *Basic*. Das Erstellen eines AKS-Clusters wird in einem späteren Schritt behandelt. Sie müssen jedoch zunächst einige Vorschaufeatures aktivieren.

> [!IMPORTANT]
> AKS-Vorschaufeatures stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Sie werden zum Sammeln von Feedback und Fehlern mithilfe unserer Community bereitgestellt. In der Vorschauversion sind diese Features nicht für den Einsatz in der Produktion vorgesehen. Features in der öffentlichen Vorschau unterliegen dem Prinzip des „bestmöglichen Supports“. Unterstützung durch die Teams für den technischen AKS-Support steht nur während der Geschäftszeiten in der Zeitzone „Pacific Standard Time“ (PST) zur Verfügung. Weitere Informationen hierzu finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Um die Azure Load Balancer-SKU „Standard“ verwenden zu können, benötigen Sie mindestens die Version 0.4.1 der CLI-Erweiterung *aks-preview*. Installieren Sie die Erweiterung *aks-preview* der Azure-Befehlszeilenschnittstelle mithilfe des Befehls [az extension add][az-extension-add], und verwenden Sie anschließend den Befehl „az extension update“, um nach verfügbaren Updates zu suchen:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-aksazurestandardloadbalancer-preview-feature"></a>Registrieren des Vorschaufeatures „AKSAzureStandardLoadBalancer“

Wenn Sie einen AKS-Cluster erstellen möchten, der einen Lastenausgleich mit der SKU *Standard* verwenden kann, müssen Sie in Ihrem Abonnement das Featureflag *AKSAzureStandardLoadBalancer* aktivieren. Das Feature *AKSAzureStandardLoadBalancer* verwendet beim Erstellen eines Clusters mit VM-Skalierungsgruppen auch *VMSSPreview*. Dieses Feature stellt beim Konfigurieren eines Clusters die neuesten Diensterweiterungen bereit. Es wird empfohlen, das Featureflag *VMSSPreview* ebenfalls zu aktivieren. Dies ist jedoch nicht zwingend erforderlich.

> [!CAUTION]
> Wenn Sie ein Feature für ein Abonnement registrieren, können Sie die Registrierung dieses Features momentan nicht mehr aufheben. Nachdem Sie einige Vorschaufeatures aktiviert haben, können Standardwerte für alle AKS-Cluster verwendet werden, die dann im Abonnement erstellt werden. Aktivieren Sie keine Vorschaufeatures für Produktionsabonnements. Verwenden Sie ein separates Abonnement, um Vorschaufeatures zu testen und Feedback zu sammeln.

Registrieren Sie die Featureflags *VMSSPreview* und *AKSAzureStandardLoadBalancer* mithilfe des Befehls [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "VMSSPreview"
az feature register --namespace "Microsoft.ContainerService" --name "AKSAzureStandardLoadBalancer"
```

> [!NOTE]
> Jeder AKS-Cluster, der nach erfolgreicher Registrierung des Featureflags *VMSSPreview* oder *AKSAzureStandardLoadBalancer* erstellt wird, verwendet diese Vorschauclusterumgebung. Um weiterhin normale, vollständig unterstützte Cluster zu erstellen, sollten Sie keine Vorschaufeatures für Produktionsabonnements aktivieren. Verwenden Sie ein separates Test- oder Entwickungsabonnement von Azure, um Vorschaufeatures zu testen.

Es dauert einige Minuten, bis der Status *Registered (Registriert)* angezeigt wird. Sie können den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list] überprüfen:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSAzureStandardLoadBalancer')].{Name:name,State:properties.state}"
```

Wenn Sie so weit sind, aktualisieren Sie mithilfe des Befehls [az provider register][az-provider-register] die Registrierung des Ressourcenanbieters *Microsoft.ContainerService*:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Einschränkungen

Wenn Sie AKS-Cluster erstellen und verwalten, die einen Lastenausgleich mit der SKU *Standard* unterstützen, gelten folgende Einschränkungen:

* Bei Verwendung eines Lastenausgleichs mit der SKU *Standard* müssen Sie öffentliche Adressen zulassen und dürfen keine Azure-Richtlinie erstellen, die die Erstellung von IP-Adressen unterbindet. Der AKS-Cluster erstellt automatisch eine öffentliche IP-Adresse der SKU *Standard* in der gleichen Ressourcengruppe, die für den AKS-Cluster erstellt wurde. (Der Name beginnt in der Regel mit *MC_* .) AKS weist die öffentliche IP-Adresse dem Lastenausgleich mit der SKU *Standard* zu. Die öffentliche IP-Adresse ist erforderlich, um ausgehenden Datenverkehr aus dem AKS-Cluster zuzulassen. Darüber hinaus wird diese öffentliche IP-Adresse benötigt, um die Konnektivität zwischen der Steuerungsebene und den Agent-Knoten sowie die Kompatibilität mit früheren Versionen von AKS zu gewährleisten.
* Wenn Sie für einen Lastenausgleich die SKU *Standard* verwenden, benötigen Sie mindestens die Kubernetes-Version 1.13.5.

Während sich dieses Feature in der Vorschauversion befindet, gelten die folgenden zusätzlichen Einschränkungen:

* Wenn Sie für einen Lastenausgleich in AKS die SKU *Standard* verwenden, können Sie für den Lastenausgleich nicht Ihre eigene öffentliche IP-Adresse für ausgehenden Datenverkehr festlegen. Sie müssen die IP-Adresse verwenden, die AKS Ihrem Lastenausgleich zuweist.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden. Wenn Sie eine Ressourcengruppe erstellen, müssen Sie einen Speicherort angeben. An diesem Speicherort werden die Metadaten der Ressourcengruppe gespeichert. Darüber hinaus werden dort die Ressourcen in Azure ausgeführt, wenn Sie während der Ressourcenerstellung keine andere Region angeben. Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Die folgende Beispielausgabe zeigt, dass die Ressourcengruppe erfolgreich erstellt wurde:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>Erstellen eines ACS-Clusters
Wenn Sie einen AKS-Cluster verwenden möchten, der einen Lastenausgleich mit der SKU *Standard* unterstützt, muss der Parameter *load-balancer-sku* des Clusters auf *standard* festgelegt werden. Dieser Parameter sorgt bei der Clustererstellung dafür, dass ein Lastenausgleich mit der SKU *Standard* erstellt wird. Wenn Sie in Ihrem Cluster einen Dienst vom Typ *LoadBalancer* ausführen, wird die Konfiguration des Lastenausgleichs mit der SKU *Standard* mit der Konfiguration des Diensts aktualisiert. Erstellen Sie mithilfe des Befehls [az aks create][az-aks-create] einen AKS-Cluster namens *myAKSCluster*.

> [!NOTE]
> Die Eigenschaft *load-balancer-sku* kann nur bei der Clustererstellung verwendet werden. Nach der Erstellung eines AKS-Clusters kann die Lastenausgleichs-SKU nicht mehr geändert werden. Darüber hinaus kann pro Cluster immer nur eine einzelne Art von Lastenausgleichs-SKU verwendet werden.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --kubernetes-version 1.14.0 \
    --load-balancer-sku standard \
    --generate-ssh-keys
```

Nach wenigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben.

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Verwenden Sie zum Verwalten eines Kubernetes-Clusters den Kubernetes-Befehlszeilenclient [kubectl][kubectl]. Bei Verwendung von Azure Cloud Shell ist `kubectl` bereits installiert. Verwenden Sie für die lokale Installation von `kubectl` den Befehl [az aks install-cli][az-aks-install-cli]:

```azurecli
az aks install-cli
```

Mit dem Befehl [az aks get-credentials][az-aks-get-credentials] können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Mit diesem Befehl werden die Anmeldeinformationen heruntergeladen, und die Kubernetes-Befehlszeilenschnittstelle wird für deren Verwendung konfiguriert.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Verwenden Sie zum Überprüfen der Verbindung mit Ihrem Cluster den Befehl [kubectl get][kubectl-get], um eine Liste der Clusterknoten zu erhalten.

```azurecli-interactive
kubectl get nodes
```

Die folgende Beispielausgabe zeigt den in den vorherigen Schritten erstellten Knoten. Vergewissern Sie sich, dass der Knoten den Status *Bereit* hat:

```
NAME                       STATUS   ROLES   AGE     VERSION
aks-nodepool1-31718369-0   Ready    agent   6m44s   v1.14.0
```

## <a name="verify-your-cluster-uses-the-standard-sku"></a>Vergewissern, dass der Cluster die SKU *Standard* verwendet

Verwenden Sie den Befehl [az aks show][az-aks-show], um die Konfiguration Ihres Clusters anzuzeigen.

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster

{
  "aadProfile": null,
  "addonProfiles": null,
   ...
   "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "loadBalancerSku": "standard",
    ...
```

Vergewissern Sie sich, dass die Eigenschaft *loadBalancerSku* den Wert *standard* hat.

## <a name="use-the-load-balancer"></a>Verwenden des Lastenausgleichs

Erstellen Sie ein Dienstmanifest mit dem Diensttyp *LoadBalancer*, um den Lastenausgleich für Ihren Cluster zu verwenden. Erstellen Sie ein weiteres Manifest mit einer in Ihrem Cluster ausgeführten Beispielanwendung, um zu zeigen, dass der Lastenausgleich funktioniert. Diese Beispielanwendung wird über den Lastenausgleich verfügbar gemacht und kann über einen Browser angezeigt werden.

Erstellen Sie ein Manifest namens `sample.yaml`, wie im folgenden Beispiel gezeigt:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
```

Das obige Manifest konfiguriert zwei Bereitstellungen: *azure-vote-front* und *azure-vote-back*. Erstellen Sie ein Manifest namens `standard-lb.yaml`, wie im folgenden Beispiel zu sehen, um die Bereitstellung *azure-vote-front* so zu konfigurieren, dass sie unter Verwendung des Lastenausgleichs verfügbar gemacht wird:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Der Dienst *azure-vote-front* verwendet den Typ *LoadBalancer*, um den Lastenausgleich in Ihrem AKS-Cluster für die Verbindungsherstellung mit der Bereitstellung *azure-vote-front* zu konfigurieren.

Stellen Sie die Beispielanwendung und den Lastenausgleich mithilfe des Befehls [kubectl apply][kubectl-apply] bereit, und geben Sie den Namen Ihrer YAML-Manifeste an:

```console
kubectl apply -f sample.yaml
kubectl apply -f standard-lb.yaml
```

Der Lastenausgleich mit der SKU *Standard* ist nun konfiguriert, um die Beispielanwendung verfügbar zu machen. Verwenden Sie [kubectl get][kubectl-get], um die Dienstdetails von *azure-vote-front* (einschließlich der öffentlichen IP-Adresse des Lastenausgleichs) anzuzeigen. Die öffentliche IP-Adresse des Lastenausgleichs befindet sich in der Spalte *EXTERNAL-IP* (EXTERNE IP-ADRESSE). Es dauert möglicherweise ein oder zwei Minuten, bis sich die IP-Adresse von *\<pending\>* (<Ausstehend>) in eine tatsächliche externe IP-Adresse ändert, wie im folgenden Beispiel gezeigt:

```
$ kubectl get service azure-vote-front

NAME                TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
azure-vote-front    LoadBalancer   10.0.227.198   52.179.23.131   80:31201/TCP   16s
```

Navigieren Sie in einem Browser zu der öffentlichen IP-Adresse. Daraufhin sollte die Beispielanwendung angezeigt werden. Im obigen Beispiel lautet die öffentliche IP-Adresse `52.179.23.131`.

![Abbildung der Navigation zu Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

> [!NOTE]
> Sie können den Lastenausgleich auch als internen Lastenausgleich konfigurieren und keine öffentliche IP-Adresse verfügbar machen. Wenn Sie den Lastenausgleich als intern konfigurieren möchten, müssen Sie dem Dienst *LoadBalancer* die Anmerkung `service.beta.kubernetes.io/azure-load-balancer-internal: "true"` hinzufügen. Ein exemplarisches YAML-Manifest sowie weitere Details zum internen Lastenausgleich finden Sie [hier][internal-lb-yaml].

## <a name="clean-up-the-standard-sku-load-balancer-configuration"></a>Bereinigen der Konfiguration des Lastenausgleichs mit der SKU „Standard“

Verwenden Sie [kubectl delete][kubectl-delete], um die Beispielanwendung und die Konfiguration des Lastenausgleichs zu entfernen:

```console
kubectl delete -f sample.yaml
kubectl delete -f standard-lb.yaml
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
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/load-balancer-overview.md#skus
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
