---
title: Rollen, die nicht in Privileged Identity Management verwaltet werden können – Azure Active Directory | Microsoft-Dokumentation
description: In diesem Artikel werden die Rollen beschrieben, die Sie nicht in Azure AD Privileged Identity Management (PIM) verwalten können.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fac7074cf85a585c93ece60be9eea8ffb9a6345
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895203"
---
# <a name="roles-you-cant-manage-in-privileged-identity-management"></a>Rollen, die nicht in Privileged Identity Management verwaltet werden können

Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie alle [Azure AD-Rollen](../users-groups-roles/directory-assign-admin-roles.md) und alle [Azure-Ressourcenrollen](../../role-based-access-control/built-in-roles.md) verwalten. Zu diesen Rollen gehören auch benutzerdefinierten Rollen, die an Ihre Verwaltungsgruppen, Abonnements, Ressourcengruppen und Ressourcen angefügt sind. Es gibt jedoch einige Rollen, die Sie nicht verwalten können. In diesem Artikel werden die Rollen beschrieben, die Sie nicht in Privileged Identity Management verwalten können.

## <a name="classic-subscription-administrator-roles"></a>Administrator für klassisches Abonnement

Die folgenden Administratorrollen klassischer Abonnements können Sie nicht in Privileged Identity Management verwalten:

- Kontoadministrator
- Dienstadministrator
- Co-Administrator

Weitere Informationen zu Administratorrollen klassischer Abonnements finden Sie unter [Administratorrollen für klassische Abonnements, Azure RBAC-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="what-about-office-365-admin-roles"></a>Informationen zu Office 365-Administratorrollen

Rollen in Exchange Online oder SharePoint Online (mit Ausnahme von Exchange-Administrator und SharePoint-Administrator) werden in Azure AD nicht dargestellt und können daher in Privileged Identity Management nicht verwaltet werden. Weitere Informationen zu diesen Office 365-Diensten finden Sie unter [Office 365-Administratorrollen](https://docs.microsoft.com/office365/admin/add-users/about-admin-roles).

> [!NOTE]
> Der SharePoint-Administrator hat Administratorzugriff auf SharePoint Online über das SharePoint Online Admin Center und darf nahezu jede Aufgabe in SharePoint Online ausführen. Bei berechtigten Benutzern treten bei Verwendung dieser Rolle innerhalb von SharePoint nach der Aktivierung in Privileged Identity Management u.U. Verzögerungen auf.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Azure AD-Rollen in PIM](pim-how-to-add-role-to-user.md)
- [Zuweisen von Azure-Ressourcenrollen in PIM](pim-resource-roles-assign-roles.md)
