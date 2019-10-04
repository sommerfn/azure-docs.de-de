---
title: Web-App zum Aufruf von Web-APIs (Codekonfiguration) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Web-APIs aufruft (Codekonfiguration der App)
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 391546b4d3ac9ad3674897b39284fdd16e9025a1
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562267"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web-App zum Aufruf von Web-APIs – Codekonfiguration

Wie im [Szenario der Benutzeranmeldung über eine Web-App](scenario-web-app-sign-user-overview.md) gezeigt, sollten Sie sich angesichts der Tatsache, dass die Anmeldung des Benutzer an die OIDC-Middleware (Open ID Connect) delegiert wird, in den OIDC-Prozess einklinken. Die Vorgehensweise hierzu unterscheidet sich je nach verwendetem Framework (in diesem Fall ASP.NET und ASP.NET Core), aber letztlich werden Middleware-OIDC-Ereignisse abonniert. Das Prinzip ist Folgendes:

- Sie lassen ASP.NET oder ASP.NET Core einen Autorisierungscode anfordern. Hierdurch gestattet ASP.NET/ASP.NET Core dem Benutzer, sich anzumelden und seine Einwilligung zu erteilen.
- Sie abonnieren den Empfang des Autorisierungscodes durch die Web-App.
- Wenn der Autorisierungscode empfangen wird, verwenden Sie MSAL-Bibliotheken zum Einlösen des Codes, und die daraus resultierenden Zugriffstoken und Aktualisierungstoken werden im Tokencache gespeichert. Von dort aus kann der Cache in anderen Teilen der Anwendung verwendet werden, um weitere Token im Hintergrund abzurufen.

