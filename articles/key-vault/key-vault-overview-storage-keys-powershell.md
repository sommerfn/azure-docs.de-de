---
title: Mit Azure Key Vault verwaltetes Speicherkonto – PowerShell-Version
description: Das Feature für verwaltete Speicherkonten bietet eine nahtlose Integration zwischen Azure Key Vault und einem Azure-Speicherkonto.
ms.topic: conceptual
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: 225d9b715c56e4813a8e26d881c876e7bd498155
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204219"
---
# <a name="manage-storage-account-keys-with-key-vault-and-azure-powershell"></a>Verwalten von Speicherkontoschlüsseln mit Key Vault und Azure PowerShell

Ein Azure-Speicherkonto verwendet Anmeldeinformationen, die sich aus einem Kontonamen und einem Schlüssel zusammensetzen. Der Schlüssel wird automatisch generiert und fungiert eher als ein Kennwort denn als ein kryptografischer Schlüssel. Key Vault verwaltet diese Speicherkontoschlüssel, indem sie als [Key Vault-Geheimnisse](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets) gespeichert werden. 

Sie können das Key Vault-Feature für verwaltete Speicherkontoschlüssel verwenden, um Schlüssel für ein Azure Storage-Konto aufzulisten (synchronisieren) und die Schlüssel in regelmäßigen Abständen erneut zu generieren (rotieren). Sie können Schlüssel sowohl für Speicherkonten als auch für klassische Speicherkonten verwalten.

Wenn Sie das Feature für verwaltete Speicherkontoschlüssel verwenden, sollten Sie folgende Punkte beachten:

- Schlüsselwerte werden nie als Antwort an einen Aufrufer zurückgegeben.
- Ihre Speicherkontoschlüssel sollten nur durch Key Vault verwaltet werden. Verwalten Sie die Schlüssel nicht selbst, und vermeiden Sie es, die Key Vault-Prozesse zu beeinträchtigen.
- Speicherkontoschlüssel sollten nur von einem einzigen Key Vault-Objekt verwaltet werden. Lassen Sie es nicht zu, dass die Schlüssel aus mehreren Objekten verwaltet werden.
- Sie können Key Vault anweisen, Ihr Speicherkonto mit einem Benutzerprinzipal zu verwalten, aber nicht mit einem Dienstprinzipal.
- Generieren Sie Schlüssel nur mit Key Vault neu. Generieren Sie Ihre Speicherkontoschlüssel nicht manuell neu.

Wir empfehlen die Verwendung der Azure Storage-Integration in Azure Active Directory (Azure AD), dem cloudbasierten Identitäts- und Zugriffsverwaltungsdienst von Microsoft. Die Azure AD-Integration ist für [Azure-Blobs und -Warteschlangen](../storage/common/storage-auth-aad.md) verfügbar und bietet tokenbasierten OAuth2-Zugriff auf Azure Storage (genau wie Azure Key Vault).

Azure AD ermöglicht es Ihnen, Ihre Clientanwendung zu authentifizieren, indem Sie eine Anwendungs- oder Benutzeridentität anstelle von Speicherkontoanmeldeinformationen verwenden. Sie können eine [von Azure AD verwaltete Identität](/azure/active-directory/managed-identities-azure-resources/) verwenden, wenn Sie Ihre Clientanwendung in Azure ausführen. Verwaltete Identitäten machen die Clientauthentifizierung und das Speichern von Anmeldeinformationen in oder mit Ihrer Anwendung überflüssig.

Azure AD verwendet rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC), um die Autorisierung zu verwalten. Dies wird auch von Key Vault unterstützt.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="service-principal-application-id"></a>Dienstprinzipal-Anwendungs-ID

