---
title: Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie die Zugriffssteuerung von IT-Administratoren an Katalogersteller und Projektmanager delegieren, damit sie den Zugriff selbst verwalten können.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/26/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: f71007b886d3cc25a7cf9dc23d784144ed4e1fbd
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174384"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management"></a>Delegieren der Zugriffssteuerung an Katalogersteller in der Azure AD-Berechtigungsverwaltung

Wenn Sie an Benutzer delegieren möchten, die keine Administratoren sind, damit sie ihre eigenen Kataloge erstellen können, fügen Sie diese Benutzer der in der Azure AD-Berechtigungsverwaltung definierten Rolle Katalogersteller hinzu. Sie können einzelne Benutzer oder eine ganze Gruppe hinzufügen, deren Mitglieder dann Kataloge erstellen können.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>Delegieren an einen Katalogersteller als IT-Administrator

Führen Sie diese Schritte aus, um einen Benutzer der Katalogerstellerrolle zuzuordnen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im linken Menü im Abschnitt **Berechtigungsverwaltung** auf **Einstellungen**.

1. Klicken Sie auf **Edit**.

    ![Einstellungen zum Hinzufügen von Katalogerstellern](./media/entitlement-management-delegate-catalog/settings-delegate.png)

1. Klicken Sie im Abschnitt **Berechtigungsverwaltung delegieren** auf **Katalogersteller hinzufügen**, um die Benutzer oder Gruppen auszuwählen, an die Sie die Berechtigungsverwaltungsrolle delegieren möchten.

1. Klicken Sie auf **Auswählen**.

1. Klicken Sie auf **Speichern**.

## <a name="allow-delegated-roles-to-access-the-azure-portal"></a>Gewähren des Zugriffs delegierter Rollen auf das Azure-Portal

Wenn Sie delegierten Rollen wie (z. B. Katalogerstellern und Zugriffspaket-Managern), den Zugriff auf das Azure-Portal zum Verwalten von Zugriffspaketen gestatten möchten, sollten Sie die Einstellung im Verwaltungsportal prüfen.

**Erforderliche Rolle:** Globaler Administrator oder Benutzeradministrator

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Benutzer**.

1. Klicken Sie im linken Menü auf **Benutzereinstellungen**.

1. Vergewissern Sie sich, dass **Zugriff auf Azure AD-Verwaltungsportal einschränken** auf **Nein** festgelegt ist.

    ![Azure AD-Benutzereinstellungen – Verwaltungsportal](./media/entitlement-management-delegate-catalog/user-settings.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen und Verwalten eines Katalogs von Ressourcen](entitlement-management-catalog-create.md)
- [Delegieren der Zugriffssteuerung an Zugriffspaket-Manager](entitlement-management-delegate-managers.md)

