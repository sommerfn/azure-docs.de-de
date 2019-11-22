---
title: 'Azure AD v2.0: Angeleitetes Setup der JavaScript-Single-Page-Webanwendung (SPA)'
titleSuffix: Microsoft identity platform
description: In diesem Artikel erfahren Sie, wie JavaScript-Single-Page-Anwendungen eine API aufrufen können, für die Zugriffstoken des Azure Active Directory v2.0-Endpunkts erforderlich sind.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc03e6f1610fe6cef9ce72c981f6f800da8a9951
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802530"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer JavaScript-SPA (Single-Page-Webanwendung)

In diesem Leitfaden werden folgende Aktionen für eine JavaScript-Single-Page-Anwendung (SPA) beschrieben:
- Anmelden von persönlichen Konten sowie von Geschäfts-, Schul- oder Unikonten 
- Abrufen eines Zugriffstokens
- Aufrufen der Microsoft Graph-API oder anderer APIs, für die Zugriffstoken des *Microsoft Identity Platform-Endpunkts* erforderlich sind

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funktionsweise der über diesen Leitfaden generierten Beispiel-App

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Die über diesen Leitfaden erstellte Beispiel-App ermöglicht einer JavaScript-SPA das Abfragen der Microsoft Graph-API oder einer Web-API, die Token vom Microsoft Identity Platform-Endpunkt akzeptiert. In diesem Szenario wird nach der Benutzeranmeldung ein Zugriffstoken angefordert und den HTTP-Anforderungen über den Autorisierungsheader hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library für JavaScript-Vorschau|

