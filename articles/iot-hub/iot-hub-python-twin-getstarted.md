---
title: Erste Schritte mit Azure IoT Hub-Gerätezwillingen (Python) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Tags hinzufügen und eine IoT Hub-Abfrage verwenden. Sie verwenden die Azure IoT SDKs für Python, um die simulierte Geräte-App und eine Dienst-App zu implementieren, die die Tags hinzufügt und die IoT Hub-Abfrage ausführt.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: robinsh
ms.openlocfilehash: 729371ede8df3dc21f70bc5742de52f2be874507
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030005"
---
# <a name="get-started-with-device-twins-python"></a>Erste Schritte mit Gerätezwillingen (Python)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Python-Konsolen-Apps:

* **AddTagsAndQuery.py**, eine Python-Back-End-App, die Tags hinzufügt und Gerätezwillinge abfragt.

* **ReportConnectivity.py** ist eine Python-Geräte-App, die ein Gerät simuliert, das eine Verbindung mit Ihrer IoT Hub-Instanz mit der zuvor erstellten Geräteidentität herstellt und seine Verbindungsbedingung meldet.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registrieren eines neuen Geräts beim IoT-Hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>Erstellen der Dienst-App

In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, mit der dem Gerätezwilling, der Ihrer **{Geräte-ID}** zugeordnet ist, Standortmetadaten hinzugefügt werden. Anschließend werden die Gerätezwillinge abgefragt, die in dem für die Auswahl der Geräte in Redmond zuständigen IoT Hub gespeichert sind, und dann diejenigen, die eine Mobilfunkverbindung melden.

1. Öffnen Sie in Ihrem Arbeitsverzeichnis eine Eingabeaufforderung, und installieren Sie das **Azure IoT Hub Service SDK für Python**.

   ```cmd/sh
   pip install azure-iothub-service-client
   ```

   > [!NOTE]
   > Das PIP-Paket für „azure-iothub-service-client“ sind zurzeit nur für das Betriebssystem Windows verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie in einem Text-Editor eine neue Datei namens **AddTagsAndQuery.py**.

3. Fügen Sie den folgenden Code hinzu, um die erforderlichen Module aus dem Dienst-SDK zu importieren:

   ```python
   import sys
   import iothub_service_client
   from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
   from iothub_service_client import IoTHubDeviceTwin, IoTHubError
   ```

4. Fügen Sie den folgenden Code hinzu: Ersetzen Sie `[IoTHub Connection String]` durch die IoT-Hub-Verbindungszeichenfolge, die Sie unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben. Ersetzen Sie `[Device Id]` durch die Geräte-ID, die Sie unter [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) registriert haben.
  
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

