---
title: 'Tutorial: Gewähren des Zugriffs auf eine ASP.NET Core-Web-API über eine Single-Page-Webanwendung – Azure Active Directory B2C'
description: Erfahren Sie, wie Sie mit Active Directory B2C eine .NET Core-Web-API schützen und die API über eine einseitige Node.js-App aufrufen.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 07/24/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 9ac95896e67338437325e8290a96b8e42b2fa3a7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72374245"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Tutorial: Gewähren des Zugriffs auf eine ASP.NET Core-Web-API über eine Single-Page-Webanwendung mithilfe von Azure Active Directory B2C

In diesem Tutorial erfahren Sie, wie Sie eine durch Azure Active Directory B2C (Azure AD B2C) geschützte ASP.NET Core-Web-API-Ressource über eine Single-Page-Webanwendung aufrufen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen einer Web-API-Anwendung
> * Konfigurieren von Bereichen für eine Web-API
> * Gewähren von Berechtigungen für die Web-API
> * Konfigurieren des Beispiels für die Verwendung der Anwendung

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie die erforderlichen Schritte unter [Tutorial: Aktivieren der Authentifizierung in einer Single-Page-Webanwendung mit Azure Active Directory B2C](active-directory-b2c-tutorials-spa.md).
* Visual Studio 2019 oder höher oder Visual Studio Code
* .NET Core 2.2 oder höher
* Node.js

## <a name="add-a-web-api-application"></a>Hinzufügen einer Web-API-Anwendung

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Konfigurieren von Bereichen

Bereiche ermöglichen die Steuerung des Zugriffs auf geschützte Ressourcen. Bereiche werden von der Web-API verwendet, um eine bereichsbasierte Zugriffssteuerung zu implementieren. So können beispielsweise einige Benutzer Lese- und Schreibzugriff haben, während andere Benutzer nur über Leseberechtigungen verfügen. In diesem Tutorial definieren Sie Lese- und Schreibberechtigungen für die Web-API.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Notieren Sie sich den Wert **VOLLSTÄNDIGER BEREICHSWERT** für den Bereich `demo.read`, der in einem späteren Schritt bei der Konfiguration der Single-Page-Webanwendung verwendet werden soll. Der vollständige Bereichswert ist ähnlich wie bei `https://yourtenant.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>Erteilen von Berechtigungen

Wenn Sie über eine andere Anwendung eine geschützte Web-API aufrufen möchten, müssen Sie der Anwendung Berechtigungen für die Web-API erteilen.

Im vorbereitenden Tutorial haben Sie eine Webanwendung namens *webapp1* erstellt. In diesem Tutorial konfigurieren Sie diese Anwendung so, dass sie die Web-API aufruft, die Sie in einem vorherigen Abschnitt, *webapi1*, erstellt haben.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Ihre Singe-Page-Webanwendung ist für den Aufruf der geschützten Web-API registriert. Ein Benutzer authentifiziert sich mit Azure AD B2C, um die Single-Page-Webanwendung zu verwenden. Die Single-Page-Webanwendung holt eine Autorisierungsgewährung von Azure AD B2C ein, um auf die geschützte Web-API zuzugreifen.

## <a name="configure-the-sample"></a>Das Beispiel konfigurieren

Nach dem Registrieren der Web-API und dem Definieren von Bereichen konfigurieren Sie den Web-API-Code für die Verwendung Ihres Azure AD B2C-Mandanten. In diesem Tutorial konfigurieren Sie eine .NET Core-Beispielwebanwendung, die Sie von GitHub herunterladen.

[Laden Sie ein \*ZIP-Archiv](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) herunter, oder klonen Sie das Beispiel-Web-API-Projekt von GitHub.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Konfigurieren der Web-API

1. Öffnen Sie die Datei <em>B2C-WebApi/**appsettings.json**</em> in Visual Studio oder Visual Studio Code.
1. Ändern Sie den Block `AzureAdB2C` so, dass er Ihren Mandantennamen, die Anwendungs-ID der Web-API-Anwendung, den Namen Ihrer Registrierungs-/Anmelderichtlinie und die von Ihnen zuvor definierten Bereiche widerspiegelt. Der Block sollte ähnlich wie im folgenden Beispiel aussehen (mit entsprechenden `Tenant`- und `ClientId`-Werten):

    ```json
    "AzureAdB2C": {
      "Tenant": "<your-tenant-name>.onmicrosoft.com",
      "ClientId": "<webapi-application-ID>",
      "Policy": "B2C_1_signupsignin1",

      "ScopeRead": "demo.read",
      "ScopeWrite": "demo.write"
    },
    ```

#### <a name="enable-cors"></a>Aktivieren von CORS

Damit Ihre Single-Page-Webanwendung die ASP.NET Core-Web-API aufrufen kann, müssen Sie [CORS](https://docs.microsoft.com/aspnet/core/security/cors) in der Web-API aktivieren.

1. Fügen Sie in *Startup.cs* CORS zur `ConfigureServices()`-Methode hinzu.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddCors();
    ```