> [!NOTE]
> *msal.js* verwendet als Ziel den Microsoft Identity Platform-Endpunkt, was persönlichen Konten sowie Geschäfts-, Schul- oder Unikonten das Anmelden sowie das Beziehen von Token ermöglicht. Für den Microsoft Identity Platform-Endpunkt gelten [einige Einschränkungen](azure-ad-endpoint-comparison.md#limitations).
> Die Unterschiede zwischen v1.0- und v2.0-Endpunkten werden unter [Endpunktvergleich](azure-ad-endpoint-comparison.md) erläutert.

<!--end-collapse-->

## <a name="set-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

> Möchten Sie stattdessen das Projekt dieses Beispiels herunterladen? Führen Sie einen der folgenden Schritte aus:
> 
> - Wenn Sie das Projekt mit einem lokalen Webserver (beispielsweise Node.js) ausführen möchten, [laden Sie die Projektdateien herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip).
>
> - Optional: Wenn Sie das Projekt mit dem IIS-Server (Internet Information Services, Internetinformationsdienste) von Microsoft ausführen möchten, [laden Sie das Visual Studio-Projekt](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip) herunter.
>
> Wenn Sie das Codebeispiel vor der Ausführung konfigurieren möchten, navigieren Sie direkt zum [Konfigurationsschritt](#register-your-application).

## <a name="prerequisites"></a>Voraussetzungen

* Zum Durcharbeiten dieses Tutorials ist ein lokaler Webserver erforderlich, z.B. [Node.js](https://nodejs.org/en/download/), [.NET Core](https://www.microsoft.com/net/core) oder eine IIS Express-Integration mit [Visual Studio 2017](https://www.visualstudio.com/downloads/).

* Installieren Sie bei Verwenden von Node.js zum Ausführen des Projekts eine IDE (Integrated Development Environment, integrierte Entwicklungsumgebung), z.B. [Visual Studio Code](https://code.visualstudio.com/download), um die Projektdateien zu bearbeiten.

* Die Anweisungen in diesem Leitfaden basieren auf Node.js und Visual Studio 2017. Sie können aber auch eine andere Entwicklungsumgebung oder einen anderen Webserver verwenden.

## <a name="create-your-project"></a>Erstellen Ihres Projekts

> ### <a name="option-1-nodejs-or-other-web-servers"></a>Option 1: Node.js oder andere Webserver
> Vergewissern Sie sicher, dass [Node.js](https://nodejs.org/en/download/) installiert ist, und erstellen Sie einen Ordner für Ihre Anwendung.
>
> ### <a name="option-2-visual-studio"></a>Option 2: Visual Studio
> Wenn Sie Visual Studio verwenden und ein neues Projekt erstellen, gehen Sie wie folgt vor:
> 1. Klicken Sie in Visual Studio auf **Datei** > **Neu** > **Projekt**.
> 1. Wählen Sie unter **Visual C#\Web** die Option **ASP.NET-Webanwendung (.NET Framework)** aus.
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und klicken Sie auf **OK**.
> 1. Wählen Sie unter **Neue ASP.NET-Webanwendung** die Option **Leer** aus.

## <a name="create-the-spa-ui"></a>Erstellen der Benutzeroberfläche für die Single-Page-Webanwendung (SPA)
1. Erstellen Sie die Datei *index.html* für Ihre JavaScript-Einzelseitenanwendung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts). Klicken Sie mit der rechten Maustaste, wählen Sie **Hinzufügen** > **Neues Element** > **HTML-Seite** aus, und nennen Sie die Datei *index.html*.

1. Fügen Sie der Datei *index.html* den folgenden Code hinzu:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Quickstart for MSAL JS</title>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
       <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
   </head>
   <body>
       <h2>Welcome to MSAL.js Quickstart</h2><br/>
       <h4 id="WelcomeMessage"></h4>
       <button id="SignIn" onclick="signIn()">Sign In</button><br/><br/>
       <pre id="json"></pre>
       <script>
           //JS code
       </script>
   </body>
   </html>
   ```

   > [!TIP]
   > Sie können die Version von „MSAL.js“ im obigen Skript durch die neueste veröffentlichte Version unter [MSAL.js-Releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases) ersetzen.

## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Verwenden der MSAL (Microsoft Authentication Library) für die Benutzeranmeldung

Fügen Sie der Datei `index.html` in den Tags `<script></script>` den folgenden Code hinzu:

   ```JavaScript
   var msalConfig = {
       auth: {
           clientId: "Enter_the_Application_Id_here",
           authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
       },
       cache: {
           cacheLocation: "localStorage",
           storeAuthStateInCookie: true
       }
   };

   var graphConfig = {
       graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
   };

   // this can be used for login or token request, however in more complex situations
   // this can have diverging options
   var requestObj = {
        scopes: ["user.read"]
   };

   var myMSALObj = new Msal.UserAgentApplication(msalConfig);
   // Register Callbacks for redirect flow
   myMSALObj.handleRedirectCallback(authRedirectCallBack);


   function signIn() {

       myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
           //Login Success
           showWelcomeMessage();
           acquireTokenPopupAndCallMSGraph();
       }).catch(function (error) {
           console.log(error);
       });
   }

   function acquireTokenPopupAndCallMSGraph() {
       //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
       myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
       }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenPopup(popup window)
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
       });
   }


   function graphAPICallback(data) {
       document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
   }


   function showWelcomeMessage() {
       var divWelcome = document.getElementById('WelcomeMessage');
       divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
       var loginbutton = document.getElementById('SignIn');
       loginbutton.innerHTML = 'Sign Out';
       loginbutton.setAttribute('onclick', 'signOut();');
   }


   //This function can be removed if you do not need to support IE
   function acquireTokenRedirectAndCallMSGraph() {
        //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
        myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenRedirect
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenRedirect(requestObj);
            }
        });
   }


   function authRedirectCallBack(error, response) {
       if (error) {
           console.log(error);
       }
       else {
           if (response.tokenType === "access_token") {
               callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
           } else {
               console.log("token type is:" + response.tokenType);
           }
       }
   }

   function requiresInteraction(errorCode) {
       if (!errorCode || !errorCode.length) {
           return false;
       }
       return errorCode === "consent_required" ||
           errorCode === "interaction_required" ||
           errorCode === "login_required";
   }

   // Browser check variables
   var ua = window.navigator.userAgent;
   var msie = ua.indexOf('MSIE ');
   var msie11 = ua.indexOf('Trident/');
   var msedge = ua.indexOf('Edge/');
   var isIE = msie > 0 || msie11 > 0;
   var isEdge = msedge > 0;
   //If you support IE, our recommendation is that you sign-in using Redirect APIs
   //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
   // can change this to default an experience outside browser use
   var loginType = isIE ? "REDIRECT" : "POPUP";

   if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
   }
   else if (loginType === 'REDIRECT') {
       document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(requestObj);
       };
       if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
   } else {
       console.error('Please set a valid login type');
   }
   ```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Wenn ein Benutzer das erste Mal die Schaltfläche **Anmelden** auswählt, ruft die `signIn`-Methode `loginPopup` auf, um den Benutzer anzumelden. Diese Methode öffnet ein Popupfenster mit dem *Microsoft Identity Platform-Endpunkt*, um die Anmeldeinformationen des Benutzers anzufordern und zu überprüfen. Nach erfolgreicher Anmeldung wird der Benutzer zur ursprünglichen Indexseite (*index.html*) umgeleitet. Ein Token wird empfangen und von `msal.js` verarbeitet, und die im Token enthaltenen Informationen werden zwischengespeichert. Dieses Token ist das sogenannte *ID-Token*, das grundlegende Informationen zum Benutzer enthält, beispielsweise den Benutzeranzeigenamen. Wenn Sie von diesem Token bereitgestellte Daten nutzen möchten, müssen Sie sicherstellen, dass das Token durch Ihren Back-End-Server überprüft wird. So wird garantiert, dass das Token für einen gültigen Benutzer Ihrer Anwendung ausgestellt wurde.

Die in diesem Leitfaden generierte SPA ruft `acquireTokenSilent` bzw. `acquireTokenPopup` auf, um ein *Zugriffstoken* zu beziehen, das zum Abfragen von Informationen zum Benutzerprofil von der Microsoft Graph-API verwendet wird. Wenn Sie ein Beispiel benötigen, das das ID-Token überprüft, sehen Sie sich [diese Beispielanwendung](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub-Beispiel „active-directory-javascript-singlepageapp-dotnet-webapi-v2“") auf GitHub an. In diesem Beispiel wird für die Tokenüberprüfung eine ASP.NET-Web-API verwendet.

#### <a name="getting-a-user-token-interactively"></a>Interaktives Abrufen eines Benutzertokens

Nach der erstmaligen Anmeldung sollen die Benutzer nicht jedes Mal erneut zur Authentifizierung aufgefordert werden, wenn sie ein Token für den Zugriff auf eine Ressource anfordern müssen. Aus diesem Grund sollten Token in den meisten Fällen mit *acquireTokenSilent* bezogen werden. Es gibt jedoch Situationen, in denen Sie die Interaktion mit dem Microsoft Identity Platform-Endpunkt erzwingen müssen. Beispiele:

- Benutzer müssen ihre Anmeldeinformationen erneut eingeben, da das Kennwort abgelaufen ist.
- Ihre Anwendung fordert Zugriff auf eine Ressource an, und die Zustimmung des Benutzers ist erforderlich.
- Die zweistufige Authentifizierung ist erforderlich.

Durch Aufrufen von *acquireTokenPopup* wird ein Popupfenster geöffnet. (Mit *acquireTokenRedirect* werden Benutzer zum Microsoft Identity Platform-Endpunkt umgeleitet.) In diesem Fenster müssen Benutzer ihre Anmeldeinformationen bestätigen, ihre Zustimmung für die erforderliche Ressource geben oder die zweistufige Authentifizierung durchführen.

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die Methode `acquireTokenSilent` wickelt den Bezug und die Verlängerung von Token ohne Eingreifen des Benutzers ab. Nach erstmaligem Ausführen von `loginPopup` (oder `loginRedirect`) wird normalerweise die Methode `acquireTokenSilent` zum Beziehen von Token verwendet, die für den Zugriff auf geschützte Ressourcen für nachfolgende Aufrufe genutzt werden. (Aufrufe zum Anfordern oder Verlängern von Token werden im Hintergrund ausgeführt.) `acquireTokenSilent` ist in bestimmten Fällen möglicherweise nicht erfolgreich – etwa, wenn das Kennwort des Benutzers abgelaufen ist. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1. Durch sofortiges Aufrufen von `acquireTokenPopup`, um eine Benutzeranmeldeaufforderung auszulösen. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein nicht authentifizierter Inhalt in der Anwendung für den Benutzer verfügbar ist. Die in diesem Leitfaden generierte Beispielanwendung verwendet dieses Muster.

2. Anwendungen können dem Benutzer auch visuell zu verstehen geben, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden wählen oder die Anwendung `acquireTokenSilent` zu einem späteren Zeitpunkt wiederholen kann. Dies wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden – beispielsweise, wenn in der Anwendung nicht authentifizierte Inhalte vorhanden sind. In diesem Fall kann der Benutzer entscheiden, wann er sich anmelden möchte, um auf die geschützte Ressource zuzugreifen oder die veralteten Informationen zu aktualisieren.

> [!NOTE]
> In dieser Schnellstartanleitung werden die Methoden `loginRedirect` und `acquireTokenRedirect` verwendet, wenn Internet Explorer als Browser genutzt wird. Diese Vorgehensweise wird aufgrund eines [bekannten Problems](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) im Zusammenhang mit der Behandlung von Popupfenstern durch Internet Explorer verwendet.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-acquired"></a>Aufrufen der Microsoft Graph-API unter Verwendung des zuvor bezogenen Tokens

Fügen Sie der Datei `index.html` in den Tags `<script></script>` den folgenden Code hinzu:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In der Beispielanwendung aus diesem Leitfaden wird die Methode `callMSGraph()` verwendet, um eine HTTP-Anforderung vom Typ `GET` für eine geschützte Ressource auszuführen, die ein Token erfordert. Der Inhalt wird anschließend an den Aufrufer zurückgegeben. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In der Beispielanwendung aus diesem Leitfaden ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Hinzufügen einer Methode zum Abmelden des Benutzers

Fügen Sie der Datei `index.html` in den Tags `<script></script>` den folgenden Code hinzu:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```

