---
title: 'Tutorial: Azure Active Directory-Integration in Cisco Umbrella | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden (Single Sign-On, SSO) zwischen Azure Active Directory und Cisco Umbrella konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 675dca98-f119-4463-8350-d6a45d5601e3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ee91ca53e32bfdc387dc20054493d02d506a75da
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73158642"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-umbrella"></a>Tutorial: Azure Active Directory-Integration in Cisco Umbrella

In diesem Tutorial erfahren Sie, wie Sie Cisco Umbrella in Azure Active Directory (Azure AD) integrieren.
Die Integration von Cisco Umbrella in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Cisco Umbrella hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cisco Umbrella anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration in Cisco Umbrella benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein Abonnement, das für einmaliges Anmelden (Single Sign-On, SSO) bei Cisco Umbrella aktiviert ist.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Cisco Umbrella unterstützt **SP- und IDP-** initiiertes einmaliges Anmelden.

## <a name="adding-cisco-umbrella-from-the-gallery"></a>Hinzufügen von Cisco Umbrella aus dem Katalog

Zum Konfigurieren der Integration von Cisco Umbrella in Azure AD müssen Sie Cisco Umbrella aus dem Katalog zu Ihrer Liste verwalteter SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Cisco Umbrella aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie **Cisco Umbrella** in das Suchfeld ein, wählen Sie im Ergebnisbereich **Cisco Umbrella** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Cisco Umbrella in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei [Anwendungsname] mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in [Anwendungsname] eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei [Anwendungsname] müssen Sie die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Cisco Umbrella](#configure-cisco-umbrella-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Cisco Umbrella-Testbenutzers](#create-cisco-umbrella-test-user)** , um eine Entsprechung von Britta Simon in Cisco Umbrella zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit [Anwendungsname] zu konfigurieren:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Cisco Umbrella** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Im Abschnitt **Grundlegende SAML-Konfiguration** muss der Benutzer keine Schritte ausführen, weil die App bereits in Azure integriert ist.

    ![SSO-Informationen zur Domäne und zu den URLs für Cisco Umbrella](common/both-preintegrated-signon.png)

    a. Wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten, führen Sie die folgenden Schritte aus:

    b. Klicken Sie auf **Zusätzliche URLs festlegen**.

    c. Geben Sie im Textfeld **Anmelde-URL** eine URL wie die Folgende ein: `https://login.umbrella.com/sso`.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Metadaten-XML** aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

6. Kopieren Sie im Abschnitt **Cisco Umbrella einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-cisco-umbrella-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Cisco Umbrella

1. Melden Sie sich in einem anderen Browserfenster bei der Cisco Umbrella-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der linken Seite des Menüs auf **Admin** (Administrator), navigieren Sie zu **Authentication** (Authentifizierung), und klicken Sie dann auf **SAML**.

    ![Administrator](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_admin.png)

3. Wählen Sie **Other** (Andere) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Option „Other“ (Andere)](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_other.png)

4. Klicken Sie auf der Seite **Cisco Umbrella Metadata** (Cisco Umbrella-Metadaten) auf **Next** (Weiter).

    ![Metadaten](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_metadata.png)

5. Wenn Sie SAML vorab konfiguriert haben, wählen Sie auf der Registerkarte **Upload Metadata** (Metadaten hochladen) die Option **Click here to change them** (Zum Ändern hier klicken) aus, und führen Sie die folgenden Schritte aus.

    ![Schaltfläche „Next“ (Weiter)](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_next.png)

6. Laden Sie unter **Option A: Upload XML file** (Option A: XML-Datei hochladen) die **Verbundmetadaten-XML**-Datei hoch, die Sie aus dem Azure-Portal heruntergeladen haben. Nach dem Hochladen der Metadaten werden die folgenden Werte automatisch aufgefüllt. Klicken Sie anschließend auf **Next** (Weiter).

    ![Dateiauswahl](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_choosefile.png)

7. Klicken Sie im Abschnitt **Validate SAML Configuration** (SAML-Konfiguration überprüfen) auf **TEST YOUR SAML CONFIGURATION** (SAML-KONFIGURATION TESTEN).

    ![Test](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_test.png)

8. Klicken Sie auf **SPEICHERN**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cisco Umbrella gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**, **Alle Anwendungen** und dann **Cisco Umbrella** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Cisco Umbrella** ein, und wählen Sie den Eintrag aus.

    ![Cisco Umbrella-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-cisco-umbrella-test-user"></a>Erstellen des Cisco Umbrella-Testbenutzers

Damit sich Azure AD-Benutzer bei Cisco Umbrella anmelden können, müssen sie in Cisco Umbrella bereitgestellt werden.  
Im Fall von Cisco Umbrella ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich in einem anderen Browserfenster bei der Cisco Umbrella-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der linken Seite des Menüs auf **Admin** (Administrator), und navigieren Sie zu **Accounts** (Konten).

    ![Konto](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_account.png)

3. Klicken Sie oben rechts auf der Seite **Accounts** (Konten) auf **Add** (Hinzufügen), und führen Sie die folgenden Schritte aus.

    ![Der Benutzer](./media/cisco-umbrella-tutorial/tutorial_cisco-umbrella_createuser.png)

    a. Geben Sie im Feld **First Name** (Vorname) den Vornamen ein (in diesem Beispiel **Britta**).

    b. Geben Sie im Feld **Last Name** (Nachname) den Nachnamen ein (in diesem Beispiel **Simon**).

    c. Wählen Sie unter **Choose Delegated Admin Role** (Delegierte Administratorrolle auswählen) Ihre Rolle aus.
  
    d. Geben Sie im Feld **Email address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z. B. **brittasimon\@contoso.com**.

    e. Geben Sie Ihr Kennwort in das Feld **Password** (Kennwort) ein.

    f. Wiederholen Sie die Eingabe des Kennworts im Feld **Confirm Password** (Kennwort bestätigen).

    g. Klicken Sie auf **ERSTELLEN**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Cisco Umbrella“ klicken, sollten Sie automatisch bei der Cisco Umbrella-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
