---
title: Hinzufügen einer Nicht-Kataloganwendung – Microsoft Identity Platform | Microsoft-Dokumentation
description: Fügen Sie Ihrem Azure AD-Mandanten eine Nicht-Kataloganwendung hinzu.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 10/24/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6656361fd4634c46cd5216b57eb8465536319f09
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062811"
---
# <a name="add-an-unlisted-non-gallery-application-to-your-azure-ad-organization"></a>Hinzufügen einer nicht aufgeführten Anwendung (Nicht-Kataloganwendung) zu Ihrer Azure AD-Organisation

Zusätzlich zu den Optionen im [Azure AD-Anwendungskatalog](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) haben Sie die Möglichkeit, eine **Nicht-Kataloganwendung** hinzuzufügen. Sie können in Ihrer Organisation bereits vorhandene Anwendungen oder Anwendungen eines Anbieters hinzufügen, der noch nicht im Azure AD-Katalog gelistet ist. Abhängig von Ihrem [Lizenzvertrag](https://azure.microsoft.com/pricing/details/active-directory/) sind die folgenden Funktionen verfügbar:

- Self-Service-Integration von Anwendungen, die [Security Assertion Markup Language (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0)-Identitätsanbieter unterstützen (vom Dienstanbieter (SP) oder vom Identitätsanbieter (IdP) initiiert)
- Self-Service-Integration für Webanwendungen, die über eine HTML-basierte Anmeldeseite mit [kennwortbasierter einmaliger Anmeldung (Single Sign-On, SSO)](what-is-single-sign-on.md#password-based-sso)
- Self-Service-Verbindung für Anwendungen, die das [SCIM-Protokoll (System for Cross-Domain Identity Management) verwenden](use-scim-to-provision-users-and-groups.md)
- Möglichkeit zum Hinzufügen von Links zu Anwendungen im [Office 365-App-Startfeld](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) oder im [Azure AD-Zugriffsbereich](what-is-single-sign-on.md#linked-sign-on)

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal den **Unternehmensanwendungen** eine Nicht-Kataloganwendung hinzufügen, ohne Code schreiben zu müssen. Wenn Sie stattdessen nach einer Anleitung für Entwickler zum Integrieren von benutzerdefinierten Apps in Azure AD suchen, finden Sie diese unter [Authentifizierungsszenarien für Azure AD](../develop/authentication-scenarios.md). Wenn Sie eine App entwickeln, die ein modernes Protokoll wie [OpenID Connect/OAuth](../develop/active-directory-v2-protocols.md) zum Authentifizieren von Benutzern verwendet, können Sie die App im Azure-Portal über die Oberfläche für [App-Registrierungen](../develop/quickstart-register-app.md) bei der Microsoft Identity Platform registrieren.

## <a name="add-a-non-gallery-application"></a>Hinzufügen einer nicht im Katalog enthaltenen Anwendung

1. Melden Sie sich mit Ihrem Administratorkonto für die Microsoft Identity Platform beim [Azure Active Directory-Portal](https://aad.portal.azure.com/) an.

2. Wählen Sie **Unternehmensanwendungen** > **Neue Anwendung** aus.

3. (Optional, aber empfohlen) Geben Sie im Suchfeld **Azure AD-Katalog durchsuchen** den Anzeigename der Anwendung ein. 

4. Wählen Sie **Eigene Anwendung erstellen** aus. Die Seite **Eigene Anwendung erstellen** wird angezeigt.

   ![Anwendung hinzufügen](media/add-non-gallery-app/create-your-own-application.png)

5. Beginnen Sie mit der Eingabe des Anzeigenamens für Ihre neue Anwendung. Wenn Kataloganwendungen mit ähnlichen Namen vorhanden sind, werden diese in einer Suchergebnisliste angezeigt.

   > [!NOTE]
   > Es wird empfohlen, nach Möglichkeit die Katalogversion Ihrer Anwendung zu verwenden. Wenn die Anwendung, die Sie hinzufügen möchten, in den Suchergebnissen angezeigt wird, wählen Sie die Anwendung aus, und überspringen den Rest dieses Verfahrens.

6. Unter **Was haben Sie mit Ihrer Anwendung vor?** wählen Sie **Beliebige andere, nicht im Katalog zu findende Anwendung integrieren** aus. Diese Option wird in der Regel für SAML- und WS-Fed-Anwendungen verwendet.

   > [!NOTE]
   > Die anderen beiden Optionen werden in den folgenden Szenarien verwendet:
   >* **Anwendungsproxy für sicheren Remotezugriff auf eine lokale Anwendung konfigurieren** öffnet die Konfigurationsseite für Azure AD-Anwendungsproxy und -Connectors.
   >* **Von Ihnen bearbeitete Anwendung für die Integration in Azure AD registrieren** öffnet die Seite **App-Registrierungen**. Diese Option wird in der Regel für OpenID Connect-Anwendungen verwendet.

7. Klicken Sie auf **Erstellen**. Die Seite **Übersicht** der Anwendung wird geöffnet.

## <a name="configure-user-sign-in-properties"></a>Konfigurieren der Eigenschaften für die Benutzeranmeldung

1. Wählen Sie **Eigenschaften** aus, um den Bereich „Eigenschaften“ zur Bearbeitung zu öffnen.

    ![Bereich „Eigenschaften bearbeiten“](media/add-non-gallery-app/edit-properties.png)

2. Legen Sie mit den folgenden Optionen fest, wie sich Benutzer mit bzw. ohne Anwendungszuweisung bei der Anwendung anmelden können und ob einem Benutzer die Anwendung im Zugriffsbereich angezeigt wird.

    - Mit **Aktiviert für die Benutzeranmeldung?** wird festgelegt, ob sich Benutzer, die der Anwendung zugewiesen sind, anmelden können.
    - Mit **Benutzerzuweisung erforderlich?** wird festgelegt, ob sich Benutzer, die der Anwendung nicht zugewiesen sind, anmelden können.
    - Mit **Für Benutzer sichtbar?** wird festgelegt, ob für der App zugewiesene Benutzer die Anwendung im Zugriffsbereich und im Office 365-Startfeld angezeigt wird.

      Verhalten für **zugewiesene** Benutzer:

       | Eigenschafteneinstellungen der Anwendung | | | Zugewiesene Benutzer | |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich zugewiesene Benutzer anmelden? | Können zugewiesene Benutzer die Anwendung sehen?* |
       | Ja | Ja | Ja | Ja | Ja  |
       | Ja | Ja | no  | Ja | no   |
       | Ja | no  | Ja | Ja | Ja  |
       | Ja | no  | no  | Ja | no   |
       | no  | Ja | Ja | no  | no   |
       | no  | Ja | no  | no  | no   |
       | no  | no  | Ja | no  | no   |
       | no  | no  | no  | no  | no   |

      Verhalten für **nicht zugewiesene** Benutzer:

       | Eigenschafteneinstellungen der Anwendung | | | Nicht zugewiesene Benutzer | |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich nicht zugewiesene Benutzer anmelden? | Können nicht zugewiesene Benutzer die Anwendung sehen?* |
       | Ja | Ja | Ja | no  | no   |
       | Ja | Ja | no  | no  | no   |
       | Ja | no  | Ja | Ja | no   |
       | Ja | no  | no  | Ja | no   |
       | no  | Ja | Ja | no  | no   |
       | no  | Ja | no  | no  | no   |
       | no  | no  | Ja | no  | no   |
       | no  | no  | no  | no  | no   |

     *Kann der Benutzer die Anwendung im Zugriffsbereich und im Office 365-App-Startfeld sehen?

3. Wenn Sie ein benutzerdefiniertes Logo verwenden möchten, erstellen Sie ein Logo mit 215 x 215 Pixeln, und speichern Sie es im PNG-Format. Navigieren Sie dann zu Ihrem Logo, und laden Sie es hoch.

    ![Ändern des Logos](media/add-non-gallery-app/change-logo.png)

4. Wählen Sie abschließend **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihrer Azure AD-Organisation die Anwendung hinzugefügt haben, können Sie jetzt die gewünschte [Methode für einmaliges Anmelden auswählen](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Lesen Sie dazu die folgenden Artikel:

- [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-single-sign-on-non-gallery-applications.md)
- [Konfigurieren des einmaligen Anmeldens per Kennwort](configure-password-single-sign-on-non-gallery-applications.md)
- [Konfigurieren der Anmeldung über Link](configure-linked-sign-on.md)
