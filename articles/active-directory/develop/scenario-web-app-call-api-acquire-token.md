---
title: Web-App, die Web-APIs aufruft (Abrufen eines Tokens für die App) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Web-APIs aufruft (Abruf eines Tokens für die App).
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
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3aa144c76fb0a8e479658efdb5d43361fbbc085c
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860626"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web-App, die Web-APIs aufruft – Abrufen eines Tokens für die App

Nachdem Sie Ihr Clientanwendungsobjekt erstellt haben, verwenden Sie es, um ein Token zum Aufrufen einer Web-API abzurufen. In ASP.NET oder ASP.NET Core erfolgt dann das Aufrufen einer Web-API im Controller. Dabei werden folgende Schritte ausgeführt:

- Aus dem Tokencache wird ein Token für die Web-API abgerufen. Zum Abrufen dieses Tokens rufen Sie `AcquireTokenSilent` auf.
- Die geschützte API wird über das Zugriffstoken aufgerufen.

## <a name="aspnet-core"></a>ASP.NET Core

Die Controllermethoden sind durch ein `[Authorize]`-Attribut geschützt, mit dem erzwungen wird, dass nur authentifizierte Benutzer die Web-App verwenden dürfen. Mit dem folgenden Code wird Microsoft Graph aufgerufen.

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

Im folgenden Ausschnitt sehen Sie vereinfachten Code für die HomeController-Aktion, mit der das Token für den Aufruf von Microsoft Graph abgerufen wird.

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

Um den für dieses Szenario erforderlichen Code genau zu verstehen, sollten Sie sich den Schritt der 2. Phase ([2.1 Web-App ruft Microsoft Graph auf](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) im Tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) ansehen.

Zusätzliche Komplexität entsteht beispielsweise durch die folgenden Aktionen:

- Implementieren eines Tokencaches für die Web-App (im Tutorial werden mehrere Implementierungen aufgeführt)
- Entfernen des Kontos aus dem Cache, wenn sich der Benutzer abmeldet
- Aufrufen mehrerer APIs und Verwenden der inkrementellen Einwilligung

## <a name="aspnet"></a>ASP.NET

In ASP.NET ist die Vorgehensweise ähnlich:

- Eine durch das Attribut „[Authorize]“ geschützte Controlleraktion extrahiert die Mandanten- und die Benutzer-ID des `ClaimsPrincipal`-Members aus dem Controller. (ASP.NET verwendet `HttpContext.User`.)
- Anschließend wird ein MSAL.NET-`IConfidentialClientApplication`-Objekt erstellt.
- Zuletzt wird die `AcquireTokenSilent`-Methode der vertraulichen Clientanwendung aufgerufen.

Der Code ähnelt dem für ASP.NET Core dargestellten Code.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aufrufen einer Web-API](scenario-web-app-call-api-call-api.md)
