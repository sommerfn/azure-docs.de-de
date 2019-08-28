---
title: Interagieren mit einem IoT Plug & Play Preview-Gerät über eine Azure IoT-Lösung | Microsoft-Dokumentation
description: Als Lösungsentwickler erfahren Sie, wie Sie das Dienst-SDK zum Interagieren mit IoT Plug & Play-Geräten verwenden.
author: YasinMSFT
ms.author: yahajiza
ms.date: 07/24/2019
ms.topic: tutorial
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: philmea
ms.openlocfilehash: 5abfe0300bd61f5ccfbfccedf16659f055eb8ad4
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878611"
---
# <a name="connect-to-and-interact-with-an-iot-plug-and-play-preview-device"></a>Herstellen einer Verbindung und Interagieren mit einem IoT Plug & Play Preview-Gerät

In dieser Anleitung erfahren Sie, wie Sie mithilfe der Beispiele im Node-Dienst-SDK die Interaktion Ihrer IoT-Lösung mit IoT Plug & Play Preview-Geräten erleben können.

Schließen Sie (falls noch nicht geschehen) zunächst den Schnellstart zum [Verbinden eines IoT Plug & Play Geräts mit Ihrer Lösung](quickstart-connect-pnp-device-solution.md) ab. In diesem Schnellstart erfahren Sie mehr über das Herunterladen und Installieren des SDK sowie das Ausführen einiger Beispiele.

Öffnen Sie vor dem Ausführen der Dienstbeispiele ein neues Terminal, rufen Sie das Stammverzeichnis Ihres geklonten Repositorys auf, navigieren Sie zum Ordner **digitaltwins/quickstarts/service**, und führen Sie den folgenden Befehl zum Installieren der Abhängigkeiten aus:

```cmd/sh
npm install
```

## <a name="run-the-service-samples"></a>Ausführen der Dienstbeispiele

In den folgenden Beispielen erfahren Sie mehr über die Funktionen des Node.js-Dienst-SDK. Stellen Sie sicher, dass die Umgebungsvariable `IOTHUB_CONNECTION_STRING` in der von Ihnen verwendeten Shell festgelegt ist:

### <a name="retrieve-a-digital-twin-and-list-the-interfaces"></a>Abrufen eines digitalen Zwillings und Auflisten der Schnittstellen

**get_digital_twin.js** ruft den Ihrem Gerät zugeordneten digitalen Zwilling ab und gibt seine Komponente in der Befehlszeile aus. Das Gerätebeispiel muss nicht ausgeführt werden, damit der Vorgang erfolgreich ist.

**get_digital_twin_interface_instance.js** ruft eine einzelne Schnittstelleninstanz des Ihrem Gerät zugeordneten digitalen Zwillings ab und gibt diese in der Befehlszeile aus. Das Gerätebeispiel musst nicht ausgeführt werden.

### <a name="get-and-set-properties-using-the-node-service-sdk"></a>Abrufen und Festlegen von Eigenschaften mit dem Node-Dienst-SDK

**update_digital_twin.js** aktualisiert mithilfe eines vollständigen Patches eine beschreibbare Eigenschaft auf dem digitalen Zwilling Ihres Geräts. Sie können mehrere Eigenschaften für mehrere Schnittstellen aktualisieren. Damit der Vorgang erfolgreich ist, muss das Gerätebeispiel zum gleichen Zeitpunkt ausgeführt werden. Bei einem erfolgreichen Vorgang gibt das Gerätebeispiel etwas über das Aktualisieren einer Eigenschaft aus, und das Dienstbeispiel gibt einen aktualisierten digitalen Zwilling im Terminal aus.

### <a name="send-a-command-and-retrieve-the-response-using-the-node-service-sdk"></a>Senden eines Befehls und Abrufen der Reaktion mit dem Node-Dienst-SDK

**invoke_command.js** ruft einen synchronen Befehl auf dem digitalen Zwilling Ihres Geräts auf. Damit der Vorgang erfolgreich ist, muss das Gerätebeispiel zum gleichen Zeitpunkt ausgeführt werden. Bei einem erfolgreichen Vorgang gibt das Gerätebeispiel etwas über das Bestätigen eines Befehls aus, und der Dienstclient gibt das Ergebnis des Befehls im Terminal aus.

### <a name="connect-to-the-public-repository-and-retrieve-a-model-definition-using-the-node-service-sdk"></a>Herstellen einer Verbindung mit dem öffentlichen Repository und Abrufen einer Modelldefinition mit dem Node-Dienst-SDK

