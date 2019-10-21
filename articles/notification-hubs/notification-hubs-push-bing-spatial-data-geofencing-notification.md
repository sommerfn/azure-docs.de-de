---
title: Senden von Pushbenachrichtigungen mit Azure Notification Hubs und Bing Spatial Data | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie standortbasierte Pushbenachrichtigungen mit Azure Notification Hubs und Bing Spatial Data bereitstellen.
services: notification-hubs
documentationcenter: windows
keywords: Pushbenachrichtigungen, Pushbenachrichtigungen
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: f41beea1-0d62-4418-9ffc-c9d70607a1b7
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 510e2648db3076a0e3ee2535c6058f7ed212f558
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387486"
---
# <a name="tutorial-send-location-based-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Tutorial: Senden standortbasierter Pushbenachrichtigungen mit Azure Notification Hubs und Bing Spatial Data

In diesem Tutorial erfahren Sie, wie Sie standortbasierte Pushbenachrichtigungen mit Azure Notification Hubs und Bing Spatial Data bereitstellen.

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Einrichten der Datenquelle
> * Einrichten der UWP-Anwendung
> * Einrichten des Back-Ends
> * Testen von Pushbenachrichtigungen in der UWP-App (Universelle Windows-Plattform)

## <a name="prerequisites"></a>Voraussetzungen