## <a name="register-your-application"></a>Anwendung registrieren

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wenn Sie in Ihrem Konto auf mehrere Mandanten zugreifen können, wählen Sie rechts oben das Konto aus, und legen Sie Ihre Portalsitzung auf den gewünschten Azure AD-Mandanten fest.
1. Navigieren Sie auf der Microsoft Identity Platform für Entwickler zur Seite [App-Registrierungen](https://go.microsoft.com/fwlink/?linkid=2083908).
1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** einen Namen für Ihre Anwendung ein.
1. Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
1. Wählen Sie im Abschnitt **Umleitungs-URI** in der Dropdownliste die Plattform **Web** aus, und legen Sie den Wert auf die URL der Anwendung (basierend auf Ihrem Webserver) fest.

   Informationen zum Festlegen und Abrufen einer Umleitungs-URL für Node.js und Visual Studio finden Sie weiter unten im Abschnitt „Festlegen einer Umleitungs-URL für Node.js“ bzw. unter [Festlegen einer Umleitungs-URL für Visual Studio](#set-a-redirect-url-for-visual-studio).

1. Wählen Sie **Registrieren**.
1. Notieren Sie sich für die spätere Verwendung auf der Seite **Übersicht** den Wert von **Anwendungs-ID (Client)** .
1. Für diesen Schnellstart muss der [Flow zur impliziten Genehmigung](v2-oauth2-implicit-grant-flow.md) aktiviert werden. Wählen Sie im linken Bereich der registrierten Anwendung die Option **Authentifizierung** aus.
1. Aktivieren Sie unter **Erweiterte Einstellungen** und **Implizite Gewährung** die Kontrollkästchen **ID-Token** und **Zugriffstoken**. ID- und Zugriffstoken sind erforderlich, da diese App Benutzer anmelden und eine API aufrufen muss.
1. Wählen Sie **Speichern** aus.

> #### <a name="set-a-redirect-url-for-nodejs"></a>Festlegen einer Umleitungs-URL für Node.js
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird zwar der Port 30662 verwendet, Sie können aber auch einen beliebigen anderen verfügbaren Port verwenden.
>
> Wenn Sie eine Umleitungs-URL in den Registrierungsinformationen der Anwendung einrichten möchten, kehren Sie zum Bereich **Anwendungsregistrierung** zurück und führen eine der folgenden Aktionen aus:
>
> - Legen Sie *`http://localhost:30662/`* als **Umleitungs-URL** fest.
> - Wenn Sie einen benutzerdefinierten TCP-Port nutzen, verwenden Sie *`http://localhost:<port>/`* (wobei *\<port>* die benutzerdefinierte TCP-Portnummer ist).
>
> #### <a name="set-a-redirect-url-for-visual-studio"></a>Festlegen einer Umleitungs-URL für Visual Studio
> Die Umleitungs-URL für Visual Studio erhalten Sie wie folgt:
> 1. Wählen Sie im Projektmappen-Explorer das Projekt aus.
>
>    Das Fenster **Eigenschaften** wird geöffnet. Sollte es nicht geöffnet werden, drücken Sie F4.
>
>    ![Das Eigenschaftenfenster des JavaScriptSPA-Projekts](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)
>
> 1. Kopieren Sie den Wert von **URL**.
> 1. Kehren Sie zurück zum Bereich **Anwendungsregistrierung**, und fügen Sie den kopierten Wert als **Umleitungs-URL** ein.

#### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

1. Fügen Sie in der Datei *index.html*, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie am Anfang der Datei zwischen den `<script></script>`-Tags den folgenden Code hinzu.

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "<Enter_the_Application_Id_here>",
            authority: "https://login.microsoftonline.com/<Enter_the_Tenant_info_here>"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    ```

    Hierbei gilt:
    - *\<Enter_the_Application_Id_here>* ist die **Anwendungs-ID (Client)** für die von Ihnen registrierte Anwendung.
    - *\<Enter_the_Tenant_info_here >* wird auf eine der folgenden Optionen festgelegt:
       - Falls Ihre Anwendung *nur Konten in diesem Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (beispielsweise *contoso.microsoft.com*).
       - Wenn Ihre Anwendung *Konten in einem beliebigen Organisationsverzeichnis* unterstützt, ersetzen Sie diesen Wert durch **organizations**.
       - Wenn Ihre Anwendung *Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten* unterstützt, ersetzen Sie diesen Wert durch **common**. Wenn Sie die Unterstützung *ausschließlich auf persönliche Microsoft-Konten* beschränken möchten, ersetzen Sie diesen Wert durch **consumers**.

## <a name="test-your-code"></a>Testen Ihres Codes

Testen Sie Ihren Code in einer der folgenden Umgebungen.

### <a name="test-with-nodejs"></a>Testen mit Node.js

Wenn Sie nicht Visual Studio verwenden, stellen Sie sicher, dass Ihr Webserver gestartet wurde.

1. Konfigurieren Sie den Server zum Lauschen am TCP-Port, der auf dem Speicherort Ihrer Datei *index.html* basiert. Starten Sie für Node.js den Webserver, um an dem Port zu lauschen, indem Sie an einer Eingabeaufforderung im Anwendungsordner die folgenden Befehle ausführen:

    ```bash
    npm install
    node server.js
    ```
1. Geben Sie in Ihrem Browser **http://\<span>\</span>localhost:30662** oder **http://\<span>\</span>localhost:{Port}** ,ein. Hierbei steht *Port* für den Port, an dem Ihr Webserver lauscht. Der Inhalt der Datei *index.html* und die Schaltfläche **Anmelden** sollten angezeigt werden.

### <a name="test-with-visual-studio"></a>Testen mit Visual Studio

Wählen Sie bei Verwendung von Visual Studio die Projektmappe aus, und drücken Sie dann F5, um das Projekt auszuführen. Der Browser wird geöffnet, und Sie werden zu http://<span></span>localhost:{Port} weitergeleitet, wo die Schaltfläche **Anmelden** angezeigt werden sollte.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Nachdem der Browser die Datei *index.html* geladen hat, klicken Sie auf **Anmelden**. Sie werden aufgefordert, sich mit dem Microsoft Identity Platform-Endpunkt anzumelden:

![Das Fenster zum Anmelden bei Ihrem JavaScript SPA-Konto](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)

### <a name="provide-consent-for-application-access"></a>Zustimmen zum Anwendungszugriff

Wenn Sie sich zum ersten Mal bei Ihrer Anwendung anmelden, werden Sie aufgefordert, ihr Zugriff auf Ihr Profil zu gewähren und sich anzumelden:

![Das Fenster „Angeforderte Berechtigungen“](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Anzeigen von Anwendungsergebnissen

Nach der Anmeldung werden Ihre Benutzerprofilinformationen in der angezeigten Antwort der Microsoft Graph-API zurückgegeben:

![Ergebnisse des Aufrufs der Microsoft Graph-API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich *user.read*, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die beim Registrierungsportal registriert wird. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt beispielsweise den Bereich *Calendars.Read*, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung *Calendars.Read* hinzufügen. Fügen Sie dann dem Aufruf von `acquireTokenSilent` den Bereich *Calendars.Read* hinzu.

>[!NOTE]
>Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich benötigt (nicht empfohlen), können Sie in den Aufrufen zum Beziehen von Token *clientId* als Bereich verwenden.

<!--end-collapse-->

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
