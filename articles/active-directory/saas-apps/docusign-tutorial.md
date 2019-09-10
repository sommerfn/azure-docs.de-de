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
ms.openlocfilehash: fe1f018612e889f49993895d88cdaf9ad732b393
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306295"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit DocuSign

In diesem Tutorial erfahren Sie, wie Sie DocuSign in Azure Active Directory (Azure AD) integrieren. Die Integration von DocuSign in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf DocuSign hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei DocuSign anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* DocuSign-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* DocuSign unterstützt **SP**-initiiertes einmaliges Anmelden.

* DocuSign unterstützt die **Just-in-Time**-Benutzerbereitstellung.

* DocuSign unterstützt die [automatische Benutzerbereitstellung](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial).

## <a name="adding-docusign-from-the-gallery"></a>Hinzufügen von DocuSign aus dem Katalog

Zum Konfigurieren der Integration von DocuSign in Azure AD müssen Sie DocuSign über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **DocuSign** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **DocuSign** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für DocuSign

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit DocuSign mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in DocuSign eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit DocuSign die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für DocuSign](#configure-docusign-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines DocuSign-Testbenutzers](#create-docusign-test-user)** , um eine Entsprechung von B. Simon in DocuSign zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **DocuSign** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Dies wird später in diesem Tutorial beschrieben (siehe Abschnitt **SAML 2.0-Endpunkte anzeigen**).

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **DocuSign einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf DocuSign gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **DocuSign** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-docusign-sso"></a>Konfigurieren des einmaligen Anmeldens für DocuSign

1. Wenn Sie die Konfiguration in DocuSign automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

2. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **DocuSign einrichten**, um zur Anwendung DocuSign weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei DocuSign anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 5.

    ![Einrichtungskonfiguration](common/setup-sso.png)

3. Wenn Sie DocuSign manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der DocuSign-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

4. Klicken Sie oben rechts auf der Profilseite auf das **Logo**, und klicken Sie dann auf **Zur Admin-Umgebung wechseln**.
  
    ![Konfigurieren der einmaligen Anmeldung][51]

5. Klicken Sie auf der Seite Ihrer Domänenlösungen auf **Domänen**.

    ![Konfigurieren der einmaligen Anmeldung][50]

6. Klicken Sie unter dem Abschnitt **Domänen** auf **Domäne anfordern**.

    ![Konfigurieren der einmaligen Anmeldung][52]

7. Geben Sie im Dialogfeld **Domäne anfordern** im Textfeld **Domänenname** Ihre Unternehmensdomäne ein, und klicken Sie dann auf **Anfordern**. Achten Sie darauf, die Domäne zu überprüfen, und vergewissern Sie sich, dass der Status „Aktiv“ lautet.

    ![Konfigurieren der einmaligen Anmeldung][53]

8. Klicken Sie auf der Seite Ihrer Domänenlösungen auf **Identitätsanbieter**.
  
    ![Konfigurieren der einmaligen Anmeldung][54]

9. Klicken Sie unter dem Abschnitt **Identitätsanbieter** auf **Identitätsanbieter hinzufügen**. 

    ![Konfigurieren der einmaligen Anmeldung][55]

10. Führen Sie auf der Seite mit den **Identitätsanbietereinstellungen** die folgenden Schritte aus:

    ![Konfigurieren der einmaligen Anmeldung][56]

    a. Geben Sie im Textfeld **Name** einen eindeutigen Namen für die Konfiguration ein. Verwenden Sie keine Leerzeichen.

    b. Fügen Sie in das Textfeld **Aussteller des Identitätsanbieters** den Wert vom **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Anmelde-URL des Identitätsanbieters** den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie in das Textfeld **Abmelde-URL des Identitätsanbieters** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. Wählen Sie **Authentifizierungsanforderung signieren** aus.

    f. Wählen Sie für **Send AuthN request by** (Authentifizierungsanforderung senden per) die Option **POST** aus.

    g. Wählen Sie für **Send logout request by** (Abmeldeanforderung senden per) die Option **GET** aus.

    h. Klicken Sie im Abschnitt **Benutzerdefinierte Attributzuordnung** auf **Neue Zuordnung hinzufügen**.

    ![Konfigurieren der einmaligen Anmeldung][62]

    i. Wählen Sie das Feld aus, das Sie dem Azure AD-Anspruch zuordnen möchten. In diesem Beispiel wird der Anspruch **emailaddress** mit dem Wert **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** zugeordnet. Dies ist der Standardanspruchsname aus Azure AD für den E-Mail-Anspruch. Klicken Sie dann auf **Speichern**.

    ![Konfigurieren der einmaligen Anmeldung][57]

    > [!NOTE]
    > Verwenden Sie den entsprechenden **Benutzerbezeichner** , um den Benutzer aus Azure AD der DocuSign-Benutzerzuordnung zuzuordnen. Wählen Sie das richtige Feld, und geben Sie den entsprechenden Wert basierend auf den Einstellungen Ihrer Organisation ein.

    j. Klicken Sie im Abschnitt **Zertifikat des Identitätsanbieters** auf **Zertifikat hinzufügen**. Laden Sie dann das aus dem Azure AD-Portal heruntergeladene Zertifikat hoch, und klicken Sie auf **Speichern**.

    ![Konfigurieren der einmaligen Anmeldung][58]

    k. Klicken Sie im Abschnitt **Identitätsanbieter** auf **Aktionen**, und klicken Sie dann auf **Endpunkte**.

    ![Konfigurieren der einmaligen Anmeldung][59]

    l. Führen Sie im **DocuSign-Verwaltungsportal** im Abschnitt **SAML 2.0-Endpunkte anzeigen** die folgenden Schritte aus:

    ![Konfigurieren der einmaligen Anmeldung][60]

    * Kopieren Sie die **Aussteller-URL des Dienstanbieters**, und fügen Sie die URL dann im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Bezeichner** ein.

    * Kopieren Sie die **Service Provider Login URL** (Anmelde-URL des Dienstanbieters), und fügen Sie sie dann im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Anmelde-URL** ein.

    * Klicken Sie unten auf der Seite auf **Schließen**

### <a name="create-docusign-test-user"></a>Erstellen eines DocuSign-Testbenutzers

In diesem Abschnitt wird in DocuSign ein Benutzer namens B. Simon erstellt. DocuSign unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in DocuSign vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

>[!Note]
>Setzen Sie sich mit dem[Supportteam von DocuSign](https://support.docusign.com/) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „DocuSign“ klicken, sollten Sie automatisch bei Ihrer DocuSign-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

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