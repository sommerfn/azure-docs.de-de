---
title: Installieren von Azure IoT Edge unter Linux | Microsoft-Dokumentation
description: Anweisungen zur Installation von Azure IoT Edge auf Linux AMD64-Geräten mit ausgeführtem Ubuntu
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: 822efe2534d49c0995a672232107cc322e547989
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227508"
---
# <a name="install-the-azure-iot-edge-runtime-on-linux-x64"></a>Installieren der Azure IoT Edge-Runtime unter Linux (x64)

Die Azure IoT Edge-Runtime verwandelt ein Gerät in ein IoT Edge-Gerät. Die Runtime kann auf verschiedensten Geräten bereitgestellt werden – vom kleinen Raspberry Pi bis hin zum großen industriellen Server. Wenn ein Gerät mit der IoT Edge-Runtime konfiguriert wurde, können Sie darauf Geschäftslogik aus der Cloud bereitstellen.

Weitere Informationen finden Sie unter [Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur](iot-edge-runtime.md).

In diesem Artikel sind die Schritte zum Installieren der Azure IoT Edge-Runtime auf dem Ubuntu Linux-x64-IoT Edge-Gerät (Intel/AMD) aufgeführt. Unter [Von Azure IoT Edge unterstützte Systeme](support.md#operating-systems) finden Sie eine Liste der unterstützten AMD64-Betriebssysteme.

> [!NOTE]
> Pakete in den Linux-Softwarerepositorys unterliegen den Lizenzbedingungen im jeweiligen Paket (/usr/share/doc/*Paketname*). Lesen Sie vor Verwendung des Pakets die Lizenzbedingungen. Durch die Installation und Nutzung des Pakets erklären Sie sich mit diesen Bedingungen einverstanden. Wenn Sie mit den Lizenzbedingungen nicht einverstanden sind, verwenden Sie das Paket nicht.

## <a name="install-the-latest-version"></a>Installieren Sie die neuste Version

Verwenden Sie die folgenden Abschnitte, um die neuste Version von Azure IoT Edge auf Ihren Geräten zu installieren. 

### <a name="register-microsoft-key-and-software-repository-feed"></a>Registrieren des Feeds für Microsoft-Schlüssel und Softwarerepositorys

Bereiten Sie Ihr Gerät für die IoT Edge-Laufzeitinstallation vor.


Installieren Sie die Repositorykonfiguration. Wählen Sie entweder den Codeausschnitt **16.04** oder **18.04**, der für Ihre Version von Ubuntu geeignet ist:

> [!NOTE]
> Stellen Sie sicher, dass Sie den Codeausschnitt aus dem richtigen Codefeld für Ihre Version von Ubuntu auswählen.

* Für **Ubuntu 16.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   ```

* Für **Ubuntu 18.04**:
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > ./microsoft-prod.list
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

#### <a name="verify-your-linux-kernel-for-moby-compatibility"></a>Überprüfen Sie Ihren Linux-Kernel auf Moby-Kompatibilität

Viele Hersteller von eingebetteten Geräten stellen Gerätebilder bereit, die benutzerdefinierte Linux-Kernel enthalten, denen Funktionen für die Container-Runtimekompatibilität fehlen können. Wenn Sie Probleme beim Installieren der empfohlenen [Moby](https://github.com/moby/moby)-Containerruntime haben, dann können Sie versuchen, Ihre Linux-Kernelkonfiguration mit dem [check-config](https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh)-Skript aus dem offiziellen [Moby GitHub-Repository](https://github.com/moby/moby) zu korrigieren, indem Sie die folgenden Befehle auf dem Gerät ausführen.

   ```bash
   curl -sSL https://raw.githubusercontent.com/moby/moby/master/contrib/check-config.sh -o check-config.sh
   chmod +x check-config.sh
   ./check-config.sh
   ```

Dadurch erhalten Sie eine detaillierte Ausgabe, die den Status der Kernelfunktionen erhält, auf die die Moby-Runtime zugreift. Sie sollten sicherstellen, dass alle Elemente unter `Generally Necessary` und `Network Drivers` aktiviert sind, damit Ihr Kernel vollständig mit der Moby-Runtime kompatibel ist.  Wenn Sie fehlende Funktionen gefunden haben, können Sie sie aktivieren, indem Sie Ihren Kernel aus der Quelle neu erstellen und die dazugehörigen Module für die passende Kernel .config auswählen.  Auf ähnliche Art und Weise müssen Sie bei einem Kernelkonfigurationsgenerator wie defconfig oder menuconfig die entsprechenden Funktionen suchen und aktivieren, und dann den Kernel dementsprechend neu erstellen.  Sobald Sie Ihren bearbeiteten Kernel bereitgestellt haben, sollten sie das „check-config“-Skript erneut ausführen, um zu überprüfen, dass die von Ihnen identifizierten Funktionen auch erfolgreich aktiviert wurden.

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

Sobald IoT Edge erfolgreich installiert ist, wird die Ausgabe Sie auffordern, die Konfigurationsdatei zu aktualisieren. Folgen Sie den Schritten im Abschnitt [„Konfigurieren des Daemons für Azure IoT Edge-Sicherheit“](#configure-the-azure-iot-edge-security-daemon), um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="install-a-specific-version"></a>Installieren einer spezifischen Version

Wenn Sie eine bestimmte Version von Azure IoT Edge installieren möchten, dann können Sie die Komponentendateien direkt aus dem IoT Edge GitHub-Repository auswählen. Verwenden Sie die folgenden Schritte, um alle IoT Edge-Komponenten auf Ihr Gerät zu laden: die Moby-Engine und CLI, libiothsm und den IoT Edge-Sicherheits-Daemon.

1. Navigieren Sie zu den [Veröffentlichungen von Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases) und suchen Sie die Version, die Sie verwenden möchten. 

2. Erweitern Sie den Abschnitt **„Assets“** , um zu der Version zu gelangen.

3. Bei allen Versionen können Updates für die Moby-Engine vorhanden sein. Wenn Sie Dateien sehen, die mit **„moby-engine“** und **„moby-cli“** beginnen, dann verwenden Sie die folgenden Befehle, um diese Komponenten zu aktualisieren. Wenn Sie keine Moby-Dateien sehen und Moby noch nicht auf Ihrem Gerät installiert haben, dann durchsuchen Sie die älteren Veröffentlichungsassets, bis sie sie finden. 

   1. Suchen Sie die **„moby-engine“** -Datei, die zur Architektur Ihres IoT Edge-Geräts passt. Klicken Sie mit der rechten Maustaste auf den Dateilink und kopieren Sie die Linkadresse.

   2. Verwenden Sie den kopierten Link im folgenden Befehl, um die Version der Moby-Engine zu installieren: 

      ```bash
      curl -L <moby-engine link> -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb
      ```

   3. Suchen Sie die **„moby-cli“** -Datei, die zur Architektur Ihres IoT Edge-Geräts passt. Die Moby CLI ist eine optionale Komponente, die jedoch in der Entwicklungsphase hilfreich sein kann. Klicken Sie mit der rechten Maustaste auf den Dateilink und kopieren Sie die Linkadresse. 

   4. Verwenden Sie den kopierten Link im folgenden Befehl, um die Version der Moby CLI zu installieren: 

      ```bash
      curl -L <moby-cli link> -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb
      ```

4. Bei jeder neuen Veröffentlichung sollten Sie neue Dateien für den IoT Edge-Sicherheits-Daemon und hsmlib finden. Verwenden Sie die folgenden Befehle, um diese Komponenten zu aktualiseren. 

   1. Suchen Sie die **„libiothsm-std“** -Datei, die zur Architektur Ihres IoT Edge-Geräts passt. Klicken Sie mit der rechten Maustaste auf den Dateilink und kopieren Sie die Linkadresse. 

   2. Verwenden Sie den kopierten Link im folgenden Befehl, um die Version der hsmlib zu installieren:

      ```bash
      curl -L <libiothsm-std link> -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb
      ```
   
   3. Suchen Sie die **„iotedge“** -Datei, die zur Architektur Ihres IoT Edge-Geräts passt. Klicken Sie mit der rechten Maustaste auf den Dateilink und kopieren Sie die Linkadresse. 

   4. Verwenden Sie den kopierten Link im folgenden Befehl, um die Version der des IoT Edge-Sicherheits-Daemons zu installieren. 

      ```bash
      curl -L <iotedge link> -o iotedge.deb && sudo dpkg -i ./iotedge.deb
      ```

Sobald IoT Edge erfolgreich installiert ist, wird die Ausgabe Sie auffordern, die Konfigurationsdatei zu aktualisieren. Halten Sie sich an die Schritte im nächsten Abschnitt, um die Bereitstellung Ihres Geräts abzuschließen. 

## <a name="configure-the-azure-iot-edge-security-daemon"></a>Konfigurieren des Azure IoT Edge-Sicherheits-Daemons

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

Speichern und schließen Sie die Datei.

   `CTRL + X`, `Y`, `Enter`

Starten Sie nach Eingabe der Bereitstellungsinformationen in der Konfigurationsdatei den Daemon neu:

```bash
sudo systemctl restart iotedge
```

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn Sie im vorherigen Abschnitt die Schritte für die **manuelle Konfiguration** ausgeführt haben, müsste die IoT Edge-Runtime erfolgreich bereitgestellt worden sein und auf Ihrem Gerät ausgeführt werden. Wenn Sie die Schritte für die **automatische Konfiguration** ausgeführt haben, sind einige weitere Schritte erforderlich, damit die Runtime Ihr Gerät in Ihrem Namen bei Ihrem IoT-Hub registrieren kann. Informationen zu den nächsten Schritten finden Sie unter [Erstellen und Bereitstellen eines simulierten TPM-IoT Edge-Geräts auf einem virtuellen Linux-Computer](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

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

Auf Geräten mit Ressourceneinschränkungen wird dringend empfohlen, die Umgebungsvariable *OptimizeForPerformance* auf *false* festzulegen, wie im [Leitfaden zur Problembehandlung](troubleshoot.md) beschrieben.

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

Wenn Sie Probleme mit der ordnungsgemäßen Installation der IoT Edge-Runtime haben, lesen Sie die Informationen auf der Seite [Problembehandlung](troubleshoot.md).

Weitere Informationen zum Aktualisieren einer vorhandenen Installation auf die aktuelle Version von IoT Edge finden Sie unter [Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime](how-to-update-iot-edge.md).
