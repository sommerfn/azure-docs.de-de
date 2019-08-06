---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit Node | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für Node.js Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 06/28/2017
ms.openlocfilehash: f78f53f259234dc949ce5b18ccc7714b32e239f9
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404033"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Dieses Tutorial baut auf dem Code im Tutorial [Senden von C2D-Nachrichten mit IoT Hub (Java)](iot-hub-node-node-c2d.md) auf, um zu zeigen, wie Sie die [IoT Hub-Funktionen zum Hochladen von Dateien](iot-hub-devguide-file-upload.md) zum Hochladen einer Datei in [Azure Blob Storage](../storage/index.yml) nutzen. Das Tutorial veranschaulicht folgende Vorgehensweisen:

* Sicheres Bereitstellen eines Geräts mit einem Azure-Blob-URI für das Hochladen einer Datei.

* Verwenden der IoT Hub-Dateihochlade-Benachrichtigungen zum Auslösen der Dateiverarbeitung in Ihrem App-Back-End.

Die Schnellstartanleitung [Senden von Telemetriedaten von einem Gerät an einen IoT Hub](quickstart-send-telemetry-node.md) veranschaulicht die grundlegenden Gerät-zu-Cloud-Messagingfunktionen von IoT Hub. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiel:

* Große Dateien, die Bilder enthalten
* Videos
* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden
* Eine Form vorverarbeiteter Daten.

Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory](../data-factory/introduction.md) oder dem [Hadoop](../hdinsight/index.yml)-Stapel verarbeitet. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

Am Ende dieses Tutorials führen Sie zwei Node.js-Konsolen-Apps aus:

* **SimulatedDevice.js** lädt mithilfe eines SAS-URI, den Ihr IoT-Hub bereitstellt, eine Datei in den Speicher hoch.

* **ReadFileUploadNotification.js** empfängt Benachrichtigungen zum Hochladen von Dateien von Ihrem IoT-Hub.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, .NET, Javascript, Python und Java). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js, Version 10.0.x oder höher.

* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt erstellen Sie die Geräte-App zum Hochladen einer Datei in IoT Hub.

1. Erstellen Sie einen leeren Ordner mit dem Namen ```simulateddevice```.  Erstellen Sie im Ordner ```simulateddevice``` die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner ```simulateddevice``` den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

3. Erstellen Sie mit einem Text-Editor im Ordner ```simulateddevice``` die Datei **SimulatedDevice.js**.

4. Fügen Sie am Anfang der Datei **SimulatedDevice.js** die folgenden ```require```-Anweisungen hinzu:

    ```javascript
    'use strict';

    var fs = require('fs');
    var mqtt = require('azure-iot-device-mqtt').Mqtt;
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    ```

5. Fügen Sie die Variable `deviceconnectionstring` hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz.  Ersetzen Sie `{deviceconnectionstring}` durch den Namen des Geräts, das Sie im Abschnitt *Erstellen eines IoT Hubs* erstellt haben:

    ```javascript
    var connectionString = '{deviceconnectionstring}';
    var filename = 'myimage.png';
    ```

    > [!NOTE]
    > Der Einfachheit halber ist die Verbindungszeichenfolge im Code enthalten: Dies ist jedoch kein empfohlenes Verfahren. Abhängig von Ihrem Anwendungsfall und der Architektur sollten sie unter Umständen sicherere Methoden zum Speichern dieses Geheimnisses in Betracht ziehen.

6. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Client herzustellen:

    ```javascript
    var client = clientFromConnectionString(connectionString);
    console.log('Client connected');
    ```

7. Erstellen Sie einen Rückruf, und verwenden Sie die **uploadToBlob**-Funktion zum Hochladen der Datei.

    ```javascript
    fs.stat(filename, function (err, stats) {
        const rr = fs.createReadStream(filename);

        client.uploadToBlob(filename, rr, stats.size, function (err) {
            if (err) {
                console.error('Error uploading file: ' + err.toString());
            } else {
                console.log('File uploaded');
            }
        });
    });
    ```

8. Speichern und schließen Sie die Datei **SimulatedDevice.js** .

