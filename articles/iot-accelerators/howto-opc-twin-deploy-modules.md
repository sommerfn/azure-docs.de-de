---
title: Bereitstellen des OPC Twin-Moduls für Azure von Grund auf | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie OPC Twin mithilfe des Blatts „IoT Edge“ im Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle von Grund auf neu bereitstellen.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 96a4afff3e58bfa1ebf661909f380aa525fea76e
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820134"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>Bereitstellen des OPC Twin-Moduls und der Abhängigkeiten von Grund auf

Das OPCTwin-Modul wird auf IoT Edge ausgeführt und stellt mehrere Edge-Dienste für die OPC Device Twin- und Registry-Dienste (Gerätezwilling und Registrierung) zur Verfügung. 

Es gibt mehrere Optionen zum Bereitstellen von Modulen für Ihr [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) Gateway, z. B.:

- [Bereitstellen von Azure IoT Edge-Modulen über das Azure-Portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [Bedarfsgerechtes Bereitstellen und Überwachen von IoT Edge-Modulen mithilfe der Azure CLI](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Weitere Informationen zu den Details und eine Anleitung zur Bereitstellung finden Sie im [GitHub-Repository](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Bereitstellungsmanifest

Alle Module werden mit einem Bereitstellungsmanifest bereitgestellt.  Ein Beispielmanifest für die Bereitstellung von [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) und [OPC Twin](https://github.com/Azure/azure-iiot-opc-twin-module) ist unten angegeben.

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "runtime": {
            "type": "docker",
            "settings": {
              "minDockerVersion": "v1.25",
              "loggingOptions": "",
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": ""
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "opctwin": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
                "createOptions": "{\"NetworkingConfig\": {\"EndpointsConfig\": {\"host\": {}}}, \"HostConfig\": {\"NetworkMode\": \"host\" }}"
              }
            },
            "opcpublisher": {
              "version": "2.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
                "createOptions": "{\"Hostname\":\"publisher\",\"Cmd\":[\"publisher\",\"--pf=./pn.json\",\"--di=60\",\"--to\",\"--aa\",\"--si=0\",\"--ms=0\"],\"ExposedPorts\":{\"62222/tcp\":{}},\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"PortBindings\":{\"62222/tcp\":[{\"HostPort\":\"62222\"}]}}}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
            "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Bereitstellen über das Azure-Portal

Die einfachste Möglichkeit zum Bereitstellen der Module auf einem Azure IoT Edge-Gatewaygerät ist die Verwendung des Azure-Portals.  

### <a name="prerequisites"></a>Voraussetzungen

1. Stellen Sie die OPC Twin-[Abhängigkeiten](howto-opc-twin-deploy-dependencies.md) bereit, und beschaffen Sie die sich ergebende `.env`-Datei. Notieren Sie sich den bereitgestellten `hub name` der Variablen `PCS_IOTHUBREACT_HUB_NAME` in der sich ergebenden `.env`-Datei.

2. Registrieren und starten Sie ein [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux)- oder [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows)-IoT Edge-Gateway, und notieren Sie sich dessen `device id`.

### <a name="deploy-to-an-edge-device"></a>Bereitstellen auf einem Edgegerät

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu Ihrem IoT Hub.

2. Wählen Sie im Menü auf der linken Seite die Option **IoT Edge**.

3. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts.

4. Wählen Sie **Module festlegen** aus.

5. Wählen Sie im Abschnitt **Bereitstellungsmodule** der Seite die Optionen **Hinzufügen** und **IoT Edge-Modul**.

6. Verwenden Sie im Dialogfeld **Benutzerdefiniertes IoT Edge-Modul** den Namen `opctwin` als Namen für das Modul, und geben Sie dann den *Image-URI* für den Container wie folgt an:

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   Verwenden Sie den folgenden JSON-Code für die *Optionen für Containererstellung*:

   ```json
   {"NetworkingConfig": {"EndpointsConfig": {"host": {}}}, "HostConfig": {"NetworkMode": "host" }}
   ```

   Füllen Sie bei Bedarf die optionalen Felder aus. Weitere Informationen zu Containererstellungsoptionen, Neustartrichtlinien und gewünschtem Status finden Sie unter [Gewünschte EdgeAgent-Eigenschaften](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Weitere Informationen zum Modulzwilling finden Sie unter [Definieren oder Aktualisieren gewünschter Eigenschaften](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. Wählen Sie **Speichern**, und wiederholen Sie Schritt **5**.  

8. Verwenden Sie im Dialogfeld „Benutzerdefiniertes IoT Edge-Modul“ den Namen `opcpublisher` als Namen für das Modul und den *Image-URI* für den Container wie folgt: 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   Verwenden Sie den folgenden JSON-Code für die *Optionen für Containererstellung*:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Wählen Sie **Speichern** und dann **Weiter**, um mit dem Abschnitt „Routen“ fortzufahren.

10. Fügen Sie auf der Registerkarte „Routen“ Folgendes hinzu: 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
      }
    }
    ```

    Wählen Sie anschließend **Weiter**.

11. Überprüfen Sie die Bereitstellungsinformationen und das Manifest.  Es sollte dem obigen Bereitstellungsmanifest ähneln.  Klicken Sie auf **Submit** (Senden).

12. Nachdem Sie die Module auf Ihrem Gerät bereitgestellt haben, können Sie sie auf der Seite **Gerätedetails** des Portals anzeigen. Auf dieser Seite werden die Namen der einzelnen bereitgestellten Modul sowie nützliche Informationen wie der Bereitstellungsstatus und der Exitcode angezeigt.

## <a name="deploying-using-azure-cli"></a>Bereitstellen per Azure CLI

### <a name="prerequisites"></a>Voraussetzungen

1. Installieren Sie die aktuelle Version der [Azure-Befehlszeilenschnittstelle (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) über [diese Seite](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="quickstart"></a>Schnellstart

1. Speichern Sie das obige Bereitstellungsmanifest in der Datei `deployment.json`.  

2. Wenden Sie die Konfiguration mit dem folgenden Befehl auf ein IoT Edge-Gerät an:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   Beim Parameter `device id` wird die Groß-/Kleinschreibung berücksichtigt. Der content-Parameter verweist auf die gespeicherte Bereitstellungsmanifestdatei. 
    ![az IoT Edge set-modules-Ausgabe](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Nachdem Sie die Module auf Ihrem Gerät bereitgestellt haben, können Sie sie mit dem folgenden Befehl anzeigen:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   Beim Parameter für die Geräte-ID wird die Groß-/Kleinschreibung berücksichtigt. ![az iot hub module-identity list-Ausgabe](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie OPC Twin von Grund auf bereitstellen, lautet unser Vorschlag für den nächsten Schritt:

> [!div class="nextstepaction"]
> [Bereitstellen von OPC Twin in einem vorhandenen Projekt](howto-opc-twin-deploy-existing.md)
