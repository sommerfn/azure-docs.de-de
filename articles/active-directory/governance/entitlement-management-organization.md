---
title: Hinzufügen einer verbundenen Organisation in der Azure AD-Berechtigungsverwaltung – Azure Active Directory
description: Erfahren Sie, wie Sie Personen außerhalb Ihres Unternehmens das Anfordern von Zugriffspaketen gestatten, um eine gemeinsame Arbeit an Projekten zu ermöglichen.
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
ms.date: 10/24/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a8e5dc43170af8f33ad266edfa02ac5b41ca67a
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73174141"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Hinzufügen einer verbundenen Organisation in der Azure AD-Berechtigungsverwaltung

Dank der Azure AD-Berechtigungsverwaltung ist es möglich, mit Personen außerhalb Ihres Unternehmens zusammenzuarbeiten. Wenn Sie häufig mit Benutzern in einem externen Azure AD-Verzeichnis oder einer externen Domäne zusammenarbeiten, können Sie dieses Verzeichnis oder diese Domäne als verbundene Organisation hinzufügen. Dieser Artikel beschreibt, wie Sie eine verbundene Organisation hinzufügen, sodass Sie Benutzern außerhalb Ihrer Organisation das Anfordern von Ressourcen in Ihrem Verzeichnis gestatten können.

## <a name="what-is-a-connected-organization"></a>Was ist eine verbundene Organisation?

Eine verbundene Organisation ist ein externes Azure AD-Verzeichnis bzw. eine externe Domäne, mit der eine Beziehung besteht.

Angenommen, Sie arbeiten bei der Woodgrove Bank und möchten mit zwei externen Organisationen zusammenarbeiten: dem Graphic Design Institute und Contoso. Ihr Kontakt am Graphic Design Institute hat Ihnen mitgeteilt, dass im Institut Azure AD verwendet wird und die Namen der Graphic Design Institute-Benutzer auf `graphicdesigninstitute.com` enden. Von Ihrem Ansprechpartner bei Contoso wurden Sie darüber informiert, dass Azure AD noch nicht eingesetzt wird, die Contoso-Benutzer aber über einen Benutzerprinzipalnamen verfügen, der auf `contoso.com` endet.

Sie können zwei verbundene Organisationen konfigurieren – eine für das Graphic Design Institute mit der Domäne `graphicdesigninstitute.com` und eine für Contoso mit der Domäne `contoso.com`. Wenn Sie diese beiden verbundenen Unternehmen anschließend zu einer Richtlinie hinzufügen, können Benutzer beider Organisationen, deren Benutzername mit der Richtlinie konform ist, Zugriffspakete anfordern. Am Graphic Design Institute wird darüber hinaus Azure AD eingesetzt. Wenn das Institut also Unterdomänen wie z. B. `graphicdesigninstitute.example` verwendet, können Benutzer mit diesem Benutzerprinzipalnamen über dieselbe Richtlinie ebenfalls Zugriffspakete anfordern.

![Beispiel für eine verbundene Organisation](./media/entitlement-management-organization/connected-organization-example.png)

Wie sich Benutzer aus dem Azure AD-Verzeichnis oder der Domäne authentifizieren, hängt vom Authentifizierungstyp ab. Die Authentifizierungstypen für verbundene Organisationen lauten wie folgt:

- Azure AD
- [Direkter Verbund](../b2b/direct-federation.md)
- [Einmalkennung](../b2b/one-time-passcode.md) (Domäne)

## <a name="add-a-connected-organization"></a>Hinzufügen einer verbundenen Organisation

Führen Sie die folgenden Schritte aus, um ein externes Azure AD-Verzeichnis oder eine externe Domäne als verbundene Organisation hinzuzufügen.

**Erforderliche Rolle:** „Globaler Administrator“, „Benutzeradministrator“ oder „Gasteinladender“

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü links auf **Verbundene Organisationen** und anschließend auf **Verbundene Organisation hinzufügen**.

    ![Identity Governance – Verbundene Organisationen – Verbundene Organisation hinzufügen](./media/entitlement-management-organization/connected-organization.png)

1. Geben Sie auf der Registerkarte **Grundlagen** einen Anzeigenamen und eine Beschreibung für die Organisation ein.

    ![Verbundene Organisation hinzufügen – Registerkarte „Grundlagen“](./media/entitlement-management-organization/organization-basics.png)

1. Klicken Sie auf der Registerkarte **Verzeichnis und Domäne** auf **Verzeichnis und Domäne hinzufügen**, um den Bereich „Verzeichnisse und Domänen auswählen“ zu öffnen.

