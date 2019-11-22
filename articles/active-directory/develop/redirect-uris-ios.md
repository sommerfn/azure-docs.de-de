---
title: Verwenden von Umleitungs-URIs mit der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL)
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Unterschiede zwischen der Microsoft Authentication Library für Objective-C (MSAL für iOS und macOS) und der Azure AD-Authentifizierungsbibliothek für Objective-C (ADAL.ObjC) sowie über die Migration zwischen ihnen.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: jak
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b78906a03eb9dc96fb58dea4ceabff953f2a3e4f
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803785"
---
# <a name="using-redirect-uris-with-the-microsoft-authentication-library-for-ios-and-macos"></a>Verwenden von Umleitungs-URIs mit der Microsoft Authentication Library für iOS und macOS

Wenn ein Benutzer sich authentifiziert, sendet Azure Active Directory (Azure AD) das Token mithilfe des Umleitungs-URI, der bei der Azure AD-Anwendung registriert ist, an die App.

Die Microsoft Authentication Library (MSAL) erfordert, dass der Umleitungs-URI bei der Azure AD-App in einem bestimmten Format registriert ist. Wenn kein Umleitungs-URI angegeben wird, verwendet die MSAL einen Standardumleitungs-URI. Das Format ist `msauth.[Your_Bundle_Id]://auth`.

Das Standardformat für den Umleitungs-URI kann für die meisten Apps und Szenarien, einschließlich vermittelter Authentifizierung und der Systemwebansicht, verwendet werden. Verwenden Sie nach Möglichkeit das Standardformat.

Für erweiterte Szenarien müssen Sie jedoch den Umleitungs-URI möglicherweise ändern, wie unten beschrieben.

## <a name="scenarios-that-require-a-different-redirect-uri"></a>Szenarien, die einen anderen Umleitungs-URI erfordern

### <a name="cross-app-single-sign-on-sso"></a>Anwendungsübergreifendes einmaliges Anmelden (Single Sign-On, SSO)

Damit Token von der Microsoft Identity Plattform App-übergreifend verwendet werden können, muss jede App dieselbe Client-ID oder Anwendungs-ID aufweisen. Dies ist der eindeutige Bezeichner, der für Sie bereitgestellt wurde, als Sie Ihre App im Portal registriert haben (nicht die Anwendungspaket-ID, die Sie pro App bei Apple registrieren).

Die Umleitungs-URIs müssen für jede iOS-App unterschiedlich sein. So kann der Microsoft-Identitätsdienst unterschiedliche Apps, die über die gleiche Anwendungs-ID verfügen, eindeutig identifizieren. Für jede Anwendung können im Azure-Portal mehrere Umleitungs-URIs registriert sein. Jede App innerhalb Ihrer Suite kann einen anderen Umleitungs-URI verwenden. Beispiel:

Im Azure-Portal ist die folgende Anwendungsregistrierung vorhanden:

    Client ID: ABCDE-12345 (this is a single client ID)
    RedirectUris: msauth.com.contoso.app1://auth, msauth.com.contoso.app2://auth, msauth.com.contoso.app3://auth

App1 verwendet den Umleitungs-URI `msauth.com.contoso.app1://auth`, App2 verwendet `msauth.com.contoso.app2://auth`, App3 verwendet `msauth.com.contoso.app1://auth`

### <a name="migrating-from-adal-to-msal"></a>Migrieren von ADAL zu MSAL

Wenn Sie Code, in dem die Azure AD-Authentifizierungsbibliothek (Azure AD Authentication Library, ADAL) verwendet wird, zur MSAL migrieren, ist möglicherweise bereits ein Umleitungs-URI für Ihre App konfiguriert. Sie können weiterhin denselben Umleitungs-URI verwenden, sofern Ihre ADAL-App für die Unterstützung von vermittelter Authentifizierung konfiguriert wurde und der Umleitungs-URI die MSAL-Formatanforderungen für Umleitungs-URIs erfüllt.

