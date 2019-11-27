---
title: Entfernen von Einschränkungen beim Erstellen von App-Registrierungen – Azure AD | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine benutzerdefinierte Rolle zuweisen, um uneingeschränkte App-Registrierungen in Azure Active Directory (AD) zuzulassen.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12803e2f65e17155e8bbcaf4842789adc101b0dd
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74024410"
---
# <a name="quickstart-grant-permission-to-create-unlimited-app-registrations"></a>Schnellstart: Erteilen einer Berechtigung zum Erstellen unbegrenzter App-Registrierungen

In dieser Schnellstartanleitung wird eine benutzerdefinierte Rolle mit der Berechtigung zum Erstellen einer unbegrenzten Anzahl von App-Registrierungen erstellt und anschließend einem Benutzer zugewiesen. Der zugewiesene Benutzer kann dann das Azure AD-Portal, Azure AD PowerShell, die Azure AD Graph-API oder die Microsoft Graph-API verwenden, um Anwendungsregistrierungen zu erstellen. Im Gegensatz zur integrierten Rolle „Anwendungsentwickler“ ermöglicht diese Rolle die Erstellung einer unbegrenzten Anzahl von Anwendungsregistrierungen. Die Rolle „Anwendungsentwickler“ ermöglicht zwar die Erstellung, die Gesamtanzahl erstellter Objekte ist jedoch auf 250 beschränkt, um zu verhindern, dass das [verzeichnisweite Objektkontingent](directory-service-limits-restrictions.md) erreicht wird.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisite"></a>Voraussetzung

Die am wenigsten privilegierte Rolle zum Erstellen und Zuweisen benutzerdefinierter Azure AD-Rollen ist „Administrator für privilegierte Rollen“.

## <a name="create-a-new-custom-role-using-the-azure-ad-portal"></a>Erstellen einer neuen benutzerdefinierten Rolle über das Azure AD-Portal