Wenn Sie die gleichen Anweisungen wie für die Dienst- und Gerätebeispiele verwenden, müssen Sie die folgende Umgebungsvariable festlegen:

* `AZURE_IOT_MODEL_REPOSITORY_CONNECTION_STRING`

Diese Verbindungszeichenfolge finden Sie im [Azure Certified for IoT-Portal](https://preview.catalog.azureiotsolutions.com) auf der Registerkarte **Verbindungszeichenfolgen** für Ihr **Unternehmensrepository**.

Die Verbindungszeichenfolge sieht folgendermaßen aus:

```text
HostName={repo host name};RepositoryId={repo ID};SharedAccessKeyName={repo key ID};SharedAccessKey={repo key secret}
```

Nachdem Sie diese vier Umgebungsvariablen festgelegt haben, führen Sie das Beispiel auf die gleiche Weise wie die anderen Beispiele aus:

```cmd/sh
node model_repo.js
```

In diesem Beispiel wird die Schnittstelle **ModelDiscovery** heruntergeladen, und dieses Modell wird im Terminal ausgegeben.

### <a name="run-queries-in-iot-hub-based-on-capability-models-and-interfaces"></a>Ausführen von Abfragen in IoT Hub basierend auf Funktionsmodellen und Schnittstellen

Die IoT Hub-Abfragesprache unterstützt `HAS_INTERFACE` und `HAS_CAPABILITYMODEL`, wie Sie in den folgenden Beispielen sehen:

```sql
select * from devices where HAS_INTERFACE('id without version', version)
```

```sql
select * from devices where HAS_CAPABILITYMODEL('id without version', version)
```

### <a name="creating-digital-twin-routes"></a>Erstellen von Routen für digitale Zwillinge

Ihre Lösung kann Benachrichtigungen zu Änderungsereignissen beim digitalen Zwilling empfangen. Um diese Benachrichtigungen zu abonnieren, senden Sie die Benachrichtigungen mit der [IoT Hub-Routingfunktion](../iot-hub/iot-hub-devguide-endpoints.md) an einen Endpunkt wie Blob Storage, Event Hubs oder eine Service Bus-Warteschlange.

So erstellen Sie Routen für digitale Zwillinge:

1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Ressource.
1. Wählen Sie **Nachrichtenrouting** aus.
1. Klicken Sie auf der Registerkarte **Routen** auf **Hinzufügen**.
1. Geben Sie einen Wert in das Feld **Name** ein, und wählen Sie einen **Endpunkt** aus. Wenn Sie keinen Endpunkt konfiguriert haben, wählen Sie **Endpunkt hinzufügen** aus.
1. Wählen Sie im Dropdown **Datenquelle** die Option **Digital Twin Change Events** (Änderungsereignisse beim digitalen Zwilling) aus.
1. Wählen Sie **Speichern** aus.

Die folgende JSON zeigt ein Beispiel eines Änderungsereignisses bei einem digitalen Zwilling:

```json
{
  "interfaces": {
    "urn_azureiot_ModelDiscovery_DigitalTwin": {
      "name": "urn_azureiot_ModelDiscovery_DigitalTwin",
      "properties": {
        "modelInformation": {
          "reported": {
            "value": {
              "modelId": "urn:domain:capabilitymodel:TestCapability:1",
              "interfaces": {
                "MyInterfaceFoo": "urn:domain:interfaces:FooInterface:1",
                "urn_azureiot_ModelDiscovery_DigitalTwin": "urn:azureiot:ModelDiscovery:DigitalTwin:1"
              }
            }
          }
        }
      }
    },
    "MyInterfaceFoo": {
      "name": "MyInterfaceFoo",
      "properties": {
        "property_1": { "desired": { "value": "value_1" } },
        "property_2": {
          "desired": { "value": 20 },
          "reported": {
            "value": 10,
            "desiredState": {
              "code": 200,
              "version": 22,
              "subCode": 400,
              "description": ""
            }
          }
        },
        "property_3": { "reported": { "value": "value_3" } }
      }
    }
  },
  "version": 4
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit Dienstlösungen für die Interaktion mit Ihren IoT Plug & Play Geräten vertraut sind, empfehlen wir Ihnen, sich im nächsten Schritt über die [Modellermittlung](concepts-model-discovery.md) zu informieren.
