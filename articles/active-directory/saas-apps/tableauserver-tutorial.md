---
title: 'Tutorial: Azure Active Directory-Integration von Tableau Server | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Server konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/22/2019
ms.author: jeedes
ms.openlocfilehash: f9ef179c1a93d8b2f97c47eb4c68d0312d55d3d1
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825981"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Azure Active Directory-Integration von Tableau Server

In diesem Tutorial erfahren Sie, wie Sie Tableau Server in Azure Active Directory (Azure AD) integrieren.
Die Integration von Tableau Server in Azure AD bietet die folgenden Vorteile:

* In Azure AD lässt sich steuern, wer Zugriff auf Tableau Server hat.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Server anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Tableau Server konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Tableau Server-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Tableau Server unterstützt **SP-initiiertes** einmaliges Anmelden.

## <a name="adding-tableau-server-from-the-gallery"></a>Hinzufügen von Tableau Server aus dem Katalog

Zum Konfigurieren der Integration von Tableau Server in Azure AD müssen Sie Tableau Server aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Tableau Server aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Tableau Server** ein, wählen Sie im Ergebnisfenster **Tableau Server** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Tableau Server in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Tableau Server basierend auf einem Testbenutzer mit dem Namen **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Server eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Server müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Tableau Server](#configure-tableau-server-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Tableau Server-Testbenutzers](#create-tableau-server-test-user)** , um eine Entsprechung von Britta Simon in Tableau Server zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Tableau Server die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Tableau Server** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Tableau Server](common/sp-identifier-reply.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://azure.<domain name>.link`

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `https://azure.<domain name>.link`.

    c. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie die Werte mit der aktuellen URL und dem Bezeichner von der Seite „Tableau Server Configuration“ (Tableau Server-Konfiguration) (wird später im Tutorial erläutert).

5. Die Tableau Server-Anwendung erwartet einen benutzerdefinierten Anspruch vom Typ **Benutzername**, der wie folgt definiert werden muss. Dies wird als Benutzer-ID anstelle des Anspruchs „Eindeutige Benutzer-ID“ verwendet. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute und Ansprüche** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute und Ansprüche** zu öffnen.

    ![image](common/edit-attribute.png)

6. Konfigurieren Sie im Dialogfeld **Benutzerattribute und Ansprüche** im Abschnitt **Benutzeransprüche** das SAML-Tokenattribut wie in der obigen Abbildung gezeigt, und führen Sie die folgenden Schritte aus:

    | NAME | Quellattribut | Namespace |
    | ---------------| --------------- | ----------- |
    | username | user.userprincipalname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | | |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie den **Namespace**-Wert ein.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

7. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML**-Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

8. Kopieren Sie im Abschnitt **Tableau Server einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-tableau-server-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Tableau Server

1. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem Tableau Server-Mandanten anmelden.

2. Wählen Sie auf der Registerkarte **CONFIGURATION (KONFIGURATION)** **User Identity & Access (Benutzeridentität und Zugriff)** aus, und wählen Sie dann die Registerkarte **Authentication Method (Authentifizierungsmethode)** aus.

    ![Configure single sign-on](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. Führen Sie auf der Seite **CONFIGURATION (KONFIGURATION)** die folgenden Schritte aus:

    ![Configure single sign-on](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. Wählen Sie als **Authentication Method (Authentifizierungsmethode)** die Option „SAML“ aus.

    b. Aktivieren Sie das Kontrollkästchen **Enable SAML Authentication for the server (SAML-Authentifizierung für den Server aktivieren)** .

    c. „Tableau Server return URL“: Die URL, auf die Tableau Server-Benutzer zugreifen, z. B. <http://tableau_server>. Ein Verwenden von `http://localhost` ist nicht zu empfehlen. Die Verwendung einer URL mit einem nachstehenden Schrägstrich (z.B. `http://tableau_server/`) wird nicht unterstützt. Kopieren Sie den Wert im Feld **Tableau Server return URL** (Tableau Server-Rückgabe-URL), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ins Textfeld **Anmelde-URL** ein.

    d. „SAML entity ID“: Die Entitäts-ID zur eindeutigen Identifizierung Ihrer Tableau Server-Installation durch den IdP. Sie können in dieses Feld erneut Ihre Tableau Server-URL eingeben, es muss jedoch nicht die Tableau Server-URL verwendet werden. Kopieren Sie den Wert im Feld **SAML entity ID** (SAML-Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ins Textfeld **Bezeichner** ein.

    e. Klicken Sie auf **Download XML Metadata File (XML-Metadatendatei herunterladen)** , und öffnen Sie die Datei im Text-Editor. Suchen Sie nach „Assertion Consumer Service URL“ mit HTTP Post und Index 0, und kopieren Sie die URL. Fügen Sie sie im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ins Textfeld **Reply URL** ein.

    f. Suchen Sie nach der Datei mit Ihren Verbundmetadaten, die Sie aus dem Azure-Portal heruntergeladen haben, und laden Sie sie in die **SAML Idp metadata file** hoch.

    g. Geben Sie die Namen für die Attribute ein, die der IdP verwendet, um Benutzernamen, Anzeigenamen und E-Mail-Adressen aufzunehmen.

    h. Klicken Sie unten auf der Seite auf **Speichern**.

    > [!NOTE]
    > Kunden müssen ein Zertifikat in die Tableau Server SAML-SSO-Konfiguration hochladen, das im SSO-Fluss ignoriert wird. Wenn Sie Hilfe bei der Konfiguration von SAML für Tableau Server benötigen, finden Sie weitere Informationen im Artikel [Konfigurieren von SAML](https://help.tableau.com/current/server/en-gb/saml_config_steps_tsm_ui.htm).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Tableau Server gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **Tableau Server** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Tableau Server**.

    ![Tableau Server-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-tableau-server-test-user"></a>Erstellen eines Tableau Server-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Tableau Server. Sie müssen alle Benutzer in Tableau Server bereitstellen.

Der username-Wert des Benutzers muss dem Wert entsprechen, den Sie im benutzerdefinierten Attribut **username** in Azure AD konfiguriert haben. Bei ordnungsgemäßer Zuordnung sollte die Integration funktionieren: Konfigurieren des einmaligen Anmeldens in Azure AD.

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an den Tableau Server-Administrator in Ihrer Organisation.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Server“ klicken, sollten Sie automatisch bei der Tableau Server-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

