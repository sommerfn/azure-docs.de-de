---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit BeyondTrust Remote Support | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BeyondTrust Remote Support konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 193b163f-bdee-4974-b16d-777c51b991df
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff21c3ee7721c82232e668ddb9645895080cf79
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082036"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-beyondtrust-remote-support"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit BeyondTrust Remote Support

In diesem Tutorial erfahren Sie, wie Sie BeyondTrust Remote Support in Azure Active Directory (Azure AD) integrieren. Die Integration von BeyondTrust Remote Support in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf BeyondTrust Remote Support hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei BeyondTrust Remote Support anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein BeyondTrust Remote Support-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* BeyondTrust Remote Support unterstützt **SP**-initiiertes einmaliges Anmelden.
* BeyondTrust Remote Support unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-beyondtrust-remote-support-from-the-gallery"></a>Hinzufügen von BeyondTrust Remote Support aus dem Katalog

Zum Konfigurieren der Integration von BeyondTrust Remote Support in Azure AD müssen Sie BeyondTrust Remote Support über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **BeyondTrust Remote Support** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **BeyondTrust Remote Support** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-beyondtrust-remote-support"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für BeyondTrust Remote Support

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit BeyondTrust Remote Support mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BeyondTrust Remote Support eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit BeyondTrust Remote Support die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für BeyondTrust Remote Support](#configure-beyondtrust-remote-support-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines BeyondTrust Remote Support-Testbenutzers](#create-beyondtrust-remote-support-test-user)** , um eine Entsprechung von B. Simon in BeyondTrust Remote Support zu erhalten, die mit der Benutzerdarstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **BeyondTrust Remote Support** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<HOSTNAME>.bomgar.com/saml`

    b. Geben Sie im Feld **Bezeichner** eine URL im folgenden Format ein: `https://<HOSTNAME>.bomgar.com`.

    c. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<HOSTNAME>.bomgar.com/saml/sso`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL. Diese Werte werden später in diesem Tutorial erklärt.

1. BeyondTrust Remote Support erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von BeyondTrust Remote Support erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.

    | NAME |  Quellattribut|
    | ---------------| ----------|
    | Givenname | user.givenname |
    | Emailaddress | user.mail |
    | NAME | user.userprincipalname |
    | Username | user.userprincipalname |
    | Gruppen | user.groups |
    | Eindeutige Benutzer-ID | user.userprincipalname |

    > [!NOTE]
    > Wenn Sie Azure AD-Gruppen für die BeyondTrust Remote Support-Anwendung zuweisen, müssen Sie die Option „Im Anspruch zurückgegebene Gruppen“ von „None“ in „SecurityGroup“ ändern. Die Gruppen werden als Objekt-IDs in die Anwendung importiert. Die Objekt-ID der Azure AD-Gruppe finden Sie, indem Sie die Eigenschaften in der Azure Active Directory-Schnittstelle überprüfen. Dies ist erforderlich, um auf Azure AD-Gruppen zu verweisen und sie den richtigen Gruppenrichtlinien zuzuweisen.

1. Beim Festlegen der eindeutigen Benutzer-ID muss dieser Wert auf das folgende NameID-Format festgelegt werden: **Beständig**. Eine beständige ID ist erforderlich, damit der Benutzer richtig identifiziert und den richtigen Gruppenrichtlinien für die Berechtigungen zugeordnet werden kann. Klicken Sie auf das Bearbeitungssymbol, um das Dialogfeld **Benutzerattribute und Ansprüche** zu öffnen und den Wert der eindeutigen Benutzer-ID zu bearbeiten.

1. Klicken Sie im Bereich **Anspruch verwalten** auf **Namensbezeichnerformat auswählen**, legen Sie den Wert auf **Beständig** fest, und klicken Sie auf **Speichern**.

    ![Benutzerattribute und Ansprüche](./media/bomgarremotesupport-tutorial/attribute-unique-user-identifier.png)

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **BeyondTrust Remote Support einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf BeyondTrust Remote Support gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **BeyondTrust Remote Support** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-beyondtrust-remote-support-sso"></a>Konfigurieren von SSO für BeyondTrust Remote Support

1. Melden Sie sich in einem anderen Webbrowserfenster bei BeyondTrust Remote Support als Administrator an.

1. Klicken Sie auf das Menü **STATUS**, kopieren Sie **Bezeichner**, **Antwort-URL** und **Anmelde-URL**, und verwenden Sie diese Werte im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal.

    ![Konfigurieren von BeyondTrust Remote Support](./media/bomgarremotesupport-tutorial/config-url-values.png)

1. Navigieren Sie zu der /login-Oberfläche von BeyondTrust Remote Support unter `https://support.example.com/login`, wobei **support.example.com** der primäre Hostname Ihrer Appliance ist, und authentifizieren Sie sich mit Ihren Administratoranmeldeinformationen.

1. Navigieren Sie zu **Users & Security** (Benutzer und Sicherheit)  > **Security Providers** (Sicherheitsanbieter).

1. Wählen Sie im Dropdownmenü **SAML** aus, und klicken Sie auf die Schaltfläche **Create Provider** (Anbieter erstellen).

1. Im Abschnitt „Identity Provider Settings“ (Identitätsanbietereinstellungen) gibt es eine Option zum Hochladen der Metadaten des Identitätsanbieters. Suchen Sie die Metadaten-XML-Datei, die Sie vom Azure-Portal heruntergeladen haben, und klicken Sie auf die Schaltfläche **Upload** (Hochladen). **Entity ID** (Entitäts-ID), **Single Sign-On Service URL** (Dienst-URL für einmaliges Anmelden)und das Zertifikat werden automatisch hochgeladen, und **Protocol Binding** (Protokollbindung) muss in **HTTP POST** geändert werden. Siehe dazu den folgenden Screenshot:

    ![Konfigurieren von BeyondTrust Remote Support](./media/bomgarremotesupport-tutorial/config-uploadfile.png)

### <a name="create-beyondtrust-remote-support-test-user"></a>Erstellen eines Testbenutzers für BeyondTrust Remote Support

Hier werden die Einstellungen für die Benutzerbereitstellung konfiguriert. Auf die in diesem Abschnitt verwendeten Werte wird vom Abschnitt **Benutzerattribute und Ansprüche** im Azure-Portal verwiesen. Wir haben diese als Standardwerte konfiguriert, die zum Zeitpunkt der Erstellung bereits importiert werden. Der Wert kann jedoch bei Bedarf angepasst werden.

![Erstellen eines Benutzers](./media/bomgarremotesupport-tutorial/config-user1.png)

> [!NOTE]
> Die Gruppen- und E-Mail-Attribute sind für diese Implementierung nicht erforderlich. Wenn Sie Azure AD-Gruppen verwenden und diese den BeyondTrust Remote Support-Gruppenrichtlinien für Berechtigungen zuweisen, muss über die Eigenschaften im Azure-Portal auf die Objekt-ID der Gruppe verwiesen werden, und sie muss im Abschnitt „Verfügbare Gruppen“ abgelegt werden. Nachdem dieser Vorgang abgeschlossen ist, ist die Objekt-ID/AD-Gruppe für die Zuweisung zu einer Gruppenrichtlinie für Berechtigungen verfügbar.

![Erstellen eines Benutzers](./media/bomgarremotesupport-tutorial/config-user2.png)

![Erstellen eines Benutzers](./media/bomgarremotesupport-tutorial/config-user3.png)

> [!NOTE]
> Alternativ kann eine Standardgruppenrichtlinie für den SAML2-Sicherheitsanbieter festgelegt werden. Wenn Sie diese Option definieren, werden dadurch allen Benutzern, die sich über SAML authentifizieren, die in der Gruppenrichtlinie angegebenen Berechtigungen zugewiesen. Die Richtlinie „General Members“ (Allgemeine Mitglieder) ist in BeyondTrust Remote Support/Privileged Remote Access mit beschränkten Berechtigungen enthalten und kann verwendet werden, um die Authentifizierung zu testen und Benutzer den korrekten Richtlinien zuzuweisen. Benutzer werden über /login > „Users & Security“ (Benutzer und Sicherheit) erst dann in die SAML2-Benutzerliste eingetragen, wenn der erste erfolgreiche Authentifizierungsversuch erfolgt. Weitere Informationen zu Gruppenrichtlinien finden Sie unter den folgenden Links: `https://www.beyondtrust.com/docs/remote-support/getting-started/admin/group-policies.htm`

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BeyondTrust Remote Support“ klicken, werden Sie automatisch bei der Instanz von BeyondTrust Remote Support angemeldet, für die Sie das einmalige Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Testen von BeyondTrust Remote Support mit Azure AD](https://aad.portal.azure.com/)
