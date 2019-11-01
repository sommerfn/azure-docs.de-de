---
title: Kommunizieren mit einer Geräteanwendung in C über Azure IoT Hub-Gerätestreams (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Schnellstart führen Sie eine geräteseitige C-Anwendung aus, die über einen Gerätestream mit einem IoT-Gerät kommuniziert.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/20/2019
ms.author: robinsh
ms.openlocfilehash: b0a23486d81ac6b48bf7bb256e3583973a338002
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900918"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Schnellstart: Kommunizieren mit einer Geräteanwendung in C über IoT Hub-Gerätestreams (Vorschauversion)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub unterstützt derzeit Gerätestreams als [Previewfunktion](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Über [IoT Hub-Gerätestreams](iot-hub-device-streams-overview.md) können Dienst- und Geräteanwendungen sicher und firewallfreundlich kommunizieren. Während der Public Preview-Phase unterstützt das C SDK nur geräteseitige Gerätestreams. Aus diesem Grund wird in dieser Schnellstartanleitung nur das Ausführen der geräteseitigen Anwendung erläutert. Informationen zum Ausführen einer entsprechenden dienstseitigen Anwendung finden Sie in den folgenden Artikeln:

* [Kommunizieren mit Geräte-Apps in C# über IoT Hub-Gerätestreams](./quickstart-device-streams-echo-csharp.md)

* [Schnellstart: Kommunizieren mit einer Geräteanwendung in Node.js über IoT Hub-Gerätestreams (Vorschauversion)](./quickstart-device-streams-echo-nodejs.md)

Die geräteseitige C-Anwendung in dieser Schnellstartanleitung hat folgende Funktionen:

* Einrichten eines Gerätestreams mit einem IoT-Gerät

* Empfangen der von der dienstseitigen Anwendung gesendeten Daten und Zurückgeben dieser Daten

Der Code veranschaulicht den Initiierungsprozess eines Gerätestreams sowie das Verwenden dieses Streams zum Senden und Empfangen von Daten.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein:

* Installieren Sie [Visual Studio 2019](https://www.visualstudio.com/vs/) mit der aktivierten Workload **Desktopentwicklung mit C++** .

* Installieren Sie die aktuelle Version von [Git](https://git-scm.com/download/).

* Führen Sie den folgenden Befehl aus, um Ihrer Cloud Shell-Instanz die Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle hinzuzufügen. Die IoT-Erweiterung fügt der Azure-Befehlszeilenschnittstelle spezifische Befehle für IoT Hub, IoT Edge und IoT Device Provisioning Service (DPS) hinzu.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

Die Vorschau der Gerätestreams wird derzeit nur für IoT-Hubs unterstützt, die in folgenden Regionen erstellt werden:

  * USA (Mitte)
  * USA, Mitte (EUAP)
  * Nordeuropa
  * Asien, Südosten

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Für diesen Schnellstart verwenden Sie das [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md). Sie bereiten eine Entwicklungsumgebung vor, die zum Klonen und Erstellen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) aus GitHub verwendet wird. Das SDK auf GitHub enthält den in dieser Schnellstartanleitung verwendeten Beispielcode.

   > [!NOTE]
   > Vergewissern Sie sich vor diesem Verfahren, dass Visual Studio mit der Workload **Desktopentwicklung mit C++** installiert ist.

1. Installieren Sie das [CMake-Buildsystem](https://cmake.org/download/), wie auf der Downloadseite beschrieben.

1. Öffnen Sie eine Eingabeaufforderung oder die Git Bash-Shell. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:

    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Dieser Vorgang dauert einige Minuten.

1. Erstellen Sie wie im folgenden Befehl gezeigt ein *cmake*-Verzeichnis im Stammverzeichnis des Git-Repositorys, und navigieren Sie dann zu diesem Ordner:

    ```cmd
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Erstellen Sie mithilfe der folgenden Befehle aus dem Verzeichnis *cmake* eine spezifische SDK-Version für Ihre Entwicklungsclientplattform.

   * Linux:

      ```bash
      cmake ..
      make -j
      ```

   * Öffnen Sie unter Windows eine [Developer-Eingabeaufforderung für Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs). Führen Sie den Befehl für Ihre Version von Visual Studio aus. In dieser Schnellstartanleitung wird Visual Studio 2019 verwendet. Mit diesen Befehlen wird im Verzeichnis *cmake* eine Visual Studio-Projektmappe für das simulierte Gerät erstellt.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Or for VS2019
      cmake .. -G "Visual Studio 16 2019"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Sie müssen ein Gerät bei Ihrem IoT-Hub registrieren, damit eine Verbindung hergestellt werden kann. In diesem Abschnitt verwenden Sie Azure Cloud Shell mit der [IoT-Erweiterung](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest), um ein simuliertes Gerät zu registrieren.

1. Führen Sie in Cloud Shell den folgenden Befehl aus, um die Geräteidentität zu erstellen:

   > [!NOTE]
   > * Ersetzen Sie den Platzhalter *YourIoTHubName* durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.
   > * Verwenden Sie für den Namen des Geräts, das Sie registrieren, am besten *MyDevice*, wie bereits gezeigt. Wenn Sie für Ihr Gerät einen anderen Namen auswählen, verwenden Sie diesen im gesamten Artikel, und aktualisieren Sie den Gerätenamen in den Beispielanwendungen, bevor Sie sie ausführen.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

1. Führen Sie den folgenden Befehl in Cloud Shell aus, um die *Geräteverbindungszeichenfolge* für das soeben registrierte Gerät abzurufen:

   > [!NOTE]
   > Ersetzen Sie den Platzhalter *YourIoTHubName* durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDevice --output table
    ```

    Notieren Sie sich die zurückgegebene Verbindungszeichenfolge des Geräts zur späteren Verwendung in dieser Schnellstartanleitung. Dies sieht in etwa wie im folgenden Beispiel aus:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Kommunizieren zwischen Gerät und Dienst über Gerätestreams

In diesem Abschnitt führen Sie die geräteseitige Anwendung und die dienstseitige Anwendung aus und stellen eine Kommunikation zwischen beiden her.

### <a name="run-the-device-side-application"></a>Ausführen der geräteseitigen Anwendung

Gehen Sie zum Ausführen der geräteseitigen Anwendung folgendermaßen vor:

1. Geben Sie Ihre Geräteanmeldeinformationen an, indem Sie die Quelldatei **iothub_client_c2d_streaming_sample.c** im Ordner `iothub_client/samples/iothub_client_c2d_streaming_sample` bearbeiten und Ihre Geräteverbindungszeichenfolge angeben.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "{DeviceConnectionString}";
   ```

1. Kompilieren Sie den Code mit den folgenden Befehlen aus:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

1. Führen Sie das kompilierte Programm aus:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Ausführen der dienstseitigen Anwendung

Wie bereits erwähnt, unterstützt das IoT Hub C SDK nur geräteseitige Gerätestreams. Wenn Sie die zugehörige dienstseitige Anwendung erstellen und ausführen möchten, befolgen Sie die Anweisungen in den folgenden Schnellstartanleitungen:

* [Kommunizieren mit einer Geräte-App in C# über IoT Hub-Gerätestreams](./quickstart-device-streams-echo-csharp.md)

* [Schnellstart: Kommunizieren mit einer Geräteanwendung in Node.js über IoT Hub-Gerätestreams (Vorschauversion)](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen IoT-Hub eingerichtet, ein Gerät registriert, einen Gerätestream zwischen einer C-Anwendung auf dem Gerät und einer anderen dienstseitigen Anwendung eingerichtet und mithilfe des Streams Daten zwischen den Anwendungen hin und her gesendet.

Weitere Informationen zu Gerätestreams finden Sie hier:

> [!div class="nextstepaction"]
> [IoT Hub-Gerätestreams (Vorschau)](./iot-hub-device-streams-overview.md)
