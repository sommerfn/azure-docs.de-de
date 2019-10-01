---
title: Senden von Benachrichtigungen an bestimmte Geräte (Universelle Windows-Plattform) | Microsoft-Dokumentation
description: Verwenden Sie Azure Notification Hubs mit Tags in der Registrierung, um aktuelle Nachrichten an eine UWP-App zu senden.
services: notification-hubs
documentationcenter: windows
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 03/22/2019
ms.openlocfilehash: efe668e42e04942cc0d9fc99670057ab5bdd302a
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212129"
---
# <a name="tutorial-push-notifications-to-specific-windows-devices-running-universal-windows-platform-applications"></a>Tutorial: Senden von Pushbenachrichtigungen an bestimmte Windows-Geräte, die Anwendungen der universellen Windows-Plattform ausführen

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Übersicht

In diesem Tutorial wird gezeigt, wie Sie mit Azure Notification Hubs Benachrichtigungen zu aktuellen Nachrichten an Windows Store- oder Windows Phone 8.1-Anwendungen (kein Silverlight) senden können. Wenn Ihr Ziel Windows Phone 8.1 Silverlight ist, lesen Sie den Abschnitt zur [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md)-Version.

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an bestimmte Windows-Geräte senden, die Anwendungen der universellen Windows-Plattform (UWP) ausführen. Nachdem Sie dieses Tutorial abgeschlossen haben, können Sie die Registrierung für Kategorien aktueller Nachrichten, an denen Sie interessiert sind, durchführen. Sie erhalten dann nur für diese Kategorien Pushbenachrichtigungen.

