---
title: 'Tutorial: Azure Active Directory-Integration mit Check Point CloudGuard Dome9 Arc | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Check Point CloudGuard Dome9 Arc konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdaaab8257d3a79130902e1ba0466f9cf15484f4
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147225"
---
# <a name="tutorial-integrate-check-point-cloudguard-dome9-arc-with-azure-active-directory"></a>Tutorial: Integrieren von Check Point CloudGuard Dome9 Arc in Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Check Point CloudGuard Dome9 Arc in Azure Active Directory (Azure AD) integrieren. Bei der Integration von Check Point CloudGuard Dome9 Arc in Azure AD haben Sie folgende Möglichkeiten:

* Steuern Sie in Azure AD, wer Zugriff auf Check Point CloudGuard Dome9 Arc hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Check Point CloudGuard Dome9 Arc anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Check Point CloudGuard Dome9 Arc-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Check Point CloudGuard Dome9 Arc unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

## <a name="adding-check-point-cloudguard-dome9-arc-from-the-gallery"></a>Hinzufügen von Check Point CloudGuard Dome9 Arc aus dem Katalog

Zum Konfigurieren der Integration von Check Point CloudGuard Dome9 Arc in Azure AD müssen Sie Check Point CloudGuard Dome9 Arc aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Check Point CloudGuard Dome9 Arc** in das Suchfeld ein.
1. Wählen Sie **Check Point CloudGuard Dome9 Arc** im Ergebnisbereich aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Check Point CloudGuard Dome9 Arc mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Check Point CloudGuard Dome9 Arc eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Check Point CloudGuard Dome9 Arc müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
2. **[Konfigurieren von Check Point CloudGuard Dome9 Arc](#configure-check-point-cloudguard-dome9-arc)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen
5. **[Erstellen eines Check Point CloudGuard Dome9 Arc-Testbenutzers](#create-check-point-cloudguard-dome9-arc-test-user)** , um eine Entsprechung von B. Simon in Check Point CloudGuard Dome9 Arc zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Check Point CloudGuard Dome9 Arc** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://secure.dome9.com/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Sie wählen den Wert für Ihren Unternehmensnamen im dome9-Verwaltungsportal aus. Dies wird später in diesem Tutorial erläutert.

5. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://secure.dome9.com/sso/saml/<yourcompanyname>`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Antwort-URL und die tatsächliche Anmelde-URL ersetzt werden. Wenden Sie sich an das [Supportteam für den Check Point CloudGuard Dome9 Arc-Client](mailto:Dome9@checkpoint.com), um diese Werte zu erhalten. Sie können sich auch die Muster im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal ansehen.

6. Die Check Point CloudGuard Dome9 Arc-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen.

    ![image](common/edit-attribute.png)

7. Darüber hinaus wird von der Check Point CloudGuard Dome9 Arc-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen: 

    | NAME |  Quellattribut|
    | ---------------| --------------- |
    | memberof | user.assignedroles |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Lassen Sie den **Namespace** leer.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

1. Suchen Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** den Eintrag **Zertifikat (Base64)** . Klicken Sie auf **Herunterladen**, um das Zertifikat herunterzuladen, und speichern Sie es auf Ihrem Computer.

   ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Check Point CloudGuard Dome9 einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

   ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-check-point-cloudguard-dome9-arc"></a>Konfigurieren von Check Point CloudGuard Dome9 Arc

1. Melden Sie sich in einem anderen Webbrowserfenster bei der Check Point CloudGuard Dome9 Arc-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der oberen rechten Ecke auf **Profileinstellungen** und dann auf **Kontoeinstellungen**. 

    ![Check Point CloudGuard Dome9 Arc-Konfiguration](./media/dome9arc-tutorial/configure1.png)

3. Navigieren Sie zu **SSO**, und klicken Sie dann auf **AKTIVIEREN**.

    ![Check Point CloudGuard Dome9 Arc-Konfiguration](./media/dome9arc-tutorial/configure2.png)

4. Führen Sie im Abschnitt für die SSO-Konfiguration die folgenden Schritte aus:

    ![Check Point CloudGuard Dome9 Arc-Konfiguration](./media/dome9arc-tutorial/configure3.png)

    a. Geben Sie den Unternehmensnamen in das Textfeld **Konto-ID** ein. Dieser Wert ist für die Verwendung in der Antwort-URL vorgesehen, die im Abschnitt **Grundlegende SAML-Konfiguration** im Azure-Portal aufgeführt ist.

    b. Fügen Sie im Textfeld **Issuer** (Aussteller) den Wert von **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Idp endpoint url** (IdP-Endpunkt-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Öffnen Sie das heruntergeladene Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.

    e. Klicken Sie auf **Speichern**.

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

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Check Point CloudGuard Dome9 Arc gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste den Eintrag **Check Point CloudGuard Dome9 Arc** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-check-point-cloudguard-dome9-arc-test-user"></a>Erstellen eines Check Point CloudGuard Dome9 Arc-Testbenutzers

Damit sich Azure AD-Benutzer bei Check Point CloudGuard Dome9 Arc anmelden können, müssen sie in der Anwendung bereitgestellt werden. Check Point CloudGuard Dome9 Arc unterstützt die Just-In-Time-Bereitstellung. Damit diese funktioniert, muss der Benutzer jedoch eine bestimmte **Rolle** auswählen und sie dem Benutzer zuweisen.

   >[!Note]
   >Wenn Sie Informationen zum Erstellen der **Rolle** oder andere Details benötigen, wenden Sie sich an das [Supportteam für den Check Point CloudGuard Dome9 Arc-Client](mailto:Dome9@checkpoint.com).

**Führen Sie zum manuellen Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Check Point CloudGuard Dome9 Arc-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Benutzer und Rollen** und dann auf **Benutzer**.

    ![Mitarbeiter hinzufügen](./media/dome9arc-tutorial/user1.png)

3. Klicken Sie auf **BENUTZER HINZUFÜGEN**.

    ![Mitarbeiter hinzufügen](./media/dome9arc-tutorial/user2.png)

4. Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:

    ![Mitarbeiter hinzufügen](./media/dome9arc-tutorial/user3.png)

    a. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, z.B. B.Simon@contoso.com.

    b. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein, z. B. „B.“.

    c. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein, z.B. Simon.

    d. Legen Sie **SSO User** (SSO-Benutzer) auf **Ein** fest.

    e. Klicken Sie auf **ERSTELLEN**.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „Check Point CloudGuard Dome9 Arc“ auswählen, sollten Sie automatisch bei der Check Point CloudGuard Dome9 Arc-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)