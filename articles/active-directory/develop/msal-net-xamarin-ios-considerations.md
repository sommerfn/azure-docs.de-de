---
title: Überlegungen zu Xamarin iOS (Microsoft-Authentifizierungsbibliothek für .NET)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über spezielle Überlegungen zur Verwendung von Xamarin iOS mit der Microsoft-Authentifizierungsbibliothek für .NET (MSAL.NET).
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
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cc4eb0084e0606251d3fbd80d08723701c6a260
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175616"
---
# <a name="xamarin-ios-specific-considerations-with-msalnet"></a>Spezielle Überlegungen für Xamarin iOS mit MSAL.NET
Unter Xamarin iOS gibt es verschiedene Überlegungen, die Sie bei Verwendung von MSAL.NET berücksichtigen müssen.

- [Bekannte Probleme mit iOS 12 und der Authentifizierung](#known-issues-with-ios-12-and-authentication)
- [Überschreiben und Implementieren der `OpenUrl`-Funktion im `AppDelegate`](#implement-openurl)
- [Aktivieren von Keychaingruppen](#enable-keychain-access)
- [Aktivieren von Tokencachefreigaben](#enable-token-cache-sharing-across-ios-applications)
- [Aktivieren des Keychainzugriffs](#enable-keychain-access)

## <a name="known-issues-with-ios-12-and-authentication"></a>Bekannte Probleme mit iOS 12 und der Authentifizierung
Microsoft hat eine [Sicherheitsempfehlung](https://github.com/aspnet/AspNetCore/issues/4647) mit Informationen zur Inkompatibilität zwischen iOS 12 und einigen Arten der Authentifizierung herausgegeben. Diese Inkompatibilität führt zu Problemen mit der Anmeldung über soziale Medien sowie mit WSFed- und OIDC-Anmeldungen. Die Empfehlung enthält auch Informationen dazu, was Entwickler tun können, um derzeitige Sicherheitseinschränkungen zu entfernen, die von ASP.NET hinzugefügt werden, sodass ihre Anwendungen mit iOS 12 kompatibel werden.  

Beim Entwickeln von MSAL.NET-Anwendungen für Xamarin iOS stellen Sie möglicherweise eine Endlosschleife fest, wenn Sie versuchen, sich über iOS 12 bei Websites anzumelden (ähnlich diesem [ADAL-Problem](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/issues/1329)). 

Sie stellen möglicherweise auch ein Problem mit der ASP.NET Core-OICD-Authentifizierung bei iOS 12 Safari fest, wie in diesem [WebKit-Problem](https://bugs.webkit.org/show_bug.cgi?id=188165) beschrieben.

## <a name="implement-openurl"></a>Implementieren von OpenUrl

Zuerst müssen Sie die `OpenUrl` -Methode der `FormsApplicationDelegate`-Klasse überschreiben und `AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs` aufrufen.

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
    return true;
}
```

Sie müssen auch ein URL-Schema definieren, Berechtigungen anfordern, mit denen Ihre App eine andere App aufrufen kann, die Umleitungs-URL in einer bestimmten Form festlegen und diese Umleitungs-URL im [Azure-Portal](https://portal.azure.com) registrieren.

### <a name="enable-keychain-access"></a>Aktivieren des Keychainzugriffs

Um den Keychainzugriff zu aktivieren, muss die Anwendung über eine Keychain-Zugriffsgruppe verfügen.
Sie können die Keychain-Zugriffsgruppe mithilfe der `WithIosKeychainSecurityGroup()`-API festlegen, wenn Sie die Anwendung erstellen, wie unten dargestellt:

Um das einmalige Anmelden zu aktivieren, müssen Sie die `PublicClientApplication.iOSKeychainSecurityGroup`-Eigenschaft in allen Anwendungen auf den gleichen Wert festlegen.

Hier ein Beispiel mit MSAL v3.x:
```csharp
var builder = PublicClientApplicationBuilder
     .Create(ClientId)
     .WithIosKeychainSecurityGroup("com.microsoft.msalrocks")
     .Build();
```

„entitlements.plist“ sollte aktualisiert werden, damit die Datei wie das folgende XML-Fragment aussieht:

Diese Änderung erfolgt *zusätzlich* zum Aktivieren des Keychainzugriffs in der Datei `Entitlements.plist`. Verwenden Sie dabei entweder die unten stehende Zugriffsgruppe oder Ihre eigene:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>keychain-access-groups</key>
  <array>
    <string>$(AppIdentifierPrefix)com.microsoft.msalrocks</string>
  </array>
</dict>
</plist>
```

Hier ein Beispiel mit MSAL v4.x:

```csharp
PublicClientApplication.iOSKeychainSecurityGroup = "com.microsoft.msalrocks";
```

Wenn Sie die `WithIosKeychainSecurityGroup()`-API verwenden, wird die Sicherheitsgruppe automatisch am Ende der Team-ID (AppIdentifierPrefix) der Anwendung hinzugefügt, da dies auch erfolgt, wenn Sie die Anwendung mit XCode erstellen. Weitere Informationen finden Sie in der Dokumentation der [iOS-Berechtigungen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps). Aus diesem Grund müssen Sie die Berechtigungen aktualisieren, um in „entitlements.plist“ vor der Keychain-Zugriffsgruppe „$(AppIdentifierPrefix) einzufügen.

### <a name="enable-token-cache-sharing-across-ios-applications"></a>Aktivieren der Tokencachefreigabe über iOS-Anwendungen hinweg

Ab MSAL 2.x können Sie eine Keychain-Zugriffsgruppe angeben, die zum Beibehalten des Tokencaches über mehrere Anwendungen hinweg verwendet werden soll. Mit dieser Einstellung können Sie den Tokencache für verschiedene Anwendungen freigeben, die über die gleiche Keychain-Zugriffsgruppe verfügen. Dazu gehören mit [ADAL.NET](https://aka.ms/adal-net) entwickelte Anwendungen, MSAL.NET Xamarin.iOS-Anwendungen und native iOS-Anwendungen, die mit [ADAL.objc](https://github.com/AzureAD/azure-activedirectory-library-for-objc) oder [MSAL.objc](https://github.com/AzureAD/microsoft-authentication-library-for-objc) entwickelt wurden.

Die Freigabe des Tokencaches ermöglicht das einmalige Anmelden bei allen Anwendungen, die die gleiche Keychain-Zugriffsgruppe verwenden.

Zum Aktivieren der Cachefreigabe müssen Sie mit der Methode „WithIosKeychainSecurityGroup()“ die Keychain-Zugriffsgruppe in allen Anwendungen, die denselben Cache nutzen, auf denselben Wert festlegen, wie im obigen Beispiel gezeigt.

Weiter oben wurde erwähnt, dass von MSAL bei jeder Verwendung der `WithIosKeychainSecurityGroup()`-API „$(AppIdentifierPrefix)“ hinzugefügt wurde. Dies geschieht, weil mit dem „AppIdentifierPrefix“ bzw. der Team-ID sichergestellt wird, dass nur von demselben Herausgeber erstellte Anwendungen den Keychainzugriff gemeinsam nutzen können.

> [!NOTE]
> **Die `KeychainSecurityGroup`-Eigenschaft ist veraltet.**
> 
> Früher waren Entwickler ab MSAL 2.x gezwungen, beim Verwenden der `KeychainSecurityGroup`-Eigenschaft das TeamId-Präfix einzuschließen.
>
>  Ab MSAL 2.7.x wird bei Verwendung der neuen `iOSKeychainSecurityGroup`-Eigenschaft das TeamID-Präfix zur Laufzeit aufgelöst. Wenn diese Eigenschaft verwendet wird, darf der Wert das TeamId-Präfix nicht enthalten.
>  Verwenden Sie die neue `iOSKeychainSecurityGroup`-Eigenschaft, die von Ihnen keine Angabe der TeamId erfordert. Die frühere `KeychainSecurityGroup`-Eigenschaft ist jetzt veraltet.

### <a name="use-microsoft-authenticator"></a>Verwenden von Microsoft Authenticator

Die Anwendung kann Microsoft Authenticator (einen Broker) verwenden, um Folgendes zu aktivieren:

- Einmaliges Anmelden (Single Sign-On, SSO). Ihre Benutzer müssen sich nicht bei jeder Anwendung anmelden.
- Geräteidentifikation. Durch den Zugriff auf das Gerätezertifikat, das auf dem Gerät erstellt wurde, als es in den Arbeitsplatz eingebunden wurde. Die Anwendung ist bereit, wenn die Mandantenadministratoren den bedingten Zugriff auf die Geräte aktivieren.
- Überprüfung der Anwendungsidentifikation. Wenn eine Anwendung den Broker aufruft, übergibt sie die Umleitungs-URL, und der Broker überprüft sie.

Weitere Informationen zum Aktivieren des Brokers finden Sie unter [Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin iOS- und Android-Anwendungen](msal-net-use-brokers-with-xamarin-apps.md).

### <a name="sample-illustrating-xamarin-ios-specific-properties"></a>Beispiel zur Veranschaulichung der speziellen Xamarin iOS-Eigenschaften

Detailliertere Informationen finden Sie im Abschnitt [iOS Specific Considerations](https://github.com/azure-samples/active-directory-xamarin-native-v2#ios-specific-considerations) der readme.md-Datei des folgenden Beispiels:

Beispiel | Plattform | BESCHREIBUNG
------ | -------- | -----------
[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Eine einfache Xamarin Forms-App, die die Verwendung der MSAL zum Authentifizieren von MSA und Azure AD über den Azure AD V2.0-Endpunkt und den Zugriff auf Microsoft Graph mit dem resultierenden Token veranschaulicht.

<!--- https://github.com/Azure-Samples/active-directory-xamarin-native-v2/blob/master/ReadmeFiles/Topology.png -->
