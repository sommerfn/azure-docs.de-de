---
title: 'Tutorial: Azure Active Directory-Integration mit Marketo | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Marketo konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 0488fd1e9bc10d61d6660745acfc8c39becf3a89
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73159476"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Tutorial: Azure Active Directory-Integration mit Marketo

In diesem Tutorial erfahren Sie, wie Sie Marketo in Azure Active Directory (Azure AD) integrieren.
Die Integration von Marketo in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Marketo haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Marketo anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Marketo konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* SSO-fähiges Marketo-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Marketo unterstützt **IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-marketo-from-the-gallery"></a>Hinzufügen von Marketo aus dem Katalog

Zum Konfigurieren der Integration von Marketo in Azure AD müssen Sie Marketo aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Marketo aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie **Marketo** in das Suchfeld ein, wählen Sie im Ergebnisbereich **Marketo** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Marketo in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Marketo mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Marketo eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Marketo müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Marketo](#configure-marketo-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Marketo-Testbenutzers](#create-marketo-test-user)** , um in Marketo eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Marketo zu konfigurieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Marketo** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Marketo](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://saml.marketo.com/sp`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Clientsupportteam von Marketo](https://investors.marketo.com/contactus.cfm), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Marketo einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-marketo-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Marketo

1. Um die Munchkin-ID Ihrer Anwendung zu erhalten, melden Sie sich mit Ihren Administratoranmeldeinformationen bei Marketo an und führen Sie folgende Aktionen aus:
   
    a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-App an.
   
    b. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigieren Sie zum Menü „Integration“, und klicken Sie auf den **Munchkin-Link**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Kopieren Sie die Munchkin-ID, die auf dem Bildschirm angezeigt wird, und schließen Sie Ihre Antwort-URL im Azure AD-Konfigurations-Assistenten ab.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_12.png) 

2. Führen Sie die unten beschriebenen Schritte aus, um SSO in der Anwendung zu konfigurieren:
   
    a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-App an.
   
    b. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigieren Sie zum Menü „Integration“, und klicken Sie auf **Einmaliges Anmelden**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. Klicken zum Aktivieren der SAML-Einstellungen auf die Schaltfläche **Bearbeiten**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Aktivieren** Sie die Einstellungen für einmaliges Anmelden.
   
    f. Fügen Sie den **Azure AD-Bezeichner** in das Textfeld für die **Aussteller-ID** ein.
   
    g. Geben Sie die URL als `http://saml.marketo.com/sp` in das Textfeld **Entitäts-ID** ein.
   
    h. Wählen Sie den Speicherort der Benutzer-ID als **Namensbezeichnerelement** aus.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Wenn Ihre Benutzer-ID kein UPN-Wert ist, ändern Sie den Wert in der Registerkarte „Attribut“.
   
    i. Laden Sie das Zertifikat hoch, das Sie aus dem Azure AD-Konfigurations-Assistenten heruntergeladen haben. **Speichern** Sie die Einstellungen.
   
    j. Bearbeiten Sie die Einstellungen für Seitenumleitungen.
   
    k. Fügen Sie die **Anmelde-URL** in das Textfeld für die **Anmelde-URL** ein.
   
    l. Fügen Sie die **Abmelde-URL** in das Textfeld für die **Abmelde-URL** ein.
   
    m. Kopieren Sie in der **Fehler-URL** die **URL Ihrer Marketo-Instanz**, und klicken Sie auf die Schaltfläche **Speichern**, um die Einstellungen zu speichern.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_10.png)

3. Um SSO für Benutzer zu aktivieren, führen Sie die folgenden Aktionen aus:
   
    a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-App an.
   
    b. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Navigieren Sie zum Menü **Sicherheit**, und klicken Sie auf **Anmeldeeinstellungen**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. Aktivieren Sie die Option **SSO erfordern**, und **speichern** Sie die Einstellungen.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_14.png)

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

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Marketo gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **Marketo** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Marketo**aus.

    ![Marketo-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-marketo-test-user"></a>Erstellen eines Marketo-Testbenutzers

In diesem Abschnitt erstellen Sie in Marketo einen Benutzer namens Britta Simon. Befolgen Sie diese Schritte, um einen Benutzer in der Marketo-Plattform zu erstellen.

1. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-App an.

2. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_06.png) 

3. Navigieren Sie zum Menü **Sicherheit**, und klicken Sie auf **Benutzer und Rollen**
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_19.png)  

4. Klicken Sie auf der Registerkarte „Benutzer“ auf den Link **Neuen Benutzer einladen**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_15.png) 

5. Geben Sie die folgenden Informationen im Assistenten zum Einladen neuer Benutzer ein.
   
    a. Geben Sie die **E-Mail** -Adresse des Benutzers in das Textfeld ein.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Geben Sie den **Vornamen** in das Textfeld ein.
   
    c. Geben Sie den **Nachnamen** in das Textfeld ein.
   
    d. Klicken Sie unten auf der Seite auf **Weiter**

6. Wählen Sie auf der Registerkarte **Berechtigungen** die Option **userRoles** aus, und klicken Sie auf **Weiter**.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_17.png)
7. Klicken Sie auf die Schaltfläche **Senden**, um die Benutzereinladung zu senden.
   
    ![Configure single sign-on](./media/marketo-tutorial/tutorial_marketo_18.png)

8. Der Benutzer erhält die E-Mail-Benachrichtigung und muss auf den Link klicken sowie das Kennwort ändern, um das Konto zu aktivieren. 

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Marketo“ klicken, sollten Sie automatisch bei der Marketo-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

