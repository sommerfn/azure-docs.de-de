---
title: 'Schnellstart: Microsoft Identity Platform – Android | Azure'
description: Hier erfahren Sie, wie Android-Anwendungen eine API aufrufen können, für die Zugriffstoken vom Microsoft Identity Platform-Endpunkt erforderlich sind.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2019
ms.author: twhitney
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: e11e47952f70ce0cd212ca93eff1c38f2b3993a8
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71678052"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Schnellstart: Benutzeranmeldung und Aufrufen der Microsoft Graph-API aus einer Android-App

In diesem Schnellstart veranschaulicht ein Codebeispiel, wie eine Android-Anwendung persönliche, Geschäfts-, Schul- oder Unikonten anmelden, ein Zugriffstoken abrufen und die Microsoft Graph-API aufrufen kann.

![Screenshot der Beispiel-App](media/quickstart-v2-android/android-intro.svg)

> [!NOTE]
> **Voraussetzungen**
> * Android Studio 
> * Android 16+ erforderlich

## <a name="step-1-get-the-sample-app"></a>Schritt 1: Abrufen der Beispiel-App

[Klonen Sie den Code](https://github.com/Azure-Samples/ms-identity-android-java.git).

## <a name="step-2-register-your-application"></a>Schritt 2: Anwendung registrieren

Führen Sie die folgenden Schritte aus, um ein Anwendungsobjekt zu registrieren und die Registrierungsinformationen dieses Anwendungsobjekts manuell dem Beispielprojekt hinzuzufügen:

1. Öffnen Sie das [Azure-Portal](https://aka.ms/MobileAppReg).
1. Öffnen Sie das Blatt [App-Registrierungen](https://portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps), und klicken Sie auf **+ Neue Registrierung**.
1. Geben Sie einen **Namen** für Ihre App-Registrierung ein, und klicken Sie auf **Registrieren**, ohne einen Umleitungs-URI festzulegen.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Authentifizierung** >  **+ Plattform hinzufügen** > **Android** aus. (Möglicherweise müssen Sie oben im Blatt **Neue Benutzeroberfläche ausprobieren** auswählen, um diesen Bildschirm aufrufen zu können.)
1. Geben Sie den **Paketnamen** Ihres Projekts ein. Dieser lautet `com.azuresamples.msalandroidapp`.
1. Klicken Sie auf der Seite **Android-App konfigurieren** im Abschnitt **Signaturhash** auf **Es wird ein Signaturhash für die Entwicklung generiert**, und kopieren Sie den zu verwendenden KeyTool-Befehl für die Plattform, mit der Sie Ihre Android-App entwickeln.

   > [!Note]
   > „KeyTool. exe“ wird als Teil des Java Development Kit (JDK) installiert. Sie müssen auch das OpenSSL-Tool installieren, um den KeyTool-Befehl auszuführen.  Sie benötigen „keytool“ und das Verzeichnis „OpenSSL\bin“ im Pfad.

1. Führen Sie den aus dem Portal kopierten keytool-Befehl in einem Terminalfenster aus.
1. Geben Sie den generierten Signaturhash im Portal unter **Signaturhash** ein.
1. Klicken Sie auf `Configure`, und erstellen Sie eine Kopie der **MSAL-Konfiguration**. Im nächsten Schritt kopieren Sie sie und fügen sie in einer Konfigurationsdatei ein. Klicken Sie auf **Fertig**.

## <a name="step-3-add-your-app-registration"></a>Schritt 3: Hinzufügen der App-Registrierung

1. Öffnen Sie das Beispielprojekt in Android Studio.
1. Öffnen Sie unter **app** > **res** > **raw** die Datei **auth_config_multiple_account.json**.  Fügen Sie den Inhalt der MSAL-Konfiguration ein. Dadurch werden die Client-ID, die Mandanten-ID und der redirect_uri aus dem Portal hinzugefügt. Dies sieht in etwa wie folgt aus, jedoch mit den folgenden Werten für die Client-ID, die Mandanten-ID und den redirect_uri:

    ```json
    {
      "client_id" : "<your_client_id_here>",
      "authorization_user_agent" : "DEFAULT",
      "redirect_uri" : "<your_redirect_uri_here>",
      "account_mode" : "MULTIPLE",
      "broker_redirect_uri_registered": true,
      "authorities" : [
        {
          "type": "AAD",
          "audience": {
            "type": "AzureADandPersonalMicrosoftAccount",
            "tenant_id": "common"
          }
        }
      ]
    }
    ```

1. Öffnen Sie **app** > **res** > **raw**. Öffnen Sie die Datei **auth_config_single_account.json**, und fügen Sie den Inhalt der MSAL-Konfiguration ein. Dies sieht wie in der obigen Datei **auth_config_multiple_account.json** aus.
1. Suchen Sie in **app** > **manifests** > **AndroidManifest.xml** die `BrowserTabActivity`-Aktivität. Dieser Eintrag ermöglicht Microsoft den Rückruf an Ihre Anwendung nach Abschluss der Authentifizierung:

    ```xml
    ...
    <activity android:name="com.microsoft.identity.client.BrowserTabActivity">
                <intent-filter>
                    <action android:name="android.intent.action.VIEW" />
    
                    <category android:name="android.intent.category.DEFAULT" />
                    <category android:name="android.intent.category.BROWSABLE" />
    
                    <!--
                        Add in your scheme/host from registered redirect URI
                        note that the leading "/" is required for android:path
                        For Example:
                        <data
                            android:host="com.azuresamples.msalandroidapp"
                            android:path="/1wIqXSqBj7w+h11ZifsnqwgyKrY="
                            android:scheme="msauth" />
                    -->
    
                    <data
                        android:host="YOUR_PACKAGE_NAME - must be registered at https://aka.ms/MobileAppReg"
                        android:path="/YOUR_DECODED_SIGNATURE_HASH - must be registered at https://aka.ms/MobileAppReg"
                        android:scheme="msauth" />
                </intent-filter>
            </activity>
    ```
    
1. Ersetzen Sie den Paketnamen mit dem im Azure-Portal registrierten Wert für `android:host=`.  In diesem Fall ist dies `com.azuresamples.msalandroidapp`.

    > [!IMPORTANT]
    > Dem Wert für **android:path** **muss** das Zeichen „/“ vorangestellt sein. Andernfalls wird eine rote Linie unter dem Wert angezeigt, und die Beispiel-App wird nicht ausgeführt.
     
1. Ersetzen Sie den Schlüsselhash, den Sie durch Ausführen von keytool erhalten und im Azure-Portal eingegeben haben, durch den Wert `android:path=`. Der Signaturhash darf nicht als URL codiert sein.

## <a name="step-4-run-the-sample-app"></a>Schritt 4: Ausführen der Beispiel-App

Wählen Sie in der Dropdownliste **Verfügbare Geräte** von Android Studio Ihren Emulator und Ihr Gerät aus, und führen Sie die App aus.

Die Beispiel-App wird auf dem Bildschirm **Single Account Mode** (Einzelkontomodus) gestartet. Standardmäßig wird der Standardbereich **user.read** angegeben. Dieser wird beim Lesen Ihrer eigenen Profildaten während des Aufrufs der Microsoft Graph-API verwendet. Die URL für die Microsoft Graph-API wird standardmäßig bereitgestellt. Sie können beide Werte ggf. ändern.

![MSAL-Beispiel-App mit Nutzung durch einzelnes Konto und mehrere Konten](./media/quickstart-v2-android/quickstart-sample-app.png)

Verwenden Sie das App-Menü, um zwischen dem Einzelkontomodus und dem Modus für mehrere Konten zu wechseln.

Melden Sie sich im Einzelkontomodus mit einem Home- oder Geschäftskonto oder an:

1. Wählen Sie **Get graph data interactively** (Graph-Daten interaktiv abrufen) aus, um Benutzer nach ihren Anmeldeinformationen abzufragen. Die Ausgabe des Aufrufs der Microsoft Graph-API wird im Bildschirm unten angezeigt.
2. Wählen Sie nach der Anmeldung **Get graph data silently** (Graph-Daten im Hintergrund abrufen) aus, um die Microsoft Graph-API erneut aufzurufen, ohne dass die Anmeldeinformationen des Benutzers erneut abgefragt werden. Die Ausgabe des Aufrufs der Microsoft Graph-API wird im Bildschirm unten angezeigt.

Im Modus für mehrere Konten können Sie dieselben Schritte ausführen.  Außerdem können Sie das angemeldete Konto entfernen. Dadurch werden auch die zwischengespeicherten Token für dieses Konto entfernt.

## <a name="how-the-sample-works"></a>Funktionsweise des Beispiels

Der Code ist in Fragmente unterteilt, in denen veranschaulicht wird, wie Sie eine MSAL-App für ein einzelnes Konto und für mehrere Konten schreiben. Die Codedateien sind wie folgt organisiert:

| Datei  | Zeigt  |
|---------|---------|
| MainActivity | Verwaltet die Benutzeroberfläche |
| MSGraphRequestWrapper  | Ruft die Microsoft Graph-API mit dem von MSAL bereitgestellten Token auf |
| MultipleAccountModeFragment  | Initialisiert eine Anwendung für mehrere Konten, lädt ein Benutzerkonto und ruft ein Token für den Aufruf der Microsoft Graph-API ab |
| SingleAccountModeFragment | Initialisiert eine Anwendung für ein einzelnes Konto, lädt ein Benutzerkonto und ruft ein Token für den Aufruf der Microsoft Graph-API ab |
| res/auth_config_multiple_account.json  | Die Konfigurationsdatei für mehrere Konten |
| res/auth_config_single_account.json  | Die Konfigurationsdatei für ein Konto |
| Gradle Scripts/build.grade (Module:app) | Die Abhängigkeiten der MSAL-Bibliothek werden hier hinzugefügt |

Diese Dateien werden nun ausführlicher betrachtet, und der MSAL-spezifische Code in jeder wird hervorgehoben.

### <a name="add-msal-to-the-app"></a>Hinzufügen von MSAL zur App

MSAL ([com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) ist die Bibliothek zum Anmelden von Benutzern und Anfordern von Token, die für den Zugriff auf eine durch Microsoft Identity Platform geschützte API verwendet wird. Gradle 3.0+ installiert die Bibliothek, wenn **Gradle Scripts** > **build.gradle (Module: app)** unter **Dependencies** folgenden Code hinzufügen:

```gradle  
implementation 'com.microsoft.identity.client:msal:1.0.0'
```

Dies wird im Beispielprojekt in build.gradle (Module: app) veranschaulicht:

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.0.0-RC7'
    ...
}
```

Dadurch wird Gradle angewiesen, MSAL von Maven Central herunterzuladen und zu kompilieren.

### <a name="msal-imports"></a>MSAL-Importe

Die relevanten Importe für die MSAL-Bibliothek sind `com.microsoft.identity.client.*`.  So wird beispielsweise `import com.microsoft.identity.client.PublicClientApplication;` angezeigt. Dies ist der Namespace für die `PublicClientApplication`-Klasse, die Ihre öffentliche Clientanwendung darstellt.

### <a name="singleaccountmodefragmentjava"></a>SingleAccountModeFragment.java

Diese Datei veranschaulicht, wie Sie eine Einzelkonto-MSAL-App erstellen und eine Microsoft Graph-API aufrufen.

Einzelkonto-Apps werden nur von einem einzigen Benutzer verwendet.  Sie verfügen beispielsweise nur über ein Konto, mit dem Sie sich bei ihrer Mapping-App anmelden.

#### <a name="single-account-msal-initialization"></a>Einzelkonto-MSAL-Initialisierung

In `onCreateView()` wird eine Einzelkonto-`PublicClientApplication` mit den in der Datei `auth_config_single_account.json` gespeicherten Konfigurationsinformationen erstellt.  So initialisieren Sie die MSAL-Bibliothek für die Verwendung in einer Einzelkonto-MSAL-App:

```java
...
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createSingleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_single_account,
        new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(ISingleAccountPublicClientApplication application) {
                /**
                    * This test app assumes that the app is only going to support one account.
                    * This requires "account_mode" : "SINGLE" in the config json file.
                    **/
                mSingleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                displayError(exception);
            }
        });
```

#### <a name="sign-in-a-user"></a>Anmelden eines Benutzers

Der Code zum Anmelden eines Benutzers befindet sich in `initializeUI()` im `signInButton`-Click-Handler.

Rufen Sie `signIn()` auf, bevor Sie versuchen, Token abzurufen. `signIn()` verhält sich wie bei einem Aufruf von `acquireToken()`. Dadurch wird eine interaktive Anmeldeaufforderung für den Benutzer erhalten.

Das Anmelden eines Benutzers ist ein asynchroner Vorgang. Es wird ein Rückruf übergeben, der die Microsoft Graph-API aufruft und bei Anmeldung des Benutzers die Benutzeroberfläche aktualisiert:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Abmelden eines Benutzers

Der Code zum Abmelden eines Benutzers befindet sich in `initializeUI()`, im `signOutButton`-Click-Handler.  Das Abmelden eines Benutzers ist ein asynchroner Vorgang. Durch das Abmelden des Benutzers wird auch der Tokencache für das betreffende Konto gelöscht. Es wird ein Rückruf erstellt, mit dem die Benutzeroberfläche aktualisiert wird, sobald das Benutzerkonto abgemeldet wurde:

```java
mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
    @Override
    public void onSignOut() {
        updateUI(null);
        performOperationOnSignOut();
    }

    @Override
    public void onError(@NonNull MsalException exception) {
        displayError(exception);
    }
});
```

#### <a name="get-a-token-interactively-or-silently"></a>Interaktives Abrufen eines Tokens oder Abrufen eines Tokens im Hintergrund

Um dem Benutzer eine möglichst geringe Anzahl von Eingabeaufforderungen zu präsentieren, wird ein Token im Allgemeinen im Hintergrund abgerufen. Sollte dabei ein Fehler auftreten, wird versucht, das Token interaktiv abzurufen. Der erste Aufruf von `signIn()` durch die App fungiert als Aufruf von `acquireToken()`, durch den der Benutzer nach Anmeldeinformationen abgefragt wird.

Es gibt einige Situationen, in denen der Benutzer aufgefordert werden kann, sein Konto auszuwählen, seine Anmeldeinformationen einzugeben oder den von der App angeforderten Berechtigungen zuzustimmen:

* Wenn sich der Benutzer erstmalig bei der Anwendung anmeldet
* Wenn ein Benutzer sein Kennwort zurücksetzt, muss er seine Anmeldeinformationen eingeben.
* Wenn die Zustimmung widerrufen wurde
* Wenn die App explizit eine Zustimmung erfordert
* Wenn Ihre Anwendung zum ersten Mal Zugriff auf eine Ressource anfordert
* Wenn MFA oder andere Richtlinien für bedingten Zugriff erforderlich sind

Der Code zum interaktiven Abrufen eines Tokens (d. h. über die Benutzeroberfläche, in welcher der Benutzer Aktionen ausführen muss) befindet sich in `initializeUI()`, im `callGraphApiInteractiveButton`-Click-Handler:

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Wenn sich der Benutzer bereits angemeldet hat, gestattet `acquireTokenSilentAsync()` Apps das Anfordern von Token im Hintergrund. Dies wird in `initializeUI()` im `callGraphApiSilentButton`-Click-Handler gezeigt:

```java
/**
  * Once you've signed the user in,
  * you can perform acquireTokenSilent to obtain resources without interrupting the user.
  */
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Laden eines Kontos

