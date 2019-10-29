---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit NetSuite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und NetSuite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6d578b5d08fecde733bb7b257057e480fef83c4e
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754414"
---
# <a name="tutorial-integrate-azure-ad-single-sign-on-sso-with-netsuite"></a>Tutorial: Integrieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD mit NetSuite

In diesem Tutorial erfahren Sie, wie Sie NetSuite in Azure Active Directory (Azure AD) integrieren. Die Integration von NetSuite in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf NetSuite hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei NetSuite anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein NetSuite-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. 

NetSuite unterstützt Folgendes:

* IDP-initiiertes einmaliges Anmelden
* JIT-Benutzerbereitstellung (Just-In-Time)
* [Automatisierte Benutzerbereitstellung](NetSuite-provisioning-tutorial.md)

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="add-netsuite-from-the-gallery"></a>Hinzufügen von NetSuite aus dem Katalog

Fügen Sie zum Konfigurieren der Integration von NetSuite in Azure AD NetSuite aus dem Katalog wie folgt der Liste mit den verwalteten SaaS-Apps hinzu:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Bereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **NetSuite** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **NetSuite** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netsuite"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für NetSuite

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit NetSuite mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in NetSuite eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit NetSuite die folgenden Schritte aus:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen

    a. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit dem Benutzer B. Simon zu testen  
    b. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
1. [Konfigurieren des einmaligen Anmeldens für NetSuite](#configure-netsuite-sso), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * [Erstellen eines NetSuite-Testbenutzers](#create-the-netsuite-test-user), um ein Pendant von B. Simon in NetSuite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. [Testen Sie das einmalige Anmelden](#test-sso), um zu überprüfen, ob die Konfiguration funktioniert.

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Führen Sie die folgenden Schritte aus, um einmaliges Anmelden von Azure AD im Azure-Portal zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **NetSuite** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie im Bereich **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie im Bereich **Einmaliges Anmelden (SSO) mit SAML einrichten** neben **Grundlegende SAML-Konfiguration** das Symbol **Bearbeiten** (Stift) aus.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** im Textfeld **Antwort-URL** eine URL in einem der folgenden Formate ein:

    ```
    https://<tenant-name>.NetSuite.com/saml2/acs
    https://<tenant-name>.na1.NetSuite.com/saml2/acs
    https://<tenant-name>.na2.NetSuite.com/saml2/acs
    https://<tenant-name>.sandbox.NetSuite.com/saml2/acs
    https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs
    https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs
    ```

    > [!NOTE]
    > Bei den Werten in den vorangehenden URLs handelt es sich um Beispielwerte. Ersetzen Sie sie durch die tatsächliche Antwort-URL. Den Wert erhalten Sie vom [Supportteam für den NetSuite-Client](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml). Sie können sich auch die Formate im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

    Die NetSuite-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Sie müssen Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. 
    
1. Wählen Sie das Symbol **Bearbeiten** (Stift) aus, um den Bereich **Benutzerattribute** zu öffnen: Im Bereich wird eine Liste der Standardattribute angezeigt, wie in der folgenden Abbildung dargestellt: 

    ![Bereich „Benutzerattribute“](common/edit-attribute.png)

    Neben diesen Attributen werden von der NetSuite-Anwendung noch einige weitere Attribute in der SAML-Antwort erwartet. 

1. Führen Sie im Bereich **Benutzerattribute** unter **Benutzeransprüche** die folgenden Schritte aus, um das in der folgenden Tabelle gezeigte SAML-Tokenattribut hinzuzufügen:

    | NAME | Quellattribut | 
    | ---------------| --------------- |
    | account  | `account id` |

    a. Wählen Sie **Neuen Anspruch hinzufügen** aus, um den Bereich **Benutzeransprüche verwalten** zu öffnen.

    b. Geben Sie im Feld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie das Feld **Namespace** leer.

    d. Wählen Sie in der Dropdownliste **Quelle** den Eintrag **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Wählen Sie **Speichern** aus.

    >[!NOTE]
    >Bei dem Wert des Kontoattributs handelt es sich nicht um einen echten Wert. Sie aktualisieren diesen Wert. Dies wird später in diesem Tutorial beschrieben.

1. Suchen Sie im Bereich **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** nach **Verbundmetadaten-XML**.

1. Wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **NetSuite einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer namens B.Simon.

1. Wählen Sie im Azure-Portal im linken Bereich **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.

1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

1. Führen Sie im Eigenschaftenbereich für **Benutzer** die folgenden Schritte aus:

   a. Geben Sie im Feld **Name** den Namen **B.Simon** ein.  
   b. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein (z. B. B.Simon@contoso.com).  
   c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.  
   d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf NetSuite gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **NetSuite** aus.
1. Navigieren Sie im Übersichtsbereich zum Abschnitt **Verwalten**, und wählen Sie den Link **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Schaltfläche „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Bereich **Benutzer und Gruppen** in der Dropdownliste **Benutzer** den Eintrag **B.Simon** und anschließend am unteren Bildschirmrand die Schaltfläche **Auswählen** aus.
1. Wenn Sie einen Rollenwert in der SAML-Assertion erwarten, gehen Sie wie folgt vor:

   a. Wählen Sie im Bereich **Rolle auswählen** in der Dropdownliste die entsprechende Rolle für den Benutzer aus.  
   b. Wählen Sie unten auf dem Bildschirm die Schaltfläche **Auswählen** aus.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** die Schaltfläche **Zuweisen** aus.

## <a name="configure-netsuite-sso"></a>Konfigurieren des einmaligen Anmeldens für NetSuite

1. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrer NetSuite-Unternehmenswebsite als Administrator an.

2. Wählen Sie auf der oberen Navigationsleiste **Setup** und anschließend **Company** > **Enable Features** (Unternehmen > Features aktivieren) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Wählen Sie auf der Symbolleiste in der Mitte der Seite **SuiteCloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-suitecloud.png)

