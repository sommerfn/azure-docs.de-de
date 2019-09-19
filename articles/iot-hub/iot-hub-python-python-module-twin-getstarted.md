---
title: Erste Schritte mit der Modulidentität und dem Modulzwilling von Azure IoT Hub (Python) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit IoT-SDKs für Python eine Modulidentität erstellen und den Modulzwilling aktualisieren.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: menchi
ms.openlocfilehash: d9aeb5130f565c5fc24e2d0ced9c7ce16d2927ef
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71001160"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-python"></a>Erste Schritte mit der Modulidentität und dem Modulzwilling von IoT Hub (Python)

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Modulidentitäten und Modulzwillinge](iot-hub-devguide-module-twins.md) ähneln den Geräteidentitäten und Gerätezwillingen von Azure IoT Hub, ermöglichen jedoch eine feinere Granularität. Azure IoT Hub-Geräteidentitäten und -Gerätezwillinge ermöglichen der Back-End-Anwendung die Konfiguration eines Geräts und geben Aufschluss über den Gerätezustand. Modulidentitäten und Modulzwillinge bieten diese Funktionalität hingegen für einzelne Komponenten eines Geräts. Auf Geräten mit mehreren Komponenten (etwa auf betriebssystembasierten Geräten oder Firmwaregeräten) ermöglichen sie isolierte Konfigurationen und Zustände für die einzelnen Komponenten.
>

Am Ende dieses Tutorials verfügen Sie über zwei Python-Apps:

* **CreateIdentities**: Hiermit werden eine Geräteidentität, eine Modulidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden Ihrer Geräte- und Modulclients erstellt.

* **UpdateModuleTwinReportedProperties**: Hiermit werden aktualisierte vom Modulzwilling gemeldete Eigenschaften an Ihren IoT Hub sendet.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Erstellen einer Geräteidentität und einer Modulidentität in IoT Hub

In diesem Abschnitt erstellen Sie eine Python-App, mit der eine Geräte- und eine Modulidentität in der Identitätsregistrierung Ihrer IoT Hub-Identität erstellt werden. Ein Gerät oder Modul kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt zur Identitätsregistrierung des [IoT Hub-Entwicklerhandbuchs](iot-hub-devguide-identity-registry.md). Wenn Sie diese Konsolen-App ausführen, generiert sie eine eindeutige ID und einen eindeutigen Schlüssel für das Gerät und das Modul. Ihr Gerät und Ihr Modul verwenden diese Werte, um sich beim Senden von D2C-Nachrichten an IoT Hub zu identifizieren. Bei den IDs gilt Groß-/Kleinschreibung.

Fügen Sie Ihrer Python-Datei den folgenden Code hinzu:

```python
import sys
import iothub_service_client
from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubError

CONNECTION_STRING = "YourConnString"
DEVICE_ID = "myFirstDevice"
MODULE_ID = "myFirstModule"

try:
    # RegistryManager
    iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

    # CreateDevice
    primary_key = ""
    secondary_key = ""
    auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
    new_device = iothub_registry_manager.create_device(
        DEVICE_ID, primary_key, secondary_key, auth_method)
    print("new_device <" + DEVICE_ID +
          "> has primary key = " + new_device.primaryKey)

    # CreateModule
    new_module = iothub_registry_manager.create_module(
        DEVICE_ID, primary_key, secondary_key, MODULE_ID, auth_method)
    print("device/new_module <" + DEVICE_ID + "/" + MODULE_ID +
          "> has primary key = " + new_module.primaryKey)

except IoTHubError as iothub_error:
    print("Unexpected error {0}".format(iothub_error))
except KeyboardInterrupt:
    print("IoTHubRegistryManager sample stopped")
```

