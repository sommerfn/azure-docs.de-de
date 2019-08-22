---
title: Offlinebetrieb von Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Hier erfahren Sie, wie IoT Edge-Geräte und -Module über längere Zeiträume ohne Internetverbindung betrieben werden können und wie IoT Edge auch den Offlinebetrieb normaler IoT-Geräte ermöglichen kann.
author: kgremban
ms.author: kgremban
ms.date: 08/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6d82b353f8b485b4441853b7ff8e70e7d69f4d6a
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68986984"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Grundlegendes zu erweiterten Offlinefunktionen für IoT Edge-Geräte und -Module sowie untergeordnete Geräte

Azure IoT Edge unterstützt erweiterte Offlinevorgänge auf Ihren IoT Edge-Geräten und ermöglicht Offlinevorgänge auch auf untergeordneten Nicht-IoT Edge-Geräten. Solange ein IoT Edge-Gerät die Möglichkeit besitzt, eine Verbindung mit IoT Hub herzustellen, kann es zusammen mit allen untergeordneten Geräten auch mit unregelmäßiger oder ohne Internetverbindung funktionieren. 


## <a name="how-it-works"></a>So funktioniert's

Wenn ein IoT Edge-Gerät in den Offlinemodus wechselt, übernimmt der IoT Edge-Hub drei Rollen. Zunächst erfasst er alle für die Upstream-Übermittlung bestimmten Nachrichten und speichert sie, bis das Gerät wieder eine Verbindung herstellt. Zweitens handelt er im Auftrag von IoT Hub und authentifiziert Module und untergeordnete Geräte, sodass diese weiterhin ausgeführt werden können. Schließlich ermöglicht er die Kommunikation zwischen untergeordneten Geräten, die normalerweise über IoT Hub abgewickelt würde. 

Das folgende Beispiel veranschaulicht ein IoT Edge-Szenario im Offlinemodus:

1. **Konfigurieren von Geräten**

   Für IoT Edge-Geräte werden automatisch Offlinefunktionen aktiviert. Um diese Funktionen auf andere IoT-Geräte auszuweiten, müssen Sie eine Beziehung über- und untergeordneter Elemente zwischen den Geräten in IoT Hub deklarieren. Anschließend konfigurieren Sie die untergeordneten Geräte so, dass sie dem zugewiesenen übergeordneten Gerät vertrauen, und leiten die Gerät-zu-Cloud-Kommunikation über das übergeordnete Gerät als Gateway. 

2. **Synchronisieren mit IoT Hub**

   Mindestens einmal nach der Installation der IoT Edge-Runtime muss das IoT Edge-Gerät online sein, damit die Synchronisierung mit IoT Hub ausgeführt wird. Bei dieser Synchronisierung empfängt das IoT Edge-Gerät Details zu allen zugewiesenen untergeordneten Geräten. Das IoT Edge-Gerät aktualisiert zudem auf sichere Weise seinen lokalen Cache, um Offlinevorgänge zu aktivieren und ruft Einstellungen für das lokale Speichern von Telemetrienachrichten. 

3. **Offlineschalten**

   Während sie von IoT Hub getrennt sind, können das IoT Edge-Gerät, seine bereitgestellten Module und alle untergeordneten IoT-Geräte ohne zeitliche Beschränkung betrieben werden. Module und untergeordnete Geräte können gestartet und neu gestartet werden, indem im Offlinemodus die Authentifizierung mit dem IoT Edge-Hub durchgeführt wird. Telemetriedaten für die Upstreamübermittlung an IoT Hub werden lokal gespeichert. Die Kommunikation zwischen Modulen und untergeordneten IoT-Geräten wird durch direkte Methoden oder Nachrichten aufrechterhalten. 

4. **Erneutes Verbinden und Synchronisieren mit IoT Hub**

   Nach dem Wiederherstellen der Verbindung mit IoT Hub wird die Synchronisierung des IoT Edge-Geräts wieder ausgeführt. Lokal gespeicherte Nachrichten werden in der Reihenfolge übermittelt, in der sie gespeichert wurden. Unterschiede zwischen den gewünschten und gemeldeten Eigenschaften der Module und Geräte werden ausgeglichen. Das IoT Edge-Gerät aktualisiert seine Gruppe von zugewiesenen untergeordneten IoT-Geräten mit den Änderungen.

