---
title: 'Tutorial: Integration von Jamf Pro in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Jamf Pro konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00d03df38dee74f4767e8ff3b21d9462fd59c4c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67099585"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integration von Jamf Pro in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Jamf Pro in Azure Active Directory (Azure AD) integrieren.
Die Integration von Jamf Pro in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Jamf Pro hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Jamf Pro anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Jamf Pro konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Jamf Pro-Abonnement, das für das einmalige Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Jamf Pro unterstützt **SP- und IDP**-initiiertes SSO.

## <a name="adding-jamf-pro-from-the-gallery"></a>Hinzufügen von Jamf Pro aus dem Katalog

Zum Konfigurieren der Integration von Jamf Pro in Azure AD müssen Sie Jamf Pro aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Jamf Pro aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Jamf Pro** ein, wählen Sie im Ergebnisbereich **Jamf Pro** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Jamf Pro in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Jamf Pro mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Jamf Pro eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Jamf Pro müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Jamf Pro](#configure-jamf-pro-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Jamf Pro-Testbenutzers](#create-jamf-pro-test-user)** , um ein Pendant von Britta Simon in Jamf Pro zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Jamf Pro die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Jamf Pro** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie im **IDP**-initiierten Modus konfigurieren möchten:

    ![„Domäne und URLs für Jamf Pro“, Informationen zu einmaligem Anmelden](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/SSO`

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com`

    ![„Domäne und URLs für Jamf Pro“, Informationen zu einmaligem Anmelden](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie erhalten den tatsächlichen Wert des Bezeichners im Abschnitt **Single Sign-On** (Einmaliges Anmelden) des Jamf Pro-Portals, der später in diesem Tutorial beschrieben wird. Sie können den tatsächlichen Wert **subdomain** aus dem Bezeichnerwert extrahieren und diese **subdomain**-Informationen in Anmelde-URL und Antwort-URL verwenden. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="configure-jamf-pro-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Jamf Pro

1. Wenn Sie die Konfiguration in Jamf Pro automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![image](./media/jamfprosamlconnector-tutorial/install_extension.png)

2. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Jamf Pro einrichten**, um zur Anwendung Jamf Pro weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Jamf Pro anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 7.

    ![image](./media/jamfprosamlconnector-tutorial/d1_saml.png)

3. Wenn Sie Jamf Pro manuell einrichten möchten, öffnen Sie ein neues Webbrowserfenster, melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

4. Klicken Sie in der oberen rechten Ecke der Seite auf das **Einstellungssymbol**.

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Klicken Sie auf **Single Sign On** (Einmaliges Anmelden).

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Führen Sie auf der Seite **Single Sign-On** (Einmaliges Anmelden) die folgenden Schritte aus:

    ![Jamf Pro – einmaliges Anmelden](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Wählen Sie **Jamf Pro Server** aus, um den Zugriff per einmaligem Anmelden zu aktivieren.

    b. Wenn Sie **Allow bypass for all users** (Umgehung für alle Benutzer zulassen) auswählen, werden Benutzer nicht zur Authentifizierung an die Anmeldeseite des Identitätsanbieters umgeleitet, sondern können sich stattdessen direkt bei Jamf Pro anmelden. Wenn ein Benutzer versucht, sich über den Identitätsanbieter auf Jamf Pro zuzugreifen, erfolgen eine IdP-initiierte SSO-Authentifizierung und -Autorisierung.

    c. Wählen Sie die Option **NameID** für **USER MAPPING: SAML** (BENUTZERZUORDNUNG: SAML) aus. Diese Einstellung ist standardmäßig auf **NameID** festgelegt, Sie können jedoch auch ein benutzerdefiniertes Attribut definieren.

    d. Wählen Sie **Email** für die Option **USER MAPPING: JAMF PRO** (BENUTZERZUORDNUNG: JAMF PRO) aus. Jamf Pro ordnet von der IdP gesendete SAML-Attribute anhand von Benutzern oder von Gruppen zu. Wenn ein Benutzer versucht, auf Jamf Pro zuzugreifen, ruft Jamf Pro standardmäßig Informationen zum Benutzer aus dem Identitätsanbieter ab und gleicht sie mit den Jamf Pro-Benutzerkonten ab. Wenn das Konto eines Benutzers in Jamf Pro nicht vorhanden ist, erfolgt ein Abgleich mit dem Gruppennamen.

    e. Fügen Sie den Wert `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in das Textfeld **GROUP ATTRIBUTE NAME** (ATTRIBUTNAME DER GRUPPE) ein.

7. Scrollen Sie auf der gleichen Seite im Abschnitt **Single Sign-On** (Einmaliges Anmelden) zu **IDENTITY PROVIDER** (IDENTITÄTSANBIETER), und führen Sie die folgenden Schritte aus:

    ![Jamf Pro-Konfiguration](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Wählen Sie **Other** (Sonstige) im Dropdownmenü **IDENTITY PROVIDER** (IDENTITÄTSANBIETER) aus.

    b. Geben Sie im Textfeld **OTHER PROVIDER** (Anderer Anbieter) **Azure AD** ein.

    c. Wählen Sie **Metadata URL** (Metadaten-URL) als Option in der Dropdownliste **IDENTITY PROVIDER METADATA SOURCE** (IDENTITÄTSANBIETER-METADATENQUELLE) aus, und fügen Sie in das folgende Textfeld den Wert **Verbundmetadaten-URL der App** ein, den Sie im Azure-Portal kopiert haben.

    d. Kopieren Sie den Wert der **Entity ID** (Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Jamf Pro** in das Textfeld **Bezeichner (Entitäts-ID)** ein.

    > [!NOTE]
    > Der hier unkenntlich gemachte Name ist die Unterdomäne. Schließen Sie mit diesem Wert die Anmelde-URL und Antwort-URL im Abschnitt **Domäne und URLs für Jamf Pro** im Azure-Portal ab.

    e. Klicken Sie auf **Speichern**.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Jamf Pro gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, wählen Sie **Alle Anwendungen** aus, und wählen Sie dann **Jamf Pro** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Jamf Pro** aus.

    ![Jamf Pro-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-jamf-pro-test-user"></a>Erstellen eines Jamf Pro-Testbenutzers

Damit sich Azure AD-Benutzer bei Jamf Pro anmelden können, müssen sie in Jamf Pro bereitgestellt werden. Im Fall von Jamf Pro ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an.

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

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Jamf Pro“ klicken, sollten Sie automatisch bei Ihrer Jamf Pro-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
