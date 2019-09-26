---
title: Web-App für Benutzeranmeldungen (Anmeldung) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App zur Benutzeranmeldung erstellen (Anmeldung).
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ed61a09ffc76b4813dcb97330d3a1a436aa16eb
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086460"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Web-App für Benutzeranmeldungen: Anmeldung

Erfahren Sie, wie Sie dem Code für Ihre Web-App eine Anmeldefunktion zur Benutzeranmeldung hinzufügen.

## <a name="sign-in"></a>Anmeldung

Der im vorherigen Artikel zur [App-Codekonfiguration](scenario-web-app-sign-user-app-configuration.md) erörterte Code ist alles, was Sie zum Implementieren der Anmeldung benötigen.
Nachdem der Benutzer sich bei Ihrer App angemeldet hat, möchten Sie ihm wahrscheinlich auch das Abmelden ermöglichen. ASP.NET Core verarbeitet die Abmeldung für Sie.

## <a name="what-sign-out-involves"></a>Voraussetzungen für die Abmeldung

Beim Abmelden von einer Web-App geht es um mehr als um das Entfernen der Informationen über das angemeldete Konto aus dem Status der Web-App.
Die Web-App muss den Benutzer für die Abmeldung außerdem an den `logout`-Endpunkt von Microsoft Identity Platform umleiten. Wenn Ihre Web-App den Benutzer an den `logout`-Endpunkt umleitet, löscht dieser Endpunkt die Sitzung des Benutzers aus dem Browser. Wenn die App nicht zum `logout`-Endpunkt wechselt, kann sich der Benutzer erneut für Ihre App authentifizieren, ohne die Anmeldeinformationen erneut einzugeben, weil der Benutzer nach wie vor über eine gültige SSO-Sitzung (Single Sign-On, Einmaliges Anmelden) beim Microsoft Identity Platform-Endpunkt verfügt.