## <a name="msal-redirect-uri-format-requirements"></a>MSAL-Formatanforderungen für Umleitungs-URIs

* Der MSAL-Umleitungs-URI muss das Format `<scheme>://host` aufweisen.

    Dabei ist `<scheme>` eine eindeutige Zeichenfolge, die die App identifiziert. Sie basiert hauptsächlich auf der Paket-ID der Anwendung, um Eindeutigkeit sicherzustellen. Wenn die Paket-ID der App beispielsweise `com.contoso.myapp` lautet, hat der Umleitungs-URI das folgende Format: `msauth.com.contoso.myapp://auth`.

    Wenn Sie von der ADAL migrieren, weist der Umleitungs-URI wahrscheinlich folgendes Format auf: `<scheme>://[Your_Bundle_Id]`, wobei `scheme` eine eindeutige Zeichenfolge ist. Dieses Format ist auch bei Verwendung der MSAL gültig.

* `<scheme>` muss in der Datei „Info.plist“ der App unter `CFBundleURLTypes > CFBundleURLSchemes` registriert sein.  In diesem Beispiel wurde „Info. plist“ als Quellcode geöffnet:

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>msauth.[BUNDLE_ID]</string>
            </array>
        </dict>
    </array>
    ```
    

Die MSAL überprüft, ob der Umleitungs-URI ordnungsgemäß registriert ist, und gibt einen Fehler zurück, wenn dies nicht der Fall ist.
    
* Wenn Sie als Umleitungs-URI universelle Links verwenden möchten, muss `<scheme>` den Wert `https` aufweisen und braucht nicht in `CFBundleURLSchemes` deklariert zu werden. Konfigurieren Sie stattdessen die App und die Domäne gemäß den Anweisungen von Apple unter [Universal Links for Developers](https://developer.apple.com/ios/universal-links/) (Universelle Links für Entwickler, in englischer Sprache), und rufen Sie die `handleMSALResponse:sourceApplication:`-Methode von `MSALPublicClientApplication` auf, wenn Ihre Anwendung über einen universellen Link geöffnet wird.

## <a name="use-a-custom-redirect-uri"></a>Verwenden eines benutzerdefinierten Umleitungs-URI

Um einen benutzerdefinierten Umleitungs-URI zu verwenden, übergeben Sie den `redirectUri`-Parameter an `MSALPublicClientApplicationConfig`, und übergeben Sie dieses Objekt an `MSALPublicClientApplication`, wenn Sie das Objekt initialisieren. Wenn der Umleitungs-URI ungültig ist, gibt der Initialisierer `nil` zurück und legt den `redirectURIError` mit zusätzlichen Informationen fest.  Beispiel:

Objective-C:

```objc
MSALPublicClientApplicationConfig *config =
        [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"your-client-id"
                                                        redirectUri:@"your-redirect-uri"
                                                        authority:authority];
NSError *redirectURIError;
MSALPublicClientApplication *application =
        [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&redirectURIError];
```

Swift:

```swift
let config = MSALPublicClientApplicationConfig(clientId: "your-client-id",
                                            redirectUri: "your-redirect-uri",
                                              authority: authority)
do {
  let application = try MSALPublicClientApplication(configuration: config)
  // continue on with application          
} catch let error as NSError {
  // handle error here
}       
```



## <a name="handle-the-url-opened-event"></a>Behandeln des URL-Öffnungsereignisses

Die Anwendung sollte die MSAL aufrufen, wenn sie über URL-Schemas oder universelle Links eine Antwort empfängt. Rufen Sie die `handleMSALResponse:sourceApplication:`-Methode von `MSALPublicClientApplication`, wenn die Anwendung geöffnet wird. Hier sehen Sie ein Beispiel für benutzerdefinierte Schemas:

Objective-C:

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

Swift:

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}
```



## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr zu [Authentifizierungsfluss und Anwendungsszenarios](authentication-flows-app-scenarios.md)
