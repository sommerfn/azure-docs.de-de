---
title: 'Hinzufügen von Key Vault-Unterstützung zu Ihrem ASP.NET-Projekt mit Visual Studio: Azure Key Vault | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie einer ASP.NET- oder ASP.NET Core-Webanwendung Key Vault-Unterstützung hinzufügen.
services: key-vault
author: ghogen
manager: jillfra
ms.service: key-vault
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: ghogen
ms.openlocfilehash: 9331f13bd85d9df0d47f8fa9d0964974764691f7
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73815117"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Hinzufügen von Key Vault zu Ihrer Webanwendung mithilfe der Option „Verbundene Dienste“ in Visual Studio

In diesem Tutorial erfahren Sie, wie Sie auf einfache Weise alles hinzufügen, was Sie zum Verwalten Ihrer Geheimnisse mit Azure Key Vault für Webprojekte in Visual Studio benötigen – ganz unabhängig davon, ob Sie ASP.NET Core oder einen beliebigen Typ von ASP.NET-Projekt verwenden. Mit dem Feature „Verbundene Dienste“ in Visual Studio werden alle NuGet-Pakete und Konfigurationseinstellungen, die für eine Verbindung mit Key Vault in Azure erforderlich sind, von Visual Studio automatisch hinzugefügt.

