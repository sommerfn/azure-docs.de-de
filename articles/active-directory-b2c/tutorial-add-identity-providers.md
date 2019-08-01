---
title: 'Tutorial: Hinzufügen von Identitätsanbietern zu Ihren Anwendungen – Azure Active Directory B2C'
description: Erfahren Sie, wie Sie Ihren Anwendungen in Azure Active Directory B2C mithilfe des Azure-Portals Identitätsanbieter hinzufügen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 33f595dd36ac9448cc1276647f9943326b0b74c1
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655216"
---
# <a name="tutorial-add-identity-providers-to-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Hinzufügen von Identitätsanbietern zu Ihren Anwendungen in Azure Active Directory B2C

Sie können Benutzern in Ihren Anwendungen die Anmeldung mit verschiedenen Identitätsanbietern ermöglichen. Ein *Identitätsanbieter* erstellt und verwaltet die Identitätsinformationen und stellt gleichzeitig Authentifizierungsdienste für Anwendungen bereit. Sie können die von Azure Active Directory (Azure AD) B2C unterstützten Identitätsanbieter über das Azure-Portal Ihren [Benutzerflows](active-directory-b2c-reference-policies.md) hinzufügen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen der Identitätsanbieteranwendungen
> * Hinzufügen der Identitätsanbieter zu Ihrem Mandanten
> * Hinzufügen der Identitätsanbieter zu Ihrem Benutzerflow

In der Regel verwenden Sie nur einen Identitätsanbieter in Ihren Anwendungen, Sie haben aber die Möglichkeit, weitere hinzuzufügen. In diesem Tutorial erfahren Sie, wie Sie Ihrer Anwendung Azure AD und Facebook als Identitätsanbieter hinzufügen. Das Hinzufügen dieser beiden Identitätsanbieter zu Ihrer Anwendung ist optional. Sie können auch andere Identitätsanbieter wie etwa [Amazon](active-directory-b2c-setup-amzn-app.md), [GitHub](active-directory-b2c-setup-github-app.md), [Google](active-directory-b2c-setup-goog-app.md), [LinkedIn](active-directory-b2c-setup-li-app.md), [Microsoft](active-directory-b2c-setup-msa-app.md) oder [Twitter](active-directory-b2c-setup-twitter-app.md) hinzufügen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Benutzerflow](tutorial-create-user-flows.md), damit sich Benutzer bei Ihrer Anwendung registrieren und anmelden können.

## <a name="create-applications"></a>Erstellen von Anwendungen

Identitätsanbieteranwendungen stellen die ID und den Schlüssel für die Kommunikation mit Ihrem Azure AD B2C-Mandanten bereit. In diesem Abschnitt des Tutorials erstellen Sie eine Azure AD-Anwendung und eine Facebook-Anwendung, von denen Sie IDs und Schlüssel abrufen, um Ihrem Mandanten die Identitätsanbieter hinzuzufügen. Wenn Sie nur einen der Identitätsanbieter hinzufügen, müssen Sie nur die Anwendung für diesen Anbieter erstellen.

### <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

