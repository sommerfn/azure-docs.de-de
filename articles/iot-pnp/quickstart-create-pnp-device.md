---
title: Erstellen eines IoT Plug & Play-Geräts (Vorschauversion) | Microsoft-Dokumentation
description: Verwenden Sie ein Gerätefunktionsmodell, um einen Gerätecode zu generieren. Führen Sie anschließend den Gerätecode aus, und verfolgen Sie, wie für das Gerät die Verbindung mit Ihrer IoT Hub-Instanz hergestellt wird.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 019dbe8b977932c6a806f7efca8c0724597718d8
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818073"
---
# <a name="quickstart-use-a-device-capability-model-to-create-an-iot-plug-and-play-preview-device-windows"></a>Schnellstart: Verwenden eines Gerätefunktionsmodells zum Erstellen eines IoT Plug & Play-Geräts (Vorschauversion, Windows)

Mit einem _Gerätefunktionsmodell_ (Device Capability Model, DCM) werden die Funktionen eines IoT Plug & Play-Geräts beschrieben. Ein Gerätefunktionsmodell ist häufig einer Produkt-SKU zugeordnet. Die Funktionen, die im Gerätefunktionsmodell definiert sind, werden als wiederverwendbare Schnittstellen organisiert. Sie können ein Gerüst eines Gerätecodes aus einem Gerätefunktionsmodell generieren. In diesem Schnellstart wird veranschaulicht, wie Sie VS Code unter Windows zum Erstellen eines IoT Plug & Play-Geräts mit einem Gerätefunktionsmodell verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Schnellstarts müssen Sie auf Ihrem lokalen Computer die folgende Software installieren:

