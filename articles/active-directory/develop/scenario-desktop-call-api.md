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
ms.openlocfilehash: 4abaf234d3b216e0f67501e5d2f2f5c3f874c5d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111244"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Desktop-App, die Web-APIs aufruft – Aufrufen einer Web-API

Da Sie nun über ein Token verfügen, können Sie eine geschützte Web-API aufrufen.

## <a name="calling-a-web-api-from-net"></a>Aufrufen einer Web-API über .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

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
