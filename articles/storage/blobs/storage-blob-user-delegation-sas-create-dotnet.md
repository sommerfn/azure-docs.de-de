---
title: Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau) – Azure Storage
description: Erfahren Sie, wie Sie mit der .NET-Clientbibliothek eine Shared Access Signature (SAS) für die Benutzerdelegierung mithilfe von Azure Active Directory-Anmeldeinformationen in Azure Storage erstellen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 98ab93bbec8da17dde93c9c343703838b0279994
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900426"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

In diesem Artikel wird beschrieben, wie Sie Azure Active Directory-Anmeldeinformationen (Azure AD) verwenden, um eine SAS für die Benutzerdelegierung für einen Container oder ein Blob mit der [Azure Storage-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Storage.Blobs) zu erstellen.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-preview-packages"></a>Installieren der Vorschaupakete

In den Beispielen in diesem Artikel wird die neueste Vorschauversion der Azure Storage-Clientbibliothek für Blob Storage verwendet. Führen Sie zum Installieren des Vorschaupakets den folgenden Befehl an der NuGet-Paket-Manager-Konsole aus:

```
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

In den Beispielen in diesem Artikel wird auch die neueste Vorschauversion der [Azure Identity-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Identity/) zur Authentifizierung mit Azure AD-Anmeldeinformationen verwendet. Mit der Azure Identity-Clientbibliothek wird ein Sicherheitsprinzipal authentifiziert. Der authentifizierte Sicherheitsprinzipal kann dann die SAS für die Benutzerdelegierung erstellen. Weitere Informationen zur Azure Identity-Clientbibliothek finden Sie unter [Azure Identity-Clientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

```
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Verwenden Sie für die Authentifizierung mit Azure AD-Anmeldeinformationen über die Azure Identity-Clientbibliothek entweder einen Dienstprinzipal oder eine verwaltete Identität als Sicherheitsprinzipal, je nachdem, wo der Code ausgeführt wird. Wenn der Code in einer Entwicklungsumgebung ausgeführt wird, verwenden Sie einen Dienstprinzipal zu Testzwecken. Wenn der Code in Azure ausgeführt wird, verwenden Sie eine verwaltete Identität. In diesem Artikel wird davon ausgegangen, dass Code über die Entwicklungsumgebung ausgeführt wird. Außerdem wird erläutert, wie Sie die SAS für die Benutzerdelegierung mithilfe eines Dienstprinzipals erstellen.

Rufen Sie den Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) auf, um einen Dienstprinzipal über die Azure-Befehlszeilenschnittstelle zu erstellen und eine RBAC-Rolle zuzuweisen. Geben Sie eine Azure Storage-Datenzugriffsrolle an, die dem neuen Dienstprinzipal zugewiesen werden soll. Die Rolle muss die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** enthalten. Weitere Informationen zu den für Azure Storage bereitgestellten integrierten Rollen finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/built-in-roles.md).

Geben Sie außerdem den Bereich für die Rollenzuweisung an. Der Dienstprinzipal erstellt den Benutzerdelegierungsschlüssel, bei dem es sich um einen Vorgang handelt, der auf der Ebene des Speicherkontos ausgeführt wird. Daher sollte die Rollenzuweisung auf der Ebene des Speicherkontos, der Ressourcengruppe oder des Abonnements festgelegt werden. Weitere Informationen zu RBAC-Berechtigungen zum Erstellen einer SAS für die Benutzerdelegierung finden Sie im Abschnitt **Zuweisen von Berechtigungen mit RBAC** unter [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas).

Wenn Sie nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle zum Dienstprinzipal verfügen, müssen Sie möglicherweise den Kontobesitzer oder den Administrator bitten, die Rollenzuweisung vorzunehmen.

