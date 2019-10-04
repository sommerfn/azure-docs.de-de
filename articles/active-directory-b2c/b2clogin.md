---
title: Festlegen von Umleitungs-URLs auf b2clogin.com – Azure Active Directory B2C
description: Erfahren Sie mehr über das Verwenden von b2clogin.com in Umleitungs-URLs für Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: dbc366daac89f44d4b084081590124f81ff9cc9c
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69533746"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Festlegen von Umleitungs-URLs zu b2clogin.com für Azure Active Directory B2C

Wenn Sie einen Identitätsanbieter für die Registrierung und Anmeldung in Ihrer Azure Active Directory B2C-Anwendung (Azure AD B2C) einrichten, müssen Sie eine Umleitungs-URL angeben. In Ihren Anwendung und APIs sollte nicht mehr auf *login.microsoftonline.com* verwiesen werden. Verwenden Sie stattdessen für alle neuen Anwendungen *b2clogin.com*, und migrieren Sie vorhandene Anwendungen von *login.microsoftonline.com* nach *b2clogin.com*.

## <a name="benefits-of-b2clogincom"></a>Vorteile von „b2clogin.com“

Bei Verwendung von *b2clogin.com* als Umleitungs-URL:

* Der im Cookieheader von Microsoft-Diensten verbrauchte Speicherplatz wird reduziert.
* Ihre Umleitungs-URLs müssen keinen Verweis auf Microsoft mehr enthalten.
* Clientseitiger JavaScript-Code wird in angepassten Seiten unterstützt (derzeit als [Vorschaufunktion](user-flow-javascript-overview.md)). Aufgrund von Sicherheitseinschränkungen werden JavaScript-Code und HTML-Formularelemente von benutzerdefinierten Seiten entfernt, wenn Sie *login.microsoftonline.com* verwenden.

## <a name="overview-of-required-changes"></a>Übersicht über erforderliche Änderungen

Sie müssen möglicherweise eine Reihe von Änderungen vornehmen, um Ihre Anwendungen zu *b2clogin.com* zu migrieren:

* Ändern Sie die Umleitungs-URL in den Anwendungen Ihres Identitätsanbieters so, dass auf *b2clogin.com* verwiesen wird.
* Aktualisieren Sie Ihre Azure AD B2C-Anwendungen so, dass *b2clogin.com* in den Verweisen auf Benutzerflows und Tokenendpunkte verwendet wird.
* Aktualisieren Sie alle **zulässigen Ursprünge**, die Sie in den CORS-Einstellungen zur [Anpassung der Benutzeroberfläche](active-directory-b2c-ui-customization-custom-dynamic.md) definiert haben.

## <a name="change-identity-provider-redirect-urls"></a>Ändern der Umleitungs-URL für Identitätsanbieter

Ändern Sie auf jeder Website des Identitätsanbieters, auf der Sie eine Anwendung erstellt haben, alle vertrauenswürdigen URLs so, dass eine Umleitung zu `your-tenant-name.b2clogin.com` und nicht zu *login.microsoftonline.com* erfolgt.

Es gibt zwei Formate, die für die Umleitungs-URLs zu „b2clogin.com“ verwendet werden können. Das erste bietet den Vorteil, dass an keiner Stelle der URL „Microsoft“ auftaucht, da die Mandanten-ID (eine GUID) anstelle des Domänennamens Ihres Mandanten verwendet wird:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

Bei der zweiten Option wird der Domänenname Ihres Mandanten im Format `your-tenant-name.onmicrosoft.com` verwendet. Beispiel:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Für beide Formate:

* Ersetzen Sie `{your-tenant-name}` durch den Namen des Azure AD B2C-Mandanten.
* Entfernen Sie `/te`, sofern in der URL vorhanden.

## <a name="update-your-applications-and-apis"></a>Aktualisieren Ihrer Anwendungen und APIs

Der Code in Ihren Azure AD B2C-fähigen Anwendungen und APIs kann an verschiedenen Stellen auf `login.microsoftonline.com` verweisen. Ihr Code kann z. B. Verweise auf Benutzerflows und Tokenendpunkte enthalten. Aktualisieren Sie Folgendes, sodass auf `your-tenant-name.b2clogin.com` verwiesen wird:

* Authorization endpoint (Autorisierungsendpunkt)
* Token endpoint (Tokenendpunkt)
* Tokenaussteller

Der Autorisierungsendpunkt für die Registrierungs-/Anmelderichtlinie von Contoso würde z. B. nun wie folgt lauten:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

## <a name="microsoft-authentication-library-msal"></a>Microsoft Authentication Library (MSAL)

### <a name="validateauthority-property"></a>ValidateAuthority-Eigenschaft

Wenn Sie [MSAL.NET][msal-dotnet] v2 oder früher verwenden, legen Sie die **ValidateAuthority**-Eigenschaft bei der Clientinstanziierung auf `false` fest, um Umleitungen zu *b2clogin.com* zuzulassen. Diese Einstellung ist für MSAL.NET v3 und höher nicht erforderlich.

```CSharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

Bei Verwendung von [MSAL für JavaScript][msal-js]:

```JavaScript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md