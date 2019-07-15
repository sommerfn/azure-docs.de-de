---
title: Installieren von Azure IoT Edge unter Linux ARM32 | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge unter Linux auf einem ARM32-Gerät wie einem Raspberry Pi
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: kgremban
ms.openlocfilehash: f7004edf2bab0e22d4d1e4c1200d6e8b8ef729b3
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485946"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Installieren der Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen. 

Weitere Informationen zur Funktionsweise und zu den Komponenten der IoT Edge-Runtime finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel werden die Schritte zum Installieren der Azure IoT Edge-Runtime auf einem IoT Edge-Gerät des Typs Linux-ARM32v7/armhf erläutert. Diese Schritte funktionieren beispielsweise für Raspberry Pi-Geräte. Eine Liste der unterstützten ARM32-Betriebssysteme finden Sie unter [Von Azure IoT Edge unterstützte Systeme](support.md#operating-systems). 

>[!NOTE]
>Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="install-the-latest-version"></a>Installieren Sie die neuste Version

Verwenden Sie die folgenden Abschnitte, um die neuste Version von Azure IoT Edge auf Ihren Linux ARM-Geräten zu installieren. 

### <a name="install-the-container-runtime"></a>Installieren der Containerruntime

Azure IoT Edge basiert auf einer [OCI-kompatiblen](https://www.opencontainers.org/) Containerruntime. Für Produktionsszenarien wird dringend empfohlen, das unten angegebene [Moby-basierte](https://mobyproject.org/) Modul zu verwenden. Es ist das einzige Containermodul, das mit Azure IoT Edge offiziell unterstützt wird. Docker CE/EE-Containerimages sind mit der Moby-basierten Runtime kompatibel.

Über die nachstehenden Befehle werden sowohl die Moby-basierte Engine als auch die Befehlszeilenschnittstelle (Command-Line Interface, CLI) installiert. Die CLI ist nützlich für die Entwicklung, für Produktionsbereitstellungen jedoch optional.

```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f
```

### <a name="install-the-iot-edge-security-daemon"></a>Installieren des Daemons für IoT Edge-Sicherheit

Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem IoT Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten der restlichen IoT Edge-Runtime einen Bootstrap für das Gerät aus. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

Sobald IoT Edge erfolgreich installiert ist, wird die Ausgabe Sie auffordern, die Konfigurationsdatei zu aktualisieren. Folgen Sie den Schritten im Abschnitt [„Konfigurieren des Daemons für Azure IoT Edge-Sicherheit“](#configure-the-azure-iot-edge-security-daemon), um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="install-a-specific-version"></a>Installieren einer spezifischen Version

Wenn Sie eine spezifische Version von Azure IoT Edge installieren möchten, können Sie die Komponentendateien direkt aus dem IoT Edge GitHub-Repository auswählen. Verwenden Sie dieselben `curl`-Befehle, die in den vorherigen Abschnitten aufgeführt sind, um alle IoT Edge-Komponenten auf Ihr Gerät zu ziehen: die Moby-Engine und CLI, libiothsm und den IoT Edge-Sicherheits-Daemon. Der einzige Unterschied ist, dass Sie die **aka.ms**-URLs mit Links ersetzen, die direkt auf die Version aller Komponente verweisen, die Sie benutzen wollen.

Navigieren Sie zu den [Veröffentlichungen von Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) und suchen Sie die Version, die Sie verwenden möchten. Erweitern Sie den Abschnitt **„Assets“** , um die Version zu sehen. Wählen Sie dann die Dateien aus, die zur Architektur Ihres IoT Edge-Geräts passen. Jede IoT Edge-Veröffentlichung enthält **iotedge** und **libiothsm**-Dateien. Allerdings enthalten nicht alle Veröffentlichungen **moby-engine** oder **moby-cli**. Wenn Sie die Moby-Containerengine noch nicht installiert haben, dann suchen Sie in den älteren Versionen nach einer, die die Moby-Komponenten enthält. 

Sobald IoT Edge erfolgreich installiert ist, wird die Ausgabe Sie auffordern, die Konfigurationsdatei zu aktualisieren. Halten Sie sich an die Schritte im nächsten Abschnitt, um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Azure IoT Edge-Sicherheits-Daemons

Konfigurieren Sie die IoT Edge-Runtime für die Verknüpfung Ihres physischen Geräts mit einer Geräteidentität, die in einem Azure IoT Hub vorhanden ist. 

Der Daemon kann mithilfe der Konfigurationsdatei unter `/etc/iotedge/config.yaml` konfiguriert werden. Die Datei ist standardmäßig schreibgeschützt. Daher benötigen Sie möglicherweise erhöhte Berechtigungen, um sie bearbeiten zu können.

Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Oder Sie können den Device Provisioning-Dienst verwenden, um automatisch Geräte bereitzustellen. Dies ist hilfreich, wenn es sich um eine große Zahl von Geräten handelt. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen. 

### <a name="option-1-manual-provisioning"></a>Option 1: Manuelle Bereitstellung

Zur manuellen Bereitstellung eines Geräts müssen Sie es mit einer [Geräteverbindungszeichenfolge](how-to-register-device-portal.md) bereitstellen, die Sie durch die Registrierung eines neuen IoT Edge-Geräts in Ihrem IoT-Hub erstellen können.

Öffnen Sie die Konfigurationsdatei. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie den Bereitstellungsabschnitt der Datei, und heben Sie die Auskommentierung des **manuellen** Bereitstellungsmodus auf. Aktualisieren Sie den Wert von **device_connection_string** durch die Verbindungszeichenfolge Ihres IoT Edge-Geräts.

```yaml
provisioning:
  source: "manual"
  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
# provisioning: 
#   source: "dps"
#   global_endpoint: "https://global.azure-devices-provisioning.net"
#   scope_id: "{scope_id}"
#   registration_id: "{registration_id}"
```

Speichern und schließen Sie die Datei. 

`CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Option 2: Automatische Bereitstellung

Um ein Gerät automatisch bereitzustellen, müssen Sie [den Device Provisioning-Dienst einrichten und die Registrierungs-ID Ihres Geräts abrufen](how-to-auto-provision-simulated-device-linux.md). Die automatische Bereitstellung funktioniert nur mit Geräten, die über einen Trusted Platform Module (TPM)-Chip verfügen. Raspberry Pi-Geräte sind beispielsweise nicht standardmäßig mit TPM ausgestattet. 

Öffnen Sie die Konfigurationsdatei. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie den Bereitstellungsabschnitt der Datei, und heben Sie die Auskommentierung des **DPS**-Bereitstellungsmodus auf. Aktualisieren Sie die Werte von **scope_id** und **registration_id** mit den Werten aus Ihrem IoT Hub Device Provisioning-Dienst und Ihrem IoT Edge-Gerät mit TPM. 

```yaml
# provisioning:
#   source: "manual"
#   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
provisioning: 
  source: "dps"
  global_endpoint: "https://global.azure-devices-provisioning.net"
  scope_id: "{scope_id}"
  registration_id: "{registration_id}"
```

Speichern und schließen Sie die Datei. 

`CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie alternativ die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Erstellen und Bereitstellen eines simulierten TPM-IoT Edge-Geräts auf einem virtuellen Linux-Computer](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Sie können den Status des IoT Edge-Daemons wie folgt überprüfen:

```bash
systemctl status iotedge
```

Untersuchen Sie die Daemonprotokolle wie folgt:

```bash
journalctl -u iotedge --no-pager --no-full
```

Führen Sie die ausgeführten Module wie folgt auf:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Tipps und Vorschläge

Sie benötigen erhöhte Rechte zum Ausführen von `iotedge`-Befehlen. Melden Sie sich der Installation der Runtime von Ihrem Computer ab und anschließend wieder an, um Ihres Berechtigungen automatisch zu aktualisieren. Verwenden Sie bis dahin **sudo** vor allen `iotedge`-Befehlen.

Auf Geräten mit Ressourceneinschränkungen wird dringend empfohlen, die Umgebungsvariable *OptimizeForPerformance* auf *false* festzulegen, wie im [Leitfaden zur Problembehandlung](troubleshoot.md#stability-issues-on-resource-constrained-devices) beschrieben.

Wenn in Ihrem Netzwerk ein Proxyserver vorhanden ist, führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md) aus.

## <a name="uninstall-iot-edge"></a>Deinstallieren von IoT Edge

Wenn Sie die IoT Edge-Installation von Ihrem Linux-Gerät entfernen möchten, verwenden Sie die folgenden Befehle auf der Befehlszeile. 

Entfernen Sie die IoT Edge-Runtime. 

```bash
sudo apt-get remove --purge iotedge
```

Sobald die IoT Edge-Runtime entfernt wird, werden die Container, die sie erstellt hat, beendet. Sie sind jedoch weiterhin auf dem Gerät vorhanden. Zeigen Sie alle Container an, um die verbliebenen zu sehen. 

```bash
sudo docker ps -a
```

Löschen Sie die Container von Ihrem Gerät, einschließlich der zwei Laufzeitcontainer. 

```bash
sudo docker rm -f <container name>
```

Entfernen Sie schließlich die Containerruntime von Ihrem Gerät. 

```bash 
sudo apt-get remove --purge moby-cli
sudo apt-get remove --purge moby-engine
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein IoT Edge-Gerät für die installierte Runtime bereitgestellt haben, können Sie [IoT Edge-Module bereitstellen](how-to-deploy-modules-portal.md).

Wenn Sie Probleme mit der ordnungsgemäßen Installation der IoT Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung](troubleshoot.md#stability-issues-on-resource-constrained-devices).

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).
