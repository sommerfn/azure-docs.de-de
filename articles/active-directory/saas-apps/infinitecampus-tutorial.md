---
title: 'Tutorial: Integration von Infinite Campus in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Infinite Campus konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: d54769c1f3265e2cee619520044313fca46855a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67100366"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Tutorial: Integration von Infinite Campus in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Infinite Campus in Azure Active Directory (Azure AD) integrieren.
Die Integration von Infinite Campus in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Infinite Campus hat.
* Sie können es Ihren Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Infinite Campus angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Infinite Campus in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Infinite Campus-Abonnement, für das einmaliges Anmelden aktiviert ist
* Sie müssen mindestens ein Azure Active Directory-Administrator sein und die Campus Product-Sicherheitsrolle „Student Information System (SIS)“ ausüben, um die Konfiguration abzuschließen.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Infinite Campus unterstützt **SP**-initiiertes einmaliges Anmelden.

## <a name="adding-infinite-campus-from-the-gallery"></a>Hinzufügen von Infinite Campus aus dem Katalog

Zum Konfigurieren der Integration von Infinite Campus in Azure AD müssen Sie Infinite Campus aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Infinite Campus aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie am oberen Rand des Dialogfelds auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Infinite Campus** ein, wählen Sie im Ergebnisbereich **Infinite Campus** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Infinite Campus in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Infinite Campus basierend auf einer Testbenutzerin mit dem Namen **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Infinite Campus eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Infinite Campus müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Infinite Campus](#configure-infinite-campus-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Infinite Campus-Testbenutzers](#create-infinite-campus-test-user)** , um eine Entsprechung von Britta Simon in Infinite Campus zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Infinite Campus die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Infinite Campus** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt „Grundlegende SAML-Konfiguration“ die folgenden Schritte aus (beachten Sie, dass die Domäne je nach Hostingmodell variiert, der Wert **FULLY-QUALIFIED-DOMAIN** aber mit Ihrer Infinite Campus-Installation übereinstimmen muss):

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`

    ![SSO-Informationen zur Domäne und zu den URLs für Infinite Campus](common/sp-identifier-reply.png)

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="configure-infinite-campus-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Infinite Campus

1. Melden Sie sich in einem anderen Webbrowserfenster als Sicherheitsadministrator bei Infinite Campus an.

2. Klicken Sie links im Menü auf **System Administration** (Systemverwaltung).

    ![Administrator](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

3. Navigieren Sie zu **User Security** > **SAML Management** > **SSO Service Provider Configuration** (Benutzersicherheit -> SAML-Verwaltung -> SSO-Dienstanbieterkonfiguration).

    ![SAML](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

4. Führen Sie auf der Seite **SSO Service Provider Configuration** (SSO-Dienstanbieterkonfiguration) die folgenden Schritte aus:

    ![SSO](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

    a. Wählen Sie **Enable SAML Single Sign On** (Einmaliges Anmelden per SAML aktivieren) aus.

    b. Bearbeiten Sie den **Optional Attribute Name** (Optionalen Attributnamen), damit er **name** enthält.

    c. Wählen Sie im Bereich **Select an option to retrieve Identity Provider (IDP) server data** (Option zum Abrufen von IDP-Dienstanbieterdaten auswählen) die Option **Metadata URL** (Metadaten-URL), fügen Sie den aus dem Azure-Portal kopierten Wert für **Verbundmetadaten-URL der App** in das Feld ein, und klicken Sie auf **Sync** (Synchronisieren).

    d. Nach dem Klicken auf **Sync** (Synchronisieren) werden die Werte auf der Seite **SSO Service Provider Configuration** (SSO-Dienstanbieterkonfiguration) automatisch aufgefüllt. Diese Werte können darauf überprüft werden, ob sie den Werten aus Schritt 4 oben entsprechen.

    e. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein. Beispiel: BrittaSimon@contoso.com.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

> [!NOTE]
> Falls alle Ihre Azure-Benutzer SSO-Zugriff auf Infinite Campus haben sollen und das interne Berechtigungssystem von Infinite Campus zum Steuern des Zugriffs verwendet werden soll, können Sie die Eigenschaft **Benutzerzuweisung erforderlich** der Anwendung auf „Nein“ festlegen und dann mit den folgenden Schritten fortfahren.

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Infinite Campus gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Infinite Campus**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Infinite Campus** aus.

    ![Infinite Campus-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-infinite-campus-test-user"></a>Erstellen eines Infinite Campus-Testbenutzers

Infinite Campus besitzt eine demografiezentrierte Architektur. Wenden Sie sich an das [Supportteam von Infinite Campus](mailto:sales@infinitecampus.com), um die Benutzer zur Infinite Campus-Plattform hinzuzufügen.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Infinite Campus“ klicken, sollten Sie automatisch bei der Infinite Campus-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
