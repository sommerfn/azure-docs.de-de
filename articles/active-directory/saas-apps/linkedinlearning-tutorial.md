---
title: 'Tutorial: Azure Active Directory-Integration mit LinkedIn Learning | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedIn Learning konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ff3748213d8449704f7b1001469eb8f7a224498
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67098068"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: Azure Active Directory-Integration mit LinkedIn Learning

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Learning in Azure Active Directory (Azure AD) integrieren.
Die Integration von LinkedIn Learning in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Learning haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Learning anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LinkedIn Learning konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein SSO-fähiges LinkedIn Learning-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* LinkedIn Learning unterstützt durch **SP und IDP** initiiertes einmaliges Anmelden.
* LinkedIn Learning unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-linkedin-learning-from-the-gallery"></a>Hinzufügen von LinkedIn Learning aus dem Katalog

Zum Konfigurieren der Integration von LinkedIn Learning in Azure AD müssen Sie LinkedIn Learning aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Learning aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **LinkedIn Learning** ein, wählen Sie im Ergebnisbereich **LinkedIn Learning** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![LinkedIn Learning in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Learning mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Learning eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei LinkedIn Learning über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für LinkedIn Learning](#configure-linkedin-learning-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines LinkedIn Learning-Testbenutzers](#create-linkedin-learning-test-user)** , um in LinkedIn Learning eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit LinkedIn Learning zu konfigurieren:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **LinkedIn Learning** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für LinkedIn Learning](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie in das Textfeld **Antwort-URL** den Wert für **Assertion Consumer Service (ACS)-URL** ein, den Sie aus dem LinkedIn-Portal kopiert haben.

    c. Wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten, klicken Sie im Abschnitt **Grundlegende SAML-Konfiguration**, in dem Sie Ihre Anmelde-URL angeben, auf **Zusätzliche URLs festlegen**. Um Ihre Anmelde-URL zu erstellen, kopieren Sie die **Assertion Consumer Service (ACS)-URL**, und ersetzen dabei „/saml/“ durch „/login/“. Danach sollte die Anmelde-URL folgendes Muster aufweisen:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![SSO-Informationen zur Domäne und zu den URLs für LinkedIn Learning](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Diese Werte müssen durch die tatsächlichen Werte für Bezeichner und Antwort-URL ersetzt werden. Entsprechende Informationen finden Sie im Abschnitt **Konfigurieren des einmaligen Anmeldens für LinkedIn Learning** dieses Tutorials.

5. Die LinkedIn Learning-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. **nameidentifier** ist hier **user.userprincipalname** zugeordnet. Die LinkedIn Learning-Anwendung erwartet, dass **nameidentifier** der Wert **user.mail** zugeordnet ist. Sie müssen die Attributzuordnung daher entsprechend ändern, indem Sie auf das Symbol **Bearbeiten** klicken.

    ![image](common/edit-attribute.png)

6. Darüber hinaus wird von der LinkedIn Learning-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:
    
    | NAME | Quellattribut |
    | ---------------| --------------- |
    | email  | user.mail  |
    | department  | user.department  |
    | firstname  | user.givenname  |
    | lastname  | user.surname  |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

7. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML**-Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

### <a name="configure-linkedin-learning-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für LinkedIn Learning

1. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem LinkedIn Learning-Mandanten als Administrator an.

2. Klicken Sie im **Account Center** unter **Settings** auf **Global Settings**. Wählen Sie außerdem aus der Dropdownliste die Option **Learning - Default** aus.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, kopieren Sie die **Entitäts-ID** und die **Assertion Consumer Service (ACS)-URL**, und fügen Sie sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ein.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Laden Sie die XML-Datei hoch, die Sie gerade aus dem Azure-Portal heruntergeladen haben, indem Sie auf die Option **XML-Datei hochladen** klicken.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Configure single sign-on](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf LinkedIn Learning gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **LinkedIn Learning** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **LinkedIn Learning** ein, und wählen Sie die entsprechende Option aus.

    ![Der Link für LinkedIn Learning in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-linkedin-learning-test-user"></a>Erstellen eines LinkedIn Learning-Testbenutzers

Die LinkedIn Learning-Anwendung unterstützt die Just-In-Time-Benutzerbereitstellung. Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie im LinkedIn Learning-Portal auf der Seite mit den Administratoreinstellungen die Option **Automatically Assign licenses** (Lizenzen automatisch zuweisen), um die Just-In-Time-Bereitstellung zu aktivieren. Dadurch wird dem Benutzer auch eine Lizenz zugewiesen.

   ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LinkedIn Learning“ klicken, sollten Sie automatisch bei der LinkedIn Learning-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

