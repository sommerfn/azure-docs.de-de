---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit PureCloud by Genesys | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und PureCloud by Genesys konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 779328f4c21afb4392663e6f8840749ea505c529
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73242439"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-purecloud-by-genesys"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit PureCloud by Genesys

In diesem Tutorial erfahren Sie, wie Sie PureCloud by Genesys in Azure Active Directory (Azure AD) integrieren. Anschließend haben Sie folgende Möglichkeiten:

* Steuern Sie mithilfe von Azure AD, welche Benutzer auf PureCloud by Genesys zugreifen können.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei PureCloud by Genesys anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie nicht über ein Abonnement dieser Art verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* PureCloud by Genesys-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* PureCloud by Genesys unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

> [!NOTE]
> Die ID dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Hinzufügen von PureCloud by Genesys aus dem Katalog

Zum Konfigurieren der Integration von PureCloud by Genesys in Azure AD müssen Sie PureCloud by Genesys aus dem Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen. Gehen Sie dazu folgendermaßen vor:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **PureCloud by Genesys** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **PureCloud by Genesys** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-purecloud-by-genesys"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für PureCloud by Genesys

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit PureCloud by Genesys mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in PureCloud by Genesys eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit PureCloud by Genesys die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für PureCloud by Genesys](#configure-purecloud-by-genesys-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines PureCloud by Genesys-Testbenutzers](#create-purecloud-by-genesys-test-user)** , um eine Entsprechung für B. Simon in PureCloud by Genesys zu erhalten, die mit ihrer Azure AD-Darstellung verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Führen Sie die folgenden Schritte aus, um einmaliges Anmelden von Azure AD im Azure-Portal zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **PureCloud by Genesys** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    a. Geben Sie im Feld **Bezeichner** eine URL ein, die Ihrer Region entspricht:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. Geben Sie im Feld **Antwort-URL** eine URL ein, die Ihrer Region entspricht:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml`|

1. Wählen Sie **Zusätzliche URLs festlegen** aus, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten:

    Geben Sie im Feld **Anmelde-URL** eine URL ein, die Ihrer Region entspricht:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

1. Die PureCloud by Genesys-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute:

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von der PureCloud by Genesys-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden, wie in der folgenden Tabelle gezeigt. Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.

    | NAME | Quellattribut|
    | ---------------| --------------- |
    | Email | user.userprinicipalname |
    | OrganizationName | `Your organization name` |

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zum Eintrag **Zertifikat (Base64)** . Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **PureCloud by Genesys einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon:

1. Wählen Sie im Azure-Portal im linken Bereich **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** den Benutzernamen im folgenden Format ein: username@companydomain.extension. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie den Wert im Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt richten Sie B. Simon für die Verwendung des einmaligen Anmeldens von Azure ein, indem Sie ihr Zugriff auf PureCloud by Genesys gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **PureCloud by Genesys** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie die Schaltfläche **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B.Simon** und anschließend am unteren Bildschirmrand die Schaltfläche **Auswählen** aus.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste und dann im unteren Bildschirmbereich die Schaltfläche **Auswählen** aus.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.

## <a name="configure-purecloud-by-genesys-sso"></a>Konfigurieren des einmaligen Anmeldens für PureCloud by Genesys

1. Melden Sie sich in einem anderen Webbrowserfenster bei PureCloud by Genesys als Administrator an.

1. Wählen Sie oben **Admin** aus, und navigieren Sie unter **Integrations** (Integrationen) zu **Single Sign-on** (Einmaliges Anmelden).

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure01.png)

1. Wechseln Sie zur Registerkarte **ADFS/Azure AD (Premium)** , und führen Sie die folgenden Schritte aus:

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Wählen Sie **Browse** (Durchsuchen) aus, um das aus dem Azure-Portal heruntergeladene Base-64-codierte Zertifikat unter **ADFS Certificate** (ADFS-Zertifikat) hochzuladen.

    b. Fügen Sie in das Feld **ADFS Issuer URI** (ADFS-Aussteller-URI) den Wert von **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Feld **Target URI** (Ziel-URI) den Wert von **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Wechseln Sie für den Wert **Relying Party Identifier** (Bezeichner der vertrauenden Seite) zum Azure-Portal. Wählen Sie dann auf der Anwendungsintegrationsseite für **PureCloud by Genesys** die Registerkarte **Eigenschaften** aus, und kopieren Sie den Wert **Anwendungs-ID**. Fügen Sie ihn in das Feld **Relying Party Identifier** (Bezeichner der vertrauenden Seite) ein.

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. Wählen Sie **Speichern** aus.

### <a name="create-purecloud-by-genesys-test-user"></a>Erstellen eines PureCloud by Genesys-Testbenutzers

Damit sich Azure AD-Benutzer bei PureCloud by Genesys anmelden können, müssen sie in PureCloud by Genesys bereitgestellt werden. In PureCloud by Genesys ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich als Administrator bei PureCloud by Genesys an.

1. Wählen Sie oben **Admin** aus, und navigieren Sie unter **People & Permissions** (Personen und Berechtigungen) zu **People** (Personen).

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure03.png)

1. Wählen Sie auf der Seite **People** (Personen) die Option **Add Person** (Person hinzufügen) aus.

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure04.png)

1. Führen Sie im Dialogfeld **Add People to the Organization** (Personen der Organisation hinzufügen) die folgenden Schritte aus:

    ![Configure single sign-on](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. Geben Sie im Feld **Full Name** (Vollständiger Name) den Namen eines Benutzers ein. Beispiel:  **B.simon**

    b. Geben Sie im Feld **Email** (E-Mail) die E-Mail-Adresse des Benutzers ein. Beispiel: **b.simon\@contoso.com**

    c. Klicken Sie auf **Erstellen**.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel **PureCloud by Genesys** auswählen, sollten Sie automatisch bei dem PureCloud by Genesys-Konto angemeldet werden, für das Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist bedingter Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [PureCloud by Genesys mit Azure AD ausprobieren](https://aad.portal.azure.com/)