---
title: Verwenden mehrerer Knotenpools in Azure Kubernetes Service (AKS)
description: Informationen zum Erstellen und Verwalten mehrerer Knotenpools für einen Cluster in Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 08/9/2019
ms.author: mlearned
ms.openlocfilehash: 8a78c854e9c842915700d4a20c1a57e4f1594a2e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472451"
---
# <a name="create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Erstellen und Verwalten mehrerer Knotenpools für einen Cluster in Azure Kubernetes Service (AKS)

Im Azure Kubernetes Service (AKS) werden Knoten derselben Konfiguration zu *Knotenpools* zusammengefasst. Diese Knotenpools enthalten die zugrunde liegenden virtuellen Computer, die Ihre Anwendungen ausführen. Die anfängliche Anzahl der Knoten und ihre Größe (SKU) werden beim Erstellen eines AKS-Clusters festgelegt, der einen *Standardknotenpool* erstellt. Sie können zusätzliche Knotenpools erstellen, um Anwendungen mit unterschiedlichen Compute- oder Speicheranforderungen zu unterstützen. Verwenden Sie diese zusätzlichen Knotenpools z. B. zum Bereitstellen von GPUs für rechenintensive Anwendungen oder für den Zugriff auf leistungsstarken SSD-Speicher.

> [!NOTE]
> Diese Funktion ermöglicht eine höhere Kontrolle über das Erstellen und Verwalten mehrerer Knotenpools. Daher sind separate Befehle zum Erstellen, Aktualisieren und Löschen erforderlich. Für über `az aks create` oder `az aks update` ausgeführte Clustervorgänge wurde bisher die managedCluster-API verwendet, und diese Vorgänge stellten die einzige Möglichkeit zum Ändern der Steuerungsebene und eines einzelnen Knotenpools dar. Diese Funktion stellt einen separaten Vorgang für Agent-Pools über die agentPool-API zur Verfügung und erfordert die Verwendung des `az aks nodepool`-Befehlssatzes zum Ausführen von Vorgängen für einen einzelnen Knotenpool.

In diesem Artikel erfahren Sie, wie Sie mehrere Knotenpools in einem AKS-Cluster erstellen und verwalten.

## <a name="before-you-begin"></a>Voraussetzungen

Azure CLI-Version 2.0.76 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][install-azure-cli].

## <a name="limitations"></a>Einschränkungen

Die folgenden Einschränkungen gelten für die Erstellung und Verwaltung von AKS-Clustern, die mehrere Knotenpools unterstützen:

