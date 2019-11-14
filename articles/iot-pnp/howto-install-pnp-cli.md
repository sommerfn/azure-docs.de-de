---
title: Verwenden der Azure IoT-Erweiterung für Azure CLI zum Interagieren mit IoT Plug & Play-Vorschaugeräten | Microsoft-Dokumentation
description: Installieren Sie die Azure IoT-Erweiterung für Azure CLI, und verwenden Sie sie zum Interagieren mit den IoT Plug &Play-Geräten, die mit Ihrem IoT-Hub verbunden sind.
author: ChrisGMsft
ms.author: chrisgre
ms.date: 09/08/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: eb4f607672c39d45b7791ccaeeb6f7cff9393cb9
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73571017"
---
# <a name="install-and-use-the-azure-iot-extension-for-the-azure-cli"></a>Installieren und Verwenden der Azure IoT-Erweiterung für die Azure CLI

[Die Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) (Befehlszeilenschnittstelle) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen wie z.B. IoT Hub. Die Azure CLI ist unter Windows, Linux und MacOS verfügbar. Die Azure CLI ist auch in [Azure Cloud Shell](https://shell.azure.com) vorinstalliert. Mit der Azure CLI können Sie Azure IoT Hub-Ressourcen, Device Provisioning-Dienstinstanzen und verknüpfte Hubs verwalten, ohne Erweiterungen installieren zu müssen.

Die Azure IoT-Erweiterung für die Azure CLI ist ein Befehlszeilentool zum Interagieren mit und Testen von IoT Plug & Play-Vorschaugeräten. Sie können die Erweiterung für folgende Aufgaben verwenden:

- Herstellen einer Verbindung mit einem Gerät.
- Anzeigen der vom Gerät gesendeten Telemetriedaten.
- Arbeiten mit Geräteeigenschaften.
- Aufrufen von Gerätebefehlen.

In diesem Artikel erfahren Sie Folgendes:

- Installieren und Konfigurieren der Azure IoT-Erweiterung für die Azure CLI.
- Verwenden der Erweiterung zum Interagieren mit Ihren Geräten und Testen Ihrer Geräte.
- Verwenden der Erweiterung zum Verwalten von Schnittstellen im Modellrepository.

## <a name="install-azure-iot-extension-for-the-azure-cli"></a>Installieren der Azure IoT-Erweiterung für die Azure CLI

### <a name="step-1---install-the-azure-cli"></a>Schritt 1: Installieren der Azure CLI

Folgen Sie den [Installationsanleitungen](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) zum Einrichten der Azure CLI in Ihrer Umgebung. Um alle unten aufgeführten Befehle verwenden zu können, ist mindestens die Azure CLI-Version 2.0.73 erforderlich. Verwenden Sie `az -–version`, um dies zu überprüfen.

### <a name="step-2---install-iot-extension"></a>Schritt 2: Installieren der IoT-Erweiterung

In der [Infodatei zur IoT-Erweiterung](https://github.com/Azure/azure-iot-cli-extension) werden mehrere Wege zum Installieren der Erweiterung beschrieben. Die einfachste Möglichkeit ist die Ausführung von `az extension add --name azure-cli-iot-ext`. Nach der Installation können Sie `az extension list` verwenden, um die derzeit installierten Erweiterungen zu überprüfen, oder mit `az extension show --name azure-cli-iot-ext` Details zur IoT-Erweiterung anzeigen. Sie können `az extension remove --name azure-cli-iot-ext` verwenden, um die Erweiterung zu entfernen.

## <a name="use-azure-iot-extension-for-the-azure-cli"></a>Verwenden der Azure IoT-Erweiterung für die Azure CLI

### <a name="prerequisites"></a>Voraussetzungen

Führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Abonnement anzumelden:

```cmd/sh
az login
```

> [!NOTE]
> Wenn Sie Azure Cloud Shell verwenden, werden Sie automatisch angemeldet und müssen diesen Befehl nicht ausführen.

Um die Azure IoT-Erweiterung für die Azure CLI verwenden zu können, benötigen Sie Folgendes:

- Einen Azure IoT Hub. Es gibt viele Möglichkeiten, um einen IoT-Hub zu Ihrem Azure-Abonnement hinzuzufügen, z.B. das [Erstellen eines IoT-Hubs mit der Azure CLI](../iot-hub/iot-hub-create-using-cli.md). Sie benötigen die Verbindungszeichenfolge für den IoT-Hub, um die Befehle für die Azure IoT-Erweiterung ausführen zu können. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

    > [!NOTE]
    > Während der öffentlichen Vorschauphase sind die IoT Plug & Play-Funktionen nur für IoT-Hubs verfügbar, die in den Regionen **USA, Mitte**, **Europa, Norden** und **Japan, Osten** erstellt wurden.

- Ein in Ihrem IoT-Hub registriertes Gerät. Sie können den folgenden Azure CLI-Befehl zum Registrieren eines Geräts verwenden. Ersetzen Sie darin die Platzhalter `{YourIoTHubName}` und `{YourDeviceID}` durch Ihre Werte:

    ```cmd/sh
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

- Einige Befehle benötigen die Verbindungszeichenfolge für ein Unternehmensmodellrepository. Ein Modellrepository für Ihr Unternehmen wird erstellt, wenn Sie zum ersten Mal das [Onboarding zum Azure Certified for IoT-Portal durchführen](howto-onboard-portal.md). Ein Drittanbieter könnte die Verbindungszeichenfolge für sein Modellrepository für Sie freigeben, damit Sie auf seine Schnittstellen und Modelle zugreifen können.

### <a name="interact-with-a-device"></a>Interagieren mit einem Gerät

Sie können die Erweiterung nutzen, um die mit einem IoT-Hub verbundenen IoT Plug & Play-Geräte anzuzeigen und damit zu interagieren. Die Erweiterung funktioniert mit dem digitalen Zwilling, der das IoT Plug & Play-Gerät darstellt.

#### <a name="list-devices-and-interfaces"></a>Auflisten von Geräten und Schnittstellen

Auflisten aller Geräte auf einem IoT-Hub:

```cmd/sh
az iot hub device-identity list --hub-name {YourIoTHubName}
```

Auflisten aller Schnittstellen, die von einem IoT Plug & Play-Gerät registriert wurden:

```cmd/sh
az iot dt list-interfaces --hub-name {YourIoTHubName} --device-id {YourDeviceID}
```

#### <a name="properties"></a>Properties

Auflisten aller Eigenschaften und Eigenschaftswerte für eine Schnittstelle auf einem Gerät:

```cmd/sh
az iot dt list-properties --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login "{YourCompanyModelRepoConnectionString}"
```

Festlegen des Werts für eine Lese-/Schreibeigenschaft:

```cmd/sh
az iot dt update-property --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface-payload {JSONPayload or FilePath}
```

Ein Beispiel für eine Nutzlastdatei zum Festlegen der Eigenschaft **name** für die Schnittstelle **sensor** eines Geräts zu **Contoso** sieht so aus:

```json
{
  "sensor": {
    "properties": {
      "name": {
        "desired": {
          "value": "Contoso"
        }
      }
    }
  }
}
```

#### <a name="commands"></a>Befehle

Auflisten aller Befehle für eine Schnittstelle auf einem Gerät:

```cmd/sh
az iot dt list-commands --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --source private --repo-login {YourCompanyModelRepoConnectionString}
```

Ohne den Parameter `--repo-login` verwendet dieser Befehl das öffentliche Modellrepository.

Aufrufen eines Befehls:

```cmd/sh
az iot dt invoke-command --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --cn {CommandName} --command-payload {CommandPayload or FilePath}
```

#### <a name="digital-twin-events"></a>Digital Twins-Ereignisse

Überwachen aller IoT Plug & Play-Digital Twins-Ereignisse, die zur Event Hub-Consumergruppe **$Default** gesendet werden, von einem bestimmten Gerät und einer bestimmten Schnittstelle aus:

```cmd/sh
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID}
```

Überwachen aller IoT Plug & Play-Digital Twins-Ereignisse, die an eine bestimmte Consumergruppe gesendet werden, von einem bestimmten Gerät und einer bestimmten Schnittstelle aus:

```cmd/sh
az iot dt monitor-events --hub-name {YourIoTHubName} --device-id {YourDeviceID} --interface {YourInterfaceID} --consumer-group {YourConsumerGroup}
```

### <a name="manage-interfaces-in-a-model-repository"></a>Verwalten von Schnittstellen in einem Modellrepository

Die folgenden Befehle verwenden das öffentliche IoT Plug & Play-Modellrepository. Um ein Unternehmensmodellrepository verwenden zu können, fügen Sie das Argument `--login` zur Verbindungszeichenfolge für Ihr Modellrepository hinzu.

Auflisten der Schnittstellen im öffentlichen IoT Plug & Play-Modellrepository:

```cmd/sh
az iot pnp interface list
```

Anzeigen einer Schnittstelle im öffentlichen IoT Plug & Play-Modellrepository:

```cmd/sh
az iot pnp interface show --interface {YourInterfaceId}
```

Erstellen einer Schnittstelle in Ihrem IoT Plug & Play-Unternehmensmodellrepository:

```cmd/sh
az iot pnp interface create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Sie können eine Schnittstelle im öffentlichen Modellrepository nicht direkt erstellen.

Aktualisieren einer Schnittstelle in Ihrem IoT Plug & Play-Unternehmensmodellrepository:

```cmd/sh
az iot pnp interface update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Sie können eine Schnittstelle im öffentlichen Modellrepository nicht direkt aktualisieren.

Veröffentlichen Sie eine Schnittstelle aus Ihrem IoT-Plug & Play-Unternehmensmodellrepository im öffentlichen Modellrepository. Durch diesen Vorgang wird die Schnittstelle unveränderlich:

```cmd/sh
az iot pnp interface publish --interface {YourInterfaceID} --login {YourCompanyModelRepoConnectionString}
```

Nur Microsoft-Partner können Schnittstellen im öffentlichen Modellrepository veröffentlichen.

### <a name="manage-device-capability-models-in-a-model-repository"></a>Verwalten von Gerätefunktionsmodellen in einem Modellrepository

Die folgenden Befehle verwenden das öffentliche IoT Plug & Play-Modellrepository. Um ein Unternehmensmodellrepository verwenden zu können, fügen Sie das Argument `--login` zur Verbindungszeichenfolge für Ihr Modellrepository hinzu.

Auflisten von Gerätefunktionsmodellen im öffentlichen IoT Plug & Play-Modellrepository:

```cmd/sh
az iot pnp capability-model list
```

Anzeigen eines Gerätefunktionsmodells im öffentlichen IoT Plug & Play-Modellrepository:

```cmd/sh
az iot pnp capability-model show --model {YourModelID}
```

Erstellen eines Gerätefunktionsmodells in einem IoT Plug & Play-Unternehmensmodellrepository:

```cmd/sh
az iot pnp capability-model create --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Sie können ein Modell im öffentlichen Modellrepository nicht direkt erstellen.

Aktualisieren eines Gerätefunktionsmodells im IoT Plug & Play-Unternehmensmodellrepository:

```cmd/sh
az iot pnp capability-model update --definition {JSONPayload or FilePath} --login {YourCompanyModelRepoConnectionString}
```

Sie können ein Modell im öffentlichen Modellrepository nicht direkt aktualisieren.

Veröffentlichen Sie ein Gerätefunktionsmodell aus Ihrem IoT-Plug & Play-Unternehmensmodellrepository im öffentlichen Modellrepository. Durch diesen Vorgang wird das Modell unveränderlich:

```cmd/sh
az iot pnp capability-model publish --model {YourModelID} --login {YourCompanyModelRepoConnectionString}
```

Nur Microsoft-Partner können Modelle im öffentlichen Modellrepository veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Gewusst-wie-Artikel haben Sie erfahren, wie die Azure IoT-Erweiterung für die Azure CLI installiert und für die Interaktion mit Ihren Plug & Play-Geräten verwendet wird. Als nächsten Schritt empfehlen wir, sich darüber zu informieren, wie Sie [Modelle verwalten](./howto-manage-models.md).
