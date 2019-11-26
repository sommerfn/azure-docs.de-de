---
title: Web-App, die Benutzer anmeldet (Codekonfiguration) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Codekonfiguration)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f52fc70b54c27362575bef00c39a93d13e77cc2e
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175347"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Web-App, die Benutzer anmeldet – Codekonfiguration

Erfahren Sie, wie Sie den Code für Ihre Web-App konfigurieren, die Benutzer anmeldet.

## <a name="libraries-used-to-protect-web-apps"></a>Bibliotheken zum Schutz von Web-Apps

<!-- This section can be in an include for Web App and Web APIs -->
Die folgenden Bibliotheken werden zum Schützen einer Web-App (und einer Web-API) verwendet:

| Plattform | Bibliothek | BESCHREIBUNG |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [Identitätsmodellerweiterungen für .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Die Microsoft Identity-Erweiterung für .NET schlägt bei der direkten Verwendung durch ASP.NET und ASP.NET Core eine Reihe von DLLs vor, die sowohl unter .NET Framework und .NET Core ausgeführt werden. Über eine ASP.NET-/ASP.NET Core-Webanwendung können Sie mithilfe der **TokenValidationParameters**-Klasse die Tokenüberprüfung steuern (insbesondere in ISV-Szenarios). |
| ![Java](media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | MSAL für Java – derzeit in öffentlicher Vorschau |
| ![Python](media/sample-v2-code/small_logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | MSAL für Python – derzeit in öffentlicher Vorschau |

Wählen Sie die Registerkarte aus, die der Plattform entspricht, die Sie interessiert:

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Die Codeausschnitte in diesem und dem nächsten Artikel stammen aus dem Artikel [ASP.NET Core Web app incremental tutorial, chapter 1 (Inkrementelles Tutorial zu ASP.NET Core-Web-Apps, Kapitel 1)](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg).

In diesem Tutorial finden Sie auch ausführliche Informationen zur Implementierung.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem [ASP.NET-Web-App-Beispiel](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="javatabjava"></a>[Java](#tab/java)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem MSAL-Java-Web-App-Beispiel [Java-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="pythontabpython"></a>[Python](#tab/python)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem Web-App-Beispiel „MSAL.Python“ unter [Python-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp).

Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

---

## <a name="configuration-files"></a>Konfigurationsdateien

Webanwendungen, die Benutzer bei der Microsoft Identity Platform anmelden, werden in der Regel mithilfe von Konfigurationsdateien konfiguriert. Folgende Einstellungen sind einzugeben:

- Cloud-`Instance`, wenn Ihre App beispielsweise in nationalen Clouds ausgeführt werden soll
- Zielgruppe in `tenantId`
- `clientId` für Ihre Anwendung, die aus dem Azure-Portal kopiert wird.

Mitunter können Anwendungen durch `authority`, einer Verkettung von `instance` und `tenantId`, parametrisiert werden.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.Net Core befinden sich diese Einstellungen in der Datei [appsettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) im Abschnitt „AzureAD“.

```Json
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.microsoftonline.com/" for Azure Public cloud.
    // "https://login.microsoftonline.us/" for Azure US government.
    // "https://login.microsoftonline.de/" for Azure AD Germany
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD Audience among:
    // - the tenant Id as a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account or Microsoft personal account
    // - "consumers" to sign-in users with Microsoft personal account only
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

In ASP.NET Core gibt es eine weitere Datei ([properties\launchSettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7)), die die URL (`applicationUrl`) und den SSL-Port (`sslPort`) für Ihre Anwendung und verschiedene Profile enthält.

```Json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

Im Azure-Portal müssen die Antwort-URLs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, diesen URLs entsprechen. Für die beiden zuvor genannten Konfigurationsdateien wären diese beispielsweise `https://localhost:44321/signin-oidc`, da die applicationUrl `http://localhost:3110` ist, die `sslPort`-Eigenschaft aber angegeben ist (44321) und die `CallbackPath`-Eigenschaft in der `appsettings.json`-Konfiguration `/signin-oidc` ist.

Auf die gleiche Weise wird der Abmelde-URI auf `https://localhost:44321/signout-callback-oidc` festgelegt.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET wird die Anwendung über die Zeilen 12-15 der Datei [Web.config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) konfiguriert.

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

Im Azure-Portal müssen die Antwort-URIs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, mit diesen URLs übereinstimmen, d. h. mit `https://localhost:44326/`.

# <a name="javatabjava"></a>[Java](#tab/java)

In Java befindet sich die Konfiguration in der Datei [application.properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) unter `src/main/resources`.

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

Im Azure-Portal müssen die Antwort-URIs, die Sie auf der Seite **Authentifizierung** für Ihre Anwendung registrieren müssen, mit dem von der Anwendung definierten RedirectURIs-Eintrag übereinstimmen, also mit `http://localhost:8080/msal4jsample/secure/aad` und `http://localhost:8080/msal4jsample/graph/me`.

# <a name="pythontabpython"></a>[Python](#tab/python)

Hier finden Sie die Python-Konfigurationsdatei in [app_config.py](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py).

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.microsoftonline.com/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://graph.microsoft.com/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So token cache will be stored in server-side session
```

> [!NOTE]
> In dieser Schnellstartanleitung wird vorgeschlagen, den geheimen Clientschlüssel der Einfachheit halber in der Konfigurationsdatei zu speichern.
> In Ihrer Produktions-App sollten Sie andere Möglichkeiten zum Speichern Ihres Geheimnisses (z. B. KeyVault) oder eine Umgebungsvariable verwenden, wie in der Flask-Dokumentation beschrieben: https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>Initialisierungscode

Der Initialisierungscode ist je nach Plattform unterschiedlich. Für ASP.NET Core und ASP.NET wird die Anmeldung von Benutzern an die OpenIDConnect-Middleware delegiert. Derzeit generiert die ASP.NET-/ASP.NET Core-Vorlage Webanwendungen für den Azure AD v1.0-Endpunkt. Daher muss eine Konfigurationsänderung erfolgen, um diese an den Microsoft Identity Platform v2.0-Endpunkt anzupassen. Bei Java erfolgt dies von Spring in Zusammenarbeit mit der Anwendung.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET Core-Web-Apps (und Web-APIs) ist die Anwendung geschützt, da es das Attribut `[Authorize]` für die Controller bzw. Controlleraktionen gibt. Mit diesem Attribut wird geprüft, ob der Benutzer authentifiziert ist. Der Code für die Anwendungsinitialisierung befindet sich in der Datei `Startup.cs`. Zum Hinzufügen der Authentifizierung mit der Microsoft Identity Platform (ehemals Azure AD v2.0) müssen Sie den folgenden Code hinzufügen. Die Kommentare im Code sollten selbsterklärend sein.

  > [!NOTE]
  > Falls Sie Ihr Projekt mit dem ASP.NET Core-Standardwebprojekt in Visual Studio oder mit `dotnet new mvc` starten, ist die Methode `AddAzureAD` standardmäßig verfügbar, da die zugehörigen Pakete automatisch geladen werden.
  > Wenn Sie jedoch ein Projekt von Grund auf neu erstellen und den unten angegebenen Code verwenden, empfiehlt es sich, Ihrem Projekt das NuGet-Paket **Microsoft.AspNetCore.Authentication.AzureAD.UI** hinzuzufügen, um die Methode `AddAzureAD` verfügbar zu machen.

Der folgende Code ist in [Startup.cs#L33-L34](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34) verfügbar.

```CSharp
public class Startup
{
 ...

  // This method gets called by the runtime. Use this method to add services to the container.
  public void ConfigureServices(IServiceCollection services)
  {
    ...
      // Sign-in users with the Microsoft identity platform
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration);

      services.AddMvc(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
            options.Filters.Add(new AuthorizeFilter(policy));
            })
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    }
```

`AddMicrosoftIdentityPlatformAuthentication` ist eine in [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23) definierte Erweiterungsmethode. Sie hat folgende Aufgaben:

- Hinzufügen des Authentifizierungsdiensts
- Konfigurieren von Optionen zum Lesen der Konfigurationsdatei
- Konfigurieren der OpenID-Verbindungsoptionen dergestalt, dass die verwendete Autorität der Microsoft Identity Platform-Endpunkt (früher Azure AD v2.0) ist
- Validieren des Ausstellers des Tokens
- Zuordnen der dem Namen entsprechenden Ansprüche aus dem Anspruch „preferred_username“ im ID-Token

Zusätzlich zur Konfiguration können Sie beim Aufrufen von `AddMicrosoftIdentityPlatformAuthentication` Folgendes angeben:

- den Namen des Konfigurationsabschnitts (standardmäßig AzureAD)
- wenn Sie die OpenIdConnect-Middleware-Ereignisse nachverfolgen möchten, die Ihnen bei der Fehlerbehebung in Ihrer Webanwendung helfen können, sollte die Authentifizierung nicht funktionieren. Wenn Sie `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` auf `true` festlegen, wird Ihnen angezeigt, wie Informationen von den Komponenten der ASP.NET Core-Middleware verarbeitet werden, während sie von der HTTP-Antwort zur Identität des Benutzers in `HttpContext.User` weiterverarbeitet werden.

```CSharp
/// <summary>
/// Add authentication with Microsoft identity platform.
/// This method expects the configuration file will have a section named "AzureAd" with the necessary settings to initialize authentication options.
/// </summary>
/// <param name="services">Service collection to which to add this authentication scheme</param>
/// <param name="configuration">The Configuration object</param>
/// <param name="subscribeToOpenIdConnectMiddlewareDiagnosticsEvents">
/// Set to true if you want to debug, or just understand the OpenIdConnect events.
/// </param>
/// <returns></returns>
public static IServiceCollection AddMicrosoftIdentityPlatformAuthentication(
  this IServiceCollection services,
  IConfiguration configuration,
  string configSectionName = "AzureAd",
  bool subscribeToOpenIdConnectMiddlewareDiagnosticsEvents = false)
{
  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
      .AddAzureAD(options => configuration.Bind(configSectionName, options));
  services.Configure<AzureADOptions>(options => configuration.Bind(configSectionName, options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
      // Per the code below, this application signs in users in any Work and School
      // accounts and any Microsoft Personal Accounts.
      // If you want to direct Azure AD to restrict the users that can sign-in, change
      // the tenant value of the appsettings.json file in the following way:
      // - only Work and School accounts => 'organizations'
      // - only Microsoft Personal accounts => 'consumers'
      // - Work and School and Personal accounts => 'common'
      // If you want to restrict the users that can sign-in to only one tenant
      // set the tenant value in the appsettings.json file to the tenant ID
      // or domain of this organization
      options.Authority = options.Authority + "/v2.0/";

      // If you want to restrict the users that can sign-in to several organizations
      // Set the tenant value in the appsettings.json file to 'organizations', and add the
      // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection
      options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;

      // Set the nameClaimType to be preferred_username.
      // This change is needed because certain token claims from Azure AD V1 endpoint
      // (on which the original .NET core template is based) are different than Microsoft identity platform endpoint.
      // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
      // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
      options.TokenValidationParameters.NameClaimType = "preferred_username";

      // ...

      if (subscribeToOpenIdConnectMiddlewareDiagnosticsEvents)
      {
          OpenIdConnectMiddlewareDiagnostics.Subscribe(options.Events);
      }
  });
  return services;
}
  ...
```

Die `AadIssuerValidator`-Klasse ermöglicht, dass der Aussteller des Tokens in vielen Fällen validiert wird (v1.0- oder v2.0-Token, Anwendung mit einem oder mehreren Mandanten oder Anwendung, die Benutzer mit ihren persönlichen Microsoft-Konten, bei der öffentlichen Azure Cloud oder nationalen Clouds anmeldet). Sie ist unter [Microsoft.Identity.Web/Resource/AadIssuerValidator.cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs) verfügbar.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Der Code für die Authentifizierung in ASP.NET-Web-Apps/-Web-APIs befindet sich in der Datei [App_Start/Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61).

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="javatabjava"></a>[Java](#tab/java)

Das Java-Beispiel verwendet das Spring-Framework. Die Anwendung ist geschützt, weil Sie einen `Filter` implementieren, der jede HTTP-Antwort abfängt. Im Schnellstart für Java-Web-Apps handelt es sich dabei um den Filter `AuthFilter` in `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java`. Der Filter verarbeitet den OAuth 2.0-Autorisierungscodeflow und hat folgende Aufgaben:

- Überprüfen, ob der Benutzer authentifiziert ist (`isAuthenticated()`-Methode).
- Wenn der Benutzer nicht authentifiziert ist, wird die URL der Azure AD-Autorisierungsendpunkte berechnet und der Browser zu diesem URI umgeleitet.
- Wenn die Antwort mit dem Autorisierungscodeflow eingeht, wird mit MSAL Java das Token bezogen.
- Wenn das Token schließlich vom Tokenendpunkt (im Umleitungs-URI) empfangen wird, wird der Benutzer angemeldet.

Weitere Informationen finden Sie unter der `doFilter()`-Methode in [AuthFilter.java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java).

> [!NOTE]
> Der Code von `doFilter()` wird in einer etwas anderen Reihenfolge geschrieben, aber der Flow entspricht dem beschriebenen.

Unter [Microsoft Identity Platform und der OAuth 2.0-Autorisierungscodeflow](v2-oauth2-auth-code-flow.md) finden Sie Einzelheiten zum durch diese Methode ausgelösten Autorisierungscodeflow.

# <a name="pythontabpython"></a>[Python](#tab/python)

Das Python-Beispiel verwendet Flask. Die Initialisierung von Flask und MSAL.Python erfolgt in [app.py#L1-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28).

```Python
import uuid
import requests
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)
```

---

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie An- und Abmeldung ausgelöst werden.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [An- und Abmelden](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---