Der Code zum Laden eines Kontos befindet sich in `loadAccount()`.  Das Laden des Benutzerkontos ist ein asynchroner Vorgang, daher werden Rückrufe für die Verarbeitung beim Laden oder Wechseln des Kontos oder bei auftretenden Fehlern an MSAL übergeben.  Im folgenden Code wird außerdem `onAccountChanged()` behandelt; dies tritt auf, wenn ein Konto entfernt wird, der Benutzer zu einem anderen Konto wechselt usw.

```java
private void loadAccount() {
    ...

    mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback() {
        @Override
        public void onAccountLoaded(@Nullable IAccount activeAccount) {
            // You can use the account data to update your UI or your app database.
            updateUI(activeAccount);
        }

        @Override
        public void onAccountChanged(@Nullable IAccount priorAccount, @Nullable IAccount currentAccount) {
            if (currentAccount == null) {
                // Perform a cleanup task as the signed-in account changed.
                performOperationOnSignOut();
            }
        }

        @Override
        public void onError(@NonNull MsalException exception) {
            displayError(exception);
        }
    });
```

#### <a name="call-microsoft-graph"></a>Aufrufen von Microsoft Graph

Wenn ein Benutzer angemeldet ist, wird der Aufruf von Microsoft Graph über eine HTTP-Anforderung durch `callGraphAPI()` ausgeführt. Diese Funktion ist ein Wrapper. Dieser vereinfacht das Beispiel, indem einige Aufgaben wie Abrufen des Zugriffstokens vom `authenticationResult`, Verpacken des Aufrufs in den MSGraphRequestWrapper und Anzeigen der Ergebnisse des Aufrufs ausgeführt werden.

