---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit DocuSign | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und DocuSign konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f02492dd7c9563c8c0002a63f0b105bd0be8b14
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71345573"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit DocuSign

In diesem Tutorial erfahren Sie, wie Sie DocuSign in Microsoft Azure Active Directory (Azure AD) integrieren. Die Integration von DocuSign in Azure AD ermöglicht Folgendes:

* Steuern Sie mit Azure AD, wer Zugriff auf DocuSign hat.
* Aktivieren Sie für Ihre Benutzer die automatische Anmeldung bei DocuSign über ihre Azure AD-Konten.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein DocuSign-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung, um Folgendes zu überprüfen:

* DocuSign unterstützt SP-initiiertes (Service Provider, Dienstanbieter) einmaliges Anmelden.

* DocuSign unterstützt die *Just-in-Time*-Benutzerbereitstellung.

* DocuSign unterstützt die [automatische Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial).

## <a name="adding-docusign-from-the-gallery"></a>Hinzufügen von DocuSign aus dem Katalog

Zum Konfigurieren der Integration von DocuSign in Azure AD müssen Sie DocuSign aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Navigationsbereich auf der linken Seite den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **DocuSign** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **DocuSign** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für DocuSign

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit DocuSign mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in DocuSign eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit DocuSign die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), damit Ihre Benutzer dieses Feature verwenden können
    1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit B.Simon zu testen
    1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B.Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
