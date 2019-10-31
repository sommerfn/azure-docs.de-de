---
title: Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau) – Azure Storage
description: Erfahren Sie, wie Sie mit der .NET-Clientbibliothek eine Shared Access Signature (SAS) für die Benutzerdelegierung mithilfe von Azure Active Directory-Anmeldeinformationen in Azure Storage erstellen.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: c75a13a20c1dbb222db69145e24838deb111fb66
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595210"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit .NET (Vorschau)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

In diesem Artikel erfahren Sie, wie Sie unter Verwendung von Azure AD-Anmeldeinformationen (Azure Active Directory) eine SAS für die Benutzerdelegierung für einen Container oder ein Blob mit der Azure Storage-Clientbibliothek für .NET erstellen.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="authenticate-with-the-azure-identity-library-preview"></a>Authentifizieren mit der Azure Identity-Bibliothek (Vorschauversion)

Die Azure Identity-Clientbibliothek für .NET (Vorschauversion) dient zur Authentifizierung eines Sicherheitsprinzipals. Wenn Ihr Code in Azure ausgeführt wird, ist der Sicherheitsprinzipal eine verwaltete Identität für Azure-Ressourcen.

Wenn Ihr Code in der Entwicklungsumgebung ausgeführt wird, erfolgt die Authentifizierung ggf. automatisch. Abhängig von den verwendeten Tools kann aber auch eine Browseranmeldung erforderlich sein. Microsoft Visual Studio unterstützt einmaliges Anmelden (Single Sign-On, SSO), sodass automatisch das aktive Azure AD-Benutzerkonto für die Authentifizierung verwendet wird. Weitere Informationen zu SSO finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](../../active-directory/manage-apps/what-is-single-sign-on.md).

Bei anderen Entwicklungstools ist ggf. eine Anmeldung über einen Webbrowser erforderlich. Für die Authentifizierung über die Entwicklungsumgebung kann auch ein Dienstprinzipal verwendet werden. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../../active-directory/develop/howto-create-service-principal-portal.md).

Nach der Authentifizierung erhält die Azure Identity-Clientbibliothek Tokenanmeldeinformationen. Diese Tokenanmeldeinformationen werden im Dienstclientobjekt gekapselt, das Sie erstellen, um Vorgänge für Azure Storage auszuführen. Die Bibliothek führt diesen Schritt automatisch für Sie durch und ruft die entsprechenden Tokenanmeldeinformationen ab.

Weitere Informationen zur Azure Identity-Clientbibliothek finden Sie unter [Azure Identity-Clientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

## <a name="assign-rbac-roles-for-access-to-data"></a>Zuweisen von RBAC-Rollen für den Datenzugriff

Wenn ein Azure AD-Sicherheitsprinzipal versucht, auf Blobdaten zuzugreifen, muss dieser Sicherheitsprinzipal über Berechtigungen für die Ressource verfügen. Dem Sicherheitsprinzipal muss eine RBAC-Rolle zugewiesen werden, die den Zugriff auf Blobdaten in Azure Storage ermöglicht. Dabei spielt es keine Rolle, ob es sich bei dem Sicherheitsprinzipal um eine verwaltete Identität in Azure oder um ein Azure AD-Benutzerkonto handelt, mit dem Code in der Entwicklungsumgebung ausgeführt wird. Informationen zur Zuweisung von Berechtigungen per RBAC finden Sie unter **Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory** im Abschnitt [Zuweisen von RBAC-Rollen für Zugriffsrechte](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

## <a name="install-the-preview-packages"></a>Installieren der Vorschaupakete

In den Beispielen in diesem Artikel wird die neueste Vorschauversion der [Azure Storage-Clientbibliothek für Blob Storage](https://www.nuget.org/packages/Azure.Storage.Blobs) verwendet. Führen Sie zum Installieren des Vorschaupakets den folgenden Befehl an der NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

In den Beispielen in diesem Artikel wird auch die neueste Vorschauversion der [Azure Identity-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Identity/) zur Authentifizierung mit Azure AD-Anmeldeinformationen verwendet. Führen Sie zum Installieren des Vorschaupakets den folgenden Befehl an der NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Identity -IncludePrerelease
```

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

## <a name="get-an-authenticated-token-credential"></a>Abrufen authentifizierter Tokenanmeldeinformationen

Erstellen Sie eine Instanz der Klasse [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential), um Tokenanmeldeinformationen abzurufen, die in Ihrem Code zum Autorisieren von Anforderungen für Azure Storage verwendet werden können.

Im folgenden Codeausschnitt sehen Sie, wie Sie die authentifizierten Tokenanmeldeinformationen abrufen und damit einen Dienstclient für Blob Storage erstellen:

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