* [Buildtools für Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) mit Workloads für **C++-Buildtools** und **NuGet-Paket-Manager-Komponenten**. Oder Sie haben bereits [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 oder 2015 mit denselben Workloads installiert.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).
* [Visual Studio Code](https://code.visualstudio.com/).

### <a name="install-azure-iot-tools"></a>Installieren von Azure IoT-Tools

Führen Sie die folgenden Schritte aus, um das Erweiterungspaket [Azure IoT Tools für VS Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) zu installieren:

1. Wählen Sie in VS Code die Registerkarte **Erweiterungen**.
1. Suchen Sie nach **Azure IoT Tools**.
1. Wählen Sie **Installieren** aus.

### <a name="install-the-azure-iot-explorer"></a>Installieren von Azure IoT-Explorer

Laden Sie das neueste Release von **Azure IoT Explorer** von der [Repositoryseite](https://github.com/Azure/azure-iot-explorer/releases) des Tools herunter, und installieren Sie es, indem Sie die MSI-Datei unter „Assets“ für das neueste Update auswählen.

### <a name="get-the-connection-string-for-your-company-model-repository"></a>Abrufen der Verbindungszeichenfolge für das Modellrepository Ihres Unternehmens

Sie finden die _Verbindungszeichenfolge für das Modellrepository Ihres Unternehmens_ im Portal [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com), wenn Sie sich mit einem Geschäfts-, Schul- oder Unikonto von Microsoft oder mit Ihrer Microsoft-Partner-ID anmelden (falls vorhanden). Wählen Sie nach dem Anmelden die Option **Company repository** (Unternehmensrepository) und dann **Verbindungszeichenfolgen**.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>Vorbereiten eines IoT-Hubs

Für diesen Schnellstart benötigen Sie außerdem eine Azure IoT Hub-Instanz in Ihrem Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Während der öffentlichen Vorschauphase sind die IoT Plug & Play-Funktionen nur für IoT-Hubs verfügbar, die in den Regionen **USA, Mitte**, **Europa, Norden** und **Japan, Osten** erstellt wurden.

Fügen Sie die Microsoft Azure IoT-Erweiterung für die Azure CLI hinzu:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Führen Sie den folgenden Befehl aus, um die Geräteidentität in Ihrem IoT-Hub zu erstellen. Ersetzen Sie die Platzhalter **YourIoTHubName** und **YourDevice** durch Ihre entsprechenden Namen. Falls Sie nicht über eine IoT Hub-Instanz verfügen, dient Ihnen diese [Anleitung als Hilfe bei der Erstellung](../iot-hub/iot-hub-create-using-cli.md):

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Führen Sie die folgenden Befehle aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Führen Sie die folgenden Befehle aus, um die _IoT Hub-Verbindungszeichenfolge_ für Ihren Hub auszuführen:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

Notieren Sie sich die Geräteverbindungszeichenfolge, die wie folgt aussieht:

```json
HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyCDevice;SharedAccessKey={YourSharedAccessKey}
```

Dieser Wert wird später in der Schnellstartanleitung benötigt.

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

### <a name="get-azure-iot-device-sdk-for-c"></a>Abrufen des Azure IoT-Geräte-SDK für C

In diesem Schnellstart bereiten Sie eine Entwicklungsumgebung vor, indem Sie das Azure IoT-Geräte-SDK für C mittels [Vcpkg](https://github.com/microsoft/vcpkg) installieren.

1. Öffnen Sie eine Eingabeaufforderung. Führen Sie den folgenden Befehl aus, um Vcpkg zu installieren:

    ```cmd/sh
    git clone https://github.com/Microsoft/vcpkg.git
    cd vcpkg

    .\bootstrap-vcpkg.bat
    ```

    Führen Sie dann Folgendes aus, um die benutzerweite [Integration](https://github.com/microsoft/vcpkg/blob/master/docs/users/integration.md) zu verbinden (Hinweis: erfordert bei der ersten Verwendung Administratorrechte):

    ```cmd/sh
    .\vcpkg.exe integrate install
    ```

1. Installieren Sie Vcpkg für das Azure IoT-C-Geräte-SDK:

    ```cmd/sh
    .\vcpkg.exe install azure-iot-sdk-c[public-preview,use_prov_client]
    ```

## <a name="author-your-model"></a>Erstellen des Modells

In dieser Schnellstartanleitung verwenden Sie ein vorhandenes Beispiel für ein Gerätefunktionsmodell und die zugehörigen Schnittstellen.

1. Erstellen Sie das Verzeichnis `pnp_app` auf dem lokalen Laufwerk.

1. Laden Sie das [Gerätefunktionsmodell](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/SampleDevice.capabilitymodel.json) und das [Schnittstellenbeispiel](https://github.com/Azure/IoTPlugandPlay/blob/master/samples/EnvironmentalSensor.interface.json) herunter, und speichern Sie die Dateien im Ordner `pnp_app`.

    > [!TIP]
    > Navigieren Sie zum Herunterladen einer Datei aus GitHub zur gewünschten Datei, klicken Sie mit der rechten Maustaste auf **Raw**, und wählen Sie dann die Option **Save link as** (Link speichern als).

1. Öffnen Sie den Ordner `pnp_app` mit VS Code. Sie können die Dateien mit IntelliSense anzeigen:

    ![Gerätefunktionsmodell](media/quickstart-create-pnp-device/dcm.png)

1. Ersetzen Sie in den heruntergeladenen Dateien `<YOUR_COMPANY_NAME_HERE>` in den Feldern `@id` und `schema` durch einen eindeutigen Wert. Verwenden Sie nur die Zeichen a - z, A - Z, 0 - 9 und Unterstrich. Weitere Informationen finden Sie unter [Digital Twin Identifier Format](https://github.com/Azure/IoTPlugandPlay/tree/master/DTDL#digital-twin-identifier-format) (Bezeichnerformat für digitale Zwillinge).

## <a name="generate-the-c-code-stub"></a>Generieren des C-Code-Stubs

Da Sie jetzt über ein Gerätefunktionsmodell und die zugeordneten Schnittstellen verfügen, können Sie den Gerätecode generieren, mit dem das Modell implementiert wird. So generieren Sie den C-Codestub in VS Code

1. Drücken Sie bei geöffnetem Ordner mit den DCM-Dateien die Tastenkombination **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen. Geben Sie **IoT Plug & Play** ein, und wählen Sie **Gerätecodestub generieren** aus.

    > [!NOTE]
    > Wenn Sie die IoT Plug & Play-Codegenerator-Befehlszeilenschnittstelle zum ersten Mal verwenden, dauert es einige Sekunden, bis alles heruntergeladen und automatisch installiert wurde.

1. Wählen Sie die DCM-Datei aus, mit der Sie den Gerätecode-Stub generieren möchten.

1. Geben Sie den Projektnamen **sample_device** ein. Dies ist der Name für Ihre Geräteanwendung.

1. Wählen Sie **ANSI C** als Sprache aus.

1. Wählen Sie **Via IoT Hub device connection string** (Über IoT Hub-Geräteverbindungszeichenfolge) als Verbindungsmethode aus.

1. Wählen Sie **CMake Project on Windows** (CMake-Projekt unter Windows) als Projektvorlage aus.

1. Wählen Sie **Via Vcpkg** (Über Vcpkg) als Methode zum Einschließen des Geräte-SDK aus.

1. Ein neuer Ordner mit dem Namen **sample_device** wird am selben Speicherort wie die DCM-Datei erstellt. Dort befinden sich die generierten Gerätecodestub-Dateien. VS Code öffnet zum Anzeigen dieser Dateien ein neues Fenster.
    ![Gerätecode](media/quickstart-create-pnp-device/device-code.png)

## <a name="build-the-code"></a>Erstellen des Codes

Sie erstellen den generierten Gerätecodestub gemeinsam mit dem Geräte-SDK. Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Ordner `sample_device` den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```cmd\sh
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um den generierten Codestub zu erstellen (wobei Sie den Platzhalter durch das Verzeichnis des Vcpkg-Repositorys ersetzen):

    ```cmd\sh
    cmake .. -G "Visual Studio 16 2019" -A Win32 -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"

    cmake --build .
    ```
    
    > [!NOTE]
    > Wenn Sie Visual Studio 2017 oder 2015 verwenden, müssen Sie den CMake-Generator entsprechend den verwendeten Buildtools angeben:
    >```cmd\sh
    ># Either
    >cmake .. -G "Visual Studio 15 2017" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    ># or
    >cmake .. -G "Visual Studio 14 2015" -Duse_prov_client=ON -Dhsm_type_symm_key:BOOL=ON -DCMAKE_TOOLCHAIN_FILE="{directory of your Vcpkg repo}\scripts\buildsystems\vcpkg.cmake"
    >```

    > [!NOTE]
    > Falls CMake Ihren C++-Compiler nicht finden kann, treten beim Ausführen des obigen Befehls Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus.

1. Führen Sie nach erfolgreichem Abschluss des Buildvorgangs Ihre Anwendung aus, und übergeben Sie dabei die IoT-Hub-Geräteverbindungszeichenfolge als Parameter.

    ```cmd\sh
    .\Debug\sample_device.exe "[IoT Hub device connection string]"
    ```

1. Die Geräteanwendung beginnt mit dem Senden von Daten an IoT Hub.

    ![Geräte-App wird ausgeführt](media/quickstart-create-pnp-device/device-app-running.png)

## <a name="validate-the-code"></a>Überprüfen des Codes

### <a name="publish-device-model-files-to-model-repository"></a>Veröffentlichen von Gerätemodelldateien im Modellrepository

Zum Überprüfen des Gerätecodes mit **Azure IoT-Explorer** müssen Sie die Dateien im Modellrepository veröffentlichen.

1. Drücken Sie bei geöffnetem Ordner mit den Gerätefunktionsmodell-Dateien in VS Code **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen. Geben Sie **IoT Plug & Play: Submit files to Model Repository** (IoT Plug & Play: Dateien an Modellrepository übermitteln) ein, und wählen Sie diese Option aus.

1. Wählen Sie die Dateien `SampleDevice.capabilitymodel.json` und `EnvironmentalSensor.interface.json` aus.

1. Geben Sie die Verbindungszeichenfolge für das Modellrepository Ihres Unternehmens ein.

    > [!NOTE]
    > Sie benötigen die Verbindungszeichenfolge nur, wenn Sie sich zum ersten Mal am Repository anmelden.

1. Im VS Code-Ausgabefenster und anhand der Benachrichtigung können Sie sich vergewissern, dass die Dateien erfolgreich veröffentlicht wurden.

    > [!NOTE]
    > Falls Sie beim Veröffentlichen der Gerätemodelldateien Fehler erhalten, können Sie den Befehl **IoT Plug & Play: Sign out Model Repository** (IoT Plug & Play: Modellrepository abmelden) verwenden, um sich abzumelden, und die Schritte dann erneut ausführen.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Verwenden von Azure IoT-Explorer zum Überprüfen des Codes

1. Öffnen Sie Azure IoT-Explorer. Die Seite mit den **App-Konfigurationen** wird angezeigt.

1. Geben Sie Ihre IoT Hub-Verbindungszeichenfolge ein, und klicken Sie auf **Verbinden**.

1. Nachdem Sie eine Verbindung hergestellt haben, wird die Seite mit der Geräteübersicht angezeigt.

1. Wählen Sie zum Hinzufügen Ihres Unternehmensrepositorys **Einstellungen**, **+ Add module definition source** (Moduldefinitionsquelle hinzufügen) und dann **Company repository** (Unternehmensrepository) aus. Fügen Sie die Verbindungszeichenfolge für das Modellrepository Ihres Unternehmens hinzu, und wählen Sie **Save and Connect** (Speichern und verbinden) aus.

1. Suchen Sie auf der Seite mit der Geräteübersicht nach der Geräteidentität, die Sie zuvor erstellt haben, und wählen Sie sie aus, um weitere Details anzuzeigen.

1. Erweitern Sie die Schnittstelle mit der ID **urn:<IHR_SCHNITTSTELLENNAME>:EnvironmentalSensor:1**, um die IoT Plug & Play-Grundtypen anzuzeigen: Eigenschaften, Befehle und Telemetriedaten. Der angezeigte Schnittstellenname ist der Name, den Sie beim Erstellen des Modells eingegeben haben.

1. Wählen Sie die Seite **Telemetrie** aus, und klicken Sie auf _Start_, um die vom Gerät gesendeten Telemetriedaten anzuzeigen.

1. Wählen Sie die Seite **Properties(non-writable)** (Eigenschaften (nicht schreibbar)) aus, um die vom Gerät gemeldeten nicht schreibbaren Eigenschaften anzuzeigen.

1. Wählen Sie die Seite **Properties(writable)** (Eigenschaften (schreibbar)) aus, um die schreibbaren Eigenschaften anzuzeigen, die Sie aktualisieren können.

1. Erweitern Sie die **name**-Eigenschaft, aktualisieren Sie sie mit einem neuen Namen, und wählen Sie **Update writable property** (Schreibbare Eigenschaft aktualisieren) aus.

1. Klicken Sie zum Anzeigen des neuen Namens in der Spalte **Gemeldete Eigenschaft** oben auf der Seite auf die Schaltfläche **Aktualisieren**.

1. Wählen Sie die Seite **Befehl** aus, um alle Befehle anzuzeigen, die vom Gerät unterstützt werden.

1. Erweitern Sie den Befehl **blink** (Blinken), und legen Sie ein neues Intervall für die Blinkzeit fest. Wählen Sie **Befehl senden** aus, um den Befehl auf dem Gerät aufzurufen.

1. Navigieren Sie zum simulierten Gerät, um zu überprüfen, ob der Befehl wie erwartet ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie mit einem Gerätefunktionsmodell ein IoT Plug & Play-Gerät erstellen.

Weitere Informationen zu IoT Plug & Play finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Erstellen und Testen eines Gerätefunktionsmodells mit Visual Studio Code](tutorial-pnp-visual-studio-code.md)
