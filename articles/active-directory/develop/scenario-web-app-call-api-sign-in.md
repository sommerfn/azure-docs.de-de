---
title: Web-App, die Web-APIs aufruft (Anmelden) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Web-APIs aufruft (Anmelden)
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
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cb0190f76ddce79012a5bf97e2d813f40f9f018
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596375"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Web-App, die Web-APIs aufruft – Anmelden

Sie wissen bereits, wie Ihrer Web-App Anmeldedaten hinzugefügt werden. Dies lernen Sie in [Web app that signs-in users - add sign-in (Web-App, die Benutzer anmeldet – Hinzufügen von Anmeldedaten)](scenario-web-app-sign-user-sign-in.md).

Der Unterschied besteht hier darin, dass Sie nach der Abmeldung des Benutzers von dieser oder einer anderen Anwendung den Tokencache mit den dem Benutzer zugeordneten Tokens leeren möchten.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Abfangen des Rückrufs nach der Abmeldung – Einmaliges Abmelden

Ihre Anwendung kann das „after `logout`“-Ereignis abfangen, um beispielsweise den Eintrag aus dem Tokencache zu löschen, dem das abgemeldete Konto zugeordnet ist. Die Web-App speichert Zugriffstoken für den Benutzer in einem Cache. Durch das Abfangen des „after `logout`“-Rückrufs kann Ihre Web-App den Benutzer aus dem Tokencache entfernen.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Dieser Mechanismus wird in der `AddMsal()`-Methode von [WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157) veranschaulicht.

Die **Abmelde-URL**, die Sie für Ihre Anwendung registriert haben, ermöglicht Ihnen das Implementieren der einmaligen Abmeldung. Der Microsoft Identity Platform-Endpunkt `logout` ruft die für Ihre Anwendung registrierte **Abmelde-URL** auf. Dieser Aufruf erfolgt, wenn die Abmeldung von Ihrer Web-App, einer anderen Web-App oder dem Browser initiiert wurde. Weitere Informationen finden Sie unter [Einmaliges Abmelden](v2-protocols-oidc.md#single-sign-out).

```CSharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: removing the account from MSAL.NET cache
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

Der Code für „RemoveAccountAsync“ ist unter [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288) verfügbar.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Im ASP.NET-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="javatabjava"></a>[Java](#tab/java)

Im Java-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

# <a name="pythontabpython"></a>[Python](#tab/python)

Im Python-Beispiel werden Konten bei der globalen Abmeldung nicht aus dem Cache entfernt.

---

## <a name="next-steps"></a>Nächste Schritte

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Abrufen eines Tokens für die Web-App](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
