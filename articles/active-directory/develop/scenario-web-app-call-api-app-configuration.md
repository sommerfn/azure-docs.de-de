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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 231ecdb6afae1fc36d11b2c12aa82c7e860bb708
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175322"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web-App zum Aufruf von Web-APIs – Codekonfiguration

Wie unter [Szenario: Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md) veranschaulicht,verwendet die Web-App den OAuth 2.0-[Autorisierungscodeflow](v2-oauth2-auth-code-flow.md), um den Benutzer anzumelden. Dieser Flow besteht aus zwei Teilen:

1. Anfordern eines Autorisierungscodes. Dieser Teil delegiert einen privaten Dialog mit dem Benutzer an die Microsoft Identity Platform. Der Benutzer meldet sich an und stimmt der Verwendung von Web-APIs zu. Wenn dieser private Dialog erfolgreich beendet wird, erhält die Anwendung einen Autorisierungscode für den Umleitungs-URI.
1. Anfordern eines Zugriffstokens für die API durch Einlösen des Autorisierungscodes.

Im Artikel [Szenario: Web-App, die Benutzer anmeldet](scenario-web-app-sign-user-overview.md) wurde nur die erste Phase ausgeführt. Hier erfahren Sie, wie Sie Ihre Web-App, die Benutzer anmeldet, so ändern, dass sie jetzt Web-APIs aufruft.

## <a name="libraries-supporting-web-app-scenarios"></a>Bibliotheken mit Unterstützung für Web-App-Szenarios

Die folgenden Bibliotheken unterstützen den Autorisierungscodeflow für Web-Apps:

