---
title: 'Tutorial: Hinzufügen eines echten Geräts zu einer Azure IoT Central-Anwendung'
description: 'Tutorial: Erfahren Sie, wie Sie Ihrer Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und dieses konfigurieren. Sie können sich auch über den Code in Ihrem echten Gerät informieren.'
author: sandeeppujar
ms.author: sandeepu
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 5234c371cc4a7238148915f4dcca1faac564f608
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74108977"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Tutorial: Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

In diesem Tutorial erfahren Sie, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein *echtes Gerät* hinzufügen und das Gerät konfigurieren. In diesem Tutorial codieren Sie das echte Gerät mithilfe von Node.js und führen den Code auf einem Desktopcomputer aus. Sie benötigen für dieses Tutorial kein separates IoT-Gerät wie ein Raspberry Pi- oder MXChip IoT DevKit-Gerät.

Das Tutorial besteht aus zwei Teilen:

* Der erste Teil richtet sich an Bediener und erläutert, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein echtes Gerät hinzufügen und das Gerät konfigurieren. Am Ende dieses Teils rufen Sie eine Verbindungszeichenfolge für den zweiten Teil ab.
* Der zweite Teil richtet sich an Geräteentwickler und enthält Informationen zum Code auf Ihrem echten Gerät. Dem Beispielcode wird die Verbindungszeichenfolge aus dem ersten Teil hinzugefügt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen echten Geräts
> * Konfigurieren des echten Geräts
> * Abrufen der Verbindungszeichenfolge für das echte Gerät aus der Anwendung
> * Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung
> * Konfigurieren des Clientcodes für das echte Gerät

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, muss der Ersteller mindestens das erste Tutorial für Ersteller absolvieren, um die Azure IoT Central-Anwendung zu erstellen: [Definieren eines neuen Gerätetyps](tutorial-define-device-type.md) (obligatorisch)

Installieren Sie [Node.js](https://nodejs.org/) (Version 8.0.0 oder höher) auf Ihrem Entwicklungscomputer. Sie können `node --version` in der Befehlszeile ausführen, um Ihre Version zu überprüfen. Node.js ist für eine Vielzahl von Betriebssystemen verfügbar.

## <a name="add-a-real-device"></a>Hinzufügen eines echten Geräts

Um Ihrer Anwendung ein echtes Gerät hinzuzufügen, verwenden Sie die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage), die Sie im Tutorial [Definieren eines neuen Gerätetyps](tutorial-define-device-type.md) erstellt haben.

1. Wählen Sie im linken Bereich **Geräte** aus, um ein neues Gerät als Bediener hinzuzufügen:

   ![Seite „Device Explorer“ mit verbundener Klimaanlage](media/tutorial-add-device/explorer.png)

   Unter **Device Explorer** werden die Gerätevorlage für die **verbundene Klimaanlage** und ein simuliertes Gerät angezeigt. IoT Central erstellt automatisch ein simuliertes Gerät, wenn Sie eine Gerätevorlage erstellen.

2. Die Gerätevorlage für die **verbundene Klimaanlage** ist die im **Geräte-Explorer** ausgewählte Vorlage. Wählen Sie **+** und anschließend **Real** (Echt) aus, um damit zu beginnen, eine Verbindung mit einer echten Klimaanlage herzustellen, die diese Vorlage verwendet:

   ![Beginnen mit dem Herstellen einer Verbindung mit einer echten verbundenen Klimaanlage](media/tutorial-add-device/newreal.png)

