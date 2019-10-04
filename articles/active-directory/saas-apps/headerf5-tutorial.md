---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit F5 | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und F5 konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 59a87abb-1ec1-4438-be07-5b115676115f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea4167f5a8f4e29641a999c72f57b368190a34e0
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70165523"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit F5

In diesem Tutorial erfahren Sie, wie Sie F5 in Azure Active Directory (Azure AD) integrieren. Die Integration von F5 in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf F5 hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei F5 anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* F5-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Einmaliges Anmelden von F5 kann auf drei unterschiedliche Arten konfiguriert werden:

- [Konfigurieren des einmaligen Anmeldens von F5 für eine headerbasierte Anwendung](#configure-f5-single-sign-on-for-header-based-application)

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Kerberos-Anwendung](kerbf5-tutorial.md)

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Advanced Kerberos-Anwendung](advance-kerbf5-tutorial.md)

## <a name="adding-f5-from-the-gallery"></a>Hinzufügen von F5 aus dem Katalog

Zum Konfigurieren der Integration von F5 in Azure AD müssen Sie F5 aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **F5** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **F5** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für F5

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit F5 mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in F5 eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit F5 die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für F5](#configure-f5-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines F5-Testbenutzers](#create-f5-test-user)** , um eine Entsprechung von B. Simon in F5 zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **F5** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP**-initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://<YourCustomFQDN>.f5.com/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<YourCustomFQDN>.f5.com/`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Diese Werte erhalten Sie vom [Supportteam für den F5-Client](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45). Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **F5 einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf F5 gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **F5** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-f5-sso"></a>Konfigurieren des einmaligen Anmeldens für F5

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Kerberos-Anwendung](kerbf5-tutorial.md)

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Advanced Kerberos-Anwendung](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-header-based-application"></a>Konfigurieren des einmaligen Anmeldens von F5 für eine headerbasierte Anwendung

1. Melden Sie sich in einem neuen Webbrowserfenster bei der Unternehmenswebsite von F5 (headerbasiert) als Administrator an, und führen Sie die folgenden Schritte aus:

1. Sie müssen das Metadatenzertifikat in F5 (headerbasiert) importieren. Es wird später im Setupvorgang verwendet. Navigieren Sie zu **System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (System > Zertifikatverwaltung > Zertifikatverwaltung für Datenverkehr > SSL-Zertifikatliste). Klicken Sie in der rechten Ecke auf **Import** (Importieren).

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure12.png)
 
1. Darüber hinaus muss für den Hostnamen (`headerapp.superdemo.live`) ein **SSL-Zertifikat** angegeben werden. In diesem Beispiel wird ein Platzhalterzertifikat verwendet.

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure13.png)

1. Klicken Sie in F5 (headerbasiert) auf **Access > Guided Configuration > Federation > SAML Service Provider** (Zugriff > Interaktive Konfiguration > Verbund > SAML-Dienstanbieter).

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure01.png)

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure02.png)
 
1. Geben Sie unter **Entity ID** (Entitäts-ID) die Entitäts-ID ein. (Verwenden Sie den gleichen Wert wie in der Azure AD-Anwendungskonfiguration.)

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure03.png) 

1. Erstellen Sie einen neuen virtuellen Server, und geben Sie einen Wert für **Destination Address** (Zieladresse) ein. Die Angabe für **Redirect Port** (Umleitungsport) ist optional. Wählen Sie das **Platzhalterzertifikat** (oder das für die Anwendung hochgeladene **Zertifikat**) aus, das Sie zuvor hochgeladen haben. Wählen Sie anschließend unter **Associated Private Key** (Zugeordneter privater Schlüssel) einen Wert aus.

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure04.png) 

1. Laden Sie die **Konfigurationsmetadaten** hoch, und geben Sie einen neuen **Namen für den SAML-IDP-Connector** sowie das Verbundzertifikat an, das zuvor hochgeladen wurde.

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure05.png)
 
1. Wählen Sie **Create New** (Neu erstellen) aus, um einen neuen Back-End-App-Pool zu erstellen, und geben Sie die **IP-Adressen** der Back-End-Anwendungsserver an.

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure06.png)

1. Wählen Sie unter „Single Sign-On“ (Einmaliges Anmelden) die Option **HTTP header-based** (HTTP-Header-basiert) aus. Sie können basierend auf Ihrer Anwendung weitere Header hinzufügen. Eine Liste der SAML-Sitzungsvariablen finden Sie im Anhang.

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure07.png) 

1. Eine ausführliche Dokumentation zu den Eigenschaften für Endpunktüberprüfungen (**Endpoint Check Properties**) erhalten Sie vom [Supportteam für den F5-Client (headerbasiert)](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45).

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure08.png)

1. Eine ausführliche Dokumentation zu den Eigenschaften für die Sitzungsverwaltung (**Session Management Properties**) erhalten Sie vom [Supportteam für den F5-Client (headerbasiert)](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45).

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure09.png)

1. Sehen Sie sich die **Zusammenfassung** an, und klicken Sie auf **Deploy** (Bereitstellen).

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure10.png)

    ![Konfiguration von F5 (headerbasiert)](./media/headerf5-tutorial/configure11.png)

### <a name="create-f5-test-user"></a>Erstellen eines F5-Testbenutzers

In diesem Abschnitt erstellen Sie in F5 einen Benutzer namens B. Simon. Wenden Sie sich an das  [Supportteam für den F5-Client](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45), um die Benutzer auf der F5-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „F5“ klicken, sollten Sie automatisch bei der F5-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [F5 mit Azure AD ausprobieren](https://aad.portal.azure.com/)

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Kerberos-Anwendung](kerbf5-tutorial.md)

- [Konfigurieren des einmaligen Anmeldens von F5 für eine Advanced Kerberos-Anwendung](advance-kerbf5-tutorial.md)

