---
title: 'Gewähren der Berechtigung zum Zugreifen auf einen Azure-Schlüsseltresor für Anwendungen: Azure Key Vault | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie vielen Anwendungen die Berechtigung zum Zugreifen auf einen Schlüsseltresor gewähren.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 09/27/2019
ms.author: mbaldwin
ms.openlocfilehash: 7c31c04137a8d36adfe41a18cbc276a45483b05b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467180"
---
# <a name="provide-key-vault-authentication-with-an-access-control-policy"></a>Bereitstellen der Key Vault-Authentifizierung mit einer Zugriffssteuerungsrichtlinie

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Die Authentifizierung einer cloudbasierten Anwendung bei Key Vault erfolgt am einfachsten mit einer verwalteten Identität. Einzelheiten hierzu finden Sie unter [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](managed-identity.md).  Wenn Sie eine lokale Anwendung erstellen, eine lokale Entwicklung durchführen oder eine verwaltete Identität aus anderen Gründen nicht verwenden können, können Sie stattdessen einen Dienstprinzipal manuell registrieren und mithilfe einer Zugriffssteuerungsrichtlinie Zugriff auf Ihren Schlüsseltresor bereitstellen.  

Key Vault unterstützt bis zu 1.024 Zugriffsrichtlinieneinträge, wobei jeder Eintrag einen eindeutigen Berechtigungssatz für einen „Prinzipal“ erteilt:   So greift beispielsweise die Konsolen-App unter [Schnellstart: Azure Key Vault-Clientbibliothek für .NET](quick-create-net.md) auf Key Vault zu.

