---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist | Microsoft-Dokumentation
description: Verwenden Sie Node.js zum Herstellen einer Verbindung und zum Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 246d3eac8f9d8aff6d603ea8686e430ce0f772ea
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881589"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution"></a>Schnellstart: Interagieren mit einem IoT Plug & Play Preview-Gerät, das mit ihrer Azure IoT-Lösung verbunden ist

IoT Plug & Play Preview vereinfacht das IoT, indem es Ihnen die Interaktion mit den Funktionen eines Geräts ohne Kenntnisse der vorliegenden Geräteimplementierung ermöglicht. In diesem Schnellstart erfahren Sie, wie Sie mit Node.js eine Verbindung mit einem IoT Plug & Play-Gerät, das mit Ihrer Lösung verbunden ist, herstellen und dieses steuern.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen Node.js über [nodejs.org](https://nodejs.org) herunterladen und installieren.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>Vorbereiten eines IoT-Hubs

Für diesen Schnellstart benötigen Sie außerdem eine Azure IoT Hub-Instanz in Ihrem Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Fügen Sie die Microsoft Azure IoT-Erweiterung für die Azure CLI hinzu:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Führen Sie den folgenden Befehl aus, um die Geräteidentität in Ihrem IoT-Hub zu erstellen. Ersetzen Sie **YourIoTHubName** und **YourDevice** durch Ihre entsprechenden Namen. Falls Sie nicht über eine IoT Hub-Instanz verfügen, dient Ihnen [diese Anleitung](../iot-hub/iot-hub-create-using-cli.md) als Hilfe bei der Erstellung:

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Führen Sie die folgenden Befehle aus, um die _Geräte-Verbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Führen Sie die folgenden Befehle aus, um die _IoT Hub-Verbindungszeichenfolge_ für Ihren Hub auszuführen:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="connect-your-device"></a>Verbinden Ihres Geräts

In diesem Schnellstart verwenden Sie einen Beispielumgebungssensor, der in Node.js als IoT Plug & Play-Gerät geschrieben wird. In den folgenden Anweisungen erfahren Sie, wie Sie das Gerät installieren und ausführen:

1. Klonen Sie das GitHub-Repository:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Rufen Sie im Terminal das Stammverzeichnis Ihres geklonten Repositorys auf, navigieren Sie zum Ordner **/azure-iot-samples-node/digital-twins/Quickstarts/Device**, und installieren Sie alle Abhängigkeiten, indem Sie folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie die _Geräte-Verbindungszeichenfolge_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<your device connection string>
    ```

1. Führen Sie das Beispiel mit dem folgenden Befehl aus:

    ```cmd/sh
    node sample_device.js
    ```

1. Ihnen werden Meldungen angezeigt, dass das Gerät Telemetriedaten und seine Eigenschaften gesendet hat. Das Gerät ist für den Empfang von Befehlen und Eigenschaftsaktualisierungen bereit. Schließen Sie dieses Terminal nicht. Sie benötigen es später, um zu prüfen, ob die Dienstbeispiele funktionieren.

## <a name="build-the-solution"></a>Erstellen der Lösung

In diesem Schnellstart verwenden Sie eine Beispiel-IoT-Lösung in Node.js, um mit dem Beispielgerät zu interagieren.

1. Öffnen Sie ein anderes Terminal. Wechseln Sie zum Ordner Ihres geklonten Repositorys, und navigieren Sie zum Ordner **/azure-iot-samples-node/digital-twins/Quickstarts/Service**. Installieren Sie alle Abhängigkeiten, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    npm install
    ```

1. Konfigurieren Sie die _Hub-Verbindungszeichenfolge_:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<your hub connection string>
    ```

### <a name="read-a-property"></a>Lesen einer Eigenschaft

1. Wenn Sie das Gerät im Terminal verbinden, wird die folgende Meldung angezeigt:

    ```cmd/sh
    reported state property as online
    ```

1. Öffnen Sie die Datei **get_digital_twin.js**. Ersetzen Sie `deviceID` durch Ihre Geräte-ID, und speichern Sie die Datei.

1. Wechseln Sie zu dem Terminal, das Sie zum Ausführen des Dienstbeispiels geöffnet haben, und führen Sie den folgenden Befehl aus:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. In der Ausgabe wird unter der Komponente _environmentalSensor_ der gleiche Status gemeldet:

    ```JSON
    reported state property as online
    ```

### <a name="update-a-writable-property"></a>Aktualisieren einer schreibbaren Eigenschaft

1. Öffnen Sie die Datei **update_digital_twin_property.js**.

1. Am Anfang der Datei finden Sie eine Reihe von Konstanten, die mit Platzhaltern in Großbuchstaben definiert sind. Ersetzen Sie **deviceID** durch Ihre tatsächliche Geräte-ID, aktualisieren Sie die Konstanten mit den folgenden Werten, und speichern Sie die Datei:

    ```javascript
    const componentName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 60;
    ```

1. Wechseln Sie zu dem Terminal, das Sie zum Ausführen des Dienstbeispiels geöffnet haben, und führen Sie das Beispiel mit dem folgenden Befehl aus:

    ```cmd/sh
    node update_digital_twin_property.js
    ```

1. Im Terminal werden Informationen zum digitalen Zwilling angezeigt, die Ihrem Gerät zugeordnet sind. Bei der Komponente _environmentalSensor_ sehen Sie den neuen Wert für Helligkeit: 60.

    ```json
        "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
          "brightness": {
            "reported": {
              "value": 60,
              "desiredState": {
                "code": 200,
                "version": 14,
                "description": "helpful descriptive text"
              }
            },
            "desired": {
              "value": 60
            }
          },
          "state": {
            "reported": {
              "value": "online"
            }
          }
        }
      }
    ```

1. In Ihrem _Geräteterminal_ sehen Sie, dass das Gerät die Aktualisierung erhalten hat:

    ```cmd/sh
    Received an update for brightness: 60
    updated the property
    ```
2. Kehren Sie zu Ihrem _Dienstterminal_ zurück, und führen Sie den folgenden Befehl erneut aus, um zu überprüfen, ob die Eigenschaft aktualisiert wurde.
    
    ```cmd/sh
    node get_digital_twin.js
    ```
3. In der Ausgabe wird unter der Komponente „environmentalSensor“ der aktualisierte Helligkeitswert gemeldet. Hinweis: Es kann eine Weile dauern, bis das Gerät die Aktualisierung abgeschlossen hat. Sie können diesen Schritt wiederholen, bis das Gerät die Aktualisierung der Eigenschaft tatsächlich verarbeitet hat.
    
    ```json
      "brightness": {
        "reported": {
          "value": 60,
          }
       }
    ```

### <a name="invoke-a-command"></a>Aufrufen eines Befehls

1. Öffnen Sie die Datei **invoke_command.js**.

1. Ersetzen Sie am Anfang der Datei `deviceID` durch Ihre tatsächliche Geräte-ID. Aktualisieren Sie die Konstanten mit den folgenden Werten, und speichern Sie dann die Datei:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    ```

