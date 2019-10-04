---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Jamf Pro | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Jamf Pro konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfe426a6b3d087683e615d3212e0693b185c40f0
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212379"
---
# <a name="tutorial-azure-active-directory-sso-integration-with-jamf-pro"></a>Tutorial: Azure Active Directory-SSO-Integration mit Jamf Pro

In diesem Tutorial erfahren Sie, wie Sie Jamf Pro in Azure Active Directory (Azure AD) integrieren. Die Integration von Jamf Pro in Azure AD ermöglicht Folgendes:

* Sie können in Azure AD steuern, wer Zugriff auf Jamf Pro haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Jamf Pro anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein SSO-fähiges Jamf Pro-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Jamf Pro unterstützt **SP**- und **IdP**-initiiertes einmaliges Anmelden.

## <a name="add-jamf-pro-from-the-gallery"></a>Hinzufügen von Jamf Pro aus dem Katalog

Zum Konfigurieren der Integration von Jamf Pro in Azure AD müssen Sie Jamf Pro aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit Ihrem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Bereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff *Jamf Pro* in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Jamf Pro** aus, und fügen Sie die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-sso-in-azure-ad-for-jamf-pro"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD für Jamf Pro

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Jamf Pro mithilfe eines Testbenutzers namens B.Simon. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Jamf Pro eingerichtet werden.

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Jamf Pro.

