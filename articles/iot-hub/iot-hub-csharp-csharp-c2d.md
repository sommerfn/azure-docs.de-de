---
title: C2D-Nachrichten mit Azure IoT Hub (.NET) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der Azure IoT SDKs für .NET C2D-Nachrichten von einer Azure IoT Hub-Instanz an ein Gerät senden. Sie passen eine Geräte-App für den Empfang von C2D-Nachrichten und eine Back-End-App zum Senden der C2D-Nachrichten an.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: robinsh
ms.openlocfilehash: 5fb6b17bcbc39b4e7531f79b832853a4f1ed1fd5
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69558668"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten und einem Lösungs-Back-End ermöglicht. In der Schnellstartanleitung [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)](quickstart-send-telemetry-dotnet.md) erfahren Sie, wie Sie einen IoT-Hub erstellen, eine Geräteidentität darin bereitstellen und eine Geräte-App programmieren, die D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud) sendet.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Dieses Tutorial baut auf [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Python)](quickstart-send-telemetry-dotnet.md) auf. Es zeigt, wie Sie die folgenden Aufgaben ausführen:

* Senden von C2D-Nachrichten aus Ihrem Lösungs-Back-End an ein einzelnes Gerät über IoT Hub.

* Empfangen von C2D-Nachrichten auf einem Gerät.

* Anfordern einer Übermittlungsbestätigung (*Feedback*) von Ihrem Lösungs-Back-End für Nachrichten, die von IoT Hub an ein einzelnes Gerät gesendet wurden.

Weitere Informationen zu C2D-Nachrichten finden Sie unter [D2C- und C2D-Messaging IoT Hub](iot-hub-devguide-messaging.md).

Am Ende dieses Tutorials führen Sie zwei .NET-Konsolen-Apps aus.

* **SimulatedDevice**. Diese App stellt eine Verbindung mit Ihrem IoT-Hub her und empfängt C2D-Nachrichten. Bei dieser App handelt es sich um eine geänderte Version der App, die in [Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz](quickstart-send-telemetry-dotnet.md) erstellt wurde.

* **SendCloudToDevice**. Diese App sendet über IoT Hub eine C2D-Nachricht an die Geräte-App und empfängt die zugehörige Übermittlungsbestätigung.

> [!NOTE]
> IoT Hub bietet durch [Azure IoT-Geräte-SDKs](iot-hub-devguide-sdks.md) Unterstützung für zahlreiche Geräteplattformen und Sprachen, darunter C, Java, Python und JavaScript. Im [IoT Hub-Entwicklerhandbuch](iot-hub-devguide.md) finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit dem Code in diesem Tutorial sowie allgemeine Informationen zum Verbinden mit Azure IoT Hub.
>

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Visual Studio

* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.

## <a name="receive-messages-in-the-device-app"></a>Empfangen von Nachrichten in der Geräte-App

In diesem Abschnitt ändern Sie die Geräte-App, die Sie in [Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz](quickstart-send-telemetry-dotnet.md) erstellt haben, um C2D-Nachrichten vom IoT-Hub zu empfangen.

1. Fügen Sie in Visual Studio im **SimulatedDevice**-Projekt der **Program**-Klasse die folgende Methode hinzu.

   ```csharp
    private static async void ReceiveC2dAsync()
    {
        Console.WriteLine("\nReceiving cloud to device messages from service");
        while (true)
        {
            Message receivedMessage = await s_deviceClient.ReceiveAsync();
            if (receivedMessage == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received message: {0}", 
            Encoding.ASCII.GetString(receivedMessage.GetBytes()));
            Console.ResetColor();

            await s_deviceClient.CompleteAsync(receivedMessage);
        }
    }
   ```

1. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:

   ```csharp
   ReceiveC2dAsync();
   ```

Die `ReceiveAsync`-Methode gibt die empfangene Nachricht asynchron zurück, sobald sie vom Gerät empfangen wird. Sie gibt nach einem spezifizierbaren Zeitlimit *NULL* zurück. In diesem Beispiel wird der Standardwert von einer Minute verwendet. Wenn die App *NULL* empfängt, sollte sie weiter auf neue Nachrichten warten. Diese Anforderung ist der Grund für die `if (receivedMessage == null) continue`-Zeile.

