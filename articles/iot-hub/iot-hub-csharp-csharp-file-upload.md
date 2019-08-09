---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit .NET | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für .NET. Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 07/04/2017
ms.author: robinsh
ms.openlocfilehash: 6e41b1999033c00b277cd35173b3247a727e9a8a
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668133"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-net"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub (.NET)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Dieses Tutorial baut auf dem Code im Tutorial [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)](iot-hub-csharp-csharp-c2d.md) auf, um zu zeigen, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen. Es beschreibt Folgendes:

* Sicheres Bereitstellen eines Geräts mit einem Azure-Blob-URI für das Hochladen einer Datei.

* Verwenden der IoT Hub-Dateihochlade-Benachrichtigungen zum Auslösen der Dateiverarbeitung in Ihrem App-Back-End.

Die Schnellstartanleitung [Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (C#)](quickstart-send-telemetry-dotnet.md) und das Tutorial [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)](iot-hub-csharp-csharp-c2d.md) veranschaulichen die grundlegenden Gerät-zu-Cloud- und Cloud-zu-Gerät-Messagingfunktionen von IoT Hub. Unter [Tutorial: Konfigurieren der Nachrichtenweiterleitung mit IoT Hub](tutorial-routing.md) wird eine Möglichkeit für das zuverlässige Speichern von Gerät-zu-Cloud-Nachrichten in Azure Blob Storage beschrieben. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiel:

* Große Dateien, die Bilder enthalten
* Videos
* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden
* Eine Form vorverarbeiteter Daten

Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory](../data-factory/introduction.md) oder dem [Hadoop](../hdinsight/index.yml)-Stapel verarbeitet. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

Am Ende dieses Tutorials führen Sie zwei .NET-Konsolen-Apps aus:

* **SimulatedDevice**, eine modifizierte Version der App, die im Tutorial [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)](iot-hub-csharp-csharp-c2d.md) erstellt wurde. Diese App lädt mithilfe eines SAS-URI, den Ihr IoT Hub bereitstellt, eine Datei in den Speicher hoch.

* **ReadFileUploadNotification**, die Benachrichtigungen zum Hochladen von Dateien von Ihrem IoT Hub empfängt.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, Java, Python und JavaScript). Im [Azure IoT Developer Center](https://azure.microsoft.com/develop/iot) finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

Für dieses Tutorial benötigen Sie Folgendes:

* Visual Studio

* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt ändern Sie die Geräte-App, die Sie in [Senden von Nachrichten aus der Cloud an das Gerät mit IoT Hub (.NET)](iot-hub-csharp-csharp-c2d.md) erstellt haben, um Cloud-zu-Gerät-Nachrichten vom IoT-Hub zu empfangen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt **SimulatedDevice**. Klicken Sie auf **Hinzufügen** und dann auf **Vorhandenes Element**. Navigieren Sie zu einer Bilddatei, und fügen Sie sie Ihrem Projekt hinzu. In diesem Tutorial wird davon ausgegangen, dass das Bild `image.jpg` heißt.

1. Klicken Sie mit der rechten Maustaste auf das Bild, und klicken Sie dann auf **Eigenschaften**. Stellen Sie sicher, dass **In Ausgabeverzeichnis kopieren** auf **Immer kopieren** festgelegt ist.

    ![Screenshot zum Aktualisieren der Bildeigenschaft für „In Ausgabeverzeichnis kopieren“](./media/iot-hub-csharp-csharp-file-upload/image-properties.png)

1. Fügen Sie zu Beginn der Datei **Program.cs** die folgenden Anweisungen hinzu:

    ```csharp
    using System.IO;
    ```

1. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Die `UploadToBlobAsync` -Methode verwendet den Dateinamen und die Datenstromquelle der hochzuladenden Datei und verarbeitet das Hochladen in Speicher. Die Konsolen-App zeigt den Zeitaufwand für das Hochladen der Datei an.

1. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. exponentielles Backoff), wie es im Artikel [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults) vorgeschlagen wird.

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

In diesem Artikel erstellen Sie einen Back-End-Dienst, um Dateiuploadbenachrichtigungen von dem IoT-Hub zu erhalten, den Sie unter [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-dotnet.md) erstellt haben. Damit Ihr Dienst Dateiuploadbenachrichtigungen empfangen kann, muss er über die Berechtigung **Dienstverbindung** verfügen. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **service** erstellt, die diese Berechtigung erteilt.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Erhalten einer Benachrichtigung zum Dateiupload

In diesem Abschnitt schreiben Sie eine .NET-Konsolen-App, die Uploadbenachrichtigungen von IoT Hub empfängt.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Windows-Projekt. Nennen Sie das Projekt **ReadFileUploadNotification**.

    ![Neues Projekt in Visual Studio](./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png)

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadFileUploadNotification**, und klicken Sie dann auf **NuGet-Pakete verwalten...** .

3. Suchen Sie im Fenster **NuGet-Paket-Manager** nach **Microsoft.Azure.Devices**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

    Diese Aktion lädt das [NuGet-Paket mit dem SDK des Azure IoT-Diensts](https://www.nuget.org/packages/Microsoft.Azure.Devices/) herunter, installiert es und fügt im **ReadFileUploadNotification**-Projekt einen Verweis darauf hinzu.

4. Fügen Sie zu Beginn der Datei **Program.cs** die folgenden Anweisungen hinzu:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

5. Fügen Sie der **Program** -Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert `{iot hub connection string}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload notification: {0}", 
              string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Beachten Sie, dass das Empfangsmuster mit dem Muster zum Empfangen von Cloud-zu-Gerät-Nachrichten von der Geräte-App identisch ist.

7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen**aus. Wählen Sie **Mehrere Startprojekte** und dann die Aktion **Starten** für **ReadFileUploadNotification** und **SimulatedDevice** aus.

2. Drücken Sie **F5** . Beide Anwendungen sollten gestartet werden. In der einen Konsolen-App sollte eine Nachricht zum Abschluss des Uploads und in der anderen eine Nachricht zum Empfang der Benachrichtigung zum Upload angezeigt werden. Sie können im [Azure-Portal](https://portal.azure.com/) oder in Visual Studio Server-Explorer überprüfen, ob die hochgeladene Datei in Ihrem Azure Storage-Konto vorhanden ist.

    ![Screenshot: Ausgabebildschirm](./media/iot-hub-csharp-csharp-file-upload/run-apps1.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)](iot-hub-rm-template-powershell.md)

* [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Bereitstellen von KI auf Edge-Geräten mit Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