1. [Konfigurieren Sie das einmalige Anmelden in Azure AD](#configure-sso-in-azure-ad), damit Ihre Benutzer dieses Feature verwenden können.
    1. [Erstellen Sie einen Azure AD-Testbenutzer](#create-an-azure-ad-test-user), um das einmalige Anmelden von Azure AD mit dem Konto „B.Simon“ zu testen.
    1. [Weisen Sie den Azure AD-Testbenutzer zu](#assign-the-azure-ad-test-user), damit B.Simon das einmalige Anmelden in Azure AD verwenden kann.
1. [Konfigurieren Sie das einmalige Anmelden in Jamf Pro](#configure-sso-in-jamf-pro), um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
    1. [Erstellen Sie einen Jamf Pro-Testbenutzer](#create-a-jamf-pro-test-user), um in Jamf Pro ein Pendant von B.Simon zu erhalten, das mit der Benutzerdarstellung in Azure AD verknüpft ist.
1. [Testen Sie die SSO-Konfiguration](#test-the-sso-configuration), um sich zu vergewissern, dass sie funktioniert.

## <a name="configure-sso-in-azure-ad"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Jamf Pro** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** das Stiftsymbol für **Grundlegende SAML-Konfiguration** aus, um die Einstellungen zu bearbeiten.

   ![Bearbeiten Sie die Seite mit der grundlegenden SAML-Konfiguration.](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte für die folgenden Felder ein, wenn Sie die Anwendung im **IdP-initiierten** Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/metadata`.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com/saml/SSO`.

1. Wählen Sie **Zusätzliche URLs festlegen** aus. Wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten, geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.jamfcloud.com`.

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Den tatsächlichen Wert des Bezeichners finden Sie im Abschnitt **Single Sign-On** (Einmaliges Anmelden) des Jamf Pro-Portals. Dies wird später in diesem Tutorial beschrieben. Sie können den tatsächlichen Wert für die Unterdomäne aus dem Bezeichnerwert extrahieren und diese Information als Ihre Anmelde- und Antwort-URL verwenden. Sie können sich auch das Format im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Wählen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** die Schaltfläche **Kopieren** aus, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das SAML-Signaturzertifikat](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer namens B.Simon.

1. Wählen Sie im linken Bereich des Azure-Portals Folgendes aus: **Azure Active Directory** > **Benutzer** > **Alle Benutzer**.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** Folgendes ein: [Name]@[Unternehmensdomäne].[Erweiterung]. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt gewähren Sie B.Simon Zugriff auf Jamf Pro.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Jamf Pro** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Benutzer und Gruppen auswählen](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Auswählen der Schaltfläche „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste die Option **B.Simon** und anschließend am unteren Bildschirmrand die Schaltfläche **Auswählen** aus.
1. Falls Sie in der SAML-Assertion einen Rollenwert erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer aus. Wählen Sie anschließend im unteren Bildschirmbereich die Schaltfläche **Auswählen** aus.
1. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Schaltfläche **Zuweisen**.

## <a name="configure-sso-in-jamf-pro"></a>Konfigurieren des einmaligen Anmeldens in Jamf Pro 

1. Wenn Sie die Konfiguration in Jamf Pro automatisieren möchten, installieren Sie die **Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“** , indem Sie **Erweiterung installieren** auswählen.

    ![Seite der Browsererweiterung zur sicheren Anmeldung bei „Meine Apps“](common/install-myappssecure-extension.png)

2. Wählen Sie nach dem Hinzufügen der Browsererweiterung **Jamf Pro einrichten** aus. Melden Sie sich bei der Jamf Pro-Anwendung unter Verwendung der Administratoranmeldeinformationen an. Die Browsererweiterung konfiguriert die Anwendung automatisch und automatisiert die Schritte 3 bis 7.

    ![Einrichtungskonfigurationsseite in Jamf Pro](common/setup-sso.png)

3. Falls Sie Jamf Pro manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der Jamf Pro-Unternehmenswebsite als Administrator an. Führen Sie dann die folgenden Schritte aus:

4. Wählen Sie rechts oben auf der Seite das **Einstellungssymbol** aus.

    ![Auswählen des Einstellungssymbols in Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Wählen Sie **Single Sign-On** (Einmaliges Anmelden) aus.

    ![Auswählen von „Single Sign-On“ (Einmaliges Anmelden) in Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Gehen Sie auf der Seite **Single Sign-On** (Einmaliges Anmelden) wie folgt vor:

    ![Seite „Single Sign-On“ (Einmaliges Anmelden) in Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Aktivieren Sie das Kontrollkästchen **Enable Single Sign-On Authentication** (SSO-Authentifizierung aktivieren).

    b. Wählen Sie im Dropdownmenü **IDENTITY PROVIDER** (IDENTITÄTSANBIETER) die Option **Other** (Sonstige) aus.

    c. Geben Sie im Feld **OTHER PROVIDER** (ANDERER ANBIETER) die Zeichenfolge **Azure AD** ein.

    d. Kopieren Sie den Wert im Feld **ENTITY ID** (Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Feld **Bezeichner (Entitäts-ID)** ein.

    > [!NOTE]
    > Verwenden Sie den Wert im Feld `<SUBDOMAIN>`, um die Anmelde-URL und die Antwort-URL im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** zu vervollständigen.

    e. Wählen Sie im Dropdownmenü **IDENTITY PROVIDER METADATA SOURCE** (IDENTITÄTSANBIETER-METADATENQUELLE) die Option **Metadata URL** (Metadaten-URL) aus. Fügen Sie im daraufhin angezeigten Feld den Wert der **App-Verbundmetadaten-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

7. Scrollen Sie auf der gleichen Seite nach unten zum Abschnitt **User Mapping** (Benutzerzuordnung). Führen Sie dann die folgenden Schritte aus:   

    ![Abschnitt „User Mapping“ (Benutzerzuordnung) auf der Seite für einmaliges Anmelden in Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Wählen Sie unter **IDENTITY PROVIDER USER MAPPING** (BENUTZERZUORDNUNG DES IDENTITÄTSANBIETERS) die Option **NameID** aus. Diese Option ist standardmäßig auf **NameID** festgelegt. Sie können aber auch ein benutzerdefiniertes Attribut definieren.

    b. Wählen Sie unter **USER MAPPING JAMF PRO** (BENUTZERZUORDNUNG: JAMF PRO) die Option **Email** (E-Mail) aus. Jamf Pro ordnet vom IdP gesendete SAML-Attribute zunächst nach Benutzern und dann nach Gruppen zu. Wenn ein Benutzer versucht, auf Jamf Pro zuzugreifen, ruft Jamf Pro vom Identitätsanbieter Informationen zu dem Benutzer ab und gleicht sie mit allen Jamf Pro-Benutzerkonten ab. Wird das eingehende Benutzerkonto nicht gefunden, versucht Jamf Pro einen Abgleich anhand des Gruppennamens.

    c. Fügen Sie den Wert `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` in das Feld **IDENTITY PROVIDER GROUP ATTRIBUTE NAME** (ATTRIBUTNAME DER IDENTITÄTSANBIETERGRUPPE) ein.

    d. Aktivieren Sie das Kontrollkästchen **Allow users to bypass the Single Sign-On authentication** (Umgehung der SSO-Authentifizierung für Benutzer zulassen). Dadurch werden Benutzer zur Authentifizierung nicht auf die Anmeldeseite des Identitätsanbieters umgeleitet, sondern können sich stattdessen direkt bei Jamf Pro anmelden. Wenn ein Benutzer versucht, sich über den Identitätsanbieter auf Jamf Pro zuzugreifen, erfolgen eine IdP-initiierte SSO-Authentifizierung und -Autorisierung.

    e. Wählen Sie **Speichern** aus.

### <a name="create-a-jamf-pro-test-user"></a>Erstellen eines Jamf Pro-Testbenutzers

Damit sich Azure AD-Benutzer bei Jamf Pro anmelden können, müssen sie in Jamf Pro bereitgestellt werden. Die Bereitstellung in Jamf Pro ist eine manuelle Aufgabe.

Gehen Sie zum Bereitstellen eines Benutzerkontos wie folgt vor:

1. Melden Sie sich bei der Jamf Pro-Unternehmenswebsite als Administrator an.

2. Wählen Sie oben rechts auf der Seite das Symbol für die **Einstellungen** aus.

    ![Einstellungssymbol in Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Wählen Sie **Jamf Pro User Accounts & Groups** (Jamf Pro-Benutzerkonten und Gruppen) aus.

    ![Symbol für Jamf Pro-Benutzerkonten und Gruppen in den Einstellungen von Jamf Pro](./media/jamfprosamlconnector-tutorial/user1.png)

4. Wählen Sie **Neu**aus.

    ![Seite mit Systemeinstellungen für Jamf Pro-Benutzerkonten und Gruppen](./media/jamfprosamlconnector-tutorial/user2.png)

5. Wählen Sie **Create Standard Account** (Standardkonto erstellen) aus.

    ![Option „Create Standard Account“ (Standardkonto erstellen) auf der Seite für Jamf Pro-Benutzerkonten und Gruppen](./media/jamfprosamlconnector-tutorial/user3.png)

6. Gehen Sie im Dialogfeld **New Account** (Neues Konto) wie folgt vor:

    ![Einrichtungsoptionen für ein neues Konto in den Systemeinstellungen von Jamf Pro](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Geben Sie im Feld **USERNAME** (BENUTZERNAME) den vollständigen Namen des Testbenutzers (`Britta Simon`) ein.

    b. Wählen Sie geeignete Optionen für **ACCESS LEVEL** (ZUGRIFFSEBENE), **PRIVILEGE SET** (BERECHTIGUNGSGRUPPE) und **ACCESS STATUS** (ZUGRIFFSSTATUS) aus.

    c. Geben Sie im Feld **FULL NAME** (VOLLSTÄNDIGER NAME) den Namen `Britta Simon` ein.

    d. Geben Sie im Feld **EMAIL ADDRESS** (E-MAIL-ADRESSE) die E-Mail-Adresse des Kontos von Britta Simon ein.

    e. Geben Sie im Feld **PASSWORD** (KENNWORT) das Kennwort des Benutzers ein.

    f. Geben Sie im Feld **VERIFY PASSWORD** (KENNWORT ÜBERPRÜFEN) erneut das Kennwort des Benutzers ein.

    g. Wählen Sie **Speichern** aus.

## <a name="test-the-sso-configuration"></a>Testen der SSO-Konfiguration 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Jamf Pro“ auswählen, sollten Sie automatisch bei dem Jamf Pro-Konto angemeldet werden, für das Sie einmaliges Anmelden konfiguriert haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist bedingter Zugriff?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Jamf Pro mit Azure AD ausprobieren](https://aad.portal.azure.com/)

