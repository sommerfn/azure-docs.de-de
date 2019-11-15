---
title: Erstellen und Testen eines IoT Plug & Play-Geräts (Vorschauversion) | Microsoft-Dokumentation
description: Für Geräteentwickler wird beschrieben, wie VS Code zum Erstellen und Testen eines neuen Gerätefunktionsmodells für ein IoT Plug & Play-Gerät (Vorschauversion) verwendet wird.
author: dominicbetts
ms.author: dobett
ms.date: 07/10/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: b7b9cd1040accda4d39af4d0a18940b56a45f929
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569885"
---
# <a name="tutorial-create-and-test-a-device-capability-model-using-visual-studio-code"></a>Tutorial: Erstellen und Testen eines Gerätefunktionsmodells mit Visual Studio Code

In diesem Tutorial wird beschrieben, wie Sie als Geräteentwickler Visual Studio Code verwenden, um ein _Gerätefunktionsmodell_ zu erstellen. Sie können das Modell nutzen, um ein Codegerüst zum Ausführen auf einem Gerät zu generieren, mit dem eine Verbindung mit einer Azure IoT Hub-Instanz in der Cloud hergestellt wird.

Im Abschnitt dieses Tutorials, in dem das Erstellen des generierten Codegerüsts beschrieben wird, wird vorausgesetzt, dass Sie Windows verwenden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Gerätefunktionsmodells
> * Generieren eines Gerätecodegerüsts aus dem Modell
> * Implementieren der Stubs im generierten Code
> * Ausführen des Codes zum Testen der Interaktionen mit einem IoT-Hub

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, um mit dem Gerätefunktionsmodell arbeiten zu können:

