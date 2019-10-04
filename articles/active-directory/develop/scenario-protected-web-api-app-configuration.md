---
title: Geschützte Web-API – App-Codekonfiguration | Azure
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
ms.openlocfilehash: 9fdc30df1f932a35702b01d7146017c4ca82c91a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562328"
---
# <a name="protected-web-api-code-configuration"></a>Geschützte Web-API: Codekonfiguration

Um den Code für Ihre geschützte Web-API zu konfigurieren, müssen Sie verstehen, was APIs als geschützt definiert, wie Sie ein Träger-Token konfigurieren und wie Sie das Token validieren.

## <a name="what-defines-aspnetaspnet-core-apis-as-protected"></a>Wodurch werden ASP.NET-/ASP.NET Core-APIs als geschützt definiert?

Wie Webanwendungen sind auch die ASP.NET/ASP.NET Core Web-APIs „geschützt“, da ihren Controller-Aktionen das Attribut `[Authorize]` vorangestellt ist. So können die Controller-Aktionen nur aufgerufen werden, wenn die API mit einer autorisierten Identität aufgerufen wird.

Stellen Sie sich die folgenden Fragen:

- Woher kennt die Web-API die Identität der App, die sie aufruft? (Nur eine App kann eine Web-API aufrufen.)
- Wenn die App die Web-API im Namen eines Benutzers aufgerufen hat, wie lautet die Identität des Benutzers?

## <a name="bearer-token"></a>Bearertoken

Die Informationen über die Identität der App und über den Benutzer (es sei denn, die Web-App akzeptiert Service-to-Service-Aufrufe von einer Daemon-App) werden in dem Träger-Token gespeichert, das beim Aufruf der App im Header gesetzt wird.

Hier ist ein C#-Codebeispiel, das zeigt, wie ein Client die API aufruft, nachdem er ein Token mit der Microsoft Authentication Library for .NET (MSAL.NET) erworben hat:

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
> Das Bearertoken wurde von einer Clientanwendung beim Microsoft Identity Platform-Endpunkt *für die Web-API* angefordert. Die Web-API ist die einzige Anwendung, die das Token verifizieren und die darin enthaltenen Ansprüche anzeigen sollte. Client-Apps sollten niemals versuchen, die Ansprüche in Token zu überprüfen. (Die Web-API könnte in Zukunft verlangen, dass das Token verschlüsselt wird. Diese Anforderung würde den Zugriff für Client-Anwendungen verhindern, die Zugriffstoken anzeigen können.)

## <a name="jwtbearer-configuration"></a>JwtBearer-Konfiguration

In diesem Abschnitt wird beschrieben, wie Sie ein Träger-Token konfigurieren.

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

Wenn eine App bei einer Controller-Aktion aufgerufen wird, die ein `[Authorize]` Attribut enthält, betrachtet der ASP.NET/ASP.NET Core das Träger-Token im Autorisierungsheader der aufrufenden Anforderung und extrahiert das Zugriffstoken. Das Token wird dann an die JwtBearer-Middleware weitergeleitet, die Microsoft IdentityModel Extensions für .NET aufruft.

In ASP.NET Core wird diese Middleware in der Datei Startup.cs initialisiert:

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Die Middleware wird der Web-API durch die folgende Anweisung hinzugefügt:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Derzeit erstellen die ASP.NET Core Vorlagen Azure Active Directory (Azure AD) Web-APIs, die Benutzer innerhalb Ihrer Organisation oder einer anderen Organisation anmelden, nicht mit persönlichen Konten. Sie können sie jedoch leicht so ändern, dass sie den Endpunkt der Microsoft Identitätsplattform verwenden, indem Sie diesen Code zur Datei Startup.cs hinzufügen:

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
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Tokenüberprüfung

Die JwtBearer-Middleware wird, wie die OpenID Connect-Middleware in Webanwendungen, von `TokenValidationParameters` angewiesen, das Token zu validieren. Der Token wird entschlüsselt (je nach Bedarf), die Ansprüche werden extrahiert und die Signatur verifiziert. Die Middleware validiert dann das Token, indem sie nach diesen Daten sucht:

- Es ist auf die Web-API (Zielgruppe) ausgerichtet.
- Es wurde für eine App ausgegeben, die die Web-API aufrufen darf (Sub).
- Es wurde von einem vertrauenswürdigen Security Token Service (STS) (Emittent) ausgegeben.
- Die Tokenlebensdauer befindet sich im zulässigen Bereich (Ablauf).
- Es wurde nicht manipuliert (Signatur).

Es können auch spezielle Validierungen vorliegen. So ist es beispielsweise möglich, zu überprüfen, ob Signaturschlüssel (wenn sie in ein Token eingebettet sind) vertrauenswürdig sind und dass das Token nicht wiedergegeben wird. Für einige Protokolle sind bestimmte Validierungen erforderlich.

### <a name="validators"></a>Validierungssteuerelemente

Die Validierungsschritte werden in Validatoren erfasst, die sich alle in der [Microsoft IdentityModel Extensions for.NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) Open-Source-Bibliothek befinden, in einer Quelldatei: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

Die Validatoren werden in der folgenden Tabelle beschrieben:

| Validierungssteuerelement | BESCHREIBUNG |
|---------|---------|
| `ValidateAudience` | Stellt sicher, dass das Token für die Anwendung bestimmt ist, die das Token (für mich) validiert. |
| `ValidateIssuer` | Stellt sicher, dass das Token von einem vertrauenswürdigen STS (von einer Person, der ich vertraue) ausgestellt wurde. |
| `ValidateIssuerSigningKey` | Stellt sicher, dass die Anwendung, die das Token validiert, dem Schlüssel vertraut, mit dem das Token signiert wurde. (Ausnahme, bei der der Schlüssel in das Token eingebettet ist. In der Regel nicht erforderlich.) |
| `ValidateLifetime` | Stellt sicher, dass das Token noch (oder bereits) gültig ist. Der Validator prüft, ob die Lebensdauer des Token (`notbefore`und `expires` Ansprüche) im Bereich liegt. |
| `ValidateSignature` | Stellt sicher, dass das Token nicht manipuliert wurde. |
| `ValidateTokenReplay` | Stellt sicher, dass das Token nicht wiedergegeben wird. (Sonderfall für einige Einmalgebrauchsprotokolle.) |

Allen Validierungssteuerelementen werden Eigenschaften der `TokenValidationParameters`-Klasse zugeordnet, die selbst über die ASP.NET-/ASP.NET Core-Konfiguration initialisiert werden. In den meisten Fällen müssen Sie die Parameter nicht ändern. Es gibt eine Ausnahme, für Apps, die keine einzelnen Mandanten sind. (Das heißt, Webanwendungen, die Benutzer aus beliebigen Unternehmen oder von persönlichen Microsoft-Konten akzeptieren.) In diesem Fall muss der Aussteller validiert werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereiche und App-Rollen in Ihrem code verifizieren](scenario-protected-web-api-verification-scope-app-roles.md)