1. [Konfigurieren des einmaligen Anmeldens für DocuSign](#configure-docusign-sso), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
1. [Erstellen eines DocuSign-Testbenutzers](#create-docusign-test-user), um eine Entsprechung von B. Simon in DocuSign zu generieren, die mit ihrer Darstellung in Azure AD verknüpft ist
1. [Testen Sie das einmalige Anmelden](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert.

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Führen Sie die folgenden Schritte aus, um einmaliges Anmelden von Azure AD im Azure-Portal zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **DocuSign** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    a. Geben Sie im Feld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. Geben Sie im Feld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Die Werte in Klammern sind Platzhalter. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Ausführliche Informationen hierzu finden Sie weiter unten in diesem Tutorial im Abschnitt „View SAML 2.0 Endpoints“ (SAML 2.0-Endpunkte anzeigen).

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** nach **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **DocuSign einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im Azure-Portal im linken Bereich **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie **B.Simon** in das Feld **Name** ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge `<username>@<companydomain>.<extension>` ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie den Wert im Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie B. Simon Zugriff auf DocuSign, damit sie das einmalige Anmelden von Azure verwenden kann.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **DocuSign** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der **Benutzer**liste die Option **B.Simon** und anschließend am unteren Bildschirmrand die Schaltfläche **Auswählen** aus.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.

## <a name="configure-docusign-sso"></a>Konfigurieren des einmaligen Anmeldens für DocuSign

1. Wenn Sie die Konfiguration in DocuSign automatisieren möchten, müssen Sie die Browsererweiterung „Meine Apps“ für die sichere Anmeldung installieren, indem Sie **Erweiterung installieren** auswählen.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

2. Wählen Sie nach dem Hinzufügen der Erweiterung zum Browser **DocuSign einrichten** aus. Sie werden zur Anwendung DocuSign weitergeleitet. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei DocuSign anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch und automatisiert die Schritte 3 bis 5.

    ![Einrichtungskonfiguration](common/setup-sso.png)

3. Wenn Sie DocuSign manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der DocuSign-Unternehmenswebsite als Administrator an.

4. Wählen Sie in der oberen rechten Ecke das Profilbild und anschließend **Go to Admin** (Zur Admin-Umgebung wechseln) aus.
  
    ![Option „Go to Admin“ (Zur Admin-Umgebung wechseln) im Profil][51]

5. Wählen Sie auf der Seite Ihrer Domänenlösungen **Domains** (Domänen) aus.

    ![Domänenlösungen/Domains (Domänen)][50]

6. Wählen Sie im Abschnitt **Domains** (Domänen) die Option **CLAIM DOMAIN** (DOMÄNE ANFORDERN) aus.

    ![Option „Claim Domain“ (Domäne anfordern)][52]

7. Geben Sie im Dialogfeld **Claim a Domain** (Domäne anfordern) im Textfeld **Domain Name** (Domänenname) Ihre Unternehmensdomäne ein, und wählen Sie dann **CLAIM** (ANFORDERN) aus. Überprüfen Sie die Domäne, und vergewissern Sie sich, dass der Status „Aktiv“ lautet.

    ![Dialogfeld „Claim a Domain“ (Domäne anfordern)/„Domain Name“ (Domänenname)][53]

8. Wählen Sie auf der Seite Ihrer Domänenlösungen die Option **Identity Providers** (Identitätsanbieter) aus.
  
    ![Option „Identity Providers“ (Identitätsanbieter)][54]

9. Wählen Sie im Abschnitt **Identity Providers** (Identitätsanbieter) die Option **ADD IDENTITY PROVIDER** (IDENTITÄTSANBIETER HINZUFÜGEN) aus.

    ![Option „Add Identity Provider“ (Identitätsanbieter hinzufügen)][55]

10. Führen Sie auf der Seite **Identity Provider Settings** (Identitätsanbietereinstellungen) die folgenden Schritte aus:

    ![Felder unter „Identity Provider Settings“ (Identitätsanbietereinstellungen)][56]

    a. Geben Sie im Feld **Name** einen eindeutigen Namen für die Konfiguration ein. Verwenden Sie keine Leerzeichen.

    b. Fügen Sie in das Feld **Identity Provider Issuer** (Aussteller des Identitätsanbieters) den Wert von **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Feld **Identity Provider Login URL** (Anmelde-URL des Identitätsanbieters) die **Anmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie in das Feld **Abmelde-URL des Identitätsanbieters** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Wählen Sie **Authentifizierungsanforderung signieren** aus.

    f. Wählen Sie für **Send AuthN request by** (Authentifizierungsanforderung senden per) die Option **POST** aus.

    g. Wählen Sie für **Send logout request by** (Abmeldeanforderung senden per) die Option **GET** aus.

    h. Wählen Sie im Abschnitt **Custom Attribute Mapping** (Benutzerdefinierte Attributzuordnung) die Option **ADD NEW MAPPING** (NEUE ZUORDNUNG HINZUFÜGEN) aus.

       ![Benutzeroberfläche „Custom Attribute Mapping“ (Benutzerdefinierte Attributzuordnung)][62]

    i. Wählen Sie das Feld aus, das Sie dem Azure AD-Anspruch zuordnen möchten. In diesem Beispiel wird der Anspruch **emailaddress** dem Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` zugeordnet. Dies ist der Standardanspruchsname aus Azure AD für den E-Mail-Anspruch. Wählen Sie **SAVE** (SPEICHERN) aus.

       ![Felder unter „Custom Attribute Mapping“ (Benutzerdefinierte Attributzuordnung)][57]

       > [!NOTE]
       > Verwenden Sie den entsprechenden **Benutzerbezeichner** , um den Benutzer aus Azure AD der DocuSign-Benutzerzuordnung zuzuordnen. Wählen Sie das richtige Feld aus, und geben Sie den entsprechenden Wert basierend auf den Einstellungen Ihrer Organisation ein.

    j. Wählen Sie im Abschnitt **Identity Provider Certificates** (Identitätsanbieterzertifikate) die Option **ADD CERTIFICATE** (ZERTIFIKAT HINZUFÜGEN) aus. Laden Sie dann das aus dem Azure AD-Portal heruntergeladene Zertifikat hoch, und wählen Sie **SAVE** (SPEICHERN) aus.

       ![Identity Provider Certificates (Identitätsanbieterzertifikate)/Add Certificate (Zertifikat hinzufügen)][58]

    k. Wählen Sie im Abschnitt **Identity Providers** (Identitätsanbieter) die Option **ACTIONS** (AKTIONEN) und anschließend **Endpoints** (Endpunkte) aus.

       ![Identity Providers (Identitätsanbieter)/Endpoints (Endpunkte)][59]

    l. Führen Sie im DocuSign-Verwaltungsportal im Abschnitt **View SAML 2.0 Endpoints** (SAML 2.0-Endpunkte anzeigen) die folgenden Schritte aus:
       1. Kopieren Sie den Wert unter **Service Provider Issuer URL** (Aussteller-URL des Dienstanbieters), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Feld **Bezeichner** ein.

       1. Kopieren Sie den Wert unter **Service Provider Login URL** (Anmelde-URL des Dienstanbieters), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Feld **Anmelde-URL** ein.

       1. Klicken Sie auf **Schließen**.

       ![View SAML 2.0 Endpoints (SAML 2.0-Endpunkte anzeigen)][60]

### <a name="create-docusign-test-user"></a>Erstellen eines DocuSign-Testbenutzers

In diesem Abschnitt wird in DocuSign ein Benutzer namens B. Simon erstellt. DocuSign unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in DocuSign vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

>[!Note]
>Setzen Sie sich mit dem[Supportteam von DocuSign](https://support.docusign.com/) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „DocuSign“ auswählen, sollten Sie automatisch bei der DocuSign-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden in Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist bedingter Zugriff in Azure AD?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [DocuSign mit Azure AD ausprobieren](https://aad.portal.azure.com/)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png