4. Aktivieren Sie unter **Manage Authentication** (Authentifizierung verwalten) das Kontrollkästchen **SAML Single Sign-on** (Einmaliges Anmelden für SAML), um die Option für SAML-SSO in NetSuite zu aktivieren.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Wählen Sie auf der oberen Navigationsleiste die Option **Setup** aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-setup.png)

6. Wählen Sie in der Liste **Setup Tasks** (Setupaufgaben) die Option **Integration** aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-integration.png)

7. Wählen Sie unter **Manage Authentication** (Authentifizierung verwalten) die Option **SAML Single Sign-on** (Einmaliges Anmelden für SAML) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-saml.png)

8. Führen Sie im Bereich **SAML Setup** (SAML-Setup) unter **NetSuite-Konfiguration** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Aktivieren Sie das Kontrollkästchen **Primary Authentication Method** (Primäre Authentifizierungsmethode).

    b. Wählen Sie unter **SAMLV2 Identity Provider Metadata** (SAMLV2-Identitätsanbietermetadaten) die Option **Upload IDP Metadata File** (IDP-Metadatendatei hochladen) und dann **Browse** (Durchsuchen) aus, um die aus dem Azure-Portal heruntergeladene Metadatendatei hochzuladen.

    c. Klicken Sie auf **Submit** (Senden).

9. Wählen Sie auf der oberen NetSuite-Navigationsleiste **Setup** und anschließend **Company** > **Company Information** (Unternehmensinformationen) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-com.png)

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-account-id.png)

    b. Kopieren Sie im Bereich **Company Information** (Unternehmensinformationen) in der rechten Spalte den Wert für **Account ID** (Konto-ID).

    c. Fügen Sie den Wert für **Account ID** (Konto-ID), den Sie im NetSuite-Konto kopiert haben, in das Feld **Attributwert** in Azure AD ein. 

10. Bevor Benutzer die einmalige Anmeldung in NetSuite ausführen können, müssen ihnen zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen werden. Gehen Sie zum Zuweisen dieser Berechtigungen wie folgt vor:

    a. Wählen Sie auf der oberen Navigationsleiste die Option **Setup** aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-setup.png)

    b. Wählen Sie im linken Bereich **Users/Roles** (Benutzer/Rollen) und dann **Manage Roles** (Rollen verwalten) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Wählen Sie **New Role** (Neue Rolle) aus.

    d. Geben Sie unter **Name** einen Namen für die neue Rolle ein.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-new-role.png)

    e. Wählen Sie **Speichern** aus.

    f. Wählen Sie auf der oberen Navigationsleiste die Option **Permissions** (Berechtigungen) aus. Wählen Sie anschließend **Setup** aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-sso.png)

    g. Wählen Sie **SAML Single Sign-on** (Einmaliges Anmelden für SAML) und dann **Add** (Hinzufügen) aus.

    h. Wählen Sie **Speichern** aus.

    i. Wählen Sie auf der oberen Navigationsleiste **Setup** und anschließend **Setup Manager** (Setup-Manager) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-setup.png)

    j. Wählen Sie im linken Bereich **Users/Roles** (Benutzer/Rollen) und dann **Manage Users** (Benutzer verwalten) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Wählen Sie einen Testbenutzer aus. Wählen Sie dann **Edit** (Bearbeiten) und die Registerkarte **Access** (Zugriff) aus.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-edit-user.png)

    l. Weisen Sie im Bereich **Roles** (Rollen) die entsprechende Rolle zu, die Sie erstellt haben.

    ![Einmaliges Anmelden konfigurieren](./media/NetSuite-tutorial/ns-add-role.png)

    m. Wählen Sie **Speichern** aus.

### <a name="create-the-netsuite-test-user"></a>Erstellen eines NetSuite-Testbenutzers

In diesem Abschnitt wird in NetSuite ein Benutzer namens B. Simon erstellt. NetSuite unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in NetSuite vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „NetSuite“ auswählen, sollten Sie automatisch bei der NetSuite-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [NetSuite mit Azure AD ausprobieren](https://aad.portal.azure.com/)

