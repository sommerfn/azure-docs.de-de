---
title: Zuweisen von Azure AD-Rollen in Privileged Identity Management – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure AD-Rollen in Azure AD Privileged Identity Management (PIM) zuweisen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61efc9d9d12cb67ea9c22e44ba0060db0f224f73
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554311"
---
# <a name="assign-azure-ad-roles-in-privileged-identity-management"></a>Zuweisen von Azure AD-Rollen in Privileged Identity Management

Mit Azure Active Directory (Azure AD) kann ein globaler Administrator **dauerhafte** Rollenzuweisungen für Azure AD-Administratoren vornehmen. Diese Rollenzuweisungen können über das [Azure-Portal](../users-groups-roles/directory-assign-admin-roles.md) oder über [PowerShell-Befehle](/powershell/module/azuread#directory_roles) erstellt werden.

Der Dienst Azure AD Privileged Identity Management (PIM) ermöglicht es auch Administratoren für privilegierte Rollen, dauerhafte Administratorrollen zuzuweisen. Außerdem können Administratoren für privilegierte Rollen Benutzer als für Azure AD-Administratorrollen **berechtigt** festlegen. Ein berechtigter Administrator kann die Rolle bei Bedarf aktivieren, und die entsprechenden Berechtigungen laufen nach einem bestimmten Zeitraum ab.

## <a name="make-a-user-eligible-for-a-role"></a>Festlegen eines Benutzers als „berechtigt“ für eine Rolle

Führen Sie folgende Schritte aus, um einen Benutzer als für eine Azure AD-Administratorrolle „berechtigt“ festzulegen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit einem Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ist.

    Informationen dazu, wie Sie anderen Administratoren Zugriff zum Verwalten von Privileged Identity Management gewähren, finden Sie unter [Gewähren von Zugriff für andere Administratoren zum Verwalten von PIM](pim-how-to-give-access-to-pim.md).

1. Öffnen Sie **Azure AD Privileged Identity Management**.

    Wenn Sie Privileged Identity Management im Azure-Portal noch nie gestartet haben, informieren Sie sich zunächst auf der Seite [Einstieg in Privileged Identity Management](pim-getting-started.md).

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Rollen** oder **Mitglieder**.

    ![Azure AD-Rollen mit hervorgehobenen Menüoptionen „Rollen“ und „Mitglieder“](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Klicken Sie auf **Mitglied hinzufügen**, um das Dialogfeld „Verwaltete Mitglieder hinzufügen“ zu öffnen.

1. Klicken Sie auf **Rolle auswählen**, klicken Sie auf eine Rolle, die Sie verwalten möchten, und klicken Sie dann auf **Auswählen**.

    ![Bereich „Rolle auswählen“ mit einer Liste der Azure AD-Rollen](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Klicken Sie auf **Mitglieder auswählen**, wählen Sie die Benutzer aus, die der Rolle zugewiesen werden sollen, und klicken Sie dann auf **Auswählen**.

    ![Bereich „Mitglieder auswählen“, in dem Sie einen Benutzer auswählen können](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Klicken Sie in „Verwaltete Mitglieder hinzufügen“ auf **OK**, um den Benutzer zur Rolle hinzuzufügen.

1. Klicken Sie in der Liste der Rollen auf die soeben zugewiesene Rolle, um die Liste der Mitglieder anzuzeigen.

     Nach dem Zuweisen der Rolle wird der von Ihnen ausgewählte Benutzer in der Liste als **Berechtigt** für die Rolle angezeigt.

    ![Mitglieder einer Rolle sind zusammen mit ihrem Aktivierungszustand aufgeführt](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Nachdem der Benutzer nun für die Rolle berechtigt ist, informieren Sie ihn darüber, dass er die Rolle wie unter [Aktivieren meiner Azure AD-Rollen in PIM](pim-how-to-activate-role.md) beschrieben aktivieren kann.

    Berechtigte Administratoren werden aufgefordert, sich während der Aktivierung für die Multi-Factor Authentication (MFA) von Azure zu registrieren. Wenn sich ein Benutzer nicht für MFA registrieren kann oder ein Microsoft-Konto nutzt (in der Regel @outlook.com), müssen Sie ihn in allen Rollen als permanent einrichten.

## <a name="make-a-role-assignment-permanent"></a>Einrichten einer permanenten Rollenzuweisung

Standardmäßig sind neue Benutzer nur für eine Azure AD-Administratorrolle berechtigt. Führen Sie die folgenden Schritte aus, wenn Sie eine Rollenzuweisung permanent festlegen möchten.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Klicken Sie auf **Azure AD-Rollen**.

1. Klicken Sie auf **Mitglieder**.

    ![Liste „Mitglieder“ von Azure AD-Rollen mit Rolle und Aktivierungszustand](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Klicken Sie auf eine als **Berechtigt** markierte Rolle, die Sie als dauerhafte Rollenzuweisung festlegen möchten.

1. Klicken Sie auf **Weitere**, und klicken Sie anschließend auf **Als permanent festlegen**.

    ![Bereich mit einem Benutzer, der für eine Rolle berechtigt ist, mit geöffneter Menüoption „Mehr“](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    Die Rolle wird jetzt als **permanent** aufgeführt.

    ![Liste der Mitglieder mit Rolle und nun permanentem Aktivierungszustand](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Entfernen eines Benutzers aus einer Rolle

Sie können Benutzer aus Rollenzuweisungen entfernen. Stellen Sie dabei jedoch sicher, dass immer mindestens ein Benutzer als permanenter globaler Administrator vorhanden ist. Wenn Sie nicht sicher sind, welche Benutzer ihre Rollenzuweisungen noch benötigen, können Sie [eine Zugriffsüberprüfung für die Rolle starten](pim-how-to-start-security-review.md).

Führen Sie folgende Schritte aus, um einen Benutzer aus einer Azure AD-Administratorrolle zu entfernen.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure AD-Rollen** aus.

1. Wählen Sie **Mitglieder** aus.

    ![Liste „Mitglieder“ von Azure AD-Rollen mit Rolle und Aktivierungszustand](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Wählen Sie die zu entfernende Rollenzuweisung aus.

1. Wählen Sie **Mehr** > **Entfernen** aus.

    ![Bereich mit einem Benutzer mit einer permanenten Rolle, mit geöffneter Menüoption „Mehr“](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung der Aktion aufgefordert werden.

    ![Meldung, die fragt, ob Sie das Mitglied aus der Rolle entfernen möchten](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    Die Rollenzuweisung wurde entfernt.

## <a name="authorization-error-when-assigning-roles"></a>Autorisierungsfehler beim Zuweisen von Rollen

Wenn Sie Privileged Identity Management erst kürzlich für ein Azure-Abonnement aktiviert haben und beim Versuch, einen Benutzer einer Azure AD-Administratorrolle zuzuweisen, einen Autorisierungsfehler erhalten, verfügt der MS-PIM-Dienstprinzipal möglicherweise noch nicht über die erforderlichen Berechtigungen. Für die Rollenzuweisung muss dem MS-PIM-Dienstprinzipal in der rollenbasierten Zugriffssteuerung von Azure die Rolle [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) für den Ressourcenzugriff zugewiesen werden (anstelle von Azure AD-Administratorrollen). Anstatt zu warten, bis PIM die Rolle „Benutzerzugriffsadministrator“ zugewiesen wurde, können Sie sie auch manuell zuweisen.

Gehen Sie wie folgt vor, um dem MS-PIM-Dienstprinzipal für ein Abonnement die Rolle „Benutzerzugriffsadministrator“ zuzuweisen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator in Ihrer Azure AD-Organisation an.

1. Wählen Sie **Alle Dienste** und dann **Abonnements** aus.

1. Wählen Sie Ihr Abonnement aus.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Wählen Sie **Rollenzuweisungen** aus, um die aktuelle Liste der Rollenzuweisungen im Abonnementbereich anzuzeigen.

   ![Blatt „Zugriffssteuerung (IAM)“ für ein Abonnement](./media/pim-how-to-add-role-to-user/ms-pim-access-control.png)

1. Überprüfen Sie, ob der **MS-PIM**-Dienstprinzipal der Rolle **Benutzerzugriffsadministrator** zugewiesen ist.

1. Falls nicht, wählen Sie **Rollenzuweisung hinzufügen** aus, um den Bereich **Rollenzuweisung hinzufügen** zu öffnen.

1. Wählen Sie in der Dropdownliste **Rolle** die Rolle **Benutzerzugriffsadministrator** aus.

1. Wählen Sie in der Liste **Auswählen** den Dienstprinzipal **MS-PIM** aus.

   ![Bereich „Rollenzuweisung hinzufügen“: Hinzufügen von Berechtigungen für den Dienstprinzipal „MS-PIM“](./media/pim-how-to-add-role-to-user/ms-pim-add-permissions.png)

1. Wählen Sie zum Zuweisen einer Rolle **Speichern** aus.

   Nach kurzer Zeit wird dem MS-PIM-Dienstprinzipal im Abonnementbereich die Rolle „Benutzerzugriffsadministrator“ zugewiesen.

   ![Blatt „Zugriffssteuerung (IAM)“ mit der Rollenzuweisung „Benutzerzugriffsadministrator“ für MS-PIM](./media/pim-how-to-add-role-to-user/ms-pim-user-access-administrator.png)


## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Einstellungen für Azure AD-Rollen in PIM](pim-how-to-change-default-settings.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
