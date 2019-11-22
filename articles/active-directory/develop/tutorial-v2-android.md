---
title: 'Erste Schritte mit Android: Microsoft Identity Platform | Azure'
description: So kann eine Android-App ein Zugriffstoken von Microsoft Identity Platform abrufen und die Microsoft Graph-API oder APIs aufrufen, die Zugriffstoken erfordern
services: active-directory
documentationcenter: dev-center-name
author: tylermsft
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/10/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5417c29b62414468064338a67b188c7b7f832ac5
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73242204"
---
# <a name="tutorial-sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Tutorial: Anmelden von Benutzern und Aufrufen von Microsoft Graph aus einer Android-App

> [!NOTE]
> Dieses Tutorial wurde noch nicht für die Version 1.0 der MSAL für Android aktualisiert. Es kann jedoch mit einer älteren Version verwendet werden, die wie in diesem Tutorial beschrieben konfiguriert ist.

In diesem Tutorial wird die Integration einer Android-App in Microsoft Identity Platform beschrieben. Ihre App meldet einen Benutzer an, ruft ein Zugriffstoken zum Aufrufen der Microsoft Graph-API ab und sendet eine Anforderung an die Microsoft Graph-API.  

> [!div class="checklist"]
> * Integrieren einer Android-App in Microsoft Identity Platform
> * Anmelden eines Benutzers
> * Abrufen eines Zugriffstokens zum Aufrufen der Microsoft Graph-API
> * Rufen Sie die Microsoft Graph-API auf.  

Am Ende dieses Tutorials akzeptiert Ihre Anwendung Anmeldungen von persönlichen Microsoft-Konten (outlook.com, live.com u. a.) sowie von Geschäfts,- Schul- oder Unikonten von allen Unternehmen oder Organisationen, die Azure Active Directory nutzen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="how-this-tutorial-works"></a>So funktioniert dieses Tutorial

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

Mit der App in diesem Tutorial werden Benutzer angemeldet und Daten in ihrem Namen abgerufen.  Auf diese Daten wird über eine geschützte API (Microsoft Graph-API) zugegriffen, für die eine Autorisierung erforderlich ist und die über Microsoft Identity Platform geschützt ist.

Dies gilt insbesondere in folgenden Fällen:

* Die App meldet den Benutzer entweder über einen Browser oder über Microsoft Authenticator und das Intune-Unternehmensportal an.
* Der Endbenutzer akzeptiert die von Ihrer Anwendung angeforderten Berechtigungen.
* Ihre App stellt ein Zugriffstoken für die Microsoft Graph-API aus.
* Das Zugriffstoken ist in der HTTP-Anforderung an die Web-API enthalten.
* Die Microsoft Graph-Antwort wird verarbeitet.

Dieses Beispiel verwendet die Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) für Android zum Implementieren der Authentifizierung: [com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal).

 MSAL erneuert Token automatisch, ermöglicht das einmalige Anmelden zwischen anderen Apps auf dem Gerät und verwaltet die Konten.

## <a name="prerequisites"></a>Voraussetzungen

* Für dieses Tutorial ist Android Studio Version 3.5 oder höher erforderlich.

## <a name="create-a-project"></a>Erstellen eines Projekts

In diesem Tutorial wird ein neues Projekt erstellt. Wenn Sie stattdessen das abgeschlossene Tutorial herunterladen möchten, [laden Sie den Code](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip) herunter.

1. Öffnen Sie Android Studio, und wählen Sie **Start a new Android Studio project** (Neues Android Studio-Projekt starten) aus.
2. Wählen Sie **Basic Activity** (Standardaktivität) aus, und klicken Sie dann auf **Next** (Weiter).
3. Benennen Sie Ihre Anwendung.
4. Speichern Sie den Paketnamen. Sie geben diesen später im Azure-Portal ein.
5. Ändern Sie die Sprache von **Kotlin** in **Java**.
6. Legen Sie **Minimum API level** (Mindestebene für API) auf **API 19** oder höher fest, und klicken Sie auf **Finish**.
7. Wählen Sie in der Projektansicht in der Dropdownliste die Option **Project** aus, um die Projektdateien mit und ohne Quelle anzuzeigen, öffnen Sie **app/build.gradle**, und legen Sie `targetSdkVersion` auf `28` fest.

## <a name="register-your-application"></a>Anwendung registrieren

