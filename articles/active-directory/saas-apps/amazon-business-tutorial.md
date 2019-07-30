---
title: 'Tutorial: Azure Active Directory-Integration mit Amazon Business | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Amazon Business konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b2597a66-afd9-4f11-b14b-646b597bb6c2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d7ac085beaa85a7ddf3a6c3bfc61820e8e5a63ea
ms.sourcegitcommit: 5604661655840c428045eb837fb8704dca811da0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68496565"
---
# <a name="tutorial-integrate-amazon-business-with-azure-active-directory"></a>Tutorial: Integrieren von Amazon Business in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Amazon Business in Azure Active Directory (Azure AD) integrieren. Die Integration von [Amazon Business](https://www.amazon.com/b2b/info/amazon-business?layout=landing) in Azure AD hat folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Amazon Business haben soll.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Amazon Business anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie kein Abonnement besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige kostenlose Testversion erhalten.
* Ein für einmaliges Anmelden (Single Sign-On, SSO) geeignetes Amazon Business-Abonnement. Navigieren Sie zur Seite [Amazon Business](https://www.amazon.com/business/register/org/landing?ref_=ab_reg_mlp), um ein Amazon Business-Konto zu erstellen.

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einem bereits vorhandenen Amazon Business-Konto.

* Amazon Business unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* Amazon Business unterstützt die **Just-In-Time**-Benutzerbereitstellung.

## <a name="adding-amazon-business-from-the-gallery"></a>Hinzufügen von Amazon Business über den Katalog

Um die Integration von Amazon Business in Azure AD zu konfigurieren, müssen Sie Amazon Business über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Amazon Business** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich die Option **Amazon Business** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Amazon Business mithilfe eines Testbenutzers namens **B.Simon**.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Amazon Business zu testen und zu konfigurieren:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Amazon Business](#configure-amazon-business-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
5. **[Erstellen eines Testbenutzers für Amazon Business](#create-amazon-business-test-user)** , um in Amazon Business ein Pendant von B.Simon zu erhalten, das mit der Benutzerdarstellung in Azure AD verknüpft ist.
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Amazon Business** den Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    1. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL in einem der folgenden Formate ein:
    
       | | |
       |-|-|
       | `https://www.amazon.com`|
       | `https://www.amazon.co.jp`|
       | `https://www.amazon.de`|

    1. Geben Sie im Textfeld **Antwort-URL** eine URL in einem der folgenden Formate ein:
    
       | | |
       |-|-|
       | `https://www.amazon.com/bb/feature/sso/action/3p_redirect?idpid={idpid}`|
       | `https://www.amazon.co.jp/bb/feature/sso/action/3p_redirect?idpid={idpid}`|
       | `https://www.amazon.de/bb/feature/sso/action/3p_redirect?idpid={idpid}`|

       > [!NOTE]
       > Der Wert der Antwort-URL entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Den Wert `<idpid>` finden Sie im SSO-Konfigurationsabschnitt von Amazon Business (wird später in diesem Tutorial erläutert). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL ein: `https://www.amazon.com/`.

1. Der folgende Screenshot zeigt die Liste der Standardattribute. Bearbeiten Sie die Attribute, indem Sie im Abschnitt **Benutzerattribute und Ansprüche** auf das Symbol **Bearbeiten** klicken.

    ![Attribute](media/amazon-business-tutorial/map-attribute3.png)

1. Bearbeiten Sie Attribute, und kopieren Sie den **** Namespacewert dieser Attribute in den Editor.

    ![Attribute](media/amazon-business-tutorial/map-attribute4.png)

1. Darüber hinaus wird von der Amazon Business-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Gruppenansprüche** im Abschnitt **Benutzerattribute und Ansprüche** die folgenden Schritte aus:

    a. Klicken Sie auf den **Stift** neben **Im Anspruch zurückgegebene Gruppen**.

    ![image](./media/amazon-business-tutorial/config04.png)

    ![image](./media/amazon-business-tutorial/config05.png)

    b. Wählen Sie **Alle Gruppen** aus der Optionsfeldliste aus.

    c. Wählen Sie unter **Quellattribut** die Option **Gruppen-ID** aus.

    d. Aktivieren Sie das Kontrollkästchen **Name des Gruppenanspruchs anpassen**, und geben Sie den Gruppennamen gemäß den Anforderungen Ihrer Organisation ein.

    e. Klicken Sie auf **Speichern**.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Metadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Amazon Business einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-amazon-business-sso"></a>Konfigurieren des einmaligen Anmeldens von Amazon Business

1. Melden Sie sich in einem anderen Webbrowserfenster als Administrator bei der Amazon Business-Unternehmenswebsite an.

1. Klicken Sie auf das **** Benutzerprofil, und wählen Sie **Business Settings** (Unternehmenseinstellungen) aus.

    ![Benutzerprofil](media/amazon-business-tutorial/user-profile.png)

1. Wählen Sie im Assistenten **** für Systemintegrationen die Option für **** einmaliges Anmelden (Single Sign-On, SSO) aus.

    ![Einmaliges Anmelden (Single Sign-On, SSO)](media/amazon-business-tutorial/sso-settings.png)

1. Wählen Sie im Einrichtungs-Assistenten **** für einmaliges Anmelden den Anbieter gemäß den Anforderungen Ihrer Organisation aus, und klicken Sie auf **Next** (Weiter).

    ![Standardgruppe](media/amazon-business-tutorial/default-group1.png)

1. Wählen Sie im Assistenten **** zum Festlegen der Standardwerte für neue Benutzerkonten die Standardgruppe (**Default Group**) und anschließend die Standardkaufrolle (**Default Buying Role**) gemäß der Benutzerrolle in Ihrer Organisation aus. Klicken Sie anschließend auf **Next** (Weiter).

    ![Standardgruppe](media/amazon-business-tutorial/dafault-group2.png)

1. Klicken Sie im Assistenten **** zum Hochladen Ihrer Metadatendatei auf **Browse** (Durchsuchen), um die **** Metadaten-XML-Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie anschließend auf **Upload** (Hochladen).

    ![Verbindungsdaten](media/amazon-business-tutorial/connection-data1.png)

1. Nach dem Hochladen der heruntergeladenen Metadatendatei werden die Felder im Abschnitt für die Verbindungsdaten **** automatisch aufgefüllt. Klicken Sie danach auf **Next** (Weiter).

    ![Verbindungsdaten](media/amazon-business-tutorial/connection-data2.png)

1. Klicken Sie im Assistenten **** zum Hochladen Ihrer Attributanweisung auf **Skip** (Überspringen).

    ![Attribute](media/amazon-business-tutorial/map-attribute1.png)

1. Fügen Sie im Assistenten **** für die Attributzuordnung die Anforderungsfelder hinzu, indem Sie auf die Option **+ Add a field** (+ Feld hinzufügen) klicken. Fügen Sie die Attributwerte (einschließlich des Namespace, den Sie aus dem Abschnitt **Benutzerattribute und Ansprüche** des Azure-Portals kopiert haben) dem Feld **SAML AttributeName** (SAML-Attributname) hinzu, und klicken Sie auf **Next** (Weiter).

    ![Attribute](media/amazon-business-tutorial/map-attribute2.png)

1. Klicken Sie im Assistenten **** für Amazon-Verbindungsdaten auf **Next** (Weiter).

    ![Verbindung](media/amazon-business-tutorial/amazon-connect.png)

1. Überprüfen Sie den Status **** der konfigurierten Schritte, und klicken Sie auf **Start testing** (Test starten).

    ![Verbindung](media/amazon-business-tutorial/sso-connection1.png)

1. Klicken Sie im Assistenten **** zum Testen der SSO-Verbindung auf **Test** (Testen).

    ![Verbindung](media/amazon-business-tutorial/sso-connection2.png)

1. Kopieren Sie im Assistenten **** für die IDP-initiierte URL vor dem Klicken auf **Activate** (Aktivieren) den Wert, der **idpid** zugewiesen ist, und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** unter **Antwort-URL** in den Parameter **idpid** ein.

    ![Verbindung](media/amazon-business-tutorial/sso-connection3.png)

1. Aktivieren Sie im Assistenten **** für die SSO-Aktivierung das Kontrollkästchen **I have fully tested SSO and am ready to go live** (Ich habe SSO vollständig getestet und bin bereit zur Aktivierung), und klicken Sie anschließend auf **Switch to active** (Aktivieren).

    ![Verbindung](media/amazon-business-tutorial/sso-connection4.png)

1. Abschließend wird im Abschnitt **** mit den SSO-Verbindungsdetails der **** Status **Active** (Aktiv) angezeigt.

    ![Verbindung](media/amazon-business-tutorial/sso-connection5.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

> [!NOTE]
> Administratoren müssen die Testbenutzer bei Bedarf in ihrem Mandanten erstellen. Die folgenden Schritte zeigen die Erstellung eines Testbenutzers.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Create**.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Erstellen einer Azure AD-Sicherheitsgruppe im Azure-Portal

1. Klicken Sie auf **Azure Active Directory > Alle Gruppen**.

    ![Erstellen einer Azure AD-Sicherheitsgruppe](./media/amazon-business-tutorial/all-groups-tab.png)

1. Klicken Sie auf **Neue Gruppe**:

    ![Erstellen einer Azure AD-Sicherheitsgruppe](./media/amazon-business-tutorial/new-group-tab.png)

1. Geben Sie **Gruppentyp**, **Gruppenname**, **Gruppenbeschreibung** und **Mitgliedschaftstyp** an. Klicken Sie auf den Pfeil, um Mitglieder auszuwählen, und suchen Sie nach dem Mitglied, das Sie der Gruppe hinzufügen möchten, oder klicken Sie auf das gewünschte Mitglied. Klicken Sie auf **Auswählen**, um die ausgewählten Mitglieder hinzuzufügen, und klicken Sie anschließend auf **Erstellen**.

    ![Erstellen einer Azure AD-Sicherheitsgruppe](./media/amazon-business-tutorial/group-information.png)

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B.Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie dem Benutzer Zugriff auf Amazon Business gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Amazon Business** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

    >[!NOTE]
    > Wenn Sie die Benutzer in Azure AD nicht zuweisen, tritt der folgende Fehler auf:

    ![Link „Benutzer hinzufügen“](media/amazon-business-tutorial/assign-user.png)

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Zuweisen der Azure AD-Sicherheitsgruppe im Azure-Portal

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** > **Amazon Business** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Amazon Business** ein, und wählen Sie die entsprechende Option aus.

    ![Link „Amazon Business“ in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf **Benutzer hinzufügen**.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Suchen Sie nach der gewünschten Sicherheitsgruppe, und klicken Sie auf die Gruppe, um sie dem Abschnitt „Mitglieder auswählen“ hinzuzufügen. Klicken Sie auf **Auswählen** und anschließend auf **Zuweisen**.

    ![Suchen nach der Sicherheitsgruppe](./media/amazon-business-tutorial/assign-group.png)

    > [!NOTE]
    > Vergewissern Sie sich anhand der Benachrichtigungen auf der Menüleiste, dass die Gruppe erfolgreich der Enterprise-Anwendung im Azure-Portal zugewiesen wurde.

### <a name="create-amazon-business-test-user"></a>Erstellen eines Testbenutzers für Amazon Business

In diesem Abschnitt wird in Amazon Business ein Benutzer namens B.Simon erstellt. Amazon Business unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Amazon Business vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Amazon Business“ klicken, sollten Sie automatisch bei der Amazon Business-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
