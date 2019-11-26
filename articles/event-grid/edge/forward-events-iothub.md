---
title: Weiterleiten von Event Grid-Ereignissen an IoTHub – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Weiterleiten von Event Grid-Ereignissen an IoTHub
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 073205b5bdc3f6de80bd7e347469c3f06aeb515b
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73098682"
---
# <a name="tutorial-forward-events-to-iothub"></a>Tutorial: Weiterleiten von Ereignissen an IoTHub

In diesem Artikel werden die Schritte erläutert, die erforderlich sind, um Event Grid-Ereignisse mit Routen an andere IoT Edge-Module (IoTHub) weiterzuleiten. Dies kann sich aus folgenden Gründen empfehlen:

* Weiteres Verwenden bereits vorhandener Investitionen mit dem Routing von edgeHub
* Bevorzugtes Weiterleiten sämtlicher Ereignisse von einem Gerät ausschließlich über IoT Hub

Zum Durcharbeiten dieses Tutorials müssen Sie die folgenden Konzepte verstehen:

- [Event Grid – Begriffe](concepts.md)
- [IoT Edge-Hub](../../iot-edge/module-composition.md) 

## <a name="prerequisites"></a>Voraussetzungen 
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein **Azure-Abonnement**: Falls Sie noch kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free). 
* **Azure IoT Hub- und IoT Edge-Gerät**: Befolgen Sie die Schritte im Schnellstart für [Linux](../../iot-edge/quickstart-linux.md)- oder [Windows-Geräte](../../iot-edge/quickstart.md), sofern nicht bereits geschehen.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)]

## <a name="create-topic"></a>Thema erstellen

Als Herausgeber eines Ereignisses müssen Sie ein Event Grid-Thema erstellen. Das Thema verweist auf einen Endpunkt, an den Herausgeber Ereignisse senden können.

1. Erstellen Sie die Datei „topic4.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

   ```json
    {
          "name": "sampleTopic4",
          "properties": {
            "inputschema": "eventGridSchema"
          }
    }
    ```
1. Führen Sie den folgenden Befehl aus, um das Thema zu erstellen. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```

1. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Thema erfolgreich erstellt wurde. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```

   Beispielausgabe:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic4",
            "name": "sampleTopic4",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic4/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-event-subscription"></a>Erstellen eines Ereignisabonnements

Abonnenten können sich für Ereignisse registrieren, die in einem Thema veröffentlicht werden. Zum Empfangen von Ereignissen müssen sie ein Event Grid-Abonnement für das gewünschte Thema erstellen.

1. Erstellen Sie die Datei „subscription4.json“ mit dem unten stehenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

   ```json
    {
          "properties": {
                "destination": {
                      "endpointType": "edgeHub",
                      "properties": {
                            "outputName": "sampleSub4"
                      }
                }
          }
    }
   ```

   >[!NOTE]
   > Der `endpointType` gibt an, dass der Abonnement `edgeHub` ist. Der `outputName` gibt Ausgabe an, an die das Event Grid-Modul Ereignisse weiterleitet, die dem edgeHub-Abonnement entsprechen. So werden beispielsweise Ereignisse, die dem obigen Abonnement entsprechen, in `/messages/modules/eventgridmodule/outputs/sampleSub4` geschrieben.
