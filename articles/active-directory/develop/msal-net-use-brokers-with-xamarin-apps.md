---
title: Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin iOS- und Android-Anwendungen
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Xamarin iOS-Anwendungen, die Microsoft Authenticator verwenden können, von der Azure AD Authentication Library for .NET (ADAL.NET, Azure AD-Authentifizierungsbibliothek für .NET) zur Microsoft Authentication Library for .NET (MSAL.NET, Microsoft-Authentifizierungsbibliothek für .NET) migrieren.
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
ms.openlocfilehash: e71e9ef72e7b6caaa3894bb30c6e7e9cf762232a
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802711"
---
# <a name="use-microsoft-authenticator-or-microsoft-intune-company-portal-on-xamarin-applications"></a>Verwenden von Microsoft Authenticator oder des Microsoft Intune-Unternehmensportals für Xamarin-Anwendungen

Unter Android und iOS ermöglichen Broker wie Microsoft Authenticator oder das Microsoft Intune-Unternehmensportal (nur Android) Folgendes:

- Einmaliges Anmelden (Single Sign-On, SSO). Ihre Benutzer müssen sich nicht bei jeder Anwendung anmelden.
- Geräteidentifikation. Der Broker greift auf das Gerätezertifikat zu, das auf dem Gerät erstellt wurde, als es in den Arbeitsplatz eingebunden wurde.
- Überprüfung der Anwendungsidentifikation. Wenn eine Anwendung den Broker aufruft, übergibt sie die Umleitungs-URL, und der Broker überprüft sie.

