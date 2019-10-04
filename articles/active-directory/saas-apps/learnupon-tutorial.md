---
title: 'Tutorial: Azure Active Directory-Integration mit LearnUpon | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LearnUpon konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ecdcd8c6024b3cacb422b556718bbbdbb5d601c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098235"
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Tutorial: Azure Active Directory-Integration mit LearnUpon

In diesem Tutorial erfahren Sie, wie Sie LearnUpon in Azure Active Directory (Azure AD) integrieren.
Die Integration von LearnUpon in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf LearnUpon hat.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LearnUpon anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LearnUpon konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* LearnUpon-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.


* LearnUpon unterstützt **IDP-initiiertes** einmaliges Anmelden.

* Die Anwendung unterstützt die **Just-in-Time**-Benutzerbereitstellung.


## <a name="adding-learnupon-from-the-gallery"></a>Hinzufügen von LearnUpon aus dem Katalog

Zum Konfigurieren der Integration von LearnUpon in Azure AD müssen Sie LearnUpon aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um LearnUpon aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **LearnUpon** ein, wählen Sie im Ergebnisbereich **LearnUpon** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![LearnUpon in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit LearnUpon mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LearnUpon eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei LearnUpon müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für LearnUpon](#configure-learnupon-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines LearnUpon-Testbenutzers](#create-learnupon-test-user)** , um eine Entsprechung von Britta Simon in LearnUpon zu erhalten, die mit ihrer Entsprechung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit LearnUpon die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **LearnUpon** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für LearnUpon](common/idp-reply.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<companyname>.learnupon.com/saml/consumer`.

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Den Wert erhalten Sie vom [Supportteam für den LearnUpon-Client](https://www.learnupon.com/features/support/). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** den **FINGERABDRUCK**. Dieser Wert wird Ihren SAML-Einstellungen für LearnUpon hinzugefügt.

    ![Downloadlink für das Zertifikat](common/certificateraw.png)

6. Kopieren Sie im Abschnitt **LearnUpon einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-learnupon-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für LearnUpon

1. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich mit einem Administratorkonto bei LearnUpon an.

1. Klicken Sie auf die Registerkarte **Einstellungen** .

    ![Configure single sign-on](./media/learnupon-tutorial/tutorial_learnupon_06.png)

1. Klicken Sie auf **Einmaliges Anmelden – SAML** und anschließend auf **Allgemeine Einstellungen**, um SAML-Einstellungen zu konfigurieren.
   
    ![Configure single sign-on](./media/learnupon-tutorial/tutorial_learnupon_07.png) 

1. Führen Sie im Abschnitt **Allgemeine Einstellungen** die folgenden Schritte aus:
   
    ![Configure single sign-on](./media/learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Wählen Sie **Aktiviert**.

    b. Wählen Sie für **Version** den Wert **2.0** aus.

    c. Wählen Sie für **Bedingungen überspringen** die Option **Nein** aus.

    d. Geben Sie im Textfeld für den **POST-Parametername für SAML-Token** den Namen des POST-Parameters zur oben angegebenen SAML-Verbraucher-URL mit der zu überprüfenden und authentifizierenden SAML-Assertion ein. Beispiel: **SAMLResponse**.

    e. Geben Sie im Textfeld **Name Identifier Format** (Namensbezeichnerformat) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich die Benutzer-ID (E-Mail-Adresse) befindet. Beispiel: `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`
  
    f. Geben Sie im Textfeld **Identify Provider Location** (Anbieterstandort identifizieren) den Wert ein, der angibt, an welche Adresse die Benutzer weitergeleitet werden, wenn sie auf dem Anmeldebildschirm des Azure-Portals auf das von Ihnen hochgeladene Symbol klicken.
  
    g. Fügen Sie im Textfeld **Sign-Out URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    h. Klicken Sie auf **Manage finger prints**(Fingerabdrücke verwalten), und laden Sie dann den Fingerabdruck des heruntergeladenen Zertifikats hoch.

1. Klicken Sie auf **Benutzereinstellungen**, und führen Sie die folgenden Schritte aus:

     ![Configure single sign-on](./media/learnupon-tutorial/tutorial_learnupon_11.png)  

    a. Geben Sie im Textfeld **First Name Identifier Format** (Format der Vornamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Vorname des Benutzers befindet, etwa `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
  
    b. Geben Sie im Textfeld **Last Name Identifier Format** (Format der Nachnamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Nachname des Benutzers befindet, z.B. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein. Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf LearnUpon gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **LearnUpon** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **LearnUpon**aus.

    ![LearnUpon-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-learnupon-test-user"></a>Erstellen eines LearnUpon-Testbenutzers

In diesem Abschnitt wird in LearnUpon ein Benutzer mit dem Namen Britta Simon erstellt. LearnUpon unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in LearnUpon vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt. Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam von LearnUpon](https://www.learnupon.com/features/support/) in Verbindung.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LearnUpon“ klicken, sollten Sie automatisch bei der LearnUpon-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)