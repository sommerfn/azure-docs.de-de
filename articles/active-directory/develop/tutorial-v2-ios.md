---
title: 'Erste Schritte mit iOS: Microsoft Identity Platform | Azure'
description: So können iOS-Anwendungen (Swift) über Microsoft Identity Platform eine API aufrufen, für die Zugriffstoken erforderlich sind
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 07/15/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2968ae229c52cdd2b36111710f0aa6c0dc4fce4c
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852415"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Anmelden von Benutzern und Aufrufen von Microsoft Graph in einer iOS-App

In diesem Tutorial wird die Integration einer iOS-App in Microsoft Identity Platform beschrieben. Die App meldet einen Benutzer an, ruft ein Zugriffstoken zum Aufrufen der Microsoft Graph-API ab und sendet eine Anforderung an die Microsoft Graph-API.  

Am Ende dieses Leitfadens akzeptiert Ihre Anwendung Anmeldungen von persönlichen Microsoft-Konten (z.B. outlook.com, live.com u.a.) sowie von Geschäfts,- Schul- oder Unikonten von allen Unternehmen oder Organisationen, die Azure Active Directory nutzen.

## <a name="how-this-tutorial-works"></a>So funktioniert dieses Tutorial

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Mit der App in diesem Tutorial werden Benutzer angemeldet und Daten in ihrem Namen abgerufen.  Auf diese Daten wird über eine geschützte API (hier: Microsoft Graph-API) zugegriffen, für die eine Autorisierung erforderlich ist und die über Microsoft Identity Platform geschützt ist.

Dies gilt insbesondere in folgenden Fällen:

* Die App meldet den Benutzer entweder über einen Browser oder über Microsoft Authenticator an.
* Der Endbenutzer akzeptiert die von Ihrer Anwendung angeforderten Berechtigungen.
* Ihre App stellt ein Zugriffstoken für die Microsoft Graph-API aus.
* Das Zugriffstoken ist in der HTTP-Anforderung an die Web-API enthalten.
* Die Microsoft Graph-Antwort wird verarbeitet.

Dieses Beispiel verwendet die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) zum Implementieren der Authentifizierung. MSAL erneuert Token automatisch, ermöglicht das einmalige Anmelden zwischen anderen Apps auf dem Gerät und verwaltet die Konten.

## <a name="prerequisites"></a>Voraussetzungen

