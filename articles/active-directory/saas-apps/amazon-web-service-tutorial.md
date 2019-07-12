---
title: 'Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS) | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Amazon Web Services (AWS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f897653442a3e1b2d6098b3be60c85e75ca54f9a
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551494"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Tutorial: Integration von Amazon Web Services (AWS) mit Azure Active Directory

In diesem Tutorial erfahren Sie, wie Sie Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren. Durch die Integration von Amazon Web Services (AWS) mit Azure Active Directory haben Sie folgende Möglichkeiten:

* Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Sie können mehrere Bezeichner für mehrere Instanzen wie unten gezeigt konfigurieren.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Mit diesen Werten entfernt Azure AD den Wert von **#** und sendet den richtigen Wert `https://signin.aws.amazon.com/saml` als Benutzergruppen-URL im SAML-Token.

**Wir empfehlen die Verwendung dieses Ansatzes aus folgenden Gründen:**

a. Mit jeder Anwendung erhalten Sie ein individuelles X509-Zertifikat. Jede Instanz der AWS App-Instanz kann dann ein anderes Zertifikatsverfallsdatum haben, das auf individueller AWS-Kontenbasis verwaltet werden kann. In diesem Fall gestaltet sich ein Rollover aller Zertifikate einfach.

b. Sie können die Benutzerbereitstellung mit der AWS-App in Azure AD aktivieren, woraufhin unser Dienst alle Rollen aus diesem AWS-Konto abruft. Sie müssen die AWS-Rollen nicht manuell in der App hinzufügen oder aktualisieren.

c. Sie können den App-Besitzer, der die App direkt in Azure AD verwalten kann, einzeln für die App zuweisen.

