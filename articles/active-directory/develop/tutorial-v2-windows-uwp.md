---
title: Erste Schritte mit der Microsoft Identity Platform UWP | Azure
description: Informationen, wie die Universelle Windows-Plattform-Anwendungen (UWP) eine API aufrufen können, die Zugriffstoken vom Microsoft Identity Platform-Endpunkt anfordert.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0cafc439a24c10c4c5a678219a0e0dce84476ff
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71290853"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Aufrufen der Microsoft Graph-API über eine UWP-Anwendung (XAML)

> [!div renderon="docs"]

Diese Anleitung veranschaulicht, wie eine native UWP-Anwendung (Universelle Windows-Plattform) ein Zugriffstoken anfordern kann. Anschließend ruft die Anwendung die Microsoft Graph-API auf. Die Anleitung bezieht sich außerdem auf andere APIs, die Zugriffstoken von einem Microsoft Identity Platform-Endpunkt anfordern.

Am Ende dieser Anleitung kann Ihre Anwendung eine geschützte API sowohl mit persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen aufrufen, die mit Azure Active Directory (Azure AD) arbeiten.

>[!NOTE]
> Für diesen Leitfaden ist Visual Studio mit einer installierten Entwicklung für die Universelle Windows-Plattform erforderlich. Im Artikel [Vorbereiten](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) finden Sie Anweisungen zum Herunterladen und Konfigurieren von Visual Studio für die Entwicklung der Apps der Universellen Windows-Plattform.

## <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Zeigt, wie die in diesem Tutorial generierte Beispiel-App funktioniert](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

In diesem Leitfaden wird eine UWP-Beispielanwendung erstellt, mit der die Microsoft Graph-API abgefragt wird. In diesem Szenario wird ein Token mit dem Autorisierungsheader HTTP-Anforderungen hinzugefügt. Tokenabrufe und -verlängerungen werden von der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) gehandhabt.

## <a name="nuget-packages"></a>NuGet-Pakete

In dieser Anleitung wird das folgende NuGet-Paket verwendet:

|Bibliothek|BESCHREIBUNG|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|

## <a name="set-up-your-project"></a>Einrichten des Projekts

Dieser Abschnitt enthält detaillierte Anweisungen zum Integrieren einer Windows Desktop .NET-Anwendung (XAML) in „Mit Microsoft anmelden“. Die Anwendung kann dann Web-APIs abfragen, für die ein Token erforderlich ist, z. B. die Microsoft Graph-API.

In diesem Leitfaden wird eine Anwendung mit einer Schaltfläche erstellt, über die die Graph-API abgefragt werden kann, und es ist auch eine Schaltfläche zum Abmelden vorhanden. Darüber hinaus werden Textfelder mit den Ergebnissen der Aufrufe angezeigt.

