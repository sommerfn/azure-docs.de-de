---
title: Erstellen automatischer Bereitstellungen über die Befehlszeile – Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden der IoT-Erweiterung für Azure CLI zum Erstellen automatischer Bereitstellungen für Gruppen von IoT Edge-Geräten
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7ba38e1aa7196263b0ac64a6c92984cc3e7416a6
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72964798"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Bedarfsgerechtes Bereitstellen und Überwachen von IoT Edge-Modulen mithilfe der Azure CLI

Erstellen Sie eine **automatische IoT Edge-Bereitstellung** mithilfe der Azure-Befehlszeilenschnittstelle zum gleichzeitigen Verwalten laufender Bereitstellungen vieler Geräte. Automatische Bereitstellungen für IoT Edge sind Teil des Features [Automatische Geräteverwaltung](/azure/iot-hub/iot-hub-automatic-device-management) von IoT Hub. Bereitstellungen sind dynamische Prozesse, mit denen Sie mehrere Module auf mehreren Geräten bereitstellen, Status und Integrität der Module nachverfolgen und bei Bedarf Änderungen vornehmen können. 

Weitere Informationen finden Sie unter [Grundlegendes zu automatischen IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf](module-deployment-monitoring.md).

In diesem Artikel richten Sie die Azure CLI und die IoT-Erweiterung ein. Anschließend wird beschrieben, wie Sie mit den verfügbaren CLI-Befehlen Module auf IoT Edge-Geräten bereitstellen und den Status überwachen.

## <a name="cli-prerequisites"></a>Voraussetzungen für die Befehlszeilenschnittstelle