```java
private void callGraphAPI(final IAuthenticationResult authenticationResult) {
    MSGraphRequestWrapper.callGraphAPIUsingVolley(
            getContext(),
            graphResourceTextView.getText().toString(),
            authenticationResult.getAccessToken(),
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    /* Successfully called graph, process data and send to UI */
                    ...
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    ...
                }
            });
}
```

### <a name="auth_config_single_accountjson"></a>auth_config_single_account.json

Dies ist die Konfigurationsdatei für eine MSAL-App, die ein einziges Konto verwendet.

Eine Erläuterung dieser Felder finden Sie unter [Android-MSAL-Konfigurationsdatei](msal-configuration.md).

Beachten Sie, dass `"account_mode" : "SINGLE"` vorhanden ist; hierdurch wird diese App für die Verwendung eines einzigen Kontos konfigurieren.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

### <a name="multipleaccountmodefragmentjava"></a>MultipleAccountModeFragment.java

Diese Datei veranschaulicht, wie Sie eine MSAL-App für mehrere Konten erstellen und eine Microsoft Graph-API aufrufen. 

Ein Beispiel für eine App mit mehreren Konten ist eine E-Mail-App, die Ihnen das Arbeiten mit mehreren Benutzerkonten ermöglich (z. B. ein Geschäftskonto und ein privates Konto).

