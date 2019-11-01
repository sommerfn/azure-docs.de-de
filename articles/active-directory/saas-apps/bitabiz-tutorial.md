---
title: 'Tutorial: Azure Active Directory-Integration mit BitaBIZ | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BitaBIZ konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f827945cbeccacfdf048865b6e89b6947fe7de9
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73159385"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Tutorial: Azure Active Directory-Integration mit BitaBIZ

In diesem Tutorial erfahren Sie, wie Sie BitaBIZ in Azure Active Directory (Azure AD) integrieren.
Die Integration von BitaBIZ in Azure AD hat folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf BitaBIZ haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei BitaBIZ anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit BitaBIZ benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein BitaBIZ-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* BitaBIZ unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

## <a name="adding-bitabiz-from-the-gallery"></a>Hinzufügen von BitaBIZ über den Katalog

Zum Konfigurieren der Integration von BitaBIZ in Azure AD müssen Sie BitaBIZ aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um BitaBIZ über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld die Zeichenfolge **BitaBIZ** ein, wählen Sie im Ergebnisbereich die Option **BitaBIZ** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![BitaBIZ in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit BitaBIZ mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BitaBIZ eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei BitaBIZ müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für BitaBIZ](#configure-bitabiz-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines BitaBIZ-Testbenutzers](#create-bitabiz-test-user)** , um in BitaBIZ eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit BitaBIZ die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **BitaBIZ** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für BitaBIZ](common/idp-identifier.png)

    Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://www.bitabiz.com/<instanceId>`.

    > [!NOTE]
    > Der Wert in der obigen URL dient lediglich zur Veranschaulichung. Ersetzen Sie ihn durch den tatsächlichen Bezeichner. Dies wird später in diesem Tutorial beschrieben.

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![image](common/both-preintegrated-signon.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://www.bitabiz.com/dashboard`.

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

7. Kopieren Sie im Abschnitt **BitaBIZ einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-bitabiz-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für BitaBIZ

1. Melden Sie sich in einem anderen Webbrowserfenster als Administrator bei Ihrem BitaBIZ-Mandanten an.

2. Klicken Sie auf **SETUP ADMIN** (ADMINISTRATOR EINRICHTEN).

    ![BitaBIZ-Konfiguration](./media/bitabiz-tutorial/settings1.png)

3. Klicken Sie im Abschnitt **Add value** (Aufwerten) auf **Microsoft integrations** (Microsoft-Integrationen).

    ![BitaBIZ-Konfiguration](./media/bitabiz-tutorial/settings2.png)

4. Scrollen Sie zum Abschnitt **Microsoft Azure AD (Enable single sign on)** (Microsoft Azure AD (einmaliges Anmelden aktivieren)), und führen Sie die folgenden Schritte aus:

    ![BitaBIZ-Konfiguration](./media/bitabiz-tutorial/settings3.png)

    a. Kopieren Sie den Wert aus dem Textfeld **Entity ID (”Identifier” in Azure AD)** (Entitäts-ID ("Bezeichner" in Azure AD)), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Bezeichner** ein. 

    b. Fügen Sie im Textfeld **Azure AD Single Sign-On Service URL** (Azure AD-Dienst-URL für einmaliges Anmelden) die **Anmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Azure AD SAML Entity ID** (Azure AD-SAML-Entitäts-ID) den Wert für **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Öffnen Sie das heruntergeladene **Zertifikat (Base64)** in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Azure AD Signing Certificate (Base64 encoded)** (Azure AD-Signaturzertifikat (Base64-codiert) ein.

    e. Fügen Sie Ihren geschäftlichen E-Mail-Domänennamen (mycompany.com) in das Textfeld **Domain name** (Domänenname) ein, um SSO den Benutzern in Ihrem Unternehmen mit dieser E-Mail-Domäne zuzuweisen (OPTIONAL).

    f. Aktivieren Sie das Kontrollkästchen **SSO enabled** (SSO aktiviert) für das BitaBIZ-Konto.

    g. Klicken Sie auf **Save Azure AD configuration** (Azure AD-Konfiguration speichern), um die SSO-Konfiguration zu speichern und zu aktivieren.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BitaBIZ gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, und wählen Sie dann **Alle Anwendungen** und **BitaBIZ** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste die Option **BitaBIZ** aus.

    ![BitaBIZ-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-bitabiz-test-user"></a>Erstellen eines BitaBIZ-Testbenutzers

Damit sich Azure AD-Benutzer bei BitaBIZ anmelden können, müssen sie in BitaBIZ bereitgestellt werden.  
Im Fall von BitaBIZ muss die Bereitstellung manuell durchgeführt werden.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer BitaBIZ-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **SETUP ADMIN** (ADMINISTRATOR EINRICHTEN).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/bitabiz-tutorial/settings1.png)

3. Klicken Sie im Abschnitt **Organization** (Organisation) auf **Add users** (Benutzer hinzufügen).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/bitabiz-tutorial/user1.png)

4. Klicken Sie auf **Add new employee** (Neuen Mitarbeiter hinzufügen).

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/bitabiz-tutorial/user2.png)

5. Führen Sie auf der Dialogfeldseite **Add new employee** (Neuen Mitarbeiter hinzufügen) die folgenden Schritte aus:

    ![Hinzufügen eines Benutzers in BitaBIZ](./media/bitabiz-tutorial/user3.png)

    a. Geben Sie im Textfeld **First Name** (Vorname) den Vornamen des Benutzers ein (beispielsweise „Britta“).

    b. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen des Benutzers ein (beispielsweise „Simon“).

    c. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. Brittasimon@contoso.com, ein.

    d. Wählen Sie unter **Date of employment** (Einstellungsdatum) ein Datum aus.

    e. Für den Benutzer können optional noch weitere Benutzerattribute eingerichtet werden. Ausführlichere Informationen finden Sie in der [Dokumentation für die Mitarbeitereinrichtung](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee).

    f. Klicken Sie auf **Save employee** (Mitarbeiter speichern).

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BitaBIZ“ klicken, sollten Sie automatisch bei der BitaBIZ-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