Im folgenden Beispiel wird ein neuer Dienstprinzipal über die Azure-Befehlszeilenschnittstelle erstellt. Außerdem wird ihm die Rolle **Storage Blob Data Reader** im Kontobereich zugewiesen.

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Mit dem Befehl `az ad sp create-for-rbac` wird eine Liste der Dienstprinzipaleigenschaften im JSON-Format zurückgegeben. Kopieren Sie diese Werte, damit Sie sie zum Erstellen der erforderlichen Umgebungsvariablen im nächsten Schritt verwenden können.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```

> [!IMPORTANT]
> Die RBAC-Rollenzuweisungen können einige Minuten dauern.

## <a name="set-environment-variables"></a>Festlegen von Umgebungsvariablen

Über die Azure Identity-Clientbibliothek werden zur Laufzeit Werte aus drei Umgebungsvariablen gelesen, um den Dienstprinzipal zu authentifizieren. In der folgenden Tabelle sind die für die einzelnen Umgebungsvariablen festzulegenden Werte beschrieben.

|Umgebungsvariable|Wert
|-|-
|`AZURE_CLIENT_ID`|Die App-ID für den Dienstprinzipal
|`AZURE_TENANT_ID`|Die Azure AD-Mandanten-ID des Dienstprinzipals
|`AZURE_CLIENT_SECRET`|Das für den Dienstprinzipal generierte Kennwort

> [!IMPORTANT]
> Nachdem Sie die Umgebungsvariablen festgelegt haben, schließen Sie das Konsolenfenster, und öffnen Sie es dann erneut. Wenn Sie Visual Studio oder eine andere Entwicklungsumgebung verwenden, müssen Sie möglicherweise die Entwicklungsumgebung neu starten, damit die neuen Umgebungsvariablen registriert werden.

## <a name="add-using-directives"></a>Hinzufügen von using-Direktiven

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Vorschauversionen der Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

## <a name="authenticate-the-service-principal"></a>Authentifizieren des Dienstprinzipals

Erstellen Sie zum Authentifizieren des Dienstprinzipals eine Instanz der [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential)-Klasse. Der Konstruktor `DefaultAzureCredential` liest die Umgebungsvariablen, die Sie zuvor erstellt haben.

Im folgenden Codeausschnitt sehen Sie, wie Sie die authentifizierten Anmeldeinformationen abrufen und damit einen Dienstclient für Blob Storage erstellen.

```csharp
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Abrufen des Benutzerdelegierungsschlüssels

Jede SAS wird mit einem Schlüssel signiert. Zum Erstellen einer SAS für die Benutzerdelegierung müssen Sie zuerst einen Benutzerdelegierungsschlüssel anfordern, der dann zum Signieren der SAS verwendet wird. Der Benutzerdelegierungsschlüssel ist analog zum Kontoschlüssel, mit dem eine Dienst-SAS oder eine Konto-SAS signiert wird, mit der Ausnahme, dass er auf Ihren Azure AD-Anmeldeinformationen basiert. Wenn ein Client einen Benutzerdelegierungsschlüssel mithilfe eines OAuth 2.0-Tokens anfordert, gibt Azure Storage den Benutzerdelegierungsschlüssel im Namen des Benutzers zurück.

Nachdem Sie den Benutzerdelegierungsschlüssel abgerufen haben, können Sie mit diesem Schlüssel während seiner Lebensdauer eine beliebige Anzahl von Shared Access Signatures (SAS) für die Benutzerdelegierung erstellen. Der Benutzerdelegierungsschlüssel ist unabhängig vom OAuth 2.0-Token, das zum Abrufen des Schlüssels verwendet wird. Daher muss das Token nicht erneuert werden, solange der Schlüssel gültig ist. Sie können angeben, dass der Schlüssel für einen Zeitraum von bis zu 7 Tagen gültig ist.

Fordern Sie den Benutzerdelegierungsschlüssel mit einer der folgenden Methoden an:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

Im folgenden Codeausschnitt werden der Benutzerdelegierungsschlüssel abgerufen und die zugehörigen Eigenschaften geschrieben:

