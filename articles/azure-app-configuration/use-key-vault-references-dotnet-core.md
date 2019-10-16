---
title: 'Tutorial: Verwenden der Key Vault-Verweise von Azure App Configuration in einer ASP.NET Core-App | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Key Vault-Verweise von Azure App Configuration in einer ASP.NET Core-App verwenden.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/07/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: a14cb3035c159c82df44f686da7f7b78ef942943
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035815"
---
# <a name="tutorial-use-key-vault-references-in-an-aspnet-core-app"></a>Tutorial: Verwenden von Key Vault-Verweisen in einer ASP.NET Core-App

In diesem Tutorial erfahren Sie, wie Sie den Azure App Configuration-Dienst zusammen mit Azure Key Vault verwenden. Es handelt sich dabei um ergänzende Dienste, die in den meisten Anwendungsbereitstellungen parallel genutzt werden. Um Sie bei der gemeinsamen Nutzung zu unterstützen, ermöglicht Ihnen App Configuration das Erstellen von Schlüsseln, die auf in Key Vault gespeicherte Werte verweisen. Dadurch speichert App Configuration den URI im Key Vault-Wert und nicht den Wert selbst. Die Anwendung ruft den Wert dieses Schlüssels über den App Configuration-Clientanbieter ab, genau wie jeden anderen in App Configuration gespeicherten Schlüssel. Der Clientanbieter erkennt ihn als Key Vault-Verweis und ruft Key Vault auf, um den Wert abzurufen. Ihre Anwendung ist für die ordnungsgemäße Authentifizierung sowohl bei App Configuration als auch bei Key Vault zuständig. Die beiden Dienste kommunizieren nicht direkt.

In diesem Tutorial wird veranschaulicht, wie Sie Key Vault-Verweise in Ihren Code implementieren können. Dies baut auf der Web-App auf, die in den Schnellstartanleitungen vorgestellt wurde. Führen Sie zuerst den Schnellstart [Erstellen einer ASP.NET Core-App mit Azure App Configuration](./quickstart-aspnet-core-app.md) durch, bevor Sie fortfahren.

