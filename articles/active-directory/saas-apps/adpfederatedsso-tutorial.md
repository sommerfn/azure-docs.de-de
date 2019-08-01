---
title: 'Tutorial: Azure Active Directory-Integration mit ADP | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und ADP konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/25/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1597a4ca9cac7ba3885e863502f156d4c83aeed1
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516394"
---
# <a name="tutorial-integrate-adp-with-azure-active-directory"></a>Tutorial: Integrieren von ADP in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie ADP in Azure Active Directory (Azure AD) integrieren. Die Integration von ADP in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf ADP hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei ADP anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* ADP-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* ADP unterstützt **IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-adp-from-the-gallery"></a>Hinzufügen von ADP über den Katalog

Zum Konfigurieren der Integration von ADP in Azure AD müssen Sie ADP aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **ADP** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **ADP** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit ADP mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ADP eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit ADP müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für ADP](#configure-adp-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
5. **[Erstellen eines ADP-Testbenutzers](#create-adp-test-user)** , um in ADP eine Entsprechung von B. Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ADP** auf die Registerkarte **Eigenschaften**, und führen Sie die folgenden Schritte aus: 

    ![Eigenschaften für das einmalige Anmelden](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Legen Sie den Wert im Feld **Aktiviert für die Benutzeranmeldung** auf **Ja** fest.

    b. Kopieren Sie die **URL für den Benutzerzugriff**. Diese muss im Abschnitt **Anmelde-URL konfigurieren** eingefügt werden, dies wird später in diesem Tutorial erläutert.

    c. Legen Sie das Feld **Benutzerzuweisung erforderlich** auf **Ja** fest.

    d. Legen Sie das Feld **Für Benutzer sichtbar** auf **Nein** fest.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **ADP** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL ein: `https://fed.adp.com`.

5. Die ADP-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen. Der Anspruchsname ist immer eine Kombination aus **PersonImmutableID** und dem Wert, der **employeeid** zugeordnet werden soll.

    Die Benutzerzuordnung von Azure AD zu ADP erfolgt auf Grundlage der **employeeid**, aber je nach Anwendungseinstellungen ist auch eine Zuordnung zu einem anderen Wert möglich. Wenden Sie sich also zunächst an das [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx), um den korrekten Bezeichner eines Benutzers zu verwenden, und ordnen Sie diesen Wert dann dem Anspruch **PersonImmutableID** zu.

    ![image](common/edit-attribute.png)

6. Darüber hinaus wird von der ADP-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld „Benutzerattribute“ im Abschnitt „Benutzeransprüche“ die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen: 

    | NAME | Quellattribut|
    | ---------------| --------- |
    | PersonImmutableID  | user.employeeid |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **Speichern**.

    > [!NOTE] 
    > Bevor Sie die SAML-Assertion konfigurieren können, müssen Sie sich an Ihr [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx) wenden und für Ihren Mandanten den Wert des Attributs für den eindeutigen Benutzerbezeichner anfordern. Sie benötigen diesen Wert, um den benutzerdefinierten Anspruch für Ihre Anwendung zu konfigurieren. 

4. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

6. Kopieren Sie im Abschnitt **ADP einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-adp-sso"></a>Konfigurieren des einmaligen Anmeldens für ADP

Um das einmalige Anmelden für **ADP** zu konfigurieren, müssen Sie die heruntergeladene **Metadaten-XML** auf die [ADP-Website](https://adpfedsso.adp.com/public/login/index.fcc) hochladen.

> [!NOTE]  
> Dieser Vorgang kann einige Tage in Anspruch nehmen.

### <a name="configure-your-adp-services-for-federated-access"></a>Konfigurieren Ihrer ADP-Dienste für den Verbundzugriff

>[!Important]
> Mitarbeiter, die Verbundzugriff auf Ihre ADP-Dienste benötigen, müssen der ADP-Dienst-App zugewiesen und anschließend dem jeweiligen ADP-Dienst erneut zugewiesen werden.
Nach der Bestätigung durch Ihren ADP-Vertriebsbeauftragten konfigurieren Sie Ihre ADP-Dienste und weisen Benutzer zum Steuern des Benutzerzugriffs für die jeweiligen ADP-Dienste zu bzw. verwalten diese.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **ADP** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **ADP** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.
1. Klicken Sie im Azure-Portal auf Ihrer Anwendungsintegrationsseite für **ADP** auf die Registerkarte **Eigenschaften**, und führen Sie die folgenden Schritte aus:  

    ![Eigenschaften für SSO über Link](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Legen Sie den Wert im Feld **Aktiviert für die Benutzeranmeldung** auf **Ja** fest.

    b.  Legen Sie das Feld **Benutzerzuweisung erforderlich** auf **Ja** fest.

    c.  Legen Sie das Feld **Für Benutzer sichtbar** auf **Ja** fest.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **ADP** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.

1. Wählen Sie im Dialogfeld **SSO-Methode auswählen** unter **Modus** die Option **Verknüpft**, um Ihre Anwendung mit **ADP** zu verknüpfen.

    ![SSO über Link](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

1. Navigieren Sie zum Abschnitt **Anmelde-URL konfigurieren**, und führen Sie die folgenden Schritte aus:

    ![Eigenschaften für einmaliges Anmelden](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)

    a. Fügen Sie die **URL für den Benutzerzugriff** ein, die Sie oben aus der Registerkarte **Eigenschaften** (aus der Haupt-ADP-App) kopiert haben.
                                                             
    b. Nachfolgend werden die 5 Apps aufgeführt, die unterschiedliche **Relayzustand-URLs** unterstützen. Sie müssen die geeignete **Relayzustand-URL** für die jeweilige Anwendung manuell an die **URL für den Benutzerzugriff** anfügen.
    
    * **ADP Workforce Now**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP Workforce Now Enhanced Time**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP Vantage HCM**
        
        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise HR**

        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>&relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Speichern** Sie die Änderungen.

10. Beginnen Sie nach Bestätigung durch Ihren ADP-Vertriebsbeauftragten mit dem Test für einen oder zwei Benutzer.

    a. Weisen Sie der ADP-Dienst-App einige Benutzer zu, um den Verbundzugriff zu testen.

    b. Der Test ist erfolgreich, wenn die Benutzer auf die ADP-Dienst-App im Katalog und auf ihren ADP-Dienst zugreifen können.
 
11. Wenn der Test erfolgreich war, weisen Sie den ADP-Verbunddienst einzelnen Benutzern oder Benutzergruppen zu, und führen Sie ein Rollout für Ihre Mitarbeiter durch. Dies wird später in diesem Tutorial erläutert.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ADP gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **ADP**aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-adp-test-user"></a>Erstellen eines ADP-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens B. Simon in ADP. Wenden Sie sich an das [ADP-Supportteam](https://www.adp.com/contact-us/overview.aspx), um die Benutzer unter dem ADP-Konto hinzufügen zu lassen. 

### <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ADP“ klicken, sollten Sie automatisch bei Ihrer ADP-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

