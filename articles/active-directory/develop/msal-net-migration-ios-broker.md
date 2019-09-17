---
title: Migrieren von Xamarin iOS-Anwendungen mit Microsoft Authenticator von ADAL.NET zu MSAL.NET | Azure
description: Erfahren Sie, wie Sie Xamarin iOS-Anwendungen mit Microsoft Authenticator von der Azure AD-Authentifizierungsbibliothek für .NET (Azure AD Authentication Library for .NET, ADAL.NET) zur Microsoft Authentication Library für .NET (MSAL.NET) migrieren.
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
ms.date: 09/08/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 609aeeada78985466cb7c254cab32864e5d84b0e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70875603"
---
# <a name="migrating-ios-applications-using-microsoft-authenticator-from-adalnet-to-msalnet"></a>Migrieren von iOS-Anwendungen mit Microsoft Authenticator von ADAL.NET zu MSAL.NET

Sie verwenden ADAL.NET und den iOS-Broker, und es ist an der Zeit, zur [Microsoft-Authentifizierungsbibliothek](msal-overview.md) MSAL.NET zu migrieren, die den Broker unter iOS ab Version 4.3 unterstützt. 

Einstieg In diesem Artikel wird beschrieben, wie Sie Ihre Xamarin iOS-App von ADAL zu MSAL migrieren.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Dokument wird davon ausgegangen, dass Sie bereits über eine Xamarin iOS-App verfügen, die in den iOS-Broker integriert ist. Wenn dies nicht der Fall ist, empfiehlt es sich, direkt zu MSAL.NET zu wechseln und dort mit der Brokerimplementierung zu beginnen. Ausführliche Informationen zum Aufrufen des iOS-Brokers in MSAL.NET mit einer neuen Anwendung finden Sie in [dieser Dokumentation](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS#why-use-brokers-on-xamarinios-and-xamarinandroid-applications).

## <a name="background"></a>Hintergrund

### <a name="what-are-brokers"></a>Was sind Broker?

Broker sind von Microsoft bereitgestellte Anwendungen unter Android und iOS ([Microsoft Authenticator](https://www.microsoft.com/account/authenticator) unter iOS und Android, Intune-Unternehmensportal unter Android). 

Sie ermöglichen Folgendes:

- Einmaliges Anmelden,
- Geräteidentifikation, die einige [Richtlinien für bedingten Zugriff](../conditional-access/overview.md) erfordern (siehe [Geräteverwaltung](../conditional-access/conditions.md#device-platforms))
- Überprüfung der Anwendungsidentifikation, auch in einigen Unternehmensszenarien erforderlich (siehe z. B. [Mobile Anwendungsverwaltung (Mobile Application Management, MAM) in Intune](https://docs.microsoft.com/intune/mam-faq))

## <a name="migrate-from-adal-to-msal"></a>Migrieren von ADAL zu MSAL

### <a name="step-1-enable-the-broker"></a>Schritt 1: Aktivieren des Brokers

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
In ADAL.NET wird die Brokerunterstützung pro Authentifizierung aktiviert. Sie ist standardmäßig deaktiviert. Sie mussten im `PlatformParameters`-Konstruktor ein `useBroker`-Flag auf „true“ festlegen, um den Broker aufzurufen:

```CSharp
public PlatformParameters(
        UIViewController callerViewController, 
        bool useBroker)
```
Legen Sie auch in diesem Beispiel im plattformspezifischen Code im Seitenrenderer für iOS das `useBroker`-Flag 
auf „true“ fest:
```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```

Fügen Sie dann die Parameter in den Aufruf zum Abrufen eines Tokens (AcquireToken-Aufruf) ein:
```CSharp
 AuthenticationResult result =
                    await
                        AuthContext.AcquireTokenAsync(
                              Resource, 
                              ClientId, 
                              new Uri(RedirectURI), 
                              platformParameters)
                              .ConfigureAwait(false);
```

</td><td>
In MSAL.NET wird die Brokerunterstützung pro PublicClientApplication aktiviert. Diese Einstellung ist standardmäßig deaktiviert. Verwenden Sie zum Aktivieren der Brokerunterstützung den 

(standardmäßig auf „true“ festgelegten) `WithBroker()`-Parameter, um den Broker aufzurufen:

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos)
                .Build();
```
Im AcquireToken-Aufruf:
```CSharp
result = await app.AcquireTokenInteractive(scopes)
             .WithParentActivityOrWindow(App.RootViewController)
             .ExecuteAsync();
```
</table>

### <a name="step-2-set-a-uiviewcontroller"></a>Schritt 2: Festlegen von „UIViewController()“
In ADAL.NET haben Sie den UIViewController als Teil der PlatformParameters übergeben (siehe Beispiel in Schritt 1). Um dem Entwickler mehr Flexibilität zu bieten, wird jedoch in MSAL.NET ein Objektfenster verwendet, das aber bei normaler iOS-Verwendung nicht erforderlich ist. Damit Sie den Broker verwenden können, müssen Sie jedoch das Objektfenster festlegen, um Antworten vom Broker zu senden und zu empfangen. 
<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Der UIViewController wird an die PlatformParameters der iOS-spezifischen Plattform übergeben.

```CSharp
page.BrokerParameters = new PlatformParameters(
          this, 
          true, 
          PromptBehavior.SelectAccount);
```
</td><td>
In MSAL.NET müssen Sie zwei Schritte ausführen, um das Objektfenster für iOS festzulegen:

1) Legen Sie in der Datei `AppDelegate.cs` das `App.RootViewController`-Objekt auf einen neuen `UIViewController()` fest. Durch diese Zuweisung wird sichergestellt, dass ein UIViewController mit dem Aufruf des Brokers vorhanden ist. Wenn der UIViewController nicht ordnungsgemäß festgelegt wurde, erhalten Sie möglicherweise den folgenden Fehler: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
2) Verwenden Sie im AcquireTokenInteractive-Aufruf den `.WithParentActivityOrWindow(App.RootViewController)`-Parameter,
und übergeben Sie den Verweis an das Objektfenster, das Sie verwenden.

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

</table>

### <a name="step-3-update-appdelegate-to-handle-the-callback"></a>Schritt 3: Aktualisieren von AppDelegate zum Verarbeiten des Rückrufs
ADAL und MSAL rufen den Broker auf, und der Broker wiederum führt einen Rückruf Ihrer Anwendung über die `OpenUrl`-Methode der `AppDelegate`-Klasse aus. Weitere Informationen finden Sie [hier](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS/_edit#step-two-update-appdelegate-to-handle-the-callback).

:heavy_check_mark:**Keine Änderungen hier zwischen ADAL.NET und MSAL.NET**

### <a name="step-4-register-a-url-scheme"></a>Schritt 4: Registrieren eines URL-Schemas
ADAL.NET und MSAL.NET verwenden URLs, um den Broker aufzurufen und die Brokerantwort an die App zurückzugeben. Registrieren Sie in der Datei `Info.plist` das URL-Schema für Ihre App wie folgt:

<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Das URL-Schema ist für Ihre App eindeutig.
</td><td>
Das 

`CFBundleURLSchemes`-Name muss das Präfix 

`msauth.`

gefolgt von Ihrem `CFBundleURLName` enthalten

Beispiel: `$"msauth.(BundleId")`

```CSharp
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

> [!NOTE]
>  Dieses URL-Schema wird Teil des Umleitungs-URIs, der beim Empfangen der Brokerantwort zum eindeutigen Identifizieren der App verwendet wird.

</table>

### <a name="step-5-lsapplicationqueriesschemes"></a>Schritt 5: LSApplicationQueriesSchemes

ADAL.NET und MSAL.NET verwenden `-canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Fügen Sie in der Datei „Info.plist“ dem Abschnitt „LSApplicationQueriesSchemes“ den richtigen Bezeichner für den iOS-Broker wie folgt hinzu: 
<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>
Verwendung 

`msauth`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>
```
</td><td>
Verwendung 

`msauthv2`


```CSharp
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
</array>
```
</table>

### <a name="step-6-register-you-redirecturi-in-the-portal"></a>Schritt 6: Registrieren Ihres Umleitungs-URIs im Portal

Bei ADAL.NET und MSAL.NET bringt die Verwendung des Brokers als Ziel eine zusätzliche Anforderung an den Umleitungs-URI mit sich. Registrieren Sie den Umleitungs-URI bei Ihrer Anwendung im Portal.
<table>
<tr><td>Aktueller ADAL-Code:</td><td>MSAL-Entsprechung:</td></tr>
<tr><td>

Beispiel für `"<app-scheme>://<your.bundle.id>"`: `mytestiosapp://com.mycompany.myapp`
</td><td>

`$"msauth.{BundleId}://auth"`

Beispiel:

`public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; `

</table>

Weitere Informationen zum Registrieren des Umleitungs-URIs im Portal finden Sie unter [Verwenden von Brokern mit Xamarin.iOS-Anwendungen](msal-net-use-brokers-with-xamarin-apps.md#step-7-make-sure-the-redirect-uri-is-registered-with-your-app).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Spezielle Überlegungen für Xamarin iOS mit MSAL.NET](msal-net-xamarin-ios-considerations.md). 
