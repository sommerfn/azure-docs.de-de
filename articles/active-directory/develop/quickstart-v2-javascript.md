---
title: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer JavaScript-SPA | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie JavaScript-Anwendungen über Microsoft Identity Platform eine API aufrufen können, für die Zugriffstoken erforderlich sind.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:JavaScript
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ca9a8b87713508a581a833f60fbe863fd93919a
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795613"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa"></a>Schnellstart: Anmelden von Benutzern und Abrufen eines Zugriffstokens in einer JavaScript-SPA

In diesem Schnellstart erfahren Sie anhand eines Codebeispiels, wie in einer JavaScript-Single-Page-Anwendung (SPA) Benutzer von persönlichen Konten und Geschäfts-, Schul- und Unikonten angemeldet werden können. Eine JavaScript-Single-Page-Anwendung kann zudem ein Zugriffstoken abrufen, um die Microsoft Graph-API oder eine Web-API aufzurufen. (Eine Abbildung finden Sie unter [Funktionsweise des Beispiels](#how-the-sample-works).)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/).
* Entweder [Visual Studio Code](https://code.visualstudio.com/download) (zum Bearbeiten von Projektdateien) oder [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (zum Ausführen des Projekts als Visual Studio-Projektmappe).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Registrieren und Herunterladen Ihrer Schnellstartanwendung
> Verwenden Sie eine der folgenden Optionen, um die Schnellstartanwendung zu starten.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Option 1 (Express): Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels
>
> 1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
> 1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure Active Directory-Mandanten (Azure AD) fest.
> 1. Navigieren Sie zum neuen Bereich [Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs).
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **Registrieren** aus.
> 1. Befolgen Sie die Anweisungen zum Herunterladen und automatischen Konfigurieren der neuen Anwendung.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Option 2 (manuell): Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
>
> 1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
>
> 1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
> 1. Navigieren Sie zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908) von Microsoft Identity Platform für Entwickler.
> 1. Wählen Sie **Neue Registrierung** aus.
> 1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
> 1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
> 1. Wählen Sie im Abschnitt **Umleitungs-URI** in der Dropdownliste die Plattform **Web** aus, und legen Sie den Wert auf `http://localhost:30662/` fest.
> 1. Wählen Sie **Registrieren**. Notieren Sie sich für die spätere Verwendung auf der Seite **Übersicht** den Wert von **Anwendungs-ID (Client)** .
> 1. Für diesen Schnellstart muss der [Flow zur impliziten Genehmigung](v2-oauth2-implicit-grant-flow.md) aktiviert werden. Wählen Sie im linken Bereich der registrierten Anwendung die Option **Authentifizierung** aus.
> 1. Aktivieren Sie im Abschnitt **Erweiterte Einstellungen** unter **Implizite Gewährung** die Kontrollkästchen **ID-Token** und **Zugriffstoken**. ID-Token und Zugriffstoken sind erforderlich, da diese App Benutzer anmelden und eine API aufrufen muss.
> 1. Wählen Sie im oberen Bereich der Seite die Option **Speichern** aus.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Schritt 1: Konfigurieren Ihrer Anwendung im Azure-Portal
> Damit das Codebeispiel für diesen Schnellstart funktioniert, müssen Sie den Umleitungs-URI `http://localhost:30662/` hinzufügen und **Implizite Gewährung** aktivieren.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Make these changes for me]() (Diese Änderungen für mich vornehmen)
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-javascript/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-project"></a>Schritt 2: Herunterladen des Projekts

Wählen Sie die Option aus, die für Ihre Entwicklungsumgebung geeignet ist:

* Um das Projekt mit einem Webserver unter Verwendung von Node.js auszuführen, [laden Sie die Kernprojektdateien herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip). Verwenden Sie zum Öffnen der Dateien einen Editor wie z. B. [Visual Studio Code](https://code.visualstudio.com/).

* (Optional:) Um das Projekt mit dem IIS-Server auszuführen, [laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip). Extrahieren Sie die ZIP-Datei in einen lokalen Ordner (z.B. *C:\Azure-Samples*).

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-javascript-app"></a>Schritt 3: Konfigurieren Ihrer JavaScript-App
> Bearbeiten Sie *index.html* im Ordner *JavaScriptSPA*, und legen Sie die Werte `clientID` und `authority` unter `msalConfig` fest.

> [!div renderon="docs"]
> Bearbeiten Sie *index.html* im Ordner *JavaScriptSPA*, und ersetzen Sie `msalConfig` durch folgenden Code:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_info_here",
        redirectURI: "http://localhost:30662/"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```

> [!div renderon="docs"]
>
> Hierbei gilt:
> - *\<Enter_the_Application_Id_here>* ist die **Anwendungs-ID (Client)** für die von Ihnen registrierte Anwendung.
> - *\<Enter_the_Tenant_info_here >* wird auf eine der folgenden Optionen festgelegt:
>    - Wenn Ihre Anwendung *Nur Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (beispielsweise *contoso.microsoft.com*).
>    - Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **Organisationen**.
>    - Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**.
>
> > [!TIP]
> > Die Werte für **Anwendungs-ID (Client)** , **Verzeichnis-ID (Mandant)** und **Unterstützte Kontotypen** finden Sie im Azure-Portal auf der Seite **Übersicht** der App.
>

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Schritt 3: Ihre App ist konfiguriert und betriebsbereit
> Wir haben das Projekt mit Werten der Eigenschaften ihrer App konfiguriert. 

> [!div renderon="docs"]
> #### <a name="step-4-run-the-project"></a>Schritt 4: Ausführen des Projekts

* Wenn Sie [Node.js](https://nodejs.org/en/download/) verwenden, gehen Sie wie folgt vor:

    1. Führen Sie über das Projektverzeichnis den folgenden Befehl aus, um den Server zu starten:

        ```batch
        npm install
        node server.js
        ```

    1. Öffnen Sie einen Webbrowser, und navigieren Sie zu `http://localhost:30662/`.
    1. Wählen Sie **Anmelden** aus, um die Anmeldung zu starten, und rufen Sie anschließend die Microsoft Graph-API auf.


* Wählen Sie bei Verwendung von [Visual Studio](https://visualstudio.microsoft.com/downloads/) die Projektmappe aus, und drücken Sie dann F5, um das Projekt auszuführen.

Wählen Sie **Anmelden** aus, wenn die Anwendung im Browser geladen wurde. Bei der ersten Anmeldung werden Sie aufgefordert, Ihre Zustimmung zu geben, dass die Anwendung auf Ihr Profil zugreifen und Sie anmelden darf. Bei erfolgreicher Anmeldung sollten Ihre Benutzerprofilinformationen auf der Seite angezeigt werden.

## <a name="more-information"></a>Weitere Informationen

### <a name="how-the-sample-works"></a>Funktionsweise des Beispiels

![Funktionsweise der Beispiel-App in diesem Schnellstart](media/quickstart-v2-javascript/javascriptspa-intro.svg)

### <a name="msaljs"></a>msal.js

Über die MSAL-Bibliothek werden Benutzer angemeldet und die Token angefordert, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet werden. Die Datei *index.html* des Schnellstarts enthält einen Verweis auf die Bibliothek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> Sie können die vorstehende Version durch die letzte veröffentlichte Version unter [MSAL.js-Releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases) ersetzen.


Wenn Sie Node.js installiert haben, können Sie die aktuelle Version auch über Node.js-Paket-Manager (npm) herunterladen:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL-Initialisierung

Im Schnellstartcode wird auch veranschaulicht, wie Sie die MSAL-Bibliothek initialisieren:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here",
        redirectURI: "http://localhost:30662/"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Hierbei gilt:  |  |
> |---------|---------|
> |`clientId`     | Die Anwendungs-ID der im Azure-Portal registrierten Anwendung.|
> |`authority`    | (Optional:) Die Autoritäts-URL zur Unterstützung von Kontotypen, wie im Konfigurationsabschnitt oben beschrieben. Die Standardautorität lautet `https://login.microsoftonline.com/common`. |
> |`redirectURI`     | Der konfigurierte Antwort-/Umleitungs-URI der Anwendungsregistrierung. In diesem Fall `http://localhost:30662/`. |
> |`cacheLocation`  | (Optional:) Hiermit wird der Browserspeicher für den Authentifizierungsstatus festgelegt. Der Standardwert lautet „sessionStorage“.   |
> |`storeAuthStateInCookie`  | (Optional:) Die Bibliothek, in der der für die Überprüfung der Authentifizierungsflows in den Browsercookies erforderliche Authentifizierungsanforderungsstatus gespeichert wird. Dieses Cookie wird für die Browser IE und Edge festgelegt, um die Auswirkungen bestimmter [bekannter Probleme](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) zu minimieren. |

Weitere Informationen zu den verfügbaren konfigurierbaren Optionen finden Sie unter [Initialisieren von Clientanwendungen](msal-js-initializing-client-applications.md).

### <a name="sign-in-users"></a>Anmelden von Benutzern

Der folgende Codeausschnitt veranschaulicht, wie Sie Benutzer anmelden:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Hierbei gilt:  |  |
> |---------|---------|
> | `scopes`   | (Optional:) Enthält Bereiche, die bei der Anmeldung für die Benutzereinwilligung angefordert werden. Beispiel: `[ "user.read" ]` für Microsoft Graph oder `[ "<Application ID URL>/scope" ]` für benutzerdefinierte Web-APIs (d.h. `api://<Application ID>/access_as_user`). |

> [!TIP]
> Alternativ hierzu können Sie auch die `loginRedirect`-Methode verwenden, um die aktuelle Seite auf die Anmeldeseite umzuleiten, anstatt an das Popupfenster.

### <a name="request-tokens"></a>Anfordern von Token

MSAL verwendet drei Methoden zum Abrufen von Token: `acquireTokenRedirect`, `acquireTokenPopup` und `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die Methode `acquireTokenSilent` verwaltet das Abrufen und Erneuern von Token ohne Eingreifen des Benutzers. Nachdem die Methode `loginRedirect` oder `loginPopup` zum ersten Mal ausgeführt wurde, wird normalerweise die `acquireTokenSilent`-Methode zum Abrufen der Token genutzt, die für den Zugriff auf geschützte Ressourcen für nachfolgende Aufrufe verwendet werden. Aufrufe zum Anfordern oder Verlängern von Token werden im Hintergrund ausgeführt.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Hierbei gilt:  |  |
> |---------|---------|
> | `scopes`   | Enthält Bereiche, die für die Rückgabe im Zugriffstoken für die API angefordert werden. Beispiel: `[ "user.read" ]` für Microsoft Graph oder `[ "<Application ID URL>/scope" ]` für benutzerdefinierte Web-APIs (d.h. `api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

In manchen Fällen müssen Sie die Interaktion der Benutzer mit dem Microsoft Identity Platform-Endpunkt erzwingen. Beispiel:
* Benutzer müssen ihre Anmeldeinformationen gegebenenfalls erneut eingeben, weil ihr Kennwort abgelaufen ist.
* Ihre Anwendung fordert Zugriff auf zusätzliche Ressourcenbereiche an, für die der Benutzer seine Zustimmung erteilen muss.
* Die zweistufige Authentifizierung ist erforderlich.

Das übliche empfohlene Muster für die meisten Anwendungen ist zuerst das Aufrufen von `acquireTokenSilent`, das anschließende Abfangen der Ausnahme und dann das Aufrufen von `acquireTokenPopup` (oder `acquireTokenRedirect`), um eine interaktive Anforderung zu starten.

Durch Aufrufen von `acquireTokenPopup` wird ein Popupfenster für die Anmeldung angezeigt. (Über `acquireTokenRedirect` werden Benutzer an den Microsoft Identity Platform-Endpunkt umgeleitet.) In diesem Fenster müssen Benutzer interagieren, indem sie ihre Anmeldeinformationen bestätigen, ihre Zustimmung zur erforderlichen Ressource erteilen oder die zweistufige Authentifizierung durchführen.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> [!NOTE]
> In diesem Schnellstart werden bei Verwendung von Microsoft Internet Explorer die Methoden `loginRedirect` und `acquireTokenRedirect` verwendet. Dies liegt an einem [bekannten Problem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) bei der Verarbeitung von Popupfenstern in Internet Explorer.

## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Anleitung zum Erstellen der Anwendung für diesen Schnellstart finden Sie in folgendem Artikel:

> [!div class="nextstepaction"]
> [Aufrufen der Microsoft Graph-API aus einer JavaScript-SPA](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Die Dokumentation, häufig gestellte Fragen, Probleme und mehr zum MSAL-Repository finden Sie hier:

> [!div class="nextstepaction"]
> [MSAL.js – GitHub-Repository](https://github.com/AzureAD/microsoft-authentication-library-for-js)

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
