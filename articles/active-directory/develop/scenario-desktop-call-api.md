---
title: Desktop-App, die Web-APIs aufruft (Aufrufen einer Web-API) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (Aufrufen einer Web-API)
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56d3d01e39adfeb6bf2ef5e7e7d595f49c90f5a5
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268278"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Desktop-App, die Web-APIs aufruft – Aufrufen einer Web-API

Da Sie nun über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="calling-a-web-api-from-net"></a>Aufrufen einer Web-API über .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-a-web-api-in-msal-for-ios-and-macos"></a>Aufrufen einer Web-API in MSAL für iOS und macOS

Diese Methoden zum Abrufen von Token geben ein `MSALResult`-Objekt zurück. `MSALResult` stellt eine `accessToken`-Eigenschaft bereit, die verwendet werden kann, um eine Web-API aufzurufen. Das Zugriffstoken muss dem HTTP-Autorisierungsheader hinzugefügt werden, bevor der Aufruf zum Zugriff auf die geschützte Web-API erfolgt.

Objective-C:

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };
        
NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift:

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Aufrufen mehrerer APIs – Inkrementelle Zustimmung und bedingter Zugriff

Wenn Sie mehrere APIs für den gleichen Benutzer aufrufen müssen, können Sie, sobald Sie ein Token für die erste API abgerufen haben, einfach `AcquireTokenSilent` aufrufen. Dadurch erhalten Sie in den meisten Fällen automatisch ein Token für die anderen APIs.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

In folgenden Fällen müssen Sie aktiv werden:

- Der Benutzer hat der ersten API zugestimmt, muss nun aber weiteren Bereichen zustimmen (inkrementelle Zustimmung).
- Für die erste API war keine mehrstufige Authentifizierung erforderlich, für die nächste wird diese jedoch benötigt.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Übergang in die Produktion](scenario-desktop-production.md)