* Ein [IoT-Hub](../iot-hub/iot-hub-create-using-cli.md) in Ihrem Azure-Abonnement 
* [IoT Edge-Geräte](how-to-register-device.md#prerequisites-for-the-azure-cli) mit installierter IoT Edge-Runtime
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ist in Ihrer Umgebung vorhanden. Ihre Azure CLI-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az --version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt. 
* Die [IoT-Erweiterung für die Azure CLI](https://github.com/Azure/azure-iot-cli-extension) ist vorhanden.

## <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Weitere Informationen finden Sie unter [Informationen zum Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md).

Wenn Sie Module mithilfe der Azure CLI bereitstellen möchten, speichern Sie das Bereitstellungsmanifest lokal als TXT-Datei. Sie verwenden den Dateipfad im nächsten Abschnitt, wenn Sie den Befehl zum Anwenden der Konfiguration auf Ihr Gerät ausführen. 

Hier sehen Sie ein Beispiel für ein grundlegendes Bereitstellungsmanifest mit einem Modul:

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
              "registryCredentials": {
                "registryName": {
                  "username": "",
                  "password": "",
                  "address": ""
                }
              }
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": "{}"
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{}"
              }
            }
          },
          "modules": {
            "SimulatedTemperatureSensor": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                "createOptions": "{}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "route": "FROM /* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      },
      "SimulatedTemperatureSensor": {
        "properties.desired": {}
      }
    }
  }
}
```

## <a name="identify-devices-using-tags"></a>Identifizieren von Geräten mithilfe von Tags

Bevor Sie eine Bereitstellung erstellen können, müssen Sie angeben können, welche Geräte Sie ansprechen möchten. Azure IoT Edge erkennt Geräte anhand von **Tags** im Gerätezwilling. Jedes Gerät kann mehrere Tags aufweisen, die Sie auf eine für Ihre Lösung sinnvolle Weise definieren können. Wenn Sie beispielsweise einen Campus mit intelligenten Gebäuden verwalten, können Sie einem Gerät etwa die folgenden Tags hinzufügen:

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

Weitere Informationen zu Gerätezwillingen und Tags finden Sie unter [Verstehen und Verwenden von Gerätezwillingen in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md).

## <a name="create-a-deployment"></a>Erstellen einer Bereitstellung

Zum Bereitstellen von Modulen auf Ihren Zielgeräten erstellen Sie eine Bereitstellung, die sich aus dem Bereitstellungsmanifest sowie weiteren Parametern zusammensetzt. 

Führen Sie zum Erstellen einer Bereitstellung den Befehl [az group deployment create](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-create) aus:

```cli
az iot edge deployment create --deployment-id [deployment id] --hub-name [hub name] --content [file path] --labels "[labels]" --target-condition "[target query]" --priority [int]
```

Für diesen Befehl werden die folgenden Parameter verwendet: 

* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub erstellt wird. Geben Sie Ihrer Bereitstellung einen eindeutigen Namen, der bis zu 128 Kleinbuchstaben umfasst. Verwenden Sie dabei weder Leerzeichen noch die folgenden ungültigen Zeichen: `& ^ [ ] { } \ | " < > /`.
* **--hub-name**: Name des IoT-Hubs, in dem die Bereitstellung erstellt wird. Der Hub muss aus dem aktuellen Abonnement stammen. Ändern Sie Ihr aktuelles Abonnement mit dem Befehl `az account set -s [subscription name]`.
* **--content**: Dateipfad zur JSON-Datei mit dem Bereitstellungsmanifest 
* **--labels**: Fügen Sie Bezeichnungen hinzu, um Ihre Bereitstellungen im Blick zu behalten. Bezeichnungen sind Name-Wert-Paare, die Ihre Bereitstellung beschreiben. Bezeichnungen verwenden JSON-Formatierung für die Namen und Werte. Zum Beispiel, `{"HostPlatform":"Linux", "Version:"3.0.1"}`
* **--target-condition**: Geben Sie eine Zielbedingung ein, um festzulegen, auf welche Geräte diese Bereitstellung ausgerichtet werden soll. Die Bedingung basiert auf den Gerätezwillingstags oder auf den gemeldeten Gerätezwillingseigenschaften und muss dem Ausdrucksformat entsprechen. Beispiel: `tags.environment='test' and properties.reported.devicemodel='4000x'`. 
* **--priority** – ein positiver Integer. Wenn mindestens zwei Bereitstellungen auf dasselbe Gerät ausgerichtet sind, wird die Bereitstellung mit dem höchsten numerischen Wert für die Priorität angewendet.

## <a name="monitor-a-deployment"></a>Überwachen einer Bereitstellung

Mit dem Befehl [az iot edge deployment show](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show) können Sie die Details einer einzelnen Bereitstellung anzeigen:

```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
```

Für diesen Befehl werden die folgenden Parameter verwendet:
* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist
* **--hub-name**: Der Name des IoT-Hubs, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.

Überprüfen Sie die Bereitstellung im Befehlsfenster. Die Eigenschaft **metrics** enthält eine Anzahl für jede Metrik, die von den einzelnen Hubs ausgewertet wird:

* **targetedCount**: Eine Systemmetrik, die die Anzahl von Gerätezwillingen in IoT Hub angibt, die die Zielbedingung erfüllen.
* **appliedCount**: Eine Systemmetrik, die die Anzahl von Geräten angibt, auf deren Modulzwillinge in IoT Hub die Bereitstellungsinhalte angewendet wurden.
* **reportedSuccessfulCount**: Eine Gerätemetrik, die die Anzahl von IoT Edge-Geräten in der Bereitstellung angibt, für die von der IoT Edge-Clientruntime eine Erfolgsmeldung ausgegeben wurde.
* **reportedFailedCount**: Eine Gerätemetrik, die die Anzahl von IoT -Geräten in der Bereitstellung angibt, für die von der IoT Edge-Clientruntime eine Fehlermeldung ausgegeben wurde.

Sie können mit dem folgenden Befehl eine Liste der Geräte-IDs oder Objekte für jede der Metriken mithilfe des Befehls [az iot edge deployment show-metric](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-show-metric) anzeigen:

```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
```

Für diesen Befehl werden die folgenden Parameter verwendet: 
* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist
* **--metric-id**: Der Name der Metrik, für die Sie eine Liste der Geräte-IDs anzeigen möchten, z.B. `reportedFailedCount`.
* **--hub-name** – Name des IoT Hub, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.

## <a name="modify-a-deployment"></a>Ändern einer Bereitstellung

Wenn Sie Änderungen an einer Bereitstellung vornehmen, werden diese sofort auf alle Zielgeräte repliziert. 

Wenn Sie die Zielbedingung ändern, erfolgen die nachfolgend aufgeführten Anpassungen:

* Wenn ein Gerät die alte Zielbedingung nicht erfüllt, wohl aber die neue, und diese Bereitstellung die höchste Priorität für das Gerät hat, wird diese Bereitstellung auf das Gerät angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, die Zielbedingung nicht mehr erfüllt, wird diese Bereitstellung deinstalliert, und die Bereitstellung mit der nächsthöheren Priorität wird angewendet. 
* Wenn ein Gerät, auf dem diese Bereitstellung gegenwärtig ausgeführt wird, diese Zielbedingung wie auch die Zielbedingungen aller anderen Bereitstellungen nicht erfüllt, erfolgt keine Änderung auf dem Gerät. Das Gerät führt die aktuellen Module im aktuellen Zustand weiter aus, wird aber nicht mehr als Teil dieser Bereitstellung verwaltet. Sobald es die Zielbedingung einer anderen Bereitstellung erfüllt, wird diese Bereitstellung deinstalliert, und die neue Bereitstellung wird angewendet. 

Führen Sie zum Aktualisieren einer Bereitstellung den Befehl [az group deployment update](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-update) aus:

```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
```

Für diesen Befehl werden die folgenden Parameter verwendet:
* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist
* **--hub-name**: Der Name des IoT-Hubs, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.
* **--set**: Aktualisieren einer Eigenschaft in der Bereitstellung. Sie können die folgenden Eigenschaften aktualisieren:
  * targetCondition – z.B. `targetCondition=tags.location.state='Oregon'`
  * Bezeichnungen 
  * priority


## <a name="delete-a-deployment"></a>Löschen einer Bereitstellung

Wenn Sie eine Bereitstellung löschen, übernehmen alle Geräte die Bereitstellung mit der jeweils nächsthöheren Priorität. Wenn Ihre Geräte die Zielbedingung keiner anderen Bereitstellung erfüllen, werden die Module beim Löschen der Bereitstellung nicht entfernt. 

Führen Sie zum Löschen einer Bereitstellung den Befehl [az group deployment delete](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/edge/deployment?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-edge-deployment-delete) aus:

```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
```

Für diesen Befehl werden die folgenden Parameter verwendet: 
* **--deployment-id**: Der Name der Bereitstellung, die im IoT-Hub vorhanden ist
* **--hub-name**: Der Name des IoT-Hubs, in dem die Bereitstellung vorhanden ist. Der Hub muss aus dem aktuellen Abonnement stammen. Wechseln Sie mit dem Befehl `az account set -s [subscription name]` zum gewünschten Abonnement.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum [Bereitstellen von Modulen auf IoT Edge-Geräten](module-deployment-monitoring.md).
