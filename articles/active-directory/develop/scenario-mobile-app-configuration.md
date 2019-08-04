---
title: Mobile App zum Aufrufen von Web-APIs (Codekonfiguration) – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Codekonfiguration der App).
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
ms.date: 07/23/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bdf9210eb88b2057cf861b208f19d3e6f562e9a
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414795"
---
# <a name="mobile-app-that-calls-web-apis---code-configuration"></a>Mobile App zum Aufrufen von Web-APIs – Codekonfiguration

Nachdem Sie Ihre Anwendung erstellt haben, erfahren Sie, wie Sie den Code anhand der Parameter der Anwendung konfigurieren, die Sie bei der App-Registrierung erhalten haben. Mobile Anwendungen weisen auch einige komplexe Besonderheiten auf, die sich auf die Anpassung an das Framework beziehen, das zum Erstellen dieser Apps verwendet wird.

## <a name="msal-libraries-supporting-mobile-apps"></a>MSAL-Bibliotheken, die mobile Apps unterstützen

Die folgenden Microsoft-Bibliotheken unterstützen mobile Apps:

  MSAL-Bibliothek | BESCHREIBUNG
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Zum Entwickeln portabler Anwendungen. Die von MSAL.NET unterstützten Plattformen zum Erstellen einer mobilen Anwendung sind UWP, Xamarin.iOS und Xamarin.Android.
  ![MSAL.iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL.iOS | Zum Entwickeln nativer iOS-Anwendungen mit Objective-C oder Swift
  ![MSAL.Android](media/sample-v2-code/logo_android.png) <br/> MSAL.Android | Zum Entwickeln nativer Android-Anwendungen in Java für Android

## <a name="configuring-the-application"></a>Konfigurieren der Anwendung

Mobile Anwendungen verwenden die MSAL-Klasse `PublicClientApplication`. Nachfolgend wird veranschaulicht, wie sie instanziiert wird:

### <a name="android"></a>Android

```Java
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);
```

### <a name="ios"></a>iOS

```swift
// Initialize the app.
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
}
```

### <a name="xamarin-or-uwp"></a>Xamarin oder UWP

Im folgenden Absatz wird erläutert, wie Sie den Code der Anwendung für Xamarin.iOS-, Xamarin.Android- und UWP-Apps konfigurieren. Der erste Schritt besteht darin, die Anwendung zu instanziieren. Ein optionaler Schritt ist das Konfigurieren des Brokers.

#### <a name="instantiating-the-application"></a>Instanziieren der Anwendung

In Xamarin oder auf der UWP sieht die einfachste Methode zum Instanziieren der Anwendung wie folgt aus, wobei die `ClientId` der global eindeutige Bezeichner (Globally Unique Identifier, GUID) Ihrer Anwendung gemäß der Registrierung ist.

```CSharp
var app = PublicClientApplicationBuilder.Create(clientId)
                                        .Build();
```

Es gibt zusätzliche *WithParameter*-Methoden, die das übergeordnete Benutzeroberflächenelement festlegen, die Standardautorität außer Kraft setzen, einen Clientnamen und eine Clientversion angeben (für Telemetrie), einen Umleitungs-URI angeben oder die zu verwendende HTTP-Factory angeben (z. B. zum Verwalten von Proxys und zum Angeben von Telemetriedaten und Protokollierung). Das ist das Thema der folgenden Absätze.

##### <a name="specifying-the-parent-uiwindowactivity"></a>Angeben des übergeordneten Benutzeroberflächenelements/Fensters oder der übergeordneten Aktivität

Unter Android müssen Sie die übergeordnete Aktivität übergeben, bevor Sie die interaktive Authentifizierung durchführen. Unter iOS müssen Sie bei Verwendung eines Brokers den ViewController übergeben. Auf dieselbe Weise möchten Sie möglicherweise auf der UWP das übergeordnete Fenster übergeben. Dies ist möglich, wenn Sie das Token abrufen, aber es ist auch möglich, bei der Erstellung der App einen Rückruf anzugeben, bei dem ein Delegierter das übergeordnete Benutzeroberflächenelement zurückgibt.

```CSharp
IPublicClientApplication application = PublicClientApplicationBuilder.Create(clientId)
  .ParentActivityOrWindowFunc(() => parentUi)
  .Build();
```

Unter Android empfehlen wir Ihnen das [hier beschriebene](https://github.com/jamesmontemagno/CurrentActivityPlugin) `CurrentActivityPlugin`.  Der `PublicClientApplication`-Generator-Code sieht dann wie folgt aus:

```CSharp
// Requires MSAL.NET 4.2 or above
var pca = PublicClientApplicationBuilder
  .Create("<your-client-id-here>")
  .WithParentActivityOrWindow(() => CrossCurrentActivity.Current)
  .Build();
```

##### <a name="more-app-building-parameters"></a>Weitere Parameter für die App-Erstellung

- Eine Liste aller bei `PublicClientApplicationBuilder` verfügbaren Modifizierer finden Sie in der Referenzdokumentation [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- Die Beschreibung aller bei `PublicClientApplicationOptions` verfügbar gemachten Optionen finden Sie in der Referenzdokumentation [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions).

#### <a name="xamarin-ios-specific-considerations"></a>Xamarin iOS-spezifische Aspekte

Bei Xamarin iOS müssen Sie bei Verwendung von MSAL.NET mehrere Aspekte berücksichtigen:

1. [Überschreiben und Implementieren der `OpenUrl`-Funktion im `AppDelegate`](msal-net-xamarin-ios-considerations.md#implement-openurl)
1. [Aktivieren von Keychaingruppen](msal-net-xamarin-ios-considerations.md#enable-keychain-access)
1. [Aktivieren von Tokencachefreigaben](msal-net-xamarin-ios-considerations.md#enable-token-cache-sharing-across-ios-applications)
1. [Aktivieren des Keychainzugriffs](msal-net-xamarin-ios-considerations.md#enable-keychain-access)

Die Details finden Sie unter [Spezielle Überlegungen für Xamarin iOS mit MSAL.NET](msal-net-xamarin-ios-considerations.md).

#### <a name="other-xamarin-android-specific-considerations"></a>Weitere Xamarin Android-spezifische Aspekte

Für Xamarin Android gelten die folgenden Besonderheiten:

- [Sicherstellen der Rückkehr des Steuerelements zur MSAL-Bibliothek nach Ende des interaktiven Teils des Authentifizierungsablaufs](msal-net-xamarin-android-considerations.md#ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends)
- [Aktualisieren des Android-Manifests](msal-net-xamarin-android-considerations.md#update-the-android-manifest)
- [Verwenden der eingebetteten Webansicht (optional)](msal-net-xamarin-android-considerations.md#use-the-embedded-web-view-optional)
- [Problembehandlung](msal-net-xamarin-android-considerations.md#troubleshooting)

Die Details finden Sie unter [Besondere Überlegungen zu Xamarin.Android mit MSAL.NET](msal-net-xamarin-android-considerations.md).

Schließlich gibt es noch einige Besonderheiten, die Sie über die Browser unter Android kennen sollten. Diese werden unter [Besondere Überlegungen zu Xamarin.Android mit MSAL.NET](msal-net-system-browser-android-considerations.md) erläutert.

#### <a name="uwp-specific-considerations"></a>UWP-spezifische Aspekte

Auf der UWP können Sie Unternehmensnetzwerke verwenden. Weitere Informationen zu den Besonderheiten der UWP finden Sie unter [Spezifische Überlegungen zur Universellen Windows-Plattform mit MSAL.NET](msal-net-uwp-considerations.md).

## <a name="configuring-the-application-to-use-the-broker"></a>Konfigurieren der Anwendung für die Verwendung des Brokers

### <a name="why-use-brokers-on-xamarinios-and-xamarinandroid-applications"></a>Warum sollten Broker für Xamarin.iOS- und Xamarin.Android-Anwendungen verwendet werden?

Unter Android und iOS ermöglichen Broker Folgendes:

- Einmaliges Anmelden (Single Sign-On, SSO). Ihre Benutzer müssen sich nicht bei jeder Anwendung anmelden.
- Geräteidentifikation. Ermöglicht Azure AD-gerätebezogene Richtlinien für den bedingten Zugriff durch den Zugriff auf das Gerätezertifikat, das auf dem Gerät erstellt wurde, als es dem Arbeitsplatz beigetreten ist.
- Überprüfung der Anwendungsidentifikation. Wenn eine Anwendung den Broker aufruft, übergibt sie die Umleitungs-URL, und der Broker überprüft sie.

### <a name="enable-the-brokers-on-xamarin"></a>Aktivieren der Broker in Xamarin

Verwenden Sie zum Aktivieren einer dieser Funktionen den `WithBroker()`-Parameter, wenn Sie die `PublicClientApplicationBuilder.CreateApplication`-Methode aufrufen. `.WithBroker()` ist standardmäßig auf „true“ festgelegt. Führen Sie unter [iOS](#brokered-authentication-for-xamarinios) die folgenden Schritte aus.

### <a name="brokered-authentication-for-xamarinios"></a>Brokerauthentifizierung für Xamarin.iOS

Führen Sie die folgenden Schritte aus, um Ihre Xamarin.iOS-App für die Kommunikation mit der [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458)-App zu aktivieren.

#### <a name="step-1-enable-broker-support"></a>Schritt 1: Aktivieren der Brokerunterstützung

Die Brokerunterstützung wird pro `PublicClientApplication` aktiviert. Standardmäßig ist es deaktiviert. Sie müssen den (standardmäßig auf „true“ festgelegten) `WithBroker()`-Parameter verwenden, wenn Sie die `PublicClientApplication` über den `PublicClientApplicationBuilder` erstellen.

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

#### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Schritt 2: Aktualisieren von AppDelegate zum Verarbeiten des Rückrufs

Wenn MSAL.NET den Broker aufruft, führt der Broker wiederum einen Rückruf Ihrer Anwendung über die `AppDelegate.OpenUrl`-Methode aus. Da MSAL auf die Antwort vom Broker wartet, muss Ihre Anwendung kooperieren, damit der Rückruf von MSAL.NET erfolgen kann. Dazu aktualisieren Sie die Datei `AppDelegate.cs`, um die folgende Methode außer Kraft zu setzen.

```CSharp
public override bool OpenUrl(UIApplication app, NSUrl url,
                             string sourceApplication,
                             NSObject annotation)
{
 if (AuthenticationContinuationHelper.IsBrokerResponse(sourceApplication))
 {
  AuthenticationContinuationHelper.SetBrokerContinuationEventArgs(url);
  return true;
 }
 else if (!AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url))
 {
  return false;
 }
 return true;
}
```

Diese Methode wird jedes Mal aufgerufen, wenn die Anwendung gestartet wird. Sie wird verwendet, um die Antwort vom Broker zu verarbeiten und den von MSAL.NET initiierten Authentifizierungsprozess abzuschließen.

#### <a name="step-3-set-a-uiviewcontroller"></a>Schritt 3: Festlegen von „UIViewController()“

Bei Xamarin iOS müssen Sie normalerweise kein Objektfenster festlegen, aber in diesem Fall machen Sie das, um Antworten von einem Broker zu senden und zu empfangen. Legen Sie in der Datei `AppDelegate.cs` einen ViewController fest.

Führen Sie die folgenden Schritte aus, um das Objektfenster festzulegen:

1) Legen Sie in der Datei `AppDelegate.cs` das `App.RootViewController`-Objekt auf einen neuen `UIViewController()` fest. Dadurch wird sichergestellt, dass ein `UIViewController` bei dem Aufruf des Brokers vorhanden ist. Wenn der UIViewController nicht ordnungsgemäß festgelegt wurde, erhalten Sie möglicherweise den folgenden Fehler: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) Verwenden Sie im AcquireTokenInteractive-Aufruf den `.WithParentActivityOrWindow(App.RootViewController)`-Parameter, und übergeben Sie den Verweis an das Objektfenster, das Sie verwenden.

**Beispiel:**

In `App.cs`:
```CSharp
   public static object RootViewController { get; set; }
```
In `AppDelegate.cs`:
```CSharp
   LoadApplication(new App());
   App.RootViewController = new UIViewController();
```
Im AcquireToken-Aufruf:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```

#### <a name="step-4-register-a-url-scheme"></a>Schritt 4: Registrieren eines URL-Schemas

MSAL.NET verwendet URLs, um den Broker aufzurufen und die Brokerantwort dann an Ihre App zurückzugeben. Um den Roundtrip abzuschließen, müssen Sie in der Datei `Info.plist` ein URL-Schema für Ihre App registrieren.

Stellen Sie `CFBundleURLSchemes` das Präfix `msauth` voran. Fügen Sie dann am Ende `CFBundleURLName` hinzu.

`$"msauth.(BundleId)"`

**Beispiel:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Dieses URL-Schema wird Teil des Umleitungs-URIs, der beim Empfangen der Brokerantwort zum eindeutigen Identifizieren Ihrer App verwendet wird.

```XML
 <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.yourcompany.xforms</string>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.com.yourcompany.xforms</string>
        </array>
      </dict>
    </array>
```

#### <a name="step-5-lsapplicationqueriesschemes"></a>Schritt 5: LSApplicationQueriesSchemes

MSAL verwendet `–canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Unter iOS 9 wurden die Schemas, die von einer Anwendung abgefragt werden können, von Apple fest vorgegeben.

Sie müssen in der Datei `Info.plist` dem Abschnitt `LSApplicationQueriesSchemes` die Zeichenfolge **`msauthv2`** **hinzufügen**.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="brokered-authentication-for-xamarinandroid"></a>Brokerauthentifizierung für Xamarin.Android

MSAL.NET unterstützt noch keine Broker für Android.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Abrufen eines Tokens](scenario-mobile-acquire-token.md)