* [Visual Studio Code](https://code.visualstudio.com/download): VS Code ist für mehrere Plattformen verfügbar.
* Erweiterungspaket [Azure IoT Tools für VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools). Führen Sie die folgenden Schritte aus, um das Erweiterungspaket in VS Code zu installieren:

    1. Wählen Sie in VS Code die Registerkarte **Erweiterungen**.
    1. Suchen Sie nach **Azure IoT Tools**.
    1. Wählen Sie **Installieren** aus.

Zum Erstellen des generierten C-Codes unter Windows in diesem Tutorial benötigen Sie Folgendes:

* [Buildtools für Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) mit Workloads für **C++-Buildtools** und **NuGet-Paket-Manager-Komponenten**. Oder Sie haben bereits [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 oder 2015 mit denselben Workloads installiert.
* [Git-Client](https://git-scm.com/download)
* [CMake](https://cmake.org/download/)

Sie benötigen Folgendes, um Ihren Gerätecode in diesem Tutorial zu testen:

* [Azure IoT-Explorer](https://github.com/Azure/azure-iot-explorer/releases).
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="model-your-device"></a>Modellieren Ihres Geräts

Sie verwenden die _Digital Twin Definition Language_, um ein Gerätefunktionsmodell zu erstellen. Ein Modell besteht in der Regel aus mehreren _Schnittstellendefinitionsdateien_ und einer Modelldatei. **Azure IoT Tools für VS Code** enthält Tools, die Sie beim Erstellen und Bearbeiten dieser JSON-Dateien unterstützen.

### <a name="create-the-interface-file"></a>Erstellen der Schnittstellendatei

Erstellen Sie wie folgt eine Schnittstellendatei, mit der die Funktionen Ihres IoT-Geräts in VS Code definiert werden:

1. Erstellen Sie einen Ordner mit dem Namen **devicemodel**.

1. Starten Sie VS Code, und drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Schnittstelle erstellen**.

1. Navigieren Sie zum von Ihnen erstellten Ordner **devicemodel**, und wählen Sie ihn aus.

1. Geben Sie anschließend **EnvironmentalSensor** als Namen der Schnittstelle ein, und drücken Sie die **EINGABETASTE**. VS Code erstellt eine Beispielschnittstellendatei mit dem Namen **EnvironmentalSensor.interface.json**.

1. Ersetzen Sie den Inhalt dieser Datei durch den folgenden JSON-Code und `{your name}` im Feld `@id` durch einen eindeutigen Wert. Verwenden Sie nur die Zeichen a - z, A - Z, 0 - 9 und Unterstrich. Weitere Informationen finden Sie unter [Digital Twin Identifier Format](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format) (Bezeichnerformat für digitale Zwillinge). Die Schnittstellen-ID muss für die Speicherung der Schnittstelle im Repository eindeutig sein:

    ```json
    {
      "@id": "urn:{your name}:EnvironmentalSensor:1",
      "@type": "Interface",
      "displayName": "Environmental Sensor",
      "description": "Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
      "comment": "Requires temperature and humidity sensors.",
      "contents": [
        {
          "@type": "Property",
          "displayName": "Device State",
          "description": "The state of the device. Two states online/offline are available.",
          "name": "state",
          "schema": "boolean"
        },
        {
          "@type": "Property",
          "displayName": "Customer Name",
          "description": "The name of the customer currently operating the device.",
          "name": "name",
          "schema": "string",
          "writable": true
        },
        {
          "@type": "Property",
          "displayName": "Brightness Level",
          "description": "The brightness level for the light on the device. Can be specified as 1 (high), 2 (medium), 3 (low)",
          "name": "brightness",
          "writable": true,
          "schema": "long"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Temperature"
          ],
          "description": "Current temperature on the device",
          "displayName": "Temperature",
          "name": "temp",
          "schema": "double",
          "unit": "Units/Temperature/fahrenheit"
        },
        {
          "@type": [
            "Telemetry",
            "SemanticType/Humidity"
          ],
          "description": "Current humidity on the device",
          "displayName": "Humidity",
          "name": "humid",
          "schema": "double",
          "unit": "Units/Humidity/percent"
        },
        {
          "@type": "Telemetry",
          "name": "magnetometer",
          "displayName": "Magnetometer",
          "comment": "This shows a complex telemetry that contains a magnetometer reading.",
          "schema": {
            "@type": "Object",
            "fields": [
              {
                "name": "x",
                "schema": "integer"
              },
              {
                "name": "y",
                "schema": "integer"
              },
              {
                "name": "z",
                "schema": "integer"
              }
            ]
          }
        },
        {
          "@type": "Command",
          "name": "turnon",
          "response": {
            "name": "turnon",
            "schema": "string"
          },
          "comment": "This Commands will turn-on the LED light on the device.",
          "commandType": "synchronous"
        },
        {
          "@type": "Command",
          "name": "turnoff",
          "comment": "This Commands will turn-off the LED light on the device.",
          "response": {
            "name": "turnoff",
            "schema": "string"
          },
          "commandType": "synchronous"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
    }
    ```

    Mit dieser Schnittstelle werden Geräteeigenschaften (z. B. **Kundenname**), Telemetriedatentypen (z. B.**Temperatur**) und Befehle (z. B.**turnon**) definiert.

1. Fügen Sie am Ende dieser Schnittstellendatei eine Befehlsfunktion mit dem Namen **blink** hinzu. Achten Sie darauf, dass Sie vor dem Befehl ein Komma hinzufügen. Versuchen Sie, die Definition einzugeben, um zu ermitteln, wie Sie von IntelliSense, AutoVervollständigen und der Überprüfung beim Bearbeiten einer Schnittstellendefinition unterstützt werden:

    ```json
    {
      "@type": "Command",
      "description": "This command will begin blinking the LED for given time interval.",
      "name": "blink",
      "request": {
        "name": "blinkRequest",
        "schema": {
          "@type": "Object",
          "fields": [
            {
              "name": "interval",
              "schema": "long"
            }
          ]
        }
      },
      "response": {
        "name": "blinkResponse",
        "schema": "string"
      },
      "commandType": "synchronous"
    }
    ```

1. Speichern Sie die Datei .

### <a name="create-the-model-file"></a>Erstellen der Modelldatei

Die Modelldatei gibt die Schnittstellen an, die von Ihrem IoT Plug & Play-Gerät implementiert werden. Normalerweise enthält ein Modell mindestens zwei Schnittstellen: eine oder mehr, mit denen die spezifischen Funktionen Ihres Geräts definiert werden, und eine Standardschnittstelle, die von allen IoT Plug & Play-Geräten implementiert werden muss.

Erstellen Sie wie folgt eine Modelldatei, mit der die Schnittstellen angegeben werden, die von Ihrem IoT Plug & Play-Gerät in VS Code implementiert werden:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Funktionsmodell erstellen**. Geben Sie anschließend **SensorboxModel** als Namen des Modells ein. VS Code erstellt eine Beispielschnittstellendatei mit dem Namen **SensorboxModel.capabilitymodel.json**.

1. Ersetzen Sie den Inhalt dieser Datei durch den folgenden JSON-Code, und ersetzen Sie `{your name}` im Feld `@id` und in der Schnittstelle `EnvironmentalSensor` durch den gleichen Wert, den Sie in der Datei **EnvironmentalSensor.interface.json** verwendet haben. Die Schnittstellen-ID muss für die Speicherung der Schnittstelle im Repository eindeutig sein:

    ```json
    {
      "@id": "urn:{your name}:SensorboxModel:1",
      "@type": "CapabilityModel",
      "displayName": "Environmental Sensorbox Model",
      "implements": [
        {
          "schema": "urn:{your name}:EnvironmentalSensor:1",
          "name": "environmentalSensor"
        },
        {
          "schema": "urn:azureiot:DeviceManagement:DeviceInformation:1",
          "name": "deviceinfo"
        }
      ],
      "@context": "http://azureiot.com/v1/contexts/CapabilityModel.json"
    }
    ```

    Mit dem Modell wird ein Gerät definiert, mit dem Ihre Schnittstelle **EnvironmentalSensor** und die Standardschnittstelle **DeviceInformation** implementiert werden.

1. Speichern Sie die Datei .

### <a name="download-the-deviceinformation-interface"></a>Herunterladen der Schnittstelle „DeviceInformation“

Bevor Sie aus dem Modell ein Codegerüst generieren können, müssen Sie aus dem *öffentlichen Modellrepository* eine lokale Kopie von **DeviceInformation** erstellen. Das öffentliche Modellrepository enthält bereits die Schnittstelle **DeviceInformation**.

Laden Sie die Schnittstelle **DeviceInformation** mit VS Code wie folgt aus dem öffentlichen Modellrepository herunter:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, wählen Sie den Befehl **Open Model Repository** (Modellrepository öffnen), und wählen Sie anschließend **Open Public Model Repository** (Öffentliches Modellrepository öffnen).

1. Wählen Sie **Schnittstellen** und dann die Geräteinformationsschnittstelle mit der ID `urn:azureiot:DeviceManagement:DeviceInformation:1`. Wählen Sie anschließend **Herunterladen**.

Sie verfügen jetzt über die drei Dateien, aus denen Ihr Gerätefunktionsmodell besteht:

* urn_azureiot_DeviceManagement_DeviceInformation_1.interface.json
* EnvironmentalSensor.interface.json
* SensorboxModel.capabilitymodel.json

## <a name="publish-the-model"></a>Veröffentlichen des Modells

Damit das Tool Azure IoT-Explorer Ihr Gerätefunktionsmodell lesen kann, müssen Sie es in Ihrem Unternehmensrepository veröffentlichen. Zum Veröffentlichen aus VS Code benötigen Sie die Verbindungszeichenfolge für das Unternehmensrepository:

1. Navigieren Sie zum [Azure Certified for IoT-Portal](https://aka.ms/ACFI).

1. Verwenden Sie Ihr Microsoft-_Geschäftskonto_, um sich beim Portal anzumelden.

1. Wählen Sie die Option **Company repository** (Unternehmensrepository) und dann **Verbindungszeichenfolgen**.

1. Kopieren Sie die Verbindungszeichenfolge.

Öffnen Sie Ihr Unternehmensrepository in VS Code wie folgt:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Open Model Repository** (IoT Plug & Play: Modellrepository öffnen).

1. Wählen Sie **Open Organizational Model Repository** (Modellrepository der Organisation öffnen), und fügen Sie Ihre Verbindungszeichenfolge ein.

1. Drücken Sie die **EINGABETASTE**, um Ihr Unternehmensrepository zu öffnen.

Veröffentlichen Sie Ihr Gerätefunktionsmodell und die Schnittstellen für Ihr Unternehmensrepository:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Submit files to Model Repository** (IoT Plug & Play: Dateien an Modellrepository übermitteln).

1. Wählen Sie die Dateien **EnvironmentalSensor.interface.json** und **SensorboxModel.capabilitymodel.json** und dann **OK**.

Ihre Dateien sind jetzt in Ihrem Unternehmensrepository gespeichert.

## <a name="generate-code"></a>Generieren von Code

Sie können **Azure IoT Tools für VS Code** verwenden, um ein C-Codegerüst aus Ihrem Modell zu generieren. Generieren Sie das Codegerüst in VS Code wie folgt:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie dann den Befehl **IoT Plug & Play: Generate Device Code Stub** (IoT Plug & Play: Gerätecode-Stub generieren).

1. Wählen Sie Ihre Funktionsmodelldatei **SensorboxModel.capabilitymodel.json** aus.

1. Geben Sie **sensorbox_app** als Projektnamen ein.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie **Via IoT Hub device connection string** (Über IoT Hub-Geräteverbindungszeichenfolge) als Verbindungsmethode aus.

1. Wählen Sie **CMake Project on Windows** (CMake-Projekt unter Windows) als Projektvorlage aus.

1. Wählen Sie **Via Vcpkg** (Über Vcpkg) als Methode zum Einschließen des Geräte-SDK aus.

VS Code generiert das C-Codegerüst und speichert die Dateien im Ordner **sensorbox_app** des Ordners **modelcode**. VS Code öffnet ein neues Fenster, das die generierten Codedateien enthält.

## <a name="update-the-generated-code"></a>Aktualisieren des generierten Codes

Bevor Sie den Code erstellen und ausführen können, müssen Sie die Eigenschaften, Telemetriedaten und Befehle mit Stubausführung implementieren.

Stellen Sie wie folgt Implementierungen für den Code mit Stubausführung in VS Code bereit:

1. Öffnen Sie die Datei **SensorboxModel_impl.c**.

1. Fügen Sie Code hinzu, um die Funktionen mit Stubausführung zu implementieren.

1. Speichern Sie die Änderungen.

## <a name="build-the-code"></a>Erstellen des Codes

Bevor Sie den Code zum Testen Ihres IoT Plug & Play-Geräts mit einer Azure IoT Hub-Instanz ausführen, müssen Sie ihn kompilieren.

Befolgen Sie die Anleitung in der Datei **Readme.md** im Ordner **sensorbox_app**, um den Code in Windows zu erstellen und auszuführen. Der folgende Abschnitt enthält eine Anleitung zum Abrufen einer Geräteverbindungszeichenfolge, die beim Ausführen des Gerätecodes verwendet wird.

## <a name="test-the-code"></a>Testen des Codes

Wenn Sie den Code ausführen, wird eine Verbindung mit IoT Hub hergestellt und mit dem Senden von Beispieltelemetriedaten und Eigenschaftswerten begonnen. Das Gerät antwortet auch auf Befehle, die von IoT Hub gesendet werden. Überprüfen Sie dieses Verhalten wie folgt:

1. Erstellen Sie wie folgt einen IoT-Hub:

    ```azurecli-interactive
    az group create --name environmentalsensorresources --location centralus
    az iot hub create --name {your iot hub name} \
      --resource-group environmentalsensorresources --sku F1
    ```

1. Fügen Sie Ihrem IoT-Hub ein Gerät hinzu, und rufen Sie die zugehörige Verbindungszeichenfolge ab:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {your iot hub name} --device-id MyPnPDevice
    az iot hub device-identity show-connection-string --hub-name {your iot hub name} --device-id MyPnPDevice --output table
    ```

    Notieren Sie sich die Verbindungszeichenfolge.

1. Navigieren Sie an einer Eingabeaufforderung zum Ordner **azure-iot-sdk-c**, in dem Sie das SDK und die Beispiele erstellt haben. Navigieren Sie dann zum Ordner **cmake\\sensorbox_app\\Release**.

1. Führen Sie den folgenden Befehl aus:

    ```cmd
    sensorbox_app.exe {your device connection string}
    ```

1. Verwenden Sie das Tool Azure IoT-Explorer zum Interagieren mit dem IoT Plug & Play-Gerät, das mit Ihrem IoT-Hub verbunden ist. Weitere Informationen finden Sie unter [Install and use Azure IoT Explorer](./howto-install-iot-explorer.md) (Installieren und Verwenden von Azure IoT-Explorer).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem folgenden Artikel fort, nachdem Sie ein IoT Plug & Play-Gerät erstellt haben, das bereit für die Zertifizierung ist:

> [!div class="nextstepaction"]
> [Erstellen eines zertifizierungsbereiten Geräts](tutorial-build-device-certification.md)