#### <a name="multiple-account-msal-initialization"></a>MSAL-Initialisierung für mehrere Konten

In `onCreateView()` wird ein App-Objekt für mehrere Konten(`IMultipleAccountPublicClientApplication`) mit den in der Datei `auth_config_multiple_account.json file` gespeicherten Konfigurationsinformationen erstellt:

```java
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

Das erstellte `MultipleAccountPublicClientApplication`-Objekt wird in einer Klassenmember-Variablen gespeichert, sodass es für die Interaktion mit der MSAL-Bibliothek verwendet werden kann, um Token abzurufen und das Benutzerkonto zu laden und zu entfernen.

#### <a name="load-an-account"></a>Laden eines Kontos

Apps für mehrere Konten rufen im Allgemeinen `GetAccounts()` auf, um das gewünschte Konto für MSAL-Vorgänge auszuwählen. Der Code zum Laden eines Kontos befindet sich in `loadAccount()`.  Das Laden des Benutzerkontos ist ein asynchroner Vorgang. Daher werden die Situationen, in denen ein Konto geladen oder gewechselt oder ein Fehler auftritt, von einem Rückruf behandelt.

```java
/**
    * Load the currently signed-in account, if there's any.
    * In the shared device mode, if the user is signed out from the device, the app can also perform the clean-up work in onAccountChanged().
    */
