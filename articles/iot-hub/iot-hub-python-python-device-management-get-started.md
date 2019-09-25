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
ms.openlocfilehash: eb5085db10c5763a4173f460eabde6afcccd5aff
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71000443"
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

## <a name="prerequisites"></a>Voraussetzungen

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

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iot-device** zu installieren:

    ```cmd/sh
    pip install azure-iot-device
    ```

   > [!NOTE]
   > Die PIP-Pakete für „azure-iothub-service-client“ sind nur für das Betriebssystem Windows verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsverzeichnis eine Datei namens **dmpatterns_getstarted_device.py**.

3. Fügen Sie am Anfang der Datei `import`dmpatterns_getstarted_device.py**die folgenden**-Anweisungen ein.

    ```python
    import threading
    import time
    import datetime
    from azure.iot.device import IoTHubDeviceClient, MethodResponse
    ```

4. Fügen Sie die Variable **CONNECTION_STRING** hinzu. Ersetzen Sie den Platzhalterwert `{deviceConnectionString}` durch Ihre Geräteverbindungszeichenfolge. Diese Verbindungszeichenfolge haben Sie zuvor unter [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) kopiert.  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

5. Fügen Sie die folgenden Funktionsrückrufe hinzu, um die direkte Methode auf dem Gerät zu implementieren.

    ```python
    def reboot_listener(client):
        while True:
            # Receive the direct method request
            method_request = client.receive_method_request("rebootDevice")  # blocking call

            # Act on the method by rebooting the device...
            print( "Rebooting device" )
            time.sleep(20)
            print( "Device rebooted")

            # ...and patching the reported properties
            current_time = str(datetime.datetime.now())
            reported_props = {"rebootTime": current_time}
            client.patch_twin_reported_properties(reported_props)
            print( "Device twins updated with latest rebootTime")

            # Send a method response indicating the method request was resolved
            resp_status = 200
            resp_payload = {"Response": "This is the response from the device"}
            method_response = MethodResponse(method_request.request_id, resp_status, resp_payload)
            client.send_method_response(method_response)
    ```

6. Starten Sie den Listener der direkten Methode, und warten Sie.

    ```python
    def iothub_client_init():
        client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)
        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            # Start a thread listening for "rebootDevice" direct method invocations
            reboot_listener_thread = threading.Thread(target=reboot_listener, args=(client,))
            reboot_listener_thread.daemon = True
            reboot_listener_thread.start()

            while True:
                time.sleep(1000)

        except KeyboardInterrupt:
            print ( "IoTHubDeviceClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "IoTHubDeviceClient waiting for commands, press Ctrl-C to exit" )

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