---
title: Abrufen der Zustimmung für mehrere Ressourcen (Microsoft Authentication Library für .NET) | Azure
description: Erfahren Sie, wie ein Benutzer mit der Microsoft Authentication Library für .NET (MSAL.NET) die Vorabzustimmung für mehrere Ressourcen abrufen kann.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ded7a6fc465b4cfc98d26f65195f89de8381ac6
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532390"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Benutzer erhält per MSAL.NET die Zustimmung für mehrere Ressourcen
Der Microsoft Identity Platform-Endpunkt lässt es nicht zu, dass Sie ein Token für mehrere Ressourcen gleichzeitig abrufen. Wenn Sie die Microsoft Authentication Library für .NET (MSAL.NET) verwenden, darf der Bereichsparameter in der Tokenabrufmethode nur Bereiche für eine einzelne Ressource enthalten. Sie können allerdings vorab mehreren Ressourcen zustimmen, indem Sie mit der Generatormethode `.WithExtraScopeToConsent` zusätzliche Bereiche angeben.

> [!NOTE]
> Das Abrufen der Zustimmung für mehrere Ressourcen funktioniert für Microsoft Identity Platform, nicht aber für Azure AD B2C. Azure AD B2C unterstützt nur die Administrator- und nicht die Benutzereinwilligung.

Beispiel: Sie verfügen über zwei Ressourcen mit jeweils zwei Bereichen:

- https:\//mytenant.onmicrosoft.com/customerapi (mit den beiden Bereichen `customer.read` und `customer.write`)
- https:\//mytenant.onmicrosoft.com/vendorapi (mit den beiden Bereichen `vendor.read` und `vendor.write`)

Verwenden Sie dann den `.WithExtraScopeToConsent`-Modifizierer, der über den im folgenden Beispiel gezeigten *extraScopesToConsent*-Parameter verfügt:

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Auf diese Weise erhalten Sie ein Zugriffstoken für die erste Web-API. Wenn Sie dann auf die zweite Web-API zugreifen müssen, können Sie das Token automatisch aus dem Tokencache abrufen:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