private void loadAccount() {
    ...
    mMultipleAccountApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            // You can use the account data to update your UI or your app database.
            accountList = result;
            updateUI(accountList);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

#### <a name="get-a-token-interactively-or-silently"></a>Interaktives Abrufen eines Tokens oder Abrufen eines Tokens im Hintergrund

Es gibt einige Situationen, in denen der Benutzer aufgefordert werden kann, sein Konto auszuwählen, seine Anmeldeinformationen einzugeben oder den von der App angeforderten Berechtigungen zuzustimmen:

* Erstmaliges Anmelden von Benutzern bei der Anwendung.
* Wenn ein Benutzer sein Kennwort zurücksetzt, muss er seine Anmeldeinformationen eingeben. 
* Wenn die Zustimmung widerrufen wurde 
* Wenn die App explizit eine Zustimmung erfordert 
* Wenn Ihre Anwendung zum ersten Mal Zugriff auf eine Ressource anfordert
* Wenn MFA oder andere Richtlinien für bedingten Zugriff erforderlich sind

Apps für mehrere Konten rufen Token im Allgemeinen interaktiv (d. h. der Benutzer muss über die Benutzeroberfläche Aktionen ausführen) mit einem Aufruf von `acquireToken()` ab.  Der Code zum interaktiven Abrufen eines Tokens befindet sich in `initializeUI()`, im `callGraphApiInteractiveButton`-Click-Handler:

```java
/**
 * Acquire token interactively. It will also create an account object for the silent call as a result (to be obtained by getAccount()).
 *
 * If acquireTokenSilent() returns an error that requires an interaction,
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your SSO refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 */
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Apps sollten nicht fordern, dass sich die Benutzer jedes Mal anmelden, wenn sie ein Token anfordern. Wenn sich der Benutzer bereits angemeldet hat, gestattet `acquireTokenSilentAsync()` Apps das Anfordern von Token ohne Eingabeaufforderung für den Benutzer. Dies wird in `initializeUI()` im `callGraphApiSilentButton`-Click-Handler gezeigt:

```java
/**
 * Performs acquireToken without interrupting the user.
 *
 * This requires an account object of the account you're obtaining a token for.
 * (can be obtained via getAccount()).
 */
mMultipleAccountApp.acquireTokenSilentAsync(getScopes(),
accountList.get(accountListSpinner.getSelectedItemPosition()),
AUTHORITY,
getAuthSilentCallback());
```

#### <a name="remove-an-account"></a>Entfernen eines Kontos

Der Code zum Entfernen eines Kontos und aller zwischengespeicherten Token für das Konto befindet sich in `initializeUI()` im Händler für die Schaltfläche zum Entfernen eines Kontos. Bevor Sie ein Konto entfernen können, benötigen Sie ein Kontoobjekt, das Sie von MSAL-Funktionen wie `getAccounts()` und `acquireToken()` abrufen können. Da das Entfernen eines Kontos ein asynchroner Vorgang ist, wird der `onRemoved`-Rückruf zum Aktualisieren der Benutzeroberfläche bereitgestellt.

```java
/**
  * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
  */
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccount();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>auth_config_multiple_account.json

Dies ist die Konfigurationsdatei für eine MSAL-App, die mehrere Konten verwendet.

Eine Erläuterung dieser Felder finden Sie unter [Android-MSAL-Konfigurationsdatei](msal-configuration.md).

Im Unterschied zur Konfigurationsdatei [auth_config_single_account.json](#auth_config_single_accountjson) enthält diese Konfigurationsdatei `"account_mode" : "MULTIPLE"` anstelle von `"account_mode" : "SINGLE"`, da es sich um eine App für mehrere Konten handelt.

```json
{
  "client_id" : "<your_client_id_here>",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "<your_redirect_uri_here>",
  "account_mode" : "MULTIPLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Informieren Sie sich über die Schritte zum Erstellen der in diesem Schnellstart verwendeten Anwendung.

Probieren Sie das Android-Tutorial aus, um eine vollständige Schritt-für-Schritt-Anleitung zum Erstellen von Anwendungen und neuen Features zu erhalten, einschließlich einer vollständigen Erläuterung dieses Schnellstarts.

> [!div class="nextstepaction"]
> [Tutorial: Aufrufen der Graph-API (Android)](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Wiki: MSAL für Android-Bibliothek

Erfahren Sie mehr über die MSAL-Bibliothek für Android:

> [!div class="nextstepaction"]
> [Wiki: MSAL für Android-Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
