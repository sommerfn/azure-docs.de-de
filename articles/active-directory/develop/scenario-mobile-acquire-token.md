---
title: Mobile App, die Web-APIs aufruft – Abrufen eines Tokens für die App
titleSuffix: Microsoft identity platform
description: Hier finden Sie Informationen zum Erstellen einer mobilen App, die Web-APIs aufruft (Abrufen eines Tokens für die App).
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b1d6a0e4bfffaaf4ed0fa836bcf62a7f1104e6a
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803769"
---
# <a name="mobile-app-that-calls-web-apis---get-a-token"></a>Mobile App, die Web-APIs aufruft – Abrufen eines Tokens

Bevor Sie geschützte Web-APIs aufrufen können, benötigt Ihre App ein Zugriffstoken. In diesem Artikel werden Sie durch den Prozess zum Abrufen eines Tokens mit der Microsoft Authentication Library (MSAL) geführt.

## <a name="scopes-to-request"></a>Anzufordernde Bereiche

Wenn Sie ein Token anfordern, müssen Sie einen Bereich definieren. Der Gültigkeitsbereich bestimmt, auf welche Daten Ihre App zugreifen kann.  

Die einfachste Vorgehensweise besteht darin, das `App ID URI`-Objekt der Web-API mit dem Bereich `.default` zu kombinieren. Dadurch wird Microsoft Identity Platform darüber informiert, dass Ihre App alle im Portal festgelegten Bereiche benötigt.

#### <a name="android"></a>Android
```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
```

#### <a name="ios"></a>iOS
```swift
let scopes = ["https://graph.microsoft.com/.default"]
```

#### <a name="xamarin"></a>Xamarin
```CSharp 
var scopes = new [] {"https://graph.microsoft.com/.default"};
```

## <a name="get-tokens"></a>Abrufen von Token

### <a name="acquire-tokens-via-msal"></a>Abrufen von Token über MSAL

Mit der MSAL können Apps Token stillschweigend und interaktiv abrufen. Rufen Sie diese Methoden auf, dann gibt MSAL ein Zugriffstoken für die angeforderten Bereiche zurück. Die korrekte Vorgehensweise ist es, eine automatische Anforderung und anschließend eine interaktive Anforderung auszuführen.

#### <a name="android"></a>Android

```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);

// Check if there are any accounts we can sign in silently.
// Result is in the silent callback (success or error).
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {

        if (accounts.isEmpty() && accounts.size() == 1) {
            // TODO: Create a silent callback to catch successful or failed request.
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts or > 1 account. */
        }
    }
});    

[...]

// No accounts found. Interactively request a token.
// TODO: Create an interactive callback to catch successful or failed request.
sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());        
```

#### <a name="ios"></a>iOS

**Versuchen Sie zunächst, ein Token automatisch abzurufen:**

Objective-C:

```objc

NSArray *scopes = @[@"https://graph.microsoft.com/.default"];
NSString *accountIdentifier = @"my.account.id";
    
MSALAccount *account = [application accountForIdentifier:accountIdentifier error:nil];
    
MSALSilentTokenParameters *silentParams = [[MSALSilentTokenParameters alloc] initWithScopes:scopes account:account];
[application acquireTokenSilentWithParameters:silentParams completionBlock:^(MSALResult *result, NSError *error) {
        
    if (!error)
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;
            
        // Access token to call the Web API
        NSString *accessToken = result.accessToken;
    }
        
    // Check the error
    if (error && [error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
    {
        // Interactive auth will be required, call acquireTokenWithParameters:error:
        return;
    }
}];
```
 
Swift:

```swift

let scopes = ["https://graph.microsoft.com/.default"]
let accountIdentifier = "my.account.id"
        
guard let account = try? application.account(forIdentifier: accountIdentifier) else { return }
let silentParameters = MSALSilentTokenParameters(scopes: scopes, account: account)
application.acquireTokenSilent(with: silentParameters) { (result, error) in
            
    guard let authResult = result, error == nil else {
                
    let nsError = error! as NSError
                
    if (nsError.domain == MSALErrorDomain &&
        nsError.code == MSALError.interactionRequired.rawValue) {
                    
            // Interactive auth will be required, call acquireToken()
            return
         }
         return
     }
            
    // You'll want to get the account identifier to retrieve and reuse the account
    // for later acquireToken calls
    let accountIdentifier = authResult.account.identifier
            
    // Access token to call the Web API
    let accessToken = authResult.accessToken
}
```

