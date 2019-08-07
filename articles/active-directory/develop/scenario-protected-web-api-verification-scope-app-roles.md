---
title: Geschützte Web-API – App-Code Konfiguration | Azure Active Directory
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
ms.openlocfilehash: b249b99faa62e73b9aa3247f71f88767fca96f01
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488847"
---
# <a name="protected-web-api-adding-authorization-to-your-api"></a>Geschützte Web-API: Hinzufügen von Autorisierung zu Ihrer API

Dieser Artikel beschreibt, wie Sie Ihrer Web-API eine Berechtigung hinzufügen können. Dieser Schutz stellt sicher, dass die API nur aufgerufen wird von:

- Anwendungen im Auftrag von Benutzern, die über den richtigen Bereichen verfügen.
- Daemon-Apps, die die richtigen Anwendungs-Rollen haben.

> [!NOTE]
> Die Codeausschnitte in diesem Artikel wurden aus folgenden voll funktionsfähigen Beispielen extrahiert:
>
> - [Inkrementelles Tutorial zur ASP.NET Core-Web-API](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37) auf GitHub
> - [Beispiel zur ASP.NET-Web-API](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

Um eine ASP.NET/ASP.NET Core Web-API zu schützen, müssen Sie das `[Authorize]` Attribut zu einem dieser Elemente hinzufügen:

- Die Steuerung selbst, wenn Sie möchten, dass alle Aktionen der Steuerung geschützt werden
- Die individuelle Controller-Aktion für Ihre API

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Aber dieser Schutz reicht nicht aus. Es garantiert nur, dass der ASP.NET/ASP.NET Core das Token validiert. Ihre API muss verifizieren, dass das Token, mit dem Ihre Web-API aufgerufen wird, mit den von ihr erwarteten Ansprüchen angefordert wurde, insbesondere:

- Die *Bereiche*, wenn die API im Namen eines Benutzers aufgerufen wird.
- Die *App-Rollen*, wenn API von einer Daemon App aufgerufen wird.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Verifizierung von Scopes in APIs, die im Namen von Benutzern aufgerufen werden

Wenn Ihre API von einer Client-App im Namen eines Benutzers aufgerufen wird, muss sie ein Träger-Token anfordern, das bestimmte Bereiche für die API hat. (Siehe[ Code-Konfiguration | Inhaber-Token ](scenario-protected-web-api-app-configuration.md#bearer-token).)

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

Die `VerifyUserHasAnyAcceptedScope`-Methode würde Folgendes tun:

- Vergewissern Sie sich, dass ein Anspruch mit dem Namen `http://schemas.microsoft.com/identity/claims/scope` oder `scp` vorhanden ist.
- Stellen Sie sicher, dass der Anspruch einen Wert hat, der den von der API erwarteten Umfang enthält.

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

Dieser [Beispielcode](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47) ist für ASP.NET Core. Für ASP.NET ersetzen Sie einfach `HttpContext.User`durch `ClaimsPrincipal.Current`, und ersetzen Sie die Anspruchstyp `"http://schemas.microsoft.com/identity/claims/scope"`durch `"scp"`. (Siehe auch den Code-Ausschnitt weiter unten in diesem Artikel.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Überprüfen von App-Rollen in APIs, die von Daemon-Apps aufgerufen werden

Wenn Ihre Web-API von einer[ Daemon-App](scenario-daemon-overview.md) aufgerufen wird, sollte diese App eine Anwendungsberechtigung für Ihre Web-API benötigen. Wir haben in [Exposing App-Berechtigungen (app roles)](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) gesehen, dass Ihre API solche Berechtigungen bereitstellt (z.B. die `access_as_application` App Rolle).
Sie müssen nun Ihre APIs verifizieren lassen, dass das empfangene Token den `roles`Anspruch enthält und ob dieser Anspruch den erwarteten Wert hat. Der Code, der diese Überprüfung durchführt, ähnelt dem Code, der delegierte Berechtigungen überprüft, mit der Ausnahme, dass anstatt auf `scopes`, Ihre Controlleraktion lediglich auf `roles` testet:

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

Die `ValidateAppRole()` Methode kann Folgendes sein:

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

Dieses Mal gilt der Codeausschnitt für ASP.NET. Für ASP.NET Core ersetzen Sie einfach `ClaimsPrincipal.Current` mit `HttpContext.User`, und ersetzen Sie den `"roles"`-Anspruchsnamen mit `"http://schemas.microsoft.com/identity/claims/roles"`. (Siehe auch den Code-Ausschnitt oben in diesem Artikel.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Das Akzeptieren von App-only-Token, wenn die Web-API nur von Daemon-Apps aufgerufen werden soll

Der `roles`-Anspruch wird auch für Benutzer in Benutzerzuordnungsmustern verwendet. (Siehe [How to: Fügen Sie App-Rollen in Ihre Anwendung ein und erhalten Sie sie im Token](howto-add-app-roles-in-azure-ad-apps.md).) Wenn Sie also nur Rollen überprüfen, können sich Apps als Benutzer anmelden und umgekehrt, wenn die Rollen beiden zuordenbar sind. Wir empfehlen Ihnen, verschiedene Rollen für Benutzer und Apps zu deklarieren, um diese Verwechslung zu vermeiden.

Wenn Sie zulassen möchten, dass nur Daemon-Applikationen Ihre Web-API aufrufen, fügen Sie bei der Validierung der App-Rolle eine Bedingung hinzu, dass das Token ein reines App-Token ist:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Das Überprüfen der inversen Bedingung erlaubt es nur Apps, die sich bei einem Benutzer anmelden, Ihre API aufzurufen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-protected-web-api-production.md)
