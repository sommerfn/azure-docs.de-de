---
title: Verbinden von IoT Plug & Play-Beispielgerätecode (Vorschauversion) mit IoT Hub (Linux) | Microsoft-Dokumentation
description: Erstellen Sie unter Linux IoT Plug & Play-Beispielgerätecode (Vorschauversion), der eine Verbindung mit einem IoT-Hub herstellt. Verwenden Sie die Azure-Befehlszeilenschnittstelle, um die vom Gerät an den Hub gesendeten Informationen anzuzeigen.
author: dominicbetts
ms.author: dobett
ms.date: 09/10/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 38a6deb8d021c5e6a20d765cc7aee5b86042f557
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585976"
---
# <a name="quickstart-connect-a-sample-iot-plug-and-play-preview-device-application-running-on-linux-to-iot-hub"></a>Schnellstart: Verbinden einer unter Linux ausgeführten IoT Plug & Play-Beispielgeräteanwendung (Vorschauversion) mit IoT Hub

In diesem Schnellstart erfahren Sie, wie Sie eine IoT Plug & Play-Beispielgeräteanwendung unter Linux erstellen, sie mit Ihrem IoT-Hub verbinden und mit der Azure-Befehlszeilenschnittstelle die an den Hub gesendeten Informationen anzeigen. Die Beispielanwendung wird in C geschrieben und ist im Azure IoT-Geräte-SDK für C enthalten. Ein Lösungsentwickler kann mithilfe der Azure-Befehlszeilenschnittstelle mehr über die Funktionen eines IoT Plug & Play-Geräts erfahren, ohne Gerätecode anzeigen zu müssen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart wird davon ausgegangen, dass Sie Ubuntu Linux verwenden. Die Schritte in diesem Tutorial wurden unter Ubuntu 18.04 getestet.

Zum Abschließen dieses Schnellstarts müssen Sie auf Ihrem lokalen Linux-Computer die folgende Software installieren:

Installieren Sie **GCC**, **Git**, **cmake** und alle Abhängigkeiten mit dem Befehl `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Überprüfen Sie, ob die Version von `cmake` höher als **2.8.12** und die Version von **GCC** höher als **4.4.7** ist.

```sh
cmake --version
gcc --version
```

## <a name="prepare-an-iot-hub"></a>Vorbereiten eines IoT-Hubs

Für diesen Schnellstart benötigen Sie außerdem eine Azure IoT Hub-Instanz in Ihrem Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!IMPORTANT]
> Während der öffentlichen Vorschauphase sind die IoT Plug & Play-Funktionen nur für IoT-Hubs verfügbar, die in den Regionen **USA, Mitte**, **Europa, Norden** und **Japan, Osten** erstellt wurden.

Wenn Sie die Azure-Befehlszeilenschnittstelle lokal verwenden, sollte mindestens `az` Version **2.0.73** verwendet werden. Für Azure Cloud Shell wird die neueste Version verwendet. Überprüfen Sie die auf dem Computer installierte Version mit dem Befehl `az --version`.

Fügen Sie die Microsoft Azure IoT-Erweiterung für die Azure CLI hinzu:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Für die Schritte in diesem Schnellstart ist mindestens Version **0.8.5** der Erweiterung erforderlich. Überprüfen Sie die installierte Version mit dem Befehl `az extension list`, und aktualisieren Sie sie bei Bedarf mit dem Befehl `az extension update`.

Wenn Sie die Befehlszeilenschnittstelle lokal verwenden, melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Abonnement an:

```bash
az login
```

Wenn Sie Azure Cloud Shell verwenden, sind Sie bereits automatisch angemeldet.

Wenn Sie nicht über einen IoT-Hub verfügen, befolgen Sie [diese Anwendungen, um einen zu erstellen](../iot-hub/iot-hub-create-using-cli.md). Während der Public Preview-Phase ist IoT Plug & Play in den Regionen „Europa, Norden“, „USA, Mitte“ und „Japan, Osten“ verfügbar. Achten Sie darauf, dass Sie Ihren Hub in einer dieser Regionen erstellen.

Führen Sie den folgenden Befehl aus, um die Geräteidentität in Ihrem IoT-Hub zu erstellen. Ersetzen Sie den Platzhalter **YourIoTHubName** durch den richtigen Namen Ihres IoT-Hubs:

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id mydevice
```