1. Öffnen Sie das [Azure-Portal](https://aka.ms/MobileAppReg).
2. Öffnen Sie das Blatt [App-Registrierungen](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), und klicken Sie auf **+ Neue Registrierung**.
3. Geben Sie einen **Namen** für Ihre App ein, und klicken Sie auf **Registrieren**, ohne einen Umleitungs-URI festzulegen.
4. Wählen Sie im Abschnitt **Verwalten** des angezeigten Bereichs die Option **Authentifizierung** >  **+ Plattform hinzu** > **Android** aus. (Möglicherweise müssen Sie oben auf dem Blatt „Switch to the new experience“ (Zur neuen Benutzeroberfläche wechseln) auswählen, um diesen Abschnitt anzeigen zu können.)
5. Geben Sie den Paketnamen Ihres Projekts ein. Wenn Sie den Code heruntergeladen haben, lautet dieser `com.azuresamples.msalandroidapp`.
6. Klicken Sie im Abschnitt **Signaturhash** der Seite **Android-App konfigurieren** auf **Generieren eines Signaturhashs für die Entwicklung**, und kopieren Sie den KeyTool-Befehl, um ihn auf Ihrer Plattform zu verwenden.

   > [!Note]
   > „KeyTool. exe“ wird als Teil des Java Development Kit (JDK) installiert. Sie müssen auch das OpenSSL-Tool installieren, um den KeyTool-Befehl auszuführen.

7. Geben Sie den von KeyTool generierten **Signaturhash** ein.
8. Klicken Sie auf `Configure`, und speichern Sie die **MSAL-Konfiguration**, die auf der Seite **Android-Konfiguration** angezeigt wird, damit Sie diese später beim Konfigurieren Ihrer App eingeben können.  Klicken Sie auf **Fertig**.

## <a name="build-your-app"></a>Erstellen der App

### <a name="add-your-app-registration"></a>Hinzufügen der App-Registrierung

1. Navigieren Sie im Projektbereich von Android Studio zu **app\src\main\res**.
2. Klicken Sie mit der rechten Maustaste auf **res**, und wählen Sie **New** > **Directory** („Neu“ > „Verzeichnis“) aus. Geben Sie `raw` als neuen Verzeichnisnamen ein, und klicken Sie auf **OK**.
3. Erstellen Sie unter **app** > **src** > **main** > **res** > **raw** eine neue JSON-Datei namens `auth_config.json`, und fügen Sie die MSAL-Konfiguration ein, die Sie zuvor gespeichert haben. Weitere Informationen finden Sie unter [MSAL-Konfiguration](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
4. Fügen Sie in **app** > **src** > **main** > **AndroidManifest.xml** die folgende `BrowserTabActivity`-Aktivität zum Hauptteil der Anwendung hinzu. Dieser Eintrag ermöglicht Microsoft den Rückruf an Ihre Anwendung nach Abschluss der Authentifizierung:

    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
        </intent-filter>
    </activity>
    ```

    Ersetzen Sie den Wert `android:host=` durch den Paketnamen, den Sie im Azure-Portal registriert haben.
    Ersetzen Sie den Wert `android:path=` durch den Schlüsselhash, den Sie im Azure-Portal registriert haben. Der Signaturhash darf nicht als URL codiert sein.

5. Fügen Sie in der Datei **AndroidManifest.xml** direkt über dem Tag `<application>` die folgenden Berechtigungen hinzu:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

### <a name="create-the-apps-ui"></a>Erstellen der App-Benutzeroberfläche

1. Navigieren Sie im Android Studio-Projektfenster zu **app** > **src** > **main** > **res** > **layout**, und öffnen Sie **activity_main.xml** sowie die Ansicht **Text**.
2. Ändern Sie das Layout der Aktivität beispielsweise von `<androidx.coordinatorlayout.widget.CoordinatorLayout` zu `<androidx.coordinatorlayout.widget.DrawerLayout`. 
3. Fügen Sie die Eigenschaft `android:orientation="vertical"` zum Knoten `LinearLayout` hinzu.
4. Fügen Sie den folgenden Code in den Knoten `LinearLayout` ein, wobei Sie den aktuellen Inhalt ersetzen:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

### <a name="add-msal-to-your-project"></a>Hinzufügen von MSAL zu Ihrem Projekt

1. Navigieren Sie im Android Studio-Projektfenster zu **app** > **src** > **build.gradle**.
2. Fügen Sie unter **Dependencies** (Abhängigkeiten) Folgendes ein:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3+'
    ```

### <a name="use-msal"></a>Verwendung von MSAL

Nehmen Sie nun Änderungen in `MainActivity.java` vor, um die MSAL in Ihrer App hinzuzufügen und zu verwenden.
Navigieren Sie im Android Studio-Projektfenster zu **app** > **src** > **main** > **java** > **com.example.(Ihre App)** , und öffnen Sie `MainActivity.java`.

#### <a name="required-imports"></a>Erforderliche Importe

Fügen Sie die folgenden Importe am Anfang von `MainActivity.java` hinzu:

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
import com.microsoft.identity.client.exception.*;
```

#### <a name="instantiate-msal"></a>Instanziieren der MSAL

Innerhalb der Klasse `MainActivity` müssen wir MSAL zusammen mit einigen Konfigurationen über die Aufgaben der App instanziieren, einschließlich der Bereiche und der Web-API, auf die wir zugreifen möchten.

Kopieren Sie die folgenden Variablen in die Klasse `MainActivity`:

```java
final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

/* UI & Debugging Variables */
private static final String TAG = MainActivity.class.getSimpleName();
Button callGraphButton;
Button signOutButton;

/* Azure AD Variables */
private PublicClientApplication sampleApp;
private IAuthenticationResult authResult;
```

Ersetzen Sie den Inhalt von `onCreate()` durch den folgenden Code, um die MSAL zu instanziieren:

```java
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

callGraphButton = (Button) findViewById(R.id.callGraph);
signOutButton = (Button) findViewById(R.id.clearCache);

callGraphButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onCallGraphClicked();
    }
});

signOutButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onSignOutClicked();
    }
});

/* Configure your sample app and save state for this activity */
sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);

/* Attempt to get a user and acquireTokenSilent */
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {
        if (!accounts.isEmpty()) {
            /* This sample doesn't support multi-account scenarios, use the first account */
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts */
        }
    }
});
```

Der obige Code versucht, Benutzer, die Ihre Anwendung öffnen, im Hintergrund über `getAccounts()` und im Erfolgsfall über `acquireTokenSilentAsync()` anzumelden.  In den nächsten Abschnitten implementieren wir den Rückrufhandler für den Fall, dass keine angemeldeten Konten vorhanden sind.

#### <a name="use-msal-to-get-tokens"></a>Verwenden von MSAL zum Abrufen von Token

Jetzt können wir die Verarbeitungslogik der Benutzeroberfläche der App implementieren und Token interaktiv über MSAL beziehen.

MSAL macht zwei primäre Methoden zum Abrufen von Token verfügbar: `acquireTokenSilentAsync()` und `acquireToken()`.  

`acquireTokenSilentAsync()` meldet einen Benutzer an und ruft Token ohne Benutzerinteraktion ab, wenn ein Konto vorhanden ist. Wenn dies gelingt, MSAL die Token an Ihre App. Gelingt es nicht, wird ein `MsalUiRequiredException`-Element generiert.  Wenn diese Ausnahme generiert wird oder Sie möchten, dass Ihre Benutzer sich interaktiv anmelden (Anmeldeinformationen, MFA oder andere Richtlinien für bedingten Zugriff können möglicherweise erforderlich sein), verwenden Sie `acquireToken()`.  

`acquireToken()` zeigt die Benutzeroberfläche an, wenn versucht wird, einen Benutzer anzumelden und Token abzurufen. Möglicherweise werden jedoch Sitzungscookies im Browser oder ein Konto im Microsoft Authenticator verwendet, um die interaktive Anmeldung per SSO bereitzustellen.

Erstellen Sie die folgenden drei Benutzeroberflächenmethoden in der `MainActivity`-Klasse:

```java
/* Set the UI for successful token acquisition data */
private void updateSuccessUI() {
    callGraphButton.setVisibility(View.INVISIBLE);
    signOutButton.setVisibility(View.VISIBLE);
    findViewById(R.id.welcome).setVisibility(View.VISIBLE);
    ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
            authResult.getAccount().getUsername());
    findViewById(R.id.graphData).setVisibility(View.VISIBLE);
}

/* Set the UI for signed out account */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");

    Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
            .show();
}

/* Use MSAL to acquireToken for the end-user
 * Callback will call Graph api w/ access token & update UI
 */
private void onCallGraphClicked() {
    sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
}
```

Fügen Sie die folgenden Methoden hinzu, um die aktuelle Aktivität abzurufen und unbeaufsichtigte und interaktive Rückrufe zu verarbeiten:

```java
public Activity getActivity() {
    return this;
}

/* Callback used in for silent acquireToken calls.
 * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
 * else errors that we need to do an interactive request.
 */
private AuthenticationCallback getAuthSilentCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}

/* Callback used for interactive request.  If succeeds we use the access
 * token to call the Microsoft Graph. Does not check cache
 */
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");
            Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

