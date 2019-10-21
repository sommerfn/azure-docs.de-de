---
title: Integrieren von Azure Container Registry in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Azure Kubernetes Service (AKS) und Azure Container Registry integrieren
services: container-service
author: mlearned
manager: gwallace
ms.service: container-service
ms.topic: article
ms.date: 09/17/2018
ms.author: mlearned
ms.openlocfilehash: bbd08e49256886a1df334cbf36e6e468bb8f3895
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286782"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Authentifizieren per Azure Container Registry über Azure Kubernetes Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Kubernetes Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. In diesem Artikel werden Beispiele für die Konfiguration der Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben.

Sie können die AKS- und ACR-Integration mit einigen einfachen Befehlen über die Azure-Befehlszeilenschnittstelle einrichten.

## <a name="before-you-begin"></a>Voraussetzungen

Voraussetzungen für diese Beispiele sind:

* Rolle **Besitzer** oder **Azure-Kontoadministrator** im **Azure-Abonnement**
* Azure CLI, Version 2.0.73 oder höher

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Erstellen eines neuen AKS-Clusters mit ACR-Integration

Sie können die AKS- und ACR-Integration während der erstmaligen Erstellung Ihres AKS-Clusters einrichten.  Damit ein AKS-Cluster mit ACR interagieren kann, wird ein Azure Active Directory-**Dienstprinzipal** verwendet. Mit dem folgenden CLI-Befehl können Sie eine vorhandene ACR-Instanz in Ihrem Abonnement autorisieren und die entsprechende **ACRPull**-Rolle für den Dienstprinzipal konfigurieren. Geben Sie gültige Werte für die unten stehenden Parameter an.  Die Parameter in eckigen Klammern sind optional.
```azurecli
# set this to the name of your Azure Container Registry.  It must be globally unique
MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --attach-acr $MYACR

```
Alternativ können Sie den ACR-Namen mithilfe einer ACR-Ressourcen-ID angeben, die das folgende Format aufweist:

/subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.ContainerRegistry/registries/\<Name\> 
 
```azurecli
az aks create -n myAKSCluster -g myResourceGroup --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

Dieser Schritt kann mehrere Minuten in Anspruch nehmen.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Konfigurieren der ACR-Integration für vorhandene AKS-Cluster

Integrieren Sie eine vorhandene ACR-Instanz in vorhandene AKS-Cluster, indem Sie wie unten gezeigt gültige Werte für **acr-name** oder **acr-resource-id** angeben.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
```
oder
```
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Sie können die Integration zwischen einer ACR und einem AKS-Cluster auch folgendermaßen entfernen:
```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
```
oder
```
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

## <a name="working-with-acr--aks"></a>Arbeiten mit ACR und AKS

### <a name="import-an-image-into-your-acr"></a>Importieren eines Images in Ihre ACR

Sie importieren ein Image aus dem Docker-Hub in Ihre ACR, indem Sie Folgendes ausführen:


```azurecli
az acr import  -n <myContainerRegistry> --source docker.io/library/nginx:latest --image nginx:v1
```

### <a name="deploy-the-sample-image-from-acr-to-aks"></a>Bereitstellen des Beispielimages aus ACR in AKS

Sicherstellen, dass Sie über die richtigen AKS-Anmeldeinformationen verfügen

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Erstellen Sie eine Datei namens **acr-nginx.yaml**, die Folgendes enthält:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80
```

Führen Sie als Nächstes diese Bereitstellung in Ihrem AKS-Cluster aus:
```
kubectl apply -f acr-nginx.yaml
```

Sie können die Bereitstellung überwachen, indem Sie Folgendes ausführen:
```
kubectl get pods
```
Sie sollten über zwei laufende Pods verfügen.
```
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
