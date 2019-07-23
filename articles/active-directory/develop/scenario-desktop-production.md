---
title: Desktop-App, die Web-APIs aufruft (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (Übergang in die Produktion).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2343a416bd810792e7267b94395f953aa4f880a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111201"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Desktop-App, die Web-APIs aufruft (Übergang in die Produktion)

In diesem Artikel wird beschrieben, wie Sie Ihre Anwendung weiter verbessern und der Übergang in die Produktion gelingt.

## <a name="handling-errors-in-desktop-applications"></a>Behandeln von Fehlern in Desktopanwendungen

Sie haben anhand der unterschiedlichen Flüsse und Codeausschnitte gelernt, wie Sie Fehler für automatisch ausgeführte Flüsse behandeln. Außerdem haben Sie erfahren, dass in bestimmten Fällen eine Interaktion erforderlich ist (nämlich bei einer inkrementellen Einwilligung und bei bedingtem Zugriff).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Einholen der Benutzereinwilligung im Vorfeld für verschiedene Ressourcen

> [!NOTE]
> Wenn Sie für mehrere Ressourcen eine Einwilligung einholen müssen, ist dies mit Microsoft Identity Platform, nicht aber mit Azure Active Directory B2C (Azure AD) möglich. Azure AD B2C unterstützt nur die Administrator- und nicht die Benutzereinwilligung.

Sie können über den Endpunkt von Microsoft Identity Platform (v2.0) kein Token für mehrere Ressourcen gleichzeitig abrufen. Aus diesem Grund kann der `scopes`-Parameter nur Bereiche für eine einzelne Ressource enthalten. Mit dem Parameter `extraScopesToConsent` können Sie sicherstellen, dass der Benutzer vorab seine Einwilligung zu mehreren Ressourcen erteilt.

Angenommen, Sie verfügen über zwei Ressourcen mit jeweils zwei Bereichen:

- `https://mytenant.onmicrosoft.com/customerapi` mit den beiden Bereichen `customer.read` und `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` mit den beiden Bereichen `vendor.read` und `vendor.write`

In diesem Fall sollten Sie den `.WithAdditionalPromptToConsent`-Modifizierer verwenden, der über den Parameter `extraScopesToConsent` verfügt.

Beispiel:

```CSharp
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

Durch diesen Aufruf wird ein Zugriffstoken für die erste Web-API aufgerufen.

Die zweite Web-API können Sie bei Bedarf folgendermaßen aufrufen:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Bei jedem Ausführen der App ist für ein persönliches Microsoft-Konto eine erneute Einwilligung erforderlich

Für Benutzer eines persönlichen Microsoft-Kontos besteht das beabsichtigte Verhalten darin, bei jedem Aufruf des nativen Clients (Desktop/mobile App) zur Autorisierung erneut eine Einwilligung einzuholen. Im Gegensatz zu einer vertraulichen Clientanwendung, die zum Nachweis ihrer Identität ein Geheimnis mit Microsoft Identity Platform austauscht, ist die Identität des nativen Clients grundsätzlich unsicher. Microsoft Identity Plattform schränkt dieses Sicherheitsrisiko für Verbraucherdienste ein, indem der Benutzer bei jeder Anwendungsautorisierung seine Einwilligung geben muss.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
