---
title: 'Tutorial: Azure Active Directory-Integration mit Five9 Plus Adapter (CTI, Contact Center-Agents) | Microsoft-Dokumentation'
description: Informationen zum Konfigurieren des einmaligen Anmeldens zwischen Azure Active Directory und Five9 Plus Adapter (CTI, Contact Center-Agents)
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/04/2019
ms.author: jeedes
ms.openlocfilehash: dd4eab2d8e877cac468097c0a6ed6aa3cfb750b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67102447"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Tutorial: Azure Active Directory-Integration mit Five9 Plus Adapter (CTI, Contact Center-Agents)

In diesem Tutorial erfahren Sie, wie Sie Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory (Azure AD) integrieren.
Durch das Integrieren von Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory erhalten Sie die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Five9 Plus Adapter (CTI, Contact Center Agents) hat.
* Sie können es Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei Five9 Plus Adapter (CTI, Contact Center Agents) angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Konfiguration mit Five9 Plus Adapter (CTI, Contact Center-Agents) benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie nicht über eine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* Five9 Plus Adapter (CTI, Contact Center Agents)-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Five9 Plus Adapter (CTI, Contact Center Agents) unterstützt **IDP**-initiiertes einmaliges Anmelden.

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Hinzufügen von Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog

Um die Integration von Five9 Plus Adapter (CTI, Contact Center-Agents) in Azure Active Directory zu konfigurieren, müssen Sie Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um Five9 Plus Adapter (CTI, Contact Center-Agents) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Five9 Plus Adapter (CTI, Contact Center Agents)** ein, wählen Sie im Ergebnisbereich die Option **Five9 Plus Adapter (CTI, Contact Center Agents)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Five9 Plus Adapter (CTI, Contact Center Agents) in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Five9 Plus Adapter (CTI, Contact Center Agents) mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Five9 Plus Adapter (CTI, Contact Center Agents) eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Five9 Plus Adapter (CTI, Contact Center-Agents) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Five9 Plus Adapter (CTI, Contact Center Agents)](#configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Five9 Plus Adapter (CTI, Contact Center Agents)-Testbenutzers](#create-five9-plus-adapter-cti-contact-center-agents-test-user)** , um eine Entsprechung von Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie für die Konfiguration des einmaligen Anmeldens von Azure AD mit Five9 Plus Adapter (CTI, Contact Center Agents) die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Five9 Plus Adapter (CTI, Contact Center Agents)** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Five9 Plus Adapter (CTI, Contact Center Agents)](common/idp-intiated.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein:
    
    |    Environment      |       URL      |
    | :-- | :-- |
    | Für „Five9 Plus Adapter für Microsoft Dynamics CRM“ | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Für „Five9 Plus Adapter für Zendesk“ | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Für „Five9 Plus Adapter für Agent Desktop Toolkit“ | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein:

    |      Environment     |      URL      |
    | :--                  | :--           |
    | Für „Five9 Plus Adapter für Microsoft Dynamics CRM“ | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Für „Five9 Plus Adapter für Zendesk“ | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Für „Five9 Plus Adapter für Agent Desktop Toolkit“ | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

6. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

7. Kopieren Sie im Abschnitt **Five9 Plus Adapter (CTI, Contact Center Agents) einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-five9-plus-adapter-cti-contact-center-agents-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Five9 Plus Adapter (CTI, Contact Center Agents)

1. Zum Konfigurieren des einmaligen Anmeldens bei **Five9 Plus Adapter (CTI, Contact Center Agents)** müssen Sie das heruntergeladene **Zertifikat (Base64)** und die entsprechenden kopierten URLs an das [Supportteam von Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact) senden. Um zudem SSO weiter zu konfigurieren, befolgen Sie die nachstehenden Schritte gemäß des Adapters:

    a. Administratorhandbuch „Five9 Plus Adapter für Agent Desktop Toolkit“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. Administratorhandbuch „Five9 Plus Adapter für Microsoft Dynamics CRM“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. Administratorhandbuch „Five9 Plus Adapter für Zendesk“: [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers 

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** den Namen `brittasimon@yourcompanydomain.extension` ein. Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Five9 Plus Adapter (CTI, Contact Center-Agents) gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **Five9 Plus Adapter (CTI, Contact Center Agents)** .

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Liste der Anwendungen **Five9 Plus Adapter (CTI, Contact Center-Agents)** aus.

    ![Link „Five9 Plus Adapter (CTI, Contact Center Agents)“ in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Erstellen eines Five9 Plus Adapter (CTI, Contact Center Agents)-Testbenutzers

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Five9 Plus Adapter (CTI, Contact Center-Agents). Arbeiten Sie mit dem [Supportteam von Five9 Plus Adapter (CTI, Contact Center-Agents)](https://www.five9.com/about/contact), um die Benutzer zur Five9 Plus Adapter-Plattform (CTI, Contact Center-Agents) hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Five9 Plus Adapter (CTI, Contact Center Agents)“ klicken, sollten Sie automatisch bei Ihrer Instanz von Five9 Plus Adapter (CTI, Contact Center Agents) angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

