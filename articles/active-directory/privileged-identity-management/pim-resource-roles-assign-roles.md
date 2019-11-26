---
title: Zuweisen von Azure-Ressourcenrollen in Privileged Identity Management – Azure Active Directory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) zuweisen.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34051a31c6ccf69356f330d7c5ecb009f760857a
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895943"
---
# <a name="assign-azure-resource-roles-in-privileged-identity-management"></a>Zuweisen von Azure-Ressourcenrollen in Privileged Identity Management

Azure Active Directory Privileged Identity Management (Azure AD PIM) kann sowohl die integrierten Azure-Ressourcenrollen als auch benutzerdefinierte Rollen verwalten. Beispiele:

- Owner (Besitzer)
- Benutzerzugriffsadministrator
- Mitwirkender
- Sicherheitsadministrator
- Sicherheits-Manager

> [!NOTE]
> Benutzer oder Mitglieder von Gruppen, die der Rolle „Besitzer“ oder „Benutzerzugriffsadministrator“ für das Abonnement zugewiesen sind, sowie globale Azure AD-Administratoren, die die Abonnementverwaltung in Azure AD ermöglichen, haben standardmäßig Ressourcenadministratorberechtigungen. Diese Administratoren können Rollen zuweisen, Rolleneinstellungen konfigurieren und den Zugriff mithilfe von Privileged Identity Management für Azure-Ressourcen überprüfen. Ohne Ressourcenadministratorberechtigung kann ein Benutzer in Privileged Identity Management keine Ressourcen verwalten. Eine Liste mit den integrierten Rollen für Azure-Ressourcen finden Sie [hier](../../role-based-access-control/built-in-roles.md).

## <a name="assign-a-role"></a>Zuweisen einer Rolle

Führen Sie folgende Schritte aus, um einen Benutzer für eine Azure AD-Ressourcenrolle als „berechtigt“ festzulegen.

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com/) als Benutzer an, der ein Mitglied der Rolle [Administrator für privilegierte Rollen](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) ist.

    Informationen dazu, wie Sie anderen Administratoren Zugriff zum Verwalten von Privileged Identity Management gewähren, finden Sie unter [Gewähren von Zugriff für andere Administratoren zum Verwalten von PIM](pim-how-to-give-access-to-pim.md).

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure-Ressourcen** aus.

1. Verwenden Sie den **Ressourcenfilter** zum Filtern der Liste verwalteter Ressourcen.

    ![Liste der zu verwaltenden Azure-Ressourcen](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Wählen Sie die Ressource aus, die Sie verwalten möchten, z.B. ein Abonnement oder eine Verwaltungsgruppe.

1. Wählen Sie unter „Verwalten“ den Eintrag **Rollen** aus, um die Liste der Rollen für Azure-Ressourcen anzuzeigen.

    ![Azure-Ressourcenrollen](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Wählen Sie **Mitglied hinzufügen** aus, um den Bereich „Neue Zuweisung“ zu öffnen.

1. Wählen Sie **Rolle auswählen** aus, um den Bereich „Rolle auswählen“ zu öffnen.

    ![Bereich „Neue Zuweisung“](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Wählen Sie die Rolle aus, die Sie zuweisen möchten, und klicken Sie dann auf **Auswählen**.

    Der Bereich „Mitglied oder Gruppe auswählen“ wird geöffnet.

1. Wählen Sie zuerst ein Mitglied oder eine Gruppe, dem bzw. der die Rolle zugewiesen werden soll, und dann **Auswählen** aus.

    ![Bereich „Mitglied oder Gruppe auswählen“](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Der Bereich „Mitgliedschaftseinstellungen“ wird geöffnet.

1. Wählen Sie in der Liste **Zuweisungstyp** entweder **Berechtigt** oder **Aktiv** aus.

    ![Bereich „Mitgliedschaftseinstellungen“](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Privileged Identity Management für Azure-Ressourcen verfügt über zwei unterschiedliche Zuweisungstypen:

    - Für **berechtigte** Zuweisungen muss das Mitglied der Rolle eine Aktion durchführen, um die Rolle verwenden zu können. Beispiele für Aktionen sind eine erfolgreiche Multi-Factor Authentication-Überprüfung (MFA), die Angabe einer geschäftlichen Begründung oder das Anfordern einer Genehmigung von den angegebenen genehmigenden Personen.

    - Für **aktive** Zuweisungen ist es nicht erforderlich, dass das Mitglied eine Aktion durchführt, um die Rolle nutzen zu können. Für Mitglieder, die als „Aktiv“ zugewiesen sind, sind die Berechtigungen immer der Rolle zugewiesen.

1. Wenn die Zuweisung dauerhaft sein soll (dauerhaft berechtigt oder dauerhaft zugewiesen), aktivieren Sie das Kontrollkästchen **Dauerhaft**.

    Je nach Rolleneinstellungen wird das Kontrollkästchen möglicherweise nicht angezeigt oder ist nicht änderbar.

1. Wenn Sie einen bestimmten Zuweisungszeitraum angeben möchten, deaktivieren Sie dieses Kontrollkästchen, und ändern Sie nach Bedarf die Felder für den Start- und/oder den Endzeitpunkt (Datum und Uhrzeit).

    ![Mitgliedschaftseinstellungen: Datum und Uhrzeit](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Wählen Sie abschließend **Fertig** aus.

    ![Neue Zuweisung: Hinzufügen](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Wählen Sie **Hinzufügen** aus, um die neue Rollenzuweisung zu erstellen. Eine Benachrichtigung zum Status wird angezeigt.

    ![Neue Zuweisung: Benachrichtigung](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Aktualisieren oder Entfernen einer vorhandenen Rollenzuweisung

Befolgen Sie diese Anweisungen zum Aktualisieren oder Entfernen einer vorhandenen Rollenzuweisung.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie **Azure-Ressourcen** aus.

1. Wählen Sie die Ressource aus, die Sie verwalten möchten, z.B. ein Abonnement oder eine Verwaltungsgruppe.

1. Wählen Sie unter „Verwalten“ den Eintrag **Rollen** aus, um die Liste der Rollen für Azure-Ressourcen anzuzeigen.

    ![Azure-Ressourcenrollen: Rolle auswählen](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Wählen Sie die Rolle aus, die Sie aktualisieren oder entfernen möchten.

1. Suchen Sie die Rollenzuweisung auf den Registerkarten **Berechtigte Rollen** oder **Aktive Rollen**.

    ![Aktualisieren oder Entfernen der Rollenzuweisung](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Wählen Sie **Aktualisieren** oder **Entfernen** aus, um die Rollenzuweisung zu aktualisieren oder zu entfernen.

    Informationen zum Erweitern einer Rollenzuweisung finden Sie unter [Verlängern oder Erneuern von Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Nächste Schritte

- [Verlängern oder Erneuern von Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-renew-extend.md)
- [Konfigurieren von Einstellungen für Azure-Ressourcenrollen in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
