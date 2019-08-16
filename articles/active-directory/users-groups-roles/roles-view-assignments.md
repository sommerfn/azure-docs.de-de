---
title: Anzeigen von Berechtigungen der Administratorrolle im Admin Center | Microsoft-Dokumentation
description: Sie können jetzt Mitglieder einer Azure AD-Administratorrolle im Azure AD Admin Center anzeigen und verwalten.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf45d45481712e30d40bacec9a3c4d80d1ed56b6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707431"
---
# <a name="view-custom-role-assignments-in-azure-active-directory"></a>Anzeigen der Zuweisungen benutzerdefinierter Rollen in Azure Active Directory

In diesem Artikel wird beschrieben, wie Sie benutzerdefinierte Rollen anzeigen, die Sie in Azure Active Directory (Azure AD) zugewiesen haben. In Azure Active Directory (Azure AD) können Rollen auf Verzeichnisebene oder im Bereich einer einzelnen Anwendung zugewiesen werden. Rollenzuweisungen im Verzeichnisbereich werden der Liste mit Rollenzuweisungen für eine einzelne Anwendung hinzugefügt. Rollenzuweisungen im Bereich einer einzelnen Anwendungen werden jedoch nicht zur Liste der Zuweisungen auf Verzeichnisebene hinzugefügt.

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-the-azure-ad-portal"></a>Anzeigen der Zuweisungen einer Rolle im Verzeichnisbereich mithilfe des Azure AD-Portals

1. Melden Sie sich beim  [Azure AD Admin Center](https://aad.portal.azure.com) mit Berechtigungen vom Typ „Administrator für privilegierte Rollen“ oder „Globaler Administrator“ in der Azure AD-Organisation an.
1. Wählen Sie  **Azure Active Directory** >  **Rollen und Administratoren** und anschließend eine Rolle aus, um sie zu öffnen und ihre Eigenschaften anzuzeigen.
1. Wählen Sie **Zuweisungen** aus, um die Zuweisungen für die Rolle anzuzeigen.

    ![Anzeigen von Rollenzuweisungen und Berechtigungen beim Öffnen einer Rolle in der Liste](./media/roles-view-assignments/role-assignments.png)

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-azure-ad-powershell"></a>Anzeigen der Zuweisungen einer Rolle im Verzeichnisbereich mithilfe von Azure AD PowerShell

Sie können die Zuweisung von Azure AD-Administratorrollen an Benutzer mithilfe von Azure PowerShell automatisieren. Dieser Artikel verwendet das [Azure Active Directory PowerShell Version 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles)-Modul.

### <a name="prepare-powershell"></a>Vorbereiten von PowerShell

Zunächst müssen Sie das [Azure AD PowerShell-Vorschaumodul herunterladen](https://www.powershellgallery.com/packages/AzureAD/).

Verwenden Sie die folgenden Befehle, um das Azure AD PowerShell-Modul zu installieren:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Überprüfen Sie mithilfe des folgenden Befehls, ob das Modul verwendet werden kann:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>Anzeigen der Zuweisungen einer Rolle

Beispiel für das Anzeigen der Zuweisungen einer Rolle:

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-microsoft-graph-api"></a>Anzeigen der Zuweisungen einer Rolle im Verzeichnisbereich mithilfe der Microsoft Graph-API

HTTP-Anforderung zum Abrufen einer Rollenzuweisung für eine bestimmte Rollendefinition

GET

``` HTTP
https://graph.windows.net/<tenantDomain-or-tenantId>/roleAssignments?api-version=1.61-internal&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

response

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview"></a>Anzeigen der Zuweisungen einer Rolle im Bereich einer einzelnen Anwendung mithilfe des Azure AD-Portals (Vorschau)

1. Melden Sie sich beim  [Azure AD Admin Center](https://aad.portal.azure.com) mit Berechtigungen vom Typ „Administrator für privilegierte Rollen“ oder „Globaler Administrator“ in der Azure AD-Organisation an.
1. Wählen Sie Azure Active Directory aus. Wählen Sie anschließend **App-Registrierungen** und dann die gewünschte App-Registrierung aus, um ihre Eigenschaften anzuzeigen. Möglicherweise müssen Sie die Option **Alle Anwendungen** auswählen, um die vollständige Liste der App-Registrierungen in ihrer Azure AD-Organisation anzuzeigen.

    ![Erstellen oder Bearbeiten von App-Registrierungen auf der Seite „App-Registrierungen“](./media/roles-create-custom/appreg-all-apps.png)

1. Wählen Sie  **Rollen und Administratoren** und anschließend eine Rolle aus, um ihre Eigenschaften anzuzeigen.

    ![Anzeigen der Rollenzuweisungen einer App-Registrierung auf der Seite „App-Registrierungen“](./media/roles-view-assignments/appreg-assignments.png)

1. Wählen Sie **Zuweisungen** aus, um die Zuweisungen für die Rolle anzuzeigen.

    ![Anzeigen der Rollenzuweisungen einer App-Registrierung über die Eigenschaften einer App-Registrierung](./media/roles-view-assignments/appreg-assignments-2.png)

## <a name="next-steps"></a>Nächste Schritte

* Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
* Weitere Informationen zu Rollen und zur Zuweisung von Administratorrollen finden Sie unter [Zuweisen von Administratorrollen](directory-assign-admin-roles.md).
* Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
