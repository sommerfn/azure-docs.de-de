---
title: Aktualisieren der IoT Edge-Version auf Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Aktualisieren von IoT Edge-Geräten auf die neuesten Versionen des Sicherheitsdaemons und der IoT Edge-Runtime
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 27f0ebab1fd87eb1870c5a8be21c4f80be4132f1
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698583"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>Aktualisieren des IoT Edge-Sicherheitsdaemons und der Runtime

Wenn neue Versionen des IoT Edge-Diensts veröffentlicht werden, sollten Sie Ihre IoT Edge-Geräte aktualisieren, damit sie über die neuesten Funktionen und Verbesserungen der Sicherheit verfügen. Dieser Artikel enthält Informationen zum Aktualisieren Ihrer IoT Edge-Geräte, wenn eine neue Version verfügbar ist. 

Zwei Komponenten eines IoT Edge-Geräts müssen bei der Umstellung auf eine neue Version aktualisiert werden. Die erste ist der Sicherheitsdaemon, der auf dem Gerät ausgeführt wird und beim Starten des Geräts die Runtimemodule startet. Der Sicherheitsdaemon kann zurzeit nur vom Gerät selbst aktualisiert werden. Die zweite Komponente ist die Runtime, die aus den IoT Edge-Hub- und den IoT Edge-Agent-Modulen besteht. Je nachdem, wie Ihre Bereitstellung strukturiert ist, kann die Runtime vom Gerät oder remote aktualisiert werden. 

