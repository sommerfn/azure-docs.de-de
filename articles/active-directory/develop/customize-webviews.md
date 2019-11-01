---
title: Anpassen von Browsern und Webansichten
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie die Browseroberfläche von MSAL für iOS und macOS beim Anmelden von Benutzern anpassen.
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcb314e46094bb6c283a17508c35b7fc17e010e5
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803376"
---
# <a name="how-to-customize-browsers-and-webviews-for-iosmacos"></a>Gewusst wie: Anpassen von Browsern und Webansichten für iOS/macOS

Ein Webbrowser ist Voraussetzung für die interaktive Authentifizierung. Unter iOS verwendet die Microsoft Authentication Library (MSAL) standardmäßig den Systemwebbrowser (der möglicherweise im Vordergrund angezeigt wird) für die interaktive Authentifizierung bei der Anmeldung von Benutzern. Die Nutzung des Systembrowsers hat den Vorteil, dass der SSO-Status (Single Sign-On, einmaliges Anmelden) für andere Anwendungen und Webanwendungen freigegeben wird.

Sie können die Benutzeroberflächen ändern, indem Sie die Konfiguration an andere Optionen zum Anzeigen von Webinhalten anpassen, wie etwa:

Nur für iOS:

- [ASWebAuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession?language=objc)
- [SFAuthenticationSession](https://developer.apple.com/documentation/safariservices/sfauthenticationsession?language=objc) 
- [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?language=objc)

Für iOS und macOS:

- [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview?language=objc)

MSAL für macOS unterstützt nur `WKWebView`.

## <a name="system-browsers"></a>Systembrowser

Für iOS gelten `ASWebAuthenticationSession`, `SFAuthenticationSession` und `SFSafariViewController` als Systembrowser. Im Allgemeinen geben Systembrowser Cookies und andere Websitedaten für die Safari-Browseranwendung frei.

Standardmäßig erkennt MSAL die iOS-Version dynamisch und wählt den empfohlenen Systembrowser aus, der für diese Version verfügbar ist. Unter iOS 12 und höher wird `ASWebAuthenticationSession` verwendet. 

| Version | Webbrowser |
|:-------------:|:-------------:|
| iOS 12 und höher | ASWebAuthenticationSession |
| iOS 11 | SFAuthenticationSession |
| iOS 10 | SFSafariViewController |

Für MSAL-Apps können Entwickler auch einen anderen Systembrowser auswählen:

- `SFAuthenticationSession` entspricht der iOS 11-Version von `ASWebAuthenticationSession`.
- `SFSafariViewController` ist universeller einsetzbar, stellt eine Schnittstelle für die Internetsuche bereit und kann auch zum Anmelden verwendet werden. In iOS 9 und 10 werden Cookies und andere Websitedaten für Safari freigegeben. Dies gilt jedoch nicht für iOS 11 und höher.

## <a name="in-app-browser"></a>In-App-Browser

[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ist ein In-App-Browser zum Anzeigen von Webinhalten. Er gibt keine Cookies oder Websitedaten für andere Instanzen von **WKWebView** oder den Safari-Browser frei. WKWebView ist ein plattformübergreifender Browser, der sowohl für iOS als auch für macOS verfügbar ist.

## <a name="cookie-sharing-and-single-sign-on-sso-implications"></a>Auswirkungen von Cookies-Freigabe auf einmaliges Anmelden

Die Wahl des Browsers hat durch den jeweiligen Umgang mit der Freigabe von Cookies Auswirkungen auf das einmalige Anmelden (Single Sign-On, SSO). In der folgenden Tabelle werden die Auswirkungen der einzelnen Browser auf das einmalige Anmelden zusammengefasst.

| Technologie    | Browsertyp  | Verfügbarkeit in iOS | Verfügbarkeit in macOS | Freigabe von Cookies und anderen Daten  | Verfügbarkeit in MSAL | SSO |
|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|-------------:|
| [ASWebAuthenticationSession](https://developer.apple.com/documentation/authenticationservices/aswebauthenticationsession) | System | iOS 12 und höher | macOS 10.15 und höher | Ja | nur iOS | mit Safari-Instanzen
| [SFAuthenticationSession](https://developer.apple.com/documentation/safariservices/sfauthenticationsession) | System | iOS 11 und höher | – | Ja | nur iOS |  mit Safari-Instanzen
| [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) | System | iOS 11 und höher | – | Nein | nur iOS | nein**
| **SFSafariViewController** | System | iOS 10 | – | Ja | nur iOS |  mit Safari-Instanzen
| **WKWebView**  | In-App | iOS 8 und höher | macOS 10.10 und höher | Nein | iOS und macOS | nein**

** Für die einmalige Anmeldung müssen Token von Apps gemeinsam genutzt werden. Hierfür ist ein Tokencache oder eine Brokeranwendung wie z. B. Microsoft Authenticator für iOS erforderlich.

## <a name="change-the-default-browser-for-the-request"></a>Ändern des Standardbrowsers für die Anforderung

Sie können einen In-App-Browser oder je nach Benutzererfahrung einen bestimmten Systembrowser verwenden, indem Sie die folgende Eigenschaft in `MSALWebviewParameters` ändern:

```objc
@property (nonatomic) MSALWebviewType webviewType;
```

## <a name="change-per-interactive-request"></a>Ändern des Standardbrowsers mittels interaktiver Anforderung

Sie können jede Anforderung so konfigurieren, dass der Standardbrowser überschrieben wird, indem Sie die Eigenschaft `MSALInteractiveTokenParameters.webviewParameters.webviewType` vor der Übergabe an die API `acquireTokenWithParameters:completionBlock:` ändern.

Außerdem unterstützt MSAL die Übergabe eines benutzerdefinierten `WKWebView`-Browsers durch Festlegen der Eigenschaft `MSALInteractiveTokenParameters.webviewParameters.customWebView`.

Beispiel:

Objective-C
```objc
UIViewController *myParentController = ...;
WKWebView *myCustomWebView = ...;
MSALWebviewParameters *webViewParameters = [[MSALWebviewParameters alloc] initWithParentViewController:myParentController];
webViewParameters.webviewType = MSALWebviewTypeWKWebView;
webViewParameters.customWebview = myCustomWebView;
MSALInteractiveTokenParameters *interactiveParameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"myscope"] webviewParameters:webViewParameters];
    
[app acquireTokenWithParameters:interactiveParameters completionBlock:completionBlock];
```
Swift
```swift
let myParentController: UIViewController = ...
let myCustomWebView: WKWebView = ...
let webViewParameters = MSALWebviewParameters(parentViewController: myParentController)
webViewParameters.webviewType = MSALWebviewType.wkWebView
webViewParameters.customWebview = myCustomWebView
let interactiveParameters = MSALInteractiveTokenParameters(scopes: ["myscope"], webviewParameters: webViewParameters)

app.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
```

Bei einer benutzerdefinierten Webansicht wird der Status der angezeigten Webinhalte mithilfe von Benachrichtigungen angezeigt, wie z. B.:

```objc
/*! Fired at the start of a resource load in the webview. The URL of the load, if available, will be in the @"url" key in the userInfo dictionary */
extern NSString *MSALWebAuthDidStartLoadNotification;

/*! Fired when a resource finishes loading in the webview. */
extern NSString *MSALWebAuthDidFinishLoadNotification;

/*! Fired when web authentication fails due to reasons originating from the network. Look at the @"error" key in the userInfo dictionary for more details.*/
extern NSString *MSALWebAuthDidFailNotification;

/*! Fired when authentication finishes */
extern NSString *MSALWebAuthDidCompleteNotification;

/*! Fired before ADAL invokes the broker app */
extern NSString *MSALWebAuthWillSwitchToBrokerApp;
```

### <a name="options"></a>Optionen

Alle von MSAL unterstützten Typen von Webbrowsern werden in der Enumeration [MSALWebviewType](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALDefinitions.h#L47) deklariert.

```objc
typedef NS_ENUM(NSInteger, MSALWebviewType)
{
#if TARGET_OS_IPHONE
    // For iOS 11 and up, uses AuthenticationSession (ASWebAuthenticationSession
    // or SFAuthenticationSession).
    // For older versions, with AuthenticationSession not being available, uses
    // SafariViewController.
    MSALWebviewTypeDefault,
    
    // Use SFAuthenticationSession/ASWebAuthenticationSession
    MSALWebviewTypeAuthenticationSession,
    
    // Use SFSafariViewController for all versions.
    MSALWebviewTypeSafariViewController,
    
#endif
    // Use WKWebView
    MSALWebviewTypeWKWebView,
};
```

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr zu [Authentifizierungsfluss und Anwendungsszenarios](authentication-flows-app-scenarios.md)