> [!Note]
> Stellen Sie sicher, dass Sie nur Katalog-Apps verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein SSO-fähiges AWS-Abonnement (Amazon Web Services).

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung. Amazon Web Services (AWS) unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog

Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Amazon Web Services (AWS)** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich die Option **Amazon Web Services (AWS)** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Amazon Web Services (AWS) mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Services (AWS) eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen
2. **[Konfigurieren von Amazon Web Services (AWS)](#configure-amazon-web-services-aws)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren.
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
5. **[Erstellen eines Testbenutzers für Amazon Web Services (AWS)](#create-amazon-web-services-aws-test-user)** , um in Amazon Web Services (AWS) ein Pendant von B. Simon zu erhalten, das mit der Benutzerdarstellung in Azure AD verknüpft ist.
6. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Im Abschnitt **SAML-Basiskonfiguration** ist die Anwendung vorkonfiguriert und die notwendigen URLs sind bereits mit Azure vorausgefüllt. Der Benutzer muss die Konfiguration speichern, indem er auf die Schaltfläche **Speichern** klickt.

5. Wenn Sie mehr als eine Instanz konfigurieren, geben Sie den ID-Wert an. Geben Sie ab der zweiten Instanz den ID-Wert in folgendem Format an. Verwenden Sie ein **#** -Zeichen, um einen eindeutigen SPN-Wert anzugeben.

    `https://signin.aws.amazon.com/saml#2`

6. Die AWS-Anwendung (Amazon Web Services) erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute. Klicken Sie auf das Symbol **Bearbeiten**, um das Dialogfeld „Benutzerattribute“ zu öffnen.

    ![image](common/edit-attribute.png)

7. Darüber hinaus wird von der Amazon Web Services (AWS)-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden. Führen Sie im Dialogfeld **Benutzerattribute** im Abschnitt **Benutzeransprüche** die folgenden Schritte aus, um das SAML-Tokenattribut wie in der folgenden Tabelle gezeigt hinzuzufügen:

    | NAME  | Quellattribut  | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Role            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | „Geben Sie einen Wert zwischen 900 Sekunden (15 Minuten) und 43.200 Sekunden (12 Stunden) an“ |  https://aws.amazon.com/SAML/Attributes |

    a. Klicken Sie auf **Neuen Anspruch hinzufügen**, um das Dialogfeld **Benutzeransprüche verwalten** zu öffnen.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie im Textfeld **Namespace** den für die betreffende Zeile angezeigten Namespacewert ein.

    d. Wählen Sie „Source“ als **Attribut** aus.

    e. Geben Sie in der Liste **Quellattribut** den für diese Zeile angezeigten Attributwert ein.

    f. Klicken Sie auf **OK**.

    g. Klicken Sie auf **Speichern**.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML**, und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

   ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Amazon Web Services (AWS) einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

   ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Konfigurieren von Amazon Web Services (AWS)

1. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

2. Klicken Sie auf **AWS Home**.

    ![Einmaliges Anmelden konfigurieren – Startseite][11]

3. Klicken Sie auf **Identitäts- und Zugriffsverwaltung**.

    ![Einmaliges Anmelden konfigurieren – Identität][12]

4. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**.

    ![Einmaliges Anmelden konfigurieren – Anbieter][13]

5. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren – Dialogfeld][14]

    a. Wählen Sie für **Anbietertyp** die Option **SAML** aus.

    b. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (zum Beispiel: *WAAD*).

    c. Klicken Sie auf **Datei auswählen**, um die aus dem Azure-Portal heruntergeladene **Metadatendatei** hochzuladen.

    d. Klicken Sie auf **Nächster Schritt**.

6. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**.

    ![Einmaliges Anmelden konfigurieren – Überprüfen][15]

7. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Rolle erstellen**.

    ![Einmaliges Anmelden konfigurieren – Rollen][16]

8. Führen Sie auf der Seite **Rolle erstellen** die folgenden Schritte aus:  

    ![Einmaliges Anmelden konfigurieren – Vertrauensstellung][19]

    a. Wählen Sie **SAML 2.0-Verbund** unter **Typ der vertrauenswürdige Entität auswählen**.

    b. Wählen Sie im**Abschnitt zum Auswählen eines SAML 2.0-Anbieters** den **SAML-Anbieter** aus, den Sie zuvor erstellt haben (Beispiel: *WAAD*).

    c. Wählen Sie **Programmgesteuerten und AWS Management Console-Zugriff zulassen** aus.
  
    d. Klicken Sie auf **Weiter: Permissions** (Weiter: Berechtigungen).

9. Fügen Sie im Dialogfeld **Berechtigungsrichtlinien anfügen** die entsprechende Richtlinie für Ihre Organisation an. Klicken Sie auf **Weiter: Review** (Weiter: Überprüfen).  

    ![Einmaliges Anmelden konfigurieren – Richtlinie][33]

10. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren – Überprüfung][34]

    a. Geben Sie im Textfeld **Rollenname** Ihren Rollennamen ein.

    b. Geben Sie im Textfeld **Rollenbeschreibung** die Beschreibung ein.

    c. Klicken Sie auf **Rolle erstellen**.

    d. Erstellen Sie so viele Rollen, wie Sie benötigen, und ordnen Sie diese dem Identitätsanbieter zu.

11. Verwenden Sie die Anmeldeinformationen des AWS-Dienstkontos, um die Rollen aus dem AWS-Konto in der Azure AD-Benutzerbereitstellung abzurufen. Öffnen Sie hierzu die Startseite der AWS-Konsole.

