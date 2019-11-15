---
title: 'Schnellstart: Azure Key Vault-Clientbibliothek für .NET (v4)'
description: Erfahren Sie, wie Sie mithilfe der .NET-Clientbibliothek (v4) Geheimnisse in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 05/20/2019
ms.service: key-vault
ms.topic: quickstart
ms.openlocfilehash: c789d48656173721432779aeaba0530950527fa1
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73646926"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v4"></a>Schnellstart: Azure Key Vault-Clientbibliothek für .NET (SDK v4)

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für .NET. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren.

Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die Key Vault-Clientbibliothek für .NET ermöglicht Folgendes:

- Verbessern der Sicherheit und der Kontrolle über Schlüssel und Kennwörter
- Erstellen und Importieren von Verschlüsselungsschlüsseln in wenigen Minuten
- Verringern der Wartezeit durch Cloudskalierung und globale Redundanz
- Vereinfachen und Automatisieren von Aufgaben für SSL-/TLS-Zertifikate
- Verwenden FIPS 140-2 Level 2-zertifizierter HSMs

[API-Referenzdokumentation](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paket (NuGet)](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
* [.NET Core 2.1 SDK oder höher](https://dotnet.microsoft.com/download/dotnet-core/2.1)
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/overview)

In dieser Schnellstartanleitung wird davon ausgegangen, dass Sie Befehle für `dotnet`, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) und Windows in einem Windows-Terminal ausführen (beispielsweise [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6), [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-6) oder [Azure Cloud Shell](https://shell.azure.com/)).

## <a name="setting-up"></a>Einrichten

### <a name="create-new-net-console-app"></a>Erstellen einer neuen .NET-Konsolen-App

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE.

Erstellen Sie in einem Konsolenfenster mithilfe des Befehls `dotnet new` eine neue Konsolen-App mit dem Namen `akv-dotnet`.


```console
dotnet new console -n key-vault-console-app
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```console
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten.

```console
Build succeeded.
 0 Warning(s)
 0 Error(s)
```

### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie über das Konsolenfenster die Azure Key Vault-Clientbibliothek für .NET:

```console
dotnet add package Azure.Security.KeyVault.Secrets --version 4.0.0
```

Im Rahmen dieser Schnellstartanleitung müssen außerdem folgende Pakete installiert werden:

```console
dotnet add package Azure.Identity --version 1.0.0
```

### <a name="create-a-resource-group-and-key-vault"></a>Erstellen einer Ressourcengruppe und eines Schlüsseltresors

In dieser Schnellstartanleitung wird eine vorab erstellte Azure Key Vault-Instanz verwendet. Eine Anleitung zum Erstellen eines Schlüsseltresors finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe der Azure CLI](quick-create-cli.md), [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe von PowerShell](quick-create-powershell.md) oder [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](quick-create-portal.md). Alternativ können Sie einfach die folgenden Azure CLI-Befehle ausführen.

> [!Important]
> Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen „<your-unique-keyvault-name>“ durch den Namen Ihres Schlüsseltresors.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Eine cloudbasierte .NET-Anwendung kann am einfachsten mit einer verwalteten Identität authentifiziert werden. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](managed-identity.md). Der Einfachheit halber wird in dieser Schnellstartanleitung allerdings eine .NET-Konsolenanwendung erstellt. Zum Authentifizieren einer Desktopanwendung mit Azure müssen ein Dienstprinzipal und eine Zugriffssteuerungsrichtlinie verwendet werden.

Erstellen Sie mithilfe des Azure CLI-Befehls [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) einen Dienstprinzipal:

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

Notieren Sie sich clientId, clientSecret und tenantId, da wir diese Werte in den nachfolgenden Schritten verwenden.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Erstellen Sie eine Zugriffsrichtlinie für Ihren Schlüsseltresor, die dem Dienstprinzipal Berechtigungen erteilt, indem Sie die Client-ID an den Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) übergeben. Erteilen Sie dem Dienstprinzipal Berechtigungen zum Abrufen, Auflisten und Festlegen von Schlüsseln und Geheimnissen.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Festlegen von Umgebungsvariablen

Die DefaultAzureCredential-Methode in unserer Anwendung basiert auf drei Umgebungsvariablen: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` und `AZURE_TENANT_ID`. Legen Sie diese Variablen auf die Werte von clientId, clientSecret und tenantId fest, die Sie sich im Schritt [Erstellen eines Dienstprinzipals](#create-a-service-principal) notiert haben.

Sie müssen außerdem den Namen Ihres Schlüsseltresors als Umgebungsvariable `KEY_VAULT_NAME` speichern.

```console
setx AZURE_CLIENT_ID <your-clientID>

