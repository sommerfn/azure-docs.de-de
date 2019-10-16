---
title: Verwenden von GitHub Actions zum Vornehmen von Codeaktualisierungen in Azure Functions
description: Erfahren Sie, wie Sie mit GitHub Actions einen Workflow zum Erstellen und Bereitstellen von Azure Functions-Projekten in GitHub definieren.
author: ahmedelnably
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: aelnably
ms.openlocfilehash: 483ac9380fa8d58f294112cb6c80e0393fa01589
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72028975"
---
# <a name="continuous-delivery-by-using-github-action"></a>Continuous Delivery mit GitHub Actions

Mit [GitHub Actions](https://github.com/features/actions) können Sie einen Workflow definieren, um Ihren Funktionscode für eine Funktions-App in Azure automatisch zu erstellen und bereitzustellen. 

> [!IMPORTANT]  
> GitHub Actions liegt zurzeit in einer Betaversion vor. Sie müssen sich zunächst mit Ihrem GitHub-Konto [registrieren, um die Vorschau nutzen zu können](https://github.com/features/actions).

In GitHub Actions ist ein [Workflow](https://help.github.com/articles/about-github-actions#workflow) ein automatisierter Prozess, den Sie in Ihrem GitHub-Repository definieren. Dieser Prozess informiert GitHub, wie Ihr Funktions-App-Projekt auf GitHub erstellt und bereitgestellt werden soll. 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden. 

Bei einem Azure Functions-Workflow umfasst die Datei drei Abschnitte: 

| `Section` | Aufgaben |
| ------- | ----- |
| **Authentifizierung** | <ol><li>Definieren eines Dienstprinzipals.</li><li>Herunterladen des Veröffentlichungsprofils</li><li>Erstellen eines GitHub-Geheimnisses.</li></ol>|
| **Build** | <ol><li>Einrichten der Umgebung.</li><li>Erstellen Sie die Funktions-App.</li></ol> |
| **Bereitstellen** | <ol><li>Bereitstellen der Funktions-App.</li></ol>|

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Sie können mit dem Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](/cli/azure/) einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

Ersetzen Sie in diesem Beispiel die Platzhalter in der Ressource durch Ihre Abonnement-ID, die Ressourcengruppe und den Namen der Funktions-App. Bei der Ausgabe handelt es sich um die Anmeldeinformationen für die Rollenzuweisung, die Zugriff auf ihre Funktions-App bereitstellen. Kopieren Sie dieses JSON-Objekt, das Sie zum Authentifizieren aus GitHub verwenden können.

> [!NOTE]
> Sie müssen keinen Dienstprinzipal erstellen, wenn Sie das Veröffentlichungsprofil für die Authentifizierung verwenden.

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Aus diesem Grund ist der Bereich im vorherigen Beispiel auf die spezifische Funktions-App und nicht auf die gesamte Ressourcengruppe eingeschränkt.

## <a name="download-the-publishing-profile"></a>Herunterladen des Veröffentlichungsprofils

Sie können das Veröffentlichungsprofil Ihrer Funktions-App herunterladen, indem Sie auf der Seite **Übersicht** Ihrer App auf **Veröffentlichungsprofil abrufen** klicken.

   ![Veröffentlichungsprofil herunterladen](media/functions-how-to-github-actions/get-publish-profile.png)

Kopieren Sie den Inhalt der Datei.

## <a name="configure-the-github-secret"></a>Konfigurieren des GitHub-Geheimnisses

1. Navigieren Sie in [GitHub](https://github.com) zu Ihrem Repository, und wählen Sie **Einstellungen** > **Geheimnisse** > **Neues Geheimnis hinzufügen** aus.

   ![Hinzufügen des Geheimnisses](media/functions-how-to-github-actions/add-secret.png)

1. Verwenden Sie `AZURE_CREDENTIALS` als **Namen** und die kopierte Befehlsausgabe als **Wert**, und wählen Sie dann **Geheimnis hinzufügen** aus. Wenn Sie das Veröffentlichungsprofil verwenden, geben Sie `SCM_CREDENTIALS` als **Namen** und den Dateiinhalt als **Wert** an.

GitHub kann sich jetzt bei ihrer Funktions-App in Azure authentifizieren.

## <a name="set-up-the-environment"></a>Einrichten der Umgebung 

Das Einrichten der Umgebung kann mithilfe einer der Aktionen zum Veröffentlichen des Setups erfolgen.

|Sprache | Setupaktion |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**    | `actions/setup-java` |
|**JavaScript**     | `actions/setup-node` |
|**Python**   | `actions/setup-python` |

Die folgenden Beispiele zeigen den Teil des Workflows, der die Umgebung für die verschiedenen unterstützten Sprachen einrichtet:

**JavaScript**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```

**Python**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-function-app"></a>Erstellen der Funktions-App

Dies hängt von der Sprache und den von Azure Functions unterstützten Sprachen ab. Dieser Abschnitt sollte die Standardbuildschritte der einzelnen Sprachen beinhalten.

Die folgenden Beispiele zeigen den Teil des Workflows, der die Funktions-App in den verschiedenen unterstützten Sprachen erstellt:

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
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
        # If your function app project is not located in your repository's root
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
        # If your function app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - name: 'Run mvn'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for maven build in pushd
        pushd . ./POM_ARTIFACT_ID
        mvn clean package
        mvn azure-functions:package
        popd
```

## <a name="deploy-the-function-app"></a>Bereitstellen der Funktionen-App

Wenn Sie Ihren Code in einer Funktions-App bereitstellen möchten, müssen Sie die Aktion `Azure/functions-action` verwenden. Diese Aktion verfügt über zwei Parameter:

|Parameter |Erklärung  |
|---------|---------|
|**_app-name_** | (Erforderlich) Der Name Ihrer Funktions-App. |
|_**slot-name**_ | (Optional) Der Name des [Bereitstellungsslots](functions-deployment-slots.md), in dem die Bereitstellung erfolgen soll. Der Slot muss in ihrer Funktions-App bereits definiert sein. |


Im folgenden Beispiel wird Version 1 von `functions-action` verwendet:

```yaml
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: PLEASE_REPLACE_THIS_WITH_YOUR_FUNCTION_APP_NAME
```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige YAML-Datei des Workflows finden Sie in einer der Dateien mit `functionapp` im Namen im [Repository mit Workflowbeispielen für Azure GitHub Actions ](https://github.com/Azure/actions-workflow-samples). Sie können diese Beispiele als Ausgangspunkt für den Workflow verwenden.

> [!div class="nextstepaction"]
> [Weitere Informationen zu GitHub Actions](https://help.github.com/en/articles/about-github-actions)
