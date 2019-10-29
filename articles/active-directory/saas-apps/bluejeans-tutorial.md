---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit BlueJeans for Azure AD | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BlueJeans for Azure AD konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/18/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfc38f63c5b6361122c236543320b91d22faa70a
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595046"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-bluejeans-for-azure-ad"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit BlueJeans for Azure AD

In diesem Tutorial erfahren Sie, wie Sie BlueJeans for Azure AD in Azure Active Directory (Azure AD) integrieren. Die Integration von BlueJeans for Azure AD in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf BlueJeans for Azure AD hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei BlueJeans for Azure AD anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein BlueJeans for Azure AD-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* BlueJeans for Azure AD unterstützt **SP-initiiertes** einmaliges Anmelden.

* BlueJeans for Azure AD unterstützt die [**automatisierte** Benutzerbereitstellung](bluejeans-provisioning-tutorial.md).

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-bluejeans-for-azure-ad-from-the-gallery"></a>Hinzufügen von BlueJeans for Azure AD aus dem Katalog

Zum Konfigurieren der Integration von BlueJeans for Azure AD in Azure AD müssen Sie BlueJeans for Azure AD aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **BlueJeans for Azure AD** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **BlueJeans for Azure AD** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-bluejeans-for-azure-ad"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für BlueJeans for Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit BlueJeans for Azure AD mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BlueJeans for Azure AD eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit BlueJeans for Azure AD die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für BlueJeans for Azure AD](#configure-bluejeans-for-azure-ad-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines BlueJeans for Azure AD-Testbenutzers](#create-bluejeans-for-azure-ad-test-user)** , um ein Pendant von B. Simon in BlueJeans for Azure AD zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **BlueJeans for Azure AD** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.bluejeans.com`

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Den Wert erhalten Sie vom [Supportteam für den BlueJeans for Azure AD-Client](https://support.bluejeans.com/contact). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **BlueJeans for Azure AD einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BlueJeans for Azure AD gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **BlueJeans for Azure AD** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-bluejeans-for-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens für BlueJeans for Azure AD

1. Melden Sie sich in einem anderen Webbrowserfenster bei der **BlueJeans for Azure AD**-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **ADMIN \> GROUP SETTINGS \> SECURITY** (ADMINISTRATOR > GRUPPENEINSTELLUNGEN > SICHERHEIT).

    ![Administrator](./media/bluejeans-tutorial/ic785868.png "Administrator")

3. Führen Sie im Abschnitt **SECURITY** (SICHERHEIT) die folgenden Schritte aus:

    ![SAML Single Sign On](./media/bluejeans-tutorial/ic785869.png "SAML Single Sign On") (SAML-SSO)

    a. Wählen Sie **SAML Single Sign On**aus.

    b. Wählen Sie **Enable automatic provisioning**aus.

4. Führen Sie die folgenden Schritte aus:

    ![Zertifikatpfad](./media/bluejeans-tutorial/ic785870.png "Zertifikatpfad")

    a. Klicken Sie auf **Choose File** (Datei auswählen), um das Base64-codierte Zertifikat hochzuladen, das Sie über das Azure-Portal heruntergeladen haben.

    b. Fügen Sie in das Textfeld **Login URL** (Anmelde-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Password Change URL** (URL für Kennwortänderung) den Wert der **URL für Kennwortänderung** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Fügen Sie in das Textfeld **Logout URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

5. Führen Sie die folgenden Schritte aus:

    ![Save Changes](./media/bluejeans-tutorial/ic785874.png "Änderungen speichern")

    a. Geben Sie in das Textfeld **Benutzer-ID** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    b. Geben Sie in das Textfeld **E-Mail** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    c. Klicken Sie auf **ÄNDERUNGEN SPEICHERN**.

### <a name="create-bluejeans-for-azure-ad-test-user"></a>Erstellen eines BlueJeans for Azure AD-Testbenutzers

In diesem Abschnitt wird in BlueJeans for Azure AD ein Benutzer namens B. Simon erstellt. BlueJeans for Azure AD unterstützt die automatische Benutzerbereitstellung (standardmäßig aktiviert). Weitere Details zur Konfiguration der automatischen Benutzerbereitstellung finden Sie [hier](bluejeans-provisioning-tutorial.md).

**Wenn Sie einen Benutzer manuell erstellen möchten, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der **BlueJeans for Azure AD**-Unternehmenswebsite als Administrator an.

2. Navigieren Sie zu **ADMIN \> MANAGE USERS \> ADD USER** (ADMINISTRATOR > BENUTZER VERWALTEN > BENUTZER HINZUFÜGEN).

    ![Administrator](./media/bluejeans-tutorial/ic785877.png "Administrator")

    > [!IMPORTANT]
    > Die Registerkarte **ADD USER** (BENUTZER HINZUFÜGEN) ist nur verfügbar, wenn auf der Registerkarte **SECURITY** (SICHERHEIT) die Option **Enable automatic provisioning** (Automatische Bereitstellung aktivieren) deaktiviert ist.

3. Führen Sie im Abschnitt **ADD USER** (BENUTZER HINZUFÜGEN) die folgenden Schritte aus:

    ![Add User](./media/bluejeans-tutorial/ic785886.png "Benutzer hinzufügen") (Benutzer hinzufügen)

    a. Geben Sie im Textfeld **First name** (Vorname) den Vornamen des Benutzers ein, z. B. **B**.

    b. Geben Sie im Textfeld **Last Name** (Nachname) den Nachnamen des Benutzers ein, z.B. **Simon**.

    c. Geben Sie im Textfeld **Pick a BlueJeans for Azure AD Username** (BlueJeans for Azure AD-Benutzernamen auswählen) den Namen eines Benutzers ein, etwa **Brittasimon**.

    d. Geben Sie in das Textfeld **Create a Password** (Kennwort erstellen) Ihr Kennwort ein.

    e. Geben Sie im Textfeld **Company** (Unternehmen) Ihr Unternehmen ein.

    f. Geben Sie im Textfeld **Email Address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z.B. `b.simon\@contoso.com`.

    g. Geben Sie im Textfeld **Create a BlueJeans for Azure AD Meeting I.D** (BlueJeans for Azure AD-Meeting-ID erstellen) Ihre Meeting-ID ein.

    h. Geben Sie im Textfeld **Pick a Moderator Passcode** (Moderatorenkennung auswählen) Ihre Kennung ein.

    i. Klicken Sie auf **CONTINUE** (WEITER).

    ![Add User](./media/bluejeans-tutorial/ic785887.png "Benutzer hinzufügen") (Benutzer hinzufügen)

    J. Klicken Sie auf **BENUTZER HINZUFÜGEN**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von BlueJeans for Azure AD-Benutzerkonten oder mithilfe von APIs bereitstellen.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BlueJeans for Azure AD“ klicken, sollten Sie automatisch bei der BlueJeans for Azure AD-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [BlueJeans for Azure AD mit Azure AD ausprobieren](https://aad.portal.azure.com/)