#### <a name="use-msal-for-sign-out"></a>Verwenden von MSAL zur Abmeldung

Als Nächstes fügen Sie Unterstützung für die Abmeldung hinzu.

> [!Important]
> Die Abmeldung mit MSAL entfernt alle bekannten Informationen über einen Benutzer aus der Anwendung. Der Benutzer verfügt aber dennoch über eine aktive Sitzung auf seinem Gerät. Wenn der Benutzer versucht, sich erneut anzumelden, wird möglicherweise eine Benutzeroberfläche zum Anmelden angezeigt, die Anmeldeinformationen müssen jedoch möglicherweise nicht erneut eingegeben werden, da die Gerätesitzung noch aktiv ist.

Um eine Abmeldefunktion hinzuzufügen, fügen Sie die folgende Methode in die `MainActivity`-Klasse ein. Diese Methode durchläuft alle Konten und entfernt sie:

```java
/* Clears an account's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 * User will get interactive SSO if trying to sign back-in.
 */
private void onSignOutClicked() {
    /* Attempt to get a user and acquireTokenSilent
     * If this fails we do an interactive request
     */
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (accounts.isEmpty()) {
                /* No accounts to remove */

            } else {
                for (final IAccount account : accounts) {
                    sampleApp.removeAccount(
                            account,
                            new PublicClientApplication.AccountsRemovedCallback() {
                        @Override
                        public void onAccountsRemoved(Boolean isSuccess) {
                            if (isSuccess) {
                                /* successfully removed account */
                            } else {
                                /* failed to remove account */
                            }
                        }
                    });
                }
            }

            updateSignedOutUI();
        }
    });
}
```

#### <a name="call-the-microsoft-graph-api"></a>Aufrufen der Microsoft Graph-API

Sobald Sie ein Token empfangen haben, können Sie eine Anforderung an die [Microsoft Graph-API](https://graph.microsoft.com) senden. Das Zugriffstoken befindet sich im `AuthenticationResult` in der `onSuccess()`-Methode des Authentifizierungsrückrufs. Zum Erstellen einer autorisierten Anforderung muss Ihre App das Zugriffstoken dem HTTP-Header hinzufügen:

| Headerschlüssel    | value                 |
| ------------- | --------------------- |
| Authorization | Bearer \<access-token> |

Fügen Sie die folgenden beiden Methoden in der `MainActivity`-Klasse hinzu, um Graph aufzurufen und die Benutzeroberfläche zu aktualisieren:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```

#### <a name="multi-account-applications"></a>Anwendungen mit mehreren Konten

Diese App ist für ein Szenario mit einem einzelnen Konto konzipiert. MSAL unterstützt auch Szenarien mit mehreren Konten, in diesem Fall müssen die Apps jedoch einige zusätzliche Schritte ausführen. Sie müssen eine Benutzeroberfläche erstellen, um dem Benutzer bei der Auswahl des Kontos zu helfen, das er für die einzelnen Aktionen verwenden möchte, die Token erfordern. Als Alternative kann Ihre Anwendung eine Heuristik implementieren, um das zu verwendende Konto über die `getAccounts()`-Methode auszuwählen.

## <a name="test-your-app"></a>Testen Ihrer App

### <a name="run-locally"></a>Lokales Ausführen

Erstellen Sie die App, und stellen Sie sie auf einem Testgerät oder in einem Emulator bereit. Sie sollten sich anmelden und Token für Azure AD oder persönliche Microsoft-Konten abrufen können.

Nach der Anmeldung zeigt die App die vom Microsoft Graph `/me`-Endpunkt zurückgegebenen Daten an.

### <a name="consent"></a>Zustimmung

Wenn sich ein Benutzer zum ersten Mal bei Ihrer App anmeldet, wird er von Microsoft aufgefordert, den angeforderten Berechtigungen zuzustimmen.  Die meisten Benutzer können zustimmen, allerdings haben einige Azure AD-Mandanten die Zustimmung durch Benutzer deaktiviert, sodass Administratoren im Namen aller Benutzer zustimmen müssen. Zur Unterstützung dieses Szenarios müssen Sie die Bereiche Ihrer App im Azure-Portal registrieren.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie das im Schritt [Registrieren Ihrer Anwendung](#register-your-application) erstellte App-Objekt, wenn Sie es nicht mehr benötigen.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Sollten Probleme mit diesem Tutorial oder mit Microsoft Identity Platform auftreten, besuchen Sie die Seite [Hilfe und Support](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options).

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie eine kurze Umfrage mit zwei Fragen beantworten.

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
