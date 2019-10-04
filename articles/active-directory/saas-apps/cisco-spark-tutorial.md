---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Cisco Webex | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cisco Webex konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 503de723894388a198abbb687221cb1403a6fa84
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104426"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cisco-webex"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Cisco Webex

In diesem Tutorial erfahren Sie, wie Sie Cisco Webex in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Cisco Webex in Azure AD haben Sie folgende Möglichkeiten:

* Sie können in Azure AD steuern, wer auf Cisco Webex Zugriff hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Cisco Webex anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Cisco Webex-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Cisco Webex unterstützt **SP-initiiertes** einmaliges Anmelden sowie die **automatisierte** Benutzerbereitstellung.

## <a name="adding-cisco-webex-from-the-gallery"></a>Hinzufügen von Cisco Webex aus dem Katalog

Zum Konfigurieren der Integration von Cisco Webex in Azure AD müssen Sie Cisco Webex aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Cisco Webex** in das Suchfeld ein.
1. Wählen Sie **Cisco Webex** im Ergebnisbereich aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cisco-webex"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Cisco Webex

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Cisco Webex mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cisco Webex eingerichtet werden.

Sie müssen die folgenden Aufgaben ausführen, um das einmalige Anmelden von Azure AD bei Cisco Webex zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
2. **[Konfigurieren von Cisco Webex](#configure-cisco-webex)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Cisco Webex-Testbenutzers](#create-cisco-webex-test-user)** , um ein Pendant zu B. Simon in Cisco Webex zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
3. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Cisco Webex** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Laden Sie im Abschnitt **Grundlegende SAML-Konfiguration** die heruntergeladene **Metadatendatei des Dienstanbieters** herunter, und konfigurieren Sie die Anwendung folgendermaßen:

    >[!Note]
    >Die Dienstanbieter-Metadatendatei rufen Sie später im Tutorial im Abschnitt **Konfigurieren von Cisco Webex** ab. 

    a. Klicken Sie auf **Metadatendatei hochladen**.

    b. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.

    c. Nachdem Sie die Metadatendatei des Dienstanbieters hochgeladen haben, werden im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für **Bezeichner** und **Antwort-URL** automatisch ausgefüllt, wie unten dargestellt:

    Fügen Sie im Textfeld **Anmelde-URL** den Wert von **Antwort-URL** ein, der automatisch aus der hochgeladenen SP-Metadatendatei ausgefüllt wird.

5. Die Cisco Webex-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen.

    ![image](common/edit-attribute.png)

6. Darüber hinaus wird von der Cisco Webex-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:
    
    | NAME |  Quellattribut|
    | ---------------|--------- |
    | uid | user.userprincipalname |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

   ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Cisco Webex einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cisco Webex gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Cisco Webex**aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann am unteren Bildschirmrand auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-cisco-webex"></a>Konfigurieren von Cisco Webex

1. Melden Sie sich beim [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) mit vollständigen Administratorberechtigungen an.

2. Wählen **Settings** (Einstellungen) aus, und klicken Sie im Bereich **Authentication** (Authentifizierung) auf **Modify** (Ändern).

    ![Configure single sign-on](./media/cisco-spark-tutorial/tutorial-cisco-spark-10.png)
  
3. Wählen Sie **Integrate a 3rd-party identity provider. (Advanced)** (Identitätsanbieter von Drittanbieter integrieren (Erweitert)) aus, und wechseln Sie zum nächsten Bildschirm.

4. Laden Sie auf der Seite **Import Idp Metadata** (IdP-Metadaten importieren) die Azure AD-Metadatendatei hoch, indem Sie diese entweder per Drag & Drop auf die Seite ziehen oder sie mithilfe der Option zum Durchsuchen auswählen. Wählen Sie anschließend **Require certificate signed by a certificate authority in Metadata (more secure)** (Signatur einer Zertifizierungsstelle in den Metadaten erzwingen (sicherer)) aus, und klicken Sie auf **Weiter**.

    ![Configure single sign-on](./media/cisco-spark-tutorial/tutorial-cisco-spark-11.png)

5. Wählen Sie **Test SSO Connection** (SSO-Verbindung testen) aus, und authentifizieren Sie sich, wenn eine neue Browserregisterkarte geöffnet wird, durch Ihre Anmeldung bei Azure AD.

6. Wechseln Sie zurück zur Browserregisterkarte **Cisco Cloud Collaboration Management**. Wenn der Test erfolgreich war, wählen Sie **This test was successful. Enable Single Sign-On option** (Der Test war erfolgreich. Single Sign-On aktivieren), und klicken Sie auf **Weiter**.

### <a name="create-cisco-webex-test-user"></a>Erstellen eines Cisco Webex-Testbenutzers

In diesem Abschnitt erstellen Sie in Cisco Webex einen Benutzer mit dem Namen B. Simon. In diesem Abschnitt erstellen Sie in Cisco Webex einen Benutzer mit dem Namen B. Simon.

1. Wechseln Sie zum [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/), und verwenden Sie dabei vollständige Administratorberechtigungen.

2. Klicken Sie auf **Users** und dann auf **Manage Users**.
   
    ![Configure single sign-on](./media/cisco-spark-tutorial/tutorial-cisco-spark-12.png) 

3. Wählen Sie im Fenster **Manage User** (Benutzer verwalten) die Option **Manually add or modify users** (Benutzer manuell hinzufügen oder ändern) aus, und klicken Sie auf **Weiter**.

4. Wählen Sie **Names and Email address** (Namen und E-Mail-Adressen) aus. Füllen Sie das Textfeld anschließend wie folgt aus:

    ![Configure single sign-on](./media/cisco-spark-tutorial/tutorial-cisco-spark-13.png) 

    a. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein (beispielsweise **B**).

    b. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein (beispielsweise **Simon**).

    c. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. b.simon@contoso.com, ein.

5. Klicken Sie auf das Pluszeichen, um B. Simon hinzuzufügen. Klicken Sie auf **Weiter**.

6. Klicken Sie im Fenster **Add Services for Users** (Dienste für Benutzer hinzufügen) auf **Speichern** und dann auf **Fertig stellen**.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich auf die Kachel „Cisco Webex“ klicken, sollten Sie automatisch bei der Cisco Webex-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Cisco Webex mit Azure AD ausprobieren](https://aad.portal.azure.com)