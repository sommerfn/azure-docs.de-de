---
title: 'Ändern der Mandanten-ID des Schlüsseltresors nach einem Abonnementwechsel: Azure Key Vault | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Mandanten-ID für einen Schlüsseltresor ändern, nachdem ein Abonnement in einen anderen Mandanten verschoben wurde.
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 3819742e82fe6877b6a1aa58e52eec01b6b05515
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001247"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


Wenn Sie in einem Abonnement einen neuen Schlüsseltresor erstellen, wird er automatisch an die standardmäßige Azure Active Directory-Mandanten-ID für das Abonnement gebunden. Außerdem werden auch alle Zugriffsrichtlinieneinträge an diese Mandanten-ID gebunden. 

Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Prinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben:

* Ändern Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren im Abonnement zugeordnet ist, in den Mandanten B.
* Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.
* Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die Mandant B zugeordnet sind.

Wenn beispielsweise der Schlüsseltresor „myvault“ in einem Abonnement enthalten ist, das von Mandant A zu Mandant B verschoben wurde, können Sie Azure PowerShell verwenden, um die Mandanten-ID zu ändern und alte Zugriffsrichtlinien zu entfernen.

```azurepowershell
Select-AzSubscription -SubscriptionId <your-subscriptionId>                # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your key vault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your key vault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your key vault resides in
$vault.Properties.AccessPolicies = @()                                     # Access policies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                             # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the key vault's properties.
````

Alternativ können Sie die Azure-Befehlszeilenschnittstelle verwenden.

```azurecli
az account set <your-subscriptionId>                                       # Select your Azure Subscription
tenantId=$(az account show --query tenantId)                               # Get your tenantId
az keyvault update -n myvault --remove Properties.accessPolicies           # Remove the access policies
az keyvault update -n myvault --set Properties.tenantId=$tenantId          # Update the key vault tenantId
```

Nachdem Sie Ihren Tresor nun der richtigen Mandanten-ID zugeordnet haben und alte Zugriffsrichtlinieneinträge entfernt wurden, können Sie neue Zugriffsrichtlinieneinträge mit dem Azure PowerShell-Cmdlet [Set-AzKeyVaultAccessPolicy](https://powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) oder dem Azure CLI-Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) festlegen.

Wenn Sie eine verwaltete Identität für Azure-Ressourcen verwenden, müssen Sie sie ebenfalls auf den neuen Azure AD-Mandanten aktualisieren. Weitere Informationen zu verwalteten Identitäten finden Sie unter [Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität](managed-identity.md).


Wenn Sie MSI verwenden, müssen Sie auch die MSI-Identität aktualisieren, da sich die alte Identität nicht mehr im richtigen AAD-Mandanten befindet.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault), wenn Sie Fragen zu Azure Key Vault haben.
