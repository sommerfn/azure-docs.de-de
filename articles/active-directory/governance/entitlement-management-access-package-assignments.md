---
title: Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion) – Azure Active Directory
description: Erfahren Sie, wie Sie Zuweisungen für ein Zugriffspaket in der Azure Active Directory-Berechtigungsverwaltung (Vorschauversion) anzeigen, hinzufügen und entfernen.
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
ms.openlocfilehash: 694c8866a69d8488511be1670ba3d3013a4a3423
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72392025"
---
# <a name="view-add-and-remove-assignments-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Anzeigen, Hinzufügen und Entfernen von Zuweisungen für ein Zugriffspaket in der Azure AD-Berechtigungsverwaltung (Vorschauversion)

> [!IMPORTANT]
> Die Berechtigungsverwaltung von Azure Active Directory (Azure AD) befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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

1. Klicken Sie auf **Benutzer hinzufügen**, um die Benutzer auszuwählen, die Sie dem Zugriffspaket zuweisen möchten.

1. Wählen Sie in der Liste **Richtlinie auswählen** eine Richtlinie mit der Einstellung [Keine (nur direkte Zuweisungen eines Administrators)](entitlement-management-access-package-request-policy.md#none-administrator-direct-assignments-only) aus.

    Enthält das Zugriffspaket keine Richtlinien dieses Typs, können Sie diese hinzufügen. Klicken Sie dazu auf **Neue Richtlinie erstellen**.

1. Legen Sie Datum und Uhrzeit fest, wann die Zuweisung der ausgewählten Benutzer beginnen und enden soll. Wenn Sie kein Enddatum angeben, werden die Lebenszykluseinstellungen der Richtlinie verwendet.

1. Geben Sie zur Datenbankpflege optional eine Begründung für Ihre direkte Zuweisung an.

1. Klicken Sie auf **Hinzufügen**, um die ausgewählten Benutzer dem Zugriffspaket direkt zuzuweisen.

    Klicken Sie nach kurzer Zeit auf **Aktualisieren**, um die Benutzer in der Liste „Zuweisungen“ anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

- [Ändern der Anforderungs- und Genehmigungseinstellungen für ein Zugriffspaket](entitlement-management-access-package-request-policy.md)
- [Anzeigen von Berichten und Protokollen](entitlement-management-reports.md)
