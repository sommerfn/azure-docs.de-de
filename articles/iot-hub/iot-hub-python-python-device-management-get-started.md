---
title: Erste Schritte mit der Azure IoT Hub-Geräteverwaltung (Python) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe der IoT Hub-Geräteverwaltung den Neustart eines Remotegeräts initiieren. Sie verwenden das Azure IoT SDK für Python, um eine simulierte Geräte-App mit einer direkten Methode und eine Dienst-App zum Aufrufen der direkten Methode zu implementieren.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: robinsh
ms.openlocfilehash: 287dbd3d6da4aa2bf5bd1da652cdeaeda3136321
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69907732"
---
# <a name="get-started-with-device-management-python"></a>Erste Schritte mit der Geräteverwaltung (Python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Verwenden des Azure-Portals zum Erstellen einer IoT Hub-Instanz und einer Geräteidentität im IoT Hub

* Erstellen Sie eine simulierte Geräte-App, die eine direkte Methode enthält, die das Gerät neu startet. Direkte Methoden werden aus der Cloud aufgerufen.

* Erstellen Sie eine Python-Konsolenanwendung, die über Ihren IoT Hub die direkte Methode zum Neustart in der simulierten Geräte-App aufruft.

Am Ende dieses Tutorials verfügen Sie über zwei Python-Konsolen-Apps:

* **dmpatterns_getstarted_device.py**: Stellt mit der zuvor erstellten Geräteidentität eine Verbindung mit dem IoT Hub her, empfängt eine direkte Methode zum Neustart, simuliert einen physischen Neustart und meldet den Zeitpunkt des letzten Neustarts.

* **dmpatterns_getstarted_service.py**: Ruft eine direkte Methode in der simulierten Geräte-App auf und zeigt die Antwort sowie die gemeldeten aktualisierten Eigenschaften an.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

Nachfolgend sind die Installationsanweisungen für die erforderlichen Komponenten aufgeführt.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registrieren eines neuen Geräts beim IoT-Hub

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App

In diesem Abschnitt führen Sie folgende Schritte aus:

* Erstellen einer Python-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet

* Simulieren eines Geräteneustarts

* Verwenden der gemeldeten Eigenschaften, um Gerätezwillingsabfragen zu ermöglichen, die Geräte und den Zeitpunkt ihres letzten Neustarts ermitteln

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iot-device-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

   > [!NOTE]
   > Die PIP-Pakete für „azure-iothub-service-client“ und „azure-iothub-device-client“ sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsverzeichnis eine Datei namens **dmpatterns_getstarted_device.py**.

3. Fügen Sie am Anfang der Datei `import`dmpatterns_getstarted_device.py**die folgenden**-Anweisungen ein.

    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

4. Fügen Sie Variablen (einschließlich der Variablen **VERBINDUNGSZEICHENFOLGE**) und die Client-Initialisierung hinzu.  Ersetzen Sie den Platzhalterwert `{deviceConnectionString}` durch Ihre Geräteverbindungszeichenfolge. Diese Verbindungszeichenfolge haben Sie zuvor unter [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) kopiert.  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

5. Fügen Sie die folgenden Funktionsrückrufe hinzu, um die direkte Methode auf dem Gerät zu implementieren.

    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS

        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS

        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
            time.sleep(20)

            print ( "Device rebooted." )

            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            print ( "Updating device twins: rebootTime" )

        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200

        return device_method_return_value
    ```

6. Starten Sie den Listener der direkten Methode, und warten Sie.

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

7. Speichern und schließen Sie die Datei **dmpatterns_getstarted_device.py**.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. exponentielles Backoff), wie es im Artikel [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults) vorgeschlagen wird.

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Auslösen eines Remoteneustarts auf dem Gerät über eine direkte Methode

In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die einen Remote-Neustart auf einem Gerät über eine direkte Methode auslöst. Die App verwendet Gerätezwillingsabfragen, um den Zeitpunkt des letzten Neustarts bei diesem Gerät zu ermitteln.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iot-service-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-service-client
    ```

   > [!NOTE]
   > Die PIP-Pakete für „azure-iothub-service-client“ und „azure-iothub-device-client“ sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsverzeichnis eine Datei namens **dmpatterns_getstarted_service.py**.

3. Fügen Sie am Anfang der Datei **dmpatterns_getstarted_service.py** die folgenden `import`-Anweisungen ein.

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

4. Fügen Sie die folgenden Variablendeklarationen hinzu. Ersetzen Sie den Platzhalterwert `{IoTHubConnectionString}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben. Ersetzen Sie den Platzhalterwert `{deviceId}` durch die Geräte-ID, die Sie unter [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) registriert haben.

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

5. Fügen Sie die folgende Funktion zum Aufrufen der Gerätemethode zum Neustart des Zielgeräts hinzu, fragen Sie dann die Gerätezwillinge ab, und rufen Sie den Zeitpunkt des letzten Neustarts ab.

    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )

            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

6. Speichern und schließen Sie die Datei **dmpatterns_getstarted_service.py**.

## <a name="run-the-apps"></a>Ausführen der Apps

Jetzt können Sie die Apps ausführen.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.

    ```cmd/sh
    python dmpatterns_getstarted_device.py
    ```

2. Führen Sie an einer anderen Eingabeaufforderung den folgenden Befehl aus, um den Remoteneustart und die Abfrage an den Gerätezwilling zum Suchen des letzten Neustartzeitpunkts auszulösen.

    ```cmd/sh
    python dmpatterns_getstarted_service.py
    ```

3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

   Das folgende Beispiel zeigt die Reaktion des Geräts auf die direkte Methode für den Neustart:

   ![Ausgabe der simulierten Geräte-App](./media/iot-hub-python-python-device-management-get-started/device.png)

   Das folgende Beispiel zeigt, wie der Dienst die direkte Methode für den Neustart aufruft und den Status des Gerätezwillings abfragt:

   ![Ausgabe des Diensts für die Neustartauslösung](./media/iot-hub-python-python-device-management-get-started/service.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]