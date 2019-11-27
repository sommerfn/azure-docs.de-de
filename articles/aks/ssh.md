---
title: Zugreifen per SSH auf Azure Kubernetes Service-Clusterknoten (AKS)
description: Erfahren Sie, wie Sie eine SSH-Verbindung mit Azure Kubernetes Service-Clusterknoten (AKS) zur Problembehandlung und für Wartungsaufgaben erstellen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 07/31/2019
ms.author: mlearned
ms.openlocfilehash: d855e7a65b7e1ad24dcfc4fe6a6d5e02f9004bb0
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089543"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Herstellen einer SSH-Verbindung mit Azure Kubernetes Service-Clusterknoten (AKS) zur Wartung oder Problembehandlung

Während des Lebenszyklus des Azure Kubernetes Service-Clusters (AKS) müssen Sie möglicherweise auf einen AKS-Knoten zugreifen. Dieser Zugriff kann zur Wartung, Protokollsammlung oder für andere Vorgänge der Problembehandlung erforderlich sein. Sie können mithilfe von SSH (Secure Shell) auf AKS-Knoten zugreifen, einschließlich Windows Server-Knoten (zurzeit in der Vorschauversion in AKS). Außerdem können Sie [RDP-Verbindungen (Remotedesktopprotokoll) zum Herstellen einer Verbindung mit Windows Server-Knoten verwenden][aks-windows-rdp]. Aus Sicherheitsgründen werden die AKS-Knoten nicht im Internet verfügbar gemacht. Zum Herstellen einer SSH-Verbindung mit den AKS-Knoten verwenden Sie die private IP-Adresse.

In diesem Artikel wird gezeigt, wie Sie eine SSH-Verbindung mit einem AKS-Knoten über die privaten IP-Adressen erstellen.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Standardmäßig werden SSH-Schlüssel abgerufen oder generiert und dann beim Erstellen eines AKS-Clusters den Knoten hinzugefügt. In diesem Artikel wird veranschaulicht, wie Sie andere SSH-Schlüssel als die Schlüssel angeben, die Sie beim Erstellen Ihres AKS-Clusters verwendet haben. Außerdem wird im Artikel veranschaulicht, wie Sie die private IP-Adresse Ihres Knotens ermitteln und eine SSH-Verbindung damit herstellen. Wenn Sie keinen anderen SSH-Schlüssel angeben müssen, können Sie den Schritt zum Hinzufügen des öffentlichen SSH-Schlüssels zum Knoten überspringen.

In diesem Artikel wird auch vorausgesetzt, dass Sie über einen SSH-Schlüssel verfügen. Sie können einen SSH-Schlüssel mit [macOS oder Linux][ssh-nix] bzw. [Windows][ssh-windows] erstellen. Speichern Sie das Schlüsselpaar in einem OpenSSH-Format anstelle des PuTTY-Standardformats für private Schlüssel, wenn Sie PuttyGen zum Erstellen des Schlüsselpaars verwenden.

Außerdem muss mindestens die Version 2.0.64 der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie  `az --version` aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="configure-virtual-machine-scale-set-based-aks-clusters-for-ssh-access"></a>Konfigurieren von auf VM-Skalierungsgruppen basierenden AKS-Clustern für SSH-Zugriff

Suchen Sie nach dem Namen der VM-Skalierungsgruppe Ihres Clusters, und fügen Sie Ihren öffentlichen SSH-Schlüssel dieser Skalierungsgruppe hinzu, um Ihren auf VM-Skalierungsgruppen basierenden Cluster für den SSH-Zugriff zu konfigurieren.

Verwenden Sie den Befehl [az aks show][az-aks-show], um den Ressourcengruppennamen Ihres AKS-Clusters abzurufen, und anschließend den Befehl [az vmss list][az-vmss-list], um den Namen Ihrer Skalierungsgruppe abzurufen.

```azurecli-interactive
CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
SCALE_SET_NAME=$(az vmss list --resource-group $CLUSTER_RESOURCE_GROUP --query [0].name -o tsv)
```

Im obigen Beispiel wird der Name der Clusterressourcengruppe für *myAKSCluster* in *myResourceGroup* dem Element *CLUSTER_RESOURCE_GROUP* zugewiesen. Im Beispiel wird *CLUSTER_RESOURCE_GROUP* dann verwendet, um den Namen der Skalierungsgruppe aufzulisten und *SCALE_SET_NAME* zuzuweisen.  

> [!IMPORTANT]
> Zu diesem Zeitpunkt sollten Sie nur Ihre SSH-Schlüssel für Ihre auf VM-Skalierungsgruppen basierenden AKS-Cluster mithilfe der Azure CLI aktualisieren.
> 
> Für Linux-Knoten können SSH-Schlüssel derzeit nur mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI) hinzugefügt werden. Wenn Sie per SSH eine Verbindung mit einem Windows Server-Knoten herstellen möchten, verwenden Sie die SSH-Schlüssel, die Sie beim Erstellen des AKS-Clusters bereitgestellt haben. Überspringen Sie dann die nächsten Befehle zum Hinzufügen Ihres öffentlichen SSH-Schlüssels. Sie benötigen trotzdem noch die IP-Adresse des Knotens, für den Sie die Problembehandlung durchführen möchten. Sie ist im letzten Befehl dieses Abschnitts angegeben. Alternativ können Sie eine [Verbindung mit Windows Server-Knoten über RDP-Verbindungen (Remotedesktopprotokoll) herstellen][aks-windows-rdp], anstatt über SSH.

