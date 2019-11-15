---
title: Erstellen eines IoT Plug & Play Preview-Geräts (Vorschauversion) für die Zertifizierung | Microsoft-Dokumentation
description: Als Geräteentwickler erfahren Sie hier mehr darüber, wie Sie ein IoT Plug & Play Preview-Gerät für die Zertifizierung erstellen.
author: tbhagwat3
ms.author: tanmayb
ms.date: 06/28/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: e4dd5215812f0fd1a43afe0923601417bc8e6916
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569639"
---
# <a name="build-an-iot-plug-and-play-preview-device-thats-ready-for-certification"></a>Erstellen eines Azure IoT Plug & Play Preview-Geräts für die Zertifizierung

In diesem Tutorial erfahren Sie, wie Sie als Geräteentwickler ein IoT Plug & Play Preview-Gerät für die Zertifizierung erstellen.

Bei den Zertifizierungstests wird überprüft, ob:

- Ihr IoT Plug & Play-Gerätecode auf Ihrem Gerät installiert ist.
- Ihr IoT Plug & Play-Gerätecode mit dem Azure IoT SDK erstellt wurde.
- Ihr Gerätecode den [Azure IoT Hub Device Provisioning-Dienst](../iot-dps/about-iot-dps.md) unterstützt.
- Ihr Gerätecode die Schnittstelle „Geräteinformationen“ implementiert.
- das Funktionsmodell und der Gerätecode mit IoT Central kompatibel sind.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- [Visual Studio Code](https://code.visualstudio.com/download)
- Erweiterungspaket [Azure IoT Tools für VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

Außerdem benötigen Sie das IoT Plug & Play-Gerät, das Sie im [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines Geräts](quickstart-create-pnp-device.md) erstellt haben.

## <a name="store-a-capability-model-and-interfaces"></a>Speichern von Gerätefunktionsmodellen und Schnittstellen

Bei IoT Plug & Play-Geräten müssen Sie ein Funktionsmodell und Schnittstellen erstellen, die die Funktionen des Geräts als JSON-Dateien definieren.

Sie können diese JSON-Dateien an drei verschiedenen Speicherorten speichern:

- im öffentlichen Modellrepository
- im Modellrepository Ihres Unternehmens
- auf Ihrem Gerät

Zurzeit müssen die Dateien zur Zertifizierung Ihres Geräts entweder im Repository Ihres Unternehmensmodells oder im öffentlichen Modellrepository gespeichert werden.

## <a name="include-the-required-interfaces"></a>Einbinden der erforderlichen Schnittstellen

Um den Zertifizierungsprozess zu bestehen, müssen Sie die Schnittstelle **Geräteinformationen** in Ihr Funktionsmodell einbinden und implementieren. Diese Schnittstelle weist die folgende Identifikation auf:

```json
"@id": "urn:azureiot:DeviceManagement:DeviceInformation:1"
```

> [!NOTE]
> Wenn Sie den [Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines Geräts](quickstart-create-pnp-device.md)abgeschlossen haben, dann ist die Schnittstelle **Geräteinformationen** bereits in Ihr Modell eingebunden.

Fügen Sie zum Einbinden der Schnittstelle **Geräteinformationen** die Schnittstellen-ID der Eigenschaft `implements` des Funktionsmodells hinzu:

```json
{
  "@id": "urn:yourcompanyname:sample:ThermostatDevice:1",
  "@type": "CapabilityModel",
  "displayName": "Thermostat T-1000",
  "implements": [
    "urn:yourcompanyname:sample:Thermostat:1",
    "urn:azureiot:DeviceManagement:DeviceInformation:1"
  ],
  "@context": "http://azureiot.com/v1/contexts/CapabilityModel.json"
}
```

So zeigen Sie die Schnittstelle **Geräteinformationen** in VS Code an:

1. Öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette.

1. Geben Sie **Plug & Play** ein, und wählen Sie **IoT Plug & Play Open Model Repository** (IoT Plug & Play-Modellrepository öffnen) aus. Wählen Sie **Open Public Model Repository** (Öffentliches Modellrepository öffnen) aus. Das öffentliche Modellrepository wird in VS Code geöffnet.

1. Wählen Sie im öffentlichen Modellrepository die Registerkarte **Schnittstellen** aus, und geben Sie **Geräteinformationen** in das Feld „Filter“ ein.

1. Wählen Sie in der gefilterten Liste die Schnittstelle **Geräteinformationen** aus, um eine lokale Kopie davon zu erstellen, und wählen Sie dann **Download** aus. VS Code zeigt die Schnittstellendatei an.

Anzeigen der Schnittstelle **Geräteinformationen** mithilfe der Azure CLI:

1. [Installieren Sie die Erweiterung für die Azure IoT-Befehlszeilenschnittstelle (Azure CLI)](howto-install-pnp-cli.md).

1. Verwenden Sie den folgenden Azure CLI-Befehl, um eine Schnittstelle mit der ID der Schnittstelle „Geräteinformationen“ anzuzeigen:

    ```cmd/sh
    az iot pnp interface show --interface urn:azureiot:DeviceManagement:DeviceInformation:1
    ```

Weitere Informationen finden Sie unter [Installieren und Verwenden der Azure IoT-Erweiterung für Azure CLI](howto-install-pnp-cli.md).

## <a name="update-device-code"></a>Aktualisieren des Gerätecodes

### <a name="enable-device-provisioning-through-the-azure-iot-device-provisioning-service-dps"></a>Aktivieren der Gerätebereitstellung über Azure IoT Device Provisioning Service (DPS)

Zum Zertifizieren des Geräts müssen Sie die Gerätebereitstellung über [Azure IoT Device Provisioning Service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) aktivieren. Um die Verwendung von DPS zu aktivieren, können Sie in VS Code den C-Code-Stub generieren. Folgen Sie diesen Schritten:

1. Öffnen Sie den Ordner mit der DCM-Datei, und öffnen Sie mit **STRG+UMSCHALT+P** die Befehlspalette. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Generate Device Code Stub** (Gerätecode-Stub generieren) aus.

1. Wählen Sie die DCM-Datei aus, mit der Sie den Gerätecode-Stub generieren möchten.

1. Geben Sie den Namen Ihrer Geräteanwendung als Projektnamen ein.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie als Verbindungsmethode die Option **Via DPS (Device Provisioning Service) symmetric key** (Mit symmetrischem Schlüssel per Device Provisioning-Dienst) aus.

1. Wählen Sie abhängig vom Betriebssystem des Geräts **CMake Project on Windows** (CMake-Projekt unter Windows) oder **CMake Project on Linux** (CMake-Projekt unter Linux) als Projektvorlage aus.

1. VS Code öffnet ein neues Fenster mit generierten Gerätecode-Stubdateien.

1. Geben Sie nach dem Erstellen des Codes die DPS-Anmeldeinformationen (**DPS-ID-Bereich**, **symmetrischer DPS-Schlüssel**, **Geräte-ID**) als Parameter für die Anwendung ein. Informationen zum Ermitteln der Anmeldeinformationen im Zertifizierungsportal finden Sie unter [Verbinden und Testen Ihres IoT Plug & Play-Geräts](tutorial-certification-test.md#connect-and-discover-interfaces).

    ```cmd/sh
    .\your_pnp_app.exe [DPS ID Scope] [DPS symmetric key] [device ID]
    ```

### <a name="implement-standard-interfaces"></a>Implementieren von Standardschnittstellen

#### <a name="implement-the-model-information-and-sdk-information-interfaces"></a>Implementieren von Schnittstellen für Modellinformationen und SDK-Informationen

Das Azure IoT-Geräte-SDK implementiert die Schnittstellen für Modellinformationen und SDK-Informationen. Wenn Sie in VS Code die Funktion für die Codegenerierung verwenden, dann verwendet Ihr Gerätecode das IoT Plug & Play Device SDK.

Wenn Sie das Azure IoT-Geräte-SDK nicht verwenden möchten, können Sie den SDK-Quellcode als Referenz für Ihre eigene Implementierung verwenden.

#### <a name="implement-the-device-information-interface"></a>Implementieren der Schnittstelle „Geräteinformationen“

Implementieren Sie die Schnittstelle **Geräteinformationen** auf Ihrem Gerät, und stellen Sie zur Laufzeit gerätespezifische Informationen vom Gerät bereit.

Sie können eine Beispielimplementierung der Schnittstelle **Geräteinformationen** für [Linux](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) als Referenz verwenden.

### <a name="implement-all-the-capabilities-defined-in-your-model"></a>Implementieren aller in Ihrem Modell definierten Funktionen

Während der Zertifizierung wird das Gerät programmgesteuert getestet, um sicherzustellen, dass es die in seinen Schnittstellen definierten Funktionen implementiert. Reagieren Sie mit dem HTTP-Statuscode 501 auf die Lese-/Schreibeigenschaften und Befehlsanforderungen, wenn Ihr Gerät diese nicht implementiert.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie ein IoT Plug & Play-Gerät für die Zertifizierung erstellt haben, können Sie mit dem folgenden Schritt fortfahren:

> [!div class="nextstepaction"]
> [Zertifizieren Ihres Geräts](tutorial-certification-test.md)