1. Wechseln Sie zu dem Terminal, das Sie zum Ausführen des Dienstbeispiels geöffnet haben. Verwenden Sie den folgenden Befehl zum Ausführen des Beispiels:

    ```cmd/sh
    node invoke_command.js
    ```

1. Ein erfolgreicher Vorgang sieht im Terminal wie die folgende Ausgabe aus:

    ```cmd/sh
    invoking command blink on component environmentalSensor for device test...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "33e536d3-14f7-4105-88f3-629b9933851c",
      "_response": "helpful response text"
    }
    ```

1. Im _Geräteterminal_ sehen Sie, dass der Befehl bestätigt wurde:

    ```cmd/sh
    received command: blink for component: environmentalSensor
    acknowledgement succeeded.
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit weiteren Artikeln fortfahren möchten, können Sie die Ressourcen behalten, die Sie in diesem Schnellstart verwendet haben. Andernfalls können Sie die Ressourcen löschen, die Sie für diesen Schnellstart erstellt haben, um zusätzliche Gebühren zu vermeiden.

Führen Sie mit der Azure CLI die folgenden Schritte aus, um den Hub und das registrierte Gerät zu löschen:

```azurecli-interactive
az group delete --name <Your group name>
```

Führen Sie mit der Azure CLI die folgenden Schritte aus, um nur das mit Ihrer IoT Hub-Instanz registrierte Gerät zu löschen:

```azurecli-interactive
az iot hub device-identity delete --hub-name [YourIoTHubName] --device-id [YourDevice]
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einer IoT-Lösung verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