* Der Standardknotenpool (der erste) kann nicht gelöscht werden.
* Das Add-On für das HTTP-Anwendungsrouting kann nicht verwendet werden.
* Sie können Knotenpools nicht mit einer vorhandenen Resource Manager-Vorlage hinzufügen oder löschen wie mit den meisten Vorgängen. Stattdessen [verwenden Sie eine gesonderte Resource Manager-Vorlage](#manage-node-pools-using-a-resource-manager-template), um Änderungen an Knotenpools in einem AKS-Cluster vorzunehmen.
* Der Name eines Knotenpools muss mit einem Kleinbuchstaben beginnen und darf nur alphanumerische Zeichen enthalten. Bei Linux-Knotenpools muss die Länge zwischen einem und zwölf Zeichen liegen. Bei Windows-Knotenpools muss die Länge zwischen einem und sechs Zeichen betragen.
* Der AKS-Cluster kann maximal acht Knotenpools umfassen.
* Der AKS-Cluster kann maximal 400 Knoten in diesen acht Knotenpools enthalten.
* Alle Knotenpools müssen sich in demselben Subnetz befinden.
* Der AKS-Cluster muss VM-Skalierungsgruppen für die Knoten verwenden.

## <a name="create-an-aks-cluster"></a>Erstellen eines AKS-Clusters

Erstellen Sie zu Beginn einen AKS-Cluster mit einem einzelnen Knotenpool. Im folgenden Beispiel wird der Befehl [az group create][az-group-create] verwendet, um eine Ressourcengruppe namens *myResourceGroup* in der Region *eastus* zu erstellen. Anschließend wird mit dem Befehl [az aks create][az-aks-create] ein AKS-Cluster mit dem Namen *myAKSCluster* erstellt. *--kubernetes-version* *1.13.10* wird verwendet, um die Aktualisierung eines Knotenpools in einem nachfolgenden Schritt zu veranschaulichen. Sie können eine beliebige [unterstützte Kubernetes-Version][supported-versions] angeben.

> [!NOTE]
> Die Load Balancer-SKU *Basic* wird bei Verwendung mehrerer Knotenpools nicht unterstützt. Standardmäßig werden AKS-Cluster mit der Load Balancer-SKU *Standard* erstellt.

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 2 \
    --generate-ssh-keys \
    --kubernetes-version 1.13.10 \
    --load-balancer-sku standard
```

Die Erstellung des Clusters dauert einige Minuten.

> [!NOTE]
> Um sicherzustellen, dass Ihr Cluster zuverlässig funktioniert, sollten Sie mindestens zwei (2) Knoten im Standardknotenpool ausführen, da wichtige Systemdienste in diesem Knotenpool ausgeführt werden.

Wenn der Cluster bereit ist, verwenden Sie den Befehl [az aks get-credentials][az-aks-get-credentials], um die Clusteranmeldeinformationen für die Verwendung mit `kubectl` zu erhalten:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Hinzufügen eines Knotenpools

Der im vorherigen Schritt erstellte Cluster verfügt über einen einzelnen Knotenpool. Fügen Sie als Nächstes einen zweiten Knotenpool mit dem Befehl [az aks nodepool add][az-aks-nodepool-add] hinzu. Das folgende Beispiel erstellt einen Knotenpool namens *mynodepool*, der *3* Knoten ausführt:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --kubernetes-version 1.12.7
```

> [!NOTE]
> Der Name eines Knotenpools muss mit einem Kleinbuchstaben beginnen und darf nur alphanumerische Zeichen enthalten. Bei Linux-Knotenpools muss die Länge zwischen einem und zwölf Zeichen liegen. Bei Windows-Knotenpools muss die Länge zwischen einem und sechs Zeichen betragen.

Um den Status Ihrer Knotenpools anzuzeigen, verwenden Sie den Befehl [az aks node pool list][az-aks-nodepool-list], und geben Sie Ihre Ressourcengruppe und den Clusternamen an:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster
```

Die folgende Beispielausgabe zeigt, dass *mynodepool* erfolgreich mit drei Knoten im Knotenpool erstellt wurde. Wenn der AKS-Cluster im vorherigen Schritt erstellt wurde, dann wurde ein Standardknotenpool (*nodepool1*) mit *2* Knoten erstellt.

```console
$ az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.12.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

> [!TIP]
> Wenn beim Hinzufügen eines Knotenpools kein *OrchestratorVersion* oder *VmSize* angegeben wurde, werden die Knoten basierend auf den Standardwerten für den AKS-Cluster erstellt. In diesem Beispiel wurden die Kubernetes-Version *1.13.10* und die Knotengröße *Standard_DS2_v2* verwendet.

## <a name="upgrade-a-node-pool"></a>Durchführen eines Upgrades für einen Knotenpool
 
> [!NOTE]
> Upgrade- und Skalierungsvorgänge für einen Cluster- oder Knotenpool können nicht gleichzeitig ausgeführt werden. Bei dem Versuch wird ein Fehler zurückgegeben. Stattdessen muss jeder Vorgangstyp für die Zielressource abgeschlossen sein, bevor eine neue Anforderung an dieselbe Ressource gerichtet werden kann. Weitere Informationen hierzu finden Sie in unserem [Leitfaden zur Problembehandlung](https://aka.ms/aks-pending-upgrade).

Beim anfänglichen Erstellen Ihres AKS-Clusters im ersten Schritt wurde für `--kubernetes-version` der Wert *1.13.10* angegeben. Dadurch wurde die Kubernetes-Version sowohl für die Steuerungsebene als auch für den Standardknotenpool festgelegt. Mit den Befehlen in diesem Abschnitt wird gezeigt, wie ein Upgrade für einen einzelnen spezifischen Knotenpool durchgeführt wird.

Die Beziehung zwischen dem Upgrade der Kubernetes-Version der Steuerungsebene und des Knotenpools wird im [Abschnitt weiter unten](#upgrade-a-cluster-control-plane-with-multiple-node-pools) erläutert.

> [!NOTE]
> Die Betriebssystem-Imageversion des Knotenpools ist an die Kubernetes-Version des Clusters gebunden. Upgrades für Betriebssystemimages erhalten Sie nur nach einem Clusterupgrade.

Da in diesem Beispiel zwei Knotenpools vorhanden sind, muss [az aks nodepool upgrade][az-aks-nodepool-upgrade] für das Upgrade eines Knotenpools verwendet werden. Führen Sie für *mynodepool* ein Upgrade auf Kubernetes *1.13.10* durch. Verwenden Sie den Befehl [az aks nodepool upgrade][az-aks-nodepool-upgrade], um ein Upgrade für den Knotenpool durchzuführen, wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.13.10 \
    --no-wait
```

Listen Sie den Status Ihrer Knotenpools mit dem Befehl [az aks node pool list][az-aks-nodepool-list] erneut auf. Das folgende Beispiel zeigt, dass *mynodepool* den Zustand *Upgrading* (Wird aktualisiert) auf *1.13.10* aufweist:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Upgrading",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Es dauert einige Minuten, bis ein Upgrade der Knoten auf die angegebene Version erfolgt ist.

Als bewährte Methode sollten Sie alle Knotenpools in einem AKS-Cluster auf dieselbe Kubernetes-Version aktualisieren. Beim Standardverhalten von `az aks upgrade` werden alle Knotenpools gemeinsam mit der Steuerungsebene aktualisiert, um diese Anpassung zu erzielen. Durch die Möglichkeit, ein Upgrade für einzelne Knotenpools durchzuführen, können Sie ein paralleles Upgrade durchführen und Pods zwischen Knotenpools planen, um die Anwendungsbetriebszeit innerhalb der oben genannten Einschränkungen aufrechtzuerhalten.

## <a name="upgrade-a-cluster-control-plane-with-multiple-node-pools"></a>Aktualisieren einer Clustersteuerungsebene mit mehreren Knotenpools

> [!NOTE]
> Kubernetes verwendet als Standardversionierungsschema die [semantische Versionierung](https://semver.org/). Die Versionsnummer wird im Format *x.y.z* angegeben. *x* steht dabei für die Hauptversion, *y* für die Nebenversion und *z* für die Patchversion. Ein Beispiel: Bei der Version *1.12.6* ist „1“ die Hauptversion, „12“ die Nebenversion und „6“ die Patchversion. Die Kubernetes-Version der Steuerungsebene sowie des ursprünglichen Knotenpools wird im Rahmen der Clustererstellung festgelegt. Bei allen zusätzlichen Knotenpools wird die Kubernetes-Version festgelegt, wenn sie dem Cluster hinzugefügt werden. Die Kubernetes-Versionen können sich zwischen Knotenpools sowie zwischen einem Knotenpool und der Steuerungsebene unterscheiden. Dabei gelten jedoch folgende Einschränkungen:
> 
> * Die Knotenpoolversion muss die gleiche Hauptversion haben wie die Steuerungsebene.
> * Die Knotenpoolversion kann eine Nebenversion niedriger sein als die Version der Steuerungsebene.
> * Die Knotenpoolversion kann eine beliebige Patchversion haben, solange die beiden anderen Bedingungen erfüllt sind.

Ein AKS-Cluster verfügt über zwei Clusterressourcenobjekte mit zugeordneten Kubernetes-Versionen. Beim ersten Objekt handelt es sich um eine Kubernetes-Version der Steuerungsebene. Das zweite ist ein Agent-Pool mit einer Kubernetes-Version. Eine Steuerungsebene ist einem oder mehreren Knotenpools zugeordnet. Das Verhalten eines Upgradevorgangs hängt davon ab, welcher Azure CLI-Befehl verwendet wird.

* Zum Aktualisieren der Steuerungsebene muss `az aks upgrade` verwendet werden.
   * Dadurch werden die Version der Steuerungsebene und alle Knotenpools im Cluster aktualisiert.
   * Beim Übergeben von `az aks upgrade` mit dem Flag `--control-plane-only` wird nur die Clustersteuerungsebene aktualisiert und keiner der zugeordneten Knotenpools geändert.
* Zum Aktualisieren einzelner Knotenpools muss `az aks nodepool upgrade` verwendet werden.
   * Dadurch wird nur der Zielknotenpool mit der angegebenen Kubernetes-Version aktualisiert.

Die Beziehung zwischen Kubernetes-Versionen von Knotenpools muss auch einem Satz von Regeln folgen.

* Sie können weder die Kubernetes-Version der Steuerungsebene noch die des Knotenpools herabstufen.
* Wenn die Kubernetes-Version eines Knotenpools nicht angegeben ist, hängt das Verhalten vom verwendeten Client ab. Bei der Deklaration in der Resource Manager-Vorlage wird die für den Knotenpool definierte vorhandene Version verwendet. Wenn kein Wert festgelegt ist, wird die Version der Steuerungsebene verwendet.
* Sie können eine Steuerungsebene oder einen Knotenpool zu einem bestimmten Zeitpunkt aktualisieren oder skalieren. Die beiden Vorgänge können jedoch nicht gleichzeitig übermittelt werden.
* Die Kubernetes-Version eines Knotenpools muss der Hauptversion der Steuerungsebene entsprechen.
* Die Kubernetes-Version eines Knotenpools darf maximal zwei (2) Versionen niedriger sein als die der Steuerungsebene, aber niemals höher.
* Die Kubernetes-Patchversion eines Knotenpools kann niedriger sein als die Version der Steuerungsebene oder dieser entsprechen, aber sie darf niemals höher sein.

## <a name="scale-a-node-pool-manually"></a>Manuelles Skalieren eines Knotenpools

Wenn sich die Anforderungen der Workloads Ihrer Anwendungen ändern, müssen Sie möglicherweise die Anzahl der Knoten in einem Knotenpool skalieren. Die Anzahl der Knoten kann erhöht oder verringert werden.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Um die Anzahl der Knoten in einem Knotenpool zu skalieren, verwenden Sie den Befehl [az aks node pool scale][az-aks-nodepool-scale]. Das folgende Beispiel skaliert die Anzahl der Knoten in *mynodepool* auf *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Listen Sie den Status Ihrer Knotenpools mit dem Befehl [az aks node pool list][az-aks-nodepool-list] erneut auf. Das folgende Beispiel zeigt, dass *mynodepool* den Zustand *Scaling* (Wird skaliert) mit einer neuen Anzahl von *5* Knoten aufweist:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Scaling",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Es dauert einige Minuten, bis die Skalierung abgeschlossen ist.

## <a name="scale-a-specific-node-pool-automatically-by-enabling-the-cluster-autoscaler"></a>Automatisches Skalieren eines bestimmten Knotenpools durch Aktivieren der automatischen Clusterskalierung

AKS bietet eine separate Funktion zum automatischen Skalieren von Knotenpools mit einem als [automatische Clusterskalierung](cluster-autoscaler.md) bezeichneten Feature. Das Feature kann mit einer eindeutigen minimalen und maximalen Anzahl von Skalierungen pro Knotenpool aktiviert werden. Erfahren Sie, wie Sie [die automatische Clusterskalierung pro Knotenpool verwenden](cluster-autoscaler.md#use-the-cluster-autoscaler-with-multiple-node-pools-enabled).

## <a name="delete-a-node-pool"></a>Löschen eines Knotenpools

Wenn Sie einen Pool nicht mehr benötigen, können Sie ihn löschen und die zugrunde liegenden VM-Knoten entfernen. Um einen Knotenpool zu löschen, verwenden Sie den Befehl [az aks node pool delete][az-aks-nodepool-delete], und geben Sie den Namen des Knotenpools an. Das folgende Beispiel löscht den in den vorherigen Schritten erstellten Knotenpool *mynodepool*:

> [!CAUTION]
> Es gibt keine Wiederherstellungsoptionen für Datenverluste, die beim Löschen eines Knotenpools auftreten können. Wenn Pods nicht in anderen Knotenpools geplant werden können, sind diese Anwendungen nicht verfügbar. Stellen Sie sicher, dass Sie einen Knotenpool nicht löschen, wenn aktive Anwendungen keine Datensicherungen aufweisen oder nicht in anderen Knotenpools in Ihrem Cluster ausgeführt werden können.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

Die folgende Beispielausgabe des Befehls [az aks node pool list][az-aks-nodepool-list] zeigt, dass sich *mynodepool* im Zustand *Deleting* (Wird gelöscht) befindet:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Deleting",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Das Löschen der Knoten und des Knotenpools dauert einige Minuten.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Angeben einer VM-Größe für einen Knotenpool

In den vorherigen Beispielen zur Erstellung eines Knotenpools wurde für die im Cluster erstellten Knoten eine VM-Standardgröße verwendet. Ein üblicheres Szenario ist es, Knotenpools mit unterschiedlichen VM-Größen und -Funktionen zu erstellen. Sie können z. B. einen Knotenpool erstellen, der Knoten mit einer großen Anzahl an CPUs oder mit viel Arbeitsspeicher enthält, oder einen Knotenpool, der GPU-Unterstützung bietet. Im nächsten Schritt teilen Sie dem Kubernetes-Planer durch [Verwenden von Taints und Toleranzen](#schedule-pods-using-taints-and-tolerations) mit, wie Sie den Zugriff auf Pods, die auf diesen Knoten ausgeführt werden können, einschränken können.

Erstellen Sie im folgenden Beispiel einen GPU-basierten Knotenpool, der die VM-Größe *Standard_NC6* verwendet. Diese virtuellen Computer werden von der NVIDIA Tesla K80-Karte unterstützt. Informationen zu den verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure][vm-sizes].

Erstellen Sie erneut einen Knotenpool mit dem Befehl [az aks node pool add][az-aks-nodepool-add]. Geben Sie diesmal den Namen *gpunodepool* und mit dem Parameter `--node-vm-size` die Größe *Standard_NC6* an:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

Die folgende Beispielausgabe des Befehls [az aks node pool list][az-aks-nodepool-list] zeigt, dass *gpunodepool* Knoten mit der angegebenen *VmSize* (VM-Größe) *erstellt*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "gpunodepool",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Creating",
    ...
    "vmSize": "Standard_NC6",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.13.10",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Es dauert einige Minuten, bis *gpunodepool* erfolgreich erstellt wurde.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Planen von Pods mit Taints und Toleranzen

Sie verfügen jetzt über zwei Knotenpools in Ihrem Cluster – den ursprünglich erstellten Standardknotenpool und den GPU-basierten Knotenpool. Verwenden Sie den Befehl [kubectl get nodes][kubectl-get], um die Knoten in Ihrem Cluster anzuzeigen. Die folgende Beispielausgabe zeigt die Knoten:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.13.10
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.13.10
```

Der Kubernetes-Planer kann Taints und Toleranzen verwenden, um einzuschränken, welche Workloads auf Knoten ausgeführt werden können.

* Ein **Taint** wird auf einen Knoten angewendet, der anzeigt, dass nur bestimmte Pods darauf geplant werden können.
* Für den Pod wird anschließend eine **Toleranz** angewendet, damit dieser den Taint des Knotens *tolerieren* kann.

Weitere Informationen zur Verwendung der erweiterten geplanten Kubernetes-Features finden Sie unter [Best Practices für erweiterte Scheduler-Funktionen in Azure Kubernetes Service (AKS)][taints-tolerations].

In diesem Beispiel wenden Sie mit dem Befehl [kubectl taint node][kubectl-taint] einen Taint auf Ihren GPU-basierten Knoten an. Geben Sie den Namen Ihres GPU-basierten Knotens aus der Ausgabe des vorherigen `kubectl get nodes` Befehls an. Der Taint wird als *Schlüssel:Wert* und dann eine Planungsoption angewendet. Das folgende Beispiel verwendet das Paar *sku=gpu* und definiert Pods, die ansonsten die Option *NoSchedule* aufweisen:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

Das folgende einfache YAML-Beispielmanifest verwendet eine Toleranz, damit der Kubernetes-Planer einen NGINX-Pod auf dem GPU-basierten Knoten ausführen kann. Ein geeigneteres, aber zeitintensiveres Beispiel für die Ausführung eines Tensorflow-Auftrags für den MNIST-Datensatz finden Sie unter [Verwenden von GPUs für computeintensive Workloads in Azure Kubernetes Service (AKS)][gpu-cluster].

Erstellen Sie eine Datei mit dem Namen `gpu-toleration.yaml`, und fügen Sie den folgenden YAML-Beispielcode ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Planen Sie den Pod mit dem Befehl `kubectl apply -f gpu-toleration.yaml`:

```console
kubectl apply -f gpu-toleration.yaml
```

Es dauert einige Sekunden, um den Pod zu planen und das NGINX-Image per Pull abzurufen. Verwenden Sie den Befehl [kubectl describe pod][kubectl-describe], um den Podstatus anzuzeigen. Die folgende kompakte Beispielausgabe zeigt, dass die Toleranz *sku=gpu:NoSchedule* angewendet wird. Im Ereignisabschnitt hat der Planer den Pod dem GPU-basierten Knoten *aks-gpunodepool-28993262-vmss000000* zugewiesen:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Nur Pods mit diesem Taint können auf Knoten in *gpunodepool* geplant werden. Alle anderen Pods werden im Knotenpool *nodepool1* geplant. Wenn Sie weitere Knotenpools erstellen, können Sie mit zusätzlichen Taints und Toleranzen einschränken, welche Pods für diese Knotenressourcen geplant werden können.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Verwalteten von Knotenpools mithilfe einer Resource Manager-Vorlage

Wenn Sie eine Azure Resource Manager-Vorlage zum Erstellen und Verwalten von Ressourcen verwenden, können Sie die Einstellungen in Ihrer Vorlage normalerweise aktualisieren und neu bereitstellen, um die Ressource zu aktualisieren. Bei Knotenpools in AKS kann das Profil des Anfangsknotenpools nicht mehr aktualisiert werden, nachdem der AKS-Cluster erstellt wurde. Dieses Verhalten bedeutet, dass Sie eine vorhandene Resource Manager-Vorlage nicht aktualisieren können, um eine Änderung an den Knotenpools vorzunehmen und dann neu bereitzustellen. Stattdessen müssen Sie eine separate Resource Manager-Vorlage erstellen, die nur die Agent-Pools für einen vorhandenen AKS-Cluster aktualisiert.

Erstellen Sie eine Vorlage wie `aks-agentpools.json`, und fügen Sie das folgende Beispielmanifest ein. Diese Beispielvorlage konfiguriert die folgenden Einstellungen:

* Aktualisiert den *Linux*-Agent-Pool namens *myagentpool*, sodass dieser drei Knoten ausführt.
* Legt die Knoten im Knotenpool auf die Ausführung von Kubernetes-Version *1.13.10* fest.
* Definiert die Knotengröße als *Standard_DS2_v2*.

Bearbeiten Sie diese Werte nach Bedarf, um Knotenpools nach Bedarf zu aktualisieren, hinzuzufügen oder zu löschen:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of your existing AKS cluster."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "The location of your existing AKS cluster."
            }
        },
        "agentPoolName": {
            "type": "string",
            "defaultValue": "myagentpool",
            "metadata": {
                "description": "The name of the agent pool to create or update."
            }
        },
        "vnetSubnetId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The Vnet subnet resource ID for your existing AKS cluster."
            }
        }
    },
    "variables": {
        "apiVersion": {
            "aks": "2019-04-01"
        },
        "agentPoolProfiles": {
            "maxPods": 30,
            "osDiskSizeGB": 0,
            "agentCount": 3,
            "agentVmSize": "Standard_DS2_v2",
            "osType": "Linux",
            "vnetSubnetId": "[parameters('vnetSubnetId')]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.ContainerService/managedClusters/agentPools",
            "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
            "location": "[parameters('location')]",
            "properties": {
                "maxPods": "[variables('agentPoolProfiles').maxPods]",
                "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
                "count": "[variables('agentPoolProfiles').agentCount]",
                "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
                "osType": "[variables('agentPoolProfiles').osType]",
                "storageProfile": "ManagedDisks",
                "type": "VirtualMachineScaleSets",
                "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
                "orchestratorVersion": "1.13.10"
            }
        }
    ]
}
```

Stellen Sie diese Vorlage mithilfe des Befehls [az group deployment create][az-group-deployment-create] bereit, wie im folgenden Beispiel gezeigt. Sie werden zur Eingabe des Namens und Speicherorts des vorhandenen AKS-Clusters aufgefordert:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Es kann ein paar Minuten dauern, bis Ihr AKS-Cluster aktualisiert wird, abhängig von den Knoteneinstellungen und Vorgängen, die Sie in Ihrer Resource Manager-Vorlage definieren.

## <a name="assign-a-public-ip-per-node-in-a-node-pool"></a>Zuweisen einer öffentlichen IP-Adresse pro Knoten in einem Knotenpool

> [!WARNING]
> Während sich die Funktion zur Zuweisung einer öffentlichen IP-Adresse pro Knoten in der Vorschau befindet, kann sie nicht mit *Load Balancern der Standard-SKU in AKS* verwendet werden, weil Load Balancer-Regeln möglicherweise mit der VM-Bereitstellung in Konflikt stehen. In der Vorschauversion müssen Sie die *Load-Balancer-SKU „Basic“* verwenden, wenn Sie eine öffentliche IP-Adresse pro Knoten zuweisen müssen.

AKS-Knoten benötigen keine eigene öffentliche IP-Adresse für die Kommunikation. In einigen Szenarien müssen Knoten in einem Knotenpool jedoch möglicherweise jeweils über eine eigene öffentliche IP-Adresse verfügen. Ein Beispiel hierfür ist Gaming, bei dem eine Konsole eine direkte Verbindung mit einem virtuellen Cloudcomputer herstellen muss, um Hops zu minimieren. Dies kann erreicht werden, indem Sie sich für eine separate Previewfunktion für öffentliche IP-Adressen für Knoten (Node Public IP) (Vorschauversion) registrieren.

```azurecli-interactive
az feature register --name NodePublicIPPreview --namespace Microsoft.ContainerService
```

Stellen Sie nach der erfolgreichen Registrierung eine Azure Resource Manager-Vorlage bereit. Folgen Sie dazu den [obigen](#manage-node-pools-using-a-resource-manager-template) Anweisungen, und fügen Sie die folgende Eigenschaft (boolescher Wert) „enableNodePublicIP“ für die agentPoolProfiles hinzu. Sie müssen diese Eigenschaft auf `true` festlegen, weil sie standardmäßig auf `false` festgelegt wird, wenn keine Angabe erfolgt. Diese Eigenschaft wird nur zur Erstellungszeit verwendet und erfordert mindestens die API-Version 2019-06-01. Die Anwendung kann sowohl auf Linux- als auch auf Windows-Knotenpools erfolgen.

```
"agentPoolProfiles":[  
    {  
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]",
      "enableNodePublicIP":true
    }
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Artikel haben Sie einen AKS-Cluster erstellt, der GPU-basierte Knoten enthält. Um unnötige Kosten zu reduzieren, können Sie *gpunodepool* oder den gesamten AKS-Cluster löschen.

Verwenden Sie zum Löschen des GPU-basierten Knotenpools den Befehl [az aks nodepool delete][az-aks-nodepool-delete], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Wenn Sie den Cluster selbst löschen möchten, verwenden Sie den Befehl [az group delete][az-group-delete], um die AKS-Ressourcengruppe zu löschen:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie mehrere Knotenpools in einem AKS-Cluster erstellen und verwalten. Weitere Informationen zum knotenpoolübergreifenden Steuern von Pods finden Sie unter [Best Practices für erweiterte Scheduler-Funktionen in Azure Kubernetes Service (AKS)][operator-best-practices-advanced-scheduler].

Informationen zum Erstellen und Verwenden von Windows Server-Containerknotenpools finden Sie unter [Erstellen eines Windows Server-Containers in AKS][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
