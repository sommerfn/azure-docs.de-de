---
title: Planen von Aufträgen mit Azure IoT Hub (Python) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Azure IoT Hub-Auftrag planen, um eine direkte Methode auf mehreren Geräten aufzurufen. Sie verwenden die Azure IoT SDKs für Python, um die simulierten Geräte-Apps und eine Dienst-App für die Auftragsausführung zu implementieren.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: robinsh
ms.openlocfilehash: bc93dcea74f0660a4f56aec237eaae5a4e3eeb9a
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69904524"
---
# <a name="schedule-and-broadcast-jobs-python"></a>Planen und Übertragen von Aufträgen (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub ist ein vollständig verwalteter Dienst, mit dem eine Back-End-App Aufträge zur Planung und Aktualisierung von Millionen von Geräten erstellen und nachverfolgen kann.  Aufträge können für die folgenden Aktionen verwendet werden:

* Aktualisieren gewünschter Eigenschaften
* Aktualisieren von Tags
* Aufrufen direkter Methoden

Vom Konzept her umschließt ein Auftrag eine dieser Aktionen und verfolgt den Ausführungsfortschritt für eine Gruppe von Geräten nach, die anhand einer Gerätezwillingsabfrage definiert wird.  Mit einem Auftrag kann eine Back-End-App beispielsweise eine Neustartmethode auf 10.000 Geräten aufrufen – angegeben durch eine Gerätezwillingsabfrage und geplant für einen späteren Zeitpunkt.  Diese Anwendung kann dann den Fortschritt nachverfolgen, wenn diese Geräte jeweils die Neustartmethode empfangen und ausführen.

Weitere Informationen zu diesen Funktionen finden Sie in den folgenden Artikeln:

* Gerätezwillinge und Eigenschaften: [Erste Schritte mit Gerätezwillingen](iot-hub-python-twin-getstarted.md) und [Tutorial: Verwenden der Eigenschaften von Gerätezwillingen](tutorial-device-twins.md)

