---
title: 'Tutorial: Azure Active Directory-Integration mit AirWatch | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und AirWatch konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f0a684c6a38ba3a95438941f668b36b23d278df
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227718"
---
# <a name="tutorial-integrate-airwatch-with-azure-active-directory"></a>Tutorial: Integrieren von AirWatch in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie AirWatch in Azure Active Directory (Azure AD) integrieren. Die Integration von AirWatch in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf AirWatch hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei AirWatch anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie kein Abonnement besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige kostenlose Testversion erhalten.
* Ein AirWatch-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. AirWatch unterstützt **SP-initiiertes** einmaliges Anmelden.

## <a name="adding-airwatch-from-the-gallery"></a>Hinzufügen von AirWatch aus dem Katalog

Zum Konfigurieren der Integration von AirWatch in Azure AD müssen Sie AirWatch aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **AirWatch** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **AirWatch** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit AirWatch mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in AirWatch eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit AirWatch müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für AirWatch](#configure-airwatch-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines AirWatch-Testbenutzers](#create-airwatch-test-user)** , um eine Entsprechung von Britta Simon in AirWatch zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
5. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **AirWatch** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie auf der Seite **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`.

    1. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** folgenden Wert ein: `AirWatch`.

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von AirWatch](https://www.air-watch.com/company/contact-us/), um diesen Wert zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die AirWatch-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf die Schaltfläche **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](common/edit-attribute.png)

1. Bearbeiten Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die Ansprüche mithilfe des Symbols zum **Bearbeiten**, oder fügen Sie die Ansprüche über **Neuen Anspruch hinzufügen** hinzu, um das SAML-Tokenattribut wie in der obigen Abbildung gezeigt zu konfigurieren. Führen Sie dann die folgenden Schritte aus:

    | NAME |  Quellattribut|
    |---------------|----------------|
    | UID | user.userprincipalname |
    | | |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um die Metadaten-XML herunterzuladen und auf Ihrem Computer zu speichern.

   ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **AirWatch einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

   ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-airwatch-sso"></a>Konfigurieren des einmaligen Anmeldens für AirWatch

1. Melden Sie sich in einem anderen Webbrowserfenster bei der AirWatch-Unternehmenswebsite als Administrator an.

1. Auf der Einstellungsseite: Wählen Sie **Settings > Enterprise Integration > Directory Services** (Einstellungen > Unternehmensintegration > Verzeichnisdienste) aus.

   ![Einstellungen](./media/airwatch-tutorial/ic791921.png "Einstellungen")

1. Klicken Sie auf die Registerkarte **Benutzer**, geben Sie im Textfeld **Basis-DN** Ihren Domänennamen ein, und klicken Sie dann auf **Speichern**.

   ![Benutzer](./media/airwatch-tutorial/ic791922.png "Benutzer")

1. Klicken Sie auf die Registerkarte **Server** .

   ![Server](./media/airwatch-tutorial/ic791923.png "Server")

1. Führen Sie im Abschnitt für **LDAP** die folgenden Schritte aus:

    ![Hochladen](./media/airwatch-tutorial/ic791924.png "LDAP")   

    a. Wählen Sie unter **Verzeichnistyp** die Option **Keiner** aus.

    b. Aktivieren Sie **Use SAML For Authentication**.

1. Klicken Sie im Abschnitt **SAML 2.0** auf **Upload** (Hochladen), um das heruntergeladene Zertifikat hochzuladen.

    ![Hochladen](./media/airwatch-tutorial/ic791932.png "Hochladen")

1. Führen Sie im Abschnitt **Request** die folgenden Schritte aus:

    ![Anforderung](./media/airwatch-tutorial/ic791925.png "Anforderung")  

    a. Wählen Sie als **Anforderungsbindungstyp** die Option **POST** aus.

    b. Kopieren Sie im Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für AirWatch** den Wert für **Anmelde-URL**, und fügen Sie ihn in das Textfeld **Identity Provider Single Sign On URL** (SSO-URL des Identitätsanbieters) ein.

    c. Wählen Sie als **NameID-Format** die Option **E-Mail-Adresse** aus.

    d. Wählen Sie unter **Authentication Request Security** (Sicherheit der Authentifizierungsanforderung) die Option **None** (Keine) aus.

    e. Klicken Sie auf **Speichern**.

1. Klicken Sie erneut auf die Registerkarte **User** .

    ![Benutzer](./media/airwatch-tutorial/ic791926.png "Benutzer")

1. Führen Sie im Abschnitt **Attribute** die folgenden Schritte aus:

    ![Attribut](./media/airwatch-tutorial/ic791927.png "Attribut")

    a. Geben Sie im Textfeld **Objektbezeichner** Folgendes ein: `http://schemas.microsoft.com/identity/claims/objectidentifier`.

    b. Geben Sie im Textfeld **Benutzername** Folgendes ein: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    c. Geben Sie im Textfeld **Anzeigename** Folgendes ein: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    d. Geben Sie im Textfeld **Vorname** Folgendes ein: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    e. Geben Sie im Textfeld **Nachname** Folgendes ein: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    f. Geben Sie in das Textfeld **E-Mail** die Adresse `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.

    g. Klicken Sie auf **Speichern**.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf AirWatch gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **AirWatch** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-airwatch-test-user"></a>Erstellen eines AirWatch-Testbenutzers

Damit sich Azure AD-Benutzer bei AirWatch anmelden können, müssen sie in AirWatch bereitgestellt werden. Im Fall von AirWatch ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der **AirWatch**-Unternehmenswebsite als Administrator an.

2. Klicken Sie im Navigationsbereich links auf **Konten**, und klicken Sie dann auf **Benutzer**.
  
   ![Benutzer](./media/airwatch-tutorial/ic791929.png "Benutzer")

3. Klicken Sie im Menü **Users** (Benutzer) auf **List View** (Listenansicht), und klicken Sie dann auf **Add > Add User** (Hinzufügen > Benutzer hinzufügen).
  
   ![Benutzer hinzufügen](./media/airwatch-tutorial/ic791930.png "Benutzer hinzufügen")

4. Führen Sie im Dialogfeld **Add / Edit User** die folgenden Schritte aus:

   ![Benutzer hinzufügen](./media/airwatch-tutorial/ic791931.png "Benutzer hinzufügen")

   a. Geben Sie in die Textfelder **Benutzername**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname** und **E-Mail-Adresse** die Informationen eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.

   b. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von AirWatch-Benutzerkonten oder mithilfe der von AirWatch bereitgestellten APIs erstellen.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „AirWatch“ auswählen, sollten Sie automatisch bei der AirWatch-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
