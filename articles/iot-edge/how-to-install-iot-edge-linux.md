---
title: Installieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge auf Linux-Geräten unter Ubuntu oder Raspbian
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 9bc4d60eab0dac80d1b2b524f32bc506a66dee18
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72516663"
---
# <a name="install-the-azure-iot-edge-runtime-on-debian-based-linux-systems"></a>Installieren der Azure IoT Edge-Runtime auf Debian-basierten Linux-Systemen

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen. Weitere Informationen finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel werden die Schritte zum Installieren der Azure IoT Edge-Runtime auf einem X64-, ARM32- oder ARM64-Gerät unter Linux erläutert. Installationspakete sind für Ubuntu Server 16.04, Ubuntu Server 18.04 und Raspbian Stretch verfügbar. Unter [Von Azure IoT Edge unterstützte Systeme](support.md#operating-systems) finden Sie eine Liste der unterstützten Linux-Betriebssysteme und -Architekturen.

>[!NOTE]
>Die Unterstützung für ARM64-Geräte befindet sich in der [öffentlichen Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

> [!NOTE]
> Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="install-the-latest-runtime-version"></a>Installieren der neuesten Runtimeversion

Verwenden Sie die folgenden Abschnitte, um die neuste Version der Azure IoT Edge-Runtime auf Ihrem Gerät zu installieren. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Registrieren des Feeds für Microsoft-Schlüssel und Softwarerepositorys

Bereiten Sie Ihr Gerät für die IoT Edge-Laufzeitinstallation vor.

Installieren Sie die Repositorykonfiguration. Wählen Sie den **16.04**- oder **18.04**-Befehl aus, der dem Betriebssystem Ihres Geräts entspricht:

* **Ubuntu Server 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Ubuntu Server 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list > ./microsoft-prod.list
   ```

* **Raspbian Stretch**:
   ```bash
   curl https://packages.microsoft.com/config/debian/stretch/multiarch/prod.list > ./microsoft-prod.list
   ```

Kopieren Sie die generierte Liste.

   ```bash
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

Installieren des öffentlichen Schlüssels von Microsoft GPG

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-the-container-runtime"></a>Installieren der Containerruntime

Azure IoT Edge basiert auf einer [OCI-kompatiblen](https://www.opencontainers.org/) Containerruntime. Für Produktionsszenarien wird empfohlen, das unten angegebene [Moby-basierte](https://mobyproject.org/) Modul zu verwenden. Es ist das einzige Containermodul, das mit Azure IoT Edge offiziell unterstützt wird. Docker CE/EE-Containerimages sind mit der Moby-Runtime kompatibel.

Führen Sie das apt-Update aus.

   ```bash
   sudo apt-get update
   ```

Installieren Sie die Moby-Engine.

   ```bash
   sudo apt-get install moby-engine
   ```

Installieren Sie die Moby-Befehlszeilenschnittstelle (Command-Line Interface, CLI). Die CLI ist nützlich für die Entwicklung, für Produktionsbereitstellungen jedoch optional.

   ```bash
   sudo apt-get install moby-cli
   ```

Wenn beim Installieren der Moby-Containerruntime Fehler auftreten, führen Sie die Schritte im Abschnitt [Überprüfen Sie Ihren Linux-Kernel auf Moby-Kompatibilität](#verify-your-linux-kernel-for-moby-compatibility) weiter unten in diesem Artikel aus. 

### <a name="install-the-azure-iot-edge-security-daemon"></a>Installieren des Daemons für Azure IoT Edge-Sicherheit

Der **Daemon für IoT Edge-Sicherheit** dient zum Bereitstellen und Einhalten von Sicherheitsstandards auf dem IoT Edge-Gerät. Der Daemon wird bei jedem Start gestartet und führt durch Starten der restlichen IoT Edge-Runtime einen Bootstrap für das Gerät aus.

Der Installationsbefehl installiert außerdem die Standardversion von **libiothsm**, falls sie noch nicht vorhanden ist.

Führen Sie das apt-Update aus.

   ```bash
   sudo apt-get update
   ```

Installieren Sie den Sicherheits-Daemon. Das Paket wird unter `/etc/iotedge/` installiert.

   ```bash
   sudo apt-get install iotedge
   ```

Sobald IoT Edge erfolgreich installiert ist, werden Sie in der Ausgabe aufgefordert, die Konfigurationsdatei zu aktualisieren. Folgen Sie den Schritten im Abschnitt [Konfigurieren des Daemons für Azure IoT Edge-Sicherheit](#configure-the-security-daemon), um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="install-a-specific-runtime-version"></a>Installieren einer spezifischen Runtimeversion

Wenn Sie eine bestimmte Version der Azure IoT Edge-Runtime installieren möchten, können Sie die Komponentendateien direkt aus dem IoT Edge-GitHub-Repository auswählen. Gehen Sie anhand der folgenden Schritte vor, um alle IoT Edge-Komponenten auf Ihr Gerät zu laden: die Moby-Engine und -Befehlszeilenschnittstelle, libiothsm und den IoT Edge-Sicherheits-Daemon.

1. Navigieren Sie zu den [Veröffentlichungen von Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases), und suchen Sie die Version, die Sie verwenden möchten. 

2. Erweitern Sie den Abschnitt **Assets** für diese Version.

3. Alle Releases können Updates für die Moby-Engine enthalten. Wenn Dateien mit **moby-engine** und **moby-cli** beginnen, verwenden Sie die folgenden Befehle, um diese Komponenten zu aktualisieren. Wenn keine Moby-Dateien angezeigt werden, durchsuchen Sie die Ressourcen der älteren Releases, bis Sie die neueste Version finden. 

   1. Suchen Sie die **moby-engine**-Datei, die der Architektur Ihres IoT Edge-Geräts entspricht. Klicken Sie mit der rechten Maustaste auf den Dateilink, und kopieren Sie die Linkadresse.

   2. Verwenden Sie den kopierten Link im folgenden Befehl, um diese Version der Moby-Engine zu installieren: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. Suchen Sie die **moby-cli**-Datei, die der Architektur Ihres IoT Edge-Geräts entspricht. Die Moby-Befehlszeilenschnittstelle (Command-Line Interface, CLI) ist eine optionale Komponente, die jedoch bei der Entwicklung hilfreich sein kann. Klicken Sie mit der rechten Maustaste auf den Dateilink, und kopieren Sie die Linkadresse. 

   4. Verwenden Sie den kopierten Link im folgenden Befehl, um diese Version der Moby-Befehlszeilenschnittstelle zu installieren: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Jedes Release sollte neue Dateien für den IoT Edge-Sicherheits-Daemon und hsmlib enthalten. Verwenden Sie die folgenden Befehle, um diese Komponenten zu aktualisieren. 

   1. Suchen Sie die **libiothsm-std**-Datei, die der Architektur Ihres IoT Edge-Geräts entspricht. Klicken Sie mit der rechten Maustaste auf den Dateilink, und kopieren Sie die Linkadresse. 

   2. Verwenden Sie den kopierten Link im folgenden Befehl, um diese Version von hsmlib zu installieren:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. Suchen Sie die **iotedge**-Datei, die der Architektur Ihres IoT Edge-Geräts entspricht. Klicken Sie mit der rechten Maustaste auf den Dateilink, und kopieren Sie die Linkadresse. 

   4. Verwenden Sie den kopierten Link im folgenden Befehl, um diese Version des IoT Edge-Sicherheits-Daemons zu installieren. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Sobald IoT Edge erfolgreich installiert ist, werden Sie in der Ausgabe aufgefordert, die Konfigurationsdatei zu aktualisieren. Befolgen Sie die Schritte im nächsten Abschnitt, um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="configure-the-security-daemon"></a>Konfigurieren des Sicherheitsdaemons

Konfigurieren Sie die IoT Edge-Runtime für die Verknüpfung Ihres physischen Geräts mit einer Geräteidentität, die in einem Azure IoT Hub vorhanden ist.

Der Daemon kann mithilfe der Konfigurationsdatei unter `/etc/iotedge/config.yaml` konfiguriert werden. Die Datei ist standardmäßig schreibgeschützt. Sie benötigen möglicherweise erhöhte Berechtigungen, um sie bearbeiten können.

Ein einzelnes IoT Edge-Gerät kann manuell bereitgestellt werden, indem eine von IoT Hub bereitgestellte Gerät-Verbindungszeichenfolge verwendet wird. Oder Sie können den Device Provisioning-Dienst verwenden, um automatisch Geräte bereitzustellen. Dies ist hilfreich, wenn es sich um eine große Zahl von Geräten handelt. Je nach gewählter Bereitstellung können Sie dazu das richtige Installationsskript auswählen.

### <a name="option-1-manual-provisioning"></a>Option 1: Manuelle Bereitstellung

Zur manuellen Bereitstellung eines Geräts müssen Sie es mit einer [Geräteverbindungszeichenfolge](how-to-register-device-portal.md) bereitstellen, die Sie durch die Registrierung eines neuen Geräts in Ihrem IoT-Hub erstellen können.

Öffnen Sie die Konfigurationsdatei.

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie die Bereitstellungskonfigurationen in der Datei, und heben Sie die Auskommentierung des Abschnitts zum **manuellen Bereitstellungsmodus** auf. Aktualisieren Sie den Wert von **device_connection_string** durch die Verbindungszeichenfolge Ihres IoT Edge-Geräts. Stellen Sie sicher, dass alle anderen Bereitstellungsabschnitte auskommentiert sind.

   ```yaml
   # Manual provisioning configuration
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   # provisioning:
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   attestation:
   #     method: "tpm"
   #     registration_id: "{registration_id}"
```
Verwenden Sie zum Einfügen des Inhalts der Zwischenablage in Nano `Shift+Right Click`, oder drücken Sie `Shift+Insert`.

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Option 2: Automatische Bereitstellung

Um ein Gerät automatisch bereitzustellen, müssen Sie [den Device Provisioning-Dienst einrichten und die Registrierungs-ID Ihres Geräts abrufen](how-to-auto-provision-simulated-device-linux.md). Es gibt eine Reihe von Nachweismechanismen, die von IoT Edge unterstützt werden, wenn die automatische Bereitstellung verwendet wird. Ihre Hardwareanforderungen haben aber auch Auswirkungen auf Ihre Auswahl. Beispielsweise verfügen Raspberry Pi-Geräte nicht standardmäßig über einen TPM-Chip (Trusted Platform Module).

Öffnen Sie die Konfigurationsdatei.

```bash
sudo nano /etc/iotedge/config.yaml
```

Suchen Sie die Bereitstellungskonfigurationen in der Datei, und heben Sie die Auskommentierung des Abschnitts für Ihren Nachweismechanismus auf. Wenn Sie z. B. den TPM-Nachweis verwenden, aktualisieren Sie die Werte von **scope_id** und **registration_id** mit den Werten aus Ihrer IoT Hub Device Provisioning Service-Instanz bzw. Ihrem IoT Edge-Gerät mit TPM.

   ```yaml
   # Manual provisioning configuration
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # DPS TPM provisioning configuration
   provisioning:
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     attestation:
       method: "tpm"
       registration_id: "{registration_id}"
   ```

Verwenden Sie zum Einfügen des Inhalts der Zwischenablage in Nano `Shift+Right Click`, oder drücken Sie `Shift+Insert`.

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Erstellen und Bereitstellen eines simulierten TPM-IoT Edge-Geräts auf einem virtuellen Linux-Computer](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Sie können den Status des IoT Edge-Daemons überprüfen:

```bash
systemctl status iotedge
```

Untersuchen Sie die Daemonprotokolle:

```bash
journalctl -u iotedge --no-pager --no-full
```

Führen Sie eine automatisierte Überprüfung auf die häufigsten Konfigurations- und Netzwerkfehler durch: 

```bash
sudo iotedge check
```

Und listen Sie derzeit ausgeführte Module auf:

```bash
sudo iotedge list
```

Nach der Installation von IoT Edge auf Ihrem Gerät sollte als einziges Modul **edgeAgent** ausgeführt werden. Nachdem Sie die erste Bereitstellung erstellt haben, wird auch das andere Systemmodul **$edgeHub** auf dem Gerät gestartet. Weitere Informationen finden Sie unter [Bereitstellen eines IoT Edge-Moduls](how-to-deploy-modules-portal.md).

## <a name="tips-and-troubleshooting"></a>Tipps und Problembehandlung

Sie benötigen erhöhte Rechte zum Ausführen von `iotedge`-Befehlen. Melden Sie sich der Installation der Runtime von Ihrem Computer ab und anschließend wieder an, um Ihres Berechtigungen automatisch zu aktualisieren. Verwenden Sie bis dahin **sudo** vor allen `iotedge`-Befehlen.

Auf Geräten mit Ressourceneinschränkungen wird dringend empfohlen, die Umgebungsvariable *OptimizeForPerformance* auf *false* festzulegen, wie im [Leitfaden zur Problembehandlung](troubleshoot.md) beschrieben.

Wenn in Ihrem Netzwerk ein Proxyserver vorhanden ist, führen Sie die Schritte in [Konfigurieren eines IoT Edge-Geräts für die Kommunikation über einen Proxyserver](how-to-configure-proxy-support.md) aus.

### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Überprüfen des Linux-Kernels auf Moby-Kompatibilität

Viele Hersteller von eingebetteten Geräten stellen Geräteimages mit benutzerdefinierten Linux-Kernels bereit, denen die Funktionen für die Containerruntime-Kompatibilität fehlen. Wenn beim Installieren der empfohlenen Moby-Containerruntime Probleme auftreten, können Sie versuchen, Ihre Linux-Kernelkonfiguration mit dem [check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh)-Skript aus dem offiziellen [Moby GitHub-Repository](https://github.com/moby/moby) zu korrigieren. Führen Sie auf dem Gerät die folgenden Befehle aus, um Ihre Kernelkonfiguration zu überprüfen:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Die detaillierte Ausgabe zeigt den Status der Kernelfunktionen, die von der Moby-Runtime verwendet werden. Sie sollten sicherstellen, dass alle Elemente unter `Generally Necessary` und `Network Drivers` aktiviert sind, damit Ihr Kernel vollständig mit der Moby-Runtime kompatibel ist.  Wenn Sie fehlende Funktionen identifiziert haben, aktivieren Sie diese, indem Sie Ihren Kernel aus der Quelle neu erstellen und die zugehörigen Module auswählen, die in die entsprechende Konfigurationsdatei für den Kernel eingebunden werden sollen.  Gleiches gilt, wenn Sie einen Kernelkonfigurationsgenerator wie defconfig oder menuconfig verwenden: Suchen und aktivieren Sie die entsprechenden Funktionen suchen und aktivieren, und erstellen Sie dann den Kernel dementsprechend neu.  Sobald Sie Ihren bearbeiteten Kernel bereitgestellt haben, führen Sie das check-config-Skript erneut aus, um zu überprüfen, ob alle erforderlichen Funktionen erfolgreich aktiviert wurden.


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

Wenn Sie Probleme mit der ordnungsgemäßen Installation der IoT Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung](troubleshoot.md).

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).