Ausführliche Informationen zur Key Vault-Zugriffssteuerung finden Sie unter [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](overview-security.md#identity-and-access-management). Ausführliche Informationen zur Zugriffssteuerung für [Schlüssel, Geheimnisse und Zertifikate](about-keys-secrets-and-certificates.md) finden Sie unter: 

- [Schlüsselzugriffssteuerung](about-keys-secrets-and-certificates.md#key-access-control)
- [Geheimniszugriffssteuerung](about-keys-secrets-and-certificates.md#secret-access-control)
- [Zertifikatzugriffssteuerung](about-keys-secrets-and-certificates.md#certificate-access-control)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Einen Schlüsseltresor. Sie können einen vorhandenen Schlüsseltresor verwenden oder einen neuen Schlüsseltresor erstellen, indem Sie die Schritte in einem der folgenden Schnellstarts ausführen:
   - [Erstellen eines Schlüsseltresors mit der Azure CLI](quick-create-cli.md)
   - [Erstellen eines Schlüsseltresors mit Azure PowerShell](quick-create-powershell.md)
   - [Erstellen eines Schlüsseltresors im Azure-Portal](quick-create-portal.md)
- Die [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/overview). Alternativ können Sie auch das [Azure-Portal](https://portal.azure.com) verwenden.

## <a name="grant-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor

Jeder Schlüsseltresor-Zugriffsrichtlinieneintrag gewährt einem Prinzipal einen eindeutigen Satz von Berechtigungen:

- **Eine Anwendung:** Wenn die Anwendung cloudbasiert ist, verwenden Sie stattdessen nach Möglichkeit eine [verwaltete Identität für den Zugriff auf Azure Key Vault](managed-identity.md).
- **Eine Azure AD-Gruppe:** Obwohl der Schlüsseltresor nur 1.024 Zugriffsrichtlinieneinträge unterstützt, können Sie einer einzelnen Azure AD-Gruppe mehrere Anwendungen und Benutzer hinzufügen und diese Gruppe anschließend als einzelnen Eintrag zu Ihrer Zugriffssteuerungsrichtlinie hinzufügen.
- **Ein Benutzer:** Es wird **nicht empfohlen**, Benutzern direkten Zugriff auf einen Schlüsseltresor zu gewähren. Im Idealfall werden Benutzer zu einer Azure AD-Gruppe hinzugefügt, die wiederum Zugriff auf den Schlüsseltresor erhält. Weitere Informationen finden Sie unter [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](overview-security.md#identity-and-access-management).


### <a name="get-the-objectid"></a>Abrufen der Objekt-ID (objectID)

Damit Sie einer Anwendung, einer Azure AD-Gruppe oder einem Benutzer Zugriff auf Ihren Schlüsseltresor gewähren können, müssen Sie zuerst die entsprechende Objekt-ID abrufen.

#### <a name="applications"></a>ANWENDUNGEN

Die Objekt-ID für Anwendungen entspricht dem zugeordneten Dienstprinzipal. Ausführliche Informationen zu Dienstprinzipalen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md). 

Es gibt zwei Möglichkeiten zum Abrufen einer Objekt-ID für eine Anwendung.  Die erste Möglichkeit besteht darin, Ihre Anwendung bei Azure Active Directory zu registrieren. Führen Sie dafür die Schritte aus, die unter [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](../active-directory/develop/quickstart-register-app.md) beschrieben sind. Nach Abschluss der Registrierung wird die Objekt-ID als „Anwendungs-ID (Client)“ aufgeführt.

Die zweite Möglichkeit besteht darin, einen Dienstprinzipal in einem Terminalfenster zu erstellen. Verwenden Sie bei Verwendung der Azure CLI den Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac).

```azurecli-interactive
az ad sp create-for-rbac -n "http://mySP"
```

Die Objekt-ID wird in der Ausgabe als `clientID` aufgeführt.

Verwenden Sie bei Verwendung von Azure PowerShell das Cmdlet [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal?view=azps-2.7.0).


```azurepowershell-interactive
New-AzADServicePrincipal -DisplayName mySP
```

Die Objekt-ID wird in der Ausgabe als `Id` (nicht `ApplicationId`) aufgeführt.

#### <a name="azure-ad-groups"></a>Azure AD-Gruppen

Sie können einer Azure AD-Gruppe mehrere Anwendungen und Benutzer hinzufügen und der Gruppe anschließend Zugriff auf Ihren Schlüsseltresor gewähren.  Weitere Informationen finden Sie weiter unten im Abschnitt [Erstellen und Hinzufügen von Mitgliedern zu einer Azure AD-Gruppe](#creating-and-adding-members-to-an-azure-ad-group).

Verwenden Sie den Befehl [az ad group list](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-list), um die Objekt-ID einer Azure AD-Gruppe mit der Azure CLI zu ermitteln. Aufgrund der großen Anzahl von Gruppen, die sich möglicherweise in Ihrer Organisation befinden, müssen Sie auch eine Suchzeichenfolge für den Parameter `--display-name` bereitstellen.

```azurecli-interactive
az ad group list --display-name <search-string>
```
Die Objekt-ID wird im JSON-Code zurückgegeben:

```json
    "objectId": "48b21bfb-74d6-48d2-868f-ff9eeaf38a64",
    "objectType": "Group",
    "odata.type": "Microsoft.DirectoryServices.Group",
```

Verwenden Sie das Cmdlet [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup?view=azps-2.7.0), um die Objekt-ID einer Azure AD-Gruppe mit Azure PowerShell zu ermitteln. Aufgrund der großen Anzahl von Gruppen, die sich möglicherweise in Ihrer Organisation befinden, empfiehlt es sich, eine Suchzeichenfolge für den Parameter `-SearchString` bereitzustellen.

```azurepowershell-interactive
Get-AzADGroup -SearchString <search-string>
```

In der Ausgabe wird die Objekt-ID als `Id` aufgeführt:

```console
...
Id                    : 1cef38c4-388c-45a9-b5ae-3d88375e166a
...
```

#### <a name="users"></a>Benutzer

Sie können auch einen einzelnen Benutzer zu einer Zugriffssteuerungsrichtlinie eines Schlüsseltresors hinzufügen. **Hiervon wird jedoch abgeraten.** Wir empfehlen Ihnen stattdessen, Benutzer zu einer Azure AD-Gruppe und die Gruppe zu den Richtlinien hinzuzufügen.

Wenn Sie trotzdem einen Benutzer mit der Azure CLI suchen möchten, verwenden Sie den Befehl [az ad user show](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-show), und übergeben Sie die E-Mail-Adresse des Benutzers an den Parameter `--id`.


```azurecli-interactive
az ad user show --id <email-address-of-user>
```

Die Objekt-ID des Benutzers wird in der Ausgabe zurückgegeben:

```console
  ...
  "objectId": "f76a2a6f-3b6d-4735-9abd-14dccbf70fd9",
  "objectType": "User",
  ...
```

Verwenden Sie das Cmdlet [Get-AzADUser](/powershell/module/az.resources/get-azaduser?view=azps-2.7.0), um einen Benutzer mit Azure PowerShell zu finden, und übergeben Sie die E-Mail-Adresse des Benutzers an den Parameter `-UserPrincipalName`.

```azurepowershell-interactive
 Get-AzAdUser -UserPrincipalName <email-address-of-user>
```

Die Objekt-ID des Benutzers wird in der Ausgabe als `Id` zurückgegeben.

```console
...
Id                : f76a2a6f-3b6d-4735-9abd-14dccbf70fd9
Type              :
```

### <a name="give-the-principal-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für den Prinzipal

Nachdem Sie nun über eine Objekt-ID Ihres Prinzipals verfügen, können Sie eine Zugriffsrichtlinie für Ihren Schlüsseltresor erstellen, die ihm die Berechtigungen „get“, „list“, „set“ und „delete“ für Schlüssel und Geheimnisse sowie alle gewünschten zusätzlichen Berechtigungen erteilt.

Bei der Azure CLI wird dazu die Objekt-ID an den Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) übergeben.

```azurecli-interactive
az keyvault set-policy -n <your-unique-keyvault-name> --spn <ApplicationID-of-your-service-principal> --secret-permissions get list set delete --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

Bei Azure PowerShell wird dazu die Objekt-ID an das Cmdlet [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0) übergeben. 

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy –VaultName <your-key-vault-name> -PermissionsToKeys create,decrypt,delete,encrypt,get,list,unwrapKey,wrapKey -PermissionsToSecrets get,list,set,delete -ApplicationId <Id>

```

## <a name="creating-and-adding-members-to-an-azure-ad-group"></a>Erstellen und Hinzufügen von Mitgliedern zu einer Azure AD-Gruppe

Sie können eine Azure AD-Gruppe erstellen, Anwendungen und Benutzer zur Gruppe hinzufügen und der Gruppe anschließend Zugriff auf Ihren Schlüsseltresor gewähren.  Dies ermöglicht es Ihnen, einem Schlüsseltresor eine Reihe von Anwendungen als einzelnen Zugriffsrichtlinieneintrag hinzuzufügen, und es ist nicht mehr erforderlich, Benutzern direkten Zugriff auf Ihren Schlüsseltresor zu gewähren (wovon abgeraten wird). Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen](../active-directory/fundamentals/active-directory-manage-groups.md).

### <a name="additional-prerequisites"></a>Zusätzliche Voraussetzungen

Zusätzlich zu den [oben genannten Voraussetzungen](#prerequisites) benötigen Sie Berechtigungen zum Erstellen/Bearbeiten von Gruppen in Ihrem Azure Active Directory-Mandanten. Falls Sie nicht über die entsprechenden Berechtigungen verfügen, müssen Sie sich ggf. an Ihren Azure Active Directory-Administrator wenden.

Wenn Sie PowerShell verwenden möchten, benötigen Sie außerdem das [Azure AD PowerShell-Modul](https://www.powershellgallery.com/packages/AzureAD/2.0.2.50).

### <a name="create-an-azure-active-directory-group"></a>Erstellen einer Azure Active Directory-Gruppe

Erstellen Sie eine neue Azure Active Directory-Gruppe mithilfe des Azure CLI-Befehls [az ad group create](/cli/azure/ad/group?view=azure-cli-latest#az-ad-group-create) oder des Azure PowerShell-Cmdlets [New-AzureADGroup](/powershell/module/azuread/new-azureadgroup?view=azureadps-2.0).


```azurecli-interactive
az ad group create --display-name <your-group-display-name> --mail-nickname <your-group-mail-nickname>
```

```powershell
New-AzADGroup -DisplayName <your-group-display-name> -MailNickName <your-group-mail-nickname>
```

Notieren Sie sich in beiden Fällen die Gruppen-ID (GroupId) der neu erstellten Gruppen, da Sie sie für die folgenden Schritte benötigen.

### <a name="find-the-objectids-of-your-applications-and-users"></a>Ermitteln der Objekt-IDs Ihrer Anwendungen und Benutzer

Sie können die Objekt-IDs Ihrer Anwendungen mithilfe der Azure CLI mit dem Befehl [az ad sp list](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-list) und dem Parameter `--show-mine` ermitteln.

```azurecli-interactive
az ad sp list --show-mine
```

Suchen Sie nach den Objekt-IDs Ihrer Anwendungen, indem Sie Azure PowerShell mit dem Cmdlet [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal?view=azps-2.7.0) verwenden, und übergeben Sie eine Suchzeichenfolge an den Parameter `-SearchString`.

```azurepowershell-interactive
Get-AzADServicePrincipal -SearchString <search-string>
```

Führen Sie die oben unter [Benutzer](#users) beschriebenen Schritte aus, um die Objekt-IDs Ihrer Benutzer zu ermitteln.

### <a name="add-your-applications-and-users-to-the-group"></a>Hinzufügen von Anwendungen und Benutzern zur Gruppe

Fügen Sie nun die Objekt-IDs zu Ihrer neu erstellten Azure AD-Gruppe hinzu.

Verwenden Sie bei Verwendung der Azure CLI [az ad group member add](/cli/azure/ad/group/member?view=azure-cli-latest#az-ad-group-member-add), und übergeben Sie die Objekt-ID an den Parameter `--member-id`.


```azurecli-interactive
az ad group member add -g <groupId> --member-id <objectId>
```

Verwenden Sie bei Verwendung von Azure PowerShell das Cmdlet [Add-AzADGroupMember](/powershell/module/az.resources/add-azadgroupmember?view=azps-2.7.0), und übergeben Sie die Objekt-ID an den Parameter `-MemberObjectId`.

```azurepowershell-interactive
Add-AzADGroupMember -TargetGroupObjectId <groupId> -MemberObjectId <objectId> 
```

### <a name="give-the-ad-group-access-to-your-key-vault"></a>Gewähren des Zugriffs auf Ihren Schlüsseltresor für die AD-Gruppe

Gewähren Sie der AD-Gruppe abschließend Berechtigungen für Ihren Schlüsseltresor mithilfe des Azure CLI-Befehls [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) oder des Azure PowerShell-Cmdlets [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy?view=azps-2.7.0). Beispiele finden Sie im Abschnitt [Gewähren des Zugriffs auf Ihren Schlüsseltresor für die Anwendung, Azure AD-Gruppe oder den Benutzer](#give-the-principal-access-to-your-key-vault).


## <a name="next-steps"></a>Nächste Schritte

- [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](overview-security.md#identity-and-access-management)
- [Verwenden einer verwalteten App Service-Identität für den Zugriff auf Azure Key Vault](managed-identity.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [Sicherer Zugriff auf einen Schlüsseltresor](key-vault-secure-your-key-vault.md)
- [Entwicklerhandbuch zu Azure Key Vault](key-vault-developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](key-vault-best-practices.md)
