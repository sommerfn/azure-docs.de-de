---
title: Vorschauversion – Erstellen eines Windows Server-Containers auf einem Azure Kubernetes Service (AKS)-Cluster
description: Hier erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle schnell einen Kubernetes-Cluster erstellen und eine Anwendung in einem Windows Server-Container in Azure Kubernetes Service (AKS) bereitstellen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/17/2019
ms.author: mlearned
ms.openlocfilehash: ff4367194f06a8a6895c9c16252b01c3b94995d3
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72241259"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Vorschauversion – Erstellen eines Windows Server-Containers auf einem Azure Kubernetes Service (AKS)-Cluster mit der Azure-Befehlszeilenschnittstelle

Azure Kubernetes Service (AKS) ist ein verwalteter Kubernetes-Dienst, mit dem Sie schnell Cluster bereitstellen und verwalten können. In diesem Artikel stellen Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle bereit. Sie stellen auch eine ASP.NET-Beispielanwendung in einem Windows Server-Container für den Cluster bereit.

Diese Funktion steht derzeit als Vorschau zur Verfügung.

![Abbildung der Navigation zur ASP.NET-Beispielanwendung](media/windows-container/asp-net-sample-app.png)

Für diesen Artikel werden Grundkenntnisse in Bezug auf die Kubernetes-Konzepte vorausgesetzt. Weitere Informationen finden Sie unter [Grundlegende Kubernetes-Konzepte für Azure Kubernetes Service (AKS)][kubernetes-concepts].

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittstelle (CLI) lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 2.0.61 der Azure CLI ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

## <a name="before-you-begin"></a>Voraussetzungen

Sie müssen nach der Erstellung Ihres Clusters einen zusätzlichen Knotenpool hinzufügen, der einen Windows Server-Container ausführen kann. Das Hinzufügen eines zusätzlichen Knotenpools wird in einem späteren Schritt behandelt, aber Sie müssen zunächst einige Previewfunktion aktivieren.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Installieren der CLI-Erweiterung „aks-preview“

Für die Verwendung von Windows Server-Containern benötigen Sie *aks-preview*-CLI-Erweiterungsversion 0.4.12 oder höher. Installieren Sie die Azure CLI-Erweiterung *aks-preview* mit dem Befehl [az extension add][az-extension-add], und suchen Sie dann mit dem Befehl [az extension update][az-extension-update] nach verfügbaren Updates:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-windows-preview-feature"></a>Registrieren der Windows-Previewfunktion

