---
title: Überprüfen von Bereichen und App-Rollen mit geschützter Web-API
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen und den Code Ihrer Anwendung konfigurieren.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4340d92bdfe871010021edcbefcde62ab8202462
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73149238"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>Geschützte Web-API: Überprüfen von Bereichen und App-Rollen

Dieser Artikel beschreibt, wie Sie Ihrer Web-API eine Berechtigung hinzufügen können. Dieser Schutz stellt sicher, dass die API nur aufgerufen wird von:

- Anwendungen im Auftrag von Benutzern, die über die richtigen Bereiche verfügen
- Daemon-Apps mit den richtigen Anwendungsrollen

> [!NOTE]
> Die Codeausschnitte in diesem Artikel wurden aus folgenden voll funktionsfähigen Beispielen extrahiert:
>
> - [Inkrementelles Tutorial zur ASP.NET Core-Web-API](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37) auf GitHub
> - [Beispiel zur ASP.NET-Web-API](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

Um eine ASP.NET/ASP.NET Core-Web-API zu schützen, müssen Sie das `[Authorize]`-Attribut einem dieser Elemente hinzufügen:

- dem Controller selbst, wenn alle Aktionen des Controllers geschützt werden sollen
- einer individuellen Controlleraktion für Ihre API

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Aber dieser Schutz reicht nicht aus. Es wird nur garantiert, dass ASP.NET/ASP.NET Core das Token validiert. Ihre API muss verifizieren, ob das zum Aufrufen Ihrer Web-API verwendete Token mit den erwarteten Ansprüchen angefordert wurde. Dazu zählen insbesonders:

- Die *Bereiche*, wenn die API im Namen eines Benutzers aufgerufen wird.
- Die *App-Rollen*, wenn die API von einer Daemon-App aufgerufen wird.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Überprüfen von Bereichen in APIs, die im Namen von Benutzern aufgerufen werden

Wenn Ihre API von einer Client-App im Namen eines Benutzers aufgerufen wird, muss sie ein Bearertoken anfordern, das bestimmte Bereiche für die API enthält. (Siehe [Code-Konfiguration | Bearertoken](scenario-protected-web-api-app-configuration.md#bearer-token).)

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

Mit der `VerifyUserHasAnyAcceptedScope`-Methode würde etwa Folgendes passieren:

- Überprüfen, ob ein Anspruch mit dem Namen `http://schemas.microsoft.com/identity/claims/scope` oder `scp` vorhanden ist.
- Überprüfen, ob der Anspruch einen Wert hat, der den von der API erwarteten Bereich enthält.

```CSharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

Dieser [Beispielcode](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47) ist für ASP.NET Core. Ersetzen Sie bei ASP.NET einfach `HttpContext.User` durch `ClaimsPrincipal.Current`, und ersetzen Sie den Anspruchstyp `"http://schemas.microsoft.com/identity/claims/scope"` durch `"scp"`. (Beachten Sie auch den Codeausschnitt weiter unten in diesem Artikel.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Überprüfen von App-Rollen in APIs, die von Daemon-Apps aufgerufen werden

Wenn Ihre Web-API von einer [Daemon-App](scenario-daemon-overview.md) aufgerufen wird, sollte diese App eine Anwendungsberechtigung für Ihre Web-API anfordern. Unter [Verfügbarmachen von Anwendungsberechtigungen (App-Rollen)](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) haben Sie gesehen, dass Ihre API solche Berechtigungen verfügbar macht (z.B. die App Rolle `access_as_application`).
Ihre APIs müssen nun überprüfen, ob das empfangene Token den `roles`-Anspruch enthält und ob dieser Anspruch den erwarteten Wert hat. Der Code für diese Überprüfung ähnelt dem Code, der delegierte Berechtigungen überprüft, mit der Ausnahme, dass Ihre Controlleraktion jetzt `roles`, und nicht `scopes` testet:

```CSharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

Die `ValidateAppRole()`-Methode könnte etwa wie folgt aussehen:

```CSharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage 
        { StatusCode = HttpStatusCode.Unauthorized, 
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found" 
        });
    }
}
}
```

Dieses Mal gilt der Codeausschnitt für ASP.NET. Ersetzen Sie bei ASP.NET Core einfach `ClaimsPrincipal.Current` durch `HttpContext.User` und den Anspruchsnamen `"roles"` durch `"http://schemas.microsoft.com/identity/claims/roles"`. (Beachten Sie auch den Codeausschnitt weiter oben in diesem Artikel.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Akzeptieren von reinen App-Token, wenn die Web-API nur von Daemon-Apps aufgerufen werden sollte

Der `roles`-Anspruch wird auch für Benutzer in Benutzerzuweisungsmustern verwendet. (Siehe hierzu [Gewusst wie: Hinzufügen von App-Rollen in Ihrer Anwendung und Empfangen der Rollen im Token](howto-add-app-roles-in-azure-ad-apps.md).) Wenn Sie also nur Rollen überprüfen, können sich Apps als Benutzer anmelden und umgekehrt, wenn die Rollen beiden zuweisbar sind. Wir empfehlen, verschiedene Rollen für Benutzer und Apps zu deklarieren, um diese Verwechslung zu vermeiden.

Wenn nur Daemon-Apps Ihre Web-API aufrufen sollen, fügen Sie bei der Validierung der App-Rolle die Bedingung hinzu, dass das Token ein reines App-Token ist:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Durch das Aktivieren der inversen Bedingung können nur Apps, die einen Benutzer anmelden, Ihre API aufrufen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-protected-web-api-production.md)