Um die Anmeldung für Benutzer von Azure AD zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten registrieren. Der Azure AD-Mandant ist nicht mit Ihrem Azure AD B2C-Mandanten identisch.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das Verzeichnis auswählen, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie einen Namen für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
1. Übernehmen Sie die Auswahl **Accounts in this organizational directory only** (Nur Konten in diesem Organisationsverzeichnis) für diese Anwendung.
1. Übernehmen Sie den Wert **Web** als **Umleitungs-URI**, geben Sie die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-B2C-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Beispiel: `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.

    Alle URLs sollten jetzt [b2clogin.com](b2clogin.md) verwenden.

1. Wählen Sie **Registrieren** aus, und notieren Sie sich den Wert unter **Anwendungs-ID (Client)** für einen späteren Schritt.
1. Wählen Sie im Anwendungsmenü unter **Verwalten** die Option **Zertifikate & Geheimnisse** und anschließend **Neuer geheimer Clientschlüssel** aus.
1. Geben Sie unter **Beschreibung** eine Beschreibung für Ihren geheimen Clientschlüssel ein. Beispiel: `Azure AD B2C App Secret`.
1. Wählen Sie den Ablaufzeitraum aus. Übernehmen Sie für diese Anwendung die Auswahl **In 1 Jahr** aus.
1. Wählen Sie **Hinzufügen** aus, und notieren Sie sich den Wert des neuen geheimen Clientschlüssels für einen späteren Schritt.

### <a name="create-a-facebook-application"></a>Erstellen einer Facebook-Anwendung

Um ein Facebook-Konto als Identitätsanbieter in Azure AD B2C verwenden zu können, müssen Sie eine Anwendung bei Facebook erstellen. Wenn Sie noch über kein Facebook-Konto verfügen, können Sie unter [https://www.facebook.com/](https://www.facebook.com/) eines erstellen.

1. Melden Sie sich auf der [Facebook-Entwickler-Website](https://developers.facebook.com/) mit den Anmeldeinformationen für Ihr Facebook-Konto an.
1. Wenn Sie dies noch nicht getan haben, müssen Sie sich als Facebook-Entwickler registrieren. Dazu wählen Sie oben rechts auf der Seite die Option **Get Started** (Erste Schritte) aus, akzeptieren die Facebook-Richtlinien und führen die Registrierungsschritte aus.
1. Wählen Sie **My Apps** (Meine Apps) und dann **Create App** (App erstellen) aus.
1. Geben Sie unter **Display Name** einen Anzeigenamen und unter **Contact Email** (Kontakt-E-Mail) eine gültige E-Mail-Adresse ein.
1. Klicken Sie auf **Create App ID** (App-ID erstellen). Hierzu müssen Sie möglicherweise die Richtlinien für die Facebook-Plattform akzeptieren und eine Online-Sicherheitsüberprüfung durchführen.
1. Wählen Sie **Settings** > **Basic** (Einstellungen > Allgemeines) aus.
1. Wählen Sie eine **Kategorie**, z.B. `Business and Pages`. Dieser Wert ist für Facebook erforderlich, wird aber von Azure AD B2C nicht verwendet.
1. Wählen Sie unten auf der Seite die Option **Add Platform** (Plattform hinzufügen) und dann **Website** aus.
1. Geben Sie für **Website-URL** `https://your-tenant-name.b2clogin.com/` ein, und ersetzen Sie `your-tenant-name` durch Ihren Mandantennnamen.
1. Geben Sie unter **Privacy Policy URL** (Datenrichtlinien-URL) eine URL ein, z.B. `http://www.contoso.com/`. Die Datenschutzrichtlinien-URL ist eine von Ihnen verwaltete Seite mit Datenschutzinformationen für Ihre Anwendung.
1. Klicken Sie auf **Save changes** (Änderungen speichern).
1. Notieren Sie sich im oberen Bereich der Seite den Wert von **App ID** (App-ID).
1. Wählen Sie neben **App Secret** (App-Geheimnis) die Option **Show** (Anzeigen) aus, und notieren Sie sich den Wert. Sie benötigen sowohl die App-ID als auch das App-Geheimnis, um Facebook als Identitätsanbieter in Ihrem Mandanten konfigurieren zu können. Das App-Geheimnis **** ist eine wichtige Sicherheitsanmeldeinformation, die sicher aufbewahrt werden muss.
1. Wählen Sie das Pluszeichen neben **PRODUCTS** (PRODUKTE) und dann unter **Facebook Login** (Facebook-Anmeldung) die Option **Set up** (Einrichten) aus.
1. Wählen Sie im linken Menü unter **Facebook Login** (Facebook-Anmeldung) die Option **Settings** (Einstellungen) aus.
1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Valid OAuth redirect URIs** (Gültige OAuth-Umleitungs-URIs) ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Wählen Sie unten auf der Seite die Option **Save Changes** (Änderungen speichern) aus.
1. Klicken Sie rechts oben auf der Seite auf den Selektor **Status**, und legen Sie ihn auf **On** (Ein) fest, um die Anwendung öffentlich und Ihre Facebook-Anwendung für Azure AD B2C verfügbar zu machen. Klicken Sie anschließend auf **Confirm** (Bestätigen). An diesem Punkt sollte sich der Status von **Entwicklung** in **Live** ändern.

## <a name="add-the-identity-providers"></a>Hinzufügen der Identitätsanbieter

Nach der Erstellung der Anwendung für den betreffenden Identitätsanbieter fügen Sie den Identitätsanbieter Ihrem Mandanten hinzu.