Weitere Informationen finden Sie im Abschnitt [Senden einer Abmeldeanforderung](v2-protocols-oidc.md#send-a-sign-out-request) in der konzeptionellen Dokumentation zu [Microsoft Identity Platform und OpenID Connect-Protokoll](v2-protocols-oidc.md).

### <a name="application-registration"></a>Anwendungsregistrierung

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Bei der Anwendungsregistrierung haben Sie einen **URI nach der Abmeldung** registriert. In diesem Tutorial haben Sie auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** die URL `https://localhost:44321/signout-oidc` registriert. Weitere Informationen finden Sie unter [Registrieren der webApp-App](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp).

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Bei der Anwendungsregistrierung haben Sie einen **URI nach der Abmeldung** registriert. In diesem Tutorial haben Sie auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** die URL `https://localhost:44308/Account/EndSession` registriert. Weitere Informationen finden Sie unter [Registrieren der webApp-App](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet).

# <a name="javatabjava"></a>[Java](#tab/java)

Bei der Anwendungsregistrierung registrieren Sie einen **URI nach der Abmeldung**. In diesem Tutorial haben Sie auf der Seite **Authentifizierung** im Abschnitt **Erweiterte Einstellungen** im Feld **Abmelde-URL** die URL `http://localhost:8080/msal4jsample/` registriert.

# <a name="pythontabpython"></a>[Python](#tab/python)

Bei der Anwendungsregistrierung registrieren Sie keinen URI nach der Abmeldung. Das Beispiel implementiert keine globale Abmeldung.

---

### <a name="sign-out-button"></a>Abmeldeschaltfläche

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Die Abmeldeschaltfläche wird in ASP.NET Core in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht und nur angezeigt, wenn ein authentifiziertes Konto vorliegt (wenn sich der Benutzer also zuvor angemeldet hat).

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Die Abmeldeschaltfläche wird in ASP.NET MVC in `Views\Shared\_LoginPartial.cshtml` verfügbar gemacht und nur angezeigt, wenn ein authentifiziertes Konto vorliegt (wenn sich der Benutzer also zuvor angemeldet hat).

```html
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

In unserem Java-Schnellstart befindet sich die Abmeldeschaltfläche in der Datei „main/resources/templates/auth_page.html“.

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Im Python-Schnellstart befindet sich die Abmeldeschaltfläche in der Datei [templates/display.html](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e1199b4c3cdcb637cf0d8306832efbd85492e123/templates/display.html#L18-L20).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Acquire Token Result </title>
</head>
<body>
    {% if cond  %}
        <p1><b>Your information</b> </p1>
        <table>
        {% for key, value in auth_result.items() %}
           <tr>
                <th> {{ key }} </th>
                <td> {{ value }} </td>
           </tr>
        {% endfor %}
        </table>
        <form action="/logout" >
            <input type="submit" value=" Logout"/>
        </form>
    {% else %}
        <p1><b> {{auth_result}} </b> </p1>
        <form action="/authenticate" >
            <input type="submit" value=" Sign-in"/>
        </form>
    {% endif %}
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>`Signout()`-Aktion des Controllers

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

In ASP.NET wird durch Auswahl der Schaltfläche **Abmelden** in der Web-App die Aktion `SignOut` auf dem `AccountController`-Controller ausgelöst. In früheren Versionen von ASP.NET Core-Vorlagen wurde der `Account`-Controller in die Web-App eingebettet. Dies ist jedoch nicht mehr der Fall, weil er jetzt Bestandteil des ASP.NET Core-Frameworks selbst ist.

Der Code für den `AccountController` steht im ASP.NET Core-Repository in [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs) zur Verfügung. Die Kontosteuerung führt folgende Aktionen durch:

- Sie legt einen OpenID-Umleitungs-URI auf `/Account/SignedOut` fest, damit der Controller zurückgerufen wird, wenn Azure AD die Abmeldung durchgeführt hat.
- Sie ruft `Signout()` auf, damit die OpenIdConnect-Middleware den Microsoft Identity Platform-`logout`-Endpunkt kontaktieren kann, der:

  - den Sitzungscookie aus dem Browser löscht und
  - einen Rückruf für die **Abmelde-URL** ausführt, die standardmäßig die als Bestandteil von ASP.NET Core bereitgestellte Seite mit der Abmeldeansicht [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) anzeigt.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET wird die Abmeldung von der `SignOut()`-Methode auf einem Controller ausgelöst (z. B. AccountController). Diese Methode gehört nicht zum ASP.NET-Framework (im Gegensatz zu den Vorgängen in ASP.NET Core). Sie verwendet nicht `async`, und aus diesem Grund geschieht Folgendes:

- Sie sendet eine OpenId-Abmeldeaufforderung.
- Sie löscht den Cache.
- Sie leitet auf die gewünschte Seite um.

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 Response.Redirect("/");
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

In Java wird die Abmeldung behandelt, indem der Abmeldeendpunkt der Microsoft Identity Platform direkt aufgerufen und der „post_logout_redirect_uri“ bereitgestellt wird.

```Java
@RequestMapping("/msal4jsample/sign_out")
    public void signOut(HttpServletRequest httpRequest, HttpServletResponse response) throws IOException {

        httpRequest.getSession().invalidate();

        String endSessionEndpoint = "https://login.microsoftonline.com/common/oauth2/v2.0/logout";

        String redirectUrl = "http://localhost:8080/msal4jsample/";
        response.sendRedirect(endSessionEndpoint + "?post_logout_redirect_uri=" +
                URLEncoder.encode(redirectUrl, "UTF-8"));
    }
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
@app.route("/logout")
def logout():
    return flask.redirect(flask.url_for('index'))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Abfangen des Aufrufs an den `logout`-Endpunkt

Der URI nach der Abmeldung ermöglicht Anwendungen, an der globalen Abmeldung teilzunehmen.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Mit der OpenIdConnect-Middleware von ASP.NET Core kann Ihre App den Aufruf an den Microsoft Identity Platform-`logout`-Endpunkt durch die Bereitstellung eines OpenId Connect-Ereignisses namens `OnRedirectToIdentityProviderForSignOut` abfangen. Ein Beispiel zum Abonnieren dieses Ereignisses (um den Tokencache zu löschen) finden Sie unter [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156).

```CSharp
               // Handling the global sign-out
                options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
                {
                    // Forget about the signed-in user
                };
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

In ASP.NET delegieren Sie die Ausführung der Abmeldung an die Middleware, indem Sie das Sitzungscookie löschen:

```CSharp
public class AccountController : Controller
{
 ...
 public void EndSession()
 {
  Request.GetOwinContext().Authentication.SignOut();
  Request.GetOwinContext().Authentication.SignOut(Microsoft.AspNet.Identity.DefaultAuthenticationTypes.ApplicationCookie);
  this.HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
 }
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

In unserem Java-Schnellstart zeigt der Umleitungs-URI nach der Abmeldung nur die Seite „index.html“ an. 

# <a name="pythontabpython"></a>[Python](#tab/python)

Im Python-Schnellstart zeigt der Umleitungs-URI nach der Abmeldung nur die Seite „index.html“ an.

---

## <a name="protocol"></a>Protocol

Weitere Informationen zum Abmelden finden Sie in der Protokolldokumentation, die über [Microsoft Identity Platform und das OpenID Connect-Protokoll](./v2-protocols-oidc.md) verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-web-app-sign-user-production.md)