Ausführliche Informationen zu den Änderungen, die durch verbundene Dienste in Ihrem Projekt durchgeführt werden, um Key Vault zu aktivieren, finden Sie unter [Verbundener Key Vault-Dienst – Auswirkungen auf mein ASP.NET 4.7.1-Projekt](#how-your-aspnet-framework-project-is-modified) bzw. unter [Verbundener Key Vault-Dienst – Auswirkungen auf mein ASP.NET Core-Projekt](#how-your-aspnet-core-project-is-modified).

## <a name="prerequisites"></a>Voraussetzungen

- **Ein Azure-Abonnement**. Falls Sie kein Abonnement besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
- Mindestens **Visual Studio 2019 (Version 16.3 Preview 1)** oder **Visual Studio 2017 (Version 15.7)** mit installierter Workload **Webentwicklung**. [Jetzt herunterladen](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Für ASP.NET (nicht Core) mit Visual Studio 2017 benötigen Sie die .NET Framework-Entwicklungstools der Version 4.7.1 oder höher, die nicht standardmäßig installiert werden. Um sie zu installieren, starten Sie den Installer für Visual Studio, wählen Sie **Ändern** und dann **Einzelne Komponenten**, erweitern Sie auf der rechten Seite den Eintrag **ASP.NET und Webentwicklung**, und wählen Sie **.NET Framework 4.7.1-Entwicklungstools** aus.
- Ein geöffnetes ASP.NET-Webprojekt der Version 4.7.1 oder höher bzw. ein geöffnetes ASP.NET Core 2.0-Webprojekt.

## <a name="add-key-vault-support-to-your-project"></a>Hinzufügen von Key Vault-Unterstützung zu Ihrem Projekt

Vergewissern Sie sich zunächst, dass Sie bei Visual Studio angemeldet sind. Melden Sie sich mit dem gleichen Konto an, das Sie auch für Ihr Azure-Abonnement verwenden. Öffnen Sie dann ein Webprojekt (ASP.NET 4.7.1 oder höher oder ASP.NET Core 2.0), und führen Sie die folgenden Schritte aus:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, dem Sie die Schlüsseltresorunterstützung hinzufügen möchten, und wählen Sie **Hinzufügen** > **Verbundener Dienst** aus.
   Die Seite „Verbundener Dienst“ wird mit den Diensten angezeigt, die Sie dem Projekt hinzufügen können.
1. Wählen Sie im Menü der verfügbaren Dienste **Schutz für Geheimnisse mit Azure Key Vault**.

   ![Auswahl von „Schutz für Geheimnisse mit Azure Key Vault“](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

1. Wählen Sie das zu verwendende Abonnement und anschließend eine neue oder bereits vorhandene Key Vault-Instanz aus. Wenn Sie sich für eine neue Key Vault-Instanz entscheiden, wird der Link **Bearbeiten** angezeigt. Wählen Sie diesen Link aus, um Ihre neue Key Vault-Instanz zu konfigurieren.

   ![Wählen Sie Ihr Abonnement aus.](media/vs-key-vault-add-connected-service/key-vault-connected-service-select-vault.png)

1. Geben Sie unter **Azure Key Vault bearbeiten** den gewünschten Namen für die Key Vault-Instanz ein.

1. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine neue Ressourcengruppe mit einem automatisch generierten eindeutigen Namen.  Wenn Sie eine neue Gruppe mit einem anderen Namen erstellen möchten, können Sie das [Azure-Portal](https://portal.azure.com) verwenden und anschließend die Seite schließen und den Vorgang neu starten, um die Liste mit den Ressourcengruppen neu zu laden.
1. Wählen Sie den **Standort** aus, an dem die Key Vault-Instanz erstellt werden soll. Wenn Ihre Webanwendung in Azure gehostet wird, wählen Sie für eine optimale Leistung die Region aus, in der die Webanwendung gehostet wird.
1. Wählen Sie einen **Tarif** aus. Weitere Informationen finden Sie unter [Key Vault – Preise](https://azure.microsoft.com/pricing/details/key-vault/).
1. Wählen Sie **OK** aus, um die Konfigurationsoptionen zu akzeptieren.
1. Nachdem Sie eine vorhandene Key Vault-Instanz ausgewählt oder eine neue Key Vault-Instanz konfiguriert haben, wählen Sie in Visual Studio auf der Registerkarte **Azure Key Vault** die Option **Hinzufügen** aus, um den verbundenen Dienst hinzuzufügen.
1. Wählen Sie den Link **In diesem Schlüsseltresor gespeicherte Geheimnisse verwalten** aus, um die Seite **Geheimnisse** für Ihre Key Vault-Instanz zu öffnen. Falls Sie die Seite oder das Projekt geschlossen haben, können Sie im [Azure-Portal](https://portal.azure.com) wie folgt dorthin navigieren: Wählen Sie **Alle Dienste** und unter **Sicherheit** die Option **Key Vault** aus, und wählen Sie anschließend Ihre Key Vault-Instanz aus.
1. Wählen Sie im Abschnitt „Key Vault“ für den gerade erstellten Schlüsseltresor die Option **Geheimnisse** aus, und klicken Sie auf **Generieren/Importieren**.

   ![Generieren/Importieren eines Geheimnisses](media/vs-key-vault-add-connected-service/azure-generate-secrets.png)

1. Geben Sie ein Geheimnis (beispielsweise *MeinGeheimnis*) ein, und weisen Sie diesem testweise einen beliebigen Zeichenfolgenwert zu. Wählen Sie anschließend die Schaltfläche **Erstellen** aus.

   ![Erstellen eines Geheimnisses](media/vs-key-vault-add-connected-service/azure-create-a-secret.png)

1. (Optional) Geben Sie ein weiteres Geheimnis ein, ordnen Sie es dieses Mal jedoch einer Kategorie zu, indem Sie es *Geheimnisse – MeinGeheimnis* nennen. Diese Syntax gibt die Kategorie „Geheimnisse“ an, die ein Geheimnis namens „MeinGeheimnis“ enthält.

Jetzt können Sie in Code auf Ihre Geheimnisse zugreifen. Die nächsten Schritte unterscheiden sich abhängig davon, ob Sie ASP.NET 4.7.1 oder ASP.NET Core verwenden.

## <a name="access-your-secrets-in-code"></a>Zugreifen auf Ihre Geheimnisse im Code

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Suchen Sie auf der Registerkarte **Durchsuchen** die beiden folgenden NuGet-Pakete, und installieren Sie sie: [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) und [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

1. Wählen Sie die Registerkarte `Program.cs` aus, und ersetzen Sie die Klasse „Program“ durch den folgenden Code:

   ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
   ```

1. Öffnen Sie als Nächstes die Datei `About.cshtml.cs`, und schreiben Sie den folgenden Code:
   1. Schließen Sie mithilfe der folgenden using-Anweisung einen Verweis auf `Microsoft.Extensions.Configuration` ein:

       ```csharp
       using Microsoft.Extensions.Configuration
       ```

   1. Fügen Sie diesen Konstruktor hinzu:

       ```csharp
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```

   1. Aktualisieren Sie die Methode `OnGet`. Ersetzen Sie den hier gezeigten Platzhalterwert durch den Namen des Geheimnisses, den Sie mit den oben angegebenen Befehlen erstellt haben.

       ```csharp
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Führen Sie die App lokal aus, indem Sie zur Infoseite navigieren. Sie sollten sehen, dass Ihr Geheimniswert abgerufen wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, wenn Sie sie nicht mehr benötigen. Dadurch werden der Schlüsseltresor und die zugehörigen Ressourcen gelöscht. So löschen Sie die Ressourcengruppe über das Portal:

1. Geben Sie den Namen Ihrer Ressourcengruppe in das Suchfeld am oberen Rand des Portals ein. Klicken Sie in den Suchergebnissen auf die Ressourcengruppe aus dieser Schnellstartanleitung.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie im Feld **GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:** den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="how-your-aspnet-core-project-is-modified"></a>Ändern des ASP.NET Core-Projekts

In diesem Abschnitt werden die genauen Änderungen identifiziert, die an einem ASP.NET-Projekt vorgenommen werden, wenn der verbundene Key Vault-Dienst mit Visual Studio hinzugefügt wird.

### <a name="added-references-for-aspnet-core"></a>Hinzugefügte Verweise für ASP.NET Core

Betrifft die .NET-Verweise der Projektdatei und NuGet-Verweise.

| type | Verweis |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

### <a name="added-files-for-aspnet-core"></a>Hinzugefügte Dateien für ASP.NET Core

- `ConnectedService.json` wurde hinzugefügt. Diese Datei enthält Informationen zum Anbieter des verbundenen Diensts und zur Version sowie einen Link zur Dokumentation.

### <a name="project-file-changes-for-aspnet-core"></a>Änderungen an der Projektdatei für ASP.NET Core

- Die Elementgruppe (ItemGroup) der verbundenen Dienste und die Datei `ConnectedServices.json` wurden hinzugefügt.

### <a name="launchsettingsjson-changes-for-aspnet-core"></a>Änderungen an „launchsettings.json“ für ASP.NET Core

- Dem IIS Express-Profil und dem Profil, das Ihrem Webprojektnamen entspricht, wurde die folgende Umgebungsvariable hinzugefügt:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure-for-aspnet-core"></a>Änderungen in Azure für ASP.NET Core

- Eine Ressourcengruppe wurde erstellt (oder eine bereits vorhandene wurde verwendet).
- Eine Key Vault-Instanz wurde in der angegebenen Ressourcengruppe erstellt.

## <a name="how-your-aspnet-framework-project-is-modified"></a>Ändern des ASP.NET Framework-Projekts

In diesem Abschnitt werden die genauen Änderungen identifiziert, die an einem ASP.NET-Projekt vorgenommen werden, wenn der verbundene Key Vault-Dienst mit Visual Studio hinzugefügt wird.

### <a name="added-references-for-aspnet-framework"></a>Hinzugefügte Verweise für ASP.NET Framework

Betrifft die Projektdatei (.NET-Verweise) und `packages.config` (NuGet-Verweise).

| type | Verweis |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files-for-aspnet-framework"></a>Hinzugefügte Dateien für ASP.NET Framework

- `ConnectedService.json` wurde hinzugefügt. Diese Datei enthält Informationen zum Anbieter des verbundenen Diensts und zur Version sowie einen Link zur Dokumentation.

### <a name="project-file-changes-for-aspnet-framework"></a>Änderungen an der Projektdatei für ASP.NET Framework

- Die Elementgruppe des verbundenen Diensts und die Datei „ConnectedServices.json“ wurden hinzugefügt.
- Verweise auf die .NET-Assemblys werden im Abschnitt [Hinzugefügte Verweise](#added-references-for-aspnet-framework) beschrieben.

### <a name="webconfig-or-appconfig-changes"></a>Änderungen an „web.config“ oder „app.config“

- Die folgenden Konfigurationseinträge wurden hinzugefügt:

    ```xml
    <configSections>
      <section
           name="configBuilders"
           type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
           restartOnExternalChanges="false"
           requirePermission="false" />
    </configSections>
    <configBuilders>
      <builders>
        <add
             name="AzureKeyVault"
             vaultName="vaultname"
             type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=1.0.0.0, Culture=neutral"
             vaultUri="https://vaultname.vault.azure.net" />
      </builders>
    </configBuilders>
    ```

### <a name="changes-on-azure-for-aspnet-framework"></a>Änderungen in Azure für ASP.NET Framework

- Eine Ressourcengruppe wurde erstellt (oder eine bereits vorhandene wurde verwendet).
- Eine Key Vault-Instanz wurde in der angegebenen Ressourcengruppe erstellt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Key Vault-Entwicklung finden Sie im [Key Vault-Entwicklerhandbuch](key-vault-developers-guide.md).
