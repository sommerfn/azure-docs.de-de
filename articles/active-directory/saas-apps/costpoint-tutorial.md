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
ms.openlocfilehash: 6c1a8b916feb2ad67623434f2b63468be72bf1aa
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68879598"
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

## <a name="adding-costpoint-from-the-gallery"></a>Hinzufügen von Costpoint aus dem Katalog

Zum Konfigurieren der Integration von Costpoint in Azure AD müssen Sie Costpoint aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Costpoint** in das Suchfeld ein.
1. Wählen Sie **Costpoint** im Ergebnisbereich aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Costpoint mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Costpoint eingerichtet werden.

Sie müssen die folgenden Aufgaben ausführen, um das einmalige Anmelden von Azure AD bei Costpoint zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
2. **[Konfigurieren von Costpoint](#configure-costpoint)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
5. **[Erstellen eines Costpoint-Testbenutzers](#create-costpoint-test-user)** , um ein Pendant von Britta Simon in Costpoint zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Costpoint** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie über eine **Dienstanbieter-Metadatendatei** verfügen:

    > [!NOTE]
    > Sie rufen die Dienstanbieter-Metadatendatei weiter unten in diesem Tutorial im Abschnitt **Generieren von Costpoint-Metadaten** ab.
 
    1. Klicken Sie auf **Metadatendatei hochladen**.
    
    1. Klicken Sie auf das **Ordnerlogo**, wählen Sie die Metadatendatei aus, und klicken Sie auf **Hochladen**.
    
    1. Nachdem die Metadatendatei erfolgreich hochgeladen wurde, werden die Werte **Bezeichner** und **Antwort-URL** im Textfeld des Costpoint-Abschnitts automatisch aufgefüllt:

        > [!Note]
        > Falls die Werte **Bezeichner** und **Antwort-URL** nicht automatisch aufgefüllt werden, geben Sie die erforderlichen Werte manuell ein. Vergewissern Sie sich, dass **Bezeichner (Entitäts-ID)** und **Antwort-URL (Assertionsverbraucherdienst-URL)** richtig festgelegt sind und es sich beim Wert unter **ACS-URL** um eine gültige Costpoint-URL handelt, die auf **/LoginServlet.cps** endet.

    1. Klicken Sie auf **Zusätzliche URLs festlegen**.

    1. Geben Sie im Textfeld **Relayzustand** einen Wert nach folgendem Muster ein:`system=[your system], (for example, **system=DELTEKCP**)`

1. Sie können die Anwendung folgendermaßen im **SP-initiierten Modus** konfigurieren:
    
    Geben Sie im Textfeld **Anmelde-URL** eine URL ein: `https://costpointteea.deltek.com/cpweb/cploginform.htm`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit den tatsächlichen Werten für „Bezeichner“, „Antwort-URL“ und „Relayzustand“. Wenden Sie sich an das [Clientsupportteam von Costpoint](https://www.deltek.com/about/contact-us), um diese Werts zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf das Kopiersymbol, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie im Editor.

   ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="generate-costpoint-metadata"></a>Generieren von Costpoint-Metadaten

Die SAML-SSO-Konfiguration für Costpoint wird im Leitfaden **DeltekCostpoint711Security.pdf** erläutert. Lesen Sie dort die Abschnitte **SAML Single Sign-on Setup -> Configure SAML Single Sign-on between Costpoint and Azure AD** (SAML-SSO-Einrichtung > Konfigurieren von SSO für SAML zwischen Costpoint und Azure AD). Befolgen Sie die Anweisungen, und generieren Sie die Datei **Costpoint SP Federation Metadata XML** (Costpoint-SP-Verbundmetadaten-XML). Verwenden Sie diese Datei im Azure-Portal unter **Grundlegende SAML-Konfiguration**.

![Costpoint-Konfigurationshilfsprogramm](./media/costpoint-tutorial/config02.png)

> [!NOTE]
> Den Leitfaden **DeltekCostpoint711Security.pdf** erhalten Sie vom [Supportteam für den Costpoint-Client](https://www.deltek.com/about/contact-us). Falls Sie diese Datei nicht haben, fordern Sie sie beim Supportteam an.

### <a name="configure-costpoint"></a>Konfigurieren von Costpoint

Kehren Sie zum **Costpoint-Konfigurationshilfsprogramm** zurück, und fügen Sie die **App-Verbundmetadaten-URL** in das Textfeld **IdP Federation Metadata XML** (IdP-Verbundmetadaten-XML) ein. Fahren Sie mit den Anweisungen im Leitfaden **DeltekCostpoint711Security.pdf** fort, um die Costpoint-SAML-Einrichtung abzuschließen. 

![Costpoint-Konfigurationshilfsprogramm](./media/costpoint-tutorial/config01.png)

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Costpoint gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Costpoint** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie die Schaltfläche **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste den Eintrag **Britta Simon** aus, und klicken Sie anschließend im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-costpoint-test-user"></a>Erstellen eines Costpoint-Testbenutzers

In diesem Abschnitt erstellen Sie einen Benutzer in Costpoint. Angenommen, die **Benutzer-ID** lautet **B.SIMON** und der Name **B.Simon**. Arbeiten Sie mit dem [Supportteam für den Costpoint-Client](https://www.deltek.com/about/contact-us) zusammen, um die Benutzer zur Costpoint-Plattform hinzuzufügen. Der Benutzer muss erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.
 
Nach der Erstellung muss für **Authentication Method** (Authentifizierungsmethode) die Option **Active Directory** ausgewählt werden, das Kontrollkästchen **SAML Single Sign-on** (Einmaliges Anmelden für SAML) muss aktiviert werden, und für **Active Directory or Certificate ID** (Active Directory- oder Zertifikat-ID) muss der Benutzername von Azure Active Directory angegeben werden, wie in der folgenden Abbildung gezeigt:

![Costpoint-Benutzer](./media/costpoint-tutorial/user01.png)

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „Costpoint“ auswählen, sollten Sie automatisch bei der Costpoint-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)