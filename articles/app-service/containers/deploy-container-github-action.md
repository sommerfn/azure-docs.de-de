---
title: Bereitstellen Ihres Containers über eine CI/CD-Pipeline mithilfe von GitHub-Aktionen – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von GitHub-Aktionen Ihren Container in App Service bereitstellen.
services: app-service
documentationcenter: ''
author: cephalin
manager: gwallace
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: 7fbd7b571f5590ff35d52062cc621069a47b619c
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73620232"
---
# <a name="deploy-a-custom-container-to-app-service-using-github-actions"></a>Bereitstellen eines benutzerdefinierten Containers in App Service mithilfe von GitHub-Aktionen

[GitHub-Aktionen](https://help.github.com/en/articles/about-github-actions) bieten Ihnen die Flexibilität, einen automatisierten Workflow für den Softwareentwicklungs-Lebenszyklus zu erstellen. Mit der [Azure App Service-Aktion für Container](https://github.com/Azure/webapps-container-deploy) können Sie Ihren Workflow automatisieren, um Apps mithilfe von GitHub-Aktionen als [benutzerdefinierte Container in App Service](https://azure.microsoft.com/services/app-service/containers/) bereitzustellen.

> [!IMPORTANT]
> GitHub Actions liegt zurzeit in einer Betaversion vor. Sie müssen sich zunächst mit Ihrem GitHub-Konto [registrieren, um die Vorschau nutzen zu können](https://github.com/features/actions).
> 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden.

Bei einem Azure App Service-Containerworkflow umfasst die Datei drei Abschnitte:

|`Section`  |Aufgaben  |
|---------|---------|
|**Authentifizierung** | 1. Definieren eines Dienstprinzipals. <br /> 2. Erstellen eines GitHub-Geheimnisses. |
|**Build** | 1. Einrichten der Umgebung. <br /> 2. Erstellen des Containerimages |
|**Bereitstellen** | 1. Bereitstellen des Containerimages |

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Sie können mit dem Befehl [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](https://docs.microsoft.com/cli/azure/) einen [Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com/) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                            --sdk-auth
                            
  # Replace {subscription-id}, {resource-group} with the subscription, resource group details of the WebApp
```

Die Ausgabe ist ein JSON-Objekt mit den Anmeldeinformationen für die Rollenzuweisung, die ähnlich wie unten Zugriff auf Ihre App Service-App gewähren. Kopieren Sie dieses JSON-Objekt für die Authentifizierung aus GitHub.

 ```azurecli 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Sie können den Bereich im obigen Azure CLI-Befehl auf die spezifische App Service-App und die Azure Container Registry-Instanz einschränken, in die die Containerimages mithilfe von Push übertragen werden.

## <a name="configure-the-github-secret"></a>Konfigurieren des GitHub-Geheimnisses

Im folgenden Beispiel werden Anmeldeinformationen auf Benutzerebene, z. B. ein Azure-Dienstprinzipal, für die Bereitstellung verwendet. Führen Sie zum Konfigurieren des Geheimnisses die folgenden Schritte aus:

1. Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

2. Fügen Sie die Inhalte des nachstehenden `az cli`-Befehls als Wert der Geheimnisvariablen ein. Beispiel: `AZURE_CREDENTIALS`.

    
    ```azurecli
    az ad sp create-for-rbac --name "myApp" --role contributor \
                                --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                --sdk-auth
                                
    # Replace {subscription-id}, {resource-group} with the subscription, resource group details
    ```

3. Ersetzen Sie jetzt in der Workflowdatei in Ihrem Branch `.github/workflows/workflow.yml` das Geheimnis in der Aktion für die Azure-Anmeldung durch Ihr Geheimnis.

4. Definieren Sie in gleicher Weise die folgenden zusätzlichen Geheimnisse für die Containerregistrierungs-Anmeldeinformationen, und legen Sie sie in der Aktion für die Docker-Anmeldung fest. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

5. Nach der Definition werden die Geheimnisse wie folgt angezeigt.

    ![Containergeheimnisse](../media/app-service-github-actions/app-service-secrets-container.png)

## <a name="build-the-container-image"></a>Erstellen des Containerimages

Das folgende Beispiel zeigt einen Teil des Workflows zum Erstellen des Docker-Images.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
```

## <a name="deploy-to-an-app-service-container"></a>Bereitstellung in einem App Service-Container

Um Ihr Image in einem benutzerdefinierten Container in App Service bereitzustellen, verwenden Sie die Aktion `azure/webapps-container-deploy@v1`. Diese Aktion umfasst fünf Parameter:

| **Parameter**  | **Erklärung**  |
|---------|---------|
| **app-name** | (Erforderlich): Name der App Service-App. | 
| **slot-name** | (Optional): Geben Sie einen vorhandene Slot ein, bei dem es sich nicht um den Produktionsslot handelt. |
| **images** | (Erforderlich): Geben Sie den vollqualifizierten Namen des bzw. der Containerimages an. Beispiel: „myregistry.azurecr.io/nginx:latest“ oder „python:3.7.2-alpine/“. Für eine App mit mehreren Containern können mehrere Namen von Containerimages angegeben werden (mehrere getrennte Zeilen). |
| **configuration-file** | (Optional): Der Pfad der Docker-Compose-Datei. Es muss sich um einen vollqualifizierten Pfad oder einen Pfad relativ zum Standardarbeitsverzeichnis handeln. Dieser Parameter ist beim Konfigurieren von Apps mit mehreren Containern erforderlich. |
| **container-command** | (Optional): Geben Sie den Startbefehl ein. Beispiel: „dotnet run“ oder „dotnet filename.dll“. |

Nachstehend wird ein Beispielworkflow zum Erstellen und Bereitstellen einer Node.js-App in einem benutzerdefinierten Container in App Service angezeigt.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }} 
      
    - uses: azure/webapps-container-deploy@v1
      with:
        app-name: 'node-rnc'
        images: 'contoso.azurecr.io/nodejssampleapp:${{ github.sha }}'
    
    - name: Azure logout
      run: |
        az logout
```

## <a name="next-steps"></a>Nächste Schritte

Die verfügbaren Aktionen sind auf verschiedene GitHub-Repositorys verteilt, jeweils mit Dokumentation und Beispielen, um Sie bei der Verwendung von GitHub für CI/CD und der Bereitstellung Ihrer Apps in Azure zu unterstützen.

- [Azure-Anmeldung](https://github.com/Azure/login)

- [Azure-Web-App](https://github.com/Azure/webapps-deploy)

- [Azure-Web-App für Container](https://github.com/Azure/webapps-container-deploy)

- [Docker-Anmeldung/-Abmeldung](https://github.com/Azure/docker-login)

- [Ereignisse zum Auslösen von Workflows](https://help.github.com/en/articles/events-that-trigger-workflows)

- [Kubernetes-Bereitstellung](https://github.com/Azure/k8s-deploy)

- [CI-Workflows für den Einstieg](https://github.com/actions/starter-workflows)

- [Workflows zum Einstieg für die Bereitstellung in Azure](https://github.com/Azure/actions-workflow-samples)
