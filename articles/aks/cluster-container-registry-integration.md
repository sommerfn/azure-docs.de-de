---
title: Integrieren von Azure Container Registry in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Azure Kubernetes Service (AKS) und Azure Container Registry integrieren
services: container-service
author: mlearned
manager: gwallace
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: mlearned
ms.openlocfilehash: ec017901e36a01042485e9aeca2431c8a6838ab8
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69536246"
---
# <a name="preview---authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Vorschau: Authentifizieren bei Azure Container Registry aus Azure Kubernetes Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Kubernetes Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. In diesem Artikel werden die empfohlenen Konfigurationen für die Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben.

Sie können die AKS- und ACR-Integration mit einigen einfachen Befehlen über die Azure-Befehlszeilenschnittstelle einrichten.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service](support-policies.md)
> * [Häufig gestellte Fragen zum Azure-Support](faq.md)

## <a name="before-you-begin"></a>Voraussetzungen

Sie benötigen Folgendes:

* Rolle **Besitzer** oder **Azure-Kontoadministrator** im **Azure-Abonnement**
* Außerdem benötigen Sie mindestens die Version 2.0.70 der Azure-Befehlszeilenschnittstelle und die Erweiterung „aks-preview 0.4.8“.
* Auf Ihrem Client muss [Docker installiert sein](https://docs.docker.com/install/), und Sie benötigen Zugriff auf den [Docker-Hub](https://hub.docker.com/).

## <a name="install-latest-aks-cli-preview-extension"></a>Installieren der neuesten AKS-CLI-Vorschauerweiterung

Sie benötigen die Erweiterung **aks-preview 0.4.8** oder höher.

```azurecli
az extension remove --name aks-preview 
az extension add -y --name aks-preview
```

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>Erstellen eines neuen AKS-Clusters mit ACR-Integration

Sie können die AKS- und ACR-Integration während der erstmaligen Erstellung Ihres AKS-Clusters einrichten.  Damit ein AKS-Cluster mit ACR interagieren kann, wird ein Azure Active Directory-**Dienstprinzipal** verwendet. Der folgende CLI-Befehl erstellt eine ACR-Instanz in der Ressourcengruppe, die Sie angeben, und konfiguriert die entsprechende **ACRpull**-Rolle für den Dienstprinzipal. Wenn der *ACR-Name* nicht vorhanden ist, wird automatisch ein Standard- ACR-Name (`aks<resource-group>acr`) erstellt.  Geben Sie gültige Werte für die unten stehenden Parameter an.  Die Parameter in eckigen Klammern sind optional.
```azurecli
az login
az aks create -n myAKSCluster -g myResourceGroup --enable-acr [--acr <acr-name-or-resource-id>]
```
Dieser Schritt kann mehrere Minuten in Anspruch nehmen.

## <a name="create-acr-integration-for-existing-aks-clusters"></a>Erstellen der ACR-Integration für vorhandene AKS-Cluster

Integrieren Sie ACR in vorhandene ACR-Cluster, indem Sie unten gültige Werte für **acr-name** und **acr-resource-id** angeben.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --enable-acr --acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --enable-acr --acr <acr-resource-id>
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