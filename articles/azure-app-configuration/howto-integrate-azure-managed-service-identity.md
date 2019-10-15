---
title: Integrieren mit verwalteten Azure-Identitäten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie verwaltete Azure-Identitäten verwenden, um die Authentifizierung für Azure App Configuration durchzuführen und Zugriff darauf zu erhalten.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 3a5517c31cdac0bf6f5ea386a8614d15521d4479
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035532"
---
# <a name="integrate-with-azure-managed-identities"></a>Integrieren mit verwalteten Azure-Identitäten

Mit [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) von Azure Active Directory wird die Verwaltung von Geheimnissen für Ihre Cloudanwendung vereinfacht. Mit einer verwalteten Identität können Sie Ihren Code für die Nutzung des Dienstprinzipals einrichten, der für den Azure-Computedienst erstellt wurde, auf dem die Ausführung erfolgt. Eine verwaltete Identität wird anstelle von separaten Anmeldeinformationen verwendet, die in Azure Key Vault oder in einer lokalen Verbindungszeichenfolge gespeichert sind. 

Azure App Configuration und die zugehörigen .NET Core-, .NET- und Java Spring-Clientbibliotheken verfügen über integrierte MSI-Unterstützung (Managed Service Identity, Verwaltete Dienstidentität). Zwar müssen Sie MSI nicht verwenden, doch entfällt hierbei die Notwendigkeit für das Zugriffstoken mit Geheimnissen. Ihr Code kann nur mithilfe des Dienstendpunkts auf den App-Konfigurationsspeicher zuzugreifen. Sie können diese URL direkt in Ihren Code einbetten, ohne sich Sorgen machen zu müssen, dass geheime Schlüssel offengelegt werden.

In diesem Tutorial wird veranschaulicht, wie Sie MSI für den Zugriff auf App Configuration nutzen können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Durchlaufen Sie zuerst die Schnellstartanleitung zum [Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md), bevor Sie fortfahren.

Außerdem wird in diesem Tutorial optional gezeigt, wie Sie MSI in Verbindung mit den Key Vault-Verweisen von App Configuration verwenden können. Auf diese Weise können Sie nahtlos auf geheime Schlüssel zugreifen, die in Key Vault gespeichert sind, sowie auf Konfigurationswerte in App Configuration. Wenn Sie diese Funktion erkunden möchten, stellen Sie zuerst [Verwenden von Key Vault-Verweisen mit ASP.NET Core](./use-key-vault-references-dotnet-core.md) fertig.