> [!NOTE]
> Möchten Sie lieber das Visual Studio-Projekt dieses Beispiels herunterladen, anstatt die Erstellung durchzuführen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip), und fahren Sie mit dem Schritt [Anwendungsregistrierung](#register-your-application "aAnwendungsregistrierung") fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

### <a name="create-your-application"></a>Erstellen der Anwendung

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.
1. Wählen Sie unter **Neues Projekt erstellen** die Option **Leere App (Universelles Windows)** für C# und dann **Weiter** aus.
1. Geben Sie der App unter **Neues Projekt konfigurieren** einen Namen, und wählen Sie **Erstellen** aus.
1. Wählen Sie unter **Neues UWP-Projekt (Universelle Windows-Plattform)** eine beliebige Version für **Ziel** und **Minimum** und anschließend **OK** aus.

   ![Mindestversion und Zielversion](./media/tutorial-v2-windows-uwp/select-uwp-target-minimum.png)

### <a name="add-microsoft-authentication-library-to-your-project"></a>Hinzufügen der MSAL (Microsoft Authentication Library) zu Ihrem Projekt

1. Klicken Sie in Visual Studio auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
1. Kopieren Sie folgenden Befehl, und fügen Sie ihn in das Fenster **Paket-Manager-Konsole** ein:

    ```powershell
    Install-Package Microsoft.Identity.Client
    ```

   > [!NOTE]
   > Der obige Befehl installiert die [Microsoft Authentication Library (MSAL)](https://aka.ms/msal-net). Die MSAL kümmert sich um das Erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken für den Zugriff auf APIs, die von Microsoft Identity Platform geschützt werden.

### <a name="create-your-applications-ui"></a>Erstellen der Benutzeroberfläche Ihrer Anwendung

Visual Studio erstellt als Teil Ihrer Projektvorlage die Datei *MainPage.xaml*. Öffnen Sie diese Datei, und ersetzen Sie den Knoten **Grid** Ihrer Anwendung durch den folgenden Code:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```

### <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Verwenden der MSAL zum Abrufen eines Tokens für die Microsoft Graph-API

Dieser Abschnitt zeigt, wie Sie über die MSAL ein Token für die Microsoft Graph-API abrufen können. Nehmen Sie Änderungen an der Datei *MainPage.xaml.cs* vor.

1. Fügen Sie in *MainPage.xaml.cs* die folgenden Verweise hinzu:

    ```csharp
    using Microsoft.Identity.Client;
    using System.Diagnostics;
    using System.Threading.Tasks;
    ```

1. Ersetzen Sie die `MainPage`-Klasse durch den folgenden Code:

    ```csharp
    public sealed partial class MainPage : Page
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - the content of Tenant with the information about the accounts allowed to sign in in your application:
        //   - for Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use organizations
        //   - for any Work or School accounts, or Microsoft personal account, use common
        //   - for Microsoft Personal account, use consumers
        private const string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";        

        public IPublicClientApplication PublicClientApp { get; } 

        public MainPage()
        {
          this.InitializeComponent();

          PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount)
                .WithLogging((level, message, containsPii) =>
                {
                    Debug.WriteLine($"MSAL: {level} {message} ");
                }, LogLevel.Warning, enablePiiLogging:false,enableDefaultPlatformLogging:true)
                .WithUseCorporateNetwork(true)
                .Build();
        }

        /// <summary>
        /// Call AcquireTokenInteractive - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
         AuthenticationResult authResult = null;
         ResultText.Text = string.Empty;
         TokenInfoText.Text = string.Empty;

         // It's good practice to not do work on the UI thread, so use ConfigureAwait(false) whenever possible.            
         IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false); 
         IAccount firstAccount = accounts.FirstOrDefault();

         try
         {
          authResult = await PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                                  .ExecuteAsync();
         }
         catch (MsalUiRequiredException ex)
         {
          // A MsalUiRequiredException happened on AcquireTokenSilent.
          // This indicates you need to call AcquireTokenInteractive to acquire a token
          System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

          try
          {
           authResult = await PublicClientApp.AcquireTokenInteractive(scopes)
                                                      .ExecuteAsync()
                                                      .ConfigureAwait(false);
           }
           catch (MsalException msalex)
           {
            await DisplayMessageAsync($"Error Acquiring Token:{System.Environment.NewLine}{msalex}");
           }
          }
          catch (Exception ex)
          {
           await DisplayMessageAsync($"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}");
           return;
          }

          if (authResult != null)
          {
           var content = await GetHttpContentWithToken(graphAPIEndpoint,
                                                       authResult.AccessToken).ConfigureAwait(false);

           // Go back to the UI thread to make changes to the UI
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
            ResultText.Text = content;
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
           });
          }
        }
    }
    ```

#### Interaktives Abrufen eines Benutzertokens<a name="more-information"></a>

Die `AcquireTokenInteractive`-Methode führt zum Öffnen eines Fensters, in dem Benutzer zum Anmelden aufgefordert werden. Bei Anwendungen müssen sich Benutzer in der Regel interaktiv anmelden, wenn sie zum ersten Mal auf eine geschützte Ressource zugreifen. Die Anmeldung kann auch erforderlich sein, wenn beim automatischen Vorgang zum Beziehen eines Tokens ein Fehler auftritt (beispielsweise bei einem abgelaufenen Kennwort des Benutzers).

#### <a name="get-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens

Die `AcquireTokenSilent`-Methode dient zum Verwalten des Abrufens und Erneuerns von Token ohne Benutzereingriff. Nachdem `AcquireTokenInteractive` zum ersten Mal ausgeführt und der Benutzer zum Eingeben seiner Anmeldeinformationen aufgefordert wurde, verwenden Sie die `AcquireTokenSilent`-Methode, um Token für spätere Aufrufe anzufordern. Mit dieser Methode werden Token im Hintergrund abgerufen. Die Zwischenspeicherung und Verlängerung des Tokens wird per MSAL durchgeführt.

Für die `AcquireTokenSilent`-Methode tritt schließlich ein Fehler auf. Gründe für Fehler sind beispielsweise ein Benutzer, der sich abgemeldet oder sein Kennwort auf einem anderen Gerät geändert hat. Wenn per MSAL erkannt wird, dass für das Problem eine interaktive Aktion erforderlich ist, wird eine `MsalUiRequiredException`-Ausnahme ausgelöst. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

* Ihre Anwendung ruft sofort `AcquireTokenInteractive` auf. Dieser Aufruf führt dazu, dass der Benutzer zum Anmelden aufgefordert wird. Dieser Ansatz wird normalerweise für Onlineanwendungen verwendet, in denen keine Offlineinhalte für den Benutzer verfügbar sind. Die in diesem Leitfaden generierte Beispielanwendung basiert auf diesem Muster. Sie können dies bei der ersten Ausführung des Beispiels in Aktion sehen.

   Da bisher noch kein Benutzer die Anwendung genutzt hat, enthält `accounts.FirstOrDefault()` einen NULL-Wert, und die Ausnahme `MsalUiRequiredException` wird ausgelöst.

   Der Code im Beispiel behandelt die Ausnahme dann durch das Aufrufen von `AcquireTokenInteractive`. Dieser Aufruf führt dazu, dass der Benutzer zum Anmelden aufgefordert wird.

* In Ihrer Anwendung erhalten Benutzer einen visuellen Hinweis, dass sie sich anmelden müssen, damit diese den richtigen Zeitpunkt für die Anmeldung auswählen können. Die Anwendung kann später noch einmal versuchen, `AcquireTokenSilent` auszuführen. Wählen Sie diesen Ansatz, wenn Benutzer andere Anwendungsfunktionen ohne Störungen nutzen können, z.B. wenn Offlineinhalte in der Anwendung verfügbar sind. In diesem Fall können Benutzer entscheiden, wann sie sich anmelden möchten. Die Anwendung kann versuchen, `AcquireTokenSilent` zu wiederholen, nachdem das Netzwerk vorübergehend nicht verfügbar war.

### <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

Fügen Sie der Datei *MainPage.xaml.cs* die folgende neue Methode hinzu:

   ```csharp
   /// <summary>
   /// Perform an HTTP GET request to a URL using an HTTP Authorization header
   /// </summary>
   /// <param name="url">The URL</param>
   /// <param name="token">The token</param>
   /// <returns>String containing the results of the GET operation</returns>
   public async Task<string> GetHttpContentWithToken(string url, string token)
   {
       var httpClient = new System.Net.Http.HttpClient();
       System.Net.Http.HttpResponseMessage response;
       try
       {
           var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
           // Add the token in Authorization header
           request.Headers.Authorization = 
             new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
           response = await httpClient.SendAsync(request);
           var content = await response.Content.ReadAsStringAsync();
           return content;
       }
       catch (Exception ex)
        {
           return ex.ToString();
       }
    }
   ```

 Diese Methode sendet eine `GET`-Anforderung über die Graph-API, indem ein `Authorization`-Header verwendet wird.

#### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In dieser Beispielanwendung dient die `GetHttpContentWithToken`-Methode zum Senden einer HTTP `GET`-Anforderung an eine geschützte Ressource, die ein Token erfordert, und zum anschließenden Zurückgeben des Inhalts an den Aufrufer. Diese Methode fügt das abgerufene Token in den **HTTP-Autorisierungsheader** ein. In diesem Beispiel ist die Ressource der Endpunkt **me** der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.

### <a name="add-a-method-to-sign-out-the-user"></a>Hinzufügen einer Methode zum Abmelden des Benutzers

Fügen Sie folgende Methode zur Datei *MainPage.xaml.cs* hinzu, um den Benutzer abzumelden:

   ```csharp
   /// <summary>
   /// Sign out the current user
   /// </summary>
   private async void SignOutButton_Click(object sender, RoutedEventArgs e)
   {
       IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false);
       IAccount firstAccount = accounts.FirstOrDefault();

       try
       {
           await PublicClientApp.RemoveAsync(firstAccount).ConfigureAwait(false);
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
               ResultText.Text = "User has signed out";
               this.CallGraphButton.Visibility = Visibility.Visible;
                   this.SignOutButton.Visibility = Visibility.Collapsed;
               });
           }
           catch (MsalException ex)
           {
               ResultText.Text = $"Error signing out user: {ex.Message}";
           }
       }
   ```

> [!NOTE]
> Bei MSAL.NET werden asynchrone Methoden zum Abrufen von Token oder Bearbeiten von Konten verwendet. Sie müssen dafür sorgen, dass UI-Aktionen im UI-Thread unterstützt werden. Dies ist der Grund für den `Dispatcher.RunAsync`-Aufruf und die Vorsichtsmaßnahmen zum Aufrufen von `ConfigureAwait(false)`.

#### Weitere Informationen zum Abmelden<a name="more-information-on-sign-out"></a>

Die `SignOutButton_Click`-Methode entfernt den Benutzer aus dem MSAL-Benutzercache. Dadurch wird die MSAL effektiv angewiesen, den aktuellen Benutzer zu vergessen, und eine zukünftige Anforderung zum Abrufen eines Tokens ist nur erfolgreich, wenn diese interaktiv ist.

Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, MSAL unterstützt Szenarien, in denen sich der Benutzer bei mehr als einem Konto anmelden kann. Ein Beispiel ist eine E-Mail-Anwendung, in der ein Benutzer mehrere Konten hat.

### <a name="display-basic-token-information"></a>Anzeigen grundlegender Informationen zum Token

Fügen Sie *MainPage.xaml.cs* die folgende Methode hinzu, um grundlegende Informationen zum Token anzuzeigen:

   ```csharp
   /// <summary>
   /// Display basic information contained in the token. Needs to be called from the UI thread.
   /// </summary>
   private void DisplayBasicTokenInfo(AuthenticationResult authResult)
   {
       TokenInfoText.Text = "";
       if (authResult != null)
       {
           TokenInfoText.Text += $"User Name: {authResult.Account.Username}" + Environment.NewLine;
           TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
       }
   }
   ```

#### Weitere Informationen<a name="more-information-1"></a>

Mit **OpenID Connect** abgerufene ID-Token enthalten auch eine kleine Teilmenge von Informationen zum Benutzer. `DisplayBasicTokenInfo` zeigt grundlegende Informationen im Token: Zu diesen Informationen gehören der Anzeigename und die ID des Benutzers. Darüber hinaus sind auch das Ablaufdatum des Tokens und die eigentliche Zeichenfolge, die das Zugriffstoken darstellt, angegeben. Wenn Sie mehrmals auf die Schaltfläche **Microsoft Graph-API aufrufen** klicken, sehen Sie, dass dasselbe Token für nachfolgende Anforderungen wiederverwendet wurde. Sie können auch feststellen, dass das Ablaufdatum verlängert wurde, wenn die MSAL entscheidet, dass es Zeit ist, das Token zu verlängern.

### <a name="display-message"></a>Meldung anzeigen

Fügen Sie der Datei *MainPage.xaml.cs* die folgende neue Methode hinzu:

   ```csharp
   /// <summary>
   /// Displays a message in the ResultText. Can be called from any thread.
   /// </summary>
   private async Task DisplayMessageAsync(string message)
   {
        await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
            () =>
            {
                ResultText.Text = message;
            });
        }
   ```

## <a name="register-your-application"></a>Anwendung registrieren

Nun müssen Sie Ihre Anwendung registrieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Azure Active Directory** > **App-Registrierungen**.
1. Wählen Sie **Neue Registrierung** aus. Geben Sie einen aussagekräftigen Anwendungsnamen ein, der den Benutzern der App angezeigt wird, z. B. *UWP-App-calling-MSGraph*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z. B. Skype, Xbox)** aus. Wählen Sie anschließend **Registrieren** aus, um fortzufahren.
1. Suchen Sie auf der Übersichtsseite nach dem Wert **Anwendungs-ID (Client)** , und kopieren Sie ihn. Wechseln Sie zurück zu Visual Studio, öffnen Sie die Datei *MainPage.xaml.cs*, und ersetzen Sie den Wert von `ClientId` durch diesen Wert.

Konfigurieren der Authentifizierung für Ihre Anwendung:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie in der Liste **Umleitungs-URIs** unter **TYP** die Option **Öffentlicher Client (Mobilgerät und Desktop)** aus, und geben Sie unter **UMLEITUNGS-URI** die Zeichenfolge `urn:ietf:wg:oauth:2.0:oob` ein.
1. Wählen Sie **Speichern** aus.

Konfigurieren Sie die API-Berechtigungen für Ihre Anwendung:

1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie **Berechtigung hinzufügen** aus, und stellen Sie anschließend sicher, dass Sie die Option **Microsoft-APIs** ausgewählt haben.
1. Wählen Sie **Microsoft Graph**.
1. Wählen Sie **Delegierte Berechtigungen** aus, suchen Sie nach *User.Read*, und stellen Sie sicher, dass **User.Read** ausgewählt ist.
1. Wählen Sie nach dem Vornehmen aller Änderungen die Option **Berechtigungen hinzufügen** aus, um sie zu speichern.

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Aktivieren einer integrierten Authentifizierung für Verbunddomänen (optional)

Über das Anwendungsmanifest müssen zusätzliche Funktionen aktiviert werden, um eine integrierte Windows-Authentifizierung bei der Verwendung mit einer Azure AD-Verbunddomäne zu aktivieren. Wechseln Sie in Visual Studio zurück zu Ihrer Anwendung.

1. Öffnen Sie *Package.appxmanifest*.
1. Wählen Sie **Funktionen**, und aktivieren Sie die folgenden Einstellungen:

   * **Unternehmensauthentifizierung**
   * **Private Netzwerke (Client und Server)**
   * **Freigegebene Benutzerzertifikate**

> [!IMPORTANT]
> Die [integrierte Windows-Authentifizierung](https://aka.ms/msal-net-iwa) ist standardmäßig nicht für dieses Beispiel konfiguriert. Für Anwendungen, die die Funktionen `Enterprise Authentication` oder `Shared User Certificates` anfordern, ist im Windows Store eine höhere Überprüfungsebene erforderlich. Außerdem möchten nicht alle Entwickler die Überprüfung auf der höheren Ebene durchführen. Aktivieren Sie diese Einstellung nur, wenn Sie die integrierte Windows-Authentifizierung mit einer Azure AD-Verbunddomäne benötigen.

## <a name="test-your-code"></a>Testen Ihres Codes

Drücken Sie zum Testen Ihrer Anwendung die F5-Taste, um das Projekt in Visual Studio auszuführen. Das Hauptfenster wird angezeigt:

![Benutzeroberfläche der Anwendung](./media/tutorial-v2-windows-uwp/testapp-ui-vs2019.png)

Wenn Sie Tests durchführen möchten, wählen Sie **Microsoft Graph-API aufrufen** aus. Verwenden Sie dann zum Anmelden ein Azure AD-Konto (Organisationskonto) oder ein Microsoft-Konto (z. B. live.com oder outlook.com). Wenn ein Benutzer diesen Schritt zum ersten Mal ausführt, wird in der Anwendung ein Fenster mit dem Hinweis angezeigt, dass sich der Benutzer anmelden soll.

### <a name="consent"></a>Zustimmung

Bei der allerersten Anmeldung in Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt: Wählen Sie **Ja** aus, um zum Zugriff explizit Ihre Zustimmung zu geben:

![Bildschirm für die Zugriffsgenehmigung](./media/tutorial-v2-windows-uwp/consentscreen-vs2019.png)

### <a name="expected-results"></a>Erwartete Ergebnisse

Es werden Benutzerprofilinformationen angezeigt, die vom Microsoft Graph-API-Aufruf auf dem Bildschirm **API-Aufrufergebnisse** zurückgegeben werden:

![Bildschirm „API-Aufrufergebnisse“](./media/tutorial-v2-windows-uwp/uwp-results-screen-vs2019.png)

Grundlegende Informationen zum Token, das über `AcquireTokenInteractive` oder `AcquireTokenSilent` abgerufen wurde, werden im Feld **Tokeninformationen** angezeigt:

|Eigenschaft  |Format  |BESCHREIBUNG |
|---------|---------|---------|
|`Username` |`user@domain.com` |Der zur Identifizierung des Benutzers verwendete Benutzername|
|`Token Expires` |`DateTime` |Die Uhrzeit, zu der das Token abläuft. MSAL verlängert die Ablauffrist, indem das Token bei Bedarf verlängert wird.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich `user.read`, um ein Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung hinzugefügt, die im Anwendungsregistrierungsportal registriert wird. Andere Microsoft Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern unter Umständen zusätzliche Bereiche. Die Microsoft Graph-API benötigt beispielsweise den Bereich `Calendars.Read`, um die Kalender des Benutzers aufzuführen.

Um auf die Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung `Calendars.Read` hinzufügen. Fügen Sie dem Aufruf `acquireTokenSilent` anschließend den Bereich `Calendars.Read` hinzu.

> [!NOTE]
> Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

## <a name="known-issues"></a>Bekannte Probleme

### <a name="issue-1"></a>Problem 1:

Wenn Sie sich in einer Azure AD-Verbunddomäne bei Ihrer Anwendung anmelden, tritt einer der folgenden Fehler auf:

* In der Anforderung wurde kein gültiges Clientzertifikat gefunden.
* Im Zertifikatspeicher des Benutzers wurden keine gültigen Zertifikate gefunden.
* Versuchen Sie erneut, und wählen Sie eine andere Authentifizierungsmethode aus.

Ursache: Unternehmens- und Zertifikatsfunktionen sind nicht aktiviert.

Lösung: Führen Sie die Schritte unter [Aktivieren einer integrierten Authentifizierung für Verbunddomänen (optional)](#enable-integrated-authentication-on-federated-domains-optional) aus.

### <a name="issue-2"></a>Problem 2:

Nach der Aktivierung von [Integrierte Authentifizierung für Verbunddomänen](#enable-integrated-authentication-on-federated-domains-optional) und dem Versuch, sich über Windows Hello auf einem Windows 10-Computer bei einer Umgebung mit konfigurierter mehrstufiger Authentifizierung anzumelden, wird die Liste mit den Zertifikaten angezeigt. Wenn Sie Ihre PIN verwenden möchten, wird das Fenster „PIN“ jedoch nie angezeigt.

Ursache: Bekannte Einschränkung des Webauthentifizierungsbrokers in UWP-Anwendungen, die unter Windows 10 Desktop ausgeführt werden (funktioniert unter Windows 10 Mobile).

Problemumgehung: Wählen Sie **Mit anderen Optionen anmelden** aus. Wählen Sie dann **Mit Benutzernamen und Kennwort anmelden** und **Kennwort angeben** aus, und durchlaufen Sie anschließend die Telefonauthentifizierung.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Helfen Sie uns, Microsoft Identity Platform zu verbessern. Teilen Sie uns Ihre Meinung mit, indem Sie an einer kurzen Umfrage mit zwei Fragen teilnehmen:

> [!div class="nextstepaction"]
> [Umfrage zu Microsoft Identity Platform](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