* Direkte Methoden: [Verstehen und Aufrufen direkter Methoden von IoT Hub](iot-hub-devguide-direct-methods.md) und [Tutorial: Verwenden von direkten Methoden](quickstart-control-device-python.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen einer simulierten Python-Geräte-App mit einer direkten Methode, um die Verwendung der **lockDoor**-Funktion zu ermöglichen, die vom Lösungs-Back-End aufgerufen werden kann

* Erstellen Sie eine Python-Konsolen-App, die die direkte **lockDoor**-Methode in der simulierten Geräte-App über einen Auftrag aufruft und die gewünschten Eigenschaften über einen Geräteauftrag aktualisiert.

Am Ende dieses Tutorials verfügen Sie über zwei Python-Apps:

**simDevice.py:** stellt mit der Geräteidentität eine Verbindung mit Ihrem IoT Hub her und empfängt eine direkte **lockDoor**-Methode.

**scheduleJobService.py:** ruft eine direkte Methode in der simulierten Geräte-App auf und aktualisiert die gewünschten Eigenschaften des Gerätezwillings per Auftrag.

> [!NOTE]
> Das **Azure IoT-SDK für Python** unterstützt die **Aufträge**-Funktion nicht direkt. Dieses Tutorial bietet stattdessen eine alternative Lösung mithilfe von asynchronen Threads und Timern. Weitere Updates finden Sie in der Featureliste des **Dienstclient-SDK** auf der Seite des [Azure IoT-SDK für Python](https://github.com/Azure/azure-iot-sdk-python).
>

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

Nachfolgend sind die Installationsanweisungen für die erforderlichen Komponenten aufgeführt.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registrieren eines neuen Geräts beim IoT-Hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet. Dadurch wird eine simulierte **lockDoor**-Methode ausgelöst.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iot-device-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

   > [!NOTE]
   > Die PIP-Pakete für „azure-iothub-service-client“ und „azure-iothub-device-client“ sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsverzeichnis die Datei **simDevice.py**.

3. Fügen Sie am Anfang der Datei **simDevice.py** die folgenden `import`-Anweisungen und Variablen hinzu. Ersetzen Sie `deviceConnectionString` durch die Verbindungszeichenfolge des Geräts, das Sie oben erstellt haben:

    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. Fügen Sie den folgenden Funktionsrückruf zur Behandlung der **lockDoor**-Methode hinzu:

    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

5. Fügen Sie einen weiteren Funktionsrückruf hinzu, um Updates von Gerätezwillingen zu behandeln:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

6. Fügen Sie den folgenden Code hinzu, um den Handler für die **lockDoor**-Methode zu registrieren. Schließen Sie auch die Routine `main` ein:

    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

            while True:
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
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

7. Speichern und schließen Sie die Datei **simDevice.py**.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. exponentielles Backoff), wie es im Artikel [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults) vorgeschlagen wird.
>

## <a name="get-the-iot-hub-connection-string"></a>Abrufen der IoT-Hub-Verbindungszeichenfolge

In diesem Artikel erstellen Sie einen Back-End-Dienst, der eine direkte Methode auf einem Gerät aufruft und den Gerätezwilling aktualisiert. Für den Dienst wird die Berechtigung **Dienstverbindung** benötigt, um eine direkte Methode auf einem Gerät aufzurufen. Darüber hinaus sind für den Dienst auch die Berechtigungen **Lesevorgänge in Registrierung** und **Schreibvorgänge in Registrierung** erforderlich, um Lese- und Schreibvorgänge für die Identitätsregistrierung durchführen zu können. Da keine SAS-Standardrichtlinie ausschließlich mit diesen Berechtigungen zur Verfügung steht, müssen Sie eine erstellen.

Erstellen Sie wie folgt eine SAS-Richtlinie, die die Berechtigungen **Dienstverbindung**, **Lesevorgänge in Registrierung** und **Schreibvorgänge in Registrierung** gewährt, und rufen Sie eine Verbindungszeichenfolge für diese Richtlinie ab:

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren IoT-Hub. Wählen Sie dazu am besten **Ressourcengruppen**, anschließend die Ressourcengruppe, in der sich Ihr IoT-Hub befindet, und dann in der Ressourcenliste diesen IoT-Hub aus.

2. Wählen Sie im linken Bereich Ihres IoT-Hubs **Freigegebene Zugriffsrichtlinien** aus.

3. Wählen Sie im Menü über der Richtlinienliste die Option **Hinzufügen** aus.

4. Geben Sie im Bereich **Richtlinie für den gemeinsamen Zugriff hinzufügen** einen aussagekräftigen Namen für Ihre Richtlinie ein (z. B. *serviceAndRegistryReadWrite*). Wählen Sie unter **Berechtigungen** die Optionen **Dienstverbindung** und **Schreibvorgänge in Registrierung** aus (**Lesevorgänge in Registrierung** wird bei Auswahl von **Schreibvorgänge in Registrierung** automatisch ausgewählt). Klicken Sie anschließend auf **Erstellen**.

    ![Hinzufügen einer neuen SAS-Richtlinie](./media/iot-hub-python-python-schedule-jobs/add-policy.png)

5. Wählen Sie im Bereich **SAS-Richtlinien** in der Richtlinienliste Ihre neue Richtlinie aus.

6. Wählen Sie unter **Schlüssel für gemeinsamen Zugriff** das Kopiersymbol für **Verbindungszeichenfolge – Primärschlüssel** aus, und speichern Sie den Wert.

    ![Abrufen der Verbindungszeichenfolge.](./media/iot-hub-python-python-schedule-jobs/get-connection-string.png)

Weitere Informationen zu SAS-Richtlinien und Berechtigungen für IoT-Hubs finden Sie unter [Access Control und Berechtigungen](./iot-hub-devguide-security.md#access-control-and-permissions).

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Planen von Aufträgen zum Aufrufen einer direkten Methode und Aktualisieren der Eigenschaften eines Gerätezwillings

In diesem Abschnitt erstellen Sie eine Python-Konsolen-App, mit der eine **lockDoor**-Remotefunktion auf einem Gerät mit einer direkten Methode initiiert wird, und aktualisieren die Eigenschaften des Gerätezwillings.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iot-service-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-service-client
    ```

   > [!NOTE]
   > Die PIP-Pakete für „azure-iothub-service-client“ und „azure-iothub-device-client“ sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) (Vorbereiten der Entwicklungsumgebung für Python).
   >

2. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsverzeichnis die Datei **scheduleJobService.py**.

3. Fügen Sie am Anfang der Datei **scheduleJobService.py** die folgenden `import`-Anweisungen und Variablen hinzu. Ersetzen Sie den Platzhalter `{IoTHubConnectionString}` durch die IoT-Hub-Verbindungszeichenfolge, die Sie zuvor unter [Abrufen der IoT-Hub-Verbindungszeichenfolge](#get-the-iot-hub-connection-string) kopiert haben. Ersetzen Sie den Platzhalter `{deviceId}` durch die Geräte-ID, die Sie unter [Registrieren eines neuen Geräts beim IoT-Hub](#register-a-new-device-in-the-iot-hub) registriert haben:

    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

4. Fügen Sie die folgende Funktion hinzu, mit der Geräte abgefragt werden:

    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)

        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

5. Fügen Sie die folgenden Methoden hinzu, um die Aufträge zum Aufrufen der direkten Methode und des Gerätezwillings auszuführen:

    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )

    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)

        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)

            print ( "" )
            print ( "Device twin updated." )
    ```

6. Fügen Sie den folgenden Code hinzu, um die Aufträge zu planen und den Auftragsstatus zu aktualisieren. Schließen Sie auch die Routine `main` ein:

    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()

            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )

            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()

            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )

            while True:
                print ( "" )

                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )

                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )

                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

7. Speichern und schließen Sie die Datei **scheduleJobService.py**.

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an der Eingabeaufforderung in Ihrem Arbeitsverzeichnis den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen:

    ```cmd/sh
    python simDevice.py
    ```

2. Führen Sie an einer weiteren Eingabeaufforderung in Ihrem Arbeitsverzeichnis den folgenden Befehl aus, um die Aufträge zum Absperren der Tür und zum Aktualisieren des Zwillings auszulösen:
  
    ```cmd/sh
    python scheduleJobService.py
    ```

3. Die Antworten des Geräts auf die Aktualisierungen der direkten Methode und der Gerätezwillinge wird in der Konsole angezeigt.

    ![IoT Hub-Auftrag Beispiel 1 – Geräteausgabe](./media/iot-hub-python-python-schedule-jobs/sample1-deviceoutput.png)

    ![IoT Hub-Auftrag Beispiel 2 – Geräteausgabe](./media/iot-hub-python-python-schedule-jobs/sample2-deviceoutput.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Auftrag zum Planen einer direkten Methode für ein Gerät und eines Updates der Eigenschaften eines Gerätezwillings verwendet.

Informationen zu den weiteren Schritten mit IoT Hub und Geräteverwaltungsmustern, z. B. drahtloses Firmware-Remoteupdate, finden Sie unter [Durchführen eines Firmwareupdates](tutorial-firmware-update.md).