**Wenn MSAL `MSALErrorInteractionRequired` zurückgibt, versuchen Sie, Token interaktiv abzurufen:**

Objective-C:

```objc
UIViewController *viewController = ...; // Pass a reference to the view controller that should be used when getting a token interactively
MSALWebviewParameters *webParameters = [[MSALWebviewParameters alloc] initWithParentViewController:viewController];
MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopes webviewParameters:webParameters];
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) {
    if (!error) 
    {
        // You'll want to get the account identifier to retrieve and reuse the account
        // for later acquireToken calls
        NSString *accountIdentifier = result.account.identifier;
            
        NSString *accessToken = result.accessToken;
    }
}];
```

Swift:

```swift
let viewController = ... // Pass a reference to the view controller that should be used when getting a token interactively
let webviewParameters = MSALWebviewParameters(parentViewController: viewController)
let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopes, webviewParameters: webviewParameters)
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in
                
    guard let authResult = result, error == nil else {
        print(error!.localizedDescription)
        return
    }
                
    // Get access token from result
    let accessToken = authResult.accessToken
})
```

Msal für iOS und macOS unterstützt verschiedene Modifizierer, wenn Sie ein Token interaktiv oder automatisch abrufen.
* [Allgemeine Parameter beim Abrufen eines Tokens](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALTokenParameters.html#/Configuration%20parameters)
* [Parameter für den interaktiven Tokenabruf](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALInteractiveTokenParameters.html#/Configuring%20MSALInteractiveTokenParameters)
* [Parameter für den automatischen Tokenabruf](https://azuread.github.io/microsoft-authentication-library-for-objc/Classes/MSALSilentTokenParameters.html)

#### <a name="xamarin"></a>Xamarin

Das folgende Beispiel enthält den mindestens erforderlichen Code zum interaktiven Abrufen eines Tokens zum Lesen des Benutzerprofils mit Microsoft Graph.

```CSharp
string[] scopes = new string[] {"user.read"};
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

#### <a name="mandatory-parameters-in-msalnet"></a>Erforderliche Parameter in MSAL.NET

`AcquireTokenInteractive` verfügt über nur einen obligatorischen Parameter ``scopes`` mit einer Enumeration von Zeichenfolgen, die die Bereiche definieren, für die ein Token erforderlich ist. Wenn das Token für Microsoft Graph bestimmt ist, finden sich die erforderlichen Bereiche in der API-Referenz der einzelnen Microsoft Graph-APIs im Abschnitt „Berechtigungen“. Zum [Auflisten der Kontakte des Benutzers](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts) muss beispielsweise der Bereich „User.Read“, „Contacts.Read“ verwendet werden. Siehe auch [Referenz zu Microsoft Graph-Berechtigungen](https://developer.microsoft.com/graph/docs/concepts/permissions_reference).

Wenn Sie die übergeordnete Aktivität beim Erstellen der App unter Android nicht angegeben haben, müssen Sie diese (mit `.WithParentActivityOrWindow`, siehe unten) angeben, sodass das Token nach der Interaktion an die betreffende übergeordnete Aktivität zurückgegeben wird. Wenn Sie sie nicht angeben, wird beim Aufrufen von `.ExecuteAsync()` eine Ausnahme ausgelöst.

#### <a name="specific-optional-parameters-in-msalnet"></a>Spezifische optionale Parameter in MSAL.NET

##### <a name="withprompt"></a>WithPrompt

Mit `WithPrompt()` wird die Interaktivität mit dem Benutzer gesteuert, indem eine Eingabeaufforderung angegeben wird.

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

Die Klasse definiert die folgenden Konstanten:

- ``SelectAccount``: Erzwingt STS, um das Kontoauswahl-Dialogfeld mit Konten anzuzeigen, für die der Benutzer über eine Sitzung verfügt. Diese Option ist nützlich, wenn Anwendungsentwickler Benutzern das Auswählen unter verschiedenen Identitäten ermöglichen möchten. Diese Option bewirkt, dass MSAL ``prompt=select_account`` an den Identitätsanbieter sendet. Diese Option ist die Standardeinstellung. Damit wird die bestmögliche Benutzererfahrung auf Grundlage der verfügbaren Informationen bereitgestellt (Konto, Vorhandensein einer Sitzung für den Benutzer usw. ). Ändern Sie diese Option nicht ohne triftigen Grund.
- ``Consent``: Hiermit kann der Anwendungsentwickler erzwingen, dass der Benutzer um Einwilligung gebeten wird, selbst wenn die Einwilligung zuvor erteilt wurde. In diesem Fall sendet MSAL `prompt=consent` an den Identitätsanbieter. Diese Option kann in bestimmten sicherheitsorientierten Anwendungen verwendet werden, wenn die Governance des Unternehmens vorschreibt, dass für den Benutzer bei jeder Verwendung der Anwendung das Einwilligungsdialogfeld angezeigt wird.
- ``ForceLogin``: Hiermit kann der Anwendungsentwickler angeben, dass der Dienst den Benutzer nach Anmeldeinformationen abfragt, selbst wenn eine solche Benutzerabfrage nicht erforderlich ist. Diese Option kann nützlich sein, wenn das Abrufen eines Tokens fehlschlägt, um dem Benutzer das erneute Anmelden zu ermöglichen. In diesem Fall sendet MSAL `prompt=login` an den Identitätsanbieter. Diese Option wird gelegentlich in sicherheitsorientierten Anwendungen verwendet, wenn die Governance des Unternehmens vorschreibt, dass sich Benutzer beim Zugriff auf bestimmte Teile der Anwendung stets erneut anmelden müssen.
- ``Never`` (nur für .NET 4.5 und WinRT) fragt den Benutzer nicht ab; stattdessen wird versucht, dass in der ausgeblendeten eingebetteten Webansicht gespeicherte Cookie zu verwenden (siehe unten: Webansichten in MSAL.NET). Die Verwendung dieser Option kann fehlschlagen, und in diesem Fall löst `AcquireTokenInteractive` eine Ausnahme aus. Diese weist darauf hin, dass eine UI-Interaktion erforderlich ist und dass Sie einen anderen `Prompt`-Parameter verwenden müssen.
- ``NoPrompt``: Es wird keine Eingabeaufforderung an den Identitätsanbieter gesendet. Diese Option empfiehlt sich nur Azure AD B2C-Richtlinien für die Profilbearbeitung (siehe [B2C-Besonderheiten](https://aka.ms/msal-net-b2c-specificities)).

##### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Dieser Modifizierer wird in erweiterten Szenarien verwendet, in denen der Benutzer vorab seine Einwilligung für verschiedene Ressourcen erteilen soll (und nicht die schrittweise Einwilligung, die normalerweise mit MSAL.NET/Microsoft-Identity Platform v2.0 verwendet wird). Einzelheiten finden Sie unter [Einholen der Vorauseinwilligung des Benutzers für verschiedene Ressourcen](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources).

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

##### <a name="other-optional-parameters"></a>Andere optionale Parameter

Informationen zu allen anderen optionalen Parametern für `AcquireTokenInteractive` finden Sie in der Referenzdokumentation für [AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods).

### <a name="acquire-tokens-via-the-protocol"></a>Abrufen von Token über das Protokoll

Die direkte Verwendung des Protokolls wird nicht empfohlen. Wenn Sie dies tun, werden einige Szenarien für einmaliges Anmelden (Single Sign-On, SSO), die Geräteverwaltung und den bedingten Zugriff von der App nicht unterstützt.

Wenn Sie das Protokoll zum Abrufen von Token für mobile Apps verwenden, müssen Sie zwei Anforderungen ausführen: Sie müssen einen Autorisierungscode abrufen und diesen gegen ein Token eintauschen.

#### <a name="get-authorization-code"></a>Abrufen des Autorisierungscodes

```Text
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=<CLIENT_ID>
&response_type=code
&redirect_uri=<ENCODED_REDIRECT_URI>
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2F.default
&state=12345
```

#### <a name="get-access-and-refresh-token"></a>Abrufen von Zugriffs- und Aktualisierungstoken

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=<ENCODED_REDIRECT_URI>
&grant_type=authorization_code
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aufrufen einer Web-API](scenario-mobile-call-api.md)
