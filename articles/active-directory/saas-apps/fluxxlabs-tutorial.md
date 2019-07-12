---
title: 'Tutorial: Azure Active Directory-Integration mit Fluxx Labs | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Fluxx Labs konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e624520a9d1f39bc8115ac72e9df0398065928f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102390"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Tutorial: Azure Active Directory-Integration mit Fluxx Labs

In diesem Tutorial erfahren Sie, wie Sie Fluxx Labs in Azure Active Directory (Azure AD) integrieren.
Die Integration von Fluxx Labs in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Fluxx Labs hat.
* Sie können es Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Fluxx Labs angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Fluxx Labs konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Fluxx Labs-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Fluxx Labs unterstützt **IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-fluxx-labs-from-the-gallery"></a>Hinzufügen von Fluxx Labs aus dem Katalog

Zum Konfigurieren der Integration von Fluxx Labs in Azure AD müssen Sie Fluxx Labs aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Fluxx Labs aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Fluxx Labs** ein, wählen Sie im Ergebnisbereich **Fluxx Labs** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Fluxx-Labs in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Fluxx Labs mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Fluxx Labs eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Fluxx Labs müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Fluxx Labs](#configure-fluxx-labs-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Fluxx Labs-Testbenutzers](#create-fluxx-labs-test-user)** , um eine Entsprechung von Britta Simon in Fluxx Labs zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Fluxx Labs die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Fluxx Labs** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Fluxx Labs](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein:

    | Environment | URL-Muster|
    |-------------|------------|
    | Bereitstellung | `https://<subdomain>.fluxx.io` |
    | Vor der Bereitstellung | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein:

    | Environment | URL-Muster|
    |-------------|------------|
    | Bereitstellung | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Vor der Bereitstellung | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Kundensupportteam von Fluxx Labs](mailto:travis@fluxxlabs.com), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

6. Kopieren Sie im Abschnitt **Fluxx Labs einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-fluxx-labs-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Fluxx Labs

1. Melden Sie sich in einem anderen Webbrowserfenster an der Fluxx Labs-Unternehmenswebsite als Administrator an.

2. Wählen Sie unten im Abschnitt **Settings** (Einstellungen) die Option **Admin** (Administrator).

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config1.png)

3. Klicken Sie im Verwaltungsbereich auf **Plug-Ins** > **Integrations** (Integrationen) und anschließend auf **SAML SSO-(Disabled)** (SAML-SSO (Deaktiviert)).

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config2.png)

4. Führen Sie im Abschnitt mit den Attributen die folgenden Schritte aus:

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config3.png)

    a. Aktivieren Sie das Kontrollkästchen **SAML SSO** (SAML-SSO).

    b. Geben Sie im Textfeld **Request Path** (Anforderungspfad) den Pfad **/auth/saml** ein.

    c. Geben Sie im Textfeld **Callback Path** (Rückrufpfad) den Pfad **/auth/saml/callback** ein.

    d. Geben Sie in das Textfeld **Assertion Consumer Service Url (Single Sign-On URL)** (Assertionsverbraucherdienst-URL (URL für einmaliges Anmelden)) den Wert der **Antwort-URL** ein, den Sie im Azure-Portal eingegeben haben.

    e. Geben Sie in das Textfeld **Audience (SP Entity ID)** (Zielgruppe (SP-Entitäts-ID)) den Wert unter **Bezeichner** ein, den Sie im Azure-Portal eingegeben haben.

    f. Fügen Sie in das Textfeld **Identity Provider SSO Target URL** (URL für einmaliges Anmelden des Identitätsanbieters) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    g. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.

    h. Geben Sie in das Textfeld **Name identifier Format** (Namensbezeichnerformat) den Wert `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress` ein.

    i. Klicken Sie auf **Speichern**.

    > [!NOTE]
    > Nachdem der Inhalt gespeichert wurde, wird das Feld aus Sicherheitsgründen leer angezeigt, der Wert wurde jedoch in der Konfiguration gespeichert.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen brittasimon@yourcompanydomain.extension ein. Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Fluxx Labs gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Fluxx Labs**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Fluxx Labs** aus.

    ![Fluxx Labs-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-fluxx-labs-test-user"></a>Erstellen eines Fluxx Labs-Testbenutzers

Damit sich Azure AD-Benutzer bei Fluxx Labs anmelden können, müssen sie in Fluxx Labs bereitgestellt werden. Im Fall von Fluxx Labs ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich an der Fluxx Labs-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf das unten angezeigte **Symbol**.

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config6.png)

3. Klicken Sie auf dem Dashboard auf das unten dargestellte Symbol, um die Karte **New PEOPLE** (Neue PERSONEN) zu öffnen.

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config4.png)

4. Führen Sie im Abschnitt **NEW PEOPLE** (NEUE PERSONEN) die folgenden Schritte aus:

    ![Fluxx Labs-Konfiguration](./media/fluxxlabs-tutorial/config5.png)

    a. Fluxx Labs verwendet die E-Mail-Adresse als eindeutigen Bezeichner für SSO-Anmeldenamen. Geben Sie im Feld **SSO UID** (SSO-UID) die E-Mail-Adresse des Benutzers ein, die der Benutzer für die SSO-Anmeldung verwendet.

    b. Klicken Sie auf **Speichern**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Fluxx Labs“ klicken, sollten Sie automatisch bei der Fluxx Labs-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