Verwenden Sie die Befehle [az vmss extension set][az-vmss-extension-set] und [az vmss update-instances][az-vmss-update-instances], um Ihre SSH-Schlüssel den Knoten in einer VM-Skalierungsgruppe hinzuzufügen.

```azurecli-interactive
az vmss extension set  \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --vmss-name $SCALE_SET_NAME \
    --name VMAccessForLinux \
    --publisher Microsoft.OSTCExtensions \
    --version 1.4 \
    --protected-settings "{\"username\":\"azureuser\", \"ssh_key\":\"$(cat ~/.ssh/id_rsa.pub)\"}"

az vmss update-instances --instance-ids '*' \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --name $SCALE_SET_NAME
```

Im obigen Beispiel werden die Variablen *CLUSTER_RESOURCE_GROUP* und *SCALE_SET_NAME* aus den vorherigen Befehlen verwendet. Außerdem wird im obigen Beispiel auch *~/.ssh/id_rsa.pub* als Speicherort für Ihren öffentlichen SSH-Schlüssel verwendet.

> [!NOTE]
> Der Benutzername für die AKS-Knoten lautet standardmäßig *azureuser*.

Nachdem Sie Ihren öffentlichen SSH-Schlüssel der Skalierungsgruppe hinzugefügt haben, können Sie über die zugehörige IP-Adresse eine SSH-Verbindung mit dem virtuellen Knotencomputer dieser Skalierungsgruppe herstellen. Zeigen Sie die privaten IP-Adressen der Knoten des AKS-Clusters an, indem Sie den [kubectl get-Befehl][kubectl-get] verwenden.

```console
kubectl get nodes -o wide
```

Die folgende Beispielausgabe zeigt die interne IP-Adresse aller Knoten in dem Cluster einschließlich eines Windows Server-Knotens an.

```console
$ kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Notieren Sie zur Problembehandlung die interne IP-Adresse des Knotens.

Führen Sie die Schritte unter [Erstellen der SSH-Verbindung](#create-the-ssh-connection) aus, um per SSH auf Ihren Knoten zuzugreifen.

## <a name="configure-virtual-machine-availability-set-based-aks-clusters-for-ssh-access"></a>Konfigurieren von auf VM-Verfügbarkeitsgruppen basierenden AKS-Clustern für SSH-Zugriff

Suchen Sie nach dem Namen des Linux-Knotens Ihres Clusters, und fügen Sie diesem Knoten Ihren öffentlichen SSH-Schlüssel hinzu, um Ihren auf einer VM-Verfügbarkeitsgruppe basierenden AKS-Cluster für den SSH-Zugriff zu konfigurieren.

Verwenden Sie den Befehl [az aks show][az-aks-show], um den Ressourcengruppennamen Ihres AKS-Clusters abzurufen, und dann den Befehl [az vm list][az-vm-list], um den VM-Namen des Linux-Knotens Ihres Clusters aufzulisten.

```azurecli-interactive
CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az vm list --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

Im obigen Beispiel wird der Name der Clusterressourcengruppe für *myAKSCluster* in *myResourceGroup* dem Element *CLUSTER_RESOURCE_GROUP* zugewiesen. Im Beispiel wird *CLUSTER_RESOURCE_GROUP* dann verwendet, um den Namen des virtuellen Computers aufzulisten. In der Beispielausgabe wird der Name des virtuellen Computers angezeigt: 

```
Name                      ResourceGroup                                  Location
------------------------  ---------------------------------------------  ----------
aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
```

Zum Hinzufügen Ihrer SSH-Schlüssel zum Knoten verwenden Sie den Befehl [az vm user update][az-vm-user-update].

```azurecli-interactive
az vm user update \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --name aks-nodepool1-79590246-0 \
    --username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Im obigen Beispiel werden die Variable *CLUSTER_RESOURCE_GROUP* und der Name des virtuellen Knotencomputers aus den vorherigen Befehlen verwendet. Außerdem wird im obigen Beispiel auch *~/.ssh/id_rsa.pub* als Speicherort für Ihren öffentlichen SSH-Schlüssel verwendet. Sie können auch den Inhalt Ihres öffentlichen SSH-Schlüssels verwenden, anstatt einen Pfad anzugeben.

> [!NOTE]
> Der Benutzername für die AKS-Knoten lautet standardmäßig *azureuser*.

Nachdem Sie den öffentlichen SSH-Schlüssel dem virtuellen Knotencomputer hinzugefügt haben, können Sie eine SSH-Verbindung mit diesem virtuellen Computer herstellen, indem Sie dessen IP-Adresse verwenden. Zeigen Sie die private IP-Adresse eines AKS-Clusterknotens mit dem Befehl [az vm list-ip-addresses][az-vm-list-ip-addresses] an.

```azurecli-interactive
az vm list-ip-addresses --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