1. Melden Sie sich beim  [Azure AD Admin Center](https://aad.portal.azure.com) mit Berechtigungen vom Typ „Administrator für privilegierte Rollen“ oder „Globaler Administrator“ in der Azure AD-Organisation an.
1. Wählen Sie **Azure Active Directory** > **Rollen und Administratoren** > **Neue benutzerdefinierte Rolle** aus.

    ![Erstellen oder Bearbeiten von Rollen auf der Seite „Rollen und Administratoren“](./media/roles-create-custom/new-custom-role.png)

1. Geben Sie auf der Registerkarte **Grundlagen** den Rollennamen „Application Registration Creator“ (Anwendungsregistrierungsersteller) und die Rollenbeschreibung „Can create an unlimited number of application registrations“ (Kann eine unbegrenzte Anzahl von Anwendungsregistrierungen erstellen) ein, und wählen Sie anschließend **Weiter** aus.

    ![Angeben eines Namens und einer Beschreibung für eine benutzerdefinierte Rolle auf der Registerkarte „Grundlagen“](./media/roles-quickstart-app-registration-limits/basics-tab.png)

1. Geben Sie auf der Registerkarte **Berechtigungen** die Zeichenfolge „microsoft.directory/applications/create“ in das Suchfeld ein. Aktivieren Sie die Kontrollkästchen neben den gewünschten Berechtigungen, und klicken Sie anschließend auf **Weiter**.

    ![Auswählen der Berechtigungen für eine benutzerdefinierte Rolle auf der Registerkarte „Berechtigungen“](./media/roles-quickstart-app-registration-limits/permissions-tab.png)

1. Überprüfen Sie die Berechtigungen auf der Registerkarte **Überprüfen + erstellen**, und wählen Sie **Erstellen** aus.

### <a name="assign-the-role-to-a-user-using-the-azure-ad-portal"></a>Zuweisen der Rolle zu einem Benutzer über das Azure AD-Portal

1. Melden Sie sich beim  [Azure AD Admin Center](https://aad.portal.azure.com) mit Berechtigungen vom Typ „Administrator für privilegierte Rollen“ oder „Globaler Administrator“ in Ihrer Azure AD-Organisation an.
1. Wählen Sie **Azure Active Directory** > **Rollen und Administratoren** aus.
1. Wählen Sie die Rolle „Application Registration Creator“ (Anwendungsregistrierungsersteller) und anschließend **Zuweisung hinzufügen** aus.
1. Wählen Sie den gewünschten Benutzer aus, und klicken Sie auf **Auswählen**, um ihn der Rolle hinzuzufügen.

Das war's! In dieser Schnellstartanleitung haben Sie eine benutzerdefinierte Rolle mit der Berechtigung zum Erstellen einer unbegrenzten Anzahl von App-Registrierungen erstellt und sie anschließend einem Benutzer zugewiesen.

> [!TIP]
> Wenn Sie die Rolle einer Anwendung über das Azure AD-Portal zuweisen möchten, geben Sie den Namen der Anwendung in das Suchfeld der Zuweisungsseite ein. Anwendungen werden standardmäßig nicht in der Liste angezeigt, aber in den Suchergebnissen zurückgegeben.

### <a name="app-registration-permissions"></a>App-Registrierungsberechtigungen

Für die Erstellung von Anwendungsregistrierungen stehen zwei Berechtigungen zur Verfügung, die sich jeweils unterschiedlich verhalten.

- microsoft.directory/applications/createAsOwner: Wird diese Berechtigung zugewiesen, wird der Ersteller als erster Besitzer der erstellten App-Registrierung hinzugefügt, und die erstellte App-Registrierung zählt zum Objekterstellungskontingent des Erstellers (250 Objekte).
- microsoft.directory/applicationPolicies/create: Wird diese Berechtigung zugewiesen, wird der Ersteller nicht als erster Besitzer der erstellten App-Registrierung hinzugefügt, und die erstellte App-Registrierung zählt nicht zum Kontingent von 250 erstellten Objekten des Erstellers. Verwenden Sie diese Berechtigung mit Bedacht, da der zugewiesene Benutzer in diesem Fall so viele App-Registrierungen erstellen kann, bis das Kontingent auf der Verzeichnisebene erreicht ist. Sind beide Berechtigungen zugewiesen, hat diese Berechtigung Vorrang.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Erstellen einer benutzerdefinierten Rolle mithilfe von Azure AD PowerShell

Verwenden Sie zum Erstellen einer neuen Rolle das folgende PowerShell-Skript:

``` PowerShell
# Basic role information
$description = "Application Registration Creator"
$displayName = "Can create an unlimited number of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/createAsOwner"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>Zuweisen der benutzerdefinierten Rolle mithilfe von Azure AD PowerShell

#### <a name="prepare-powershell"></a>Vorbereiten von PowerShell

Installieren Sie zunächst das Azure AD PowerShell-Modul aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.17). Importieren Sie anschließend das Azure AD PowerShell-Vorschaumodul mithilfe des folgenden Befehls:

```powershell
import-module azureadpreview
```

Gleichen Sie die Version, die durch den folgenden Befehl zurückgegeben wird, mit der hier aufgeführten Version ab, um sich zu vergewissern, dass das Modul verwendungsbereit ist:

```powershell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azureadpreview               {Add-AzureADAdministrati...}
```

#### <a name="assign-the-custom-role"></a>Zuweisen der benutzerdefinierten Rolle

Weisen Sie die Rolle mithilfe des folgenden PowerShell-Skripts zu:

``` PowerShell
# Basic role information
$description = "Application Registration Creator"
$displayName = "Can create an unlimited number of application registrations."
$templateId = (New-Guid).Guid

# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/create"
)
$resourceActions = @{'allowedResourceActions'= $allowedResourceAction}
$rolePermission = @{'resourceActions' = $resourceActions}
$rolePermissions = $rolePermission

# Create new custom admin role
$customRole = New-AzureAdRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="create-a-custom-role-using-microsoft-graph-api"></a>Erstellen einer benutzerdefinierten Rolle mithilfe der Microsoft Graph-API

HTTP-Anforderung zum Erstellen der benutzerdefinierten Rolle:

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
```

Body

```HTTP
{
    "description":"Can create an unlimited number of application registrations.",
    "displayName":"Application Registration Creator",
    "isEnabled":true,
    "rolePermissions":
    [
        {
            "resourceActions":
            {
                "allowedResourceActions":
                [
                    "microsoft.directory/applications/create"
                ]
            },
            "condition":null
        }
    ],
    "templateId":"<PROVIDE NEW GUID HERE>",
    "version":"1"
}
```

### <a name="assign-the-custom-role-using-microsoft-graph-api"></a>Zuweisen der benutzerdefinierten Rolle mithilfe der Microsoft Graph-API

Die Rollenzuweisung kombiniert eine Sicherheitsprinzipal-ID (bei der es sich um einen Benutzer- oder um einen Dienstprinzipal handeln kann), eine Rollendefinitions-ID (Rollen-ID) und einen Azure AD-Ressourcenbereich.

HTTP-Anforderung zum Zuweisen einer benutzerdefinierten Rolle:

POST

``` HTTP
https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
```

Body

``` HTTP
{
    "principalId":"<PROVIDE OBJECTID OF USER TO ASSIGN HERE>",
    "roleDefinitionId":"<PROVIDE OBJECTID OF ROLE DEFINITION HERE>",
    "resourceScopes":["/"]
}
```

## <a name="next-steps"></a>Nächste Schritte

- Im [Forum für Azure AD-Administratorrollen](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) können Sie sich gerne mit uns in Verbindung setzen.
- Weitere Informationen zu Rollen und zur Zuweisung von Administratorrollen finden Sie unter [Zuweisen von Administratorrollen](directory-assign-admin-roles.md).
- Informationen zu Standardbenutzerberechtigungen finden Sie unter [Vergleich von Standardbenutzerberechtigungen für Gäste und Mitglieder](../fundamentals/users-default-permissions.md).
