---
title: Zuweisen einer benutzerdefinierten Azure AD-Rolle in Privileged Identity Management (PIM) | Microsoft-Dokumentation
description: 'Vorgehensweise: Zuweisen einer benutzerdefinierten Azure AD-Rolle in Privileged Identity Management (PIM)'
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/06/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c4963e6563db166566cff41d381295a8a9e576b
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756357"
---
# <a name="assign-an-azure-ad-custom-role-in-privileged-identity-management"></a>Zuweisen einer benutzerdefinierten Azure AD-Rolle in Privileged Identity Management

In diesem Artikel erfahren Sie, wie Sie Privileged Identity Management verwenden, um Just-In-Time- und zeitgebundene Zuweisungen zu benutzerdefinierten Rollen zu erstellen, die für die Verwaltung von Anwendungen auf der Administratoroberfläche von Azure Active Directory (Azure AD) erstellt wurden.

- Weitere Informationen zum Erstellen benutzerdefinierter Rollen, um die Anwendungsverwaltung in Azure AD zu delegieren, finden Sie unter [Benutzerdefinierte Administratorrollen in Azure Active Directory (Vorschau)](../users-groups-roles/roles-custom-overview.md).
- Wenn Sie Privileged Identity Management noch nie verwendet haben, informieren Sie sich zunächst auf der Seite [Einstieg in die Verwendung von PIM](pim-getting-started.md).
- Informationen dazu, wie Sie anderen Administratoren Zugriff zum Verwalten von Privileged Identity Management gewähren, finden Sie unter [Gewähren von Zugriff für andere Administratoren zum Verwalten von PIM](pim-how-to-give-access-to-pim.md).

> [!NOTE]
> Benutzerdefinierte Azure AD-Rollen sind in der Vorschauversion nicht in die integrierten Verzeichnisrollen integriert. Sobald die Funktion allgemein verfügbar ist, erfolgt die Rollenverwaltung auf der Benutzeroberfläche für integrierte Rollen.

## <a name="assign-a-role"></a>Zuweisen einer Rolle

In Privileged Identity Management können Sie benutzerdefinierte Rollen verwalten, die Sie in der Anwendungsverwaltung von Azure Active Directory (Azure AD) erstellen.  Mit den folgenden Schritten erstellen Sie eine berechtigte Zuweisung zu einer benutzerdefinierten Verzeichnisrolle.

1. Melden Sie sich im Azure-Portal mit dem Benutzerkonto bei [Privileged Identity Management](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) an, das der Rolle „Administrator für privilegierte Rollen“ zugewiesen ist.
1. Wählen Sie **Benutzerdefinierte Azure AD-Rollen (Vorschau)** aus.

    ![Auswählen von „Benutzerdefinierte Azure AD-Rollen (Vorschau)“ zum Anzeigen berechtigter Rollenzuweisungen](./media/azure-ad-custom-roles-assign/view-custom.png)

1. Wählen Sie **Rollen** aus, um eine Liste der benutzerdefinierten Rollen für Azure AD-Anwendungen anzuzeigen.

    ![Auswählen von Rollen zum Anzeigen der Liste berechtigter Rollenzuweisungen](./media/azure-ad-custom-roles-assign/view-roles.png)

1. Wählen Sie **Mitglied hinzufügen** aus, um die Seite „Zuweisung“ zu öffnen.
1. Wenn Sie den Bereich der Rollenzuweisung auf eine einzelne Anwendung beschränken möchten, wählen Sie **Bereich** aus, um einen Anwendungsbereich anzugeben.

    ![Beschränken des Bereichs berechtigter Rollenzuweisungen in Azure AD](./media/azure-ad-custom-roles-assign/set-scope.png)

1. Wählen Sie **Rolle auswählen** aus, um die Liste **Rolle auswählen** zu öffnen.

    ![Auswählen der berechtigten Rolle, die einem Benutzer zugewiesen werden soll](./media/azure-ad-custom-roles-assign/select-role.png)

1. Wählen Sie die Rolle aus, die Sie zuweisen möchten, und klicken Sie dann auf **Auswählen**. Die Liste **Mitglied auswählen** wird geöffnet.

    ![Auswählen des Benutzers, dem Sie die Rolle zuweisen](./media/azure-ad-custom-roles-assign/select-member.png)

1. Wählen Sie einen Benutzer aus, dem Sie die Rolle zuweisen möchten, und klicken Sie dann auf **Auswählen**. Die Liste **Mitgliedschaftseinstellungen** wird geöffnet.

    ![Festlegen des Rollenzuweisungstyps auf „berechtigt“ oder „aktiv“](./media/azure-ad-custom-roles-assign/membership-settings.png)

1. Wählen Sie auf der Seite **Mitgliedschaftseinstellungen** die Option **Berechtigt** oder **Aktiv** aus:

    - Bei **berechtigten** Zuweisungen muss der Benutzer, der der Rolle zugewiesen ist, eine Aktion durchführen, bevor er die Rolle verwenden kann. Beispiele für Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung, die Angabe einer geschäftlichen Begründung oder das Anfordern einer Genehmigung von designierten genehmigenden Personen.
    - Bei **aktiven** Zuweisungen muss der zugewiesene Benutzer keine Aktion durchführen, um die Rolle verwenden zu können. Bei aktiven Benutzern sind die Berechtigungen der Rolle ständig zugewiesen.

1. Wenn das Kontrollkästchen **Permanent** vorhanden und verfügbar ist (dies hängt von den Rolleneinstellungen ab), können Sie angeben, ob die Zuweisung dauerhaft ist. Aktivieren Sie das Kontrollkästchen, um die Zuweisung als „dauerhaft berechtigt“ oder „dauerhaft zugewiesen“ festzulegen. Deaktivieren Sie das Kontrollkästchen, um die Dauer einer Zuweisung anzugeben.
1. Klicken Sie auf **Speichern** und dann auf **Hinzufügen**, um die neue Rollenzuweisung zu erstellen. Eine Benachrichtigung zum Status des Zuweisungsprozesses wird angezeigt.

Um die Rollenzuweisung zu überprüfen, wählen Sie in einer offenen Rolle **Zuweisungen** > **Zuweisen** aus, und überprüfen Sie, ob die Rollenzuweisung ordnungsgemäß als „berechtigt“ oder „aktiv“ identifiziert wird.

 ![Überprüfen, ob die Rollenzuweisung als „berechtigt“ oder als „aktiv“ angezeigt wird](./media/azure-ad-custom-roles-assign/verify-assignments.png)

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-assign.md)
- [Entfernen oder Aktualisieren der Zuweisung einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-update-remove.md)
- [Konfigurieren der Zuweisung einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-configure.md)
- [Rollendefinitionen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md)
