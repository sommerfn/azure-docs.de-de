---
title: Verwenden einer systemseitig zugewiesenen verwalteten Identität für den Zugriff auf Azure Key Vault
description: Erfahren Sie, wie Sie eine verwaltete Identität für App Service-Anwendungen erstellen und wie Sie damit auf Azure Key Vault zugreifen können.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 6c7a9fdb5ed60023a82984fd5be5b424c634e679
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720246"
---
# <a name="provide-key-vault-authentication-with-a-managed-identity"></a>Bereitstellen der Key Vault-Authentifizierung mit einer verwalteten Identität

Durch eine verwaltete Entität aus Azure Active Directory kann Ihre App mühelos auf andere durch Azure AD geschützte Ressourcen zugreifen. Da die Identität von der Azure-Plattform verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. Weitere Informationen finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md). 

Dieser Artikel zeigt, wie Sie eine verwaltete Identität für App Service-Anwendungen erstellen und damit auf Azure Key Vault zugreifen können. Für in Azure-VMs gehostete Anwendungen finden Sie weitere Informationen unter [Verwenden einer systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers zum Zugreifen auf Azure Key Vault](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-nonaad.md).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen 

Für diesen Leitfaden benötigen Sie die folgenden Ressourcen. 

- Einen Schlüsseltresor. Sie können einen vorhandenen Schlüsseltresor verwenden oder einen neuen Schlüsseltresor erstellen, indem Sie die Schritte in einem der folgenden Schnellstarts ausführen:
   - [Erstellen eines Schlüsseltresors mit der Azure CLI](quick-create-cli.md)
   - [Erstellen eines Schlüsseltresors mit Azure PowerShell](quick-create-powershell.md)
   - [Erstellen eines Schlüsseltresors im Azure-Portal](quick-create-portal.md)
- Eine vorhandene App Service-Anwendung, der der Zugriff auf den Schlüsseltresor gewährt werden soll. Sie können schnell eine Anwendung erstellen, indem Sie die Schritte in der [App Service-Dokumentation](../app-service/overview.md) ausführen.
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/overview) Alternativ können Sie auch das [Azure-Portal](http://portal.azure.com) verwenden.


## <a name="adding-a-system-assigned-identity"></a>Hinzufügen einer systemseitig zugewiesenen Identität 

Zunächst müssen Sie einer Anwendung eine systemseitig zugewiesene Identität hinzufügen. 
 
### <a name="azure-portal"></a>Azure-Portal 

Um eine verwaltete Entität im Portal einzurichten, erstellen Sie wie gewohnt zuerst eine Anwendung und aktivieren dann das Feature. 

1. Wenn Sie eine Funktionen-App verwenden, navigieren Sie zu **Plattformfeatures**. Scrollen Sie bei anderen App-Typen in der linken Navigationsleiste nach unten zur Gruppe **Einstellungen**. 

1. Wählen Sie **Verwaltete Identität** aus. 

1. Ändern Sie auf der Registerkarte **Systemseitig zugewiesen** den **Status** in **Ein**. Klicken Sie auf **Speichern**. 

    ![](./media/managed-identity-system-assigned.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Für diesen Schnellstart ist Version 2.0.4 oder höher der Azure CLI erforderlich. Führen Sie `az --version` aus, um Ihre aktuelle Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). 

Verwenden Sie den Befehl [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login), um sich bei der Azure CLI anzumelden:

```azurecli-interactive
az login
```

Weitere Informationen zu den Anmeldeoptionen für die Azure CLI finden Sie unter [Anmelden bei der Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest). 

Um die Identität für diese Anwendung zu erstellen, verwenden Sie den Azure CLI-Befehl [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) oder [az functionapp identity assign](/cli/azure/functionapp/identity?view=azure-cli-latest#az-functionapp-identity-assign):


```azurecli-interactive
az webapp identity assign --name myApp --resource-group myResourceGroup
```

```azurecli-interactive
az functionapp identity assign --name myApp --resource-group myResourceGroup
```

Notieren Sie sich den Wert von `PrincipalId`, der im nächsten Abschnitt benötigt wird.

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```
## <a name="grant-your-app-access-to-key-vault"></a>Gewähren des Zugriffs auf Key Vault für Ihre App 

### <a name="azure-portal"></a>Azure-Portal

1.  Navigieren Sie zur Key Vault-Ressource. 

1.  Wählen Sie **Zugriffsrichtlinien** aus, und klicken Sie auf **Zugriffsrichtlinie hinzufügen**. 

1.  Wählen Sie unter **Geheimnisberechtigungen** die Option **Abrufen, Liste** aus. 

1.  Wählen Sie **Prinzipal auswählen** aus, und geben Sie im Suchfeld den Namen der App ein.  Wählen Sie die App in der Ergebnisliste aus, und klicken Sie auf **Auswählen**. 

1.  Klicken Sie auf **Hinzufügen**, um die neue Zugriffsrichtlinie hinzuzufügen.

    ![](./media/managed-identity-access-policy.png)

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Um Ihrer Anwendung Zugriff auf den Schlüsseltresor zu gewähren, verwenden Sie den Azure CLI-Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) und geben den Parameter **ObjectId** mit der **principalId** an, die Sie sich oben notiert haben.

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <PrincipalId> --secret-permissions get list 
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Key Vault – Sicherheit: Identitäts- und Zugriffsverwaltung](overview-security.md#identity-and-access-management)
- [Bereitstellen der Key Vault-Authentifizierung mit einer Zugriffssteuerungsrichtlinie](key-vault-group-permissions-for-apps.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [Sicherer Zugriff auf einen Schlüsseltresor](key-vault-secure-your-key-vault.md)
- [Entwicklerhandbuch zu Azure Key Vault](key-vault-developers-guide.md)
- [Bewährte Methoden zum Verwenden von Key Vault](key-vault-best-practices.md)