Diese App erstellt eine Geräteidentität mit der ID **myFirstDevice** und ein Identitätsmodul mit der ID **myFirstModule** unter dem Gerät **myFirstDevice**. (Falls diese Modul-ID in der Identitätsregistrierung bereits vorhanden ist, werden mit dem Code lediglich die vorhandenen Modulinformationen abgerufen.) Anschließend zeigt die App den Primärschlüssel für diese Identität an. Sie verwenden diesen Schlüssel in der simulierten Modul-App, um eine Verbindung mit Ihrem IoT Hub herzustellen.

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräte- und Modulidentitäten, um einen sicheren Zugriff auf IoT Hub zu ermöglichen. In der Identitätsregistrierung werden Geräte-IDs und -schlüssel für die Verwendung als Sicherheitsanmeldeinformationen gespeichert. Darüber hinaus wird in der Identitätsregistrierung ein Flag für den Aktivierungszustand des jeweiligen Geräts gespeichert, mit dem Sie den Zugriff für das betreffende Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Es gibt keinen Flag „Aktiviert/deaktiviert“ für Modulidentitäten. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch](iot-hub-devguide-identity-registry.md).
>

## <a name="update-the-module-twin-using-python-device-sdk"></a>Aktualisieren des Modulzwillings mithilfe des Python-Geräte-SDKs

In diesem Abschnitt erstellen Sie auf Ihrem simulierten Gerät eine Python-App, die die vom Modulzwilling gemeldeten Eigenschaften aktualisiert.

1. **Abrufen der Modulverbindungszeichenfolge**: Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Navigieren Sie zu Ihrer IoT Hub-Instanz, und klicken Sie auf „IoT-Geräte“. Suchen Sie nach „myFirstDevice“, und öffnen Sie den Eintrag. Sie sehen, dass „myFirstModule“ erfolgreich erstellt wurde. Kopieren Sie die Modulverbindungszeichenfolge. Sie wird im nächsten Schritt benötigt.

   ![Moduldetails im Azure-Portal](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

2. **Erstellen der App „UpdateModuleTwinReportedProperties“**

   Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubDeviceTwin, IoTHubError

    CONNECTION_STRING = "FILL IN CONNECTION STRING"
    DEVICE_ID = "MyFirstDevice"
    MODULE_ID = "MyFirstModule"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"telemetryInterval\":122}}}"

    try:
        iothub_twin = IoTHubDeviceTwin(CONNECTION_STRING)

        twin_info = iothub_twin.get_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Twin before update    :" )
        print ( "{0}".format(twin_info) )

        twin_info_updated = iothub_twin.update_twin(DEVICE_ID, MODULE_ID, UPDATE_JSON)
        print ( "" )
        print ( "Twin after update     :" )
        print ( "{0}".format(twin_info_updated) )

    except IoTHubError as iothub_error:
        print ( "Unexpected error {0}".format(iothub_error) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

In diesem Codebeispiel wird gezeigt, wie Sie den Modulzwilling abrufen und die gemeldeten Eigenschaften mit dem AMQP-Protokoll aktualisieren.

## <a name="get-updates-on-the-device-side"></a>Abrufen von Updates auf Geräteseite

Neben dem oben aufgeführten Code können Sie den nachstehenden Codeblock hinzufügen, um die Updatenachricht des Gerätezwillings auf Ihrem Gerät abzurufen.

```python
import time
import threading
from azure.iot.device import IoTHubModuleClient

CONNECTION_STRING = "{deviceConnectionString}"


def twin_update_listener(client):
    while True:
        patch = client.receive_twin_desired_properties_patch()  # blocking call
        print("")
        print("Twin desired properties patch received:")
        print(patch)

def iothub_client_sample_run():
    try:
        module_client = IoTHubModuleClient.create_from_connection_string(CONNECTION_STRING)

        twin_update_listener_thread = threading.Thread(target=twin_update_listener, args=(module_client,))
        twin_update_listener_thread.daemon = True
        twin_update_listener_thread.start()

        while True:
            time.sleep(1000000)

    except KeyboardInterrupt:
        print("IoTHubModuleClient sample stopped")


if __name__ == '__main__':
    print ( "Starting the IoT Hub Python sample..." )
    print ( "IoTHubModuleClient waiting for commands, press Ctrl-C to exit" )

    iothub_client_sample_run()
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Erste Schritte mit der Geräteverwaltung](iot-hub-node-node-device-management-get-started.md)

* [Erste Schritte mit IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)