Führen Sie die folgenden Befehle aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id mydevice --output table
```

## <a name="prepare-the-development-environment"></a>Vorbereiten der Entwicklungsumgebung

In dieser Schnellstartanleitung bereiten Sie eine Entwicklungsumgebung vor, die Sie zum Klonen und Erstellen des Azure IoT-Geräte-SDK für C verwenden können.

Öffnen Sie eine Eingabeaufforderung. Führen Sie den folgenden Befehl zum Klonen des [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c)-GitHub-Repositorys aus:

```bash
git clone https://github.com/Azure/azure-iot-sdk-c --recursive -b public-preview
```

Es dauert mehrere Minuten, bis dieser Befehl abgeschlossen ist.

## <a name="build-the-code"></a>Erstellen des Codes

Sie verwenden das Geräte-SDK, um den enthaltenen Beispielcode zu erstellen. Die von Ihnen erstellte Anwendung simuliert ein Gerät, mit dem eine Verbindung mit einem IoT-Hub hergestellt wird. Die Anwendung sendet Telemetriedaten und Eigenschaften und empfängt Befehle.

1. Erstellen Sie im Stammordner des Geräte-SDK den Unterordner `cmake`, und navigieren Sie zu diesem Ordner:

    ```bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Führen Sie die folgenden Befehle aus, um das Geräte-SDK und den generierten Code-Stub zu erstellen:

    ```bash
    cmake ..
    cmake --build .
    ```

## <a name="update-your-model-repository"></a>Aktualisieren Ihres Modellrepositorys

Bevor Sie das Beispiel ausführen, fügen Sie dem Unternehmensmodellrepository das Gerätefunktionsmodell und die Schnittstellendefinitionen hinzu:

1. Melden Sie sich mit Ihrem Microsoft-Geschäfts-, Schul- oder Unikonto oder ggf. Ihrer Microsoft-Partner-ID beim [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com) an.

1. Wählen Sie **Company repository** (Unternehmensrepository) und dann **Capability models** (Funktionsmodelle) aus.

1. Wählen Sie **Neu** und dann **Hochladen** aus.

1. Wählen Sie die Datei `SampleDevice.capabilitymodel.json` im Ordner `digitaltwin_client/samples` im Stammordner des Geräte-SDK aus. Wählen Sie **Öffnen** und dann **Speichern** aus, um die Modelldatei in Ihr Repository hochzuladen.

1. Wählen Sie **Company repository** (Unternehmensrepository) und dann **Schnittstellen** aus.

1. Wählen Sie **Neu** und dann **Hochladen** aus.

1. Wählen Sie die Datei `EnvironmentalSensor.interface.json` im Ordner `digitaltwin_client/samples/digitaltwin_sample_environmental_sensor` im Stammordner des Geräte-SDK aus. Wählen Sie **Öffnen** und dann **Speichern** aus, um die Schnittstellendatei in Ihr Repository hochzuladen.

1. Wählen Sie die Option **Company repository** (Unternehmensrepository) und dann **Verbindungszeichenfolgen**. Notieren Sie sich die erste Verbindungszeichenfolge für das Unternehmensmodellrepository, da Sie diese später in diesem Schnellstart verwenden.

## <a name="run-the-sample"></a>Ausführen des Beispiels

Führen Sie eine Beispielanwendung im SDK aus, um ein IoT Plug & Play-Gerät zu simulieren, das Telemetriedaten an Ihren IoT-Hub sendet. So führen Sie die Beispielanwendung aus

1. Navigieren Sie im Ordner `cmake` zu dem Ordner, der die ausführbare Datei enthält:

    ```bash
    cd digitaltwin_client/samples/digitaltwin_sample_device
    ```

1. Führen Sie die ausführbare Datei aus:

    ```bash
    ./digitaltwin_sample_device "{your device connection string}"
    ```

Das simulierte Gerät beginnt, Telemetriedaten zu senden und auf Befehle und Eigenschaftsupdates zu lauschen.

### <a name="use-the-azure-iot-cli-to-validate-the-code"></a>Verwenden der Azure IoT-Befehlszeilenschnittstelle zum Überprüfen des Codes

Vergewissern Sie sich nach dem Starten des Geräteclientbeispiels, dass es mit der Azure-Befehlszeilenschnittstelle funktioniert.

Verwenden Sie den folgenden Befehl, um die Telemetriedaten anzuzeigen, die das Beispielgerät sendet. Möglicherweise müssen Sie ein oder zwei Minuten warten, bevor Telemetriedaten in der Ausgabe aufgeführt werden:

```azurecli-interactive
az iot dt monitor-events --hub-name {your IoT hub} --device-id mydevice
```

Verwenden Sie den folgenden Befehl, um die vom Gerät gesendeten Eigenschaften anzuzeigen:

```azurecli-interactive
az iot dt list-properties --hub-name {your IoT hub} --device-id mydevice --interface sensor --source private --repo-login "{your company model repository connection string}"
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie ein IoT Plug & Play-Gerät mit einem IoT-Hub verbinden. Weitere Informationen zum Erstellen einer Lösung, die mit Ihren IoT Plug & Play-Geräten interagiert, finden Sie unter:

> [!div class="nextstepaction"]
> [Gewusst wie: Herstellen einer Verbindung und Interagieren mit einem Gerät](howto-develop-solution.md)