Zum Aktivieren einer dieser Funktionen müssen Anwendungsentwickler beim Aufrufen der `PublicClientApplicationBuilder.CreateApplication`-Methode den `WithBroker()`-Parameter verwenden. `.WithBroker()` ist standardmäßig auf „true“ festgelegt. Außerdem müssen Entwickler die hier aufgeführten Schritte für [iOS](#brokered-authentication-for-ios)- oder [Android](#brokered-authentication-for-android)-Anwendungen ausführen.

## <a name="brokered-authentication-for-ios"></a>Brokerauthentifizierung für iOS

Führen Sie diese Schritte aus, um Ihre Xamarin.iOS-App für die Kommunikation mit der [Microsoft Authenticator](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458)-App zu aktivieren.

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
Wenn die Microsoft Authentication Library for .NET (MSAL.NET) den Broker aufruft, führt der Broker wiederum einen Rückruf Ihrer Anwendung über die `OpenUrl`-Methode der `AppDelegate`-Klasse aus. Da MSAL auf die Antwort vom Broker wartet, muss Ihre Anwendung kooperieren, damit der Rückruf von MSAL.NET erfolgen kann. Um diese Kooperation zu ermöglichen, müssen Sie die Datei `AppDelegate.cs` zur Außerkraftsetzung der folgenden Methode aktualisieren.

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
Sie müssen in der Datei `AppDelegate.cs` ein Objektfenster festlegen. Normalerweise müssen Sie mit Xamarin iOS nicht das Objektfenster festlegen. Um Antworten an den Broker zu senden und von ihm zu empfangen, benötigen Sie ein Objektfenster. 

Zu diesem Zweck müssen Sie zwei Schritte ausführen. 
1. Legen Sie in der Datei `AppDelegate.cs` das `App.RootViewController`-Objekt auf einen neuen `UIViewController()` fest. Diese Zuweisung stellt sicher, dass ein UIViewController mit dem Aufruf des Brokers vorhanden ist. Wenn der Controller nicht ordnungsgemäß festgelegt wird, erhalten Sie möglicherweise folgende Fehlermeldung: `"uiviewcontroller_required_for_ios_broker":"UIViewController is null, so MSAL.NET cannot invoke the iOS broker. See https://aka.ms/msal-net-ios-broker"`
1. Verwenden Sie im AcquireTokenInteractive-Aufruf den `.WithParentActivityOrWindow(App.RootViewController)`-Parameter, und übergeben Sie den Verweis an das Objektfenster, das Sie verwenden.

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
MSAL.NET verwendet URLs, um den Broker aufzurufen und die Brokerantwort dann an Ihre App zurückzugeben. Um den Roundtrip abzuschließen, registrieren Sie in der Datei `Info.plist` ein URL-Schema für Ihre App.

Der `CFBundleURLSchemes`-Name muss das Präfix `msauth.` gefolgt von Ihrem `CFBundleURLName` enthalten.

`$"msauth.(BundleId)"`

**Beispiel:**

`msauth.com.yourcompany.xforms`

> [!NOTE]
> Dieses URL-Schema wird Teil des Umleitungs-URI, der zur eindeutigen Identifizierung Ihrer App verwendet wird, wenn die Antwort vom Broker empfangen wird.

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

### <a name="step-5-add-the-broker-identifier-to-the-lsapplicationqueriesschemes-section"></a>Schritt 5: Hinzufügen der Broker-ID im Abschnitt LSApplicationQueriesSchemes
MSAL verwendet `–canOpenURL:`, um zu überprüfen, ob der Broker auf dem Gerät installiert ist. Unter iOS 9 wurden die Schemas, die von einer Anwendung abgefragt werden können, von Apple fest vorgegeben. 

Fügen Sie `msauthv2` dem Abschnitt `LSApplicationQueriesSchemes` der `Info.plist`-Datei hinzu.

```XML 
<key>LSApplicationQueriesSchemes</key>
    <array>
      <string>msauthv2</string>
    </array>
```

### <a name="step-6-register-your-redirect-uri-in-the-application-portal"></a>Schritt 6: Registrieren des Umleitungs-URI im Anwendungsportal
Die Verwendung des Brokers bringt eine zusätzliche Anforderung an Ihren Umleitungs-URI mit sich. Der Umleitungs-URI _muss_ das folgende Format aufweisen:
```CSharp
$"msauth.{BundleId}://auth"
```
**Beispiel:**
```CSharp
public static string redirectUriOnIos = "msauth.com.yourcompany.XForms://auth"; 
```
Beachten Sie, dass der Umleitungs-URI mit dem `CFBundleURLSchemes`-Namen übereinstimmt, den Sie in der Datei `Info.plist` hinzugefügt haben.

### <a name="step-7-make-sure-the-redirect-uri-is-registered-with-your-app"></a>Schritt 7: Sicherstellen, dass der Umleitungs-URI bei Ihrer App registriert wird

Dieser Umleitungs-URI muss im App-Registrierungsportal (https://portal.azure.com) als gültiger Umleitungs-URI für Ihre Anwendung registriert werden. 

Das Portal bietet ein neues App-Registrierungsportal, das Sie beim Berechnen des Brokerantwort-URIs aus der Paket-ID unterstützt.

1. Wählen Sie in der App-Registrierung die Option **Authentifizierung** aus, und wählen Sie dann **Neue Benutzeroberfläche ausprobieren** aus.

   ![Ausprobieren der neuen Benutzeroberfläche für die App-Registrierung](media/msal-net-use-brokers-with-xamarin-apps/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

1. Wählen Sie **Eine Plattform hinzufügen** aus.

   ![Hinzufügen einer Plattform](media/msal-net-use-brokers-with-xamarin-apps/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

1. Wenn die Liste der Plattformen unterstützt wird, wählen Sie **iOS** aus.

   ![Konfigurieren von iOS](media/msal-net-use-brokers-with-xamarin-apps/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

1. Geben Sie die Paket-ID wie angefordert ein, und wählen Sie dann **Konfigurieren** aus.

   ![Eingeben der Paket-ID](media/msal-net-use-brokers-with-xamarin-apps/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

1. Der Umleitungs-URI wird für Sie berechnet.

   ![Kopieren des Umleitungs-URI](media/msal-net-use-brokers-with-xamarin-apps/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

## <a name="brokered-authentication-for-android"></a>Brokerauthentifizierung für Android

Die Brokerunterstützung ist für Android nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über [Spezifische Überlegungen zur Universellen Windows-Plattform mit MSAL.NET](msal-net-uwp-considerations.md).