Informationen zur neuesten Version von Azure IoT Edge finden Sie unter [Azure IoT Edge releases](https://github.com/Azure/azure-iotedge/releases) (Azure IoT Edge-Versionen).

## <a name="update-the-security-daemon"></a>Aktualisieren des Sicherheitsdaemons

Der IoT Edge-Sicherheitsdaemon ist eine native Komponente, die mithilfe des Paket-Managers auf dem IoT Edge-Gerät aktualisiert werden muss. 

Überprüfen Sie die Version des auf Ihrem Geräts ausgeführten Sicherheitsdaemons mit dem Befehl `iotedge version`. 

### <a name="linux-devices"></a>Linux-Geräte

Verwenden Sie auf Linux x64-Geräten „apt-get“ oder Ihren geeigneten Paket-Manager, um den Sicherheitsdaemon zu aktualisieren. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

Verwenden Sie für Linux ARM32-Geräte die Schritte unter [„Installieren der Azure IoT Edge-Runtime unter Linux (ARM32v7/armhf)“](how-to-install-iot-edge-linux-arm.md), um die neuste Version des Sicherheits-Daemons zu installieren. 

### <a name="windows-devices"></a>Windows-Geräte

Verwenden Sie auf Windows-Geräten das PowerShell-Skript, um den Sicherheitsdaemon zu aktualisieren. Das Skript lädt per Pull automatisch die neueste Version des Sicherheitsdaemons herunter. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Update-IoTEdge -ContainerOs <Windows or Linux>
```

Wenn Sie den Update-IoTEdge-Befehl ausführen, wird der Sicherheitsdaemon zusammen mit den beiden Runtimecontainerimages von Ihrem Gerät entfernt. Die Datei „config.yaml“ verbleibt auf dem Gerät, ebenso wie Daten der Moby-Containerengine (sofern Sie Windows-Container verwenden). Durch die Beibehaltung der Konfigurationsinformationen müssen Sie während des Aktualisierungsvorgangs die Verbindungszeichenfolge oder die Device Provisioning Service-Informationen für Ihr Gerät nicht erneut angeben. 

Wenn Sie eine bestimmte Version des Sicherheitsdaemons installieren möchten, laden Sie die entsprechende Datei „Microsoft-Azure-IoTEdge.cab“ aus den [IoT Edge-Releases](https://github.com/Azure/azure-iotedge/releases) herunter. Verwenden Sie anschließend den Parameter `-OfflineInstallationPath`, um auf den Speicherort der heruntergeladenen Datei zu zeigen. Weitere Informationen finden Sie unter [Offlineinstallation](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Aktualisieren der Runtimecontainer

Die Art und Weise, in der Sie die IoT Edge-Agent- und IoT Edge-Hubcontainer aktualisieren, hängt davon ab, ob Sie in Ihrer Bereitstellung fortlaufende Versionstags (wie 1.0) oder spezifische Versionstags (wie 1.0.7) verwenden. 

Überprüfen Sie die Version der aktuell auf Ihrem Gerät ausgeführten IoT Edge-Agent- und IoT Edge-Hubmodule mithilfe der Befehle `iotedge logs edgeAgent` und `iotedge logs edgeHub`. 

  ![Suchen der Containerversion in Protokollen](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Grundlagen von IoT Edge-Tags

Die IoT Edge-Agent- und IoT Edge-Hubimages sind im Tag mit der IoT Edge-Version gekennzeichnet, der sie zugeordnet sind. Es gibt zwei Möglichkeiten zum Verwenden von Tags mit den Runtime-Images: 

* **Fortlaufende Tags**: Verwenden Sie nur die ersten zwei Stellen der Versionsnummer, um zum neuesten Image zu gelangen, das mit diesen Stellen übereinstimmt. Beispielsweise wird, sobald eine neue Version veröffentlicht wird, 1.0 immer so aktualisiert, dass auf die neueste 1.0.x-Version verwiesen wird. Wenn die Containerruntime auf Ihrem IoT Edge-Gerät das Image erneut per Pull herunterlädt, werden die Laufzeitmodule auf die neueste Version aktualisiert. Dieses Vorgehen wird für Entwicklungszwecke vorgeschlagen. Bereitstellungen aus dem Azure-Portal weisen standardmäßig fortlaufende Tags auf. 
* **Spezifische Tags**: Verwenden Sie alle drei Stellen der Versionsnummer, um die Imageversion explizit festzulegen. Beispielsweise ändert sich 1.0.7 nach der ursprünglichen Veröffentlichung nicht. Sie können im Bereitstellungsmanifest eine neue Versionsnummer deklarieren, wenn Sie zur Aktualisierung bereit sind. Dieses Vorgehen wird für Produktionszwecke vorgeschlagen.

### <a name="update-a-rolling-tag-image"></a>Aktualisieren eines Images mit fortlaufendem Tag

Wenn Sie in Ihrer Bereitstellung fortlaufende Tags verwenden (Beispiel: mcr.microsoft.com/azureiotedge-hub:**1.0**), müssen Sie die Containerruntime auf Ihrem Gerät dazu zwingen, per Pull die neueste Version des Images zu laden. 

Löschen Sie die lokale Version des Images von Ihrem IoT Edge-Gerät. Auf Windows-Computern werden bei der Deinstallation des Sicherheitsdaemons auch die Runtime-Images entfernt, sodass Sie diesen Schritt nicht erneut ausführen müssen. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Möglicherweise müssen Sie das force-Flag `-f` verwenden, um die Images zu entfernen. 

Der IoT Edge-Dienst lädt per Pull die neuesten Versionen der Runtimeimages herunter und startet sie automatisch wieder auf Ihrem Gerät. 

### <a name="update-a-specific-tag-image"></a>Aktualisieren eines Images mit spezifischem Tag

Wenn Sie in Ihrer Bereitstellung spezifische Tags verwenden (Beispiel: mcr.microsoft.com/azureiotedge-hub:**1.0.7**), brauchen Sie lediglich das Tag in Ihrem Bereitstellungsmanifest zu aktualisieren und die Änderungen auf Ihr Gerät anzuwenden. 

Im Azure-Portal sind die Images zum Bereitstellen der Runtime im Abschnitt **Erweiterte Einstellungen für die Edge-Laufzeit konfigurieren** deklariert. 

![Konfigurieren erweiterter Einstellungen für die Edge-Runtime](./media/how-to-update-iot-edge/configure-runtime.png)

Aktualisieren Sie in einem JSON-Bereitstellungsmanifest die Modulimages im Abschnitt **systemModules**. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.7",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.7",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="update-to-a-release-candidate-version"></a>Update für eine veröffentlichte Version eines Release Candidates

Azure IoT Edge veröffentlicht regelmäßig neue Versionen des IoT Edge-Dienstes. Vor jeder stabilen Veröffentlichung gibt es einen oder mehrere Release Candidate-Versionen (RC). Die RC-Versionen enthalten alle geplanten Funktionen für die Veröffentlichung, werden allerdings noch getestet und validiert, bis es zu einer stabilien Veröffentlichung kommt. Wenn Sie eine neue Funktion vorab testen möchten, dann können Sie die RC-Version installieren und uns Feedback über GitHub schicken. 

Release Candidate-Versionen werden genauso nummeriert, wie die Veröffentlichungen, haben jedoch **„-rc“** und eine inkrementelle Zahl am Ende. Sie finden die Release Candidates in derselben Liste der [Azure IoT Edge-Veröffentlichungen,](https://github.com/Azure/azure-iotedge/releases) wie die stabilen Versionen. Suchen Sie z. B. nach **1.0.7-rc1** und **1.0.7-rc2**. Dies sind die zwei Release Candidates, die vor **1.0.7** veröffentlicht wurden. Sie werden außerdem feststellen, dass die RC-Versionen über die Bezeichnung **„Vorabversion“** verfügen. 

Release Candidate-Versionen als Vorschauversionen sind nicht in der neusten Version enthalten, die reguläre Installationsprogramme aufrufen. Stattdessen müssen Sie die Assets für die RC-Version selber aufrufen, die Sie testen möchten. Je nach Betriebssystem Ihres IoT Edge-Geräts können sie die folgenden Abschnitte verwenden, um Ihr IoT Edge auf eine bestimmte Version zu aktualisieren:

* [Linux](how-to-install-iot-edge-linux.md#install-a-specific-runtime-version)
* [Windows](how-to-install-iot-edge-windows.md#offline-installation)

## <a name="next-steps"></a>Nächste Schritte

Zeigen Sie die neuesten [Azure IoT Edge releases](https://github.com/Azure/azure-iotedge/releases) (Azure IoT Edge-Versionen) an.

Bleiben Sie mit neuesten Updates und Ankündigungen im [Internet of Things-Blog](https://azure.microsoft.com/blog/topics/internet-of-things/) auf dem Laufenden 