5. Fügen Sie den folgenden Code der Datei **AddTagsAndQuery.py** hinzu:

    ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

            iothub_registry_statistics = iothub_registry_manager.get_statistics()
            print ( "Total device count                       : {0}".format(iothub_registry_statistics.totalDeviceCount) )
            print ( "Enabled device count                     : {0}".format(iothub_registry_statistics.enabledDeviceCount) )
            print ( "Disabled device count                    : {0}".format(iothub_registry_statistics.disabledDeviceCount) )
            print ( "" )

            number_of_devices = iothub_registry_statistics.totalDeviceCount
            dev_list = iothub_registry_manager.get_device_list(number_of_devices)

            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            for device in range(0, number_of_devices):
                if dev_list[device].deviceId == DEVICE_ID:
                    twin_info = iothub_twin_method.update_twin(dev_list[device].deviceId, UPDATE_JSON)

            print ( "Devices in Redmond: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)

                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    print ( dev_list[device].deviceId )

            print ( "" )

            print ( "Devices in Redmond using cellular network: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)

                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    if twin_info.find(UPDATE_JSON_CLIENT_SEARCH) > -1:
                        print ( dev_list[device].deviceId )

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "IoTHub sample stopped" )
    ```

    Das **Registry**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Der Code initialisiert zuerst das **Registrierungs**-Objekt, aktualisiert dann den Gerätezwilling für **deviceId** und führt schließlich zwei Abfragen aus. Mit der ersten werden nur die Gerätezwillinge von Geräten in der Anlage **Redmond43** ausgewählt. Mit der zweiten wird die Abfrage so angepasst, dass nur die Geräte ausgewählt werden, die über ein Mobilfunknetz verbunden sind.

6. Fügen Sie am Ende der Datei **AddTagsAndQuery.py** den folgenden Code ein, um die Funktion **iothub_service_sample_run** zu implementieren:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

7. Führen Sie die Anwendung mit folgendem Befehl aus:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    In den Ergebnissen für die Abfrage, mit der alle Geräte in der Anlage **Redmond43** abgefragt werden, sollte ein Gerät angezeigt werden und keines für die Abfrage, mit der die Ergebnisse auf die über ein Mobilfunknetz verbundenen Geräte beschränkt werden.

    ![erste Abfrage zeigt alle Geräte in Redmond](./media/iot-hub-python-twin-getstarted/service-1.png)

Im nächsten Abschnitt erstellen Sie eine Geräte-App, mit der die Verbindungsinformationen gemeldet und das Ergebnis der im vorherigen Abschnitt ausgeführten Abfrage geändert wird.

## <a name="create-the-device-app"></a>Erstellen der Geräte-App

In diesem Abschnitt erstellen Sie eine Python-Konsolenanwendung, die als Ihre **{Geräte-ID}** eine Verbindung mit dem Hub herstellt und dann die gemeldeten Eigenschaften des zugehörigen Gerätezwillings so aktualisiert, dass sie die Informationen über die Verbindung mit einem Mobilfunknetz enthalten.

1. Installieren Sie über eine Eingabeaufforderung in Ihrem Arbeitsverzeichnis das **Azure IoT Hub Device SDK für Python**:

    ```cmd/sh
    pip install azure-iot-device
    ```

2. Erstellen Sie in einem Text-Editor eine neue Datei namens **ReportConnectivity.py**.

3. Fügen Sie den folgenden Code hinzu, um die erforderlichen Module aus dem Geräte-SDK zu importieren:

    ```python
    import time
    import threading
    from azure.iot.device import IoTHubModuleClient
    ```

4. Fügen Sie den folgenden Code hinzu: Ersetzen Sie den Platzhalterwert `[IoTHub Device Connection String]` durch die Geräteverbindungszeichenfolge, die Sie im Abschnitt [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) kopiert haben.

    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    ```

5. Fügen Sie folgenden Code der Datei **ReportConnectivity.py** hinzu, um die Gerätezwillingsfunktionalität zu implementieren:

    ```python
    def twin_update_listener(client):
        while True:
            patch = client.receive_twin_desired_properties_patch()  # blocking call
            print("Twin patch received:")
            print(patch)

    def iothub_client_init():
        client = IoTHubModuleClient.create_from_connection_string(CONNECTION_STRING)
        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            twin_update_listener_thread = threading.Thread(target=twin_update_listener, args=(client,))
            twin_update_listener_thread.daemon = True
            twin_update_listener_thread.start()

            # Send reported 
            print ( "Sending data as reported property..." )
            reported_patch = {"connectivity": "cellular"}
            client.patch_twin_reported_properties(reported_patch)
            print ( "Reported properties updated" )

            while True:
                time.sleep(1000000)
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```

    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code werden nach dem Initialisieren des **Client**-Objekts der Gerätezwilling für Ihr Gerät abgerufen und die zugehörige gemeldete Eigenschaft mit den Verbindungsinformationen aktualisiert.

6. Fügen Sie am Ende der Datei **ReportConnectivity.py** den folgenden Code ein, um die Funktion **iothub_client_sample_run** zu implementieren:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )
        print ( "IoTHubModuleClient waiting for commands, press Ctrl-C to exit" )

        iothub_client_sample_run()
    ```

7. Führen Sie die Geräte-App aus:

    ```cmd/sh
    python ReportConnectivity.py
    ```

    Es sollte die Bestätigung angezeigt werden, dass die Gerätezwillinge aktualisiert wurden.

    ![Zwillinge aktualisieren](./media/iot-hub-python-twin-getstarted/device-1.png)

8. Da das Gerät nun die Verbindungsinformationen gemeldet hat, sollten diese in beiden Abfragen angezeigt werden. Gehen Sie zurück, und führen Sie die Abfragen erneut aus:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    Nun sollte Ihre **{Geräte-ID}** in beiden Abfrageergebnisse angezeigt werden.

    ![zweite Abfrage](./media/iot-hub-python-twin-getstarted/service-2.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben Gerätemetadaten aus einer Back-End-App als Tags hinzugefügt und eine simulierte Geräte-App zum Melden von Geräteverbindungsinformationen im Gerätezwilling geschrieben. Sie haben zudem erfahren, wie diese Informationen mithilfe der Registrierung abgefragt werden können.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub](quickstart-send-telemetry-python.md).

* Konfigurieren von Geräten mit den gewünschten Eigenschaften des Gerätezwillings im Tutorial [Konfigurieren Ihrer Geräte über einen Back-End-Dienst](tutorial-device-twins.md).

* Interaktives Steuern von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App): [Schnellstart: Steuern eines mit einer IoT Hub-Instanz verbundenen Geräts (Node.js)](quickstart-control-device-python.md).