Für die Ausführung der Schritte dieses Tutorials können Sie einen beliebigen Code-Editor verwenden. [Visual Studio Code](https://code.visualstudio.com/) ist eine hervorragende Option, die auf Windows-, macOS- und Linux-Plattformen verfügbar ist.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines App-Konfigurationsschlüssels, der auf einen in Key Vault gespeicherten Wert verweist
> * Zugreifen auf den Wert dieses Schlüssels über eine ASP.NET Core-Webanwendung

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie für dieses Tutorial das [.NET Core SDK](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-vault"></a>Erstellen eines Tresors

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Option **Ressource erstellen**.

    ![Ausgabe nach Erstellung der Key Vault-Instanz](./media/quickstarts/search-services.png)
2. Geben Sie **Key Vault** in das Suchfeld ein.
3. Wählen Sie in der Ergebnisliste **Key Vault** aus.
4. Klicken Sie im Abschnitt „Key Vault“ auf **Erstellen**.
5. Geben Sie im Abschnitt **Schlüsseltresor erstellen** folgende Informationen ein:
    - **Name**: Es ist ein eindeutiger Name erforderlich. Für diesen Schnellstart verwenden wir **Contoso-vault2**. 
    - **Abonnement**: Wählen Sie ein Abonnement aus.
    - Klicken Sie unter **Ressourcengruppe** auf **Neu erstellen**, und geben Sie einen Ressourcengruppennamen ein.
    - Wählen Sie im Pulldownmenü **Speicherort** einen Speicherort aus.
    - Behalten Sie bei den anderen Optionen die Standardeinstellungen bei.
6. Klicken Sie nach der Angabe der obigen Informationen auf **Erstellen**.

An diesem Punkt ist nur Ihr Azure-Konto für den Zugriff auf diesen neuen Tresor autorisiert.

![Ausgabe nach Erstellung der Key Vault-Instanz](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Hinzufügen eines Geheimnisses zu Key Vault

Ein Geheimnis kann dem Tresor mit einigen wenigen Zusatzschritten hinzugefügt werden. In diesem Fall fügen wir eine Nachricht hinzu, die wir verwenden können, um den Key Vault-Abruf zu testen. Die Nachricht heißt **Message**, und wir speichern den Wert **Hallo von Key Vault** darin.

1. Klicken Sie auf den Key Vault-Eigenschaftenseiten auf **Geheimnisse**.
1. Klicken Sie auf **Generieren/Importieren**.
1. Wählen Sie auf dem Bildschirm **Geheimnis erstellen** folgende Werte aus:
    - **Uploadoptionen**: Manuell
    - **Name**: `Message`
    - **Value**: Hallo von Key Vault
    - Behalten Sie bei den anderen Optionen die Standardwerte bei. Klicken Sie auf **Create**.

## <a name="add-a-key-vault-reference-to-app-config"></a>Hinzufügen eines Key Vault-Verweises zur App-Konfiguration

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Alle Ressourcen** aus, und wählen Sie dann die Instanz des App-Konfigurationsspeichers aus, die Sie in der Schnellstartanleitung erstellt haben.

1. Klicken Sie auf den **Konfigurations-Explorer**.

1. Klicken Sie auf **+ Erstellen** > **Key Vault-Verweis**, und wählen Sie die folgenden Werte aus:
    - **Key**: TestApp:Settings:KeyVaultMessage
    - **Bezeichnung:** Nicht ausfüllen
    - **Abonnement**, **Ressourcengruppe**, **Schlüsseltresor**: Wählen Sie die Optionen aus, die der im vorherigen Abschnitt erstellten Key Vault-Instanz entsprechen.
    - **Geheimnis**: Wählen Sie das Geheimnis namens **Message** aus, das Sie im vorherigen Abschnitt erstellt haben.

## <a name="connect-to-key-vault"></a>Verbindung mit Key Vault herstellen

1. In diesem Tutorial verwenden Sie einen Dienstprinzipal für die Authentifizierung bei Key Vault. Verwenden Sie zum Erstellen dieses Dienstprinzipals den Azure CLI-Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac):

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Daraufhin wird eine Reihe von Schlüssel-Wert-Paaren zurückgegeben. 

    ```console
    {
    "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
    "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
    "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
    "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

1. Führen Sie den folgenden Befehl aus, um dem Dienstprinzipal den Zugriff auf Ihren Schlüsseltresor zu erlauben:

        az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey

1. Fügen Sie dem Geheimnismanager Geheimnisse für *clientId* und *clientSecret* hinzu. Diese Befehle müssen im Verzeichnis ausgeführt werden, in dem sich die *.csproj*-Datei befindet.

        dotnet user-secrets set ConnectionStrings:KeyVaultClientId <clientId-of-your-service-principal>        
        dotnet user-secrets set ConnectionStrings:KeyVaultClientSecret <clientSecret-of-your-service-principal>

> [!NOTE]
> Diese Key Vault-Anmeldeinformationen werden nur innerhalb der Anwendung verwendet. Ihre Anwendung authentifiziert sich mit diesen Anmeldeinformationen direkt bei Key Vault. Sie werden nie an den App Configuration-Dienst übermittelt.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Aktualisieren des Codes für die Verwendung eines Key Vault-Verweises

1. Öffnen Sie *Program.cs*, und fügen Sie Verweise auf erforderliche Pakete hinzu.

    ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

1. Aktualisieren Sie die `CreateWebHostBuilder`-Methode für die Verwendung von App Configuration, indem Sie die Methode `config.AddAzureAppConfiguration()` aufrufen. Fügen Sie die Option `UseAzureKeyVault` ein, und übergeben Sie einen neuen `KeyVaultClient`-Verweis an Ihre Key Vault-Instanz.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                KeyVaultClient kvClient = new KeyVaultClient(async (authority, resource, scope) =>
                {
                    var adCredential = new ClientCredential(settings["ConnectionStrings:KeyVaultClientId"], settings["ConnectionStrings:KeyVaultClientSecret"]);
                    var authenticationContext = new AuthenticationContext(authority, null);
                    return (await authenticationContext.AcquireTokenAsync(resource, adCredential)).AccessToken;
                });

                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                            .UseAzureKeyVault(kvClient); });
            })
            .UseStartup<Startup>();
    ```

1. Nachdem Sie bei der Initialisierung der Verbindung mit App Configuration den Verweis *KeyVaultClient* an die `UseAzureKeyVault`-Methode übergeben haben, können Sie auf die Werte der Key Vault-Verweise genauso zugreifen wie auf die Werte der regulären App Configuration-Schlüssel. Um diesen Prozess in Aktion zu sehen, öffnen Sie die Datei *Index.cshtml* im Verzeichnis „Ansichten“ > „Home“. Ersetzen Sie ihren Inhalt durch den folgenden Code:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]
        and @Configuration["TestApp:Settings:KeyVaultMessage"]</h1>
    ```

    Sie greifen auf den Wert des Key Vault-Verweises *TestApp:Settings:KeyVaultMessage* genauso zu,wie auf den Konfigurationswert *TestApp:Settings:Message*

## <a name="build-and-run-the-app-locally"></a>Lokales Erstellen und Ausführen der App

1. Führen Sie den folgenden Befehl in der Befehlsshell aus, um die App mithilfe der .NET Core-CLI zu erstellen:

        dotnet build

2. Führen Sie nach der erfolgreichen Erstellung den folgenden Befehl aus, um die Web-App lokal auszuführen:

        dotnet run

3. Öffnen Sie ein Browserfenster, und navigieren Sie zu `http://localhost:5000`. Dies ist die Standard-URL für die lokal gehostete Web-App.

    ![Schnellstartanleitung: Lokales Starten der App](./media/key-vault-reference-launch-local.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine verwaltete Azure-Dienstidentität hinzugefügt, um den Zugriff auf App Configuration zu optimieren und die Verwaltung der Anmeldeinformationen für Ihre App zu verbessern. Fahren Sie mit den Azure CLI-Beispielen fort, um mehr über die Verwendung von App Configuration zu erfahren.

> [!div class="nextstepaction"]
> [CLI-Beispiele](./cli-samples.md)
