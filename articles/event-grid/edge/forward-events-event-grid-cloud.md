---
title: Weiterleiten von Edge-Ereignissen an die Event Grid-Cloud – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Weiterleiten von Edge-Ereignissen an die Event Grid-Cloud
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 502a495bad4115daf9f0f4ffed276a307adf1fc4
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73100651"
---
# <a name="tutorial-forward-events-to-event-grid-cloud"></a>Tutorial: Weiterleiten von Ereignissen an die Event Grid-Cloud

In diesem Artikel werden die Schritte erläutert, die erforderlich sind, um Edge-Ereignisse an Event Grid in der Azure-Cloud weiterzuleiten. Dies kann sich aus folgenden Gründen empfehlen:

* Reagieren auf Edge-Ereignisse in der Cloud.
* Weiterleiten von Ereignissen an Event Grid in der Cloud und Verwenden von Azure Event Hubs oder Azure Storage-Warteschlangen, um Ereignisse vor der Verarbeitung in der Cloud zu puffern.

Zur Durchführung dieses Tutorials müssen Sie mit den Event Grid-Konzepten in [Edge](concepts.md) und [Azure](../concepts.md) vertraut sein.

## <a name="prerequisites"></a>Voraussetzungen 
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein **Azure-Abonnement**: Falls Sie noch kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free). 
* **Azure IoT Hub- und IoT Edge-Gerät**: Befolgen Sie die Schritte im Schnellstart für [Linux](../../iot-edge/quickstart-linux.md)- oder [Windows-Geräte](../../iot-edge/quickstart.md), sofern nicht bereits geschehen.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)] 
## <a name="create-event-grid-topic-and-subscription-in-cloud"></a>Erstellen eines Event Grid-Themas und -Abonnements in der Cloud

Erstellen Sie ein Event Grid-Thema und -Abonnement in der Cloud, indem Sie [dieses Tutorial](../custom-event-quickstart-portal.md) absolvieren. Notieren Sie sich `topicURL`, `sasKey` und `topicName` des neu erstellten Themas, das Sie später in diesem Tutorial verwenden werden.

Wenn Sie z. B. ein Thema namens `testegcloudtopic` in der Region „USA, Westen“ erstellt haben, würden die Werte in etwa wie folgt aussehen:

* **TopicUrl**: `https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events`
* **TopicName**: `testegcloudtopic`
* **SasKey**: Verfügbar unter **AccessKey** Ihres Themas. Verwenden Sie **key1**.

## <a name="create-event-grid-topic-at-the-edge"></a>Erstellen eines Event Grid-Themas an der Edge

1. Erstellen Sie die Datei „topic3.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        {
          "name": "sampleTopic3",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```
1. Führen Sie den folgenden Befehl aus, um das Thema zu erstellen. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```
1. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Thema erfolgreich erstellt wurde. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```

   Beispielausgabe:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic3",
            "name": "sampleTopic3",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic3/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```
  
## <a name="create-event-grid-subscription-at-the-edge"></a>Erstellen eines Event Grid-Abonnements auf dem Edge


1. Erstellen Sie eine Datei „subscription3.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

   ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                        "endpointUrl": "<your-event-grid-cloud-topic-endpoint-url>?api-version=2018-01-01",
                        "sasKey": "<your-event-grid-topic-saskey>",
                        "topicName": null
              }
            }
          }
    }
   ```

   >[!NOTE]
   > Die **endpointUrl** gibt an, dass sich die URL des Event Grid-Themas in der Cloud befindet. **sasKey** verweist auf den Schlüssel des Event Grid-Cloudthemas. Der Wert in **topicName** wird verwendet, um alle an Event Grid ausgehenden Ereignisse zu stempeln. Dies kann nützlich sein, wenn Sie in einer Event Grid-Domäne veröffentlichen. Weitere Informationen zum Event Grid-Domänenthema finden Sie unter [Ereignisdomänen](../event-domains.md).

    Beispiel:
  
    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                "endpointUrl": "https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01",
                 "sasKey": "<your-event-grid-topic-saskey>",
                 "topicName": null
              }
            }
          }
        }
    ```

2. Führen Sie den folgenden Befehl aus, um das Abonnement zu erstellen. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

     ```sh
     curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/eventSubscriptions/sampleSubscription3?api-version=2019-01-01-preview
     ```

3. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Abonnement erfolgreich erstellt wurde. Der HTTP-Statuscode „200 OK“ sollte zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/eventSubscriptions/sampleSubscription3?api-version=2019-01-01-preview
    ```

    Beispielausgabe:

    ```json
         {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic3/eventSubscriptions/sampleSubscription3",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription3",
          "properties": {
            "Topic": "sampleTopic3",
            "destination": {
              "endpointType": "eventGrid",
              "properties": {
                "endpointUrl": "https://testegcloudtopic.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01",
                "sasKey": "<your-event-grid-topic-saskey>",
                "topicName": null
              }
            }
          }
        }
    ```

## <a name="publish-an-event-at-the-edge"></a>Veröffentlichen eines Ereignisses an der Edge

1. Erstellen Sie die Datei „event3.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in der [API-Dokumentation](api.md).

    ```json
        [
          {
            "id": "eventId-egcloud-0",
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

1. Führen Sie den folgenden Befehl aus:

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event3.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3/events?api-version=2019-01-01-preview
    ```

## <a name="verify-edge-event-in-cloud"></a>Überprüfen des Edge-Ereignisses in der Cloud

Informationen zum Anzeigen von Ereignissen, die vom Cloudthema übermittelt werden, finden Sie im [Tutorial](../custom-event-quickstart-portal.md).

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

* Führen Sie den folgenden Befehl aus, um das Thema und alle zugehörigen Abonnements zu löschen.

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic3?api-version=2019-01-01-preview
    ```

* Löschen Sie auch die in der Cloud (Azure Event Grid) erstellten Themen und Abonnements.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Ereignis auf dem Edge veröffentlicht und an Event Grid in der Azure-Cloud weitergeleitet. Da Sie nun mit den grundlegenden Schritten zum Weiterleiten an Event Grid in der Cloud vertraut sind:

* Informationen zum Behandeln von Problemen bei der Verwendung von Azure Event Grid in IoT Edge finden Sie im [Leitfaden zur Problembehandlung](troubleshoot.md).
* Arbeiten Sie dieses [Tutorial](forward-events-iothub.md) durch, um Ereignisse an IoTHub weiterzuleiten.
* Arbeiten Sie dieses [Tutorial](pub-sub-events-webhook-cloud.md) durch, um Ereignisse an Webhook in der Cloud weiterzuleiten.