Der Aufruf von `CompleteAsync()` teilt IoT Hub mit, dass die Nachricht erfolgreich verarbeitet wurde. Die Nachricht kann sicher aus der Gerätewarteschlange entfernt werden. Wenn ein Fehler aufgetreten ist, der den Abschluss der Verarbeitung der Nachricht durch die Geräte-App verhindert hat, übermittelt IoT Hub sie erneut. Die Nachrichtenverarbeitungslogik in der Geräte-App muss *idempotent* sein, sodass der mehrmalige Empfang derselben Nachricht dasselbe Ergebnis erzeugt.

Eine Anwendung kann eine Nachricht auch vorübergehend verwerfen, sodass IoT Hub die Nachricht für die zukünftige Verwendung in der Warteschlange beibehält. Oder die Anwendung kann eine Nachricht ablehnen, wodurch die Nachricht dauerhaft aus der Warteschlange entfernt wird. Weitere Informationen zum Lebenszyklus von C2D-Nachrichten finden Sie unter [D2C- und C2D-Messaging mit IoT Hub](iot-hub-devguide-messaging.md).

   > [!NOTE]
   > Wird HTTPS anstelle von MQTT oder AMQP als Transport verwendet, erfolgt von der `ReceiveAsync`-Methode sofort eine Rückgabe. Das unterstützte Muster für C2D-Nachrichten mit HTTPS entspricht Geräten, die nicht permanent verbunden sind und weniger häufig Nachrichten abrufen (d.h. seltener als alle 25 Minuten). Die Ausgabe von mehr HTTPS-Empfangsvorgängen führt dazu, dass die Anforderungen von IoT Hub gedrosselt werden. Weitere Informationen zu den Unterschieden zwischen der MQTT-, AMQP- und HTTPS-Unterstützung sowie zur IoT Hub-Drosselung finden Sie unter [D2C- und C2D-Messaging mit IoT Hub](iot-hub-devguide-messaging.md).
   >

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

In diesem Artikel erstellen Sie einen Back-End-Dienst, um C2D-Nachrichten über die IoT Hub-Instanz zu senden, die Sie in [Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz ](quickstart-send-telemetry-dotnet.md) erstellt haben. Damit Ihr Dienst Cloud-zu-Gerät-Nachrichten senden kann, muss er über die Berechtigung **Dienstverbindung** verfügen. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **service** erstellt, die diese Berechtigung erteilt.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="send-a-cloud-to-device-message"></a>Senden einer C2D-Nachricht

Jetzt schreiben Sie eine .NET-Konsolen-App, die C2D-Nachrichten an die Geräte-App sendet.

1. Wählen Sie in der aktuellen Visual Studio-Projektmappe **Datei** > **Neu** > **Projekt** aus. Wählen Sie in **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** und dann **Weiter** aus.

1. Nennen Sie das Projekt *SendCloudToDevice*. Wählen Sie unter **Projektmappe** die Option **Zur Projektmappe hinzufügen** aus, und übernehmen Sie die neueste Version von .NET Framework. Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

   ![Konfigurieren eines neuen Projekts in Visual Studio](./media/iot-hub-csharp-csharp-c2d/sendcloudtodevice-project-configure.png)

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die neue Projektmappe, und wählen Sie **NuGet-Pakete verwalten** aus.

1. Wählen Sie in **NuGet-Pakete verwalten** die Option **Durchsuchen**, anschließend „Suchen nach“ und dann **Microsoft.Azure.Devices** aus. Wählen Sie **Installieren** aus.

   In diesem Schritt wird das [NuGet-Paket mit dem Dienst-SDK für Azure IoT](https://www.nuget.org/packages/Microsoft.Azure.Devices/) heruntergeladen, installiert und mit einem Verweis versehen.

1. Fügen Sie am Anfang der Datei **Program.cs** die folgende `using`-Anweisung hinzu.

   ``` csharp
   using Microsoft.Azure.Devices;
   ```

1. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben.

   ``` csharp
   static ServiceClient serviceClient;
   static string connectionString = "{iot hub connection string}";
   ```

1. Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Legen Sie den Gerätenamen auf die Zeichenfolge fest, die Sie beim Definieren des Geräts in [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)](quickstart-send-telemetry-dotnet.md) verwendet haben.

   ``` csharp
   private async static Task SendCloudToDeviceMessageAsync()
   {
        var commandMessage = new
         Message(Encoding.ASCII.GetBytes("Cloud to device message."));
        await serviceClient.SendAsync("myFirstDevice", commandMessage);
   }
   ```

   Diese Methode sendet eine neue C2D-Nachricht an das Gerät mit der ID `myFirstDevice`. Ändern Sie diesen Parameter nur, falls Sie den in [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)](quickstart-send-telemetry-dotnet.md) verwendeten Parameter geändert haben.

1. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu.

   ``` csharp
   Console.WriteLine("Send Cloud-to-Device message\n");
   serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

   Console.WriteLine("Press any key to send a C2D message.");
   Console.ReadLine();
   SendCloudToDeviceMessageAsync().Wait();
   Console.ReadLine();
   ```

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen** aus.

1. Wählen Sie in **Allgemeine Eigenschaften** > **Startprojekt** die Option **Mehrere Startprojekte** und dann die Aktion **Starten** für **ReadDeviceToCloudMessages**, **SimulatedDevice** und **SendCloudToDevice** aus. Klicken Sie zum Speichern der Änderungen auf **OK** .

1. Drücken Sie **F5**. Alle drei Anwendungen sollten beginnen. Wählen Sie das Fenster **SendCloudToDevice** aus, und drücken Sie die **EINGABETASTE**. Die von der Geräte-App empfangene Meldung sollte angezeigt werden.

   ![Nachrichtenempfangs-App](./media/iot-hub-csharp-csharp-c2d/sendc2d1.png)

## <a name="receive-delivery-feedback"></a>Empfangen von Übermittlungsfeedback

Es ist möglich, für jede C2D-Nachricht Übermittlungsbestätigungen (oder Ablaufbestätigungen) von IoT Hub anzufordern. Mit dieser Option kann die Wiederholungs- oder Kompensierungslogik problemlos vom Lösungs-Back-End informiert werden. Weitere Informationen zu C2D-Feedback finden Sie unter [D2C- und C2D-Messaging mit IoT Hub](iot-hub-devguide-messaging.md).

In diesem Abschnitt ändern Sie die **SendCloudToDevice**-App so, dass sie Feedback von IoT Hub anfordert und empfängt.

1. Fügen Sie in Visual Studio im **SendCloudToDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.

   ```csharp
   private async static void ReceiveFeedbackAsync()
   {
        var feedbackReceiver = serviceClient.GetFeedbackReceiver();

        Console.WriteLine("\nReceiving c2d feedback from service");
        while (true)
        {
            var feedbackBatch = await feedbackReceiver.ReceiveAsync();
            if (feedbackBatch == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received feedback: {0}",
              string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
            Console.ResetColor();

            await feedbackReceiver.CompleteAsync(feedbackBatch);
        }
    }
    ```

    Beachten Sie, dass das Empfangsmuster mit dem Muster zum Empfangen von Cloud-zu-Gerät-Nachrichten von der Geräte-App identisch ist.

1. Fügen Sie in der Methode **Main** die folgende Zeile direkt nach `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` hinzu.

   ``` csharp
   ReceiveFeedbackAsync();
   ```

1. Um für die Übermittlung der C2D-Nachricht Feedback anzufordern, müssen Sie in der **SendCloudToDeviceMessageAsync** -Methode eine Eigenschaft angeben. Fügen Sie direkt nach der Zeile `var commandMessage = new Message(...);` die folgende Zeile hinzu.

   ``` csharp
   commandMessage.Ack = DeliveryAcknowledgement.Full;
   ```

1. Drücken Sie **F5**, um die Apps auszuführen. Alle drei Anwendungen sollten gestartet werden. Wählen Sie das Fenster **SendCloudToDevice** aus, und drücken Sie die **EINGABETASTE**. Die Nachricht sollte von der Geräte-App empfangen werden, und nach einigen Sekunden sollte die Feedbacknachricht von der **SendCloudToDevice**-Anwendung empfangen werden.

   ![Nachrichtenempfangs-App](./media/iot-hub-csharp-csharp-c2d/sendc2d2.png)

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren, z.B. exponentielles Backoff, wie es in [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults) vorgeschlagen wird.
>

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie gelernt, wie C2D-Nachrichten gesendet und empfangen werden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT-Solution Accelerator für die Remoteüberwachung](https://docs.microsoft.com/azure/iot-suite/).

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [Entwicklungsleitfaden für IoT Hub](iot-hub-devguide.md).
