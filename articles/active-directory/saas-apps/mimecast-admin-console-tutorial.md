---
title: 'Tutorial: Azure Active Directory-Integration mit Mimecast Admin Console | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Mimecast Admin Console konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9edadd6462052f82f92c05c1678f845ece856cfb
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73160651"
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Tutorial: Azure Active Directory-Integration mit Mimecast Admin Console

In diesem Tutorial erfahren Sie, wie Sie Mimecast Admin Console in Azure Active Directory (Azure AD) integrieren.
Die Integration von Mimecast Admin Console in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Mimecast Admin Console hat.
* Sie können es Ihren Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Mimecast Admin Console angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Mimecast Admin Console konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Mimecast Admin Console-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Mimecast Admin Console unterstützt **SP**-initiiertes einmaliges Anmelden.

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Hinzufügen von Mimecast Admin Console aus dem Katalog

Zum Konfigurieren der Integration von Mimecast Admin Console in Azure AD müssen Sie Mimecast Admin Console über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Mimecast Admin Console aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Mimecast Admin Console** ein, wählen Sie im Ergebnisbereich **Mimecast Admin Console** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Mimecast Admin Console in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Mimecast Admin Console mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mimecast Admin Console eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Mimecast Admin Console müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Mimecast Admin Console](#configure-mimecast-admin-console-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Mimecast Admin Console-Testbenutzers](#create-mimecast-admin-console-test-user)** , um eine Entsprechung von Britta Simon in Mimecast Admin Console zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Mimecast Admin Console die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Mimecast Admin Console** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Mimecast Admin Console](common/sp-signonurl.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein:
    
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Die Anmelde-URL ist regionsspezifisch.

4. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Mimecast Admin Console einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-mimecast-admin-console-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Mimecast Admin Console

1. Melden Sie sich in einem anderen Webbrowserfenster bei Mimecast Admin Console als Administrator an.

2. Wechseln Sie zu **Dienste \> Anwendung**.

    ![Dienste](./media/mimecast-admin-console-tutorial/ic794998.png "Dienste")

3. Klicken Sie auf **Authentifizierungsprofile**.

    ![Authentication Profiles (Authentifizierungsprofile)](./media/mimecast-admin-console-tutorial/ic794999.png "Authentifizierungsprofile")
    
4. Klicken Sie auf **Neues Authentifizierungsprofil**.

    ![New Authentication Profile (Neues Authentifizierungsprofil)](./media/mimecast-admin-console-tutorial/ic795000.png "Neue Authentifizierungsprofile")

5. Führen Sie im Abschnitt **Authentifizierungsprofil** die folgenden Schritte aus:

    ![Authentication Profile (Authentifizierungsprofil)](./media/mimecast-admin-console-tutorial/ic795015.png "Authentifizierungsprofil")
    
    a. Geben Sie im Textfeld **Beschreibung** einen Namen für die Konfiguration ein.
    
    b. Wählen Sie **SAML-Authentifizierung für Mimecast Admin Console aktivieren**aus.
    
    c. Wählen Sie für **Anbieter** die Option **Azure Active Directory** aus.
    
    d. Fügen Sie den **Azure AD-Bezeichner**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Aussteller-URL** ein.
    
    e. Fügen Sie in das Textfeld **Login URL** (Anmelde-URL) die **Anmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    f. Fügen Sie in das Textfeld **Logout URL** (Abmelde-URL) die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.
    
    >[!NOTE]
    >Die Werte für Anmelde-URL und Abmelde-URL sind bei Mimecast Admin Console gleich.
    
    g. Öffnen Sie das Base64-Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, in Editor, entfernen Sie die erste Zeile ( *--* ) und die letzte Zeile ( *--* ), und kopieren Sie den übrigen Inhalt in die Zwischenablage. Fügen Sie ihn anschließend in das Textfeld **Identity Provider Certificate (Metadata)** (Identitätsanbieterzertifikat (Metadaten)) ein.
    
    h. Wählen Sie **Einmaliges Anmelden zulassen**aus.
    
    i. Klicken Sie auf **Speichern**.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mimecast Admin Console gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Mimecast Admin Console**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Mimecast Admin Console** ein, und wählen Sie den Eintrag aus.

    ![Mimecast Admin Console-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-mimecast-admin-console-test-user"></a>Erstellen eines Mimecast Admin Console-Testbenutzers

Damit sich Azure AD-Benutzer bei Mimecast Admin Console anmelden können, müssen Sie in Mimecast Personal Portal bereitgestellt werden. Im Fall von Mimecast Admin Console ist die Bereitstellung eine manuelle Aufgabe.

* Sie müssen eine Domäne registrieren, bevor Sie Benutzer erstellen können.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei **Mimecast Admin Console** als Administrator an.

2. Wechseln Sie zu **Verzeichnisse \> Intern**.
   
    ![Verzeichnisse](./media/mimecast-admin-console-tutorial/ic795003.png "Verzeichnisse")

3. Klicken Sie auf **Neue Domäne registrieren**.
   
    ![Register New Domain (Neue Domäne registrieren)](./media/mimecast-admin-console-tutorial/ic795004.png "Neue Domäne registrieren")

4. Nachdem die neue Domäne erstellt wurde, klicken Sie auf **Neue Adresse**.
   
    ![New Address (Neue Adresse)](./media/mimecast-admin-console-tutorial/ic795005.png "Neue Adresse")

5. Führen Sie im Dialogfeld „Neue Adresse“ die folgenden Schritte aus:
   
    ![Speichern](./media/mimecast-admin-console-tutorial/ic795006.png "Speichern")
   
    a. Geben Sie die Attribute **E-Mail-Adresse**, **Globaler Name**, **Kennwort** und **Kennwort bestätigen** eines gültigen Azure AD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.

    b. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Mimecast Admin Console-Benutzerkonten oder mithilfe der von Mimecast Admin Console bereitgestellten APIs erstellen. 

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Mimecast Admin Console“ klicken, sollten Sie automatisch bei Ihrer Mimecast Admin Console-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist bedingter Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