Ein Azure AD-Mandant stellt jede registrierte Anwendung mit einem [Dienstprinzipal](/azure/active-directory/develop/developer-glossary#service-principal-object) bereit. Der Dienstprinzipal dient als Anwendungs-ID, die während der Autorisierungseinrichtung verwendet wird, um über RBAC auf andere Azure-Ressourcen zuzugreifen.

Key Vault ist eine Microsoft-Anwendung, die in allen Azure AD-Mandanten vorab registriert ist. Key Vault wird unter derselben Anwendungs-ID in jeder Azure-Cloud registriert.

| Mandanten | Cloud | Anwendungs-ID |
| --- | --- | --- |
| Azure AD | Azure Government | `7e7c393b-45d0-48b1-a35e-2905ddf8183c` |
| Azure AD | Azure, öffentlich | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |
| Andere  | Any | `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` |

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Leitfaden müssen Sie zunächst Folgendes ausführen:

- [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps?view=azps-2.6.0)
- [Erstellen eines Schlüsseltresors](quick-create-powershell.md)
- [Erstellen Sie ein Azure-Speicherkonto](../storage/common/storage-quickstart-create-account.md?tabs=azure-powershell). Der Speicherkontoname darf nur aus Kleinbuchstaben und Zahlen bestehen. Der Name muss zwischen 3 und 24 Zeichen lang sein.
      

## <a name="manage-storage-account-keys"></a>Verwalten von Speicherkontoschlüsseln

### <a name="connect-to-your-azure-account"></a>Herstellen einer Verbindung mit Ihrem Azure-Konto

Authentifizieren Sie Ihre PowerShell-Sitzung mithilfe des Cmdlets [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0). 

```azurepowershell-interactive
Connect-AzAccount
```
Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie diese mithilfe des Cmdlets [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?view=azps-2.5.0) auflisten und das Abonnement angeben, das Sie mit dem Cmdlet [Set-AzContext](/powershell/module/az.accounts/set-azcontext?view=azps-2.5.0) verwenden möchten. 

```azurepowershell-interactive
Set-AzContext -SubscriptionId <subscriptionId>
```

### <a name="set-variables"></a>Festlegen von Variablen

Legen Sie zunächst in den folgenden Schritten die Variablen fest, die von den PowerShell-Cmdlets verwendet werden sollen. Achten Sie darauf, dass Sie die Platzhalter <YourResourceGroupName>, <YourStorageAccountName> und <YourKeyVaultName> aktualisieren und „$keyVaultSpAppId“ auf `cfa8b339-82a2-471a-a3c9-0fc0be7a4093` festlegen (wie oben in [Dienstprinzipal-Anwendungs-ID](#service-principal-application-id) angegeben).

Außerdem verwenden wir die Azure PowerShell-Cmdlets [Get-AzContext](/powershell/module/az.accounts/get-azcontext?view=azps-2.6.0) und [Get-AzStorageAccount](/powershell/module/az.storage/get-azstorageaccount?view=azps-2.6.0), um Ihre Benutzer-ID und den Kontext Ihres Azure Storage-Kontos abzurufen.

```azurepowershell-interactive
$resourceGroupName = <YourResourceGroupName>
$storageAccountName = <YourStorageAccountName>
$keyVaultName = <YourKeyVaultName>
$keyVaultSpAppId = "cfa8b339-82a2-471a-a3c9-0fc0be7a4093"
$storageAccountKey = "key1"

# Get your User Id
$userId = (Get-AzContext).Account.Id

# Get a reference to your Azure storage account
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName
```

### <a name="give-key-vault-access-to-your-storage-account"></a>Gewähren von Zugriff auf Ihr Speicherkonto für Key Vault

Damit Key Vault auf Ihre Speicherkontoschlüssel zugreifen und diese verwalten kann, müssen Sie Key Vault zum Zugriff auf Ihr Speicherkonto autorisieren. Die Key Vault-Anwendung benötigt die Berechtigungen zum *Auflisten* und *Regenerieren* von Schlüsseln für Ihr Speicherkonto. Diese Berechtigungen werden über die integrierte RBAC-Dienstrolle [Speicherkonto-Schlüsseloperator](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role) aktiviert. 

Verwenden Sie das Azure PowerShell-Cmdlet [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment?view=azps-2.6.0), um dem Key Vault-Dienstprinzipal diese Rolle zuzuweisen und den Bereich auf Ihr Speicherkonto einzuschränken.

```azurepowershell-interactive
# Assign RBAC role "Storage Account Key Operator Service Role" to Key Vault, limiting the access scope to your storage account. For a classic storage account, use "Classic Storage Account Key Operator Service Role." 
New-AzRoleAssignment -ApplicationId $keyVaultSpAppId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storageAccount.Id
```

Nach erfolgreicher Rollenzuweisung sollten eine Ausgabe ähnlich wie im folgenden Beispiel angezeigt werden:

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : storage account Key Operator Service Role
RoleDefinitionId   : 81a9662b-bebf-436f-a333-f67b29880f12
ObjectId           : 93c27d83-f79b-4cb2-8dd4-4aa716542e74
ObjectType         : ServicePrincipal
CanDelegate        : False
```

Wenn Key Vault bereits der Rolle für Ihr Speicherkonto zugewiesen wurde, wird die Fehlermeldung *„Die Rollenzuweisung ist bereits vorhanden.“* angezeigt. Sie können die Rollenzuweisung auch im Azure-Portal über die Seite „Zugriffssteuerung (IAM)“ für das Speicherkonto überprüfen.  

### <a name="give-your-user-account-permission-to-managed-storage-accounts"></a>Gewähren von Benutzerkontoberechtigungen für verwaltete Speicherkonten

Verwenden Sie das Azure PowerShell-Cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.6.0), um die Key Vault-Zugriffsrichtlinie zu aktualisieren und Ihrem Benutzerkonto Speicherkontoberechtigungen zu gewähren.

```azurepowershell-interactive
# Give your user principal access to all storage account permissions, on your Key Vault instance

Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userId -PermissionsToStorage get, list, delete, set, update, regeneratekey, getsas, listsas, deletesas, setsas, recover, backup, restore, purge
```

Beachten Sie, dass im Azure-Portal auf der Seite „Zugriffsrichtlinien“ für das Speicherkonto keine Berechtigungen für Speicherkonten verfügbar sind.

### <a name="add-a-managed-storage-account-to-your-key-vault-instance"></a>Hinzufügen eines verwalteten Speicherkontos zu Ihrer Key Vault-Instanz

Verwenden Sie das Azure PowerShell-Cmdlet [Add-AzKeyVaultManagedStorageAccount](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount?view=azps-2.6.0), um ein verwaltetes Speicherkonto in der Key Vault-Instanz zu erstellen. Mit dem Schalter `-DisableAutoRegenerateKey` wird angegeben, dass die Speicherkontoschlüssel NICHT neu generiert werden.

```azurepowershell-interactive
# Add your storage account to your Key Vault's managed storage accounts

Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -DisableAutoRegenerateKey
```

Nach dem erfolgreichen Hinzufügen des Speicherkontos ohne erneute Generierung der Schlüssel sollte eine ähnliche Ausgabe wie die im folgenden Beispiel angezeigt werden:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : False
Regeneration Period : 90.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

### <a name="enable-key-regeneration"></a>Aktivieren der erneuten Schlüsselgenerierung

Wenn die Speicherkontoschlüssel in regelmäßigen Abständen von Key Vault neu generiert werden sollen, können Sie das Azure PowerShell-Cmdlet [Add-AzKeyVaultManagedStorageAccount](/powershell/module/az.keyvault/add-azkeyvaultmanagedstorageaccount?view=azps-2.6.0) verwenden, um einen Regenerierungszeitraum festzulegen. In diesem Beispiel wird der Zeitraum auf drei Tage festgelegt. Nach drei Tagen generiert Key Vault „key2“ neu und wechselt beim aktiven Schlüssel von „key2“ zu „key1“.

```azurepowershell-interactive
$regenPeriod = [System.Timespan]::FromDays(3)

Add-AzKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storageAccountName -AccountResourceId $storageAccount.Id -ActiveKeyName $storageAccountKey -RegenerationPeriod $regenPeriod
```

Nach dem erfolgreichen Hinzufügen des Speicherkontos mit erneuter Generierung der Schlüssel sollte eine ähnliche Ausgabe wie die im folgenden Beispiel angezeigt werden:

```console
Id                  : https://kvcontoso.vault.azure.net:443/storage/sacontoso
Vault Name          : kvcontoso
AccountName         : sacontoso
Account Resource Id : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgContoso/providers/Microsoft.Storage/storageAccounts/sacontoso
Active Key Name     : key1
Auto Regenerate Key : True
Regeneration Period : 3.00:00:00
Enabled             : True
Created             : 11/19/2018 11:54:47 PM
Updated             : 11/19/2018 11:54:47 PM
Tags                : 
```

## <a name="shared-access-signature-tokens"></a>SAS-Token (Shared Access Signature)

Sie können Key Vault auch anweisen, SAS-Tokens (Shared Access Signature) zu erstellen. Shared Access Signatures bieten delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Sie können Clients Zugriff auf Ressourcen unter Ihrem Speicherkonto gewähren, ohne dafür Ihre Kontoschlüssel freigeben zu müssen. Eine SAS (Shared Access Signature) bietet Ihnen eine sichere Methode zur Freigabe Ihrer Speicherressourcen, ohne Ihre Kontoschlüssel zu gefährden.

Die Befehle in diesem Abschnitt führen Sie die folgenden Aktionen:

- Legen Sie eine SAS-Definition für ein Konto fest. 
- Erstellen eines SAS-Tokens für das Konto für Blob-, Datei-, Tabellen- und Warteschlangendienst. Das Token wird für die Ressourcentypen „Dienst“, „Container“ und „Objekt“ erstellt. Das Token wird über HTTPS mit allen Berechtigungen und mit dem angegebenen Start- und Enddatum erstellt.
- Festlegen einer SAS-Definition für von Key Vault verwalteten Speicher im Tresor. Die Definition hat den Vorlagen-URI des SAS-Tokens, das erstellt wurde. Die Definition hat den SAS-Typ `account` und ist für N Tage gültig.
- Vergewissern Sie sich, dass die SAS (Shared Access Signature) in Ihrem Schlüsseltresor als Geheimnis gespeichert wurde.
- 
### <a name="set-variables"></a>Festlegen von Variablen

Legen Sie zunächst in den folgenden Schritten die Variablen fest, die von den PowerShell-Cmdlets verwendet werden sollen. Achten Sie darauf, die Platzhalter <YourStorageAccountName> und <YourKeyVaultName> zu aktualisieren.

Außerdem verwenden wir das Azure PowerShell-Cmdlets [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext?view=azps-2.6.0), um den Kontext Ihres Azure Storage-Kontos abzurufen.

```azurepowershell-interactive
$storageAccountName = <YourStorageAccountName>
$keyVaultName = <YourKeyVaultName>

$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -Protocol Https -StorageAccountKey Key1
```

### <a name="create-a-shared-access-signature-token"></a>Erstellen eines SAS-Tokens

Erstellen Sie eine SAS-Definition mithilfe des Azure PowerShell-Cmdlets [New-AzStorageAccountSASToken](/powershell/module/az.storage/new-azstorageaccountsastoken?view=azps-2.6.0).
 
```azurepowershell-interactive
$start = [System.DateTime]::Now.AddDays(-1)
$end = [System.DateTime]::Now.AddMonths(1)

$sasToken = New-AzStorageAccountSasToken -Service blob,file,Table,Queue -ResourceType Service,Container,Object -Permission "racwdlup" -Protocol HttpsOnly -StartTime $start -ExpiryTime $end -Context $storageContext
```
Der Wert von „$sasToken“ sieht in etwa wie folgt aus.

```console
?sv=2018-11-09&sig=5GWqHFkEOtM7W9alOgoXSCOJO%2B55qJr4J7tHQjCId9S%3D&spr=https&st=2019-09-18T18%3A25%3A00Z&se=2019-10-19T18%3A25%3A00Z&srt=sco&ss=bfqt&sp=racupwdl
```

### <a name="generate-a-shared-access-signature-definition"></a>Generieren einer SAS-Definition

Verwenden Sie das Azure PowerShell-Cmdlet [Set-AzKeyVaultManagedStorageSasDefinition](/powershell/module/az.keyvault/set-azkeyvaultmanagedstoragesasdefinition?view=azps-2.6.0), um eine SAS-Definition zu erstellen.  Sie können einen Namen Ihrer Wahl für den Parameter `-Name` angeben.

```azurepowershell-interactive
Set-AzKeyVaultManagedStorageSasDefinition -AccountName $storageAccountName -VaultName $keyVaultName -Name <YourSASDefinitionName> -TemplateUri $sasToken -SasType 'account' -ValidityPeriod ([System.Timespan]::FromDays(30))
```

### <a name="verify-the-shared-access-signature-definition"></a>Überprüfen der SAS-Definition

Mithilfe des Azure PowerShell-Cmdlets [Get-AzKeyVaultSecret](/powershell/module/az.keyvault/get-azkeyvaultsecret?view=azps-2.6.0) können Sie überprüfen, ob die SAS-Definition in Ihrem Schlüsseltresor gespeichert wurde.

Suchen Sie zunächst die SAS-Definition in Ihrem Schlüsseltresor.

```azurepowershell-interactive
Get-AzKeyVaultSecret -vault-name <YourKeyVaultName>
```

Das Geheimnis, das Ihrer SAS-Definition entspricht, weist die folgenden Eigenschaften auf:

```console
Vault Name   : <YourKeyVaultName>
Name         : <SecretName>
...
Content Type : application/vnd.ms-sastoken-storage
Tags         :
```

Sie können jetzt das Cmdlet [Get-AzKeyVaultSecret](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) und die `Name`-Eigenschaft des Geheimnisses verwenden, um den Inhalt dieses Geheimnisses anzuzeigen.

```azurepowershell-interactive
$secret = Get-AzKeyVaultSecret -VaultName <YourKeyVaultName> -Name <SecretName>

Write-Host $secret.SecretValueText
```

Die Ausgabe dieses Befehls zeigt die Zeichenfolge Ihrer SAS-Definition an.


## <a name="next-steps"></a>Nächste Schritte

- [Beispiele für verwaltete Speicherkontoschlüssel](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [PowerShell-Referenz zu Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
