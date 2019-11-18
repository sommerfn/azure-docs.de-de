---
title: 'Schnellstart: Azure Key Vault-Clientbibliothek für .NET (SDK v3)'
description: Erfahren Sie, wie Sie mithilfe der .NET-Clientbibliothek (v3) Geheimnisse in einer Azure Key Vault-Instanz erstellen, daraus abrufen und löschen.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 11/05/2019
ms.service: key-vault
ms.topic: quickstart
ms.openlocfilehash: 7546d5c6089a7867b1f8b5bf45d23c81bddd0da9
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647901"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v3"></a>Schnellstart: Azure Key Vault-Clientbibliothek für .NET (SDK v3)

Hier finden Sie Informationen zu den ersten Schritten mit der Azure Key Vault-Clientbibliothek für .NET. Führen Sie die weiter unten beschriebenen Schritte aus, um das Paket zu installieren und den Beispielcode für grundlegende Aufgaben auszuprobieren.

> [!NOTE]
> In dieser Schnellstartanleitung wird die Version v 3.0.4 der Microsoft.Azure.KeyVault-Clientbibliothek verwendet. Wenn Sie die aktuelle Version der Key Vault-Clientbibliothek verwenden möchten, finden Sie weitere Informationen unter [Schnellstart: Azure Key Vault-Clientbibliothek für .NET](quick-create-net.md). 

Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Die Key Vault-Clientbibliothek für .NET ermöglicht Folgendes:

- Verbessern der Sicherheit und der Kontrolle über Schlüssel und Kennwörter
- Erstellen und Importieren von Verschlüsselungsschlüsseln in wenigen Minuten
- Verringern der Wartezeit durch Cloudskalierung und globale Redundanz
- Vereinfachen und Automatisieren von Aufgaben für SSL-/TLS-Zertifikate
- Verwenden FIPS 140-2 Level 2-zertifizierter HSMs

[API-Referenzdokumentation](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/)

> [!NOTE]
> Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen „<your-unique-keyvault-name>“ durch den Namen Ihres Schlüsseltresors.


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
dotnet new console -n akvdotnet
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
dotnet add package Microsoft.Azure.KeyVault
```

Im Rahmen dieser Schnellstartanleitung müssen außerdem folgende Pakete installiert werden:

```console
dotnet add package System.Threading.Tasks
dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
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

Notieren Sie sich die Client-ID (clientId) und das Clientgeheimnis (clientSecret). Sie werden weiter unten im Schritt [Authentifizieren bei Ihrem Schlüsseltresor](#authenticate-to-your-key-vault) benötigt.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Dienstprinzipal

Erstellen Sie eine Zugriffsrichtlinie für Ihren Schlüsseltresor, die dem Dienstprinzipal Berechtigungen erteilt, indem Sie die Client-ID an den Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) übergeben. Erteilen Sie dem Dienstprinzipal Berechtigungen zum Abrufen, Auflisten und Festlegen von Schlüsseln und Geheimnissen.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

## <a name="object-model"></a>Objektmodell

Mit der Azure Key Vault-Clientbibliothek für .NET können Sie Schlüssel und zugehörige Objekte wie etwa Zertifikate und Geheimnisse verwalten. In den folgenden Codebeispielen wird gezeigt, wie Sie ein Geheimnis festlegen und ein Geheimnis abrufen.

Die gesamte Konsolen-App steht unter https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet zur Verfügung.

## <a name="code-examples"></a>Codebeispiele

### <a name="add-directives"></a>Hinzufügen von Anweisungen

Fügen Sie am Anfang Ihres Codes die folgenden Anweisungen hinzu:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=directives)]

### <a name="authenticate-to-your-key-vault"></a>Authentifizieren bei Ihrem Schlüsseltresor

In dieser .NET-Schnellstartanleitung werden Umgebungsvariablen verwendet, um Anmeldeinformationen zu speichern, die nicht im Code platziert werden sollten. 

Legen Sie vor dem Erstellen und Ausführen Ihrer App mithilfe des Befehls `setx` die Umgebungsvariablen `akvClientId`, `akvClientSecret`, `akvTenantId` und `akvSubscriptionId` auf die Werte fest, die Sie weiter oben notiert haben.

```console
setx akvClientId <your-clientID>

setx akvClientSecret <your-clientSecret>
````

Bei jedem Aufruf von `setx` sollte eine Erfolgsmeldung mit dem Hinweis angezeigt werden, dass der angegebene Wert gespeichert wurde.

Weisen Sie diese Umgebungsvariablen zu Zeichenfolgen in Ihrem Code zu, und übergeben Sie sie an die Klasse [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient), um Ihre Anwendung zu authentifizieren:

[!code-csharp[Authentication](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=authentication)]

### <a name="save-a-secret"></a>Speichern eines Geheimnisses

Nachdem Ihre Anwendung authentifiziert wurde, können Sie Ihrem Schlüsseltresor mithilfe der Methode [SetSecretAsync](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) ein Geheimnis hinzufügen. Hierzu benötigen Sie die URL Ihres Schlüsseltresors (im Format `https://<your-unique-keyvault-name>.vault.azure.net/secrets/`). Außerdem ist ein Name für das Geheimnis erforderlich. (In diesem Beispiel wird „mySecret“ verwendet.)  Es empfiehlt sich gegebenenfalls, diese Zeichenfolgen einer Variablen zuzuweisen, um sie wiederverwenden zu können.

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=setsecret)]

Mithilfe des Befehls [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) können Sie sich vergewissern, dass das Geheimnis festgelegt wurde:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Abrufen eines Geheimnisses

Nun können Sie den zuvor festgelegten Wert mithilfe der Methode [GetSecretAsync](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync) abrufen.

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=getsecret)]

Ihr Geheimnis ist jetzt als `keyvaultSecret.Value;` gespeichert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcen nicht mehr benötigen, können Sie Azure CLI oder Azure PowerShell verwenden, um Ihren Schlüsseltresor und die zugehörige Ressourcengruppe zu entfernen.

```azurecli
az group delete -g "myResourceGroup" -l "EastUS" 
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Schlüsseltresor erstellt, ein Geheimnis gespeichert und dieses Geheimnis abgerufen. Die gesamte Konsolen-App steht in [GitHub](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet) zur Verfügung.

Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET](service-to-service-authentication.md)
- [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](key-vault-developers-guide.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [Bewährte Methoden zum Verwenden von Key Vault](key-vault-best-practices.md)
