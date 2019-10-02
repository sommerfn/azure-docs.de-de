---
title: Hinzufügen oder Ändern von Azure-Abonnementadministratoren
description: Beschreibt das Hinzufügen oder Ändern eines Azure-Abonnementadministrators mithilfe der rollenbasierten Zugriffssteuerung (Role-based Access Control, RBAC).
author: genlin
manager: dcscontentpm
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: banders
ms.openlocfilehash: 2054fbb7d0a9f450ad487fc0f03d0af920c6cc4b
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71260922"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Hinzufügen oder Ändern von Azure-Abonnementadministratoren


Für die Verwaltung von Azure-Ressourcen müssen Sie über die entsprechende Administratorrolle verfügen. Azure bietet ein als [rollenbasierte Zugriffssteuerung](../role-based-access-control/overview.md) (Role-Based Access Control, RBAC) bezeichnetes Autorisierungssystem mit verschiedenen integrierten Rollen, unter denen Sie wählen können. Sie können diesen Rollen verschiedene Gültigkeitsbereiche zuweisen, wie etwa Verwaltungsgruppe, Abonnement oder Ressourcengruppe. Standardmäßig kann die Person, die ein neues Azure-Abonnement erstellt, anderen Benutzern Administratorzugriff auf ein Abonnement gewähren.

In diesem Artikel wird beschrieben, wie die Administratorrolle für einen Benutzer mithilfe von RBAC auf Abonnementebene hinzugefügt oder geändert werden kann.

Microsoft empfiehlt das Verwalten des Zugriffs auf Ressourcen mithilfe von RBAC. Wenn Sie aber weiterhin das klassische Bereitstellungsmodell verwenden und die klassischen Ressourcen mit dem [PowerShell-Modul für die Azure-Dienstverwaltung](https://docs.microsoft.com/powershell/module/servicemanagement/azure) verwalten, müssen Sie einen klassischen Administrator verwenden.

> [!TIP]
> Wenn Sie klassische Ressourcen nur über das Azure-Portal verwalten, müssen Sie den klassischen Administrator nicht verwenden.

Weitere Informationen finden Sie unter [Azure Resource Manager und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md) und [Azure classic subscription administrators](../role-based-access-control/classic-administrators.md) (Klassische Azure-Abonnementadministratoren).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-subscription-administrator"></a>Zuweisen eines Abonnementadministrators

Um einen Benutzer als Administrator eines Azure-Abonnements festzulegen, weist ihm ein vorhandener Administrator die Rolle [Besitzer](../role-based-access-control/built-in-roles.md#owner) (eine RBAC-Rolle) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer vollständigen Zugriff auf alle Ressourcen im Abonnement, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

1. Öffnen Sie die [Seite „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

### <a name="to-assign-a-user-as-an-administrator"></a>Zuweisen eines Benutzers als Administrator

1. Melden Sie sich beim Azure-Portal als Besitzer des Abonnements an, und öffnen Sie [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Klicken Sie auf das Abonnement, für das Sie Zugriff erteilen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

    ![Screenshot von Rollenzuweisungen](./media/billing-add-change-azure-subscription-administrator/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**, um den Bereich **Rollenzuweisung hinzufügen** zu öffnen.

    Wenn Sie keine Berechtigungen zum Zuweisen von Rollen besitzen, ist die Option deaktiviert.

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

    ![Screenshot mit ausgewählter Besitzerrolle](./media/billing-add-change-azure-subscription-administrator/add-role.png)

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

    Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die rollenbasierte Zugriffssteuerung (RBAC)?](../role-based-access-control/overview.md)
* [Grundlegendes zu den verschiedenen Rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Gewusst wie: Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Berechtigungen der Administratorrolle in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