* **Azure-Abonnement**. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
* [Visual Studio 2015 Update 1](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) oder höher ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409)).
* Neueste Version des [Azure SDK](https://azure.microsoft.com/downloads/)
* [Bing Maps Dev Center-Konto](https://www.bingmapsportal.com/) (Sie können kostenlos ein Konto erstellen und Ihrem Microsoft-Konto zuordnen)

## <a name="set-up-the-data-source"></a>Einrichten der Datenquelle

1. Melden Sie sich beim [Bing Maps Dev Center](https://www.bingmapsportal.com/) an.
2. Klicken Sie in der oberen Navigationsleiste auf **Datenquellen**, und wählen Sie **Datenquellen verwalten** aus.

    ![](./media/notification-hubs-geofence/bing-maps-manage-data.png)
3. Wenn Sie noch nicht über eine Datenquelle verfügen, wird ein Link angezeigt, über den Sie eine Datenquelle erstellen können. Wählen Sie **Daten als Datenquelle hochladen** aus. Sie können auch das Menü **Datenquellen** > **Daten hochladen** verwenden.

    ![](./media/notification-hubs-geofence/bing-maps-create-data.png)
4. Erstellen Sie auf Ihrer Festplatte eine Datei namens `NotificationHubsGeofence.pipe` mit folgendem Inhalt: In diesem Tutorial verwenden Sie eine pipebasierte Beispieldatei, die einen Teil des Hafengebiets von San Francisco eingrenzt:

    ```text
    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    ```

    Die Pipedatei stellt diese Entität dar:

    ![](./media/notification-hubs-geofence/bing-maps-geofence.png)
5. Führen Sie auf der Seite **Datenquelle hochladen** die folgenden Aktionen aus:
   1. Wählen Sie **Pipe** als **Datenformat** aus.
   2. Navigieren Sie zu der Datei `NotificationHubGeofence.pipe`, die Sie im vorherigen Schritt erstellt haben, und wählen Sie sie aus.
   3. Klicken Sie auf die Schaltfläche **Hochladen**.

      > [!NOTE]
      > Unter Umständen werden Sie aufgefordert, einen neuen Schlüssel als **Hauptschlüssel** anzugeben, der sich vom **Abfrageschlüssel** unterscheidet. Erstellen Sie im Dashboard einfach einen neuen Schlüssel, und aktualisieren Sie die Seite zum Hochladen der Datenquelle.
6. Nachdem Sie die Datendatei hochgeladen haben, müssen Sie sicherstellen, dass Sie die Datenquelle veröffentlichen. Wählen Sie wie zuvor **Datenquellen** -> **Datenquellen verwalten** aus.
7. Wählen Sie in der Liste Ihre Datenquelle aus, und klicken Sie in der Spalte **Aktionen** auf **Veröffentlichen**.

    ![](./media/notification-hubs-geofence/publish-button.png)
8. Wechseln Sie zur Registerkarte **Veröffentlichte Datenquellen**, und vergewissern Sie sich, dass Ihre Datenquelle in der Liste angezeigt wird.

    ![](./media/notification-hubs-geofence/bing-maps-published-data.png)
9. Wählen Sie **Bearbeiten** aus. Sie sehen auf einen Blick, welche Standorte Sie in die Daten eingeschlossen haben.

    ![](./media/notification-hubs-geofence/bing-maps-data-details.png)

    An diesem Punkt werden die Grenzen für den von Ihnen erstellten Geofence nicht angezeigt. Sie benötigen lediglich die Bestätigung, dass sich der angegebene Standort in der richtigen Umgebung befindet.
10. Sie verfügen nun über alle Anforderungen für die Datenquelle. Wählen Sie zum Abrufen der Details zur Anforderungs-URL für den API-Aufruf im Bing Maps Dev Center die Option **Datenquellen** und dann **Datenquelleninformationen**.

    ![](./media/notification-hubs-geofence/bing-maps-data-info.png)

    Die **Abfrage-URL** ist der Endpunkt, für den Sie Abfragen ausführen können, um zu überprüfen, ob sich das Gerät derzeit innerhalb der Grenzen eines Standorts befindet. Zur Überprüfung führen Sie einfach einen GET-Aufruf mit den folgenden angefügten Parametern für die Abfrage-URL aus:

    ```text
    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY
    ```

    Bing Maps führt automatisch die notwendigen Berechnungen durch, um zu ermitteln, ob sich das Gerät innerhalb des Geofence befindet. Nachdem Sie die Anforderung über einen Browser (oder cURL) ausgeführt haben, erhalten Sie eine JSON-Standardantwort:

    ![](./media/notification-hubs-geofence/bing-maps-json.png)

    Diese Antwort erfolgt nur, wenn sich der Punkt tatsächlich innerhalb der festgelegten Grenzen befindet. Andernfalls erhalten Sie einen leeren **results**-Bucket:

    ![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="set-up-the-uwp-application"></a>Einrichten der UWP-Anwendung

1. Starten Sie in Visual Studio ein neues Projekt vom Typ **Leere App (Universelle Windows-App)** .

    ![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

    Nachdem die Projekterstellung abgeschlossen ist, sollten Sie über das Grundgerüst der App verfügen. Nun führen wir die Einrichtung für die Geofencing-Infrastruktur durch. Da Sie für diese Lösung Bing-Dienste verwenden, ist ein öffentlicher REST-API-Endpunkt vorhanden, mit dem Sie spezielle Standortrahmen abfragen können:

    ```text
    http://spatial.virtualearth.net/REST/v1/data/
    ```
    Geben Sie die folgenden Parameter an:

   * **Datenquellen-ID** und **Datenquellenname**: In der Bing Maps-API enthalten Datenquellen Metadaten in verschiedenen „Buckets“, z. B. Standorte und Geschäftszeiten.  
   * **Entitätsname** : Gibt die Entität an, die Sie als Referenzpunkt für die Benachrichtigung verwenden möchten.
   * **Bing Maps-API-Schlüssel**: Dies ist der Schlüssel, den Sie beim Erstellen des Bing Dev Center-Kontos abgerufen haben.

     Da die Datenquelle jetzt bereitsteht, können Sie mit der Arbeit an der UWP-Anwendung beginnen.
2. Aktivieren Sie Standortdienste für Ihre Anwendung. Öffnen Sie im **Projektmappen-Explorer** die Datei `Package.appxmanifest`.

    ![](./media/notification-hubs-geofence/vs-package-manifest.png)
3. Wechseln Sie auf der Registerkarte mit den Paketeigenschaften, die geöffnet wird, zu **Funktionen**, und wählen Sie **Standort** aus.

    ![](./media/notification-hubs-geofence/vs-package-location.png)
4. Erstellen Sie in Ihrer Projektmappe einen neuen Ordner mit dem Namen `Core` und fügen diesem eine neue Datei namens `LocationHelper.cs` hinzu:

    ![](./media/notification-hubs-geofence/vs-location-helper.png)

    In der `LocationHelper`-Klasse ist Code vorhanden, um den Benutzerstandort über die System-API abzurufen:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }
    ```

    Weitere Informationen zum Abrufen des Standorts von Benutzern in UWP-Apps finden Sie unter [Abrufen der Position eines Benutzers](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).
5. Um zu überprüfen, ob die Standorterfassung funktioniert, öffnen Sie die Codeseite der Hauptseite (`MainPage.xaml.cs`). Erstellen Sie einen neuen Ereignishandler für das `Loaded`-Ereignis im `MainPage`-Konstruktor.

    ```csharp
    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }
    ```

    Die Implementierung des Ereignishandlers lautet wie folgt:

    ```csharp
    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }
    ```
6. Führen Sie die Anwendung aus, und gewähren Sie ihr Zugriff auf Ihren Standort.

    ![](./media/notification-hubs-geofence/notification-hubs-location-access.png)
7. Nach dem Starten der Anwendung sollten die Koordinaten im Fenster **Ausgabe** angezeigt werden:

    ![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

    Jetzt wissen Sie, dass die Standorterfassung funktioniert. Wenn Sie möchten, können Sie den Testereignishandler „Loaded“ jetzt entfernen, da er nicht mehr benötigt wird.
8. Der nächste Schritt ist das Erfassen der Standortänderungen. Fügen Sie in der `LocationHelper`-Klasse den Ereignishandler für `PositionChanged` hinzu:

    ```csharp
    geolocator.PositionChanged += Geolocator_PositionChanged;
    ```

    Die Implementierung zeigt die Standortkoordinaten im Fenster **Ausgabe** an:

    ```csharp
    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }
    ```

## <a name="set-up-the-backend"></a>Einrichten des Back-Ends

1. Laden Sie das [.NET-Back-End-Beispiel von GitHub herunter](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).
2. Öffnen Sie nach Abschluss des Downloads den Ordner `NotifyUsers`, und öffnen Sie dann die Datei `NotifyUsers.sln` in Visual Studio.
3. Geben Sie das Projekt `AppBackend` mit der Option **Als Startprojekt festlegen** als Startprojekt an, und starten Sie es.

    ![](./media/notification-hubs-geofence/vs-startup-project.png)

    Das Projekt ist bereits zum Senden von Pushbenachrichtigungen an Zielgeräte konfiguriert, sodass Sie nur noch zwei Dinge tun müssen: die richtige Verbindungszeichenfolge für den Notification Hub angeben und eine Grenzidentifizierung hinzufügen, damit die Benachrichtigung nur dann gesendet wird, wenn sich der Benutzer innerhalb des Geofence befindet.

4. Öffnen Sie zum Konfigurieren der Verbindungszeichenfolge im Ordner `Models` die Datei `Notifications.cs`. Die Funktion `NotificationHubClient.CreateClientFromConnectionString` sollte die Informationen zu Ihrem Notification Hub enthalten, die Sie im [Azure-Portal](https://portal.azure.com) abrufen können (Seite **Zugriffsrichtlinien** in den **Einstellungen**). Speichern Sie die aktualisierte Konfigurationsdatei.
5. Erstellen Sie ein Modell für das Bing Maps-API-Ergebnis. Die einfachste Möglichkeit besteht darin, den Ordner `Models` zu öffnen und **Hinzufügen** > **Klasse** auszuwählen. Vergeben Sie den Namen `GeofenceBoundary.cs`. Wenn dies erledigt ist, kopieren Sie die JSON aus der API-Antwort, die Sie im ersten Abschnitt erhalten haben. Verwenden Sie in Visual Studio die Optionen **Bearbeiten** > **Inhalte einfügen** > **JSON als Klassen einfügen**.

    Auf diese Weise stellen Sie sicher, dass das Objekt genau wie gewünscht deserialisiert wird. Der resultierende Klassensatz sollte wie die folgende Klasse aussehen:

    ```csharp
    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }
    ```
6. Öffnen Sie nun `Controllers` > `NotificationsController.cs`. Aktualisieren Sie den POST-Aufruf, sodass der Längen- und Breitengrad des Ziels einbezogen werden. Fügen Sie der Funktionssignatur zu diesem Zweck zwei Zeichenfolgen hinzu: `latitude` und `longitude`.

    ```csharp
    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)
    ```
7. Erstellen Sie im Projekt eine neue Klasse mit dem Namen `ApiHelper.cs`. Sie verwenden diese Klasse zum Herstellen der Verbindung mit Bing, um Grenzschnittpunkte zu überprüfen. Implementieren Sie eine `IsPointWithinBounds`-Funktion, wie im folgenden Code veranschaulicht:

    ```csharp
    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }
    ```

    > [!IMPORTANT]
    > Stellen Sie sicher, dass Sie den API-Endpunkt durch die Abfrage-URL ersetzen, die Sie weiter oben aus Bing Dev Center abgerufen haben (dasselbe gilt für den API-Schlüssel).

    Wenn die Abfrage zu Ergebnissen führt, bedeutet dies, dass sich der angegebene Punkt innerhalb der Grenzen des Geofence befindet. Die Funktion gibt also `true` zurück. Wenn keine Ergebnisse vorliegen, teilt Bing Ihnen mit, dass sich der Punkt außerhalb des Suchbereichs befindet. Die Funktion gibt also `false` zurück.
8. Erstellen Sie in `NotificationsController.cs` direkt vor der switch-Anweisung eine Überprüfung:

    ```csharp
    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }
    ```

## <a name="test-push-notifications-in-the-uwp-app"></a>Testen von Pushbenachrichtigungen in der UWP-App

1. Sie sollten in der UWP-App jetzt Benachrichtigungen testen können. Erstellen Sie in der `LocationHelper`-Klasse die neue Funktion `SendLocationToBackend`:

    ```csharp
    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }
    ```

    > [!NOTE]
    > Legen Sie die `POST_URL` auf den Speicherort Ihrer bereitgestellten Webanwendung fest. Für den Zweck dieses Tutorials ist die lokale Ausführung in Ordnung. Wenn Sie jedoch eine öffentliche Version bereitstellen möchten, müssen Sie diese über einen externen Anbieter hosten.
2. Registrieren Sie die UWP-App für Pushbenachrichtigungen. Wählen Sie in Visual Studio **Projekt** > **Store** > **App mit Store verknüpfen**.

    ![](./media/notification-hubs-geofence/vs-associate-with-store.png)
3. Stellen Sie nach dem Anmelden an Ihrem Entwicklerkonto sicher, dass Sie eine vorhandene App auswählen oder eine neue App erstellen und ihr das Paket zuordnen.
4. Wechseln Sie zum Dev Center, und öffnen Sie die erstellte App. Wählen Sie **Dienste** > **Pushbenachrichtigungen** > **Live Services-Website**.

    ![](./media/notification-hubs-geofence/ms-live-services.png)
5. Notieren Sie sich von der Website den **geheimen Schlüssel der Anwendung** und die **Paket-SID**. Sie benötigen beide Angaben im Azure-Portal. Öffnen Sie Ihren Notification Hub, wählen Sie **Einstellungen** > **Notification Services** > **Windows (WNS)** aus, und geben Sie die Informationen in die erforderlichen Felder ein.

    ![](./media/notification-hubs-geofence/notification-hubs-wns.png)
6. Wählen Sie **Speichern** aus.
7. Öffnen Sie im **Projektmappen-Explorer** die Option **Verweise**, und wählen Sie **NuGet-Pakete verwalten**. Fügen Sie einen Verweis auf die **verwaltete Microsoft Azure Service Bus-Bibliothek** hinzu. Suchen Sie einfach nach `WindowsAzure.Messaging.Managed`, und fügen Sie das Element zum Projekt hinzu.

    ![](./media/notification-hubs-geofence/vs-nuget.png)
8. Erstellen Sie den Ereignishandler `MainPage_Loaded` zu Testzwecken noch einmal und fügen diesen Codeausschnitt hinzu:

    ```csharp
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }
    ```

    Der Code registriert die App beim Notification Hub. Sie sind fertig!
9. Im `LocationHelper`-Element im Handler `Geolocator_PositionChanged` können Sie Testcode hinzufügen, der die Festlegung des Standorts innerhalb des Geofence erzwingt:

    ```csharp
    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");
    ```

10. Da Sie nicht die echten Koordinaten übergeben (die sich derzeit ggf. nicht innerhalb der Grenzen befinden) und vordefinierte Testwerte verwenden, wird beim Aktualisieren eine Benachrichtigung angezeigt:

    ![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="next-steps"></a>Nächste Schritte

Es gibt einige Schritte, deren Ausführung unter Umständen erforderlich ist, um sicherzustellen, dass die Lösung bereit für die Produktion ist.

1. Zunächst sollten Sie sicherstellen, dass die Geofences dynamisch sind. Hierfür ist etwas zusätzliche Arbeit mit der Bing-API erforderlich, um neue Grenzen innerhalb der vorhandenen Datenquelle hochladen zu können. Weitere Informationen finden Sie in der [Dokumentation zur Bing Spatial Data Services-API](https://msdn.microsoft.com/library/ff701734.aspx).
2. Außerdem kann es bei der Sicherstellung, dass die Bereitstellung für die richtigen Teilnehmer erfolgt, ratsam sein, das [Tagging](notification-hubs-tags-segment-push-message.md)zu verwenden.

Die in diesem Tutorial vorgestellte Lösung beschreibt ein Szenario, in dem Sie viele verschiedene Plattformen nutzen können, sodass das Geofencing nicht auf systemspezifische Funktionen beschränkt ist. Die universelle Windows-Plattform verfügt aber standardmäßig über Funktionen zum [Erkennen von Geofences](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).
