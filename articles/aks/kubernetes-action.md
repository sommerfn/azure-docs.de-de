---
title: Erstellen, Testen und Bereitstellen von Containern für den Azure Kubernetes Service mithilfe von GitHub Actions
description: Erfahren Sie, wie Sie mit GitHub Actions Ihren Container für Kubernetes bereitstellen können.
services: container-service
author: azooinmyluggage
ms.service: container-service
ms.topic: article
ms.date: 11/04/2019
ms.author: atulmal
ms.openlocfilehash: cc2d6df952b2e0aa9b9f4d4e1dcb4859a5bb3790
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74130529"
---
# <a name="github-actions-for-deploying-to-kubernetes-service"></a>GitHub Actions für die Bereitstellung im Kubernetes Service

[GitHub Actions](https://help.github.com/en/articles/about-github-actions) bietet Ihnen die Flexibilität, einen automatisierten Workflow für den Softwareentwicklungs-Lebenszyklus zu erstellen. Die Kubernetes-Aktion [azure/aks-set-context@v1](https://github.com/Azure/aks-set-context) erleichtert die Bereitstellung in Azure Kubernetes Service-Clustern. Die Aktion legt den Ziel-AKS-Clusterkontext fest, der von anderen Aktionen wie [azure/k8s-deploy](https://github.com/Azure/k8s-deploy/tree/master), [azure/k8s-create-secret](https://github.com/Azure/k8s-create-secret/tree/master) usw. verwendet werden oder beliebige kubectl-Befehle ausführen kann.

> [!IMPORTANT]
> GitHub Actions liegt zurzeit in einer Betaversion vor. Sie müssen sich zunächst mit Ihrem GitHub-Konto [registrieren, um die Vorschau nutzen zu können](https://github.com/features/actions).
> 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden.

Für einen Workflow, der auf AKS ausgerichtet ist, besteht die Datei aus drei Abschnitten:

|`Section`  |Aufgaben  |
|---------|---------|
|**Authentifizierung** | Anmelden bei einer privaten Containerregistrierung (ACR). |
|**Build** | Erstellen und Pushen des Containerimages.  |
|**Bereitstellen** | 1. Festlegen des Ziel-AKS-Clusters. |
| |2. Erstellen eines generischen/docker-registry-Geheimnisses im Kubernetes-Cluster.  |
||3. Bereitstellen auf dem Kubernetes-Cluster.|

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Sie können mit dem Befehl [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](https://docs.microsoft.com/cli/azure/) einen [Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com/) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```

Ersetzen Sie im obigen Befehl die Platzhalter durch Ihre Abonnement-ID und Ressourcengruppe. Bei der Ausgabe handelt es sich um die Anmeldeinformationen für die Rollenzuweisung, die Zugriff auf ihre Ressource bereitstellen. Der Befehl sollte ein JSON-Objekt ausgeben, das diesem ähnelt.

```json
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```
Kopieren Sie dieses JSON-Objekt, das Sie zum Authentifizieren aus GitHub verwenden können.

## <a name="configure-the-github-secrets"></a>Konfigurieren der GitHub-Geheimnisse

Führen Sie zum Konfigurieren der Geheimnisse die folgenden Schritte aus:

1. Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

    ![secrets](media/kubernetes-action/secrets.png)

2. Fügen Sie die Inhalte des obigen `az cli`-Befehls als Wert der Geheimnisvariablen ein. Beispiel: `AZURE_CREDENTIALS`.

3. Definieren Sie in gleicher Weise die folgenden zusätzlichen Geheimnisse für die Containerregistrierungs-Anmeldeinformationen, und legen Sie sie in der Aktion für die Docker-Anmeldung fest. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

4. Nach der Definition werden die Geheimnisse wie folgt angezeigt.

    ![kubernetes-secrets](media/kubernetes-action/kubernetes-secrets.png)

##  <a name="build-a-container-image-and-deploy-to-azure-kubernetes-service-cluster"></a>Erstellen eines Containerimages und Bereitstellen im Azure Kubernetes Service-Cluster

Das Erstellen und Pushen der Containerimages erfolgt mit der Aktion `Azure/docker-login@v1`. Zum Bereitstellen eines Containerimages in AKS müssen Sie die Aktion `Azure/k8s-deploy@v1` verwenden. Diese Aktion umfasst fünf Parameter:

| **Parameter**  | **Erklärung**  |
|---------|---------|
| **namespace** | (Optional) Wählen Sie den Ziel-Kubernetes-Namespace aus. Wenn der Namespace nicht angegeben ist, werden die Befehle im Standardnamespace ausgeführt. | 
| **manifests** |  (Erforderlich) Pfad zu den Manifestdateien, die für die Bereitstellung verwendet werden. |
| **images** | (Optional) Vollständig qualifizierte Ressourcen-URL der Images, die für Ersetzungen in den Manifestdateien verwendet werden sollen. |
| **imagepullsecrets** | (Optional) Name eines docker-registry-Geheimnisses, das bereits im Cluster eingerichtet wurde. Jeder dieser Geheimnisnamen wird im Feld „imagePullSecrets“ für die Workloads hinzugefügt, die in den Eingabemanifestdateien gefunden werden. |
| **kubectl-version** | (Optional) Installiert eine bestimmte Version der kubectl-Binärdatei. |

### <a name="deploy-to-azure-kubernetes-service-cluster"></a>Bereitstellen im Azure Kubernetes Service-Cluster

End-to-End-Workflow für die Erstellung von Containerimages und die Bereitstellung in einem Azure Kubernetes Service-Cluster.

```yaml
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    
    - uses: Azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/k8sdemo:${{ github.sha }}
        docker push contoso.azurecr.io/k8sdemo:${{ github.sha }}
      
    # Set the target AKS cluster.
    - uses: Azure/aks-set-context@v1
      with:
        creds: '${{ secrets.AZURE_CREDENTIALS }}'
        cluster-name: contoso
        resource-group: contoso-rg
        
    - uses: Azure/k8s-create-secret@v1
      with:
        container-registry-url: contoso.azurecr.io
        container-registry-username: ${{ secrets.REGISTRY_USERNAME }}
        container-registry-password: ${{ secrets.REGISTRY_PASSWORD }}
        secret-name: demo-k8s-secret

    - uses: Azure/k8s-deploy@v1
      with:
        manifests: |
          manifests/deployment.yml
          manifests/service.yml
        images: |
          demo.azurecr.io/k8sdemo:${{ github.sha }}
        imagepullsecrets: |
          demo-k8s-secret
```

## <a name="next-steps"></a>Nächste Schritte

Die verfügbaren Aktionen sind auf verschiedene GitHub-Repositorys verteilt, jeweils mit Dokumentation und Beispielen, um Sie bei der Verwendung von GitHub für CI/CD und der Bereitstellung Ihrer Apps in Azure zu unterstützen.

- [setup-kubectl](https://github.com/Azure/setup-kubectl)

- [k8s-set-context](https://github.com/Azure/k8s-set-context)

- [aks-set-context](https://github.com/Azure/aks-set-context)

- [k8s-create-secret](https://github.com/Azure/k8s-create-secret)

- [k8s-deploy](https://github.com/Azure/k8s-deploy)

- [webapps-container-deploy](https://github.com/Azure/webapps-container-deploy)

- [actions-workflow-samples](https://github.com/Azure/actions-workflow-samples)
