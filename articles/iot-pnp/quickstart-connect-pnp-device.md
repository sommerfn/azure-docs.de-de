---
title: Verbinden von IoT Plug & Play-Beispielgerätecode (Vorschauversion) mit IoT Hub (Windows) | Microsoft-Dokumentation
description: Erstellen Sie unter Windows IoT Plug & Play-Beispielgerätecode (Vorschauversion), der eine Verbindung mit einem IoT-Hub herstellt, und führen Sie ihn aus. Verwenden Sie das Tool Azure IoT-Explorer, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 2dd5d197851b0090ac1af7bbde5a1ad1b951c785
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73569912"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-windows-to-iot-hub"></a>Schnellstart: Verbinden einer unter Windows ausgeführten IoT Plug & Play-Beispielgeräteanwendung (Vorschauversion) mit IoT Hub

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung erstellen, mit Ihrem IoT-Hub verbinden und mit dem Azure IoT-Explorer-Tool die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wird in C geschrieben und ist im Azure IoT-Geräte-SDK für C enthalten. Ein Lösungsentwickler kann das Azure IoT-Explorer-Tool verwenden, um mehr über die Funktionen eines IoT Plug & Play-Geräts zu erfahren, ohne Gerätecode anzeigen zu müssen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Abschließen dieses Schnellstarts müssen Sie auf Ihrem lokalen Computer die folgende Software installieren:

* [Buildtools für Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16) mit Workloads für **C++-Buildtools** und **NuGet-Paket-Manager-Komponenten**. Oder Sie haben bereits [Visual Studio (Community, Professional oder Enterprise)](https://visualstudio.microsoft.com/downloads/) 2019, 2017 oder 2015 mit denselben Workloads installiert.
* [Git](https://git-scm.com/download/).
* [CMake](https://cmake.org/download/).

### <a name="install-the-azure-iot-explorer"></a>Installieren von Azure IoT-Explorer

Laden Sie das Tool Azure IoT-Explorer von der Seite mit dem [aktuellen Release](https://github.com/Azure/azure-iot-explorer/releases) herunter.

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

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

### <a name="get-azure-iot-device-sdk-for-c"></a>Abrufen des Azure IoT-Geräte-SDK für C

In dieser Schnellstartanleitung bereiten Sie eine Entwicklungsumgebung vor, die Sie zum Klonen und Erstellen des Azure IoT-Geräte-SDK für C verwenden können.

Öffnen Sie eine Eingabeaufforderung. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:

```cmd/sh
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Sie sollten damit rechnen, dass die Ausführung dieses Vorgangs mehrere Minuten in Anspruch nimmt.

## <a name="build-the-code"></a>Erstellen des Codes

Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Stammordner des Geräte-SDK den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```cmd\sh
    cd <root folder>\azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Code-Stub zu erstellen:

    ```cmd\sh
    cmake ..
    cmake --build . -- /m /p:Configuration=Release
    ```

    > [!NOTE]
    > Falls CMake Ihren C++-Compiler nicht finden kann, treten beim Ausführen des obigen Befehls Buildfehler auf. Führen Sie den Befehl in diesem Fall an der [Visual Studio-Eingabeaufforderung](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) aus.

## <a name="run-the-device-sample"></a>Ausführen des Gerätebeispiels

Führen Sie Ihre Anwendung aus, indem Sie die IoT Hub-Geräteverbindungszeichenfolge als Parameter übergeben.

```cmd\sh
cd digitaltwin_client\samples\digitaltwin_sample_device\Release
copy ..\EnvironmentalSensor.interface.json .
digitaltwin_sample_device.exe "[IoT Hub device connection string]"
```

Die Geräteanwendung beginnt mit dem Senden von Daten an IoT Hub.

## <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Verwenden des Azure IoT-Explorers zum Überprüfen des Codes

1. Öffnen Sie Azure IoT-Explorer. Die Seite mit den **App-Konfigurationen** wird angezeigt.

1. Geben Sie Ihre IoT Hub-Verbindungszeichenfolge ein, und klicken Sie auf **Verbinden**.

1. Nachdem Sie eine Verbindung hergestellt haben, wird die Seite mit der Geräteübersicht angezeigt.

1. Wählen Sie zum Hinzufügen Ihres Unternehmensrepositorys die Option **Einstellungen**, **+ Neu** und dann **On the connected device** (Auf dem verbundenen Gerät) aus.

1. Suchen Sie auf der Seite mit der Geräteübersicht nach der Geräteidentität, die Sie zuvor erstellt haben, und wählen Sie sie aus, um weitere Details anzuzeigen.

1. Erweitern Sie die Schnittstelle mit der ID **urn:YOUR_COMPANY_NAME_HERE:EnvironmentalSensor:1**, um die IoT Plug & Play-Primitive anzuzeigen: Eigenschaften, Befehle und Telemetriedaten.

1. Wählen Sie die Seite **Telemetrie** aus, um die vom Gerät gesendeten Telemetriedaten anzuzeigen.

1. Wählen Sie die Seite **Properties(non-writable)** (Eigenschaften (nicht schreibbar)) aus, um die vom Gerät gemeldeten nicht schreibbaren Eigenschaften anzuzeigen.

1. Wählen Sie die Seite **Properties(writable)** (Eigenschaften (schreibbar)) aus, um die schreibbaren Eigenschaften anzuzeigen, die Sie aktualisieren können.

1. Erweitern Sie die Eigenschaft **name**, aktualisieren Sie sie mit einem neuen Namen, und wählen Sie **update writable property** (Schreibbare Eigenschaft aktualisieren) aus. 

1. Klicken Sie zum Anzeigen des neuen Namens in der Spalte **Gemeldete Eigenschaft** oben auf der Seite auf die Schaltfläche **Aktualisieren**.

1. Wählen Sie die Seite **Befehl** aus, um alle Befehle anzuzeigen, die vom Gerät unterstützt werden.

1. Erweitern Sie den Befehl **blink** (Blinken), und legen Sie ein neues Intervall für die Blinkzeit fest. Wählen Sie **Befehl senden** aus, um den Befehl auf dem Gerät aufzurufen.

1. Navigieren Sie zum simulierten Gerät, um zu überprüfen, ob der Befehl wie erwartet ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie unter:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem IoT Plug & Play Preview-Gerät](howto-develop-solution.md)