1. Geben Sie einen Domänennamen ein, um nach dem Azure AD-Verzeichnis oder der Domäne zu suchen. Sie müssen den gesamten Domänennamen eingeben.

1. Überprüfen Sie anhand des angegebenen Namens und des Authentifizierungstyps, dass es sich um die richtige Organisation handelt. Wie Benutzer sich anmelden, hängt vom Authentifizierungstyp ab.

    ![Verbundene Organisation hinzufügen – Verzeichnisse und Domänen auswählen](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Klicken Sie auf **Hinzufügen**, um das Azure AD-Verzeichnis oder die Domäne hinzuzufügen. Aktuell kann pro verbundener Organisation nur ein Azure AD-Verzeichnis oder eine Domäne hinzugefügt werden.

    > [!NOTE]
    > Alle Benutzer aus dem Azure AD-Verzeichnis oder der Domäne können Zugriffspakete anfordern. Dies schließt in Azure AD auch Benutzer aller Unterdomänen ein, die dem Verzeichnis zugeordnet sind, sofern diese Domänen nicht über die Zulassungs- oder Verweigerungsliste von Azure B2B blockiert werden. Weitere Informationen finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](../b2b/allow-deny-list.md).

1. Nachdem Sie das Azure AD-Verzeichnis oder die Domäne hinzugefügt haben, klicken Sie auf **Auswählen**.

    Die Organisation wird in der Liste angezeigt.

    ![Verbundene Organisation hinzufügen – Registerkarte „Verzeichnisse“](./media/entitlement-management-organization/organization-directory-domain.png)

1. Fügen Sie auf der Registerkarte **Sponsoren** optional Sponsoren für diese verbundene Organisation hinzu.

    Sponsoren sind interne oder externe Benutzer, die sich bereits in Ihrem Verzeichnis befinden und als Ansprechpartner für die Beziehung mit dieser verbundenen Organisation fungieren. Interne Sponsoren sind Mitgliederbenutzer in Ihrem Verzeichnis. Externe Sponsoren sind Gastbenutzer aus der verbundenen Organisation, die zuvor eingeladen wurden und sich bereits in Ihrem Verzeichnis befinden. Sponsoren können als genehmigende Personen fungieren, wenn Benutzer in dieser verbundenen Organisation Zugriff auf ein Zugriffspaket anfordern. Informationen zum Einladen eines Gastbenutzers zu Ihrem Verzeichnis finden Sie unter [Hinzufügen von Azure Active Directory B2B-Zusammenarbeitsbenutzern über das Azure-Portal](../b2b/add-users-administrator.md).

    Wenn Sie auf **Hinzufügen/Entfernen** klicken, wird ein Bereich geöffnet, in dem Sie interne oder externe Sponsoren auswählen können. Der Bereich zeigt eine ungefilterte Liste der Benutzer und Gruppen in Ihrem Verzeichnis an.

    ![Zugriffspaket – Richtlinie – Verbundene Organisation hinzufügen – Registerkarte „Sponsoren“](./media/entitlement-management-organization/organization-sponsors.png)

1. Überprüfen Sie Ihre Organisationseinstellungen auf der Registerkarte **Überprüfen und erstellen**, und klicken Sie dann auf **Erstellen**.

    ![Zugriffspaket – Richtlinie – Verbundene Organisation hinzufügen – Registerkarte „Überprüfen und erstellen“](./media/entitlement-management-organization/organization-review-create.png)

## <a name="delete-a-connected-organization"></a>Löschen einer verbundenen Organisation

Wenn die Beziehung mit einem externen Azure AD-Verzeichnis bzw. einer externe Domäne nicht länger besteht, können Sie die verbundene Organisation löschen.

**Erforderliche Rolle:** „Globaler Administrator“, „Benutzeradministrator“ oder „Gasteinladender“

1. Klicken Sie im Azure-Portal auf **Azure Active Directory** und dann auf **Identity Governance**.

1. Klicken Sie im Menü links auf **Verbundene Organisationen** und anschließend auf die verbundene Organisation, um sie zu öffnen.

1. Klicken Sie auf der Übersichtsseite auf **Löschen**, um die verbundene Organisation zu löschen.

    Aktuell kann eine verbundene Organisation nur dann gelöscht werden, wenn keine verbundenen Benutzer vorhanden sind.

    ![Identity Governance – Verbundene Organisationen – Verbundene Organisation löschen](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Nächste Schritte

- [Steuern des Zugriffs für externe Benutzer in der Azure AD-Berechtigungsverwaltung (Vorschau)](entitlement-management-organization.md)
- [Für Benutzer, die sich nicht in Ihrem Verzeichnis befinden](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
