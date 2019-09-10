---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Jamf Pro | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Jamf Pro konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24f0d067351ab2e24b103624c8534ac8d6f2e757
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305426"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jamf-pro"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Jamf Pro

In diesem Tutorial erfahren Sie, wie Sie Jamf Pro in Azure Active Directory (Azure AD) integrieren. Die Integration von Jamf Pro in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Jamf Pro hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Jamf Pro anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Jamf Pro-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Jamf Pro unterstützt **SP- und IDP**-initiiertes SSO.

## <a name="adding-jamf-pro-from-the-gallery"></a>Hinzufügen von Jamf Pro aus dem Katalog

Zum Konfigurieren der Integration von Jamf Pro in Azure AD müssen Sie Jamf Pro aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Jamf Pro** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Jamf Pro** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-jamf-pro"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Jamf Pro

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Jamf Pro mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Jamf Pro eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Jamf Pro die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Jamf Pro](#configure-jamf-pro-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Jamf Pro-Testbenutzers](#create-jamf-pro-test-user)** , um ein Pendant von B. Simon in Jamf Pro zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Jamf Pro** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie erhalten den tatsächlichen Wert des Bezeichners im Abschnitt **Single Sign-On** (Einmaliges Anmelden) des Jamf Pro-Portals, der später in diesem Tutorial beschrieben wird. Sie können den tatsächlichen Wert **subdomain** aus dem Bezeichnerwert extrahieren und diese **subdomain**-Informationen in Anmelde-URL und Antwort-URL verwenden. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Jamf Pro gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Jamf Pro** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-jamf-pro-sso"></a>Konfigurieren des einmaligen Anmeldens für Jamf Pro

1. Wenn Sie die Konfiguration in Jamf Pro automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

2. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Jamf Pro einrichten**, um zur Anwendung Jamf Pro weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Jamf Pro anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 7.

    ![Einrichtungskonfiguration](common/setup-sso.png)

3. Wenn Sie Jamf Pro manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der Jamf Pro-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

4. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Klicken Sie auf **Single Sign On** (Einmaliges Anmelden).

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Führen Sie auf der Seite **Single Sign-On** (Einmaliges Anmelden) die folgenden Schritte aus:

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Aktivieren Sie **Enable Single Sign-On Authentication** (SSO-Authentifizierung aktivieren).

    b. Wählen Sie **Other** (Sonstige) im Dropdownmenü **IDENTITY PROVIDER** (IDENTITÄTSANBIETER) aus.

    c. Geben Sie im Textfeld **OTHER PROVIDER** (Anderer Anbieter) **Azure AD** ein.

    d. Kopieren Sie den Wert für **ENTITY ID** (ENTITÄTS-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ins Textfeld **Bezeichner (Entitäts-ID)** ein.

    > [!NOTE]
    > Sie benötigen den Wert für `<SUBDOMAIN>`, um die Anmelde-URL und die Antwort-URL im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** zu vervollständigen.

    e. Wählen Sie **Metadata URL** (Metadaten-URL) als Option in der Dropdownliste **IDENTITY PROVIDER METADATA SOURCE** (IDENTITÄTSANBIETER-METADATENQUELLE) aus, und fügen Sie in das folgende Textfeld den Wert **Verbundmetadaten-URL der App** ein, den Sie im Azure-Portal kopiert haben.

7. Scrollen Sie auf der gleichen Seite nach unten zu **User Mapping** (Benutzerzuordnung), und führen Sie die folgenden Schritte aus: 

    ![Jamf Pro – einmaliges Anmelden](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Wählen Sie unter **IDENTITY PROVIDER USER MAPPING** (BENUTZERZUORDNUNG DES IDENTITÄTSANBIETERS) die Option **NameID** aus. Diese Einstellung ist standardmäßig auf **NameID** festgelegt, Sie können jedoch auch ein benutzerdefiniertes Attribut definieren.

    b. Wählen Sie unter **USER MAPPING JAMF PRO** (BENUTZERZUORDNUNG: JAMF PRO) die Option **Email** (E-Mail) aus. Jamf Pro ordnet von der IdP gesendete SAML-Attribute anhand von Benutzern oder von Gruppen zu. Wenn ein Benutzer versucht, auf Jamf Pro zuzugreifen, ruft Jamf Pro standardmäßig Informationen zum Benutzer aus dem Identitätsanbieter ab und gleicht sie mit den Jamf Pro-Benutzerkonten ab. Wenn das Konto eines Benutzers in Jamf Pro nicht vorhanden ist, erfolgt ein Abgleich mit dem Gruppennamen.

    c. Fügen Sie den Wert `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in das Textfeld **IDENTITY PROVIDER GROUP ATTRIBUTE NAME** (ATTRIBUTNAME DER IDENTITÄTSANBIETERGRUPPE) ein.

    d. Wenn Sie **Allow users to bypass the Single Sign-On authentication** (Umgehung der SSO-Authentifizierung für Benutzer zulassen) auswählen, werden Benutzer nicht zur Authentifizierung an die Anmeldeseite des Identitätsanbieters umgeleitet, sondern können sich stattdessen direkt bei Jamf Pro anmelden. Wenn ein Benutzer versucht, sich über den Identitätsanbieter auf Jamf Pro zuzugreifen, erfolgen eine IdP-initiierte SSO-Authentifizierung und -Autorisierung.

    e. Klicken Sie auf **Speichern**.

### <a name="create-jamf-pro-test-user"></a>Erstellen eines Jamf Pro-Testbenutzers

Damit sich Azure AD-Benutzer bei Jamf Pro anmelden können, müssen sie in Jamf Pro bereitgestellt werden. Im Fall von Jamf Pro ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Klicken Sie auf **Jamf Pro User Accounts & Groups** (Jamf Pro-Benutzerkonten und Gruppen).

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user1.png)

4. Klicken Sie auf **New**.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user2.png)

5. Wählen Sie **Create Standard Account** (Standardkonto erstellen) aus.

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user3.png)

6. Führen Sie im Dialogfeld **New Account** (Neues Konto) die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Geben Sie im Textfeld **USERNAME** (BENUTZERNAME) den vollständigen Namen von Britta Simon ein.

    b. Wählen Sie die entsprechenden Optionen gemäß Ihrer Organisation für **ACCESS LEVEL** (ZUGRIFFSEBENE), **PRIVILEGE SET** (BERECHTIGUNGENGRUPPE) und **ACCESS STATUS** (ZUGRIFFSSTATUS) ein.

    c. Geben Sie im Textfeld **FULL NAME** (VOLLSTÄNDIGER NAME) den vollständigen Namen von Britta Simon ein.

    d. Geben Sie im Textfeld **EMAIL ADDRESS** (E-MAIL-ADRESSE) die E-Mail-Adresse des Kontos von Britta Simon ein.

    e. Geben Sie im Textfeld **PASSWORD** (KENNWORT) das Kennwort des Benutzers ein.

    f. Geben Sie im Textfeld **VERIFY PASSWORD** (KENNWORT ÜBERPRÜFEN) das Kennwort des Benutzers ein.

    g. Klicken Sie auf **Speichern**.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Jamf Pro“ klicken, sollten Sie automatisch bei Ihrer Jamf Pro-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Jamf Pro mit Azure AD ausprobieren](https://aad.portal.azure.com/)

