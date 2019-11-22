---
title: 'Tutorial: Azure Active Directory-Integration mit Atlassian Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Atlassian Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f901bd37a54d12dc2afe805f71c7b018a277e34
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72969623"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>Tutorial: Integrieren von Atlassian Cloud und Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Atlassian Cloud in Azure Active Directory (Azure AD) integrieren. Die Integration von Atlassian Cloud in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Atlassian Cloud hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Atlassian Cloud anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie kein Abonnement besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige kostenlose Testversion erhalten.
* Atlassian Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist
* Um SAML-SSO (Security Assertion Markup Language) für Atlassian Cloud-Produkte zu ermöglichen, müssen Sie Atlassian Access einrichten. Hier finden Sie weitere Informationen zu [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. 

* Atlassian Cloud unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Hinzufügen von Atlassian Cloud aus dem Katalog

Zum Konfigurieren der Integration von Atlassian Cloud in Azure AD müssen Sie Atlassian Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Atlassian Cloud** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Atlassian Cloud** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Atlassian Cloud mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Atlassian Cloud eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Atlassian Cloud müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Atlassian Cloud](#configure-atlassian-cloud-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Atlassian Cloud-Testbenutzers](#create-atlassian-cloud-test-user)** , um eine Entsprechung von B. Simon in Atlassian Cloud zu erhalten, die mit ihrer Benutzerdarstellung in Azure AD verknüpft ist.
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Atlassian Cloud** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://auth.atlassian.com/saml/<unique ID>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`

    c. Klicken Sie auf **Zusätzliche URLs festlegen**.

    d. Geben Sie im Textfeld **Relayzustand** eine URL nach folgendem Muster ein: `https://<instancename>.atlassian.net`.

    > [!NOTE]
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Diese tatsächlichen Werte können Sie dem Bildschirm für die **Atlassian Cloud-SAML-Konfiguration** entnehmen, die später in diesem Tutorial unter **Konfigurieren des einmaligen Anmeldens für Atlassian Cloud** erläutert wird.

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<instancename>.atlassian.net`

    > [!NOTE]
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Fügen Sie den Wert aus der Instanz ein, mit der Sie sich beim Verwaltungsportal von Atlassian Cloud anmelden.

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. Ihre Atlassian Cloud-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. **nameidentifier** ist hier **user.userprincipalname** zugeordnet. Die Atlassian Cloud-Anwendung erwartet, dass **nameidentifier** der Wert **user.mail** zugeordnet ist. Sie müssen die Attributzuordnung daher entsprechend ändern, indem Sie auf das Symbol **Bearbeiten** klicken.

    ![image](common/edit-attribute.png)

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** den Eintrag **Zertifikat (Base64)** . Klicken Sie auf **Herunterladen**, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Atlassian Cloud einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

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

In diesem Abschnitt ermöglichen Sie B. Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Atlassian Cloud gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Atlassian Cloud** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-atlassian-cloud-sso"></a>Konfigurieren des einmaligen Anmeldens für Atlassian Cloud

1. Wenn Sie die Konfiguration in Atlassian Cloud automatisieren möchten, müssen Sie die Browsererweiterung **Meine Apps** für die sichere Anmeldung installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

1. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Atlassian Cloud einrichten**, um zur Anwendung „Atlassian Cloud“ weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Atlassian Cloud anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 7.

    ![Einrichtungskonfiguration](common/setup-sso.png)

1. Wenn Sie Atlassian Cloud manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, melden Sie sich bei der Atlassian Cloud-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

1. Vor dem Konfigurieren des einmaligen Anmeldens müssen Sie Ihre Domäne überprüfen. Weitere Informationen finden Sie im Dokument zur [Atlassian-Domänenüberprüfung](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

1. Wählen Sie im linken Bereich **Security** > **SAML single sign-on** (Sicherheit -> Einmalige SAML-Anmeldung) aus. Wenn dies noch nicht erfolgt ist, abonnieren Sie Identity Manager von Atlassian.

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

1. Gehen Sie im Fenster **Add SAML configuration** (SAML-Konfiguration hinzufügen) folgendermaßen vor:

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. Fügen Sie in das Feld für die **Entitäts-ID des Identitätsanbieters** den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie in das Feld für die **SSO-URL des Identitätsanbieters** die **Anmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das aus dem Azure-Portal in einer TXT-Datei heruntergeladene Zertifikat, kopieren Sie den Wert (ohne die Zeilen *Begin Certificate* und *End Certificate*), und fügen Sie es dann in das Feld **Public X509 certificate** (Öffentliches X509-Zertifikat) ein.

    d. Klicken Sie auf **Konfiguration speichern**.

1. Um sicherzustellen, dass Sie die richtigen URLs eingerichtet haben, aktualisieren Sie folgendermaßen die Azure AD-Einstellungen:

    ![Einmaliges Anmelden konfigurieren](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. Kopieren Sie im SAML-Fenster die **SP-Identitäts-ID**, und fügen Sie sie dann im Azure-Portal unter **Grundlegende SAML-Konfiguration** für Atlassian Cloud in das Feld **Bezeichner** ein.

    b. Kopieren Sie im SAML-Fenster die **URL des SP-Assertionsverbraucherdiensts**, und fügen Sie sie dann im Azure-Portal unter **Grundlegende SAML-Konfiguration** für Atlassian Cloud in das Feld **Antwort-URL** ein. Die Anmelde-URL ist die Mandanten-URL für Ihre Atlassian Cloud.

    > [!NOTE]
    > Wenn Sie Bestandskunde sind, wählen Sie nach dem Aktualisieren der Werte für **SP Identity ID** (SP-Identitäts-ID) und **SP Assertion Consumer Service URL** (URL des SP-Assertionsverbraucherdiensts) **Yes, update configuration** (Ja, Konfiguration aktualisieren) aus. Als Neukunde können Sie diesen Schritt überspringen.

### <a name="create-atlassian-cloud-test-user"></a>Erstellen eines Atlassian Cloud-Testbenutzers

Um Azure AD-Benutzern die Anmeldung bei Atlassian Cloud zu ermöglichen, stellen Sie ihre Benutzerkonten in Atlassian Cloud manuell bereit, indem Sie wie folgt vorgehen:

1. Wählen Sie im Bereich **Administration** (Verwaltung) **Users** (Benutzer) aus.

    ![Atlassian Cloud-Benutzerlink](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

1. Um einen Benutzer in Atlassian Cloud zu erstellen, wählen Sie **Invite user** (Benutzer einladen) aus.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

1. Geben Sie im Feld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, und weisen Sie dann den Anwendungszugriff zu.

    ![Erstellen eines Atlassian Cloud-Benutzers](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

1. Um dem Benutzer eine E-Mail-Einladung zu senden, wählen Sie **Invite users** (Benutzer einladen) aus. Eine E-Mail-Einladung wird an den Benutzer gesendet, und nach dem Akzeptieren der Einladung ist der Benutzer im System aktiv.

> [!NOTE]
> Sie können auch eine Massenerstellung von Benutzern ausführen, indem Sie die Schaltfläche **Bulk Create** (Massenerstellung) im Abschnitt **Users** (Benutzer) auswählen.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich auf die Kachel „Atlassian Cloud“ klicken, sollten Sie automatisch bei Ihrer Atlassian Cloud-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Atlassian Cloud mit Azure AD ausprobieren](https://aad.portal.azure.com/)