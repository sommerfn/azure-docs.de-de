---
title: 'Schnellstart: Microsoft Identity Platform – iOS und macOS | Azure'
description: Hier finden Sie Informationen zum Anmelden von Benutzern und zum Abfragen von Microsoft Graph in einer iOS- oder macOS-Anwendung.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2019
ms.author: twhitney
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:iOS
ms.collection: M365-identity-device-management
ms.openlocfilehash: b515a7954d82cdd377cec72fa8525fbd9691351d
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73149510"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-or-macos-app"></a>Schnellstart: Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer iOS- oder macOS-App

Diese Schnellstartanleitung enthält ein Codebeispiel, das zeigt, wie eine native iOS- oder macOS-Anwendung Microsoft Identity Platform verwenden kann, um persönliche Konten und Geschäfts-, Schul- oder Unikonten anzumelden, ein Zugriffstoken abzurufen und die Microsoft Graph-API aufzurufen.

Dieser Schnellstart gilt für iOS- und macOS-Apps. Einige Schritte sind nur bei iOS-Apps erforderlich. Bei diesen Schritten wird darauf hingewiesen, dass sie nur für iOS gelten.

![Zeigt, wie die in diesem Schnellstart generierte Beispiel-App funktioniert](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Voraussetzungen**
> * XCode 10 oder höher
> * iOS 10 oder höher 
> * macOS 10.12 oder höher

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registrieren und Herunterladen Ihrer Schnellstart-App
> Die Schnellstartanwendung kann auf zwei Arten gestartet werden:
> * [Express] [Option 1: Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels](#option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample)
> * [Manuell] [Option 2: Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-the-code-sample"></a>Option 1: Registrieren und automatisches Konfigurieren Ihrer App und anschließendes Herunterladen des Codebeispiels
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
> So registrieren Sie Ihre App:
> 1. Navigieren Sie zum neuen Bereich [Azure-Portal – App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs).
> 1. Geben Sie einen Namen für Ihre Anwendung ein, und wählen Sie **Registrieren** aus.
> 1. Befolgen Sie die Anweisungen, um Ihre neue Anwendung mit nur einem Klick herunterzuladen und automatisch zu konfigurieren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Option 2: Registrieren und manuelles Konfigurieren Ihrer Anwendung und des Codebeispiels
>
> #### <a name="step-1-register-your-application"></a>Schritt 1: Anwendung registrieren
> Führen Sie die folgenden Schritte aus, um Ihre Anwendung zu registrieren und Ihrer Projektmappe manuell die Registrierungsinformationen Ihrer App hinzuzufügen:
>
> 1. Navigieren Sie zur Seite [App-Registrierungen](https://aka.ms/MobileAppReg) von Microsoft Identity Platform für Entwickler.
> 1. Wählen Sie **Neue Registrierung** aus.
> 1. Geben Sie auf der daraufhin angezeigten Seite **Anwendung registrieren** die Registrierungsinformationen für Ihre Anwendung ein:
>      - Geben Sie im Abschnitt **Name** einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird, wenn sie sich anmelden oder Ihrer App zustimmen.
>      - Überspringen Sie die weiteren Konfigurationsschritte auf dieser Seite.
>      - Wählen Sie `Register` aus.
> 1. Wählen Sie im Abschnitt **Verwalten** die Option `Authentication` > `Add Platform` > `iOS` aus.
>      - Geben Sie die ***Bündel-ID*** für Ihre Anwendung ein. Die Paket-ID ist lediglich eine Zeichenfolge, die Ihre Anwendung eindeutig identifiziert (z. B. `com.<yourname>.identitysample.MSALMacOS`). Notieren Sie sich den verwendeten Wert.
>      - Die iOS-Konfiguration ist auch bei macOS-Anwendungen anwendbar.
> 1. Wählen Sie `Configure` aus, und speichern Sie die Details der ***MSAL-Konfiguration*** zur späteren Verwendung in diesem Schnellstart.
> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>Schritt 1: Konfigurieren der Anwendung
> Damit das Codebeispiel für diese Schnellstartanleitung funktioniert, müssen Sie einen mit dem Authentifizierungsbroker kompatiblen Umleitungs-URI hinzufügen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Diese Änderung für mich vornehmen]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Bereits konfiguriert](media/quickstart-v2-ios/green-check.png): Ihre Anwendung ist mit diesen Attributen konfiguriert.

#### <a name="step-2-download-the-sample-project"></a>Schritt 2: Herunterladen des Beispielprojekts

- [Herunterladen des Codebeispiels für iOS](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)
- [Herunterladen des Codebeispiels für macOS](https://github.com/Azure-Samples/active-directory-macOS-swift-native-v2/archive/master.zip)

#### <a name="step-3-install-dependencies"></a>Schritt 3: Installieren von Abhängigkeiten

Navigieren Sie in einem Terminalfenster zu dem Ordner, der das heruntergeladene Codebeispiel enthält, und führen Sie `pod install` zum Installieren der MSAL-Bibliothek aus.

#### <a name="step-4-configure-your-project"></a>Schritt 4: Konfigurieren des Projekts

> [!div renderon="docs"]
> Wenn Sie weiter oben die erste Option ausgewählt haben, können Sie diese Schritte überspringen. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Extrahieren Sie die ZIP-Datei, und öffnen Sie das Projekt in XCode.
> 1. Bearbeiten Sie **ViewController.swift**, und ersetzen Sie die Zeile, die mit „let kClientID“ beginnt, durch den folgenden Codeausschnitt. Sie müssen den Wert für `kClientID` durch die Client-ID ersetzen, die Sie zuvor im Schnellstart beim Registrieren Ihrer App im Portal gespeichert haben:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. Öffnen Sie die Projekteinstellungen. Geben Sie im Abschnitt **Identität** den Wert für **Bundle Identifier** (Paket-ID) ein, den Sie im Portal angegeben haben.
> 1. Nur iOS: Klicken Sie mit der rechten Maustaste auf **Info.plist**, und wählen Sie **Öffnen als** > **Quellcode** aus.
> 1. Nur iOS: Ersetzen Sie unter dem dict-Stammknoten `CFBundleURLSchemes` durch den Wert für ***Bundle Id*** (Paket-ID), die Sie im Portal eingegeben haben.
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
>    ```
> 1. Erstellen Sie die App, und führen Sie die App aus!
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Dieser Schnellstart unterstützt Enter_the_Supported_Account_Info_Here.
> [!div renderon="docs"]
>
> 1. Extrahieren Sie die ZIP-Datei, und öffnen Sie das Projekt in XCode.
> 1. Bearbeiten Sie **ViewController.swift**, und ersetzen Sie die Zeile, die mit „let kClientID“ beginnt, durch den folgenden Codeausschnitt. Sie müssen den Wert für `kClientID` durch die Client-ID ersetzen, die Sie zuvor im Schnellstart beim Registrieren Ihrer App im Portal gespeichert haben:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_Here"
>    ```
> 1. Öffnen Sie die Projekteinstellungen. Geben Sie im Abschnitt **Identität** den Wert für **Bundle Identifier** (Paket-ID) ein, den Sie im Portal angegeben haben.
> 1. Nur iOS: Klicken Sie mit der rechten Maustaste auf **Info.plist**, und wählen Sie **Öffnen als** > **Quellcode** aus.
> 1. Nur iOS: Ersetzen Sie unter dem dict-Stammknoten `Enter_the_bundle_Id_Here` durch den Wert für ***Bundle Id*** (Paket-ID), den Sie im Portal verwendet haben.
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
> 
>    ```
> 1. Erstellen Sie die App, und führen Sie die App aus! 

## <a name="more-information"></a>Weitere Informationen

Lesen Sie diese Abschnitte, um mehr über diesen Schnellstart zu erfahren.

### <a name="get-msal"></a>Abrufen von MSAL

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) ist die Bibliothek zum Anmelden von Benutzern und zum Anfordern von Token, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet werden. Sie können Ihrer Anwendung MSAL mithilfe des folgenden Vorgangs hinzufügen:

```
$ vi Podfile

```
Fügen Sie der Podfile-Datei den folgenden Code (mit dem Ziel Ihres Projekts) hinzu:

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL'
end

```

Ausführen des CocoaPods-Installationsbefehls:

```pod install```

### <a name="initialize-msal"></a>MSAL initialisieren

Sie können den Verweis auf MSAL hinzufügen, indem Sie den folgenden Code hinzufügen:

```swift
import MSAL
```

Initialisieren Sie MSAL anschließend mit dem folgenden Code:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)
            
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
```

> |Hierbei gilt: ||
> |---------|---------|
> | `clientId` | Die Anwendungs-ID der in *portal.azure.com* registrierten Anwendung. |
> | `authority` | Der Microsoft Identity Platform-Endpunkt. In den meisten Fällen ist dies *https<span/>://login.microsoftonline.com/common*. |
> | `redirectUri` | Der Umleitungs-URI der Anwendung. Sie können „nil“ übergeben, um den Standardwert zu verwenden, oder Ihren benutzerdefinierten Umleitungs-URI angeben. |

### <a name="for-ios-only-additional-app-requirements"></a>Nur iOS: zusätzliche App-Anforderungen

Ihre App muss auch Folgendes in `AppDelegate` enthalten. Dadurch kann das MSAL SDK die Tokenantwort der Authentifizierungsbroker-App verarbeiten, wenn Sie die Authentifizierung ausführen.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
    }

 ```

> [!NOTE]
> Wenn Sie unter iOS 13 oder höher `UISceneDelegate` anstelle von `UIApplicationDelegate` übernehmen, platzieren Sie diesen Code stattdessen im `scene:openURLContexts:`-Rückruf (siehe [Dokumentation von Apple](https://developer.apple.com/documentation/uikit/uiscenedelegate/3238059-scene?language=objc)).
> Wenn Sie sowohl UISceneDelegate als auch UIApplicationDelegate für die Kompatibilität mit älteren iOS-Versionen unterstützen, muss der MSAL-Rückruf für beide Optionen zur Verfügung gestellt werden.

 ```swift
 func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
 ```

Ihre App muss neben `CFBundleURLTypes` über einen `LSApplicationQueriesSchemes`-Eintrag in ***Info.plist*** verfügen. Im Beispiel ist dies enthalten. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauthv2</string>
      <string>msauthv3</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Anmelden von Benutzern und Anfordern von Token

MSAL verfügt über zwei Methoden, die zum Abrufen von Token verwendet werden: `acquireToken` und `acquireTokenSilent`.

#### <a name="acquiretoken-get-a-token-interactively"></a>acquireToken: Interaktives Abrufen eines Tokens

In einigen Situationen müssen Benutzer mit Microsoft Identity Platform interagieren. In diesen Fällen muss der Endbenutzer möglicherweise sein Konto auswählen, seine Anmeldeinformationen eingeben oder den Berechtigungen Ihrer App zustimmen. Beispiel: 

* Erstmaliges Anmelden von Benutzern bei der Anwendung.
* Wenn ein Benutzer sein Kennwort zurücksetzt, muss er seine Anmeldeinformationen eingeben. 
* Wenn Ihre Anwendung zum ersten Mal Zugriff auf eine Ressource anfordert
* Wenn MFA oder andere Richtlinien für bedingten Zugriff erforderlich sind

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes, webviewParameters: self.webViewParamaters!)
self.applicationContext!.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Hierbei gilt:||
> |---------|---------|
> | `scopes` | Enthält die angeforderten Bereiche, d. h. `[ "user.read" ]` für Microsoft Graph oder `[ "<Application ID URL>/scope" ]` für benutzerdefinierte Web-APIs (`api://<Application ID>/access_as_user`) |

#### <a name="acquiretokensilent-get-an-access-token-silently"></a>acquireTokenSilent: Abrufen eines Zugriffstokens im Hintergrund

Apps sollten nicht verlangen, dass sich die Benutzer jedes Mal anmelden, wenn sie ein Token anfordern. Wenn sich der Benutzer bereits angemeldet hat, haben Apps durch diese Methode die Möglichkeit, Token im Hintergrund anzufordern. 

```swift
guard let account = try self.applicationContext!.allAccounts().first else { return }
        
let silentParams = MSALSilentTokenParameters(scopes: kScopes, account: account)
self.applicationContext!.acquireTokenSilent(with: silentParams) { (result, error) in /* Add your handling logic */}
```

> |Hierbei gilt: ||
> |---------|---------|
> | `scopes` | Enthält die angeforderten Bereiche, d. h. `[ "user.read" ]` für Microsoft Graph oder `[ "<Application ID URL>/scope" ]` für benutzerdefinierte Web-APIs (`api://<Application ID>/access_as_user`) |
> | `account` | Das Konto, für das ein Token angefordert wird. In dieser Schnellstartanleitung wird eine einzelne Kontoanwendung angezeigt. Wenn Sie eine App mit mehreren Konten erstellen möchten, müssen Sie die Logik zum Identifizieren des für Tokenanforderungen zu verwendenden Kontos mithilfe von `applicationContext.account(forHomeAccountId: self.homeAccountId)` definieren. |

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie das iOS-Tutorial aus, um eine vollständige Schritt-für-Schritt-Anleitung zum Erstellen von Anwendungen einschließlich einer vollständigen Erläuterung dieses Schnellstarts zu erhalten.

### <a name="learn-how-to-create-the-application-used-in-this-quickstart"></a>Informationen zum Erstellen der in diesem Schnellstart verwendeten Anwendung

> [!div class="nextstepaction"]
> [Tutorial: Aufrufen der Graph-API (iOS)](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