## <a name="restrictions-and-limits"></a>Einschränkungen

Die in diesem Artikel beschriebenen erweiterten Offlinefunktionen sind in der [IoT Edge-Version 1.0.7 oder höher](https://github.com/Azure/azure-iotedge/releases) verfügbar. Frühere Versionen verfügen nur über einen Teil dieser Offlinefunktionen. Für vorhandene IoT Edge-Geräte, die über keine erweiterten Offlinefunktionen verfügen, kann kein Upgrade durch Ändern der Runtime-Version ausgeführt werden. Stattdessen sind sie mit einer neuen IoT Edge-Geräteidentität zu konfigurieren, damit diese Funktionen verfügbar werden. 

Erweiterte Offlinefunktionen werden in allen Regionen unterstützt, in denen IoT Hub verfügbar ist (**Ausnahme:** USA, Osten).

Lediglich Nicht-IoT Edge-Geräte können als untergeordnete Geräte hinzugefügt werden. 

IoT Edge-Geräte und deren zugewiesene untergeordnete Geräte können nach der ersten, einmaligen Synchronisierung unbefristet im Offlinemodus betrieben werden. Die Speicherung von Nachrichten hängt jedoch von der Einstellung für die TTL (Gültigkeitsdauer) und dem verfügbaren Speicherplatz zum Ablegen der Nachrichten ab. 

## <a name="set-up-parent-and-child-devices"></a>Einrichten von übergeordneten und untergeordneten Geräten

Damit ein IoT Edge-Gerät seine erweiterten Offlinefunktionen auf untergeordnete IoT-Geräte ausweiten kann, müssen Sie zwei Schritte ausführen. Zuerst deklarieren Sie die Beziehungen über- und untergeordneter Geräte im Azure-Portal. Als Zweites schaffen Sie eine Vertrauensstellung zwischen dem übergeordneten Gerät und allen untergeordneten Geräten und konfigurieren anschließend die Gerät-zu-Cloud-Kommunikation über das übergeordnete Gerät als Gateway. 

### <a name="assign-child-devices"></a>Zuweisen von untergeordneten Geräten

Untergeordnete Geräte können beliebige Nicht-IoT Edge-Geräte sein, die beim selben IoT Hub registriert sind. Übergeordnete Geräte können über mehrere untergeordnete Geräte verfügen, doch weist ein untergeordnetes Gerät nur ein übergeordnetes Gerät auf. Es gibt drei Möglichkeiten, untergeordnete Geräte für ein Edge-Gerät festzulegen: über das Azure-Portal, mithilfe der Azure CLI oder über das IoT Hub Service SDK. 

Die folgenden Abschnitte enthalten Beispiele dafür, wie Sie die Beziehung über- und untergeordneter Geräte in IoT Hub für vorhandene IoT-Geräte deklarieren können. Wenn Sie neue Geräteidentitäten für Ihre untergeordneten Geräte erstellen, finden Sie weitere Informationen dazu unter [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md).

#### <a name="option-1-iot-hub-portal"></a>Option 1: IoT Hub-Portal

Sie können die Beziehung über- und untergeordneter Geräte beim Erstellen eines neuen Geräts deklarieren. Bei bereits vorhandenen Geräten können Sie die Beziehung auf der Gerätedetailseite des übergeordneten IoT Edge-Geräts oder des untergeordneten IoT-Geräts deklarieren. 

   ![Verwalten untergeordneter Geräte auf der Detailseite des IoT Edge-Geräts](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Option 2: Verwenden des `az`Befehlszeilentools

Mithilfe der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) mit [IoT-Erweiterung](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 oder höher) können Sie Beziehungen zwischen über- und untergeordneten Geräten über die [device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)-Unterbefehle verwalten. Im folgenden Beispiel wird eine Abfrage verwendet, um alle Nicht-IoT Edge-Geräte auf dem Hub als untergeordnete Geräte eines IoT Edge-Geräts zuzuweisen. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

Sie können die [Abfrage](../iot-hub/iot-hub-devguide-query-language.md) so ändern, dass eine andere Teilmenge von Geräten ausgewählt wird. Der Befehl kann mehrere Sekunden in Anspruch nehmen, wenn Sie eine große Gruppe von Geräten angeben.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Option 3: Verwenden des IoT Hub Service SDK 

Beziehungen zwischen über- und untergeordneten Geräten können Sie außerdem programmgesteuert mit einem C#-, Java- oder Node.js-IoT Hub Service SDK verwalten. Dies ist ein [Beispiel für die Zuordnung eines untergeordneten Geräts](https://aka.ms/set-child-iot-device-c-sharp) mit dem C#-SDK.

### <a name="set-up-the-parent-device-as-a-gateway"></a>Einrichten des übergeordneten Geräts als Gateway

Sie können sich eine Beziehung über- und untergeordneter Geräte als ein transparentes Gateway vorstellen, bei dem das untergeordnete Gerät über eine eigene Identität in IoT Hub verfügt, aber mit der Cloud über das übergeordnete Gerät kommuniziert. Für eine sichere Kommunikation muss das untergeordnete Gerät überprüfen können, dass das übergeordnete Gerät aus einer vertrauenswürdigen Quelle stammt. Andernfalls könnten Dritte schädliche Geräte einrichten, die die Identität übergeordneter Geräte annehmen und die Kommunikation abfangen. 

Eine Möglichkeit zum Erstellen dieser Vertrauensstellung wird in den folgenden Artikeln ausführlich beschrieben: 
* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Verbinden eines nachgeschalteten (untergeordneten) Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>Angeben von DNS-Servern 

Zur Verbesserung der Stabilität empfiehlt es sich dringend, die in Ihrer Umgebung verwendeten DNS-Serveradressen anzugeben. Sehen Sie sich die zwei Möglichkeiten zum [Festlegen des DNS-Servers im Artikel zur Problembehandlung](troubleshoot.md#resolution-7) an.

## <a name="optional-offline-settings"></a>Optionale Offlineeinstellungen

Wenn Ihre Geräte offline geschaltet werden, speichert das übergeordnete IoT Edge-Gerät alle Gerät-zu-Cloud-Nachrichten, bis die Verbindung wiederhergestellt ist. Das IoT Edge-Hub-Modul verwaltet die Speicherung und Weiterleitung von Offlinenachrichten. Bei Geräten, die möglicherweise über längere Zeiträume offline sind, optimieren Sie die Leistung, indem zwei IoT Edge-Hub-Einstellungen konfigurieren. 

Legen Sie zunächst eine höhere Einstellung für die Gültigkeitsdauer fest, damit der IoT Edge-Hub Nachrichten so lange aufbewahrt, bis Ihr Gerät wieder verbunden ist. Fügen Sie dann zusätzlichen Speicherplatz für den Nachrichtenspeicher hinzu. 

### <a name="time-to-live"></a>Gültigkeitsdauer

Die Gültigkeitsdauer entspricht der Zeit (in Sekunden), die eine Nachricht auf die Übermittlung warten kann, ehe sie abläuft. Der Standardwert ist 7.200 Sekunden (zwei Stunden). Der Höchstwert ist nur durch den Höchstwert einer ganzzahligen Variablen begrenzt und beträgt ca. 2 Milliarden. 

Diese Einstellung ist eine gewünschte Eigenschaft des IoT Edge-Hub, die im Modulzwilling gespeichert wird. Sie können sie im Azure-Portal oder direkt im Bereitstellungsmanifest konfigurieren. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="host-storage-for-system-modules"></a>Hostspeicher für Systemmodule

Nachrichten- und Modulzustandsinformationen werden standardmäßig im lokalen Containerdateisystem des IoT Edge-Hubs gespeichert. Für eine höhere Zuverlässigkeit, insbesondere beim Offlinebetrieb, können Sie auch Speicher auf dem Host-IoT Edge-Gerät reservieren.

Um den Speicher auf dem Hostsystem einzurichten, erstellen Sie Umgebungsvariablen für den IoT Edge-Hub und den IoT Edge-Agent, die auf einen Speicherordner im Container verweisen. Binden Sie diesen Speicherordner anschließend mit den Erstellungsoptionen an einem Ordner auf dem Hostcomputer. 

Sie können Umgebungsvariablen und die Erstellungsoptionen für das IoT Edge-Hub-Modul im Azure-Portal im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** konfigurieren. 

1. Fügen Sie sowohl für den IoT Edge-Hub als auch für den IoT Edge-Agent eine Umgebungsvariable namens **storageFolder** hinzu, die auf ein Verzeichnis im Modul verweist.
1. Fügen Sie sowohl für den IoT Edge-Hub als auch für den IoT Edge-Agent Bindungen hinzu, um ein lokales Verzeichnis auf dem Hostcomputer mit einem Verzeichnis im Modul zu verbinden. Beispiel: 

   ![Fügen Sie Erstellungsoptionen und Umgebungsvariablen für den lokalen Speicher hinzu.](./media/offline-capabilities/offline-storage.png)

Oder Sie können den lokalen Speicher auch direkt im Bereitstellungsmanifest konfigurieren. Beispiel: 

```json
"systemModules": {
    "edgeAgent": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"]
                }
            }
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        }
    },
    "edgeHub": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath"],
                    "PortBindings":{"5671/tcp":[{"HostPort":"5671"}],"8883/tcp":[{"HostPort":"8883"}],"443/tcp":[{"HostPort":"443"}]}}}
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        },
        "status": "running",
        "restartPolicy": "always"
    }
}
```

Ersetzen Sie `<HostStoragePath>` und `<ModuleStoragePath>` durch den Speicherpfad für Ihren Host und Ihr Modul; beide Werte müssen ein absoluter Pfad sein. 

Beispielsweise bedeutet `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]`, dass das Verzeichnis **/etc/iotedge/storage** in Ihrem Hostsystem dem Verzeichnis **/iotedge/storage/** für den Container zugeordnet ist. In einem weiteren Beispiel zu Windows-Systemen bedeutet `"Binds":["C:\\temp:C:\\contemp"]`, dass das Verzeichnis **C:\\temp** in Ihrem Hostsystem dem Verzeichnis **C:\\contemp** für den Container zugeordnet ist. 

Stellen Sie unter Linux sicher, dass das Benutzerprofil des IoT Edge-Hubs, UID 1000, Lese-, Schreib- und Ausführungsrechte für das Hostsystemverzeichnis hat. Diese Berechtigungen sind erforderlich, damit der IoT Edge-Hub Nachrichten im Verzeichnis speichern und später abrufen kann. (Der IOT Edge-Agent wird als Root betrieben und benötigt daher keine zusätzlichen Berechtigungen.) Es gibt mehrere Möglichkeiten, Verzeichnisberechtigungen auf Linux-Systemen zu verwalten, einschließlich der Verwendung von `chown` zum Ändern des Verzeichnisinhabers und dann `chmod` zum Ändern der Berechtigungen. Beispiel:

```bash
sudo chown 1000 <HostStoragePath>
sudo chmod 700 <HostStoragePath>
```

Weitere Details zu Erstellungsoptionen finden Sie in der [Docker-Dokumentation](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Einrichten eines transparenten Gateways für Ihre Verbindungen über- und untergeordneter Geräte: 

* [Konfigurieren eines IoT Edge-Geräts als transparentes Gateway](how-to-create-transparent-gateway.md)
* [Authentifizieren eines nachgeschalteten Geräts bei Azure IoT Hub](how-to-authenticate-downstream-device.md)
* [Verbinden eines nachgeschalteten Geräts mit einem Azure IoT Edge-Gateway](how-to-connect-downstream-device.md)
