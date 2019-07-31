---
title: Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen – Azure Storage
description: Azure Blob Storage und Azure Queue Storage unterstützen das Autorisieren des Zugriffs auf Ressourcen mit Azure Active Directory und verwaltete Identitäten für Azure-Ressourcen. Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blobs und Warteschlangen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen o. Ä. ausgeführt werden.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 07/15/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 469790660e843816cc431420e7e1407c90a7de05
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249932"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen

Azure Blob Storage und Azure Queue Storage unterstützen die Azure Active Directory-Authentifizierung (Azure AD-Authentifizierung) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md). Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blob- und Warteschlangendaten mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen und anderen Diensten ausgeführt werden. Durch Verwendung von verwalteten Identitäten für Azure-Ressourcen zusammen mit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.  

In diesem Artikel wird die Autorisierung des Zugriffs auf Blob- oder Warteschlangendaten mit einer verwalteten Identität über einen virtuellen Azure-Computer erläutert.

## <a name="enable-managed-identities-on-a-vm"></a>Aktivieren von verwalteten Identitäten auf einem virtuellen Computer

Damit Sie verwaltete Identitäten für Azure-Ressourcen zum Autorisieren des Zugriffs auf Blobs und Warteschlangen über Ihren virtuellen Computer verwenden können, müssen Sie zunächst verwaltete Identitäten für Ressourcen auf dem virtuellen Computer aktivieren. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure-Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-Clientbibliotheken](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-an-azure-ad-managed-identity"></a>Erteilen von Berechtigungen für eine verwaltete Azure AD-Identität

Damit Sie eine Anforderung an den Blob- oder Warteschlangendienst über eine verwaltete Identität in Ihrer Azure Storage-Anwendung autorisieren können, konfigurieren Sie die Einstellungen der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für diese verwaltete Identität. Azure Storage definiert RBAC-Rollen, die Berechtigungen für Blob- und Warteschlangendaten beinhalten. Wenn die RBAC-Rolle einer verwalteten Identität zugewiesen wird, werden der verwalteten Identität diese Berechtigungen für Blob- oder Warteschlangendaten im entsprechenden Bereich erteilt.

Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie in den folgenden Artikeln:

- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac-portal.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über die Azure CLI](storage-auth-aad-rbac-cli.md)
- [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über PowerShell](storage-auth-aad-rbac-powershell.md)

## <a name="azure-storage-resource-id"></a>Azure Storage-Ressourcen-ID

