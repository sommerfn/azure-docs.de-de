---
title: Aktualisieren oder Entfernen einer benutzerdefinierten Azure AD-Rollenzuweisung in Privileged Identity Management (PIM) | Microsoft-Dokumentation
description: 'Vorgehensweise: Aktualisieren oder Entfernen einer benutzerdefinierten Azure AD-Rollenzuweisung in Privileged Identity Management (PIM)'
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
ms.openlocfilehash: ccbc9fbd763b9393a64d6cfc29f6b5a9d021da88
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756335"
---
# <a name="update-or-remove-an-assigned-azure-ad-custom-role-in-privileged-identity-management"></a>Aktualisieren oder Entfernen einer zugewiesenen benutzerdefinierten Azure AD-Rolle in Privileged Identity Management

In diesem Artikel erfahren Sie, wie Sie Privileged Identity Management verwenden, um Just-In-Time- und zeitgebundene Zuweisungen zu benutzerdefinierten Rollen zu aktualisieren oder zu entfernen, die für die Anwendungsverwaltung auf der Administratoroberfläche von Azure Active Directory (Azure AD) erstellt wurden. 

- Weitere Informationen zum Erstellen benutzerdefinierter Rollen, um die Anwendungsverwaltung in Azure AD zu delegieren, finden Sie unter [Benutzerdefinierte Administratorrollen in Azure Active Directory (Vorschau)](../users-groups-roles/roles-custom-overview.md). 
- Wenn Sie Privileged Identity Management noch nie verwendet haben, informieren Sie sich zunächst auf der Seite [Einstieg in die Verwendung von PIM](pim-getting-started.md).

> [!NOTE]
> Benutzerdefinierte Azure AD-Rollen sind in der Vorschauversion nicht in die integrierten Verzeichnisrollen integriert. Sobald die Funktion allgemein verfügbar ist, erfolgt die Rollenverwaltung auf der Benutzeroberfläche für integrierte Rollen.

## <a name="update-or-remove-an-assignment"></a>Aktualisieren oder Entfernen einer Zuweisung

Führen Sie die folgenden Schritte aus, um eine vorhandene benutzerdefinierte Rollenzuweisung zu aktualisieren oder zu entfernen.

1. Melden Sie sich im Azure-Portal mit dem Benutzerkonto bei [Privileged Identity Management](https://portal.azure.com/?Microsoft_AAD_IAM_enableCustomRoleManagement=true&Microsoft_AAD_IAM_enableCustomRoleAssignment=true&feature.rbacv2roles=true&feature.rbacv2=true&Microsoft_AAD_RegisteredApps=demo#blade/Microsoft_Azure_PIMCommon/CommonMenuBlade/quickStart) an, das der Rolle „Administrator für privilegierte Rollen“ zugewiesen ist.
1. Wählen Sie **Benutzerdefinierte Azure AD-Rollen (Vorschau)** aus.

    ![Auswählen von „Benutzerdefinierte Azure AD-Rollen (Vorschau)“ zum Anzeigen berechtigter Rollenzuweisungen](./media/azure-ad-custom-roles-assign/view-custom.png)

1. Wählen Sie **Rollen** aus, um eine Liste der **Zuweisungen** von benutzerdefinierten Rollen für Azure AD-Anwendungen anzuzeigen.

    ![Auswählen von Rollen zum Anzeigen der Liste berechtigter Rollenzuweisungen](./media/azure-ad-custom-roles-update-remove/assignments-list.png)

1. Wählen Sie die Rolle aus, die Sie aktualisieren oder entfernen möchten.
1. Suchen Sie die Rollenzuweisung auf den Registerkarten **Berechtigte Rollen** oder **Aktive Rollen**.
1. Wählen Sie **Aktualisieren** oder **Entfernen** aus, um die Rollenzuweisung zu aktualisieren oder zu entfernen.

    ![Auswählen von „Aktualisieren“ oder „Entfernen“ in der berechtigten Rollenzuweisung](./media/azure-ad-custom-roles-update-remove/remove-update.png)

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-assign.md)
- [Zuweisen einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-assign.md)
- [Konfigurieren der Zuweisung einer benutzerdefinierten Azure AD-Rolle](azure-ad-custom-roles-configure.md)
