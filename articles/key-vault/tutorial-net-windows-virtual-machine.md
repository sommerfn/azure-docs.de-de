---
title: 'Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Windows-Computer in .NET | Microsoft-Dokumentation'
description: In diesem Tutorial konfigurieren Sie eine ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Ihrem Schlüsseltresor.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: e2e730c623b6ee802938b930c4874f6c4920cc64
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934401"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-net"></a>Tutorial: Verwenden von Azure Key Vault mit einem virtuellen Windows-Computer in .NET

Azure Key Vault unterstützt Sie beim Schutz von Geheimnissen wie API-Schlüsseln und Datenbank-Verbindungszeichenfolgen, die für den Zugriff auf Ihre Anwendungen, Dienste und IT-Ressourcen benötigt werden.

In diesem Tutorial wird beschrieben, wie Sie vorgehen müssen, damit eine Konsolenanwendung Informationen aus Azure Key Vault lesen kann. Verwenden Sie hierfür verwaltete Identitäten für Azure-Ressourcen. 

Das Tutorial veranschaulicht folgende Vorgehensweisen:

> [!div class="checklist"]
> * Erstellen Sie eine Ressourcengruppe.
> * Erstellen eines Schlüsseltresors
> * Hinzufügen eines Geheimnisses zum Schlüsseltresor.
> * Abrufen eines Geheimnisses aus dem Schlüsseltresor
> * Erstellen Sie einen virtuellen Azure-Computer.
> * Aktivieren einer [verwalteten Identität](../active-directory/managed-identities-azure-resources/overview.md) für den virtuellen Computer
> * Zuweisen von Berechtigungen zur VM-Identität

