---
title: Senden von Benachrichtigungen an Apps für die universelle Windows-Plattform mit Azure Notification Hubs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an eine App für die universelle Windows-Plattform senden.
services: notification-hubs
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/22/2019
ms.author: jowargo
ms.openlocfilehash: 714c2be7265a41a7791ce39bfaa1bdc12fa58cac
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340748"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-by-using-azure-notification-hubs"></a>Tutorial: Senden von Benachrichtigungen an Apps für die universelle Windows-Plattform mit Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Tutorial erstellen Sie einen Notification Hub, um Pushbenachrichtigungen an eine App für die universelle Windows-Plattform (UWP) zu senden. Sie erstellen eine leere Windows Store-App, die Pushbenachrichtigungen unter Verwendung des Windows-Pushbenachrichtigungsdiensts (WNS) empfängt. Dann senden Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte, die Ihre App ausführen.

> [!NOTE]
> Den vollständigen Code für dieses Tutorial finden Sie auf [GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/UwpSample).

Führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer App im Windows Store
> * Erstellen eines Notification Hubs
> * Erstellen einer Beispiel-Windows-App
> * Senden von Testbenachrichtigungen

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- [Microsoft Visual Studio Community 2015 oder höher](https://www.visualstudio.com/products/visual-studio-community-vs)
- [Installierte Entwicklungstools für UWP-Apps](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Ein aktives Windows Store-Konto
- Vergewissern Sie sich, dass die Einstellung **Benachrichtigungen von Apps und anderen Absendern abrufen** aktiviert ist. 
    - Starten Sie das Fenster **Einstellungen** auf Ihrem Computer.
    - Wählen Sie die Kachel **System** aus.
    - Wählen Sie links im Menü **Benachrichtigungen und Aktionen** aus. 
    - Vergewissern Sie sich, dass die Einstellung **Benachrichtigungen von Apps und anderen Absendern abrufen** aktiviert ist. Ist sie nicht aktiviert, aktivieren Sie sie. 

Dieses Tutorial muss für alle anderen Notification Hubs-Tutorials für UWP-Apps abgeschlossen werden.

## <a name="create-an-app-in-windows-store"></a>Erstellen einer App im Windows Store

Ordnen Sie Ihre App dem Windows Store zu, um Pushbenachrichtigungen an UWP-Apps senden zu können. Konfigurieren Sie anschließend Ihren Notification Hub für die Integration in WNS.

1. Navigieren Sie zum [Windows Dev Center](https://partner.microsoft.com/en-us/dashboard/windows/first-run-experience), melden Sie sich dort mit Ihrem Microsoft-Konto an, und wählen Sie dann **Neue App erstellen** aus.

    ![„Neue App“-Schaltfläche](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
2. Geben Sie einen Namen für Ihre App ein, und wählen Sie anschließend **Produktname reservieren** aus. Dadurch wird eine neue Windows Store-Registrierung für Ihre App erstellt.

    ![Store-App-Name](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
3. Erweitern Sie **App-Verwaltung**, wählen Sie **WNS/MPNS** und dann **Live Services-Website** aus. Anmelden bei Ihrem Azure-Konto Das **App-Registrierungsportal** wird auf einer neuen Registerkarte geöffnet. Alternativ können Sie direkt zum [App-Registrierungsportal](https://apps.dev.microsoft.com) navigieren und Ihren Anwendungsnamen auswählen, um zu dieser Seite zu gelangen.

    ![WNS/MPNS-Seite](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
4. Notieren Sie sich das Kennwort unter **Anwendungsgeheimnis** sowie die **Paketsicherheits-ID (SID)** .

    >[!WARNING]
    >Der geheime Schlüssel der Anwendung und die Paket-SID sind wichtige Sicherheitsanmeldeinformationen. Geben Sie diese Werte nicht weiter, und verteilen Sie sie nicht mit Ihrer Anwendung.

## <a name="create-a-notification-hub"></a>Erstellen eines Notification Hubs

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-wns-settings-for-the-hub"></a>Konfigurieren der WNS-Einstellungen für den Hub

1. Wählen Sie in der Kategorie **BENACHRICHTIGUNGSEINSTELLUNGEN** die Option **Windows (WNS)** aus.
2. Geben Sie die Werte für **Paket-SID** und **Sicherheitsschlüssel** ein, die Sie im vorherigen Abschnitt notiert haben.
3. Klicken Sie in der Symbolleiste auf **Speichern** .

    ![Die Felder für Paket-SID und Sicherheitsschlüssel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Ihr Notification Hub ist jetzt für die Zusammenarbeit mit WNS konfiguriert. Sie verfügen über die Verbindungszeichenfolgen, die Sie zum Registrieren Ihrer App und zum Senden von Benachrichtigungen benötigen.

## <a name="create-a-sample-windows-app"></a>Erstellen einer Beispiel-Windows-App

1. Öffnen Sie in Visual Studio das Menü **Datei**, und wählen Sie die Optionen **Neu** und **Projekt**.
2. Führen Sie im Dialogfenster **Neues Projekt** die folgenden Schritte aus:

    1. Erweitern Sie **Visual C#** .
    2. Wählen Sie **Windows Universal** aus.
    3. Wählen Sie **Leere App (universelles Windows)** aus.
    4. Geben Sie einen **Namen** für das Projekt ein.
    5. Klicken Sie auf **OK**.

        ![Dialogfeld "Neues Projekt"](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)
3. Übernehmen Sie die Standardwerte für die **Zielversion** und die **Mindestversion** der Plattform, und wählen Sie **OK** aus.
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Windows Store-App-Projekt, und wählen Sie **Store** und anschließend **App mit Store verknüpfen** aus. Daraufhin wird der Assistent **App mit Windows Store verknüpfen** angezeigt.
5. Melden Sie sich im Assistenten mit Ihrem Microsoft-Konto an.
6. Wählen Sie die App aus, die Sie in Schritt 2 registriert haben. Wählen Sie dann **Weiter** und anschließend **Zuordnen** aus. Dadurch werden die erforderlichen Windows Store-Registrierungsinformationen zum Anwendungsmanifest hinzugefügt.
7. Klicken Sie in Visual Studio mit der rechten Maustaste auf die Projektmappe, und wählen Sie **NuGet-Pakete verwalten** aus. Das Fenster **NuGet-Pakete verwalten** wird geöffnet.
8. Geben Sie in das Suchfeld die Zeichenfolge **WindowsAzure.Messaging.Managed** ein, wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.

    ![Das Fenster „NuGet-Pakete verwalten“][20]

    Mit dieser Aktion wird unter Verwendung des [Microsoft.Azure.NotificationHubs-NuGet-Pakets](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs) die Azure Notification Hubs-Bibliothek für Windows heruntergeladen, installiert und ein Verweis hinzugefügt.
9. Öffnen Sie die Projektdatei `App.xaml.cs`, und fügen Sie die folgenden Anweisungen hinzu:

    ```csharp
    using Windows.Networking.PushNotifications;
    using Microsoft.WindowsAzure.Messaging;
    using Windows.UI.Popups;
    ```

10. Suchen Sie in der Datei `App.xaml.cs` des Projekts die Klasse `App`, und fügen Sie die folgende `InitNotificationsAsync`-Methodendefinition hinzu:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
        var result = await hub.RegisterNativeAsync(channel.Uri);

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

    Dieser Code ruft den Kanal-URI für die App von WNS ab und registriert dann diesen Kanal-URI bei Ihrem Benachrichtigungshub.

    >[!NOTE]
    > Ersetzen Sie den Platzhalter `hub name` durch den Namen des Notification Hubs aus dem Azure-Portal. Ersetzen Sie außerdem den Verbindungszeichenfolgen-Platzhalter durch die Verbindungszeichenfolge `DefaultListenSharedAccessSignature`, die Sie in einem der vorherigen Abschnitte über die Seite **Zugriffsrichtlinien** Ihres Notification Hubs ermittelt haben.

11. Fügen Sie in der Datei `App.xaml.cs` oben im Ereignishandler `OnLaunched` den folgenden Aufruf zur neuen `InitNotificationsAsync`-Methode hinzu:

    ```csharp
    InitNotificationsAsync();
    ```

    Dadurch wird gewährleistet, dass der Kanal-URI bei jedem Start der Anwendung beim Notification Hub registriert wird.

12. Drücken Sie zum Ausführen der App auf der Tastatur die Taste **F5**. Ein Dialogfeld mit dem Registrierungsschlüssel wird angezeigt. Klicken Sie auf **OK**, um das Dialogfeld zu schließen.

    ![Erfolgreiche Registrierung](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

Ihre App kann jetzt Popupbenachrichtigungen empfangen.

## <a name="send-test-notifications"></a>Senden von Testbenachrichtigungen

Der Benachrichtigungsempfang in der App kann schnell durch Senden von Benachrichtigungen im [Azure-Portal](https://portal.azure.com/) getestet werden.

1. Wechseln Sie im Azure-Portal zur Registerkarte Übersicht, und wählen Sie auf der Symbolleiste **Testsendevorgang** aus.

    ![Schaltfläche „Testsendevorgang“](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. Führen Sie im Fenster **Testsendevorgang** die folgenden Aktionen aus:
    1. Wählen Sie für **Plattformen** die Option **Windows** aus.
    2. Wählen Sie für **Benachrichtigungstyp** die Option **Popup** aus.
    3. Wählen Sie **Senden** aus.

        ![Der Bereich „Testsendevorgang“](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Das Ergebnis des Sendevorgangs wird am unteren Rand des Fensters in der Liste **Ergebnis** angezeigt. Sie sehen auch eine Warnmeldung.

    ![Ergebnis des Sendevorgangs](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
4. Sie sehen die Benachrichtigungsmeldung **Testmeldung** auf Ihrem Desktop.

    ![Benachrichtigungsmeldung](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)

## <a name="next-steps"></a>Nächste Schritte
Sie haben über das Portal oder über eine Konsolen-App Übertragungsbenachrichtigungen an alle Ihre Windows-Geräte gesendet. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte Geräte senden, fahren Sie mit dem folgenden Tutorial fort:

> [!div class="nextstepaction"]
>[Senden von Pushbenachrichtigungen an bestimmte Geräte](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[toast catalog]: https://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: https://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: https://msdn.microsoft.com/library/windows/apps/hh779719.aspx
