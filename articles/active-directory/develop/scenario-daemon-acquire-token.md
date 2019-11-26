---
title: Daemon-App, die Web-APIs aufruft (Aufrufen von Token für die App) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Daemon-App erstellen, die Web-APIs aufruft (beim Aufrufen von Token)
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a5a3ac1438d5e958317f1899fc6c447f5c149ac
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175518"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Daemon-App, die Web-APIs aufruft – Aufruf eines Tokens

Sobald die vertrauliche Clientanwendung erstellt wurde, können Sie ein Token für die App abrufen, indem Sie ``AcquireTokenForClient`` aufrufen, den Bereich übergeben und ggf. die Aktualisierung des Tokens erzwingen.

## <a name="scopes-to-request"></a>Anzufordernde Bereiche

Der anzufordernde Bereich für einen Anmeldeinformationsfluss für Clients ist der Name der Ressource, gefolgt von `/.default`. Durch diese Notation weiß Azure AD, dass die **Berechtigungen auf Anwendungsebene** verwendet werden sollen, die im Zuge der Anwendungsregistrierung statisch deklariert wurden. Zudem müssen diese API-Berechtigungen, wie zuvor festgestellt, von einem Mandantenadministrator gewährt werden.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

In MSAL Python sähe die Konfigurationsdatei wie der folgende Codeausschnitt aus:

```Json
{
    "scope": ["https://graph.microsoft.com/.default"],
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
```

---

### <a name="case-of-azure-ad-v10-resources"></a>Bei Azure AD-Ressourcen (V1.0)

Der für die Clientanmeldeinformationen verwendete Bereich muss immer resourceId+"/.default" sein.

> [!IMPORTANT]
> Wenn MSAL bei einem Zugriffstoken nach einer Ressource fragt, die ein v1.0-Zugriffstoken akzeptiert, analysiert Azure AD die gewünschte Zielgruppe aus dem angeforderten Bereich, indem alles vor dem letzten Schrägstrich als Ressourcenbezeichner verwendet wird.
> Wenn die Ressource wie Azure SQL ( **https://database.windows.net** ) daher eine Zielgruppe mit Schrägstrich am Ende erwartet (bei Azure SQL: `https://database.windows.net/` ), müssen Sie den Bereich `https://database.windows.net//.default` anfordern (beachten Sie den doppelten Schrägstrich). Weitere Informationen finden Sie unter dem MSAL.NET-Issue [#747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Resource url's trailing slash is omitted, which caused sql auth failure (Bei der URL der Ressource wurde der nachgestellte Schrägstrich entfernt, wodurch bei der SQL-Authentifizierung ein Fehler aufgetreten ist).

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient-API

Zum Abrufen eines Tokens für die App wird `AcquireTokenForClient` oder die Entsprechung für die jeweilige Plattform verwendet.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```CSharp
using Microsoft.Identity.Client;

// With client credentials flows the scopes is ALWAYS of the shape "resource/.default", as the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application does not have sufficient permissions
    // - did you declare enough app permissions in during the app creation?
    // - did the tenant admin needs to grant permissions to the application.
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be of the form "https://resourceurl/.default"
    // Mitigation: change the scope to be as expected !
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
# The pattern to acquire a token looks like this.
result = None

# Firstly, looks up a token from cache
# Since we are looking for token for the current app, NOT for an end user,
# notice we give account parameter as None.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])

if "access_token" in result:
    # Call a protected API with the access token
    print(result["token_type"])
else:
    print(result.get("error"))
    print(result.get("error_description"))
    print(result.get("correlation_id"))  # You may need this when reporting a bug
```

# <a name="javatabjava"></a>[Java](#tab/java)

Dies ist ein Auszug aus den [MSAL-Java-Entwicklungsbeispielen](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/).

```Java
ClientCredentialParameters clientCredentialParam = ClientCredentialParameters.builder(
        Collections.singleton(GRAPH_DEFAULT_SCOPE))
        .build();

CompletableFuture<IAuthenticationResult> future = app.acquireToken(clientCredentialParam);

BiConsumer<IAuthenticationResult, Throwable> processAuthResult = (res, ex) -> {
    if (ex != null) {
        System.out.println("Oops! We have an exception - " + ex.getMessage());
    }
    System.out.println("Returned ok - " + res);
    System.out.println("ID Token - " + res.idToken());

    /* call a protected API with res.accessToken() */
};

future.whenCompleteAsync(processAuthResult);
future.join();
```

---

### <a name="protocol"></a>Protocol

Wenn Sie noch über keine Bibliothek für die Programmiersprache Ihrer Wahl verfügen, möchten Sie das Protokoll möglicherweise direkt verwenden:

#### <a name="first-case-access-token-request-with-a-shared-secret"></a>Erster Fall: Zugriffstokenanforderung mit einem gemeinsamen Geheimnis

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-token-request-with-a-certificate"></a>Zweiter Fall: Zugriffstokenanforderung mit einem Zertifikat

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

Weitere Informationen finden Sie in der Protokolldokumentation: [Microsoft Identity Platform und der Fluss von OAuth 2.0-Clientanmeldeinformationen](v2-oauth2-client-creds-grant-flow.md).

## <a name="application-token-cache"></a>Anwendungstokencache

In MSAL.NET verwendet `AcquireTokenForClient` den **Anwendungstokencache** (alle anderen AcquireTokenXX-Methoden verwenden den Benutzertokencache). Rufen Sie `AcquireTokenSilent` nicht vor `AcquireTokenForClient` auf, da `AcquireTokenSilent` den **Benutzertokencache** verwendet. `AcquireTokenForClient` überprüft den **Anwendungstokencache** selbst und aktualisiert diesen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="did-you-use-the-resourcedefault-scope"></a>Haben Sie den Bereich „resource/.default“ verwendet?

Wenn Ihnen eine Fehlermeldung angezeigt wird, nach der Sie einen ungültigen Bereich verwendet haben, haben Sie wahrscheinlich nicht den Bereich `resource/.default` verwendet.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Haben Sie die Administratoreinwilligung vergessen? Diese wird von Daemon-Apps benötigt!

Wenn Ihnen beim Aufruf der API die Fehlermeldung **Nicht genügend Berechtigungen zum Abschließen des Vorgangs** angezeigt wird, muss der Mandantenadministrator der Anwendung Berechtigungen gewähren. Weitere Informationen finden Sie weiter oben unter Schritt 6 des Abschnitts über die Registrierung der Client-App.
Ihnen wird in der Regel eine Fehlermeldung wie in der folgenden Beschreibung angezeigt:

```JSon
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon-App – Aufrufen einer Web-API](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon-App – Aufrufen einer Web-API](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon-App – Aufrufen einer Web-API](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=java)

---