3. Geben Sie die **Geräte-ID** (in Kleinbuchstaben) ein, oder verwenden Sie den vorgeschlagenen Wert. Sie können auch einen **Gerätenamen** für Ihr neues Gerät eingeben und **Erstellen** auswählen.

   ![Umbenennen des Geräts](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Konfigurieren eines echten Geräts

Das echte Gerät wird auf der Grundlage der Gerätevorlage **Connected Air Conditioner** erstellt. Sie können Ihr Gerät mithilfe von **Einstellungen** konfigurieren und Eigenschaftswerte zur Erfassung von Geräteinformationen festlegen.

1. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung **Set Temperature** (Sollwerttemperatur) den Status **no update** (Keine Aktualisierung) besitzt. Dieser Zustand ändert sich erst, wenn das echte Gerät eine Verbindung mit der Anwendung herstellt und bestätigt, dass auf die Einstellung reagiert wurde.

    ![Einstellungen mit Synchronisierungsangabe](media/tutorial-add-device/settingssyncing.png)

2. Auf der Seite **Eigenschaften** Ihres neuen, echten Geräts können Sie den Standort und das Datum der letzten Wartung bearbeiten. Die Felder für Seriennummer und Firmwareversion sind leer, bis das Gerät mit der Anwendung verbunden wird. Die Werte sind schreibgeschützt. Sie werden vom Gerät gesendet und können nicht bearbeitet werden.

    ![Geräteeigenschaften für das echte Gerät](media/tutorial-add-device/setproperties1.png)

3. Sie können die Seiten **Measurements** (Messungen), **Regeln** und **Dashboard** für Ihr echtes Gerät anzeigen.

## <a name="prepare-the-client-code"></a>Vorbereiten des Clientcodes

Der Beispielcode in diesem Artikel ist in [Node.js](https://nodejs.org/) geschrieben und zeigt genügend Code, damit ein Gerät folgende Aktionen ausführen kann:

* Herstellen einer Verbindung mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten zur Temperatur als verbundene Klimaanlage
* Senden von Geräteeigenschaften an Ihre Azure IoT Central-Anwendung
* Reagieren auf einen Bediener, der die Einstellung **Set Temperature** (Sollwerttemperatur) verwendet
* Behandeln des Echo-Befehls aus Ihrer Azure IoT Central-Anwendung

Die Artikel im Abschnitt [Nächste Schritte](#next-steps) enthalten weitere vollständige Beispiele und veranschaulichen die Verwendung anderer Programmiersprachen. Weitere Informationen zur Verbindungsherstellung zwischen Geräten und Azure IoT Central finden Sie im Artikel [Device connectivity in Azure IoT Central](concepts-connectivity.md) (Gerätekonnektivität in Azure IoT Central).

Die folgenden Schritte zeigen, wie Sie das Beispiel für [Node.js](https://nodejs.org/) vorbereiten:

### <a name="get-the-device-connection-information"></a>Abrufen der Geräteverbindungsinformationen

1. Die Verbindungszeichenfolge für eine Geräteinstanz in Ihrer Anwendung wird auf der Grundlage von Geräteinformationen generiert, die von IoT Central bereitgestellt werden.

   Klicken Sie auf dem Gerätebildschirm für Ihre echte verbundene Klimaanlage auf **Verbinden**.

   ![Geräteseite mit Link zum Anzeigen der Verbindungsinformationen](media/tutorial-add-device/connectionlink.png)

1. Notieren Sie sich auf der Seite **Geräteverbindung** die **Bereichs-ID**, die **Geräte-ID** und den **Primärschlüssel**. Diese Werte werden im weiteren Verlauf dieses Tutorials benötigt.

   ![Verbindungsdetails](media/tutorial-add-device/device-connect.png)

### <a name="prepare-the-nodejs-project"></a>Vorbereiten des Node.js-Projekts

1. Erstellen Sie auf Ihrem Entwicklungscomputer den Ordner `connectedairconditioner`.

1. Navigieren Sie in der Befehlszeilenumgebung zum erstellten Ordner `connectedairconditioner`.

1. Führen Sie zum Initialisieren Ihres Node.js-Projekts den folgenden Befehl aus, und behalten Sie dabei alle Standardwerte bei:

    ```cmd/sh
    npm init
      ```

1. Führen Sie zum Installieren der erforderlichen Pakete den folgenden Befehl aus:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt azure-iot-provisioning-device-mqtt azure-iot-security-symmetric-key --save
    ```

1. Erstellen Sie im Ordner `connectedairconditioner` mit einem Text-Editor eine Datei namens **ConnectedAirConditioner.js**.

1. Fügen Sie am Anfang der Datei **ConnectedAirConditioner.js** die folgenden `require`-Anweisungen hinzu:

    ```javascript
    'use strict';

    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var SymmetricKeySecurityClient = require('azure-iot-security-symmetric-key').SymmetricKeySecurityClient;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    ```

1. Fügen Sie der Datei folgende Variablendeklarationen hinzu. Ersetzen Sie die Platzhalter `{your Scope ID}`, `{your Device ID}` und `{your Primary Key}` durch die Geräteverbindungsinformationen, die Sie zuvor notiert haben:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var idScope = '{your Scope ID}';
    var registrationId = '{your Device ID}';
    var symmetricKey = '{your Primary Key}';
    var provisioningSecurityClient = new SymmetricKeySecurityClient(registrationId, symmetricKey);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;
    var targetTemperature = 0;
    ```

1. Speichern Sie die bisherigen Änderungen, lassen Sie die Datei aber noch geöffnet.

## <a name="review-client-code"></a>Überprüfen des Clientcodes

Im vorherigen Abschnitt haben Sie ein provisorisches Node.js-Projekt für eine Geräteanwendung erstellt, die eine Verbindung mit Ihrer Azure IoT Central-Anwendung herstellt. Im nächsten Schritt wird Code hinzugefügt, um Folgendes zu ermöglichen:

* Herstellen einer Verbindung mit Ihrer Azure IoT Central-Anwendung
* Senden von Telemetriedaten an Ihre Azure IoT Central-Anwendung
* Senden von Geräteeigenschaften an Ihre Azure IoT Central-Anwendung
* Empfangen von Einstellungen von Ihrer Azure IoT Central-Anwendung
* Behandeln des Echo-Befehls aus Ihrer Azure IoT Central-Anwendung

1. Fügen Sie der Datei **ConnectedAirConditioner.js** den folgenden Code hinzu, um temperaturbezogene Telemetriedaten an Ihre Azure IoT Central-Anwendung zu senden:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      hubClient.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    Der im JSON-Format gesendete Name des Felds muss dem Namen des Felds entsprechen, den Sie in Ihrer Gerätevorlage für die temperaturbezogenen Telemetriedaten angegeben haben. In diesem Beispiel lautet der Name des Felds **temperature**.

1. Fügen Sie zum Senden von Geräteeigenschaften wie **firmwareVersion** und **serialNumber** die folgende Definition hinzu:

    ```javascript
    // Send device properties
    function sendDeviceProperties(twin) {
      var properties = {
        firmwareVersion: "9.75",
        serialNumber: "10001"
      };
      twin.properties.reported.update(properties, (errorMessage) =>
      console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }
    ```

1. Fügen Sie die folgende Definition hinzu, um die Einstellungen zu definieren, die Ihr Gerät unterstützt (beispielsweise **setTemperature**):

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Fügen Sie für die Behandlung der von Azure IoT Central gesendeten Einstellungen die folgende Funktion hinzu, die den geeigneten Gerätecode sucht und ausführt:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

    Diese Funktion bewirkt Folgendes:

    * Sie überwacht, ob Azure IoT Central eine gewünschte Eigenschaft sendet.
    * Sie sucht die geeignete aufzurufende Funktion, um die Einstellungsänderung zu behandeln.
    * Sie gibt eine Bestätigung an Ihre Azure IoT Central-Anwendung zurück.

1. Fügen Sie die folgende Definition hinzu, um auf einen Befehl aus Ihrer Azure IoT Central-Anwendung (beispielsweise **Echo**) zu reagieren:

    ```javascript
    // Respond to the echo command
    function onCommandEcho(request, response) {
      // Display console info
      console.log(' * Echo command received');
      // Respond
      response.send(10, 'Success', function (errorMessage) {});
    }
    ```

1. Fügen Sie den folgenden Code hinzu, um die Verbindungsherstellung mit Azure IoT Central abzuschließen und die Funktionen im Clientcode einzubinden:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        hubClient.onDeviceMethod('echo', onCommandEcho);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        hubClient.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);

            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };
    ```

1. Registrieren des Geräts und Verbinden des Geräts mit Ihrer IoT Central-Anwendung:

    ```javascript
    // Start the device (connect it to Azure IoT Central).
    provisioningClient.register((err, result) => {
      if (err) {
        console.log("error registering device: " + err);
      } else {
        console.log('registration succeeded');
        console.log('assigned hub=' + result.assignedHub);
        console.log('deviceId=' + result.deviceId);
        var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';SharedAccessKey=' + symmetricKey;
        hubClient = Client.fromConnectionString(connectionString, iotHubTransport);

        hubClient.open(connectCallback);
      }
    });
    ```

1. Speichern Sie die Änderungen, die Sie vorgenommen haben.

## <a name="run-the-client-code"></a>Ausführen des Clientcodes

Nun können Sie den Clientcode ausführen und sehen, wie er mit ihrer IoT Central-Anwendung interagiert:

1. Geben Sie zum Ausführen des Beispiels den folgenden Befehl in der Befehlszeilenumgebung ein:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Achten Sie beim Ausführen dieses Befehls darauf, dass Sie sich im Ordner `connectedairconditioner` befinden.

1. Die Anwendung zeigt eine Ausgabe in der Konsole an:

   ![Ausgabe der Clientanwendung](media/tutorial-add-device/output.png)

1. Nach etwa 30 Sekunden werden die Telemetriedaten auf der Seite **Measurements** (Messungen) des Geräts angezeigt:

   ![Echte Telemetriedaten](media/tutorial-add-device/realtelemetry.png)

1. Auf der Seite **Einstellungen** sehen Sie, dass die Einstellung jetzt synchronisiert ist. Bei der erstmaligen Verbindungsherstellung hat das Gerät den Einstellungswert empfangen und die Änderung bestätigt:

   ![Einstellung synchronisiert](media/tutorial-add-device/settingsynced.png)

1. Legen Sie auf der Seite **Einstellungen** die Gerätetemperatur auf **95** fest, und klicken Sie auf **Update device** (Gerät aktualisieren). Ihre Beispielanwendung empfängt und verarbeitet diese Änderung:

   ![Empfangen und Verarbeiten der Einstellung](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > Dabei werden zwei entsprechende Nachrichten angezeigt: eine, wenn der Status `pending` gesendet wird, und eine, wenn der Status `completed` gesendet wird.

1. Auf der Seite **Measurements** (Messungen) sehen Sie, dass das Gerät höhere Temperaturwerte sendet:

    ![Temperaturbezogene Telemetriedaten sind nun höher.](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="nextstepaction"]
> * Hinzufügen eines neuen echten Geräts
> * Konfigurieren des neuen Geräts
> * Abrufen der Verbindungszeichenfolge für das echte Gerät aus der Anwendung
> * Nachvollziehen der Zuordnung zwischen Clientcode und Anwendung
> * Konfigurieren des Clientcodes für das echte Gerät

Nachdem Sie nun eine Verbindung zwischen einem echten Gerät und Ihrer Azure IoT Central-Anwendung hergestellt haben, schlagen wir Ihnen die folgenden weiteren Schritte vor.

Bediener:

* [Verwalten von Geräten](howto-manage-devices.md)
* [Verwenden von Gerätegruppen](howto-use-device-sets.md)
* [Erstellen benutzerdefinierter Analysen](howto-use-device-sets.md)

Geräteentwickler:

* [Vorbereiten und Verbinden eines DevKit-Geräts (C)](howto-connect-devkit.md)
* [Vorbereiten und Verbinden eines Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Vorbereiten und Verbinden eines Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Vorbereiten und Verbinden eines Windows 10 IoT Core-Geräts (C#)](howto-connect-windowsiotcore.md)
* [Verbinden eines generischen Node.js-Clients mit Ihrer Azure IoT Central-Anwendung](howto-connect-nodejs.md)