- Zum Erstellen der App in diesem Leitfaden ist Version 10.x von Xcode erforderlich. Können Sie XCode von der [iTunes-Website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode-Download-URL") herunterladen.
- Microsoft Authentication Library ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Sie können einen Abhängigkeitsmanager verwenden oder die Bibliothek manuell hinzufügen. In den nachstehenden Anweisungen wird die Vorgehensweise erläutert.

In diesem Tutorial wird ein neues Projekt erstellt. Wenn Sie stattdessen das abgeschlossene Tutorial herunterladen möchten, [laden Sie den Code](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) herunter.

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

1. Starten Sie Xcode, und wählen Sie **Create a new Xcode project** (Neues Xcode-Projekt erstellen) aus.
2. Wählen Sie **iOS** > **Single view App** (Einzelansicht-App) und dann **Next** (Weiter) aus.
3. Geben Sie einen Produktnamen ein.
4. Legen Sie die **Sprache** auf **Swift** fest, und wählen Sie **Next** (Weiter) aus.
5. Wählen Sie einen Ordner für das Erstellen Ihrer App aus, und klicken Sie auf **Erstellen**.

## <a name="register-your-application"></a>Anwendung registrieren

1. [Navigieren Sie zum Azure-Portal](https://aka.ms/MobileAppReg).
2. Öffnen Sie das Blatt [App-Registrierungen](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), und klicken Sie auf **+ Neue Registrierung**.
3. Geben Sie einen **Namen** für Ihre App ein, und klicken Sie auf **Registrieren**, ohne einen Umleitungs-URI festzulegen.
4. Wählen Sie im Abschnitt **Verwalten** des angezeigten Bereichs die Option **Authentifizierung** aus.
5. Klicken Sie im oberen Bildschirmbereich auf **Neue Benutzeroberfläche ausprobieren** um die neue Benutzeroberfläche für die App-Registrierung zu öffnen, und klicken Sie dann auf **+Neue Registrierung** >  **+ Plattform hinzufügen** > **iOS**.
    - Geben Sie die Bündel-ID Ihres Projekts ein. Wenn Sie den Code heruntergeladen haben, lautet diese `com.microsoft.identitysample.MSALiOS`. Wählen Sie bei der Erstellung eines eigenen Projekts Ihr Projekt in Xcode aus, und öffnen Sie die Registerkarte **General** (Allgemein). Die Paket-ID wird im Abschnitt **Identity** (Identität) angezeigt.
6. Klicken Sie auf `Configure`, und speichern Sie die **MSAL-Konfiguration**, die auf der Seite **iOS-Konfiguration** angezeigt wird, damit Sie diese später beim Konfigurieren Ihrer App eingeben können.  Klicken Sie auf **Fertig**.

## <a name="add-msal"></a>Hinzufügen von MSAL

Installieren Sie MSAL mit einer der folgenden Optionen in Ihrer App:

### <a name="cocoapods"></a>CocoaPods

1. Wenn Sie [CocoaPods](https://cocoapods.org/) verwenden, installieren Sie `MSAL`. Erstellen Sie dazu zunächst in dem Ordner, in dem sich auch die Datei vom Typ `.xcodeproj` Ihres Projekts befindet, eine leere Datei namens `podfile`. Fügen Sie Folgendes zu `podfile` hinzu:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL', '~> 0.4.0'
   end
   ```

2. Ersetzen Sie `<your-target-here>` durch den Namen Ihres Projekts.
3. Navigieren Sie in einem Terminalfenster zu dem Ordner, der das erstellte `podfile`-Element enthält, und führen Sie `pod install` zum Installieren von MSAL aus.
4. Schließen Sie Xcode, und öffnen Sie `<your project name>.xcworkspace`, um das Projekt erneut in Xcode zu laden.

### <a name="carthage"></a>Carthage

Wenn Sie [Carthage](https://github.com/Carthage/Carthage) verwenden, installieren Sie `MSAL`, indem Sie die Bibliothek Ihrer Datei vom Typ `Cartfile` hinzufügen:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

### <a name="manually"></a>Manuell

Sie können auch ein Git-Submodul oder das neueste Release als Framework in Ihrer Anwendung verwenden.

## <a name="add-your-app-registration"></a>Hinzufügen der App-Registrierung

Als Nächstes fügen Sie dem Code die App-Registrierung hinzu. 

Fügen Sie zunächst am Anfang der Dateien `ViewController.swift` und `AppDelegate.swift` die folgende import-Anweisung hinzu:

```swift
import MSAL
```

Fügen Sie anschließend `ViewController.swift` vor `viewDidLoad()` den folgenden Code hinzu:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

Ändern Sie den `kClientID` zugewiesenen Wert in Ihre Anwendungs-ID. Dieser Wert ist Teil der MSAL-Konfigurationsdaten, die Sie am Anfang dieses Tutorials zum Registrieren der Anwendung im Azure-Portal gespeichert haben.

## <a name="configure-url-schemes"></a>Konfigurieren von URL-Schemas

In diesem Schritt registrieren Sie `CFBundleURLSchemes`, damit der Benutzer nach der Anmeldung zurück zur App geleitet werden kann. `LSApplicationQueriesSchemes` ermöglicht Ihrer App darüber hinaus die Verwendung von Microsoft Authenticator.

Öffnen Sie `Info.plist` in Xcode als Quellcodedatei, und fügen Sie im Abschnitt `<dict>` Folgendes hinzu. Ersetzen Sie `[BUNDLE_ID]` durch den im Azure-Portal verwendeten Wert. Haben Sie den Code heruntergeladen, lautet dieser Wert `com.microsoft.identitysample.MSALiOS`. Wählen Sie bei der Erstellung eines eigenen Projekts Ihr Projekt in Xcode aus, und öffnen Sie die Registerkarte **General** (Allgemein). Die Paket-ID wird im Abschnitt **Identity** (Identität) angezeigt.

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
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

## <a name="create-your-apps-ui"></a>Erstellen der Benutzeroberfläche Ihrer App

Erstellen Sie nun eine Benutzeroberfläche mit einer Schaltfläche zum Aufrufen der Microsoft Graph-API, einer Schaltfläche zum Abmelden und einer Textansicht zum Anzeigen von Ausgaben. Fügen Sie dazu der Klasse `ViewController` den folgenden Code hinzu:

```swift
var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

Ersetzen Sie anschließend ebenfalls innerhalb der Klasse `ViewController` die Methode `viewDidLoad()` durch Folgendes:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>Verwendung von MSAL

### <a name="initialize-msal"></a>MSAL initialisieren

Fügen Sie der `ViewController`-Klasse die folgende `InitMSAL`-Methode hinzu:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-sign-in-callback"></a>Behandeln des Anmelderückrufs

Öffnen Sie die Datei `AppDelegate.swift` . Zur Verarbeitung des Rückrufs nach der Anmeldung fügen Sie `MSALPublicClientApplication.handleMSALResponse` wie folgt der Klasse `appDelegate` hinzu:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Abrufen von Token

Jetzt können Sie die Verarbeitungslogik der Anwendungsbenutzeroberfläche implementieren und Token interaktiv über MSAL beziehen.

MSAL macht zwei primäre Methoden zum Abrufen von Token verfügbar: `acquireTokenSilently()` und `acquireTokenInteractively()`: 

- `acquireTokenSilently()` versucht, einen Benutzer anzumelden und Token ohne Benutzerinteraktion abzurufen, wenn ein Konto vorhanden ist.

- `acquireTokenInteractively()` zeigt stets die Benutzeroberfläche an, wenn versucht wird, den Benutzer anzumelden. Möglicherweise werden jedoch Sitzungscookies im Browser oder ein Konto in Microsoft Authenticator verwendet, um eine interaktive Anmeldung per SSO bereitzustellen.

Fügen Sie der `ViewController`-Klasse den folgenden Code hinzu:

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Interaktives Abrufen eines Tokens

Der folgende Code ruft ein Token zum ersten Mal ab, indem ein `MSALInteractiveTokenParameters`-Objekt erstellt und `acquireToken` aufgerufen wird. Als Nächstes fügen Sie Code hinzu, der folgende Aktionen ausführt:

1. Erstellen von `MSALInteractiveTokenParameters` mit Bereichen
2. Aufrufen von `acquireToken()` mit den erstellten Parametern
3. Behandeln von Fehlern. Weitere Informationen finden Sie im [iOS-Leitfaden zur Fehlerbehandlung](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Verarbeiten der erfolgreichen Situation

Fügen Sie der `ViewController` -Klasse den folgenden Code hinzu.

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>Automatisches Abrufen eines Tokens

Fügen Sie der Klasse `ViewController` den folgenden Code hinzu, um automatisch ein aktualisiertes Token abzurufen. Ein `MSALSilentTokenParameters`-Objekt wird erstellt, und `acquireTokenSilent()` wird aufgerufen:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Aufrufen der Microsoft Graph-API 

Sobald Sie ein Token erhalten haben, kann Ihre App es im HTTP-Header verwenden, um eine autorisierte Anforderung an Microsoft Graph zu stellen:

| Headerschlüssel    | value                 |
| ------------- | --------------------- |
| Authorization | Bearer \<access-token> |

Fügen Sie der `ViewController`-Klasse den folgenden Code hinzu:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Weitere Informationen zur Microsoft Graph-API finden Sie [hier](https://graph.microsoft.com).

### <a name="use-msal-for-sign-out"></a>Verwenden von MSAL zur Abmeldung

Als Nächstes fügen Sie Unterstützung für die Abmeldung hinzu.

> [!Important]
> Die Abmeldung mit MSAL entfernt alle bekannten Informationen über einen Benutzer aus der Anwendung. Der Benutzer verfügt aber dennoch über eine aktive Sitzung auf seinem Gerät. Wenn der Benutzer versucht, sich erneut anzumelden, wird möglicherweise eine Benutzeroberfläche zum Anmelden angezeigt, die Anmeldeinformationen müssen jedoch möglicherweise nicht erneut eingegeben werden, da die Gerätesitzung noch aktiv ist.

Fügen Sie zum Hinzufügen einer Abmeldefunktion den folgenden Code in die `ViewController`-Klasse ein. Diese Methode durchläuft alle Konten und entfernt sie:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Aktivieren der Zwischenspeicherung von Token

MSAL speichert die Token Ihrer App standardmäßig im iOS-Schlüsselbund zwischen. 

So aktivieren Sie die Zwischenspeicherung von Token:
1. Rufen Sie Ihr Xcode-Projekt auf, und klicken Sie auf „Settings“ (Einstellungen) und dann auf die Registerkarte **Capabilities** (Funktionen) > **Enable Keychain Sharing** (Keychain-Freigabe aktivieren).
2. Klicken Sie auf **+** , und geben Sie `com.microsoft.adalcache` als Eintrag für **Keychain Groups** (Keychain-Gruppen) ein.

### <a name="add-helper-methods"></a>Hinzufügen von Hilfsmethoden

Fügen Sie die folgenden Hilfsmethoden zur Klasse `ViewController` hinzu, um das Beispiel abzuschließen:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>Anwendungen mit mehreren Konten

Diese App ist für ein Szenario mit einem einzelnen Konto konzipiert. MSAL unterstützt auch Szenarien mit mehreren Konten, in diesem Fall müssen die Apps jedoch einige zusätzliche Schritte ausführen. Sie müssen eine Benutzeroberfläche erstellen, um dem Benutzer bei der Auswahl des Kontos zu helfen, das er für die einzelnen Aktionen verwenden möchte, die Token erfordern. Als Alternative kann Ihre Anwendung eine Heuristik implementieren, um das zu verwendende Konto über die `getAccounts()`-Methode auszuwählen.

## <a name="test-your-app"></a>Testen Ihrer App

### <a name="run-locally"></a>Lokales Ausführen

Erstellen Sie die App, und stellen Sie sie auf einem Testgerät oder in einem Emulator bereit. Sie sollten sich anmelden und Token für Azure AD oder persönliche Microsoft-Konten abrufen können.

Wenn sich ein Benutzer zum ersten Mal bei Ihrer App anmeldet, wird er von Microsoft aufgefordert, den angeforderten Berechtigungen zuzustimmen.  Die meisten Benutzer können zustimmen, allerdings haben einige Azure AD-Mandanten die Zustimmung durch Benutzer deaktiviert, sodass Administratoren im Namen aller Benutzer zustimmen müssen. Zur Unterstützung dieses Szenarios müssen Sie die Bereiche Ihrer App im Azure-Portal registrieren.

Nach der Anmeldung zeigt die App die vom Microsoft Graph-Endpunkt `/me` zurückgegebenen Daten an.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Sollten Probleme mit diesem Tutorial oder mit Microsoft Identity Platform auftreten, besuchen Sie die Seite [Hilfe und Support](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options).

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)