> [!NOTE]
> Die in diesem Artikel enthaltenen Codeausschnitte wurden aus den folgenden voll funktionsfähigen Beispielen auf GitHub extrahiert:
>
> - [Inkrementelles Tutorial zur ASP.NET Core-Web-App](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)
> - [ASP.NET-Web-App-Beispiel](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotheken mit Unterstützung für Web-App-Szenarios

Folgende Bibliotheken unterstützen den Autorisierungscodeflow für Web-Apps:

| MSAL-Bibliothek | BESCHREIBUNG |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Unterstützt werden die Plattformen .NET Framework und .NET Core (nicht UWP, Xamarin.iOS und Xamarin.Android, weil mit diesen Plattformen öffentliche Clientanwendungen erstellt werden). |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | In Entwicklung – Public Preview |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | In Entwicklung – Public Preview |

## <a name="aspnet-core-configuration"></a>ASP.NET Core-Konfiguration

In ASP.NET Core werden die Vorgänge in der Datei `Startup.cs` durchgeführt. Abonnieren Sie das OpenID Connect-Ereignis `OnAuthorizationCodeReceived`, und rufen Sie über dieses Ereignis die MSAL.NET-Methode `AcquireTokenFromAuthorizationCode` auf. Dadurch werden im Tokencache das Zugriffstoken für die angeforderten `scopes` und ein Aktualisierungstoken gespeichert, das verwendet wird, um das Zugriffstoken kurz vor Ablauf zu aktualisieren oder ein Token im Namen desselben Benutzers, aber für eine andere Ressource abzurufen.

```CSharp
string[] scopes = new string[]{ "user.read" };
string[] scopesRequestedByMsalNet = new string[]{ "openid", "profile", "offline_access" };
```

Die Kommentare im folgenden Code helfen Ihnen beim Verständnis einiger komplizierter Aspekte beim Kombinieren von MSAL.NET und ASP.NET Core. Weitere Informationen finden Sie im Artikel [ASP.NET Core Web app incremental tutorial, chapter 2 (Inkrementelles Tutorial zu ASP.NET Core-Web-Apps, Kapitel 2)](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph).

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add("offline_access");
   options.Scope.Add("user.read"); // for instance

   // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
   // where it will be usable from Controllers later (through the TokenAcquisition service)
   var handler = options.Events.OnAuthorizationCodeReceived;
   options.Events.OnAuthorizationCodeReceived = async context =>
   {
    // As AcquireTokenByAuthorizationCode is asynchronous we want to tell ASP.NET core
    // that we are handing the code even if it's not done yet, so that it does 
    // not concurrently call the Token endpoint.
    context.HandleCodeRedemption();

    // Call MSAL.NET AcquireTokenByAuthorizationCode
    var application = BuildConfidentialClientApplication(context.HttpContext,
                                                         context.Principal);
    var result = await application.AcquireTokenByAuthorizationCode(scopes.Except(scopesRequestedByMsalNet), context.ProtocolMessage.Code)
                                  .ExecuteAsync();

    // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it
    // and will not send the OAuth 2.0 request in case a further call to
    // AcquireTokenByAuthorizationCodeAsync in the future for incremental consent 
    // (getting a code requesting more scopes)
    // Share the ID Token so that the identity of the user is known in the application (in 
    // HttpContext.User)
    context.HandleCodeRedemption(null, result.IdToken);

    // Call the previous handler if any
    await handler(context);
   };
```

In ASP.NET Core werden beim Erstellen der vertraulichen Clientanwendung Informationen verwendet, die sich im HttpContext befinden. Dieser `HttpContext` kennt die URL für die Web-App und den angemeldeten Benutzer (in einem `ClaimsPrincipal`). 

Darüber hinaus verwendet er die ASP.NET Core-Konfiguration, die einen Abschnitt „AzureAD“ umfasst und an die folgenden Elemente gebunden ist:

- Die `_applicationOptions`-Datenstruktur vom Typ [ConfidentialClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationoptions?view=azure-dotnet)
- Die `azureAdOptions`-Instanz vom Typ [AzureAdOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/AzureADOptions.cs), der in ASP.NET Core `Authentication.AzureAD.UI` definiert ist. Außerdem muss die Anwendung Tokencaches verwalten.

```CSharp
/// <summary>
/// Creates an MSAL Confidential client application
/// </summary>
/// <param name="httpContext">HttpContext associated with the OIDC response</param>
/// <param name="claimsPrincipal">Identity for the signed-in user</param>
/// <returns></returns>
private IConfidentialClientApplication BuildConfidentialClientApplication(HttpContext httpContext, ClaimsPrincipal claimsPrincipal)
{
 var request = httpContext.Request;

 // Find the URI of the application)
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, azureAdOptions.CallbackPath ?? string.Empty);

 // Updates the authority from the instance (including national clouds) and the tenant
 string authority = $"{azureAdOptions.Instance}{azureAdOptions.TenantId}/";

 // Instantiates the application based on the application options (including the client secret)
 var app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
               .WithRedirectUri(currentUri)
               .WithAuthority(authority)
               .Build();

 // Initialize token cache providers. In the case of Web applications, there must be one
 // token cache per user (here the key of the token cache is in the claimsPrincipal which
 // contains the identity of the signed-in user)
 if (UserTokenCacheProvider != null)
 {
  UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (AppTokenCacheProvider != null)
 {
  AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

Ausführliche Informationen zu den Tokencacheanbietern finden Sie in den [Tutorials zu ASP.NET Core-Web-Apps | Tokencaches](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/455d32f09f4f6647b066ebee583f1a708376b12f/2-WebApp-graph-user/2-2-TokenCache).

> [!NOTE]
> `AcquireTokenByAuthorizationCode` löst eigentlich den von ASP.NET angeforderten Autorisierungscode ein und ruft die Token ab, die dem MSAL.NET-Benutzertokencache hinzugefügt werden. Von dort aus werden sie anschließend in den ASP.NET Core-Controllern verwendet.

## <a name="aspnet-configuration"></a>ASP.NET-Konfiguration

Die Verarbeitung durch ASP.NET verläuft ähnlich, außer dass die Konfiguration von OpenIdConnect und das Abonnement des `OnAuthorizationCodeReceived`-Ereignisses in der Datei `App_Start\Startup.Auth.cs` erfolgen. Sie werden ähnliche Konzepte vorfinden mit dem Unterschied, dass Sie hier den RedirectUri in der Konfigurationsdatei angeben. Diese Variante ist nicht ganz so robust:

```CSharp
private void ConfigureAuth(IAppBuilder app)
{
 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

 app.UseCookieAuthentication(new CookieAuthenticationOptions { });

 app.UseOpenIdConnectAuthentication(
 new OpenIdConnectAuthenticationOptions
 {
  Authority = Globals.Authority,
  ClientId = Globals.ClientId,
  RedirectUri = Globals.RedirectUri,
  PostLogoutRedirectUri = Globals.RedirectUri,
  Scope = Globals.BasicSignInScopes, // a basic set of permissions for user sign in & profile access
  TokenValidationParameters = new TokenValidationParameters
  {
   NameClaimType = "name",
  },
  Notifications = new OpenIdConnectAuthenticationNotifications()
  {
   AuthorizationCodeReceived = OnAuthorizationCodeReceived,
  }
 });
}
```

```CSharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
{
 // Upon successful sign in, get & cache a token using MSAL
 string userId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;
 IConfidentialClientApplication clientapp;
 clientApp = ConfidentialClientApplicationBuilder.Create(Globals.ClientId)
                  .WithClientSecret(Globals.ClientSecret)
                  .WithRedirectUri(Globals.RedirectUri)
                  .WithAuthority(new Uri(Globals.Authority))
                  .Build();

  EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);

 AuthenticationResult result = await clientapp.AcquireTokenByAuthorizationCode(scopes, context.Code)
                                              .ExecuteAsync();
}
```

Schließlich kann die vertrauliche Clientanwendung ihre Identität statt mit einem geheimen Clientschlüssel auch mithilfe eines Clientzertifikats oder einer Clientassertion nachweisen.
Die Verwendung von Clientassertionen ist ein erweitertes Szenario, das unter [Clientassertionen](msal-net-client-assertions.md) detailliert erörtert wird.

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>MSAL.NET-Tokencache für eine ASP.NET-/ASP.NET Core-Web-App

In Web-Apps (oder eigentlich Web-APIs) unterscheidet sich die Implementierung des Tokencaches von den Tokencache-Implementierungen von Desktopanwendungen (die häufig [dateibasiert](scenario-desktop-acquire-token.md#file-based-token-cache) sind). Dabei kann die ASP.NET-/ASP.NET Core-Sitzung, ein Redis Cache, eine Datenbank oder sogar Azure Blob Storage verwendet werden. Im obigen Codeausschnitt ist dies das Objekt des `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);`-Methodenaufrufs, von dem ein Cachedienst gebunden wird. Die Details zu den genauen Abläufen würden den Rahmen dieses Szenarioleitfadens überschreiten, aber Links werden unten bereitgestellt.

> [!IMPORTANT]
> Ein wichtiger Aspekt ist, dass für Web-Apps und Web-APIs ein Tokencache pro Benutzer (pro Konto) verfügbar sein muss. Sie müssen den Tokencache für jedes Konto serialisieren.

Beispiele zur Verwendung von Tokencaches für Web-Apps und Web-APIs finden Sie im [Tutorial zur ASP.NET Core-Web-App](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) in Phase [2-2 Token Cache](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Informationen zu Implementierungen finden Sie im Ordner [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) in der Bibliothek [microsoft-authentication-extensions-for-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) (im Ordner [Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web)).

## <a name="next-steps"></a>Nächste Schritte

Zu diesem Zeitpunkt wird bei der Anmeldung des Benutzers ein Token im Tokencache gespeichert. Sehen wir uns an, wie dieses dann in anderen Teilen der Web-App verwendet wird.

> [!div class="nextstepaction"]
> [Bei der Web-App anmelden](scenario-web-app-call-api-sign-in.md)