```csharp
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStart);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="create-the-sas-token"></a>Erstellen des SAS-Tokens

Der folgende Codeausschnitt zeigt, wie ein neuer [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) erstellt wird und die Parameter für die SAS für die Benutzerdelegierung angegeben werden. Im Codeausschnitt werden dann die [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) aufgerufen, um die SAS-Tokenzeichenfolge abzurufen. Schließlich wird mit dem Code der gesamte URI erstellt, einschließlich der Ressourcenadresse und dem SAS-Token.

```csharp
BlobSasBuilder builder = new BlobSasBuilder()
{
    ContainerName = containerName,
    BlobName = blobName,
    Permissions = "r",
    Resource = "b",
    StartTime = DateTimeOffset.UtcNow,
    ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
};

string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

UriBuilder fullUri = new UriBuilder()
{
    Scheme = "https",
    Host = string.Format("{0}.blob.core.windows.net", accountName),
    Path = string.Format("{0}/{1}", containerName, blobName),
    Query = sasToken
};
```

## <a name="example-get-a-user-delegation-sas"></a>Beispiel: Abrufen einer SAS für die Benutzerdelegierung

In der folgenden Beispielmethode ist der gesamte Code für die Authentifizierung des Dienstprinzipals und die Erstellung der SAS für die Benutzerdelegierung zu sehen:

```csharp
async static Task<Uri> GetUserDelegationSasBlob(string accountName, string containerName, string blobName)
{
    // Construct the blob endpoint from the account name.
    string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

    // Create a new Blob service client with Azure AD credentials.  
    BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint), 
                                                            new DefaultAzureCredential());

    // Get a user delegation key for the Blob service that's valid for seven days.
    // Use the key to generate any number of shared access signatures over the lifetime of the key.
    UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                       DateTimeOffset.UtcNow.AddDays(7));

    // Read the key's properties.
    Console.WriteLine("User delegation key properties:");
    Console.WriteLine("Key signed start: {0}", key.SignedStart);
    Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
    Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
    Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
    Console.WriteLine("Key signed service: {0}", key.SignedService);
    Console.WriteLine("Key signed version: {0}", key.SignedVersion);

    // Create a SAS token that's valid a short interval.
    BlobSasBuilder sasBuilder = new BlobSasBuilder()
    {
        ContainerName = containerName,
        BlobName = blobName,
        Permissions = "r",
        Resource = "b",
        StartTime = DateTimeOffset.UtcNow,
        ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
    };

    // Use the key to get the SAS token.
    string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

    // Construct the full URI, including the SAS token.
    UriBuilder fullUri = new UriBuilder()
    {
        Scheme = "https",
        Host = string.Format("{0}.blob.core.windows.net", accountName),
        Path = string.Format("{0}/{1}", containerName, blobName),
        Query = sasToken
    };

    Console.WriteLine("User delegation SAS URI: {0}", fullUri);
    return fullUri.Uri;
}
```

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Beispiel: Lesen eines Blobs mit einer SAS für die Benutzerdelegierung

Im folgenden Beispiel wird die im vorherigen Beispiel erstellte SAS für die Benutzerdelegierung in einer simulierten Clientanwendung getestet. Wenn die SAS gültig ist, können die Inhalte des Blobs in der Clientanwendung gelesen werden. Wenn die SAS ungültig ist, z. B. weil sie abgelaufen ist, wird in Azure Storage der Fehlercode 403 (Verboten) zurückgegeben.

```csharp
private static async Task ReadBlobWithSasAsync(Uri sasUri)
{
    // Try performing blob operations using the SAS provided.

    // Create a blob client object for blob operations.
    BlobClient blobClient = new BlobClient(sasUri, null);

    // Download and read the contents of the blob.
    try
    {
        // Download blob contents to a stream and read the stream.
        BlobDownloadInfo blobDownloadInfo = await blobClient.DownloadAsync();
        using (StreamReader reader = new StreamReader(blobDownloadInfo.Content, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }

        Console.WriteLine();
        Console.WriteLine("Read operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (StorageRequestFailedException e)
    {
        // Check for a 403 (Forbidden) error. If the SAS is invalid, 
        // Azure Storage returns this error.
        if (e.Status == 403)
        {
            Console.WriteLine("Read operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Weitere Informationen

- [Vorgang zum Abrufen eines Benutzerdelegierungsschlüssels](/rest/api/storageservices/get-user-delegation-key)
- [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas)
