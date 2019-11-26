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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a259fbcf3fde84edccafbcd2fd6594ddb623edfd
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175332"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web-App, die Web-APIs aufruft – Abrufen eines Tokens für die App

Nachdem Sie Ihr Clientanwendungsobjekt erstellt haben, verwenden Sie es, um ein Token zum Aufrufen einer Web-API abzurufen. In ASP.NET oder ASP.NET Core erfolgt dann das Aufrufen einer Web-API im Controller. Dabei werden folgende Schritte ausgeführt:

- Aus dem Tokencache wird ein Token für die Web-API abgerufen. Zum Abrufen dieses Tokens rufen Sie `AcquireTokenSilent` auf.
- Die geschützte API wird über das Zugriffstoken aufgerufen.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Die Controllermethoden sind durch ein `[Authorize]`-Attribut geschützt, mit dem erzwungen wird, dass nur authentifizierte Benutzer die Web-App verwenden dürfen. Mit dem folgenden Code wird Microsoft Graph aufgerufen.

```CSharp
[Authorize]
public class HomeController : Controller
{
 readonly ITokenAcquisition tokenAcquisition;

 public HomeController(ITokenAcquisition tokenAcquisition)
 {
  this.tokenAcquisition = tokenAcquisition;
 }

 // Code for the controller actions(see code below)

}
```

Der `ITokenAcquisition`-Dienst wird von ASP.NET durch die Abhängigkeitsinjektion eingefügt.


Im folgenden Ausschnitt sehen Sie vereinfachten Code für die HomeController-Aktion, mit der das Token für den Aufruf von Microsoft Graph abgerufen wird.

```CSharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenOnBehalfOfUserAsync(scopes);

 // use the access token to call a protected web API
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

Um den für dieses Szenario erforderlichen Code genau zu verstehen, sollten Sie sich den Schritt der 2. Phase ([2.1 Web-App ruft Microsoft Graph auf](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) im Tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) ansehen.

Zusätzliche Komplexität entsteht beispielsweise durch die folgenden Aktionen:

- Aufrufen mehrerer APIs,
- Verarbeiten von inkrementeller Zustimmung und bedingtem Zugriff.

Diese erweiterten Schritte werden in Kapitel 3 des Tutorials [3-WebApp-multi-APIs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs) erörtert.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET ist die Vorgehensweise ähnlich:

- Eine durch das Attribut „[Authorize]“ geschützte Controlleraktion extrahiert die Mandanten- und die Benutzer-ID des `ClaimsPrincipal`-Members aus dem Controller. (ASP.NET verwendet `HttpContext.User`.)
- Anschließend wird ein MSAL.NET-`IConfidentialClientApplication`-Objekt erstellt.
- Zuletzt wird die `AcquireTokenSilent`-Methode der vertraulichen Clientanwendung aufgerufen.

Der Code ähnelt dem für ASP.NET Core dargestellten Code.

# <a name="javatabjava"></a>[Java](#tab/java)

Im Java-Beispiel befindet sich der Code, der eine API aufruft, in der „getUsersFromGraph“-Methode in [AuthPageController.java#L62](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62).

Damit wird versucht, `getAuthResultBySilentFlow` aufzurufen. Wenn der Benutzer mehr Bereichen zustimmen muss, verarbeitet der Code die `MsalInteractionRequiredException`, um den Benutzer aufzufordern.

```java
@RequestMapping("/msal4jsample/graph/me")
public ModelAndView getUserFromGraph(HttpServletRequest httpRequest, HttpServletResponse response)
        throws Throwable {

    IAuthenticationResult result;
    ModelAndView mav;
    try {
        result = authHelper.getAuthResultBySilentFlow(httpRequest, response);
    } catch (ExecutionException e) {
        if (e.getCause() instanceof MsalInteractionRequiredException) {

            // If silent call returns MsalInteractionRequired, then redirect to Authorization endpoint
            // so user can consent to new scopes
            String state = UUID.randomUUID().toString();
            String nonce = UUID.randomUUID().toString();

            SessionManagementHelper.storeStateAndNonceInSession(httpRequest.getSession(), state, nonce);

            String authorizationCodeUrl = authHelper.getAuthorizationCodeUrl(
                    httpRequest.getParameter("claims"),
                    "User.Read",
                    authHelper.getRedirectUriGraph(),
                    state,
                    nonce);

            return new ModelAndView("redirect:" + authorizationCodeUrl);
        } else {

            mav = new ModelAndView("error");
            mav.addObject("error", e);
            return mav;
        }
    }

    if (result == null) {
        mav = new ModelAndView("error");
        mav.addObject("error", new Exception("AuthenticationResult not found in session."));
    } else {
        mav = new ModelAndView("auth_page");
        setAccountInfo(mav, httpRequest);

        try {
            mav.addObject("userInfo", getUserInfoFromGraph(result.accessToken()));

            return mav;
        } catch (Exception e) {
            mav = new ModelAndView("error");
            mav.addObject("error", e);
        }
    }
    return mav;
}
// Code omitted here.
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Im Python-Beispiel befindet sich der Code, der Microsoft Graph aufruft, in [app.py#L53-L62](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62).

Damit wird versucht, ein Token aus dem Tokencache abzurufen, und dann wird die EB-API aufgerufen, nachdem der Autorisierungsheader festgelegt wurde. Wenn dies nicht möglich ist, wird der Benutzer erneut angemeldet.

```python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Aufrufen einer Web-API](scenario-web-app-call-api-call-api.md)
