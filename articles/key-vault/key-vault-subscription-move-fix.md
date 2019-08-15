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
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990101"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>F: Mein Abonnement wurde von Mandant A zu Mandant B verschoben. Wie ändere ich die Mandanten-ID für meinen vorhandenen Schlüsseltresor und lege für Mandant B die richtigen ACLs für Prinzipale fest?

Wenn Sie in einem Abonnement einen neuen Schlüsseltresor erstellen, wird er automatisch an die standardmäßige Azure Active Directory-Mandanten-ID für das Abonnement gebunden. Außerdem werden auch alle Zugriffsrichtlinieneinträge an diese Mandanten-ID gebunden. Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Prinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben:

* Ändern Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren des Abonnements zugeordnet ist, in den Mandanten B.
* Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.
* Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die Mandant B zugeordnet sind.

Wenn beispielsweise der Schlüsseltresor „myvault“ in einem Abonnement enthalten ist, das von Mandant A zu Mandant B verschoben wurde, können Sie die Mandanten-ID für diesen Schlüsseltresor ändern und alte Zugriffsrichtlinien entfernen.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Da sich dieser Tresor vor der Verschiebung unter Mandant A befunden hat, lautet der ursprüngliche Wert von **$vault.Properties.TenantId** „Mandant A“, während er für **(Get-AzContext).Tenant.TenantId** „Mandant B“ lautet.

Nachdem Sie Ihren Tresor nun der richtigen Mandanten-ID zugeordnet haben und alte Zugriffsrichtlinieneinträge entfernt wurden, können Sie neue Zugriffsrichtlinieneinträge mit [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy)festlegen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault), wenn Sie Fragen zu Azure Key Vault haben.
