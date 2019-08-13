---
title: Hochladen von Dateien von Geräten nach Azure IoT Hub mit Python | Microsoft-Dokumentation
description: Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für Python. Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: a529933cf4af572deacab1ae3c615ec0a0eca68f
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667865"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-python"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub (Python)

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

In diesem Artikel wird gezeigt, wie eine Datei mithilfe der [Dateiuploadfunktionen von IoT Hub](iot-hub-devguide-file-upload.md) in [Azure Blob Storage](../storage/index.yml) hochgeladen wird. Das Tutorial veranschaulicht folgende Vorgehensweisen:

* Sicheres Angeben eines Speichercontainers zum Hochladen einer Datei

* Hochladen einer Datei über Ihren IoT Hub mit dem Python-Client

Der Schnellstart [Senden von Telemetriedaten von einem Gerät an einen IoT-Hub](quickstart-send-telemetry-python.md) veranschaulicht die grundlegenden Gerät-zu-Cloud-Messagingfunktionen von IoT Hub. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

> [!NOTE]
> Das IoT Hub-Python-SDK unterstützt derzeit nur das Hochladen von zeichenbasierten Dateien wie z.B. **TXT**-Dateien.

Am Ende dieses Tutorials führen Sie die folgende Python-Konsolen-App aus:

* **FileUpload.py**, die eine Datei mit dem Python-Geräte-SDK in den Speicher hochlädt.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

Nachfolgend sind die Installationsanweisungen für die erforderlichen Komponenten aufgeführt.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt erstellen Sie die Geräte-App zum Hochladen einer Datei in IoT Hub.

1. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iothub-device-client** zu installieren:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

2. Erstellen Sie mit einem Text-Editor eine Testdatei, die Sie in den Blob Storage hochladen werden.

    > [!NOTE]
    > Das IoT Hub-Python-SDK unterstützt derzeit nur das Hochladen von zeichenbasierten Dateien wie z.B. **TXT**-Dateien.

3. Erstellen Sie mit einem Text-Editor in Ihrem Arbeitsordner die Datei **FileUpload.py**.

4. Fügen Sie am Anfang der Datei **FileUpload.py** die folgenden `import`-Anweisungen und Variablen hinzu. 

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "[Device Connection String]"
    PROTOCOL = IoTHubTransportProvider.HTTP

    PATHTOFILE = "[Full path to file]"
    FILENAME = "[File name for storage]"
    ```

5. Ersetzen Sie in Ihrer Datei `[Device Connection String]` durch die Verbindungszeichenfolge Ihres IoT Hub-Geräts. Ersetzen Sie `[Full path to file]` durch den Pfad zu der von Ihnen erstellten Testdatei oder der Datei auf Ihrem Gerät, die Sie hochladen möchten. Ersetzen Sie `[File name for storage]` durch den Namen, den Ihre Datei haben soll, nachdem sie in den Blob Storage hochgeladen wurde. 

6. Erstellen Sie einen Rückruf für die **upload_blob**-Funktion:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

7. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Client herzustellen und die Datei hochzuladen. Schließen Sie auch die Routine `main` ein:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )

            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            f = open(PATHTOFILE, "r")
            content = f.read()

            client.upload_blob_async(FILENAME, content, len(content), blob_upload_conf_callback, 0)

            print ( "" )
            print ( "File upload initiated..." )

            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )

    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

8. Speichern und schließen Sie die Datei **UploadFile.py**.

## <a name="run-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen.

1. Führen Sie an der Eingabeaufforderung in Ihrem Arbeitsordner den folgenden Befehl aus:

    ```cmd/sh
    python FileUpload.py
    ```

2. Der folgende Screenshot zeigt die Ausgabe der App **FileUpload**:

    ![Ausgabe der simulated-device-App](./media/iot-hub-python-python-file-upload/1.png)

3. Sie können das Verwaltungsportal verwenden, um die hochgeladene Datei im Speichercontainer anzuzeigen, den Sie konfiguriert haben:

    ![Hochgeladene Datei](./media/iot-hub-python-python-file-upload/2.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Erstellen einer IoT Hub-Instanz mithilfe einer Azure Resource Manager-Vorlage (PowerShell)](iot-hub-rm-template-powershell.md)

* [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md)

* [Azure IoT SDKs](iot-hub-devguide-sdks.md)