### <a name="add-the-azure-active-directory-identity-provider"></a>Hinzufügen von Azure Active Directory als Identitätsanbieter

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
1. Geben Sie einen **Namen** ein. Geben Sie beispielsweise *Contoso Azure AD* ein.
1. Klicken Sie auf **Identitätsanbietertyp**, wählen Sie **OpenID Connect (Vorschau)** aus, und klicken Sie dann auf **OK**.
1. Klicken Sie auf **Diesen Identitätsanbieter einrichten**.
1. Geben Sie für **Metadaten-URL** die folgende URL ein, und ersetzen Sie dabei `your-AD-tenant-domain` durch den Domänennamen Ihres Azure AD-Mandanten.

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Beispiel: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. Geben Sie unter **Client-ID** den zuvor notierten Wert von *Anwendungs-ID (Client)* ein.
1. Geben Sie im Feld **Geheimer Clientschlüssel** den zuvor notierten Wert von *Geheimer Clientschlüssel* ein.
1. Geben Sie optional einen Wert für **Domänenhinweis** ein. Beispiel: `ContosoAD`. [Domänenhinweise](../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) sind in der Authentifizierungsanforderung einer Anwendung enthaltene Anweisungen. Sie können verwendet werden, um die beschleunigte Anmeldung des Benutzers auf der Anmeldeseite seines Verbundidentitätsanbieters zu ermöglichen. Sie können auch von einer Anwendung für mehrere Mandanten verwendet werden, um den Benutzer beschleunigt direkt zur organisationsspezifischen Azure AD-Anmeldeseite für ihren Mandanten zu leiten.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Ansprüche dieses Identitätsanbieters zuordnen** aus, und legen Sie die folgenden Ansprüche fest:

    - Geben Sie für **Benutzer-ID** `oid` ein.
    - Geben Sie für **Anzeigename** `name` ein.
    - Geben Sie für **Vorname** `given_name` ein.
    - Geben Sie für **Nachname** `family_name` ein.
    - Geben Sie für **E-Mail** `unique_name` ein.

1. Wählen Sie **OK** und anschließend **Erstellen** aus, um Ihre Konfiguration zu speichern.

### <a name="add-the-facebook-identity-provider"></a>Hinzufügen von Facebook als Identitätsanbieter

1. Wählen Sie **Identitätsanbieter** und dann **Hinzufügen** aus.
1. Geben Sie einen **Namen** ein. Geben Sie z.B. *Facebook* ein.
1. Wählen Sie **Identitätsanbietertyp** > **Facebook** > **OK** aus.
1. Wählen Sie **Diesen Identitätsanbieter einrichten** aus, und geben Sie die zuvor notierte *App-ID* als **Client-ID** ein.
1. Geben Sie das zuvor notierte *App-Geheimnis* als **Geheimer Clientschlüssel** ein.
1. Wählen Sie **OK** und anschließend **Erstellen** aus, um die Facebook-Konfiguration zu speichern.

## <a name="update-the-user-flow"></a>Aktualisieren des Benutzerflows

In dem Tutorial, das Sie im Rahmen der Voraussetzungen abgeschlossen haben, haben Sie einen Benutzerflow mit dem Namen *B2C_1_signupsignin1* für die Registrierung und Anmeldung erstellt. In diesem Abschnitt fügen Sie die Identitätsanbieter dem Benutzerflow *B2C_1_signupsignin1* hinzu.

1. Wählen Sie **Benutzerflows (Richtlinien)** und dann den Benutzerflow *B2C_1_signupsignin1* aus.
2. Wählen Sie **Identitätsanbieter** und dann die von Ihnen hinzugefügten Identitätsanbieter **Facebook** und **Contoso Azure AD** aus.
3. Wählen Sie **Speichern** aus.

## <a name="test-the-user-flow"></a>Testen des Benutzerflows

1. Wählen Sie auf der Übersichtsseite des erstellten Benutzerflows die Option **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *webapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Wählen Sie **Benutzerflow ausführen** aus, und melden Sie sich mit dem zuvor hinzugefügten Identitätsanbieter an.
1. Wiederholen Sie die Schritte 1 bis 3 für andere Identitätsanbieter, die Sie hinzugefügt haben.

Nach erfolgreicher Anmeldung werden Sie zu `https://jwt.ms` weitergeleitet, und das decodierte Token wird in etwa wie folgt angezeigt:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "<key-ID>"
}.{
  "exp": 1562346892,
  "nbf": 1562343292,
  "ver": "1.0",
  "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
  "sub": "20000000-0000-0000-0000-000000000000",
  "aud": "30000000-0000-0000-0000-000000000000",
  "nonce": "defaultNonce",
  "iat": 1562343292,
  "auth_time": 1562343292,
  "name": "User Name",
  "idp": "facebook.com",
  "postalCode": "12345",
  "tfp": "B2C_1_signupsignin1"
}.[Signature]
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen der Identitätsanbieteranwendungen
> * Hinzufügen der Identitätsanbieter zu Ihrem Mandanten
> * Hinzufügen der Identitätsanbieter zu Ihrem Benutzerflow

Im nächsten Artikel erfahren Sie, wie Sie die Benutzeroberfläche der Seiten anpassen, die Benutzern im Zusammenhang mit ihrer Identität in Ihren Anwendungen angezeigt werden:

> [!div class="nextstepaction"]
> [Anpassen der Benutzeroberfläche Ihrer Anwendungen in Azure Active Directory B2C](tutorial-customize-ui.md)
