---
title: 'Tutorial: Azure Active Directory-Integration mit Lifesize Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Lifesize Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e886a1fa3f590ac94dbf088520e6770690ee21ac
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73159572"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Tutorial: Azure Active Directory-Integration mit Lifesize Cloud

In diesem Tutorial erfahren Sie, wie Sie Lifesize Cloud in Azure Active Directory (Azure AD) integrieren.
Die Integration von Lifesize Cloud in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf Lifesize Cloud Zugriff hat.
* Sie können es Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Lifesize Cloud angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Lifesize Cloud konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein Lifesize Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Lifesize Cloud unterstützt **SP**-initiiertes einmaliges Anmelden.

* Lifesize Cloud unterstützt die **automatisierte Benutzerbereitstellung**.

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Hinzufügen von Lifesize Cloud aus dem Katalog

Zum Konfigurieren der Integration von Lifesize Cloud in Azure AD müssen Sie Lifesize Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Lifesize Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Lifesize Cloud** ein, wählen Sie im Ergebnisbereich **Lifesize Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Lifesize Cloud in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand einer Testbenutzerin namens **Britta Simon** das einmalige Anmelden von Azure AD mit Lifesize Cloud.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Lifesize Cloud eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Lifesize Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Lifesize Cloud](#configure-lifesize-cloud-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Lifesize Cloud-Testbenutzers](#create-lifesize-cloud-test-user)** , um eine Entsprechung von Britta Simon in Lifesize Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Lifesize Cloud die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Lifesize Cloud** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Lifesize Cloud](common/sp-identifier-relay.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://login.lifesizecloud.com/ls/?acs`

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://login.lifesizecloud.com/<companyname>`

    c. Klicken Sie auf **Zusätzliche URLs festlegen**.

    d. Geben Sie im Textfeld **Relayzustand** eine URL nach folgendem Muster ein: `https://webapp.lifesizecloud.com/?ent=<identifier>`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte von Anmelde-URL, Bezeichner und Relayzustand. Wenden Sie sich an das [Kundensupportteam von Lifesize Cloud](https://www.lifesize.com/en/support), um die Werte für die Anmelde-URL und den Bezeichner zu erhalten. Den Wert für den Relayzustand können Sie der SSO-Konfiguration entnehmen, die später in diesem Tutorial erläutert wird. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Lifesize Cloud einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-lifesize-cloud-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Lifesize Cloud

1. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung bei der Lifesize Cloud-Anwendung mit Administratorrechten an.

2. Klicken Sie in der rechten oberen Ecke auf Ihren Namen und dann auf **Erweiterte Einstellungen**.

    ![Configure single sign-on](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

3. Klicken Sie in den erweiterten Einstellungen nun auf den Link **SSO-Konfiguration**. Damit wird die SSO-Konfigurationsseite für Ihre Instanz geöffnet.

    ![Configure single sign-on](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

4. Konfigurieren Sie jetzt die folgenden Werte in der SSO-Konfigurationsbenutzeroberfläche.

    ![Configure single sign-on](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    a. Fügen Sie in das Textfeld **Aussteller des Identitätsanbieters** den Wert vom **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    b.  Fügen Sie in das Textfeld **Login URL** (Anmelde-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das Base64-codierte Zertifikat im Editor, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
  
    d. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld „Vorname“ den Wert als `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` ein.

    e. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld **Nachname** den Wert als `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` ein.

    f. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld **E-Mail** den Wert als `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.

5. Um die Konfiguration zu überprüfen, klicken Sie auf die Schaltfläche **Testen**.

    >[!NOTE]
    >Zum erfolgreichen Testen müssen Sie den Konfigurations-Assistenten in Azure AD abschließen und den Benutzer oder Gruppen, die den Test durchführen können, Zugriff gewähren.

6. Aktivieren Sie SSO, indem Sie auf die Schaltfläche **SSO aktivieren** klicken.

7. Klicken Sie nun auf die Schaltfläche **Update**, damit alle Einstellungen gespeichert werden. Dadurch wird den Wert RelayState generiert. Kopieren Sie den RelayState-Wert, der im Textfeld generiert wird, und fügen Sie ihn in das Textfeld **Relayzustand** im Abschnitt **Domäne und URLs für Lifesize Cloud** ein.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf Lifesize Cloud gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, und wählen Sie dann **Alle Anwendungen** und **Lifesize Cloud** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Lifesize Cloud** aus.

    ![Lifesize Cloud-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-lifesize-cloud-test-user"></a>Erstellen eines Lifesize Cloud-Testbenutzers

In diesem Abschnitt erstellen Sie in Lifesize Cloud einen Benutzer namens Britta Simon. Lifesize Cloud unterstützt die automatische Benutzerbereitstellung. Nach erfolgreicher Authentifizierung bei Azure AD wird der Benutzer automatisch in der Anwendung bereitgestellt.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Lifesize Cloud“ klicken, sollte die Anmeldeseite der Lifesize Cloud-Anwendung angezeigt werden. Hier müssen Sie Ihren Benutzernamen eingeben und werden anschließend auf die Startseite der Anwendung umgeleitet.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