setx AZURE_CLIENT_SECRET <your-clientSecret>

setx AZURE_TENANT_ID <your-tenantId>

setx KEY_VAULT_NAME <your-key-vault-name>
````

Bei jedem Aufruf von `setx` sollte eine Erfolgsmeldung mit dem Hinweis angezeigt werden, dass der angegebene Wert gespeichert wurde.

## <a name="object-model"></a>Objektmodell

Mit der Azure Key Vault-Clientbibliothek für .NET können Sie Schlüssel und zugehörige Objekte wie etwa Zertifikate und Geheimnisse verwalten. In den folgenden Codebeispielen wird gezeigt, wie Sie einen Client erstellen und ein Geheimnis festlegen, abrufen und löschen.

Die gesamte Konsolen-App steht unter https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app zur Verfügung.

## <a name="code-examples"></a>Codebeispiele

### <a name="add-directives"></a>Hinzufügen von Anweisungen

Fügen Sie am Anfang Ihres Codes die folgenden Anweisungen hinzu:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=directives)]

### <a name="authenticate-and-create-a-client"></a>Authentifizieren und Erstellen eines Clients

Die Authentifizierung bei Ihrem Schlüsseltresor und die Erstellung eines Schlüsseltresorclients hängen von den Umgebungsvariablen im Schritt [Festlegen von Umgebungsvariablen](#set-environmental-variables) oben ab. Der Name Ihres Schlüsseltresors wird in den Schlüsseltresor-URI im Format „https://<Name-Ihres-Schlüsseltresors>.vault.azure.net“ erweitert.

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=authenticate)]

### <a name="save-a-secret"></a>Speichern eines Geheimnisses

Nachdem Ihre Anwendung authentifiziert wurde, können Sie Ihrem Schlüsseltresor mithilfe der [client.SetSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync)-Methode ein Geheimnis hinzufügen. Hierzu benötigen Sie einen Namen für das Geheimnis. In diesem Beispiel verwenden wir „mySecret“.  

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=setsecret)]

Mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) können Sie sich vergewissern, dass das Geheimnis festgelegt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Abrufen eines Geheimnisses

Nun können Sie den zuvor festgelegten Wert mithilfe der [client.GetSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync)-Methode abrufen.

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=getsecret)]

Ihr Geheimnis ist jetzt als `secret.Value` gespeichert.

### <a name="delete-a-secret"></a>Löschen eines Geheimnisses

Abschließend löschen wir das Geheimnis mithilfe der [client.DeleteSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync)-Methode aus dem Schlüsseltresor.

[!code-csharp[Delete secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=deletesecret)]

Sie können sich mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) vergewissern, dass das Geheimnis entfernt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, können Sie Azure CLI oder Azure PowerShell verwenden, um Ihren Schlüsseltresor und die zugehörige Ressourcengruppe zu entfernen.

```azurecli
az group delete -g "myResourceGroup" -l "EastUS" 
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Beispielcode

```csharp
using System;
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

namespace key_vault_console_app
{
    class Program
    {
        static void Main(string[] args)
        {
            string secretName = "mySecret";

            string keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
            var kvUri = "https://" + keyVaultName + ".vault.azure.net";

            var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());

            Console.Write("Input the value of your secret > ");
            string secretValue = Console.ReadLine();

            Console.Write("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");

            client.SetSecret(secretName, secretValue);

            Console.WriteLine(" done.");

            Console.WriteLine("Forgetting your secret.");
            secretValue = "";
            Console.WriteLine("Your secret is '" + secretValue + "'.");

            Console.WriteLine("Retrieving your secret from " + keyVaultName + ".");

            KeyVaultSecret secret = client.GetSecret(secretName);

            Console.WriteLine("Your secret is '" + secret.Value + "'.");

            Console.Write("Deleting your secret from " + keyVaultName + " ...");

            client.StartDeleteSecret(secretName);

            System.Threading.Thread.Sleep(5000);
            Console.WriteLine(" done.");

        }
    }
}
```


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Schlüsseltresor erstellt, ein Geheimnis gespeichert und dieses Geheimnis abgerufen. Die gesamte Konsolen-App steht in [GitHub](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app) zur Verfügung.

Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET](service-to-service-authentication.md)
- [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](key-vault-developers-guide.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [Bewährte Methoden zum Verwenden von Key Vault](key-vault-best-practices.md)
