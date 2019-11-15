---
title: Bereitstellen Ihres Codes über eine CI/CD-Pipeline mithilfe von GitHub-Aktionen – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von GitHub-Aktionen Ihren Code in App Service bereitstellen.
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
ms.openlocfilehash: 9842057a590b08f2207a1ea166e0ce0d457e4381
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73620518"
---
# <a name="deploy-to-app-service-using-github-actions"></a>Bereitstellen in App Service mithilfe von GitHub Actions

[GitHub Actions](https://help.github.com/en/articles/about-github-actions) bietet Ihnen die Flexibilität, einen automatisierten Workflow für den Softwareentwicklungs-Lebenszyklus zu erstellen. Mit den Azure App Service-Aktionen für GitHub können Sie Ihren Workflow automatisieren, um mithilfe von GitHub Actions in [Azure App Service](overview.md) bereitzustellen.

> [!IMPORTANT]
> GitHub Actions liegt zurzeit in einer Betaversion vor. Sie müssen sich zunächst mit Ihrem GitHub-Konto [registrieren, um die Vorschau nutzen zu können](https://github.com/features/actions).
> 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden.

Bei einem Azure App Service-Workflow umfasst die Datei drei Abschnitte:

|`Section`  |Aufgaben  |
|---------|---------|
|**Authentifizierung** | 1. Definieren eines Dienstprinzipals. <br /> 2. Erstellen eines GitHub-Geheimnisses. |
|**Build** | 1. Einrichten der Umgebung <br /> 2. Erstellen der Web-App. |
|**Bereitstellen** | 1. Bereitstellen der Web-App |

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Sie können mit dem Befehl [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](https://docs.microsoft.com/cli/azure/) einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com/) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> --sdk-auth
```

Ersetzen Sie in diesem Beispiel die Platzhalter in der Ressource durch Ihre Abonnement-ID, den Ressourcengruppennamen und den App-Namen. Bei der Ausgabe handelt es sich um die Anmeldeinformationen für die Rollenzuweisung, die Zugriff auf ihre App Service-App bereitstellen. Kopieren Sie dieses JSON-Objekt, das Sie zum Authentifizieren aus GitHub verwenden können.

> [!NOTE]
> Sie müssen keinen Dienstprinzipal erstellen, wenn Sie das Veröffentlichungsprofil für die Authentifizierung verwenden.

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Aus diesem Grund ist der Bereich im vorherigen Beispiel auf die spezifische App Service-App und nicht auf die gesamte Ressourcengruppe eingeschränkt.

## <a name="configure-the-github-secret"></a>Konfigurieren des GitHub-Geheimnisses

Sie können auch Anmeldeinformationen auf App-Ebene verwenden, z. B. das Veröffentlichungsprofil für die Bereitstellung. Führen Sie zum Konfigurieren des Geheimnisses die folgenden Schritte aus:

1. Laden Sie das Veröffentlichungsprofil für die App Service-App mit der Option **Veröffentlichungsprofil abrufen** aus dem Portal herunter.

2. Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

    ![secrets](media/app-service-github-actions/secrets.png)

3. Fügen Sie den Inhalt für die heruntergeladene Veröffentlichungsprofildatei in das Wertfeld des Geheimnisses ein.

4. Ersetzen Sie jetzt in der Workflowdatei in Ihrem Branch `.github/workflows/workflow.yml` das Geheimnis für das eingegebene `publish-profile` der Aktion für die Bereitstellung der Azure-Web-App.
    
    ```yaml
        - uses: azure/webapps-deploy@v1
          with:
            creds: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. Nach der Definition wird das Geheimnis wie folgt angezeigt.

    ![secrets](media/app-service-github-actions/app-service-secrets.png)

## <a name="set-up-the-environment"></a>Einrichten der Umgebung

Das Einrichten der Umgebung kann mithilfe einer der Setupaktionen erfolgen.

|**Sprache**  |**Setupaktion**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

Die folgenden Beispiele zeigen den Teil des Workflows, der die Umgebung für die verschiedenen unterstützten Sprachen einrichtet:

**JavaScript**

```yaml
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```
**Python**

```yaml
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-web-app"></a>Erstellen der Web-App.

Dies hängt von der Sprache und den von Azure App Service unterstützten Sprachen ab. Dieser Abschnitt sollte die Standardbuildschritte der einzelnen Sprachen beinhalten.

Die folgenden Beispiele zeigen den Teil des Workflows, der die Web-App in den verschiedenen unterstützten Sprachen erstellt.

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - uses: actions/checkout@v1
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```
## <a name="deploy-to-app-service"></a>Bereitstellen für App Service

Verwenden Sie die Aktion `azure/webapps-deploy@v1 `, um Ihren Code in einer App Service-App bereitzustellen. Diese Aktion umfasst vier Parameter:

| **Parameter**  | **Erklärung**  |
|---------|---------|
| **app-name** | (Erforderlich): Name der App Service-App. | 
| **publish-profile** | (Optional) Veröffentlichungsprofil-Dateiinhalte mit Web Deploy-Geheimnissen. |
| **package** | (Optional) Pfad zum Paket oder Ordner. *.zip, *.war, *.jar oder ein Ordner für die Bereitstellung |
| **slot-name** | (Optional): Geben Sie einen vorhandene Slot ein, bei dem es sich nicht um den Produktionsslot handelt. |

### <a name="deploy-using-publish-profile"></a>Bereitstellen über das Veröffentlichungsprofil

Nachstehend wird ein Beispielworkflow zum Erstellen und Bereitstellen einer Node.js-App in Azure mithilfe eines Veröffentlichungsprofils angezeigt.

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
    
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
       
    - name: 'Run Azure webapp deploy action using publish profile credentials'
          uses: azure/webapps-deploy@v1
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

### <a name="deploy-using-azure-service-principal"></a>Bereitstellen über den Azure-Dienstprinzipal

Nachstehend wird ein Beispielworkflow zum Erstellen und Bereitstellen einer Node.js-App in Azure mithilfe eines Azure-Dienstprinzipals angezeigt.

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
   
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
               
    # deploy web app using Azure credentials
    - uses: azure/webapps-deploy@v1
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

## <a name="next-steps"></a>Nächste Schritte

Die verfügbaren Aktionen sind auf verschiedene GitHub-Repositorys verteilt, jeweils mit Dokumentation und Beispielen, um Sie bei der Verwendung von GitHub für CI/CD und der Bereitstellung Ihrer Apps in Azure zu unterstützen.

- [Aktionsworkflow für das Bereitstellen in Azure](https://github.com/Azure/actions-workflow-samples)

- [Azure-Anmeldung](https://github.com/Azure/login)

- [Azure-Web-App](https://github.com/Azure/webapps-deploy)

- [Azure-Web-App für Container](https://github.com/Azure/webapps-container-deploy)

- [Docker-Anmeldung/-Abmeldung](https://github.com/Azure/docker-login)

- [Ereignisse zum Auslösen von Workflows](https://help.github.com/en/articles/events-that-trigger-workflows)

- [Kubernetes-Bereitstellung](https://github.com/Azure/k8s-deploy)

- [Workflows zum Einstieg](https://github.com/actions/starter-workflows)