Um einen AKS-Cluster zu erstellen, der mehrere Knotenpools verwenden und Windows Server-Container ausführen kann, aktivieren Sie zunächst die Featureflags *WindowsPreview* in Ihrem Abonnement. Das Feature *WindowsPreview* verwendet auch Poolcluster mit mehreren Knoten und die Skalierung des virtuellen Computers, um die Bereitstellung und Konfiguration der Kubernetes-Knoten zu verwalten. Registrieren Sie das Featureflag *WindowsPreview* mit dem Befehl [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Jeder AKS-Cluster, den Sie erstellen, nachdem Sie das Featureflag *WindowsPreview* erfolgreich registriert haben, verwendet diesen Vorschaucluster. Um weiterhin regelmäßige, vollständig unterstützte Cluster zu erstellen, sollten Sie keine Previewfunktionen für Produktionsabonnements aktivieren. Verwenden Sie ein separates Test- oder Entwickungsabonnement von Azure, um Vorschaufeatures zu testen.

Es dauert einige Minuten, bis die Registrierung abgeschlossen ist. Überprüfen Sie den Registrierungsstatus mit dem Befehl [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Wenn der Registrierungsstatus `Registered` lautet, drücken Sie STRG+C, um die Überwachung des Zustands zu beenden.  Aktualisieren Sie anschließend die Registrierung des *Microsoft.ContainerService*-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Erstellung und Verwaltung von AKS-Clustern, die mehrere Knotenpools unterstützen:

* Für Cluster, die erstellt wurden, nachdem Sie die *WindowsPreview* erfolgreich registriert haben, stehen mehrere Knotenpools zur Verfügung. Mehrere Knotenpools sind auch verfügbar, wenn Sie das Feature *MultiAgentpoolPreview* für Ihr Abonnement registrieren. Sie können keine Knotenpools mit einem bestehenden AKS-Cluster hinzufügen oder verwalten, die vor der erfolgreichen Registrierung dieses Features erstellt wurden.
* Sie können den ersten Knotenpool nicht löschen.

Während sich diese Funktion in der Vorschau befindet, gelten die folgenden zusätzlichen Einschränkungen:

* Der AKS-Cluster kann maximal acht Knotenpools umfassen.
* Der AKS-Cluster kann innerhalt dieser acht Knotenpools maximal 400 Knoten haben.
* Der Name des Windows Server-Knotenpools ist auf 6 Zeichen begrenzt.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist eine logische Gruppe, in der Azure-Ressourcen bereitgestellt und verwaltet werden. Wenn Sie eine Ressourcengruppe erstellen, müssen Sie einen Speicherort angeben. An diesem Speicherort werden die Metadaten der Ressourcengruppe gespeichert. Darüber hinaus werden dort die Ressourcen in Azure ausgeführt, wenn Sie während der Ressourcenerstellung keine andere Region angeben. Erstellen Sie mit dem Befehl [az group create][az-group-create] eine Ressourcengruppe.

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus*.

> [!NOTE]
> In diesem Artikel wird die Bash-Syntax für die Befehle des Tutorials verwendet.
> Stellen Sie bei Verwendung von Azure Cloud Shell sicher, dass die Dropdownliste oben links im Cloud Shell-Fenster auf **Bash** festgelegt ist.

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

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Um einen AKS-Cluster auszuführen, der Knotenpools für Windows Server-Container unterstützt, muss Ihr Cluster eine Netzwerkrichtlinie verwenden, die das [Azure CNI][azure-cni-about]-Netzwerk-Plug-In (Erweitert) verwendet. Detaillierte Informationen zur Planung der erforderlichen Subnetzbereiche sowie Netzwerküberlegungen finden Sie unter [Konfigurieren von Azure CNI-Netzwerken][use-advanced-networking]. Erstellen Sie mithilfe des Befehls [az aks create][az-aks-create] einen AKS-Cluster namens *myAKSCluster*. Dieser Befehl erstellt die erforderlichen Netzwerkressourcen, wenn sie nicht vorhanden sind.
  * Der Cluster wird mit zwei Knoten konfiguriert.
  * Die Parameter *windows-admin-password* und *windows-admin-username* legen die Anmeldeinformationen für alle Windows Server-Container fest, die auf dem Cluster erstellt wurden.

> [!NOTE]
> Um zu gewährleisten, dass Ihr Cluster zuverlässig funktioniert, sollten Sie mindestens zwei Knoten im Standardknotenpool ausführen.

Geben Sie Ihr eigenes sicheres Kennwort (*PASSWORD_WIN*) an (für diesen Artikel werden Befehle in eine BASH-Shell eingegeben):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.6 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

> [!Note]
> Falls Sie für das Kennwort einen Überprüfungsfehler erhalten, sollten Sie versuchen, Ihre Ressourcengruppe in einer anderen Region zu erstellen.
> Versuchen Sie anschließend, den Cluster mit der neuen Ressourcengruppe zu erstellen.

Nach wenigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben. Gelegentlich kann die Bereitstellung des Clusters länger als ein paar Minuten dauern. Warten Sie in diesen Fällen bis zu 10 Minuten. 

## <a name="add-a-windows-server-node-pool"></a>Hinzufügen eines Windows Server-Knotenpools

Standardmäßig wird ein AKS-Cluster mit einem Knotenpool erstellt, der Linux-Container ausführen kann. Verwenden Sie den Befehl `az aks nodepool add`, um einen zusätzlichen Knotenpool hinzuzufügen, der Windows Server-Container ausführen kann.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.6
```

Der obige Befehl erstellt einen neuen Knotenpool namens *npwin* und fügt ihn dem *myAKSCluster* hinzu. Wenn Sie einen Knotenpool erstellen, um Windows Server-Container auszuführen, ist der Standardwert für *node-vm-size* *Standard_D2s_v3*. Wenn Sie den Parameter *node-vm-size* festlegen, sollten Sie die Liste mit den [eingeschränkten VM-Größen][restricted-vm-sizes] überprüfen. Die empfohlene Mindestgröße ist *Standard_D2s_v3*. Der obige Befehl verwendet auch das Standardsubnetz im Standard-Vnet, das beim Ausführen von `az aks create` erstellt wurde.

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

Die folgende Beispielausgabe zeigt alle Knoten im Cluster. Vergewissern Sie sich, dass alle Knoten den Status *Bereit* haben:

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.6
aksnpwin987654                      Ready    agent   108s   v1.14.6
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Eine Kubernetes-Manifestdatei definiert einen gewünschten Zustand (Desired State) für den Cluster – also beispielsweise, welche Containerimages ausgeführt werden sollen. In diesem Artikel wird ein Manifest verwendet, um alle Objekte zu erstellen, die zum Ausführen der ASP.NET-Beispielanwendung in einem Windows Server-Container erforderlich sind. Dieses Manifest beinhaltet eine [Kubernetes-Bereitstellung][kubernetes-deployment] für die ASP.NET-Beispielanwendung und einen externen [Kubernetes-Dienst][kubernetes-service] für den Zugriff auf die Anwendung über das Internet.

Die ASP.NET-Beispielanwendung wird als Teil des [.NET Framework-Beispiels][dotnet-samples] bereitgestellt und in einem Windows Server-Container ausgeführt. AKS verlangt, dass Windows Server-Container auf Images von *Windows Server 2019* oder höher basieren. Mit der Kubernetes-Manifestdatei muss auch eine [Knotenauswahl][node-selector] definiert werden. So wird Ihrem AKS-Cluster mitgeteilt, dass der Pod Ihrer ASP.NET-Beispielanwendung auf einem Knoten ausgeführt werden soll, für den die Ausführung von Windows Server-Containern möglich ist.

Erstellen Sie eine Datei namens `sample.yaml`, und fügen Sie die folgende YAML-Definition ein. Bei Verwendung von Azure Cloud Shell kann diese Datei mit `vi` oder `nano` erstellt werden – genau wie bei der Verwendung eines virtuellen oder physischen Systems:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800M
          requests:
            cpu: .1
            memory: 300M
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Stellen Sie die Anwendung über den Befehl [kubectl apply][kubectl-apply] bereit, und geben Sie den Namen Ihres YAML-Manifests an:

```azurecli-interactive
kubectl apply -f sample.yaml
```

In der folgende Beispielausgabe sind die erfolgreich erstellten Bereitstellungen und Dienste aufgeführt:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Testen der Anwendung

Wenn die Anwendung ausgeführt wird, macht ein Kubernetes-Dienst das Anwendungs-Front-End im Internet verfügbar. Dieser Vorgang kann einige Minuten dauern. Gelegentlich kann die Bereitstellung des Diensts länger als ein paar Minuten dauern. Warten Sie in diesen Fällen bis zu 10 Minuten.

Verwenden Sie zum Überwachen des Fortschritts den Befehl [kubectl get service][kubectl-get] mit dem Argument `--watch`.

```azurecli-interactive
kubectl get service sample --watch
```

Die externe IP-Adresse *EXTERNAL-IP* für den *Beispieldienst* wird zunächst als *ausstehend* angezeigt.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Sobald die externe IP-Adresse (*EXTERNAL-IP*) von *ausstehend* in eine tatsächliche öffentliche IP-Adresse geändert wurde, wird, verwenden Sie `CTRL-C`, um die `kubectl`-Überwachung zu beenden. Die folgende Beispielausgabe zeigt eine gültige öffentliche IP-Adresse, die dem Dienst zugewiesen ist:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Öffnen Sie die externe IP-Adresse Ihres Diensts in einem Webbrowser, um die Beispielanwendung in Aktion zu sehen.

![Abbildung der Navigation zur ASP.NET-Beispielanwendung](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Löschen von Clustern

Wenn der Cluster nicht mehr benötigt wird, entfernen Sie mit dem Befehl [az group delete][az-group-delete] die Ressourcengruppe, den Containerdienst und alle zugehörigen Ressourcen.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Wenn Sie den Cluster löschen, wird der vom AKS-Cluster verwendete Azure Active Directory-Dienstprinzipal nicht entfernt. Schritte zum Entfernen des Dienstprinzipals finden Sie unter den [Überlegungen zum AKS-Dienstprinzipal und dessen Löschung][sp-delete].

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen Kubernetes-Cluster und eine ASP.NET-Beispielanwendung in einem Windows Server-Container bereitgestellt. [Zugreifen auf das Kubernetes-Webdashboard][kubernetes-dashboard] für den Cluster, den Sie gerade erstellt haben.

Weitere Informationen zu Azure Container Service sowie ein vollständiges Beispiel vom Code bis zur Bereitstellung finden Sie im Kubernetes-Clustertutorial.

> [!div class="nextstepaction"]
> [AKS-Tutorial][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
