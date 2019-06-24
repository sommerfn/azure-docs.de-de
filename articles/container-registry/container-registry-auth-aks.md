---
title: Authentifizieren per Azure Container Registry über Azure Kubernetes Service
description: Es wird beschrieben, wie Sie Zugriff auf Images in Ihrer privaten Containerregistrierung über den Azure Kubernetes Service gewähren, indem Sie einen Azure Active Directory-Dienstprinzipal verwenden.
services: container-service
author: dlepow
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.openlocfilehash: a541af77daf4136c0056cf9919d69c538d1dc5b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66754478"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Authentifizieren per Azure Container Registry über Azure Kubernetes Service

Wenn Sie Azure Container Registry (ACR) mit dem Azure Kubernetes Service (AKS) nutzen, ist es erforderlich, einen Authentifizierungsmechanismus einzurichten. In diesem Artikel werden die empfohlenen Konfigurationen für die Authentifizierung zwischen diesen beiden Azure-Diensten beschrieben.

Sie müssen nur eine dieser Authentifizierungsmethoden konfigurieren. Die gängigste Vorgehensweise besteht darin, [Zugriff mithilfe des AKS-Dienstprinzipals zu gewähren](#grant-aks-access-to-acr). Bei spezifischen Anforderungen können Sie optional [den Zugriff mithilfe von Kubernetes-Geheimnissen erteilen](#access-with-kubernetes-secret).

In diesem Artikel wird davon ausgegangen, dass Sie bereits einen AKS-Cluster erstellt haben und auf den Cluster mit dem `kubectl`-Befehlszeilenclient zugreifen können.

## <a name="grant-aks-access-to-acr"></a>Gewähren von AKS-Zugriff auf ACR

Wenn Sie einen AKS-Cluster erstellen, erstellt Azure auch einen Dienstprinzipal zur Unterstützung der Clusteroperabilität mit anderen Azure-Ressourcen. Sie können diesen automatisch generierten Dienstprinzipal auch für die Authentifizierung bei einer ACR-Registrierung verwenden. Dazu müssen Sie eine Azure AD-[Rollenzuweisung](../role-based-access-control/overview.md#role-assignments) erstellen, die dem Dienstprinzipal des Clusters Zugriff auf die Containerregistrierung gewährt.

Verwenden Sie das folgende Skript, um dem mit AKS generierten Dienstprinzipal Pullzugriff auf eine Azure-Containerregistrierung zu gewähren. Ändern Sie die Variablen `AKS_*` und `ACR_*` für Ihre Umgebung, bevor Sie das Skript ausführen.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Zugreifen per Kubernetes-Geheimnis

In einigen Fällen können Sie dem automatisch generierten AKS-Dienstprinzipal nicht die erforderliche Rolle für den Zugriff auf ACR zuweisen. Beispielsweise haben Sie aufgrund des Sicherheitsmodells Ihrer Organisation möglicherweise keine ausreichenden Berechtigungen in Ihrem Azure Active Directory-Mandanten, um dem mit AKS generierten Dienstprinzipal eine Rolle zuzuweisen. Damit Sie einem Dienstprinzipal eine Rolle zuweisen können, muss Ihr Azure AD-Konto über Schreibberechtigung für Ihren Azure AD-Mandanten verfügen. Wenn diese Berechtigung nicht vorhanden ist, können Sie einen neuen Dienstprinzipal erstellen und diesem dann mit einem Kubernetes-Geheimnis für einen Image-Pullvorgang Zugriff auf die Containerregistrierung gewähren.

Verwenden Sie das folgende Skript, um einen neuen Dienstprinzipal zu erstellen. (Sie verwenden dessen Anmeldeinformationen für das Kubernetes-Geheimnis für den Image-Pullvorgang.) Ändern Sie die `ACR_NAME`-Variable für Ihre Umgebung, bevor Sie das Skript ausführen.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create acrpull role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name http://$SERVICE_PRINCIPAL_NAME --role acrpull --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Nun können Sie die Anmeldeinformationen des Dienstprinzipals in einem [Kubernetes-Geheimnis für einen Image-Pullvorgang][image-pull-secret] speichern, auf das der AKS-Cluster beim Ausführen von Containern verweist.

Verwenden Sie den folgenden **kubectl**-Befehl, um das Kubernetes-Geheimnis zu erstellen. Ersetzen Sie `<acr-login-server>` durch den vollqualifizierten Namen Ihrer Azure-Containerregistrierung (im Format „acrname.azurecr.io“). Ersetzen Sie `<service-principal-ID>` und `<service-principal-password>` durch die Werte, die Sie durch Ausführen des vorhergehenden Skripts erhalten haben. Ersetzen Sie `<email-address>` mit einer gültigen E-Mail-Adresse.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Sie können jetzt das Kubernetes-Geheimnis in Podbereitstellungen verwenden, indem Sie seinen Namen (in diesem Fall „acr-auth“) im `imagePullSecrets`-Parameter angeben:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
