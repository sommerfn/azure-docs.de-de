---
title: 'Tutorial: Azure Active Directory-Integration mit Costpoint | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Costpoint konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 9ecc5f58-4462-4ade-ab73-0a4f61027504
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01168540e27605db0d240c0774159a710b5d5254
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71840092"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Tutorial: Integrieren von Costpoint in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Costpoint in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Costpoint in Azure AD haben Sie folgende Möglichkeiten:

* Steuern Sie in Azure AD, wer Zugriff auf Costpoint hat.
* Ermöglichen Sie Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Costpoint anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Costpoint-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Costpoint unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.

## <a name="generate-costpoint-metadata"></a>Generieren von Costpoint-Metadaten

Die SAML-SSO-Konfiguration für Costpoint wird im Leitfaden **DeltekCostpoint711Security.pdf** erläutert. Laden Sie diesen Leitfaden von der Deltek Costpoint-Supportwebsite herunter, und sehen Sie sich die Abschnitte **SAML Single Sign-on Setup** (SAML-SSO-Einrichtung) > **Configure SAML Single Sign-on between Costpoint and Microsoft Azure** (Konfigurieren von SSO für SAML zwischen Costpoint und Azure AD) an. Befolgen Sie die Anweisungen, und generieren Sie die Datei **Costpoint SP Federation Metadata XML** (Costpoint-SP-Verbundmetadaten-XML). 

![Costpoint-Konfigurationshilfsprogramm](./media/costpoint-tutorial/config-utility.png)

## <a name="add-costpoint-from-the-gallery"></a>Hinzufügen von Costpoint aus dem Katalog

Fügen Sie zum Integrieren von Costpoint in Azure AD Costpoint zunächst aus dem Katalog im Azure-Portal der Liste der verwalteten SaaS-Apps hinzu:

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.

   ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

1. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen** aus.

   ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.

   ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Costpoint** in das Suchfeld ein.

   ![Costpoint in der Ergebnisliste](common/search-new-app.png)

1. Wählen Sie in der Ergebnisliste **Costpoint** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sgn-on"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Costpoint mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Costpoint eingerichtet werden.

Sie müssen die folgenden Aufgaben ausführen, um das einmalige Anmelden von Azure AD bei Costpoint zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
1. **[Konfigurieren von Costpoint](#configure-costpoint)** , um die SAML-Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Erstellen eines Costpoint-Testbenutzers](#create-a-costpoint-test-user)** , um ein Pendant von B. Simon in Costpoint zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren:

1. Wählen Sie auf der Anwendungsintegrationsseite für **Costpoint** die Option **Einmaliges Anmelden** aus.

   ![Link zum Konfigurieren des einmaligen Anmeldens](common/select-sso.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie über eine *Dienstanbieter-Metadatendatei* verfügen:

   > [!NOTE]
   > Die Dienstanbieter-Metadatendatei können Sie unter [Generieren von Costpoint-Metadaten](#generate-costpoint-metadata) abrufen. Die Verwendung der Datei wird weiter unten im Tutorial erläutert.
 
   1. Wählen Sie die Schaltfläche **Metadatendatei hochladen** und dann die Datei **Costpoint SP Federation Metadata XML** (Costpoint-SP-Verbundmetadaten-XML) aus, die zuvor von Costpoint generiert wurde. Wählen Sie anschließend die Schaltfläche **Hinzufügen** aus, um die Datei hochzuladen.

      ![Hochladen der Metadatendatei](./media/costpoint-tutorial/upload-metadata.png)
    
   1. Nachdem die Metadatendatei erfolgreich hochgeladen wurde, werden die Werte **Bezeichner** und **Antwort-URL** im Costpoint-Abschnitt automatisch aufgefüllt.

      > [!NOTE]
      > Falls die Werte **Bezeichner** und **Antwort-URL** nicht automatisch aufgefüllt werden, geben Sie die erforderlichen Werte manuell ein. Vergewissern Sie sich, dass **Bezeichner (Entitäts-ID)** und **Antwort-URL (Assertionsverbraucherdienst-URL)** richtig festgelegt sind und es sich beim Wert unter **ACS-URL** um eine gültige Costpoint-URL handelt, die auf **/LoginServlet.cps** endet.

   1. Wählen Sie **Zusätzliche URLs festlegen** aus. Geben Sie für **Relayzustand** einen Wert im folgenden Format ein: `system=[your system]` (Beispiel: **system=DELTEKCP**).

1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** das **Kopiersymbol** aus, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie im Editor.

   ![SAML-Signaturzertifikat](common/copy-metadataurl.png)

### <a name="configure-costpoint"></a>Konfigurieren von Costpoint

1. Wechseln Sie zurück zum Costpoint-Konfigurationshilfsprogramm. Fügen Sie in das Textfeld **IdP Federation Metadata XML** (IdP-Verbundmetadaten-XML) den Inhalt der Datei *App-Verbundmetadaten-URL* ein. 

   ![Costpoint-Konfigurationshilfsprogramm](./media/costpoint-tutorial/config-utility-idp.png)

1. Befolgen Sie die übrigen Anweisungen im Leitfaden **DeltekCostpoint711Security.pdf**, um die Costpoint-SAML-Einrichtung abzuschließen.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens B. Simon im Azure-Portal.

1. Wählen Sie im linken Bereich des Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.

   ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

1. Wählen Sie **Neuer Benutzer** aus.

   ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

1. Führen Sie in den **Benutzereigenschaften** die folgenden Schritte aus:

   ![Dialogfeld „Benutzer“](common/user-properties.png)

   1. Geben Sie **B.Simon** in das Feld **Name** ein.
   
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge `b.simon\@yourcompanydomain.extension` ein (z. B. B.Simon@contoso.com).
   
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Costpoint gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.

1. Wählen Sie in der Anwendungsliste **Costpoint** aus.

1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Klicken Sie auf **Benutzer hinzufügen**. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

   ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **B.Simon** aus. Wählen Sie anschließend **Auswählen** aus.

1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus der Liste aus. Wählen Sie anschließend **Auswählen** aus.

1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.

### <a name="create-a-costpoint-test-user"></a>Erstellen eines Costpoint-Testbenutzers

In diesem Abschnitt erstellen Sie einen Benutzer in Costpoint. Angenommen, die Benutzer-ID lautet **B.SIMON** und der Benutzername **B.Simon**. Arbeiten Sie mit dem [Supportteam für den Costpoint-Client](https://www.deltek.com/about/contact-us) zusammen, um die Benutzer zur Costpoint-Plattform hinzuzufügen. Der Benutzer muss erstellt und aktiviert werden, damit er einmaliges Anmelden verwenden kann.

Nach der Erstellung des Benutzers muss für **Authentication Method** (Authentifizierungsmethode) die Option **Active Directory** ausgewählt werden, das Kontrollkästchen **SAML Single Sign-on** (Einmaliges Anmelden für SAML) muss aktiviert werden, und für **Active Directory or Certificate ID** (Active Directory- oder Zertifikat-ID) muss der Benutzername von Azure Active Directory angegeben werden, wie im folgenden Screenshot gezeigt:

![Costpoint-Benutzer](./media/costpoint-tutorial/costpoint-user.png)

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „Costpoint“ auswählen, sollten Sie automatisch bei der Costpoint-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