2. Führen Sie den folgenden Befehl aus, um das Abonnement zu erstellen. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/eventSubscriptions/sampleSubscription4?api-version=2019-01-01-preview
    ```
3. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Abonnement erfolgreich erstellt wurde. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/eventSubscriptions/sampleSubscription4?api-version=2019-01-01-preview
    ```

    Beispielausgabe:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic4/eventSubscriptions/sampleSubscription4",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription4",
          "properties": {
            "Topic": "sampleTopic4",
            "destination": {
                      "endpointType": "edgeHub",
                      "properties": {
                            "outputName": "sampleSub4"
                      }
                }
          }
        }
    ```

## <a name="set-up-an-edge-hub-route"></a>Einrichten einer Edge Hub-Route

Aktualisieren Sie die Edge Hub-Route für die Weiterleitung von Ereignisses des Ereignisabonnements an IoTHub wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://ms.portal.azure.com)
1. Navigieren Sie zur Instanz von **IoT Hub**.
1. Wählen Sie im Menü **IoT Edge** aus.
1. Wählen Sie in der Liste der Geräte die ID des Zielgeräts aus.
1. Wählen Sie **Module festlegen** aus.
1. Klicken Sie auf **Weiter**, und wechseln Sie zum Abschnitt über Routen.
1. Fügen Sie in den Routen eine neue Route hinzu.

  ```sh
  "fromEventGridToIoTHub":"FROM /messages/modules/eventgridmodule/outputs/sampleSub4 INTO $upstream"
  ```

  Beispiel:

  ```json
  {
      "routes": {
        "fromEventGridToIoTHub": "FROM /messages/modules/eventgridmodule/outputs/sampleSub4 INTO $upstream"
      }
  }
  ```

   >[!NOTE]
   > Die obige Route leitet für dieses Abonnement gefundenen Ereignisse an den IoT-Hub weiter. Mithilfe der Features für das [Edge Hub-Routing](../../iot-edge/module-composition.md) können Sie die Event Grid-Ereignisse weiter filtern und an andere IoT Edge-Module weiterleiten.

## <a name="setup-iot-hub-route"></a>Einrichten einer IoT Hub-Route

Sehen Sie das [Tutorial zum IoT Hub-Routing](../../iot-hub/tutorial-routing.md) ein, um eine Route vom IoT Hub einzurichten, sodass Sie vom Event Grid-Modul weitergeleitete Ereignisse anzeigen können. Verwenden Sie `true` für die Abfrage, um das Tutorial einfach zu halten.  



## <a name="publish-an-event"></a>Veröffentlichen eines Ereignisses

1. Erstellen Sie die Datei „event4.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        [
          {
            "id": "eventId-iothub-1",
            "eventType": "recordInserted",
            "subject": "myapp/vehicles/motorcycles",
            "eventTime": "2019-07-28T21:03:07+00:00",
            "dataVersion": "1.0",
            "data": {
              "make": "Ducati",
              "model": "Monster"
            }
          }
        ]
    ```

1. Führen Sie zum Veröffentlichen von Ereignissen den folgenden Befehl aus:

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event4.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Überprüfen der Ereignisübermittlung

Die Schritte zum Anzeigen von Ereignissen finden Sie im [Routing-Tutorial](../../iot-hub/tutorial-routing.md) für IoT Hub.

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

* Führen Sie den folgenden Befehl aus, um das Thema und alle zugehörigen Abonnements am Edge zu löschen:

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic4?api-version=2019-01-01-preview
    ```
* Löschen Sie auch alle Ressourcen, die Sie beim Einrichten des IoTHub-Routings in der Cloud erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Event Grid-Thema und ein Edge Hub-Abonnement erstellt und Ereignisse veröffentlicht. Da Sie nun mit den grundlegenden Schritten zum Weiterleiten an einen Edge Hub vertraut sind, sehen Sie die folgenden Artikel ein:

* Informationen zum Behandeln von Problemen bei der Verwendung von Azure Event Grid in IoT Edge finden Sie im [Leitfaden zur Problembehandlung](troubleshoot.md).
* Verwenden Sie [Edge Hub](../../iot-edge/module-composition.md)-Routenfilter, um Ereignisse zu partitionieren.
* Richten Sie die Persistenz des Event Grid-Moduls unter [Linux](persist-state-linux.md) oder [Windows](persist-state-windows.md) ein.
* Sehen Sie die [Dokumentation](configure-client-auth.md) ein, um die Clientauthentifizierung zu konfigurieren.
* Befolgen Sie die Anleitung in diesem [Tutorial](forward-events-event-grid-cloud.md), um Ereignisse an Azure Event Grid in der Cloud weiterzuleiten.