Machen Sie sich zunächst mit den [grundlegenden Konzepten von Key Vault](key-vault-whatis.md#basic-concepts) vertraut. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Für Windows, Mac und Linux:
  * [Git-Client](https://git-scm.com/downloads)
  * Für dieses Tutorial ist es erforderlich, dass Sie die Azure CLI lokal ausführen. Hierfür muss Azure CLI Version 2.0.4 oder höher installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](https://review.docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Informationen zur verwalteten Dienstidentität

Azure Key Vault dient zum sicheren Speichern von Anmeldeinformationen, damit diese nicht in Ihrem Code erscheinen. Sie müssen sich jedoch bei Azure Key Vault authentifizieren, um Ihre Schlüssel abzurufen. Und für die Authentifizierung bei Key Vault benötigen Sie wiederum Anmeldeinformationen. Ein klassisches Bootstrap-Dilemma. Die verwaltete Dienstidentität (Managed Service Identity, MSI) löst dieses Problem, indem sie eine _Bootstrap-Identität_ bereitstellt, die den Prozess vereinfacht.

Wenn Sie MSI für einen Azure-Dienst aktivieren (beispielsweise für Azure Virtual Machines, Azure App Service oder Azure Functions), erstellt Azure einen [Dienstprinzipal](key-vault-whatis.md#basic-concepts). MSI führt diesen Schritt für die Instanz des Diensts in Azure Active Directory (Azure AD) aus und fügt die Anmeldeinformationen des Dienstprinzipals in diese Instanz ein. 

![MSI](media/MSI.png)

Als Nächstes ruft Ihr Code einen lokalen, für die Azure-Ressource verfügbaren Metadatendienst auf, um ein Zugriffstoken zu erhalten. Ihr Code verwendet das vom lokalen MSI-Endpunkt erhaltene Zugriffstoken für die Authentifizierung bei einem Azure Key Vault-Dienst. 

## <a name="create-resources-and-assign-permissions"></a>Erstellen von Ressourcen und Zuweisen von Berechtigungen

Bevor Sie mit der Programmierung beginnen, müssen Sie einige Ressourcen erstellen, ein Geheimnis in Ihren Schlüsseltresor legen und Berechtigungen zuweisen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. 

Die Ressourcengruppe wird in diesem Beispiel am Standort „USA, Westen“ erstellt:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Ihre neu erstellte Ressourcengruppe wird im gesamten Tutorial verwendet.

### <a name="create-a-key-vault-and-populate-it-with-a-secret"></a>Erstellen eines Schlüsseltresors und Auffüllen mit einem Geheimnis

Um einen Schlüsseltresor in Ihrer Ressourcengruppe zu erstellen, verwenden Sie den Befehl [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) mit folgenden Informationen:

* Name des Schlüsseltresors: eine Zeichenfolge mit 3 bis 24 Zeichen, die nur Zahlen (0–9), Buchstaben (a–z, A–Z) und Bindestriche (-) enthalten darf.
* Ressourcengruppenname
* Standort: **USA, Westen**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Schlüsseltresor autorisiert.

Fügen Sie nun Ihrem Schlüsseltresor mit dem Befehl [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) ein Geheimnis hinzu.


Geben Sie den folgenden Befehl ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Dieses Geheimnis speichert den Wert **MySecret**.

### <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers
Erstellen Sie mit einer der folgenden Methoden einen virtuellen Computer:

* [Die Azure-CLI](../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../virtual-machines/windows/quick-create-powershell.md)
* [Azure-Portal](../virtual-machines/windows/quick-create-portal.md)

### <a name="assign-an-identity-to-the-vm"></a>Zuweisen einer Identität zum virtuellen Computer
Erstellen Sie für den virtuellen Computer mit dem Befehl [az vm identity assign](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) eine vom System zugewiesene Identität:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Beachten Sie die systemseitig zugewiesene Identität, die im folgenden Code angezeigt wird. Die Ausgabe des obigen Befehls lautet etwa wie folgt: 

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

### <a name="assign-permissions-to-the-vm-identity"></a>Zuweisen von Berechtigungen für die VM-Identität
Weisen Sie Ihrem Schlüsseltresor mit dem Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) die zuvor erstellten Identitätsberechtigungen zu:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Anmelden beim virtuellen Computer

Führen Sie die Anweisungen unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](../virtual-machines/windows/connect-logon.md) aus, um sich beim virtuellen Computer anzumelden.

## <a name="set-up-the-console-app"></a>Einrichten der Konsolen-App

Erstellen Sie eine Konsolen-App, und installieren Sie die erforderlichen Pakete mit dem `dotnet`-Befehl.

### <a name="install-net-core"></a>.NET Core installieren

Wechseln Sie zur [.NET-Downloadseite](https://www.microsoft.com/net/download), um .NET Core zu installieren.

### <a name="create-and-run-a-sample-net-app"></a>Erstellen und Ausführen einer .NET-Beispiel-App

Öffnen Sie eine Eingabeaufforderung.

Sie können „Hallo Welt“ auf der Konsole ausgeben, indem Sie die folgenden Befehle ausführen:

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

### <a name="install-the-packages"></a>Installieren der Pakete

 Installieren Sie im Konsolenfenster die für diesen Schnellstart erforderlichen .NET-Pakete:

 ```console
dotnet add package System.IO;
dotnet add package System.Net;
dotnet add package System.Text;
dotnet add package Newtonsoft.Json;
dotnet add package Newtonsoft.Json.Linq;
```

## <a name="edit-the-console-app"></a>Bearbeiten der Konsolen-App

Öffnen Sie die Datei *Program.cs*, und fügen Sie die folgenden Pakete hinzu:

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Bearbeiten Sie in den folgenden beiden Schritten die Klassendatei so, dass sie den Code enthält:

1. Rufen Sie ein Token vom lokalen MSI-Endpunkt auf dem virtuellen Computer ab. Dadurch wird auch ein Token von Azure AD abgerufen.
1. Übergeben Sie das Token an Ihren Schlüsseltresor, und rufen Sie anschließend Ihr Geheimnis ab. 

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from the local (URI) Managed Service Identity endpoint, which in turn fetches it from Azure AD
            var token = GetToken();

            // Step 2: Fetch the secret value from your key vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```

Der Code oben veranschaulicht, wie Sie mit Azure Key Vault Vorgänge auf einem virtuellen Windows-Computer ausführen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den virtuellen Computer und Ihren Schlüsseltresor, wenn diese nicht mehr benötigt werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Key Vault-REST-API](https://docs.microsoft.com/rest/api/keyvault/)
