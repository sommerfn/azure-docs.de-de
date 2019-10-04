---
title: Mit Azure Key Vault verwaltetes Speicherkonto – PowerShell-Version
description: Das Feature für verwaltete Speicherkonten bietet eine nahtlose Integration zwischen Azure Key Vault und einem Azure-Speicherkonto.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: 75561c643c2b8881e4b390be885b77f2fff4971d
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673453"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Abrufen von Shared Access Signature-Token im Code

Sie können Ihr Speicherkonto mit den [SAS-Token (Shared Access Signature)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) in Ihrem Schlüsseltresor verwalten. Dieser Artikel enthält Beispiele für C#-Code, der ein SAS-Token abruft und damit Operationen durchführt.  Weitere Informationen zum Erstellen und Speichern von SAS-Token finden Sie unter [Azure-Befehlszeilenschnittstelle](key-vault-ovw-storage-keys.md) oder [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und Azure PowerShell](key-vault-overview-storage-keys-powershell.md).

## <a name="code-samples"></a>Codebeispiele

In diesem Beispiel ruft der Code ein SAS-Token aus Ihrem Schlüsselspeicher ab, verwendet es zur Erstellung eines neuen Speicherkontos und erstellt dann einen neuen Blobdienstclient.  

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<YourKeyVaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Sollte Ihr SAS-Token bald ablaufen, können Sie das SAS-Token aus Ihrem Schlüsseltresor abrufen und den Code aktualisieren.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


## <a name="next-steps"></a>Nächste Schritte
- Lernen Sie Folgendes: [Verwalten von Speicherkontoschlüsseln mit Azure Key Vault und der Azure-Befehlszeilenschnittstelle](key-vault-ovw-storage-keys.md) oder [Azure PowerShell](key-vault-overview-storage-keys-powershell.md).
- Weitere Informationen finden Sie unter [Beispiele für verwaltete Speicherkontoschlüssel](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [PowerShell-Referenz zu Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