Im obigen Beispiel wird die Variable *CLUSTER_RESOURCE_GROUP* verwendet, die in den vorherigen Befehlen festgelegt wurde. Die folgende Beispielausgabe zeigt die privaten IP-Adressen der AKS-Knoten:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

## <a name="create-the-ssh-connection"></a>Erstellen der SSH-Verbindung

Zum Erstellen einer SSH-Verbindung mit einem AKS-Knoten führen Sie einen Hilfspod in Ihrem AKS-Cluster aus. Dieser Hilfspod bietet SSH-Zugriff auf den Cluster und dann zusätzlichen SSH-Knotenzugriff. Zum Erstellen und Verwenden dieses Hilfspods führen Sie die folgenden Schritte aus:

1. Führen Sie ein `debian`-Containerimage aus, und fügen Sie eine Terminalsitzung daran an. Dieser Container kann zum Erstellen einer SSH-Sitzung mit einem beliebigen Knoten im AKS-Cluster verwendet werden:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

    > [!TIP]
    > Wenn Sie Windows Server-Knoten verwenden (derzeit als Vorschauversion in AKS), müssen Sie dem Befehl einen Knotenselektor hinzufügen, um die Ausführung eines Debian-Containers auf einem Linux-Knoten zu planen:
    >
    > `kubectl run -it --rm aks-ssh --image=debian --overrides='{"apiVersion":"apps/v1","spec":{"template":{"spec":{"nodeSelector":{"beta.kubernetes.io/os":"linux"}}}}}'`

1. Installieren Sie mit `apt-get` einen SSH-Client, sobald die Terminalsitzung mit dem Container verbunden ist:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. Öffnen Sie ein neues Terminalfenster, für das keine Verbindung mit Ihrem Container besteht, und listen Sie die Pods Ihres AKS-Clusters mit dem Befehl [kubectl get pods][kubectl-get] auf. Der im vorherigen Schritt erstellte Pod beginnt mit dem Namen *aks-ssh*, wie es im folgenden Beispiel gezeigt wird:

    ```
    $ kubectl get pods
    
    NAME                       READY     STATUS    RESTARTS   AGE
    aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
    ```

1. In einem vorherigen Schritt haben Sie Ihren öffentlichen SSH-Schlüssel dem AKS-Knoten hinzugefügt, für den Sie die Problembehandlung durchführen möchten. Kopieren Sie Ihren privaten SSH-Schlüssel jetzt auf den Hilfspod. Dieser private Schlüssel wird verwendet, um die SSH-Verbindung mit dem AKS-Knoten herzustellen.

    Geben Sie Ihren eigenen *aks-ssh*-Podnamen an, den Sie im vorherigen Schritt erhalten haben. Ändern Sie bei Bedarf *~/.ssh/id_rsa* in den Speicherort Ihres privaten SSH-Schlüssels:

    ```console
    kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
    ```

1. Wechseln Sie zurück zur Terminalsitzung für Ihren Container, und aktualisieren Sie die Berechtigungen für den kopierten privaten SSH-Schlüssel `id_rsa`, damit er für Benutzer schreibgeschützt ist:

    ```console
    chmod 0600 id_rsa
    ```

1. Erstellen Sie eine SSH-Verbindung mit Ihrem AKS-Knoten. Auch hier lautet der Standardbenutzername für AKS-Knoten *azureuser*. Bestätigen Sie die Aufforderung zum Fortsetzen der Verbindung, da zuerst die Vertrauenswürdigkeit des SSH-Schlüssels überprüft wird. Anschließend wird die Bash-Eingabeaufforderung Ihres AKS-Knotens angezeigt:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4
    
    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.
    
    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud
    
    [...]
    
    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>Entfernen des SSH-Zugriffs

Wenn Sie fertig sind, führen Sie `exit` für die SSH-Sitzung und dann `exit` für die interaktive Containersitzung aus. Beim Schließen dieser Containersitzung wird der Pod, der für den SSH-Zugriff vom AKS-Cluster verwendet wurde, gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Falls Sie weitere Daten für die Problembehandlung benötigen, können Sie die [Kubelet-Protokolle anzeigen][view-kubelet-logs] oder die [Kubernetes-Masterknotenprotokolle anzeigen][view-master-logs].

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-windows-rdp]: rdp.md
[ssh-nix]: ../virtual-machines/linux/mac-create-ssh-keys.md
[ssh-windows]: ../virtual-machines/linux/ssh-from-windows.md
[az-vmss-list]: /cli/azure/vmss#az-vmss-list
[az-vmss-extension-set]: /cli/azure/vmss/extension#az-vmss-extension-set
[az-vmss-update-instances]: /cli/azure/vmss#az-vmss-update-instances
