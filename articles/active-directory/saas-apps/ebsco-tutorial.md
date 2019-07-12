---
title: 'Tutorial: Azure Active Directory-Integration mit EBSCO | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und EBSCO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: b6f1198334d8bd4ba58b4fbba02942fc8c54c578
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442859"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Tutorial: Azure Active Directory-Integration mit EBSCO

In diesem Tutorial erfahren Sie, wie Sie EBSCO in Azure Active Directory (Azure AD) integrieren.
Die Integration von EBSCO in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf EBSCO hat.
* Sie können es Ihren Benutzern ermöglichen, dass sie mit ihren Azure AD-Konten automatisch bei EBSCO angemeldet werden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit EBSCO benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Sollten Sie über keine Azure AD-Umgebung verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) verwenden.
* EBSCO-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* EBSCO unterstützt **SP**- und **IDP**-initiiertes einmaliges Anmelden.

* EBSCO unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-ebsco-from-the-gallery"></a>Hinzufügen von EBSCO aus dem Katalog

Zum Konfigurieren der Integration von EBSCO in Azure AD müssen Sie EBSCO aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um EBSCO aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie am oberen Rand des Dialogfelds auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **EBSCO** ein, wählen Sie im Ergebnisbereich **EBSCO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![EBSCO in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei EBSCO mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EBSCO eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei EBSCO müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für EBSCO](#configure-ebsco-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines EBSCO-Testbenutzers](#create-ebsco-test-user)** , um eine Entsprechung von Britta Simon in EBSCO zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in EBSCO die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **EBSCO** die Option **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    ![SSO-Informationen zur Domäne und zu den URLs für EBSCO](common/idp-identifier.png)

    Geben Sie im Textfeld **Bezeichner** eine URL ein: `pingsso.ebscohost.com`.

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![image](common/both-preintegrated-signon.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > Der Wert der Anmelde-URL entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von EBSCO](mailto:sso@ebsco.com), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

    o   **Eindeutige Elemente:**  

    o   **Custid** – Geben Sie die eindeutige EBSCO-Kunden-ID ein. 

    o   **Profile** – Clients können den Link, mit dem Benutzer zu einem bestimmten Profil umgeleitet werden (je nachdem, was sie von EBSCO erwerben), anpassen. Sie können eine bestimmte Profil-ID eingeben. Die Haupt-IDs sind eds (EBSCO Discovery Service) und ehost (EBSOCOhost-Datenbanken). Die zugehörigen Anweisungen finden Sie [hier](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

6. Ihre EBSCO-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld **Benutzerattribute** zu öffnen.

    ![image](common/edit-attribute.png)

     > [!Note]
    > Das **name**-Attribut ist obligatorisch. Es wird dem Wert **Benutzerbezeichner** in der EBSCO-Anwendung zugeordnet. Es wird standardmäßig hinzugefügt, sodass Sie es nicht manuell hinzufügen müssen.

7. Darüber hinaus wird von der EBSCO-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen: 

    | NAME | Quellattribut|
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | Nachname   | user.surname |
    | E-Mail   | user.mail |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **Speichern**.

8. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um den Ihren Anforderungen entsprechenden **Verbundmetadaten-XML**-Code aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

9. Kopieren Sie im Abschnitt **EBSCO einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-ebsco-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für EBSCO

Zum Konfigurieren des einmaligen Anmeldens aufseiten von **EBSCO** müssen Sie die heruntergeladene **Metadaten-XML** und die kopierten URLs aus dem Azure-Portal an das [Supportteam von EBSCO](mailto:sso@ebsco.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EBSCO gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen**, **Alle Anwendungen** und **EBSCO**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste die Option **EBSCO**aus.

    ![EBSCO-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-ebsco-test-user"></a>Erstellen eines EBSCO-Testbenutzers

Bei EBSCO erfolgt die Benutzerbereitstellung automatisch.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

Azure AD übermittelt die erforderlichen Daten an die EBSCO-Anwendung. Die Benutzerbereitstellung von EBSCO kann automatisch oder einmalig erfolgen. Dies hängt davon ab, ob der Client über viele bereits vorhandene EBSCOhost-Konten verfügt, in denen persönliche Einstellungen gespeichert sind. Dies kann während der Implementierung mit dem [EBSCO-Supportteam](mailto:sso@ebsco.com) erörtert werden. Der Client muss in keinem der beiden Fälle vor dem Testen EBSCOhost-Konten erstellen.

   >[!Note]
   >Sie können die Bereitstellung und Personalisierung von EBSCOhost-Benutzern automatisieren. Wenden Sie sich mit Fragen zur Just-In-Time-Benutzerbereitstellung an das [EBSCO-Supportteam](mailto:sso@ebsco.com). 

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

1. Wenn Sie im Zugriffsbereich auf die Kachel „EBSCO“ klicken, sollten Sie automatisch bei Ihrer EBSCO-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md).

2. Klicken Sie nach der Anmeldung bei der Anwendung in der rechten oberen Ecke auf die Schaltfläche **Anmelden**.

    ![EBSCO-Anmeldung in der Anwendungsliste](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Sie erhalten einmalig eine Aufforderung zum Koppeln der institutionellen/SAML-Anmeldung, die entweder **Link your existing MyEBSCOhost account to your institution account now** (Verknüpfen Sie Ihr vorhandenes MyEBSCOhost-Konto jetzt mit Ihrem Unternehmenskonto) ODER **Create a new MyEBSCOhost account and link it to your institution account** (Erstellen Sie ein neues MyEBSCOhost-Konto, und verknüpfen Sie es mit Ihrem Unternehmenskonto) lautet. Das Konto wird für die Personalisierung in der EBSCOhost-Anwendung verwendet. Wählen Sie die Option **Create a new account** (Neues Konto erstellen) aus. Daraufhin wird das Formular für die Personalisierung wie im folgenden Screenshot gezeigt vorab mit den Werten aus der SAML-Antwort aufgefüllt. Klicken Sie auf **Weiter**, um diese Auswahl zu speichern.
    
     ![Der EBSCO-Benutzer in der Anwendungsliste](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Löschen Sie nach Abschluss der oben beschriebenen Einrichtung Ihre Cookies und den Cache, und melden Sie sich erneut an. Sie müssen sich nicht erneut manuell anmelden, und die Personalisierungseinstellungen werden gespeichert.

## <a name="additional-sesources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