9. Kopieren Sie eine Bilddatei in den `simulateddevice`-Ordner und benennen Sie sie in `myimage.png` um.

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

In diesem Artikel erstellen Sie einen Back-End-Dienst, um Dateiuploadbenachrichtigungen von dem IoT-Hub zu erhalten, den Sie unter [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-node.md) erstellt haben. Damit Ihr Dienst Dateiuploadbenachrichtigungen empfangen kann, muss er über die Berechtigung **Dienstverbindung** verfügen. Standardmäßig wird jeder IoT-Hub mit einer SAS-Richtlinie namens **service** erstellt, die diese Berechtigung erteilt.

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="receive-a-file-upload-notification"></a>Erhalten einer Benachrichtigung zum Dateiupload

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die Dateiuploadbenachrichtigungen von IoT Hub empfängt.

Sie können diesen Abschnitt mit der Verbindungszeichenfolge **iothubowner** aus Ihrer IoT Hub-Instanz abschließen. Die Verbindungszeichenfolge finden Sie im [Azure-Portal](https://portal.azure.com/) auf dem Blatt **SAS-Richtlinie**.

1. Erstellen Sie einen leeren Ordner mit dem Namen ```fileuploadnotification```.  Erstellen Sie im Ordner ```fileuploadnotification``` die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:

    ```cmd/sh
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner ```fileuploadnotification``` den folgenden Befehl aus, um das SDK-Paket **azure-iothub** zu installieren:

    ```cmd/sh
    npm install azure-iothub --save
    ```

3. Erstellen Sie mit einem Text-Editor eine **FileUploadNotification.js**-Datei im Ordner `fileuploadnotification`.

4. Fügen Sie am Anfang der Datei **FileUploadNotification.js** die folgenden `require`-Anweisungen hinzu:

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die Variable `iothubconnectionstring` hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz.  Ersetzen Sie den Platzhalterwert `{iothubconnectionstring}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben:

    ```javascript
    var connectionString = '{iothubconnectionstring}';
    ```

    > [!NOTE]
    > Der Einfachheit halber ist die Verbindungszeichenfolge im Code enthalten: Dies ist jedoch kein empfohlenes Verfahren. Abhängig von Ihrem Anwendungsfall und der Architektur sollten sie unter Umständen sicherere Methoden zum Speichern dieses Geheimnisses in Betracht ziehen.

6. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Client herzustellen:

    ```javascript
    var serviceClient = Client.fromConnectionString(connectionString);
    ```

7. Öffnen Sie den Client, und verwenden Sie die **getFileNotificationReceiver**-Funktion zum Empfangen von Statusaktualisierungen.

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFileNotificationReceiver(function receiveFileUploadNotification(err, receiver){
          if (err) {
            console.error('error getting the file notification receiver: ' + err.toString());
          } else {
            receiver.on('message', function (msg) {
              console.log('File upload from device:')
              console.log(msg.getData().toString('utf-8'));
            });
          }
        });
      }
    });
    ```

8. Speichern und schließen Sie die Datei **FileUploadNotification.js**.

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

Führen Sie an der Eingabeaufforderung im `fileuploadnotification`-Ordner folgenden Befehl aus:

```cmd/sh
node FileUploadNotification.js
```

Führen Sie an der Eingabeaufforderung im `simulateddevice`-Ordner folgenden Befehl aus:

```cmd/sh
node SimulatedDevice.js
```

Der folgende Screenshot zeigt die Ausgabe der **SimulatedDevice**-App:

![Ausgabe der simulated-device-App](./media/iot-hub-node-node-file-upload/simulated-device.png)

Der folgende Screenshot zeigt die Ausgabe der **FileUploadNotification**-App:

![Ausgabe der read-file-upload-notification-App](./media/iot-hub-node-node-file-upload/read-file-upload-notification.png)

Sie können das Verwaltungsportal verwenden, um die hochgeladene Datei im Speichercontainer anzuzeigen, den Sie konfiguriert haben:

![Hochgeladene Datei](./media/iot-hub-node-node-file-upload/uploaded-file.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)](iot-hub-rm-template-powershell.md)

* [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)
