---
title: 'Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Qlik Sense Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e72ec4f9c512f6525f790d555794c1a120ac07c9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093425"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise

In diesem Tutorial erfahren Sie, wie Sie Qlik Sense Enterprise in Azure Active Directory (Azure AD) integrieren.
Die Integration von Qlik Sense Enterprise in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Qlik Sense Enterprise hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Qlik Sense Enterprise anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Qlik Sense Enterprise konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Qlik Sense Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Qlik Sense Enterprise unterstützt das **SP-initiierte** einmalige Anmelden.

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Hinzufügen von Qlik Sense Enterprise aus dem Katalog

Zum Konfigurieren der Integration von Qlik Sense Enterprise in Azure AD müssen Sie Qlik Sense Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Qlik Sense Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Qlik Sense Enterprise** ein, wählen Sie im Ergebnisbereich **Qlik Sense Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![Qlik Sense Enterprise in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Qlik Sense Enterprise mithilfe einer Testbenutzerin namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Qlik Sense Enterprise eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Qlik Sense Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für Qlik Sense Enterprise](#configure-qlik-sense-enterprise-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines Qlik Sense Enterprise-Testbenutzers](#create-qlik-sense-enterprise-test-user)** , um eine Entsprechung von Britta Simon in Qlik Sense Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Qlik Sense Enterprise die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Qlik Sense Enterprise** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Qlik Sense Enterprise](common/sp-identifier-reply.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Qlik Sense Fully Qualifed Hostname>:4443/azure/hub`.

    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein:

    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|
    | |

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

    `https://<Qlik Sense Fully Qualifed Hostname>:4443/samlauthn/`

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit der tatsächlichen Anmelde-URL, dem Bezeichner und der Antwort-URL. Diese werden später in diesem Tutorial erläutert. Wenden Sie sich alternativ an das [Clientsupportteam von Qlik Sense Enterprise](https://www.qlik.com/us/services/support), um diese Werte zu erhalten.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Verbundmetadaten-XML** aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

### <a name="configure-qlik-sense-enterprise-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für Qlik Sense Enterprise

1. Bereiten Sie die XML-Datei mit den Verbundmetadaten für das Hochladen auf den Qlik Sense-Server vor.

    > [!NOTE]
    > Vor dem Hochladen der IdP-Metadaten auf den Qlik Sense-Server muss die Datei bearbeitet werden, um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen.

    ![QlikSense][qs24]

    a. Öffnen Sie die aus dem Azure-Portal heruntergeladene Datei „FederationMetaData.xml“ in einem Text-Editor.

    b. Suchen Sie nach dem Wert **RoleDescriptor**.  Es sind vier Einträge vorhanden (jeweils zwei Paare mit öffnenden und schließenden Elementtags).

    c. Löschen Sie die RoleDescriptor-Tags und alle Informationen dazwischen aus der Datei.

    d. Speichern Sie die Datei, und bewahren Sie sie auf, um sie zu einem späteren Zeitpunkt in diesem Dokument verwenden zu können.

2. Navigieren Sie als Benutzer mit Rechten zum Erstellen von Konfigurationen für virtuelle Proxys zur Qlik Sense Qlik Management Console (QMC).

3. Klicken Sie in der QMC auf das Menüelement **Virtual Proxy**.

    ![QlikSense][qs6]

4. Klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Create new** (Neu erstellen).

    ![QlikSense][qs7]

5. Der Bearbeitungsbildschirm für den virtuellen Proxy wird angezeigt.  Auf der rechten Seite des Bildschirms befindet sich ein Menü zum Anzeigen von Konfigurationsoptionen.

    ![QlikSense][qs9]

6. Geben Sie bei aktivierter Menüoption „Identification“ die identifizierenden Informationen für die Azure-Konfiguration des virtuellen Proxys ein.

    ![QlikSense][qs8]  

    a. Das Feld **Description** (Beschreibung) enthält einen Anzeigenamen für die Konfiguration des virtuellen Proxys.  Geben Sie einen Wert als Beschreibung ein.

    b. Mit dem Feld **Prefix** wird der Endpunkt des virtuellen Proxys für die Verbindung mit Qlik Sense per einmaligem Anmelden von Azure AD angegeben.  Geben Sie einen eindeutigen Präfixnamen für diesen virtuellen Proxy ein.

    c. **Session inactivity timeout (minutes)** (Timeout bei Sitzungsinaktivität (Minuten)) ist das Timeout für Verbindungen über diesen virtuellen Proxy.

    d. Unter **Session cookie header name** (Headername für Sitzungscookie) wird der Cookiename zum Speichern des Sitzungsbezeichners für die Qlik Sense-Sitzung angegeben, die einem Benutzer nach der erfolgreichen Authentifizierung zugeordnet wird.  Dieser Name muss eindeutig sein.

7. Klicken Sie auf die Menüoption „Authentication“, um die entsprechenden Informationen anzuzeigen.  Der Bildschirm „Authentication“ wird angezeigt.

    ![QlikSense][qs10]

    a. Mit dem Dropdownmenü **Anonymous access mode** wird festgelegt, ob anonyme Benutzer über den virtuellen Proxy auf Qlik Sense zugreifen können.  Die Standardoption ist „No anonymous user“.

    b. Mit dem Dropdownmenü **Authentication method** (Authentifizierungsmethode) wird festgelegt, welches Authentifizierungsschema vom virtuellen Proxy verwendet wird.  Wählen Sie in der Dropdownliste die Option „SAML“ aus.  Daraufhin werden weitere Optionen angezeigt.

    c. Geben Sie im Feld **SAML host URI** den Hostnamen ein, den Benutzer eingeben müssen, um über diesen virtuellen SAML-Proxy auf Qlik Sense zuzugreifen.  Der Hostname ist der URI des Qlik Sense-Servers.

    d. Geben Sie im Feld **SAML entity ID**den gleichen Wert wie im Feld „SAML host URI“ ein.

    e. Unter **SAML IdP metadata** (SAML-IdP-Metadaten) wird die Datei gewählt, die Sie zuvor im Abschnitt **Edit Federation Metadata from Azure AD Configuration** (Verbundmetadaten der Azure AD-Konfiguration bearbeiten) bearbeitet haben.  **Vor dem Hochladen der IdP-Metadaten muss die Datei bearbeitet werden** , um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen.  **Verwenden Sie die obige Anleitung, falls Sie die Datei noch bearbeiten müssen.**  Wenn die Datei bearbeitet wurde, können Sie auf die Schaltfläche zum Durchsuchen klicken und die bearbeitete Metadatendatei auswählen, um sie für die Konfiguration des virtuellen Proxys hochzuladen.

    f. Geben Sie den Attributnamen oder den Schemaverweis für das SAML-Attribut ein, das für die **UserID** steht, die von Azure AD an den Qlik Sense-Server gesendet wird.  Informationen zum Schemaverweis sind auf den Azure-App-Bildschirmen zur Post-Konfiguration verfügbar.  Geben Sie zum Verwenden des Namensattributs `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    g. Geben Sie den Wert für das Benutzerverzeichnis ( **user directory** ) ein, das Benutzern zugeordnet wird, wenn diese die Authentifizierung gegenüber dem Qlik Sense-Server über Azure AD durchführen.  Hartcodierte Werte müssen in **eckige Klammern []** gesetzt werden.  Geben Sie den Namen des Attributs in diesem Textfeld **ohne** eckige Klammern ein, um ein Attribut zu verwenden, das in der Azure AD-SAML-Assertion gesendet wird.

    h. Unter **SAML signing algorithm** wird die Service Provider-Zertifikatsignatur (hier: Qlik Sense-Server) für die Konfiguration des virtuellen Proxys festgelegt.  Ändern Sie den SAML-Signaturalgorithmus in **SHA-256**, wenn für den Qlik Sense-Server ein vertrauenswürdiges Zertifikat verwendet wird, das per Microsoft Enhanced RSA und AES Cryptographic Provider generiert wurde.

    i. Im Abschnitt „SAML attribute mapping“ können weitere Attribute, z.B. Gruppen, zur Verwendung in Sicherheitsregeln an Qlik Sense gesendet werden.

8. Klicken Sie auf die Menüoption **LOAD BALANCING**, um die entsprechenden Informationen anzuzeigen.  Der Bildschirm „Load Balancing“ wird angezeigt.

    ![QlikSense][qs11]

9. Klicken Sie auf die Schaltfläche **Add new server node** (Neuen Serverknoten hinzufügen), wählen Sie den Engine-Knoten oder Knoten aus, an die von Qlik Sense zu Lastenausgleichszwecken Sitzungen gesendet werden, und klicken Sie auf die Schaltfläche **Add** (Hinzufügen).

    ![QlikSense][qs12]

10. Klicken Sie auf die Menüoption „Advanced“, um die entsprechenden Informationen anzuzeigen. Der Bildschirm „Advanced“ wird angezeigt.

    ![QlikSense][qs13]

    Unter „Host white list“ sind die Hostnamen angegeben, die akzeptiert werden, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt wird.  **Geben Sie den Hostnamen ein, der von Benutzern angegeben werden muss, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt werden soll.** Der Hostname ist der gleiche Wert wie der SAML-Host-URI ohne „https://“.

11. Klicken Sie auf die Schaltfläche **Apply** (Anwenden).

    ![QlikSense][qs14]

12. Klicken Sie auf „OK“, um die Warnmeldung mit dem Hinweis zu akzeptieren, dass mit dem virtuellen Proxy verknüpfte Proxys neu gestartet werden.

    ![QlikSense][qs15]

13. Rechts auf dem Bildschirm wird das Menü „Associated items“ angezeigt.  Klicken Sie auf die Menüoption **Proxies**.

    ![QlikSense][qs16]

14. Der Proxybildschirm wird angezeigt.  Klicken Sie unten auf die Schaltfläche **Link**, um einen Proxy mit dem virtuellen Proxy zu verknüpfen.

    ![QlikSense][qs17]

15. Wählen Sie den Proxyknoten aus, der diese Verbindung mit dem virtuellen Proxy unterstützen soll, und klicken Sie auf die Schaltfläche **Link**.  Nach dem Verknüpfen wird der Proxy in der Liste mit den zugeordneten Proxys aufgeführt.

    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

16. Nach ungefähr fünf bis zehn Sekunden wird die Meldung „Refresh QMC“ angezeigt.  Klicken Sie auf die Schaltfläche **Refresh QMC** (QMC aktualisieren).

    ![QlikSense][qs20]

17. Klicken Sie nach der QMC-Aktualisierung auf das Menüelement **Virtual proxies**. Der Eintrag mit dem neuen virtuellen SAML-Proxy wird in der Tabelle auf dem Bildschirm angezeigt.  Klicken Sie einmal auf den Eintrag für den virtuellen Proxy.

    ![QlikSense][qs51]

18. Unten auf dem Bildschirm wird die Schaltfläche „Download SP metadata“ aktiviert.  Klicken Sie auf die Schaltfläche **Download SP metadata** (SP-Metadaten herunterladen), um die Metadaten in einer Datei zu speichern.

    ![QlikSense][qs52]

19. Öffnen Sie die SP-Metadatendatei.  Sehen Sie sich die Einträge **entityID** und **AssertionConsumerService** an.  Diese Werte entsprechen dem **Bezeichner**, der **Anmelde-URL** und der **Antwort-URL** in der Azure AD-Anwendungskonfiguration. Fügen Sie diese Werte in der Azure AD-Anwendungskonfiguration im Abschnitt **Domäne und URLs für Qlik Sense Enterprise**. Sofern sie nicht übereinstimmen, ersetzen Sie sie auch im Konfigurations-Assistenten der Azure AD-App.

    ![QlikSense][qs53]

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

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Qlik Sense Enterprise gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**, **Alle Anwendungen** und dann **Qlik Sense Enterprise** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **Qlik Sense Enterprise** ein, und wählen Sie den Eintrag aus.

    ![Qlik Sense Enterprise-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-qlik-sense-enterprise-test-user"></a>Erstellen eines Qlik Sense Enterprise-Testbenutzers

In diesem Abschnitt erstellen Sie in Qlik Sense Enterprise einen Benutzer mit dem Namen Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer auf der Qlik Sense Enterprise-Plattform ggf. vom  [Qlik Sense Enterprise-Supportteam](https://www.qlik.com/us/services/support) unterstützen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Qlik Sense Enterprise“ klicken, sollten Sie automatisch bei der Qlik Sense Enterprise-Anwendung angemeldet werden, für die Sie SSO eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

