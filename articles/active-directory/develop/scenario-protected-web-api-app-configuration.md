---
title: Geschützte Web-API – App-Codekonfiguration
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
ms.openlocfilehash: a8cc02831fa00a3974da1b74b07daf581f50dd22
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569622"
---
# <a name="protected-web-api-code-configuration"></a>Geschützte Web-API: Codekonfiguration

Um den Code für Ihre geschützte Web-API zu konfigurieren, müssen Sie verstehen, wodurch APIs als geschützt definiert werden, wie Sie ein Bearertoken konfigurieren und wie Sie das Token validieren.

## <a name="what-defines-aspnetaspnet-core-apis-as-protected"></a>Wodurch werden ASP.NET-/ASP.NET Core-APIs als geschützt definiert?

Wie Webanwendungen sind auch die ASP.NET/ASP.NET Core Web-APIs „geschützt“, weil ihren Controlleraktionen das Attribut `[Authorize]` vorangestellt ist. So können die Controlleraktionen nur aufgerufen werden, wenn die API mit einer autorisierten Identität aufgerufen wird.

Stellen Sie sich die folgenden Fragen:

- Woher kennt die Web-API die Identität der App, die sie aufruft? (Nur eine App kann eine Web-API aufrufen.)
- Wenn die App die Web-API im Namen eines Benutzers aufgerufen hat, wie lautet die Identität des Benutzers?

## <a name="bearer-token"></a>Bearertoken

Die Informationen über die Identität der App und über den Benutzer (es sei denn, die Web-App akzeptiert Dienst-zu-Dienst-Aufrufe von einer Daemon-App) werden im Bearertoken gespeichert, das beim Aufruf der App im Header festgelegt wird.

Hier ist ein C#-Codebeispiel, das zeigt, wie ein Client die API aufruft, nachdem er ein Token mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET) erworben hat:

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Das Bearertoken wurde von einer Clientanwendung beim Microsoft Identity Platform-Endpunkt *für die Web-API* angefordert. Die Web-API ist die einzige Anwendung, die das Token verifizieren und die darin enthaltenen Ansprüche anzeigen sollte. Client-Apps dürfen niemals versuchen, die Ansprüche in Token zu überprüfen. (Die Web-API könnte zukünftig anfordern, dass das Token verschlüsselt wird. Diese Anforderung würde den Zugriff durch Client-Apps verhindern, die Zugriffstoken anzeigen können.)

## <a name="jwtbearer-configuration"></a>JwtBearer-Konfiguration

In diesem Abschnitt wird beschrieben, wie Sie ein Bearertoken konfigurieren.

### <a name="config-file"></a>Konfigurationsdatei

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Codeinitialisierung

Wenn eine App für eine Controlleraktion aufgerufen wird, die ein `[Authorize]`-Attribut enthält, untersucht ASP.NET/ASP.NET Core das Bearertoken im Autorisierungsheader der aufrufenden Anforderung und extrahiert das Zugriffstoken. Das Token wird dann an die JwtBearer-Middleware weitergeleitet, die die Microsoft-Identitätsmodellerweiterungen für .NET aufruft.

In ASP.NET Core wird diese Middleware in der Datei „Startup.cs“ initialisiert:

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Die Middleware wird der Web-API durch die folgende Anweisung hinzugefügt:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Aktuell erstellen die ASP.NET Core-Vorlagen Azure Active Directory-Web-APIs, die Benutzer innerhalb Ihrer Organisation oder einer anderen Organisation anmelden, nicht mit persönlichen Konten. Sie können die Vorlagen jedoch problemlos so abändern, dass sie den Endpunkt der Microsoft-Identitätsplattform verwenden, indem Sie diesen Code zur Datei „Startup.cs“ hinzufügen:

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;;
});
```

Dieser Codeausschnitt ist dem ASP.NET Core-Web-API-Tutorial in [Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/154282843da2fc2958fad151e2a11e521e358d42/Microsoft.Identity.Web/WebApiServiceCollectionExtensions.cs#L50-L63) entnommen. Die `AddProtectedWebApi`-Methode, die noch viele weitere Aufgaben ausführt, wird über „Startup.cs“ aufgerufen.

## <a name="token-validation"></a>Tokenüberprüfung

Die JwtBearer-Middleware wird, wie die OpenID Connect-Middleware in Web-Apps, von `TokenValidationParameters` angewiesen, das Token zu validieren. Der Token wird (nach Bedarf) entschlüsselt, die Ansprüche werden extrahiert und die Signatur verifiziert. Die Middleware validiert dann das Token, indem sie auf diese Daten überprüft:

- Das Token ist für die Web-API (Zielgruppe) vorgesehen.
- Das Token wurde für eine App ausgegeben, die die Web-API aufrufen darf (Antragsteller).
- Das Token wurde von einem vertrauenswürdigen Sicherheitstokendienst (Security Token Service, STS) ausgegeben (Aussteller).
- Die Tokenlebensdauer befindet sich im zulässigen Bereich (Ablauf).
- Das Token wurde nicht manipuliert (Signatur).

Es können auch spezielle Validierungen vorliegen. So kann beispielsweise sichergestellt werden, dass die Signaturschlüssel (bei Einbettung in ein Token) vertrauenswürdig sind und das Token nicht wiedergegeben wird. Für einige Protokolle sind bestimmte Validierungen erforderlich.

### <a name="validators"></a>Validierungssteuerelemente

Die Schritte zur Überprüfung werden in Validierungssteuerelementen erfasst, die sich alle in der Open-Source-Bibliothek [Microsoft-Identitätsmodellerweiterungen für .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) in einer Quelldatei befinden: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

Die Validierungssteuerelemente werden in der folgenden Tabelle beschrieben:

| Validierungssteuerelement | BESCHREIBUNG |
|---------|---------|
| `ValidateAudience` | Stellt sicher, dass das Token für die Anwendung bestimmt ist, die das Token (für mich) validiert. |
| `ValidateIssuer` | Stellt sicher, dass das Token von einem vertrauenswürdigen STS (von einem Aussteller, dem ich vertraue) ausgestellt wurde. |
| `ValidateIssuerSigningKey` | Stellt sicher, dass die Anwendung, die das Token validiert, dem Schlüssel vertraut, mit dem das Token signiert wurde. (Sonderfall, bei dem der Schlüssel in das Token eingebettet ist. In der Regel nicht erforderlich.) |
| `ValidateLifetime` | Stellt sicher, dass das Token noch (oder bereits) gültig ist. Das Validierungssteuerelement überprüft, ob die Lebensdauer des Token (`notbefore`- und `expires`-Ansprüche) im gültigen Bereich liegt. |
| `ValidateSignature` | Stellt sicher, dass das Token nicht manipuliert wurde. |
| `ValidateTokenReplay` | Stellt sicher, dass das Token nicht wiedergegeben wird. (Sonderfall für einige Protokolle mit einmaliger Verwendung.) |

Allen Validierungssteuerelementen werden Eigenschaften der `TokenValidationParameters`-Klasse zugeordnet, die selbst über die ASP.NET-/ASP.NET Core-Konfiguration initialisiert werden. In den meisten Fällen müssen Sie die Parameter nicht ändern. Es gibt eine Ausnahme für Apps, bei denen es sich nicht um Einzelmandanten handelt. (Also Webanwendungen, die Benutzer aus einer beliebigen Organisation oder persönlichen Microsoft-Konten akzeptieren.) In diesem Fall muss der Aussteller validiert werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Verifizieren von Bereichen und App-Rollen in Ihrem Code](scenario-protected-web-api-verification-scope-app-roles.md)
