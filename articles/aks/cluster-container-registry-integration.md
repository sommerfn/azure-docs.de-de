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
ms.openlocfilehash: d9d432c073872e7bb7f3562979e78989faea65eb
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72241089"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Authentifizieren per Azure Container Registry über Azure Kubernetes Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Kubernetes Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. In diesem Artikel werden die empfohlenen Konfigurationen für die Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben.

Sie können die AKS- und ACR-Integration mit einigen einfachen Befehlen über die Azure-Befehlszeilenschnittstelle einrichten.

## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen Folgendes:

* Rolle **Besitzer** oder **Azure-Kontoadministrator** im **Azure-Abonnement**
* Außerdem benötigen Sie die Azure CLI-Version 2.0.73 oder höher.
* Auf Ihrem Client muss [Docker installiert sein](https://docs.docker.com/install/), und Sie benötigen Zugriff auf den [Docker-Hub](https://hub.docker.com/).

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Erstellen eines neuen AKS-Clusters mit ACR-Integration

Sie können die AKS- und ACR-Integration während der erstmaligen Erstellung Ihres AKS-Clusters einrichten.  Damit ein AKS-Cluster mit ACR interagieren kann, wird ein Azure Active Directory-**Dienstprinzipal** verwendet. Mit dem folgenden CLI-Befehl können Sie eine vorhandene ACR-Instanz in Ihrem Abonnement autorisieren und die entsprechende **ACRPull**-Rolle für den Dienstprinzipal konfigurieren. Geben Sie gültige Werte für die unten stehenden Parameter an.  Die Parameter in eckigen Klammern sind optional.
```azurecli
az login
az acr create -n myContainerRegistry -g myContainerRegistryResourceGroup --sku basic [in case you do not have an existing ACR]
az aks create -n myAKSCluster -g myResourceGroup --attach-acr <acr-name-or-resource-id>
```
**Eine ACR-Ressourcen-ID hat folgendes Format:** 

/subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.ContainerRegistry/registries/\<Name\> 
  
Dieser Schritt kann mehrere Minuten in Anspruch nehmen.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Konfigurieren der ACR-Integration für vorhandene AKS-Cluster

Integrieren Sie eine vorhandene ACR-Instanz in vorhandene AKS-Cluster, indem Sie wie unten gezeigt gültige Werte für **acr-name** oder **acr-resource-id** angeben.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Sie können die Integration zwischen einer ACR und einem AKS-Cluster auch folgendermaßen entfernen:
```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```


## <a name="log-in-to-your-acr"></a>Anmelden bei Ihrer ACR-Instanz

Verwenden Sie den folgenden Befehl, um sich bei Ihrer ACR-Instanz anzumelden.  Ersetzen Sie den Parameter <acrname> durch den Namen Ihrer ACR-Instanz.  Der Standardwert lautet beispielsweise **aks<IhreRessourcengruppe>acr**.

```azurecli
az acr login -n <acrName>
```

## <a name="pull-an-image-from-docker-hub-and-push-to-your-acr"></a>Verwenden von Pull zum Abrufen eines Images vom Docker-Hub und Push zum Senden an Ihre ACR-Instanz

Rufen Sie ein Image per Pull vom Docker-Hub ab, taggen Sie es, und senden Sie es per Push an Ihre ACR-Instanz.

```console
acrloginservername=$(az acr show -n <acrname> -g <myResourceGroup> --query loginServer -o tsv)
docker pull nginx
```

```
$ docker tag nginx $acrloginservername/nginx:v1
$ docker push $acrloginservername/nginx:v1

The push refers to repository [someacr1.azurecr.io/nginx]
fe6a7a3b3f27: Pushed
d0673244f7d4: Pushed
d8a33133e477: Pushed
v1: digest: sha256:dc85890ba9763fe38b178b337d4ccc802874afe3c02e6c98c304f65b08af958f size: 948
```

## <a name="update-the-state-and-verify-pods"></a>Aktualisieren des Zustands und Überprüfen von Pods

Gehen Sie zum Überprüfen Ihrer Bereitstellung wie folgt vor.

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Zeigen Sie die YAML-Datei an, und bearbeiten Sie die image-Eigenschaft, indem Sie den Wert durch den ACR-Anmeldeserver, das Image und das Tag ersetzen.

```
$ cat acr-nginx.yaml

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

$ kubectl apply -f acr-nginx.yaml
$ kubectl get pods

You should have two running pods.

NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