1. Legen Sie auch innerhalb der `ConfigureServices()`-Methode den `jwtOptions.Authority`-Wert auf den folgenden Tokenaussteller-URI fest.

    Ersetzen Sie `<your-tenant-name>` durch den Namen Ihres B2C-Mandanten.

    ```csharp
    jwtOptions.Authority = $"https://<your-tenant-name>.b2clogin.com/{Configuration["AzureAdB2C:Tenant"]}/{Configuration["AzureAdB2C:Policy"]}/v2.0";
    ```

1. Konfigurieren Sie CORS in der `Configure()`-Methode.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        app.UseCors(builder =>
            builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

1. (Nur Visual Studio) Öffnen Sie unter **Eigenschaften** im Projektmappen-Explorer die Datei *launchSettings.json*, und suchen Sie dann den `iisExpress`-Block.
1. (Nur Visual Studio) Aktualisieren Sie den Wert `applicationURL` mit der Portnummer, die Sie bei der Registrierung der Anwendung *webapi1* in einem früheren Schritt angegeben haben. Beispiel:

    ```json
    "iisExpress": {
      "applicationUrl": "http://localhost:5000/",
      "sslPort": 0
    }
    ```

### <a name="configure-the-single-page-application"></a>Konfigurieren der Single-Page-Webanwendung

Die Single-Page-Webanwendung (SPA) aus dem [vorherigen Tutorial](active-directory-b2c-tutorials-spa.md) der Reihe verwendet Azure AD B2C für die Benutzerregistrierung und -anmeldung und ruft die ASP.NET Core Web-API auf, die durch den Demomandanten *frabrikamb2c* geschützt ist.

In diesem Abschnitt aktualisieren Sie die Single-Page-Webanwendung, um die ASP.NET Core Web-API aufzurufen, die durch *Ihren* Azure AD B2C-Mandanten geschützt ist und die Sie auf Ihrem lokalen Computer ausführen.

So ändern Sie die Einstellungen in der SPA

1. Öffnen Sie die Datei *index.html* im Projekt [active-directory-b2c-javascript-msal-singlepageapp][github-js-spa], das Sie im vorherigen Tutorial heruntergeladen oder kopiert haben.
1. Konfigurieren Sie das Beispiel mit dem URI für den zuvor erstellten Bereich *demo.read* und der URL der Web-API.
    1. Ersetzen Sie in der `appConfig`-Definition den Wert `b2cScopes` durch den vollständigen URI für den Bereich (den **VOLLSTÄNDIGEN BEREICHSWERT**, den Sie zuvor aufgezeichnet haben).
    1. Ändern Sie den Wert `webApi` in den Wert `applicationURL`, den Sie im vorherigen Abschnitt angegeben haben.

    Die `appConfig`-Definition sollte ähnlich wie der folgende Codeblock aussehen (mit Ihrem Mandantennamen anstelle von `<your-tenant-name>`):

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var appConfig = {
      b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/api/demo.read"],
      webApi: "http://localhost:5000/"
    };
    ```

## <a name="run-the-spa-and-web-api"></a>Ausführen der SPA und der Web-API

Abschließend führen Sie sowohl die ASP.NET Core-Web-API als auch die Single-Page-Webanwendung Node.js auf Ihrem lokalen Computer aus. Dann melden Sie sich bei der Single-Page-Webanwendung an und drücken eine Schaltfläche, um eine Anforderung an die geschützte API zu initiieren.

Obwohl beide Anwendungen in diesem Tutorial lokal ausgeführt werden, verwenden sie Azure AD B2C für die sichere Registrierung/Anmeldung und um Zugriff auf die geschützte Web-API zu gewähren.

### <a name="run-the-aspnet-core-web-api"></a>Ausführen der ASP.NET Core-Web-API

Drücken Sie in Visual Studio **F5**, um die Projektmappe *B2C-WebAPI.sln* zu erstellen und zu debuggen. Beim Starten des Projekts wird in Ihrem Standardbrowser eine Webseite mit dem Hinweis angezeigt, dass die Web-API für Anforderungen zur Verfügung steht.

Wenn Sie es vorziehen, die `dotnet`-CLI anstelle von Visual Studio zu verwenden:

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie in das Verzeichnis, das die *\*.csproj*-Datei enthält. Beispiel:

    `cd active-directory-b2c-dotnetcore-webapi/B2C-WebApi`

1. Sie können die Web-API durch Ausführen von `dotnet run` erstellen und ausführen.

    Wenn die API aktiv ist, sollte eine Ausgabe ähnlich der folgenden angezeigt werden (für das Tutorial können Sie alle `NETSDK1059`-Warnungen ignorieren):

    ```console
    $ dotnet run
    Hosting environment: Production
    Content root path: /home/user/active-directory-b2c-dotnetcore-webapi/B2C-WebApi
    Now listening on: http://localhost:5000
    Application started. Press Ctrl+C to shut down.
    ```

### <a name="run-the-single-page-app"></a>Ausführen der einseitigen App

1. Öffnen Sie ein Konsolenfenster, und wechseln Sie in das Verzeichnis mit dem Node.js-Beispiel. Beispiel:

    `cd active-directory-b2c-javascript-msal-singlepageapp`

1. Führen Sie die folgenden Befehle aus:

    ```console
    npm install && npm update
    node server.js
    ```

    Im Konsolenfenster wird die Portnummer angezeigt, unter der die Anwendung gehostet wird.

    ```console
    Listening on port 6420...
    ```

1. Navigieren Sie in Ihrem Browser zu `http://localhost:6420`, um die Anwendung anzuzeigen.
1. Melden Sie sich mit der E-Mail-Adresse und dem Kennwort an, die Sie im [vorherigen Tutorial](active-directory-b2c-tutorials-spa.md) verwendet haben. Nach erfolgreicher Anmeldung sollte die Meldung `User 'Your Username' logged-in` angezeigt werden.
1. Wählen Sie die Schaltfläche **Web-API aufrufen** aus. Die SPA erhält eine Autorisierungsgewährung von Azure AD B2C und greift dann auf die geschützte Web-API zu, um den Inhalt ihrer Indexseite anzuzeigen:

    ```Output
    Web APi returned:
    "<html>\r\n<head>\r\n  <title>Azure AD B2C API Sample</title>\r\n ...
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Hinzufügen einer Web-API-Anwendung
> * Konfigurieren von Bereichen für eine Web-API
> * Gewähren von Berechtigungen für die Web-API
> * Konfigurieren des Beispiels für die Verwendung der Anwendung

Nachdem Sie jetzt erlebt haben, wie eine SPA eine Ressource von einer geschützten Web-API anfordert, gewinnen Sie ein tieferes Verständnis dafür, wie diese Anwendungstypen miteinander und mit Azure AD B2C interagieren.

> [!div class="nextstepaction"]
> [In Azure Active Directory B2C verwendbare Anwendungstypen](active-directory-b2c-apps.md)

<!-- Links - EXTERNAL -->
[github-js-spa]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