Übertragungsszenarien werden durch das Einfügen von einem oder mehreren *Tags* möglich, wenn Sie eine Registrierung im Notification Hub erstellen. Wenn Benachrichtigungen zu einem Tag gesendet werden, erhalten alle Geräte, die für das Tag registriert wurden, diese Benachrichtigung. Weitere Informationen zu Tags finden Sie unter [Weiterleitung und Tagausdrücke](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store- und Windows Phone-Projekte der Version 8.1 und älter werden in Visual Studio 2017 nicht unterstützt. Weitere Informationen finden Sie unter [Visual Studio 2017 – Zielplattformen und Kompatibilität](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Hinzufügen der Kategorieauswahl zur mobilen App
> * Registrieren für Benachrichtigungen
> * Senden von Benachrichtigungen mit Tags
> * Ausführen der App und Erzeugen von Benachrichtigungen

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie [Tutorial: Senden von Benachrichtigungen an Apps für die universelle Windows-Plattform mit Azure Notification Hubs][get-started], bevor Sie mit diesem Tutorial beginnen.  

## <a name="add-category-selection-to-the-app"></a>Hinzufügen der Kategorieauswahl zur App

Der erste Schritt besteht darin, der vorhandenen Hauptseite Benutzeroberflächenelemente hinzuzufügen, die dem Benutzer die Auswahl der Kategorien für die Registrierung ermöglichen. Die ausgewählten Kategorien werden auf dem Gerät gespeichert. Wenn die App gestartet wird, wird eine Geräteregistrierung in Ihrem Notification Hub mit den ausgewählten Kategorien als Tags erstellt.

1. Öffnen Sie die Projektdatei „MainPage.xaml“, und kopieren Sie folgenden Code in das `Grid`-Element:

    ```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
        <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
    </Grid>
    ```

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und fügen Sie eine neue Klasse hinzu: **Notifications**. Fügen Sie der Klassendefinition den Modifizierer **public** und dann der neuen Codedatei die folgenden `using`-Anweisungen hinzu:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.Storage;
    using System.Threading.Tasks;
    ```

3. Kopieren Sie den folgenden Code in die neue `Notifications`-Klasse:

    ```csharp
    private NotificationHub hub;

    public Notifications(string hubName, string listenConnectionString)
    {
        hub = new NotificationHub(hubName, listenConnectionString);
    }

    public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
    {
        ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
        return await SubscribeToCategories(categories);
    }

    public IEnumerable<string> RetrieveCategories()
    {
        var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
        return categories != null ? categories.Split(','): new string[0];
    }

    public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        if (categories == null)
        {
            categories = RetrieveCategories();
        }

        // Using a template registration to support notifications across platforms.
        // Any template notifications that contain messageParam and a corresponding tag expression
        // will be delivered for this registration.

        const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                categories);
    }
    ```

    Diese Klasse verwendet den lokalen Speicher, um Nachrichtenkategorien zu speichern, die das Gerät empfangen soll. Anstelle der `RegisterNativeAsync`-Methode rufen Sie `RegisterTemplateAsync` auf, um die Registrierung für die Kategorien mithilfe einer Vorlagenregistrierung durchzuführen.

    Wenn Sie mehrere Vorlagen (z.B. eine Popupbenachrichtigung und eine für Kacheln) registrieren möchten, geben Sie einen Vorlagennamen an (z.B. „simpleWNSTemplateExample“). Benennen Sie die Vorlagen so, dass Sie sie später aktualisieren oder löschen können.

    >[!NOTE]
    >Wenn ein Gerät mehrere Vorlagen mit demselben Tag registriert, werden bei einer eingehenden Nachricht für das entsprechende Tag mehrere Benachrichtigungen an das Gerät verschickt (eine pro Vorlage). Dies ist hilfreich, um dieselbe logische Nachricht in mehreren visuellen Darstellungen anzuzeigen (z.B. als Signal und als Popupbenachrichtigung in einer Windows Store-Anwendung).

    Weitere Informationen finden Sie unter [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md).

4. Fügen Sie in der Projektdatei „App.xaml.cs“ der Klasse `App` die folgende Eigenschaft hinzu:

    ```csharp
    public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
    ```

    Mit dieser Eigenschaft erstellen Sie eine `Notifications`-Instanz und greifen darauf zu.

    Ersetzen Sie im Code die Platzhalter `<hub name>` und `<connection string with listen access>` durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature*, die Sie zuvor abgerufen haben.

   > [!NOTE]
   > Da Anmeldenamen, die mit einer Client-App verteilt werden, normalerweise nicht sehr sicher sind, sollten Sie nur den Schlüssel für den *Abhörzugriff* mit Ihrer Client-App verteilen. Der Abhörzugriff ermöglicht der App, sich für Benachrichtigungen zu registrieren, aber es können keine vorhandenen Registrierungen geändert und keine Benachrichtigungen versendet werden. Der Vollzugriffsschlüssel wird in einem gesicherten Back-End-Dienst für das Versenden von Benachrichtigungen und das Ändern vorhandener Registrierungen verwendet.

5. Fügen Sie in der Datei `MainPage.xaml.cs` die folgende Zeile hinzu:

    ```csharp
    using Windows.UI.Popups;
    ```

6. Fügen Sie in der Datei `MainPage.xaml.cs` die folgende Methode hinzu:

    ```csharp
    private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
    {
        var categories = new HashSet<string>();
        if (WorldToggle.IsOn) categories.Add("World");
        if (PoliticsToggle.IsOn) categories.Add("Politics");
        if (BusinessToggle.IsOn) categories.Add("Business");
        if (TechnologyToggle.IsOn) categories.Add("Technology");
        if (ScienceToggle.IsOn) categories.Add("Science");
        if (SportsToggle.IsOn) categories.Add("Sports");

        var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

        var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    ```

    Diese Methode erstellt eine Liste von Kategorien und verwendet die `Notifications`-Klasse zum Speichern der Liste im lokalen Speicher. Außerdem registriert sie die entsprechenden Tags in Ihrem Notifications Hub. Wenn die Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt.

Ihre App kann jetzt einen Satz von Kategorien im lokalen Speicher auf dem Gerät speichern. Die App registriert sich beim Notification Hub, wenn Benutzer die Kategorieauswahl ändern.

## <a name="register-for-notifications"></a>Registrieren für Benachrichtigungen

In diesem Abschnitt führen Sie die Registrierung beim Notification Hub während des Startvorgangs durch, wobei die im lokalen Speicher gespeicherten Kategorien verwendet werden.

> [!NOTE]
> Da sich der durch den Windows Notification Service (WNS) zugeteilte Kanal-URI jederzeit ändern kann, sollten Sie sich regelmäßig für Benachrichtigungen registrieren, um Benachrichtigungsfehler zu vermeiden. Dieses Beispiel registriert sich jedes Mal für Benachrichtigungen, wenn die App gestartet wird. Für häufig ausgeführte Apps (öfters als einmal täglich) können Sie die Registrierung wahrscheinlich überspringen, wenn weniger als ein Tag seit der letzten Registrierung vergangen ist, um Bandbreite einzusparen.

1. Öffnen Sie die Datei „App.xaml.cs“, und aktualisieren Sie die `notifications`-Methode, sodass zum Abonnieren von Kategorien die `InitNotificationsAsync`-Klasse verwendet wird.

    ```csharp
    // *** Remove or comment out these lines ***
    //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    //var hub = new NotificationHub("your hub name", "your listen connection string");
    //var result = await hub.RegisterNativeAsync(channel.Uri);

    var result = await notifications.SubscribeToCategories();
    ```

    Dadurch wird sichergestellt, dass beim Start der App die Kategorien aus dem lokalen Speicher abgerufen werden und eine Registrierung für diese Kategorien angefordert wird. Die `InitNotificationsAsync`-Methode wurde im Rahmen des Tutorials [Erste Schritte mit Notification Hubs][get-started] erstellt.
2. Fügen Sie in der Projektdatei `MainPage.xaml.cs` der Methode `OnNavigatedTo` den folgenden Code hinzu:

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        var categories = ((App)Application.Current).notifications.RetrieveCategories();

        if (categories.Contains("World")) WorldToggle.IsOn = true;
        if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
        if (categories.Contains("Business")) BusinessToggle.IsOn = true;
        if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
        if (categories.Contains("Science")) ScienceToggle.IsOn = true;
        if (categories.Contains("Sports")) SportsToggle.IsOn = true;
    }
    ```

    Durch diesen Code wird die Hauptseite basierend auf dem Status der zuvor gespeicherten Kategorien aktualisiert.

Die App ist nun vollständig. Sie kann einen Satz von Kategorien im lokalen Speicher des Geräts speichern, der für die Registrierung beim Notification Hub verwendet wird, wenn Benutzer die Kategorieauswahl ändern. Im nächsten Abschnitt definieren Sie ein Back-End, das Kategoriebenachrichtigungen an diese App senden kann.

## <a name="run-the-uwp-app"></a>Ausführen der UWP-App 
1. Drücken Sie in Visual Studio die Taste **F5**, um die App zu starten. Die Benutzeroberfläche der App bietet verschiedene Umschaltmöglichkeiten, mit denen Sie die Kategorien auswählen können, die Sie abonnieren möchten.

    ![App für aktuelle Nachrichten](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png)

2. Aktivieren Sie eine oder mehrere Kategorien, und klicken Sie dann auf **Subscribe**.

    Die App konvertiert die ausgewählten Kategorien in Tags, und fordert eine neue Geräteregistrierung für die ausgewählten Tags vom Notification Hub an. Die registrierten Kategorien werden zurückgegeben und in einem Dialogfeld angezeigt.

    ![Umschalter für Kategorien und Schaltfläche „Subscribe“ (Abonnieren)](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png)

## <a name="create-a-console-app-to-send-tagged-notifications"></a>Erstellen einer Konsolen-App zum Senden von Benachrichtigungen mit Tags

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-console-app-to-send-tagged-notifications"></a>Ausführen der Konsolen-App zum Senden von Benachrichtigungen mit Tags

1. Führen Sie die im vorherigen Abschnitt erstellte App aus.
2. Die Benachrichtigungen für die ausgewählten Kategorien werden als Popupbenachrichtigungen angezeigt. Wenn Sie die Benachrichtigung auswählen, wird das erste UWP-App-Fenster angezeigt. 

     ![Popupbenachrichtigungen](./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie aktuelle Nachrichten nach Kategorie übermittelt werden. Die Back-End-Anwendung sendet Benachrichtigungen mit Tags an Geräte, die sich für den Empfang von Benachrichtigungen für dieses Tag registriert haben. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Benutzer senden, unabhängig davon, welche Geräte sie verwenden, fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
> [Senden von lokalisierten Pushbenachrichtigungen](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- URLs.-->
[get-started]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: https://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
