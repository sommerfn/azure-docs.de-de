---
title: 'Tutorial: Azure Active Directory-Integration mit Absorb LMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Absorb LMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 936de76d1117c56f5a9dec48b51f33b9afa15351
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107497"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Tutorial: Azure Active Directory-Integration mit Absorb LMS

In diesem Tutorial erfahren Sie, wie Sie Absorb LMS in Azure Active Directory (Azure AD) integrieren.
Die Integration von Absorb LMS in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Absorb LMS hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Absorb LMS anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Absorb LMS konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Absorb LMS-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Absorb LMS unterstützt **IDP-initiiertes** einmaliges Anmelden.

## <a name="adding-absorb-lms-from-the-gallery"></a>Hinzufügen von Absorb LMS über den Katalog

Zum Konfigurieren der Integration von Absorb LMS in Azure AD müssen Sie Absorb LMS aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Absorb LMS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Absorb LMS** ein, wählen Sie im Ergebnisbereich **Absorb LMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Absorb LMS in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Absorb LMS mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Absorb LMS eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Absorb LMS müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Absorb LMS](#configure-absorb-lms-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Absorb LMS-Testbenutzers](#create-absorb-lms-test-user)** , um eine Entsprechung von Britta Simon in Absorb LMS zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Absorb LMS die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Absorb LMS** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![SSO-Informationen zur Domäne und zu den URLs für Absorb LMS](common/idp-intiated.png)

    Verwenden Sie für **Absorb 5 – UI** die folgende Konfiguration:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://company.myabsorb.com/account/saml`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://company.myabsorb.com/account/saml`

    Verwenden Sie für **Absorb 5 – New Learner Experience** die folgende Konfiguration:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://company.myabsorb.com/api/rest/v2/authentication/saml`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://company.myabsorb.com/api/rest/v2/authentication/saml`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von Absorb LMS](https://support.absorblms.com/hc/), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Der folgende Screenshot zeigt die Liste der Standardattribute. **nameidentifier** ist hier **user.userprincipalname** zugeordnet.

    ![image](common/edit-attribute.png)

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML**-Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

7. Kopieren Sie im Abschnitt **Absorb LMS einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-absorb-lms-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Absorb LMS

1. Melden Sie sich in einem neuen Webbrowserfenster bei der Absorb LMS-Unternehmenswebsite als Administrator an.

2. Wählen Sie oben rechts die Schaltfläche **Konto**.

    ![Schaltfläche „Konto“](./media/absorblms-tutorial/1.png)

3. Wählen Sie im Bereich „Konto“ die Option **Portaleinstellungen**.

    ![Link „Portaleinstellungen“](./media/absorblms-tutorial/2.png)

4. Wählen Sie die Registerkarte **SSO-Einstellungen verwalten** aus.

    ![Die Registerkarte „Benutzer“](./media/absorblms-tutorial/managesso.png)

5. Führen Sie auf der Konfigurationsseite zum **Verwalten der SSO-Einstellungen** die folgenden Schritte aus:

    ![Seite für die Konfiguration des einmaligen Anmeldens](./media/absorblms-tutorial/settings.png)

    a. Geben Sie im Textfeld **Name** einen Namen wie Azure AD Marketplace SSO ein.

    b. Wählen Sie **SAML** als **Methode** aus.

    c. Öffnen Sie im Editor das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben. Entfernen Sie die Tags **---BEGIN CERTIFICATE---** und **---END CERTIFICATE---** . Fügen Sie anschließend im Feld **Schlüssel** den verbleibenden Inhalt ein.

    d. Wählen Sie im Feld **Modus** die Option **Identity Provider Initiated** (Vom Identitätsanbieter initiiert).

    e. Wählen Sie im Feld **Id Property** (ID-Eigenschaft) das Attribut aus, das Sie in Azure AD als Benutzerbezeichner konfiguriert haben. Wählen Sie beispielsweise *nameidentifier* aus, wenn in Azure AD **Benutzername** ausgewählt ist.

    f. Wählen Sie **Sha256** als **Signaturtyp** aus.

    g. Fügen Sie im Feld **Anmelde-URL** die **URL für den Benutzerzugriff** von der Seite **Eigenschaften** der Anwendung im Azure-Portal hinzu.

    h. Fügen Sie als **Abmelde-URL** den Wert für **Abmelde-URL** ein, den Sie im Azure-Portal im Fenster **Anmelden konfigurieren** kopiert haben.

    i. Wechseln Sie für **Automatische Umleitung** zu **Ein**.

6. Wählen Sie **Speichern** aus.

    ![Umschalter „Only Allow SSO Login“ (Nur SSO-Anmeldung zulassen)](./media/absorblms-tutorial/save.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon\@yourcompanydomain.extension` ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Absorb LMS gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, und wählen Sie dann **Alle Anwendungen** und **Absorb LMS** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Absorb LMS** ein und wählen Sie es aus.

    ![Absorb LMS-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-absorb-lms-test-user"></a>Erstellen eines Absorb LMS-Testbenutzers

Damit sich Azure AD-Benutzer an Absorb LMS anmelden können, müssen sie in Absorb LMS eingerichtet werden. Bei Absorb LMS ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der Absorb LMS-Unternehmenswebsite als Administrator an.

2. Wählen Sie im Bereich **Benutzer** die Option **Benutzer**.

    ![Link „Benutzer“](./media/absorblms-tutorial/absorblms_userssub.png)

3. Wählen Sie die Registerkarte **Benutzer** aus.

    ![Dropdownliste „Neu hinzufügen“](./media/absorblms-tutorial/absorblms_createuser.png)

4. Gehen Sie auf der Seite **Benutzer hinzufügen** wie folgt vor:

    ![Seite „Benutzer hinzufügen“](./media/absorblms-tutorial/user.png)

    a. Geben Sie im Feld **Vorname** den Vornamen ein, z.B. **Britta**.

    b. Geben Sie im Feld **Nachname** den Nachnamen ein, z.B. **Simon**.

    c. Geben Sie im Feld **Benutzername** einen vollständigen Namen ein, z.B. **Britta Simon**.

    d. Geben Sie im Feld **Kennwort** das Benutzerkennwort ein.

    e. Geben Sie das Kennwort im Feld **Kennwort bestätigen** noch einmal ein.

    f. Legen Sie den Umschalter **Ist aktiv** auf **Aktiv** fest.

5. Wählen Sie **Speichern** aus.

    ![Umschalter „Only Allow SSO Login“ (Nur SSO-Anmeldung zulassen)](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > Standardmäßig ist die Benutzerbereitstellung beim einmaligen Anmelden nicht aktiviert. Wenn der Kunde dieses Feature aktivieren möchte, muss es gemäß der Beschreibung [in dieser](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) Dokumentation eingerichtet werden. Beachten Sie auch, dass die Benutzerbereitstellung nur unter **Absorb 5 – New Learner Experience** mit ACS URL-`https://company.myabsorb.com/api/rest/v2/authentication/saml` verfügbar ist.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Absorb LMS“ klicken, sollten Sie automatisch bei Ihrer Absorb LMS-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
