---
title: Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie Zuweisungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung anzeigen, hinzufügen und entfernen.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f06da43e758c24da65664b34dd83f876a00e3a0
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174913"
---
# <a name="view-add-and-remove-assignments-for-an-access-package-in-azure-ad-entitlement-management"></a>Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung

In der Azure AD-Berechtigungsverwaltung können Sie anzeigen, welche Benutzer Zugriffspaketen zugewiesen sind. Außerdem können Sie die jeweiligen Richtlinien und den Status anzeigen. Wenn ein Zugriffspaket eine geeignete Richtlinie enthält, können Sie dem Zugriffspaket auch direkt Benutzer zuweisen. In diesem Artikel wird beschrieben, wie Zuweisungen für Zugriffspakete angezeigt, hinzugefügt und entfernt werden.

## <a name="view-who-has-an-assignment"></a>Anzeigen von Benutzern mit Zuweisung

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie auf **Zuweisungen**, um eine Liste der aktiven Zuweisungen anzuzeigen.

    ![Liste der Zuweisungen für ein Zugriffspaket](./media/entitlement-management-access-package-assignments/assignments-list.png)

1. Klicken Sie auf eine bestimmte Zuweisung, wenn Sie zusätzliche Details anzeigen möchten.

1. Wenn Sie die Liste der Zuweisungen mit nicht ordnungsgemäß bereitgestellten Ressourcenrollen anzeigen möchten, klicken Sie auf den Filterstatus, und wählen Sie **Übermitteln** aus.

    Zusätzliche Details zu Übermittlungsfehlern werden auf der Seite **Anforderungen** unter der Anforderung des jeweiligen Benutzers angezeigt.

1. Klicken Sie zum Anzeigen von abgelaufenen Zuweisungen auf den Filterstatus, und wählen Sie **Abgelaufen** aus.

1. Wenn Sie eine CSV-Datei aus der gefilterten Liste herunterladen möchten, klicken Sie auf **Herunterladen**.

## <a name="directly-assign-a-user"></a>Direktes Zuweisen eines Benutzers

In einigen Fällen möchten Sie bestimmte Benutzer möglicherweise einem Zugriffspaket direkt zuweisen, damit diese das Zugriffspaket nicht extra anfordern müssen. Damit dies möglich ist, muss das Zugriffspaket eine Richtlinie enthalten, die direkte Zuweisungen eines Administrator erlaubt.

**Erforderliche Rolle:** Globaler Administrator, Benutzeradministrator, Katalogbesitzer oder Zugriffspaket-Manager

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü auf der linken Seite auf **Zugriffspakete**, und öffnen Sie das Zugriffspaket.

1. Klicken Sie im Menü auf der linken Seite auf **Zuweisungen**.

1. Klicken Sie auf **Neue Zuweisung**, um die Seite „Hinzufügen von Benutzern zu einem Zugriffspaket“ zu öffnen.

    ![Zuweisungen: Benutzer einem Zugriffspaket hinzufügen](./media/entitlement-management-access-package-assignments/assignments-add-user.png)

1. Klicken Sie auf **Benutzer hinzufügen**, um die Benutzer auszuwählen, die Sie diesem Zugriffspaket zuweisen möchten.

1. Wählen Sie in der Liste **Richtlinie auswählen** eine Richtlinie aus, von der künftige Anforderungen des Benutzers und der Lebenszyklus geregelt und verfolgt werden. Wenn für die ausgewählten Benutzer andere Richtlinieneinstellungen gelten sollen, können Sie auf **Neue Richtlinie erstellen** klicken, um eine neue Richtlinie hinzuzufügen.

1. Legen Sie Datum und Uhrzeit fest, wann die Zuweisung der ausgewählten Benutzer beginnen und enden soll. Wenn Sie kein Enddatum angeben, werden die Lebenszykluseinstellungen der Richtlinie verwendet.

1. Geben Sie zur Datenbankpflege optional eine Begründung für Ihre direkte Zuweisung an.

1. Klicken Sie auf **Hinzufügen**, um die ausgewählten Benutzer dem Zugriffspaket direkt zuzuweisen.

    Klicken Sie nach kurzer Zeit auf **Aktualisieren**, um die Benutzer in der Liste „Zuweisungen“ anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket](entitlement-management-access-package-request-policy.md)
- [Anzeigen von Berichten und Protokollen](entitlement-management-reports.md)
