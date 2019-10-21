---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Concur Travel and Expense | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Concur Travel and Expense konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6401ace4-71c0-4778-8b15-900f2f5f0c4c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9dddd9f6904aa5ef7840850792aeabf04666dddc
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72373412"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-concur-travel-and-expense"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Concur Travel and Expense

In diesem Tutorial erfahren Sie, wie Sie Concur Travel and Expense in Azure Active Directory (Azure AD) integrieren. Die Integration von Concur Travel and Expense in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Concur Travel and Expense hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Concur Travel and Expense anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Abonnement für Concur Travel and Expense
* Die Rolle „Unternehmensadministrator“ im Concur-Benutzerkonto. Mit dem [Concur-Self-Service-Tool für SSO](https://www.concursolutions.com/nui/authadmin/ssoadmin) können Sie testen, ob Sie über die entsprechenden Zugriffsberechtigungen verfügen. Wenn Sie keinen Zugriff haben, wenden Sie sich an den Concur-Support oder den Projektmanager für die Implementierung. 

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD.

* Concur Travel and Expense unterstützt **IDP-** und **SP-initiiertes** einmaliges Anmelden.
* Concur Travel and Expense unterstützt das Testen von SSO in Produktions- und Implementierungsumgebungen. 

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert für die drei folgenden Regionen: USA, EMEA und China. Es kann also nur eine Instanz für jede Region in einem Mandanten konfiguriert werden. 

## <a name="adding-concur-travel-and-expense-from-the-gallery"></a>Hinzufügen von Concur Travel and Expense aus dem Katalog

Zum Konfigurieren der Integration von Concur Travel and Expense in Azure AD müssen Sie Concur Travel and Expense aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Concur Travel and Expense** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Concur Travel and Expense** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-concur-travel-and-expense"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Concur Travel and Expense

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Concur Travel and Expense mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Concur Travel and Expense eingerichtet werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Concur Travel and Expense zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Concur Travel and Expense](#configure-concur-travel-and-expense-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Concur Travel and Expense-Testbenutzers](#create-concur-travel-and-expense-test-user)** , um eine Entsprechung von B. Simon in Concur Travel and Expense zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Concur Travel and Expense** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Im Abschnitt **Grundlegende SAML-Konfiguration** ist die Anwendung im **IDP-initiierten** Modus vorkonfiguriert, und die erforderlichen URLs sind bereits mit Azure vorausgefüllt. Der Benutzer muss die Konfiguration speichern, indem er auf die Schaltfläche **Speichern** klickt.

    > [!NOTE]
    > Der Bezeichner (Entitäts-ID) und die Antwort-URL (Assertionsverbraucherdienst-URL) sind regionsspezifisch. Wählen Sie die Option basierend auf dem Rechenzentrum Ihrer Concur-Entität aus. Wenn Sie das Rechenzentrum der Concur-Entität nicht kennen, wenden Sie sich an den Concur-Support. 

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Benutzerattribut**, um die Einstellungen zu bearbeiten. Die eindeutige Benutzer-ID muss mit der Concur-Benutzeranmelde-ID (login_id) übereinstimmen. In der Regel muss **user.userprincipalname** in **user.mail** geändert werden.

    ![Benutzerattribut bearbeiten](common/edit-attribute.png)

6. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um die Metadaten-XML herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Concur Travel and Expense gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Concur Travel and Expense** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-concur-travel-and-expense-sso"></a>Konfigurieren des einmaligen Anmeldens für Concur Travel and Expense

1. Zum Konfigurieren des einmaligen Anmeldens aufseiten von **Concur Travel and Expense** müssen Sie die heruntergeladene **Verbundmetadaten-XML** in das [Concur-Self-Service-Tool für SSO](https://www.concursolutions.com/nui/authadmin/ssoadmin) hochladen und sich mit einem Konto mit der Rolle „Unternehmensadministrator“ anmelden. 

1. Klicken Sie auf **Hinzufügen**.
1. Geben Sie einen benutzerdefinierten Namen für Ihren IdP ein, etwa „Azure AD (US)“. 
1. Klicken Sie auf **Upload XML File** (XML-Datei hochladen), und fügen Sie die zuvor heruntergeladene **Verbundmetadaten-XML** an.
1. Klicken Sie auf **Add Metadata** (Metadaten hinzufügen), um die Änderung zu speichern.

    ![Screenshot des Concur-Self-Service-Tools für SSO](./media/concur-travel-and-expense-tutorial/add-metadata-concur-self-service-tool.png)

### <a name="create-concur-travel-and-expense-test-user"></a>Erstellen eines Testbenutzers für Concur Travel and Expense

In diesem Abschnitt erstellen Sie in Concur Travel and Expense einen Benutzer namens B. Simon. Wenden Sie sich an das Concur-Supportteam, um die Benutzer auf der Concur Travel and Expense-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

> [!NOTE]
> Die Concur-Anmelde-ID von B. Simon muss mit dem eindeutigen Bezeichner von B. Simon in Azure AD übereinstimmen. Beispiel: Lautet der eindeutige Azure AD-Bezeichner von B. Simon `B.Simon@contoso.com`, muss die Concur-Anmelde-ID von B. Simon ebenfalls `B.Simon@contoso.com` lauten. 

## <a name="configure-concur-mobile-sso"></a>Konfigurieren von mobilem SSO für Concur
Um mobiles einmaliges Anmelden für Concur zu ermöglichen, müssen Sie die **Benutzerzugriffs-URL** an das Concur-Supportteam senden. Führen Sie die folgenden Schritte aus, um die **Benutzerzugriffs-URL** aus Azure AD abzurufen:
1. Navigieren Sie zu **Unternehmensanwendungen**.
1. Klicken Sie auf **Concur Travel and Expense**.
1. Klicken Sie auf **Eigenschaften**.
1. Kopieren Sie die **Benutzerzugriffs-URL**, und senden Sie diese URL an den Concur-Support.

> [!NOTE]
> Die Self-Service-Option zum Konfigurieren von SSO steht nicht zur Verfügung. Wenden Sie sich daher an das Concur-Supportteam, um mobiles SSO zu aktivieren. 

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Concur Travel and Expense“ klicken, sollten Sie automatisch bei der Concur Travel and Expense-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Concur Travel and Expense mit Azure AD ausprobieren](https://aad.portal.azure.com/)

