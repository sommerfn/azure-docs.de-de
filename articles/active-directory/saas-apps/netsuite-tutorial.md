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
ms.openlocfilehash: 1a9b834feacd6241e66e18a4f4e6aadc43e909c7
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438495"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netsuite"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit NetSuite

In diesem Tutorial erfahren Sie, wie Sie NetSuite in Azure Active Directory (Azure AD) integrieren. Die Integration von NetSuite in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf NetSuite hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei NetSuite anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein NetSuite-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* NetSuite unterstützt **IDP-initiiertes** einmaliges Anmelden.

* NetSuite unterstützt die **Just-in-Time**-Benutzerbereitstellung.

* NetSuite unterstützt die [automatisierte Benutzerbereitstellung](NetSuite-provisioning-tutorial.md).

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-netsuite-from-the-gallery"></a>Hinzufügen von NetSuite aus dem Katalog

Zum Konfigurieren der NetSuite-Integration in Azure AD müssen Sie NetSuite aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **NetSuite** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **NetSuite** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netsuite"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für NetSuite

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit NetSuite mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in NetSuite eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit NetSuite die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für NetSuite](#configure-netsuite-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines NetSuite-Testbenutzers](#create-netsuite-test-user)** , um ein Pendant von B. Simon in NetSuite zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **NetSuite** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein:

    | |
    |--|
    | `https://<tenant-name>.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs` |

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Wenden Sie sich an das [Kundensupportteam von NetSuite](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml), um den Wert zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Die NetSuite-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen.

    ![image](common/edit-attribute.png)

1. Darüber hinaus wird von der NetSuite-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld „Benutzerattribute“ im Abschnitt „Benutzeransprüche“ die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:

    | NAME | Quellattribut | 
    | ---------------| --------------- |
    | account  | `account id` |

    1. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    1. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    1. Lassen Sie den **Namespace** leer.

    1. Wählen Sie „Source“ als **Attribut** aus.

    1. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    1. Klicken Sie auf **OK**.

    1. Klicken Sie auf **Speichern**.

    >[!NOTE]
    >Bei dem Wert des Kontoattributs handelt es sich nicht um einen echten Wert. Sie werden diesen Wert aktualisieren. Die zugehörige Beschreibung folgt später in diesem Tutorial.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **NetSuite einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf NetSuite gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **NetSuite** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-netsuite-sso"></a>Konfigurieren des einmaligen Anmeldens für NetSuite

1. Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich bei Ihrer NetSuite-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup**, navigieren Sie zu **Unternehmen**, und klicken Sie auf **Features aktivieren**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Klicken Sie auf der Symbolleiste in der Mitte der Seite auf **SuiteCloud**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-suitecloud.png)

4. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmaliges Anmelden für SAML**, um die Option „Einmaliges Anmelden für SAML“ in NetSuite zu aktivieren.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Klicken Sie auf der Symbolleiste am oberen Rand der Seite auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

6. Klicken Sie in der Liste **SETUPTASKS** auf **Integration**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-integration.png)

7. Klicken Sie im Abschnitt **Authentifizierung verwalten** auf **Einmaliges Anmelden für SAML**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-saml.png)

8. Führen Sie auf der Seite **SAML-Setup** unter **NetSuite-Konfiguration** die folgenden Schritte aus:

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Wählen Sie **Primäre Authentifizierungsmethode** aus.

    b. Wählen Sie im Feld mit der Bezeichnung **SAMLV2-IDENTITÄTSANBIETERMETADATEN** die Option **IDP-METADATENDATEI HOCHLADEN** aus. Klicken Sie dann zum Hochladen der Metadatendatei, die Sie aus dem Azure-Portal heruntergeladen haben, auf **Durchsuchen** .

    c. Klicken Sie auf **Submit**.

9. Klicken Sie in NetSuite auf **Setup**, navigieren Sie zu **Unternehmen**, und klicken Sie im oberen Navigationsmenü auf **Unternehmensinformationen**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-com.png)

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-account-id.png)

    b. Kopieren Sie auf der Seite **Unternehmensinformationen** die **Konto-ID** in der rechten Spalte.

    c. Fügen Sie die **Konto-ID**, die Sie im NetSuite-Konto kopiert haben, in das Feld **Attributwert** in Azure AD ein. 

10. Bevor Benutzer die einmalige Anmeldung in NetSuite ausführen können, müssen ihnen zunächst die entsprechenden Berechtigungen in NetSuite zugewiesen werden. Befolgen Sie die folgenden Anweisungen, um diese Berechtigungen zuzuweisen.

    a. Klicken Sie im oberen Navigationsmenü auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

    b. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Rollen verwalten**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. Klicken Sie auf **Neue Rolle**.

    d. Geben Sie einen **Namen** für die neue Rolle ein.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-new-role.png)

    e. Klicken Sie auf **Speichern**.

    f. Klicken Sie im oberen Menü auf **Berechtigungen**. Klicken Sie dann auf **Setup**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-sso.png)

    g. Wählen Sie **Einmaliges Anmelden für SAML** aus, und klicken Sie dann auf **Hinzufügen**.

    h. Klicken Sie auf **Speichern**.

    i. Klicken Sie im oberen Navigationsmenü auf **Setup** und dann auf **Setup-Manager**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-setup.png)

    j. Wählen Sie im linken Navigationsmenü **Benutzer/Rollen** aus, und klicken Sie auf **Benutzer verwalten**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Wählen Sie einen Testbenutzer. Klicken Sie auf **Bearbeiten**, und navigieren Sie anschließend zur Registerkarte **Zugriff**.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-edit-user.png)

    l. Weisen Sie im Dialogfeld „Rollen“ die entsprechende Rolle zu, die Sie erstellt haben.

    ![Configure single sign-on](./media/NetSuite-tutorial/ns-add-role.png)

    m. Klicken Sie auf **Speichern**.

### <a name="create-netsuite-test-user"></a>Erstellen eines NetSuite-Testbenutzers

In diesem Abschnitt wird in NetSuite eine Benutzerin namens Britta Simon erstellt. NetSuite unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in NetSuite vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „NetSuite“ klicken, sollten Sie automatisch bei Ihrer NetSuite-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [NetSuite mit Azure AD ausprobieren](https://aad.portal.azure.com/)

