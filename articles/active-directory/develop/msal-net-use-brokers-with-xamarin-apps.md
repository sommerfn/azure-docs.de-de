---
title: Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin iOS- und Android-Anwendungen | Azure
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
ms.openlocfilehash: e16ad801b3b691d942c2ba0dc723ba72e24cf4c0
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70875599"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin-Anwendungen

Unter Android und iOS ermöglichen Broker wie Microsoft Authenticator oder das Microsoft Intune-Unternehmensportal (nur Android) Folgendes:

- Einmaliges Anmelden (Single Sign-On, SSO). Ihre Benutzer müssen sich nicht bei jeder Anwendung anmelden.
- Geräteidentifikation. Durch den Zugriff auf das Gerätezertifikat, das auf dem Gerät erstellt wurde, als es in den Arbeitsplatz eingebunden wurde.
- Überprüfung der Anwendungsidentifikation. Wenn eine Anwendung den Broker aufruft, übergibt sie die Umleitungs-URL, und der Broker überprüft sie.

Zum Aktivieren einer dieser Funktionen müssen Anwendungsentwickler beim Aufrufen der `PublicClientApplicationBuilder.CreateApplication`-Methode den `WithBroker()`-Parameter verwenden. `.WithBroker()` ist standardmäßig auf „true“ festgelegt. Außerdem müssen Entwickler die unten aufgeführten Schritte für [iOS](#brokered-authentication-for-ios)- oder [Android](#brokered-authentication-for-android)-Anwendungen ausführen.

## <a name="brokered-authentication-for-ios"></a>Brokerauthentifizierung für iOS

Führen Sie die folgenden Schritte aus, um Ihre Xamarin.iOS-App für die Kommunikation mit der [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458)-App zu aktivieren.

### <a name="step-1-enable-broker-support"></a>Schritt 1: Aktivieren der Brokerunterstützung
Die Brokerunterstützung wird pro PublicClientApplication aktiviert. Standardmäßig ist es deaktiviert. Verwenden Sie den (standardmäßig auf „true“ festgelegten) `WithBroker()`-Parameter, wenn Sie die PublicClientApplication über den PublicClientApplicationBuilder erstellen.

```CSharp
var app = PublicClientApplicationBuilder
                .Create(ClientId)
                .WithBroker()
                .WithReplyUri(redirectUriOnIos) // $"msauth.{Bundle.Id}://auth" (see step 6 below)
                .Build();
```

### <a name="step-2-update-appdelegate-to-handle-the-callback"></a>Schritt 2: Aktualisieren von AppDelegate zum Verarbeiten des Rückrufs
Wenn MSAL.NET den Broker aufruft, führt der Broker wiederum einen Rückruf Ihrer Anwendung über die `OpenUrl`-Methode der `AppDelegate`-Klasse aus. Da MSAL auf die Antwort vom Broker wartet, muss Ihre Anwendung kooperieren, damit der Rückruf von MSAL.NET erfolgen kann. Um diese Kooperation zu ermöglichen, müssen Sie die Datei `AppDelegate.cs` zur Außerkraftsetzung der folgenden Methode aktualisieren.

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

### <a name="step-3-set-a-uiviewcontroller"></a>Schritt 3: Festlegen von „UIViewController()“
Sie müssen in der Datei `AppDelegate.cs` ein Objektfenster festlegen. Normalerweise müssen Sie bei Xamarin iOS das Objektfenster nicht festlegen, aber Sie benötigen ein Objektfenster, um Antworten von einem Broker zu senden und zu empfangen. 

Zu diesem Zweck müssen Sie zwei Schritte ausführen. 
1) Legen Sie in der Datei `AppDelegate.cs` das `App.RootViewController`-Objekt auf einen neuen `UIViewController()` fest. Durch diese Zuweisung wird sichergestellt, dass ein UIViewController mit dem Aufruf des Brokers vorhanden ist. Wenn der UIViewController nicht ordnungsgemäß festgelegt wurde, erhalten Sie möglicherweise den folgenden Fehler: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
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

### <a name="step-4-register-a-url-scheme"></a>Schritt 4: Registrieren eines URL-Schemas
MSAL.NET verwendet URLs, um den Broker aufzurufen und die Brokerantwort dann an Ihre App zurückzugeben. Um den Roundtrip abzuschließen, müssen Sie in der Datei `Info.plist` ein URL-Schema für Ihre App registrieren.

Der `CFBundleURLSchemes`-Name muss das Präfix `msauth.` gefolgt von Ihrem `CFBundleURLName` enthalten.

`$"msauth.(BundleId)"`

**Beispiel:** 
`msauth.com.yourcompany.xforms`

> [!NOTE]
> Dies wird Teil des Umleitungs-URIs, der beim Empfangen der Brokerantwort zum eindeutigen Identifizieren Ihrer App verwendet wird.

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

### <a name="step-5-lsapplicationqueriesschemes"></a>Schritt 5: LSApplicationQueriesSchemes
MSAL verwendet `–canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Unter iOS 9 wurden die Schemas, die von einer Anwendung abgefragt werden können, von Apple fest vorgegeben. 

Sie müssen in der Datei `Info.plist` dem Abschnitt `LSApplicationQueriesSchemes` die Zeichenfolge **`msauthv2`** **hinzufügen**.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirecturi-in-the-application-portal"></a>Schritt 6: Registrieren Ihres Umleitungs-URIs im Anwendungsportal
Die Verwendung des Brokers bringt eine zusätzliche Anforderung an Ihren Umleitungs-URI mit sich. Der Umleitungs-URI _**muss**_ das folgende Format aufweisen:
```CSharp
$"msauth.{BundleId}://auth"
```
**Beispiel:**
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
**Sie werden bemerken, dass der Umleitungs-URI mit dem `CFBundleURLSchemes`-Namen übereinstimmt, den Sie in der Datei `Info.plist` hinzugefügt haben.**

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>Schritt 7: Sicherstellen, dass der Umleitungs-URI bei Ihrer App registriert wird

Dieser Umleitungs-URI muss im App-Registrierungsportal (https://portal.azure.com) als gültiger Umleitungs-URI für Ihre Anwendung registriert werden. 

Das Portal bietet ein neues App-Registrierungsportal, das Sie beim Berechnen des Brokerantwort-URIs aus der Paket-ID unterstützt:

1. Wählen Sie in der App-Registrierung die Option **Authentifizierung** aus, und wählen Sie dann **Neue Benutzeroberfläche ausprobieren** aus.
   ![Neue Benutzeroberfläche für die App-Registrierung ausprobieren](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. Wählen Sie **Plattform hinzufügen** aus.
   ![Plattform hinzufügen](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Wenn die Liste der Plattformen unterstützt wird, wählen Sie **iOS** aus.
   ![iOS konfigurieren](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Geben Sie die Paket-ID wie angefordert ein, und klicken Sie dann auf **Registrieren**.
   ![Paket-ID eingeben](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

5. Der Umleitungs-URI wird für Sie berechnet.
   ![Umleitungs-URI kopieren](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Brokerauthentifizierung für Android

Die Brokerunterstützung ist für Android nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

[Spezifische Überlegungen zur Universellen Windows-Plattform mit MSAL.NET](msal-net-uwp-considerations.md)