---
title: Veröffentlichen und Abonnieren von Ereignissen in der Cloud – Azure Event Grid in IoT Edge | Microsoft-Dokumentation
description: Veröffentlichen und Abonnieren von Ereignissen in der Cloud mit einem Webhook über Event Grid in IoT Edge
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 5fb6cab4bfeea4308873210fb5f9122b37b61dcd
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73100327"
---
# <a name="tutorial-publish-subscribe-to-events-in-cloud"></a>Tutorial: Veröffentlichen und Abonnieren von Ereignissen in der Cloud

Dieser Artikel führt Sie durch alle Schritte, die erforderlich sind, um Ereignisse mithilfe von Event Grid in IoT Edge zu veröffentlichen und zu abonnieren.

Lesen Sie die grundlegenden Informationen zu Event Grid-Themen und Abonnements unter [Event Grid-Konzepte](concepts.md), bevor Sie fortfahren.

## <a name="prerequisites"></a>Voraussetzungen 
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein **Azure-Abonnement**: Falls Sie noch kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free). 
* **Azure IoT Hub- und IoT Edge-Gerät**: Befolgen Sie die Schritte im Schnellstart für [Linux](../../iot-edge/quickstart-linux.md)- oder [Windows-Geräte](../../iot-edge/quickstart.md), sofern nicht bereits geschehen.

[!INCLUDE [event-grid-deploy-iot-edge](../../../includes/event-grid-deploy-iot-edge.md)]

## <a name="create-an-azure-function-in-the-azure-portal"></a>Erstellen einer Azure-Funktion im Azure-Portal

Folgen Sie den Schritten in diesem [Tutorial](../../azure-functions/functions-create-first-azure-function.md), um eine Azure-Funktion zu erstellen. 

Ersetzen Sie den Codeausschnitt durch den folgenden Code:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);

    log.LogInformation($"C# HTTP trigger received {data}.");
    return data != null
        ? (ActionResult)new OkResult()
        : new BadRequestObjectResult("Please pass in the request body");
}
```

Klicken Sie in Ihrer neuen Funktion rechts oben auf **Funktions-URL abrufen**, wählen Sie die Standardeinstellung (**Funktionstaste**) aus, und klicken Sie dann auf **Kopieren**. Sie verwenden die Funktions-URL später im Tutorial.

> [!NOTE]
> Weitere Beispiele und Tutorials zur Reaktion auf Ereignisse und die Verwendung von Event Grid-Ereignistriggern finden Sie in der Dokumentation zu [Azure Functions](../../azure-functions/functions-overview.md).

## <a name="create-a-topic"></a>Erstellen eines Themas

Als Herausgeber eines Ereignisses müssen Sie ein Event Grid-Thema erstellen. Ein Thema verweist auf einen Endpunkt, an den Herausgeber Ereignisse senden können.

1. Erstellen Sie eine Datei „topic2.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
         {
          "name": "sampleTopic2",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```
1. Führen Sie den folgenden Befehl aus, um das Thema zu erstellen. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```
1. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Thema erfolgreich erstellt wurde. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```

   Beispielausgabe:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic2",
            "name": "sampleTopic2",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic2/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-an-event-subscription"></a>Erstellen eines Ereignisabonnements

Abonnenten können sich für Ereignisse registrieren, die in einem Thema veröffentlicht werden. Zum Empfang von Ereignissen müssen die Abonnenten ein Event Grid-Abonnement für das gewünschte Thema erstellen.

1. Erstellen Sie eine Datei „subscription2.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-az-func-cloud-url>"
              }
            }
          }
        }
    ```

   >[!NOTE]
   > Die **endpointType**-Eigenschaft gibt an, dass es sich bei dem Abonnenten um einen Webhook handelt.  Die **endpointUrl** gibt die URL an, an der der Abonnent auf Ereignisse lauscht. Diese URL entspricht der aus dem Azure Functions-Beispiel, das Sie zuvor eingerichtet haben.
2. Führen Sie den folgenden Befehl aus, um das Abonnement zu erstellen. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/eventSubscriptions/sampleSubscription2?api-version=2019-01-01-preview
    ```
3. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Abonnement erfolgreich erstellt wurde. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/eventSubscriptions/sampleSubscription2?api-version=2019-01-01-preview
    ```

    Beispielausgabe:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic2/eventSubscriptions/sampleSubscription2",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription2",
          "properties": {
            "Topic": "sampleTopic2",
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "<your-az-func-cloud-url>"
              }
            }
          }
        }
    ```

## <a name="publish-an-event"></a>Veröffentlichen eines Ereignisses

1. Erstellen Sie eine Datei „event2.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        [
          {
            "id": "eventId-func-1",
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
1. Führen Sie zum Veröffentlichen von Ereignissen den folgenden Befehl aus.

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event2.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Überprüfen der Ereignisübermittlung

Sie können im Azure-Portal unterhalb der Option **Überwachen** für Ihre Funktion die übermittelten Ereignisse anzeigen.

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

* Führen Sie den folgenden Befehl aus, um das Thema und alle zugehörigen Abonnements zu löschen.

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic2?api-version=2019-01-01-preview
    ```

* Löschen Sie die im Azure-Portal erstellte Azure-Funktion.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Event Grid-Thema und ein Abonnement erstellt und Ereignisse veröffentlicht. Da Sie nun mit den grundlegenden Schritten vertraut sind, sehen Sie die folgenden Artikel ein:

* Informationen zum Behandeln von Problemen bei der Verwendung von Azure Event Grid in IoT Edge finden Sie im [Leitfaden zur Problembehandlung](troubleshoot.md).
* Erstellen/Aktualisieren Sie Abonnements mit [Filtern](advanced-filtering.md).
* Richten Sie die Persistenz des Event Grid-Moduls unter [Linux](persist-state-linux.md) oder [Windows](persist-state-windows.md) ein.
* Befolgen Sie die Informationen zum Konfigurieren der Clientauthentifizierung in der [Dokumentation](configure-client-auth.md).
* Befolgen Sie die Anleitung in diesem [Tutorial](forward-events-event-grid-cloud.md), um Ereignisse an Azure Event Grid in der Cloud weiterzuleiten.