| MSAL-Bibliothek | BESCHREIBUNG |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Unterstützt werden die Plattformen .NET Framework und .NET Core (nicht UWP, Xamarin.iOS und Xamarin.Android, weil mit diesen Plattformen öffentliche Clientanwendungen erstellt werden). |
| ![MSAL Python](media/sample-v2-code/logo_python.png) <br/> MSAL Python | In Entwicklung – Public Preview |
| ![MSAL Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | In Entwicklung – Public Preview |

Wählen Sie die Registerkarte aus, die der Plattform entspricht, die Sie interessiert:

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Wenn eine Benutzeranmeldung an die Middleware von OpenID Connect (OIDC) delegiert wird, möchten Sie eine Verknüpfung mit dem OIDC-Prozess herstellen. Die Vorgehensweise ist dabei je nach dem von Ihnen verwendeten Framework unterschiedlich.
Im Fall von ASP.NET Core abonnieren Sie OIDC-Middlewareereignisse. Das Prinzip ist Folgendes:

- Sie lassen ASP.NET Core einen Autorisierungscode über die OpenID Connect-Middleware anfordern. Hierdurch gestattet ASP.NET/ASP.NET Core dem Benutzer, sich anzumelden und seine Einwilligung zu erteilen.
- Sie abonnieren den Empfang des Autorisierungscodes durch die Web-App. Dieses Abonnement erfolgt über einen C#-Delegaten.
- Wenn der Autorisierungscode empfangen wird, verwenden Sie MSAL-Bibliotheken, um den Code einzulösen, und die daraus resultierenden Zugriffstoken und Aktualisierungstoken werden im Tokencache gespeichert. Von dort aus kann der Cache in anderen Teilen der Anwendung (beispielsweise in Controllern) verwendet werden, um weitere Token im Hintergrund abzurufen.

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus [Kapitel 2 des inkrementellen Tutorials zu ASP.NET Core-Web-Apps](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph). In diesem Tutorial finden Sie auch ausführliche Informationen zur Implementierung.

> [!NOTE]
> Um die unten aufgeführten Codeausschnitte vollständig verstehen zu können, müssen Sie mit den [ASP.NET Core-Grundlagen](https://docs.microsoft.com/aspnet/core/fundamentals) und insbesondere mit der [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) und den [Optionen](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options) vertraut sein.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Wenn eine Benutzeranmeldung an die Middleware von OpenID Connect (OIDC) delegiert wird, möchten Sie eine Verknüpfung mit dem OIDC-Prozess herstellen. Die Vorgehensweise ist dabei je nach dem von Ihnen verwendeten Framework unterschiedlich.
Im Fall von ASP.NET abonnieren Sie OIDC-Middlewareereignisse. Das Prinzip ist Folgendes:

- Sie lassen ASP.NET Core einen Autorisierungscode über die OpenID Connect-Middleware anfordern. Hierdurch gestattet ASP.NET/ASP.NET Core dem Benutzer, sich anzumelden und seine Einwilligung zu erteilen.
- Sie abonnieren den Empfang des Autorisierungscodes durch die Web-App. Dies ist ein C#-Delegat.
- Wenn der Autorisierungscode empfangen wird, verwenden Sie MSAL-Bibliotheken, um den Code einzulösen. Die daraus resultierenden Zugriffstoken und Aktualisierungstoken werden dann im Tokencache gespeichert. Von dort aus kann der Cache in anderen Teilen der Anwendung (beispielsweise in Controllern) verwendet werden, um weitere Token im Hintergrund abzurufen.

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem [ASP.NET-Web-App-Beispiel](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect). Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="javatabjava"></a>[Java](#tab/java)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem MSAL-Java-Web-App-Beispiel [Java-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp).
Im Beispiel wird aktuell von MSAL Java die Autorisierungscode-URL erzeugt und die Navigation zum Microsoft Identity Platform-Autorisierungsendpunkt gesteuert. Es ist auch möglich, die Sprint-Sicherheit für die Anmeldung des Benutzers zu verwenden. Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

# <a name="pythontabpython"></a>[Python](#tab/python)

Die Codeausschnitte in diesem und den folgenden Artikeln stammen aus dem Web-App-Beispiel „MSAL.Python“ unter [Python-Webanwendung für den Aufruf von Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp).
Im Beispiel wird derzeit von MSAL.Python die Autorisierungscode-URL erzeugt und die Navigation zum Microsoft Identity Platform-Autorisierungsendpunkt gesteuert. Dieses Beispiel enthält auch ausführliche Informationen zur Implementierung.

---

## <a name="code-that-redeems-the-authorization-code"></a>Code, der den Autorisierungscode einlöst

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

### <a name="startupcs"></a>Startup.cs

In ASP.NET Core gilt das Prinzip in der Datei `Startup.cs`. Abonnieren Sie das OpenID Connect-Ereignis `OnAuthorizationCodeReceived`, und rufen Sie über dieses Ereignis die MSAL.NET-Methode `AcquireTokenFromAuthorizationCode` auf. Dadurch werden im Tokencache das Zugriffstoken für die angeforderten `scopes` und ein Aktualisierungstoken gespeichert, das verwendet wird, um das Zugriffstoken kurz vor Ablauf zu aktualisieren oder ein Token im Namen desselben Benutzers, aber für eine andere Ressource abzurufen.

In der Praxis wird im [Tutorial zu ASP.NET Core-Web-Apps](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) versucht, wiederverwendbaren Code für Ihre Web-Apps bereitzustellen.

Unter [Startup.cs#L40-L42](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Startup.cs#L40-L42) finden Sie den Code, der den Aufruf der `AddMicrosoftIdentityPlatformAuthentication`-Methode enthält, die der Web-App Authentifizierung hinzufügt, und die `AddMsal`-Methode, mit der die Funktion zum Aufrufen von Web-APIs hinzugefügt wird. Beim Aufrufen von `AddInMemoryTokenCaches` geht es um die Auswahl einer der möglichen Tokencache-Implementierungen:

```CSharp
public class Startup
{
  // Code not show here

  public void ConfigureServices(IServiceCollection services)
  {
      // Token acquisition service based on MSAL.NET
      // and chosen token cache implementation
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
          .AddMsal(Configuration, new string[] { Constants.ScopeUserRead })
          .AddInMemoryTokenCaches();
  }

  // Code not show here
}
```

`Constants.ScopeUserRead` ist in [Constants.cs#L5](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Infrastructure/Constants.cs#L5) definiert.

```CSharp
public static class Constants
{
    public const string ScopeUserRead = "User.Read";
}
```

Sie haben unter [Web-App, die Benutzer anmeldet – Codekonfiguration](scenario-web-app-sign-user-app-configuration.md?tabs=aspnetcore#initialization-code) bereits den Inhalt von `AddMicrosoftIdentityPlatformAuthentication` untersucht.

### <a name="the-addmsal-method"></a>Die „AddMsal“-Methode

Den Code für `AddMsal` finden Sie unter [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159).

```CSharp

/// <summary>
/// Extensions for IServiceCollection for startup initialization.
/// </summary>
public static class WebAppServiceCollectionExtensions
{
  // Code omitted here

  /// <summary>
  /// Add MSAL support to the Web App or Web API
  /// </summary>
  /// <param name="services">Service collection to which to add authentication</param>
  /// <param name="initialScopes">Initial scopes to request at sign-in</param>
  /// <returns></returns>
  public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
  {
      // Ensure that configuration options for MSAL.NET, HttpContext accessor and the Token acquisition service
      // (encapsulating MSAL.NET) are available through dependency injection
      services.Configure<ConfidentialClientApplicationOptions>(options => configuration.Bind(configSectionName, options));
      services.AddHttpContextAccessor();
      services.AddTokenAcquisition();

      services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
      {
          // Response type
          options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

          // This scope is needed to get a refresh token when users sign-in with their Microsoft personal accounts
          // (it's required by MSAL.NET and automatically provided when users sign-in with work or school accounts)
          options.Scope.Add("offline_access");
          if (initialScopes != null)
          {
              foreach (string scope in initialScopes)
              {
                  if (!options.Scope.Contains(scope))
                  {
                      options.Scope.Add(scope);
                  }
              }
          }

          // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
          // where it will be usable from Controllers later (through the TokenAcquisition service)
          var handler = options.Events.OnAuthorizationCodeReceived;
          options.Events.OnAuthorizationCodeReceived = async context =>
          {
              var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
              await tokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync(context, options.Scope).ConfigureAwait(false);
              await handler(context).ConfigureAwait(false);
          };
      });
      return services;
  }
}
```

Mit der `AddMsal`-Methode wird Folgendes sichergestellt:

- Die ASP.NET Core-Web-App fordert sowohl ein ID-Token für den Benutzer als auch einen Authentifizierungscode an (`options.ResponseType = OpenIdConnectResponseType.CodeIdToken`).
- Der Bereich `offline_access` wird hinzugefügt. Dies ist erforderlich, damit der Benutzer zustimmt, dass die Anwendung ein Aktualisierungstoken erhalten kann.
- Die App abonniert das OIDC-Ereignis `OnAuthorizationCodeReceived` und löst den Aufruf mithilfe von MSAL.NET ein, der hier in eine wiederverwendbare Komponente gekapselt ist, die `ITokenAcquisition` implementiert.

### <a name="the-tokenacquisitionaddaccounttocachefromauthorizationcodeasync-method"></a>Die „TokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync“-Methode

Die `TokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync`-Methode finden Sie unter [Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145). Damit wird Folgendes sichergestellt:

- ASP.NET versucht nicht, den Authentifizierungscode parallel zu MSAL.NET einzulösen (`context.HandleCodeRedemption();`).
- Die Ansprüche im ID-Token stehen MSAL zur Verfügung, um einen Tokencacheschlüssel für das Konto des Benutzers zu berechnen.
- Die MSAL.NET-Anwendung wird bei Bedarf instanziiert.
- Der Code wird von der MSAL.NET-Anwendung eingelöst.
- Das neue ID-Token wird (während des Aufrufs von `context.HandleCodeRedemption(null, result.IdToken);`) für ASP.NET Core freigegeben. Das Zugriffstoken wird nicht für ASP.NET Core freigegeben. Es verbleibt im MSAL.NET-Tokencache, der dem Benutzer zugeordnet ist, und kann in ASP.NET Core-Controllern verwendet werden.

```csharp
public class TokenAcquisition : ITokenAcquisition
{
  string[] scopesRequestedByMsalNet = new string[]{ "openid", "profile", "offline_access" };

  // Code omitted here for clarity


  public async Task AddAccountToCacheFromAuthorizationCodeAsync(AuthorizationCodeReceivedContext context, IEnumerable<string> scopes)
  {
   // Code omitted here for clarity

    try
    {
      // As AcquireTokenByAuthorizationCodeAsync is asynchronous we want to tell ASP.NET core that we are handing the code
      // even if it's not done yet, so that it does not concurrently call the Token endpoint. (otherwise there will be a
      // race condition ending-up in an error from Azure AD telling "code already redeemed")
      context.HandleCodeRedemption();

      // The cache will need the claims from the ID token.
      // If they are not yet in the HttpContext.User's claims, so adding them here.
      if (!context.HttpContext.User.Claims.Any())
      {
          (context.HttpContext.User.Identity as ClaimsIdentity).AddClaims(context.Principal.Claims);
      }

      var application = GetOrBuildConfidentialClientApplication();

      // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it and will not send the OAuth 2.0 request in
      // case a further call to AcquireTokenByAuthorizationCodeAsync in the future is required for incremental consent (getting a code requesting more scopes)
      // Share the ID Token though
      var result = await application
          .AcquireTokenByAuthorizationCode(scopes.Except(_scopesRequestedByMsalNet), context.ProtocolMessage.Code)
          .ExecuteAsync()
          .ConfigureAwait(false);

      context.HandleCodeRedemption(null, result.IdToken);
  }
  catch (MsalException ex)
  {
      Debug.WriteLine(ex.Message);
      throw;
  }
 }
```

### <a name="the-tokenacquisitionbuildconfidentialclientapplication-method"></a>Die „TokenAcquisition.BuildConfidentialClientApplication“-Methode

In ASP.NET Core werden beim Erstellen der vertraulichen Clientanwendung Informationen verwendet, die sich im HttpContext befinden. Der HttpContext, der der Anforderung zugeordnet ist und über die `CurrentHttpContext`-Eigenschaft aufgerufen wird, kennt die URL für die Web-App und den angemeldeten Benutzer (in einem `ClaimsPrincipal`). Die `BuildConfidentialClientApplication` verwendet auch die ASP.NET Core-Konfiguration, die einen Abschnitt „AzureAD“ umfasst und an die folgenden Elemente gebunden ist:

- Die `_applicationOptions`-Datenstruktur vom Typ [ConfidentialClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationoptions?view=azure-dotnet)
- Die `azureAdOptions`-Instanz vom Typ [AzureAdOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/AzureADOptions.cs), der in ASP.NET Core `Authentication.AzureAD.UI` definiert ist. Außerdem muss die Anwendung Tokencaches verwalten. Weitere Informationen hierzu finden Sie im nächsten Abschnitt.

Den Code für die `GetOrBuildConfidentialClientApplication()`-Methode finden Sie unter [Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333). Darin werden Elemente verwendet, die von der Abhängigkeitsinjektion eingefügt wurden (übergeben im Konstruktor von „TokenAcquisition“ in [Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59)).

```CSharp
public class TokenAcquisition : ITokenAcquisition
{
  // Code omitted here for clarity

  // Members
  private IConfidentialClientApplication application;
  private HttpContext CurrentHttpContext => _httpContextAccessor.HttpContext;

  // The following members are set by dependency injection in the TokenAcquisition constructor
  private readonly AzureADOptions _azureAdOptions;
  private readonly ConfidentialClientApplicationOptions _applicationOptions;
  private readonly IMsalAppTokenCacheProvider _appTokenCacheProvider;
  private readonly IMsalUserTokenCacheProvider _userTokenCacheProvider;
  private readonly IHttpContextAccessor _httpContextAccessor;

  /// <summary>
  /// Creates an MSAL Confidential client application if needed
  /// </summary>
  private IConfidentialClientApplication GetOrBuildConfidentialClientApplication()
  {
    if (application == null)
    {
        application = BuildConfidentialClientApplication();
    }
    return application;
  }

  /// <summary>
  /// Creates an MSAL Confidential client application
  /// </summary>
  /// <param name="claimsPrincipal"></param>
  /// <returns></returns>
  private IConfidentialClientApplication BuildConfidentialClientApplication()
  {
    var request = CurrentHttpContext.Request;
    var azureAdOptions = _azureAdOptions;
    var applicationOptions = _applicationOptions;
    string currentUri = UriHelper.BuildAbsolute(
        request.Scheme,
        request.Host,
        request.PathBase,
        azureAdOptions.CallbackPath ?? string.Empty);

    string authority = $"{applicationOptions.Instance}{applicationOptions.TenantId}/";

    var app = ConfidentialClientApplicationBuilder
        .CreateWithApplicationOptions(applicationOptions)
        .WithRedirectUri(currentUri)
        .WithAuthority(authority)
        .Build();

    // Initialize token cache providers
    _appTokenCacheProvider?.InitializeAsync(app.AppTokenCache);
    _userTokenCacheProvider?.InitializeAsync(app.UserTokenCache);

    return app;
  }

```

### <a name="summary"></a>Zusammenfassung

Kurz gesagt, `AcquireTokenByAuthorizationCode` löst tatsächlich den von ASP.NET angeforderten Autorisierungscode ein und ruft die Token ab, die dem MSAL.NET-Benutzertokencache hinzugefügt werden. Von dort aus werden sie anschließend in den ASP.NET Core-Controllern verwendet.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Die Verarbeitung durch ASP.NET ist mit der von ASP.NET Core vergleichbar, mit der Ausnahme, dass die Konfiguration von OpenID Connect und das Abonnement des `OnAuthorizationCodeReceived`-Ereignisses in der Datei [App_Start\Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs) erfolgen. Sie werden ähnliche Konzepte wie in ASP.NET Core vorfinden – mit dem Unterschied, dass Sie in ASP.NET den Umleitungs-URI (RedirectUri) in [Web.config#L15](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/master/WebApp/Web.config#L15) angeben. Diese Konfiguration ist etwas weniger stabil als in ASP.NET Core, da Sie beim Bereitstellen Ihrer Anwendung Änderungen vornehmen müssen.

```CSharp
public partial class Startup
{
  public void ConfigureAuth(IAppBuilder app)
  {
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    // Custom middleware initialization. This is activated when the code obtained from a code_grant is present in the querystring (&code=<code>).
    app.UseOAuth2CodeRedeemer(
        new OAuth2CodeRedeemerOptions
        {
            ClientId = AuthenticationConfig.ClientId,
            ClientSecret = AuthenticationConfig.ClientSecret,
            RedirectUri = AuthenticationConfig.RedirectUri
        }
      );

  app.UseOpenIdConnectAuthentication(
      new OpenIdConnectAuthenticationOptions
      {
        // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
        Authority = AuthenticationConfig.Authority,
        ClientId = AuthenticationConfig.ClientId,
        RedirectUri = AuthenticationConfig.RedirectUri,
        PostLogoutRedirectUri = AuthenticationConfig.RedirectUri,
        Scope = AuthenticationConfig.BasicSignInScopes + " Mail.Read", // a basic set of permissions for user sign in & profile access "openid profile offline_access"
        TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = false,
            // In a real application you would use IssuerValidator for additional checks, like making sure the user's organization has signed up for your app.
            //     IssuerValidator = (issuer, token, tvp) =>
            //     {
            //        //if(MyCustomTenantValidation(issuer))
            //        return issuer;
            //        //else
            //        //    throw new SecurityTokenInvalidIssuerException("Invalid issuer");
            //    },
            //NameClaimType = "name",
        },
        Notifications = new OpenIdConnectAuthenticationNotifications()
        {
            AuthorizationCodeReceived = OnAuthorizationCodeReceived,
            AuthenticationFailed = OnAuthenticationFailed,
        }
      });
  }

  private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
  {
      // Upon successful sign in, get the access token & cache it using MSAL
      IConfidentialClientApplication clientApp = MsalAppBuilder.BuildConfidentialClientApplication(new ClaimsPrincipal(context.AuthenticationTicket.Identity));
      AuthenticationResult result = await clientApp.AcquireTokenByAuthorizationCode(new[] { "Mail.Read" }, context.Code).ExecuteAsync();
  }

  private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
  {
      notification.HandleResponse();
      notification.Response.Redirect("/Error?message=" + notification.Exception.Message);
      return Task.FromResult(0);
  }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

Lesen Sie [Web-App, die Benutzer anmeldet – Codekonfiguration](scenario-web-app-sign-user-app-configuration.md?tabs=java#initialization-code), um zu verstehen, wie das Java-Beispiel den Autorisierungscode abruft. Nachdem der Code von der App empfangen wurde, delegiert [AuthFilter.java#L51-L56](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java#L51-L56) an die `AuthHelper.processAuthenticationCodeRedirect`-Methode in [AuthHelper.java#L67-L97](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L67-L97) und ruft dann `getAuthResultByAuthCode` auf:

```Java
class AuthHelper {
  // code omitted
  void processAuthenticationCodeRedirect(HttpServletRequest httpRequest, String currentUri, String fullUrl)
            throws Throwable {

  // code omitted
  AuthenticationResponse authResponse = AuthenticationResponseParser.parse(new URI(fullUrl), params);

  // code omitted
  IAuthenticationResult result = getAuthResultByAuthCode(
                    httpRequest,
                    oidcResponse.getAuthorizationCode(),
                    currentUri);

// code omitted
  }
}
```

Die `getAuthResultByAuthCode`-Methode ist in [AuthHelper.java#L176](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L176) definiert. Sie erstellt eine MSAL-`ConfidentialClientApplication` und ruft `acquireToken()` mit den aus dem Autorisierungscode erstellten `AuthorizationCodeParameters` auf.

```Java
   private IAuthenticationResult getAuthResultByAuthCode(
            HttpServletRequest httpServletRequest,
            AuthorizationCode authorizationCode,
            String currentUri) throws Throwable {

        IAuthenticationResult result;
        ConfidentialClientApplication app;
        try {
            app = createClientApplication();

            String authCode = authorizationCode.getValue();
            AuthorizationCodeParameters parameters = AuthorizationCodeParameters.builder(
                    authCode,
                    new URI(currentUri)).
                    build();

            Future<IAuthenticationResult> future = app.acquireToken(parameters);

            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        }

        if (result == null) {
            throw new ServiceUnavailableException("authentication result was null");
        }

        SessionManagementHelper.storeTokenCacheInSession(httpServletRequest, app.tokenCache().serialize());

        return result;
    }

    private ConfidentialClientApplication createClientApplication() throws MalformedURLException {
        return ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(clientSecret)).
                authority(authority).
                build();
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Sobald der Autorisierungscodeflow angefordert wird (wie unter [Web-App, die Benutzer anmeldet – Codekonfiguration](scenario-web-app-sign-user-app-configuration.md?tabs=python#initialization-code) aufgezeigt), erfolgt der Empfang des Autorisierungscodes über die Funktion `authorized`, die Flask von der /getAToken-URL weiterleitet. Siehe [app.py#L30-L44](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L30-L44)

```python
 @app.route("/getAToken")  # Its absolute URL must match your app's redirect_uri set in AAD
def authorized():
    if request.args['state'] != session.get("state"):
        return redirect(url_for("login"))
    cache = _load_cache()
    result = _build_msal_app(cache).acquire_token_by_authorization_code(
        request.args['code'],
        scopes=app_config.SCOPE,  # Misspelled scope would cause an HTTP 400 error here
        redirect_uri=url_for("authorized", _external=True))
    if "error" in result:
        return "Login failure: %s, %s" % (
            result["error"], result.get("error_description"))
    session["user"] = result.get("id_token_claims")
    _save_cache(cache)
    return redirect(url_for("index"))
```

---

Die vertrauliche Clientanwendung kann ihre Identität statt mit einem geheimen Clientschlüssel auch mithilfe eines Clientzertifikats oder einer Clientassertion nachweisen.
Die Verwendung von Clientassertionen ist ein erweitertes Szenario, das unter [Clientassertionen](msal-net-client-assertions.md) detailliert erörtert wird.

## <a name="token-cache"></a>Tokencache

> [!IMPORTANT]
> In Web-Apps (oder eigentlich Web-APIs) unterscheidet sich die Implementierung des Tokencaches von den Tokencache-Implementierungen von Desktopanwendungen (die häufig [dateibasiert](scenario-desktop-acquire-token.md#file-based-token-cache) sind).
> Aus Sicherheits- und Leistungsgründen ist es wichtig, sicherzustellen, dass für Web-Apps und Web-APIs ein Tokencache pro Benutzer (pro Konto) verfügbar ist. Sie müssen den Tokencache für jedes Konto serialisieren.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Im ASP.NET Core-Tutorial wird die Abhängigkeitsinjektion verwendet, um Ihnen die Entscheidung über die Tokencache-Implementierung in der Datei „Startup.cs“ für Ihre Anwendung zu ermöglichen. „Microsoft.Identity.Web“ enthält eine Reihe vordefinierter Tokencache-Serialisierungsmodule, die unter [Tokencacheserialisierung](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/README.md#token-cache-serialization) beschrieben sind. Eine interessante Möglichkeit besteht darin, [verteilte Arbeitsspeichercaches](https://docs.microsoft.com/aspnet/core/performance/caching/distributed#distributed-memory-cache) von ASP.NET Core auszuwählen:

```csharp
// or use a distributed Token Cache by adding
    services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
            .AddMsal(new string[] { scopesToRequest })
            .AddDistributedTokenCaches();

// and then choose your implementation

// For instance the distributed in memory cache (not cleared when you stop the app)
services.AddDistributedMemoryCache()

// Or a Redis cache
services.AddStackExchangeRedisCache(options =>
{
 options.Configuration = "localhost";
 options.InstanceName = "SampleInstance";
});

// Or even a SQL Server token cache
services.AddDistributedSqlServerCache(options =>
{
 options.ConnectionString = _config["DistCache_ConnectionString"];
 options.SchemaName = "dbo";
 options.TableName = "TestCache";
});
```

Ausführliche Informationen zu den Tokencacheanbietern finden Sie auch in den [Tutorials zu ASP.NET Core-Web-Apps | Tokencaches](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache) in der entsprechenden Phase des Tutorials.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

In Web-Apps (oder eigentlich Web-APIs) unterscheidet sich die Implementierung des Tokencaches von den Tokencache-Implementierungen von Desktopanwendungen (die häufig [dateibasiert](scenario-desktop-acquire-token.md#file-based-token-cache) sind). Die ASP.NET-Sitzung oder der Serverarbeitsspeicher kann verwendet werden. Unter [MsalAppBuilder.cs#L39-L51](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Utils/MsalAppBuilder.cs#L39-L51) können Sie beispielsweise sehen, wie die Implementierung des Caches nach der Erstellung der MSAL.NET-Anwendung verknüpft ist.

```csharp
public static class MsalAppBuilder
{
 // Omitted code
    public static IConfidentialClientApplication BuildConfidentialClientApplication(ClaimsPrincipal currentUser)
    {
      IConfidentialClientApplication clientapp = ConfidentialClientApplicationBuilder.Create(AuthenticationConfig.ClientId)
            .WithClientSecret(AuthenticationConfig.ClientSecret)
            .WithRedirectUri(AuthenticationConfig.RedirectUri)
            .WithAuthority(new Uri(AuthenticationConfig.Authority))
            .Build();

      // After the ConfidentialClientApplication is created, we overwrite its default UserTokenCache with our implementation
      MSALPerUserMemoryTokenCache userTokenCache = new MSALPerUserMemoryTokenCache(clientapp.UserTokenCache, currentUser ?? ClaimsPrincipal.Current);

      return clientapp;
  }
```

# <a name="javatabjava"></a>[Java](#tab/java)

MSAL Java stellt Methoden zum Serialisieren und Deserialisieren des Tokencaches bereit. Das Java-Beispiel behandelt die Serialisierung in der Sitzung, wie in der `getAuthResultBySilentFlow`-Methode in [AuthHelper.java#L99-L122](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L99-L122) veranschaulicht.

```Java
IAuthenticationResult getAuthResultBySilentFlow(HttpServletRequest httpRequest, HttpServletResponse httpResponse)
      throws Throwable {

  IAuthenticationResult result =  SessionManagementHelper.getAuthSessionObject(httpRequest);

  IConfidentialClientApplication app = createClientApplication();

  Object tokenCache = httpRequest.getSession().getAttribute("token_cache");
  if (tokenCache != null) {
      app.tokenCache().deserialize(tokenCache.toString());
  }

  SilentParameters parameters = SilentParameters.builder(
          Collections.singleton("User.Read"),
          result.account()).build();

  CompletableFuture<IAuthenticationResult> future = app.acquireTokenSilently(parameters);
  IAuthenticationResult updatedResult = future.get();

  //update session with latest token cache
  SessionManagementHelper.storeTokenCacheInSession(httpRequest, app.tokenCache().serialize());

  return updatedResult;
}
```

Die Details der `SessionManagementHelper`-Klasse werden unter [](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/SessionManagementHelper.java) bereitgestellt.

# <a name="pythontabpython"></a>[Python](#tab/python)

Im Python-Beispiel wird die Unterstützung für den Cache nach Konto durch erneutes Erstellen einer vertraulichen Clientanwendung für jede Anforderung und deren Serialisierung im Flask-Sitzungscache sichergestellt:

```python
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)

# Code omitted here for simplicity

def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)
```

---

## <a name="next-steps"></a>Nächste Schritte

Zu diesem Zeitpunkt wird bei der Anmeldung des Benutzers ein Token im Tokencache gespeichert. Sehen wir uns an, wie dieses dann in anderen Teilen der Web-App verwendet wird.

> [!div class="nextstepaction"]
> [Bei der Web-App anmelden](scenario-web-app-call-api-sign-in.md)