12. Klicken Sie auf **Services** (Dienste) -> **Security, Identity & Compliance** (Sicherheit, Identität und Konformität) -> **IAM**.

    ![Rollen aus AWS-Konto abrufen](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Wählen Sie im IAM-Abschnitt die Registerkarte **Policies** (Richtlinien) aus.

    ![Rollen aus AWS-Konto abrufen](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Erstellen Sie durch Klicken auf **Richtlinie erstellen** eine neue Richtlinie, um die Rollen in Azure AD-Benutzerbereitstellung vom AWS-Konto abzuholen.

    ![Neue Richtlinie erstellen](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Erstellen Sie anhand der folgenden Schritte eine eigene Richtlinie, um alle Rollen aus AWS-Konten abzurufen:

    ![Neue Richtlinie erstellen](./media/amazon-web-service-tutorial/policy1.png)

    a. Klicken Sie im Abschnitt **Richtlinie erstellen** auf die Registerkarte **JSON**.

    b. Fügen Sie in diesem Richtliniendokument den folgenden JSON-Code hinzu.

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                "iam:ListRoles"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    c. Klicken Sie auf die Schaltfläche **Richtlinie überprüfen**, um die Richtlinie zu überprüfen.

    ![Neue Richtlinie definieren](./media/amazon-web-service-tutorial/policy5.png)

16. Führen Sie zum Festlegen der **neuen Richtlinie** die folgenden Schritte aus:

    ![Neue Richtlinie definieren](./media/amazon-web-service-tutorial/policy2.png)

    a. Als **Policy Name** (Richtlinienname) geben Sie **AzureAD_SSOUserRole_Policy** an.

    b. Sie können für die Richtlinie Text in **Description** (Beschreibung) angeben, da hierfür **This policy will allow to fetch the roles from AWS accounts** (Diese Richtlinie erlaubt das Abrufen der Rollen aus AWS) gilt.

    c. Klicken Sie auf die Schaltfläche **Richtlinie erstellen**.

17. Erstellen Sie ein neues Benutzerkonto im AWS-IAM-Dienst, indem Sie die folgenden Schritte ausführen:

    a. Klicken Sie im Navigationsmenü der AWS-IAM-Konsole auf **Users** (Benutzer).

    ![Neue Richtlinie definieren](./media/amazon-web-service-tutorial/policy3.png)

    b. Klicken Sie auf die Schaltfläche **Add user** (Benutzer hinzufügen), um einen neuen Benutzer zu erstellen.

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/policy4.png)

    c. Führen Sie im Abschnitt **Add user** (Benutzer hinzufügen) die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/adduser1.png)

    * Geben Sie **AzureADRoleManager** als Benutzernamen ein.

    * Wählen Sie für den Zugriffstyp die Option **Programmatic access** (Programmgesteuerter Zugriff). Auf diese Weise kann der Benutzer die APIs aufrufen und die Rollen aus dem AWS-Konto abrufen.

    * Klicken Sie in der unteren rechten Ecke auf die Schaltfläche **Next Permissions** (Nächste Berechtigungen).

18. Nun erstellen Sie eine neue Richtlinie für diesen Benutzer, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/adduser2.png)

    a. Klicken Sie auf die Schaltfläche **Attach existing policies directly** (Vorhandene Richtlinien direkt anfügen).

    b. Suchen Sie im Filterabschnitt **AzureAD_SSOUserRole_Policy** nach der neu erstellte Richtlinie.

    c. Wählen Sie die **Richtlinie** aus, und klicken Sie dann auf die Schaltfläche **Next: Review** (Weiter: Überprüfung).

19. Überprüfen Sie die Richtlinie für den angefügten Benutzer, indem Sie folgende Schritte ausführen:

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/adduser3.png)

    a. Überprüfen Sie den Benutzernamen, den Zugriffstyp und die Richtlinie, die dem Benutzer zugeordnet wurde.

    b. Klicken Sie in der unteren rechten Ecke auf die Schaltfläche **Create user** (Benutzer erstellen), um den Benutzer zu erstellen.

20. Laden Sie die Anmeldeinformationen eines Benutzers herunter, indem Sie die folgenden Schritte ausführen:

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/adduser4.png)

    a. Kopieren Sie **Access key ID** (Zugriffsschlüssel-ID) und **Secret access key** (Geheimer Zugriffsschlüssel) des Benutzers.

    b. Geben Sie diese Anmeldeinformationen im Azure AD-Abschnitt für die Benutzerbereitstellung ein, um die Rollen aus der AWS-Konsole abzurufen.

    c. Klicken Sie unten auf die Schaltfläche **Close** (Schließen).