Für die Ausführung der Schritte dieses Tutorials können Sie einen beliebigen Code-Editor verwenden. [Visual Studio Code](https://code.visualstudio.com/) ist eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Gewähren des Zugriffs auf App Configuration für eine verwaltete Identität.
> * Konfigurieren Ihrer App für die Verwendung einer verwalteten Identität bei der Verbindungsherstellung mit App Configuration.
> * Konfigurieren Sie optional Ihre App für die Verwendung einer verwalteten Identität, wenn Sie eine Verbindung mit Key Vault über einen Key Vault-Verweis von App Configuration herstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [.NET Core SDK](https://www.microsoft.com/net/download/windows).
* [Konfigurierte Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Hinzufügen einer verwalteten Identität

Um eine verwaltete Entität im Portal einzurichten, erstellen Sie wie gewohnt zuerst eine Anwendung und aktivieren dann das Feature.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) wie gewohnt eine App Services-Instanz. Wechseln Sie im Portal zu dieser App.

2. Scrollen Sie im linken Bereich nach unten zur Gruppe **Einstellungen**, und wählen Sie **Identität**.

3. Ändern Sie auf der Registerkarte **Systemseitig zugewiesen** den **Status** in **Ein**, und wählen Sie **Speichern** aus.

4. Antworten Sie mit **Ja**, wenn Sie gefragt werden, ob Sie die systemseitig zugewiesene verwaltete Identität aktivieren möchten.

    ![Festlegen der verwalteten Identität in App Service](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Gewähren des Zugriffs auf App Configuration

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) **Alle Ressourcen** aus, und wählen Sie dann den App-Konfigurationsspeicher aus, den Sie in der Schnellstartanleitung erstellt haben.

1. Wählen Sie die Option **Zugriffssteuerung (IAM)** .

1. Wählen Sie auf der Registerkarte **Zugriff überprüfen** im Kartenelement **Rollenzuweisung hinzufügen** den Befehl **Hinzufügen** aus.

1. Wählen Sie unter **Rolle** die Option **Mitwirkender**. Wählen Sie unter **Zugriff zuweisen zu** unter **Systemseitig zugewiesene verwaltete Identität** die Option **App Service** aus.

1. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus. Wählen Sie die App Service-Ressource für Ihre App aus.

1. Wählen Sie **Speichern** aus.

    ![Hinzufügen einer verwalteten Identität](./media/add-managed-identity.png)

1. Optional: Wenn Sie auch Zugriff auf Key Vault gewähren möchten, befolgen Sie die Anweisungen in [Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität](https://docs.microsoft.com/azure/key-vault/managed-identity).

## <a name="use-a-managed-identity"></a>Verwenden einer verwalteten Identität

1. Suchen Sie nach der URL für Ihren App-Konfigurationsspeicher. Klicken Sie hierzu im Azure-Portal auf dem zugehörigen Konfigurationsbildschirm auf die Registerkarte **Zugriffsschlüssel**.

1. Öffnen Sie die Datei *appsettings.json*, und fügen Sie das folgende Skript hinzu. Ersetzen Sie *\<service_endpoint>* (einschließlich der spitzen Klammern) durch die URL für Ihren App-Konfigurationsspeicher. 

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

1. Wenn Sie nur auf Werte zugreifen möchten, die direkt in App Configuration gespeichert sind, öffnen Sie *Program.cs*, und aktualisieren Sie die `CreateWebHostBuilder`-Methode, indem Sie die `config.AddAzureAppConfiguration()`-Methode ersetzen.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options =>
                    options.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"]));
            })
            .UseStartup<Startup>();
    ```

1. Wenn Sie App Configuration-Werte sowie Key Vault-Verweise verwenden möchten, öffnen Sie *Program.cs*, und aktualisieren Sie die `CreateWebHostBuilder`-Methode, wie unten gezeigt. Dadurch wird ein neuer `KeyVaultClient` erstellt, der einen `AzureServiceTokenProvider` verwendet und diesen Verweis an einen Aufruf der `UseAzureKeyVault`-Methode übergibt.

    ```csharp
        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    var settings = config.Build();
                    AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                    KeyVaultClient kvClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                    
                    config.AddAzureAppConfiguration(options => options.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"])).UseAzureKeyVault(kvClient));
                })
                .UseStartup<Startup>();
    ```

    Sie können jetzt auf Key Vault-Verweise zugreifen wie auf jeden anderen App Configuration-Schlüssel. Der Konfigurationsanbieter verwendet den `KeyVaultClient`, den Sie für die Authentifizierung bei Key Vault konfiguriert haben, und ruft den Wert ab.

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Bereitstellen über lokales Git

Die einfachste Möglichkeit zum Aktivieren einer lokalen Git-Bereitstellung für Ihre App mit dem Kudu-Buildserver ist die Verwendung von Azure Cloud Shell.

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Aktivieren von lokalem Git mit Kudu
Wenn Sie über kein lokales Git-Repository für Ihre App verfügen, müssen Sie eins initialisieren. Führen Sie hierzu die folgenden Befehle aus dem Projektverzeichnis Ihrer App aus:

```cmd
git init
git add .
git commit -m "Initial version"
```

Zum Aktivieren einer lokalen Git-Bereitstellung für Ihre App mit dem Kudu-Buildserver führen Sie [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in Cloud Shell aus.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Wenn Sie stattdessen eine Git-fähige App erstellen möchten, führen Sie [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in Cloud Shell mit dem Parameter `--deployment-local-git` aus.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

Die Ausgabe des Befehls `az webapp create` sieht ungefähr wie folgt aus:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Bereitstellen des Projekts

Kehren Sie zum _lokalen Terminalfenster_ zurück, und fügen Sie Ihrem lokalen Git-Repository einen Azure-Remotespeicherort hinzu. Ersetzen Sie _\<Url>_ durch die URL des Git-Remoterepositorys, die Sie beim [Aktivieren von Git für die App](#enable-local-git-with-kudu) erhalten haben.

```bash
git remote add azure <url>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Geben Sie bei Aufforderung zur Eingabe eines Kennworts das Kennwort ein, das Sie in [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben. Verwenden Sie nicht das Kennwort, mit dem Sie sich beim Azure-Portal anmelden.

```bash
git push azure master
```

Die Ausgabe enthält u. U. laufzeitspezifische Automatisierungen wie MSBuild für ASP.NET, `npm install` für Node.js und `pip install` für Python.

### <a name="browse-to-the-azure-web-app"></a>Navigieren Sie zur Azure-Web-App.

Navigieren Sie in einem Browser zu Ihrer Web-App, um sicherzustellen, dass der Inhalt bereitgestellt wird.

```bash
http://<app_name>.azurewebsites.net
```

![In App Service ausgeführte App](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Verwenden einer verwalteten Identität in anderen Sprachen

App Configuration-Anbieter für .NET Framework und Java Spring verfügen auch über integrierte Unterstützung für verwaltete Identitäten. In diesen Fällen verwenden Sie beim Konfigurieren eines Anbieters anstelle der vollständigen Verbindungszeichenfolge den URL-Endpunkt Ihres App-Konfigurationsspeichers. Für die in der Schnellstartanleitung erstellte .NET Framework-Konsolen-App geben Sie beispielsweise in der Datei *App.config* die folgenden Einstellungen an:

```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)