[!INCLUDE [storage-resource-id-include](../../../includes/storage-resource-id-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>Codebeispiel für .NET: Erstellen eines Blockblobs

Das Codebeispiel zeigt, wie Sie ein OAuth 2.0-Token von Azure AD erhalten und es zum Autorisieren einer Anforderung zum Erstellen eines Blockblobs verwenden. Damit dieses Beispiel funktioniert, führen Sie zunächst die in den vorherigen Abschnitten beschriebenen Schritte aus.

Die Clientbibliothek [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) für .NET (Vorschauversion) vereinfacht den Prozess des Abrufens und Erneuerns eines Tokens aus dem Code. Mit der Clientbibliothek zur App-Authentifizierung wird die Authentifizierung automatisch verwaltet. Mit der Bibliothek werden die Anmeldeinformationen des Entwicklers für die Authentifizierung bei der lokalen Entwicklung verwendet. Während der lokalen Entwicklung sind Entwickleranmeldeinformationen sicherer, da Sie keine Azure AD-Anmeldeinformationen erstellen oder Anmeldeinformationen zwischen Entwicklern freigeben müssen. Wenn die Lösung später in Azure bereitgestellt wird, wechselt die Bibliothek automatisch zur Verwendung der Anwendungsanmeldeinformationen.

### <a name="install-packages"></a>Installieren von Paketen

Zur Verwendung der Bibliothek für die App-Authentifizierung in einer Azure Storage-Anwendung installieren Sie das neueste Vorschaupaket von [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) sowie die neueste Version der [allgemeinen Azure Storage-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/) und der [Clientbibliothek für Azure Blob Storage für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/). Fügen Sie dem Code folgende **using**-Anweisungen hinzu:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
```

### <a name="add-the-callback-method"></a>Hinzufügen der Rückrufmethode

Mit der Rückrufmethode wird die Ablaufzeit des Tokens überprüft und dieses bei Bedarf erneuert:

```csharp
private static async Task<NewTokenAndFrequency> TokenRenewerAsync(Object state, CancellationToken cancellationToken)
{
    // Specify the resource ID for requesting Azure AD tokens for Azure Storage.
    // Note that you can also specify the root URI for your storage account as the resource ID.
    const string StorageResource = "https://storage.azure.com/";  

    // Use the same token provider to request a new token.
    var authResult = await ((AzureServiceTokenProvider)state).GetAuthenticationResultAsync(StorageResource);

    // Renew the token 5 minutes before it expires.
    var next = (authResult.ExpiresOn - DateTimeOffset.UtcNow) - TimeSpan.FromMinutes(5);
    if (next.Ticks < 0)
    {
        next = default(TimeSpan);
        Console.WriteLine("Renewing token...");
    }

    // Return the new token and the next refresh time.
    return new NewTokenAndFrequency(authResult.AccessToken, next);
}
```

### <a name="get-a-token-and-create-a-block-blob"></a>Rufen Sie ein Token ab, und erstellen Sie ein Blockblob.

Die Bibliothek zur App-Authentifizierung enthält die Klasse **AzureServiceTokenProvider**. Eine Instanz dieser Klasse kann an einen Rückruf übergeben werden, mit dem ein Token abgerufen und dann vor dem Ablaufen erneuert wird.

Im folgenden Beispiel wird ein Token abgerufen. Mithilfe dieses Tokens wird ein neues Blob erstellt und dann gelesen.

```csharp
const string blobName = "https://storagesamples.blob.core.windows.net/sample-container/blob1.txt";

// Get the initial access token and the interval at which to refresh it.
AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
var tokenAndFrequency = TokenRenewerAsync(azureServiceTokenProvider,
                                            CancellationToken.None).GetAwaiter().GetResult();

// Create storage credentials using the initial token, and connect the callback function
// to renew the token just before it expires
TokenCredential tokenCredential = new TokenCredential(tokenAndFrequency.Token,
                                                        TokenRenewerAsync,
                                                        azureServiceTokenProvider,
                                                        tokenAndFrequency.Frequency.Value);

StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a blob using the storage credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName),
                                            storageCredentials);

// Upload text to the blob.
blob.UploadTextAsync(string.Format("This is a blob named {0}", blob.Name));

// Continue to make requests against Azure Storage.
// The token is automatically refreshed as needed in the background.
do
{
    // Read blob contents
    Console.WriteLine("Time accessed: {0} Blob Content: {1}",
                        DateTimeOffset.UtcNow,
                        blob.DownloadTextAsync().Result);

    // Sleep for ten seconds, then read the contents of the blob again.
    Thread.Sleep(TimeSpan.FromSeconds(10));
} while (true);
```

Weitere Informationen zur Bibliothek zur App-Authentifizierung finden Sie unter [Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET](../../key-vault/service-to-service-authentication.md).

Weitere Informationen zum Abrufen eines Sicherheitstokens finden Sie unter [Verwenden von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer zum Abrufen eines Zugriffstokens](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

> [!NOTE]
> Zum Autorisieren von Anforderungen für Blob- oder Warteschlangendaten mit Azure AD müssen Sie für die Anforderungen HTTPS verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Informationen zum Ausführen von Azure CLI- und PowerShell-Befehlen mit Azure AD-Anmeldeinformationen finden Sie unter [Ausführen von Azure CLI- oder PowerShell-Befehlen mit Azure AD-Anmeldeinformationen für den Zugriff auf Blob- oder Warteschlangendaten](storage-auth-aad-script.md).