21. Navigieren Sie im Azure AD-Verwaltungsportal in der Amazon Web Services-App zum Abschnitt **User Provisioning** (Benutzerbereitstellung).

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/provisioning.png)

22. Geben Sie den **Zugriffsschlüssel** und den **geheimen Zugriffsschlüssel** in das Feld **Client Secret** (Geheimer Clientschlüssel) bzw. **Secret Token** (Geheimes Token) ein.

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Geben Sie in das Feld **clientsecret** (Geheimer Clientschlüssel) den AWS-Benutzerzugriffsschlüssel ein.

    b. Geben Sie in das Feld **Secret Token** (Geheimes Token) die geheimen AWS-Benutzerdaten ein.

    c. Klicken Sie auf die Schaltfläche **Test Connection** (Verbindung testen), um sicherzustellen, dass die Verbindung ordnungsgemäß funktioniert.

    d. Speichern Sie die Einstellung, indem Sie oben auf die Schaltfläche **Save** (Speichern) klicken.

23. Stellen Sie nun sicher, dass Sie im Abschnitt „Settings“ (Einstellungen) die Option „Provisioning Status“ (Bereitstellungsstatus) über den Schalter **On** (Ein) aktivieren, und klicken Sie dann oben auf die Schaltfläche **Save** (Speichern).

    ![Benutzer hinzufügen](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Create**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Amazon Web Services (AWS) gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Amazon Web Services (AWS)** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann am unteren Bildschirmrand auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-amazon-web-services-aws-test-user"></a>Erstellen eines Testbenutzers für Amazon Web Services (AWS)

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens B. Simon in Amazon Web Services (AWS). Bei Amazon Web Services (AWS) muss für das einmalige Anmelden nicht eigens ein Benutzer im AWS-System erstellt werden, daher ist hier keine Aktion erforderlich.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

Wenn Sie im Zugriffsbereich die Kachel „Amazon Web Services (AWS)“ auswählen, sollten Sie automatisch bei der AWS-Anwendung angemeldet werden, für die Sie das einmalige Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Bekannte Probleme

 * Im Abschnitt **Bereitstellung** ist im Unterabschnitt **Zuordnungen** eine Lademeldung zu sehen, die Attributzuordnungen werden aber nicht angezeigt. Aktuell wird als Bereitstellungsworkflow lediglich das Importieren von Rollen aus AWS in Azure AD für die Auswahl während der Benutzer-/Gruppenzuweisung unterstützt. Die Attributzuordnungen hierfür sind vorgegeben und nicht konfigurierbar.
 
 * Im Abschnitt **Bereitstellung** kann immer nur ein einzelner Satz von Anmeldeinformationen für einen einzelnen AWS-Mandanten eingegeben werden. Alle importierten Rollen werden in die Eigenschaft „appRoles“ des Azure AD-Objekts [servicePrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) für den AWS-Mandanten geschrieben. Mehrere AWS-Mandanten (dargestellt durch Dienstprinzipale) können Azure AD über den Katalog für die Bereitstellung hinzugefügt werden. Es gibt jedoch ein bekanntes Problem, das dazu führt, dass nicht alle importierten Rollen automatisch aus den verschiedenen AWS-Dienstprinzipalen für die Bereitstellung in den einzelnen Dienstprinzipal für einmaliges Anmelden geschrieben werden können. Zur Umgehung dieses Problems können Sie mithilfe der [Microsoft Graph-API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) alle App-Rollen extrahieren, die in die einzelnen AWS-Dienstprinzipale mit konfigurierter Bereitstellung importiert wurden. Diese Rollenzeichenfolgen können dann dem AWS-Dienstprinzipal mit konfiguriertem einmaligem Anmelden hinzugefügt werden.

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
