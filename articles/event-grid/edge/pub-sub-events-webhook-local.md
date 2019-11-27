---
title: Lokales Veröffentlichen und Abonnieren von Ereignissen – Azure Event Grid in IoT Edge | Microsoft-Dokumentation
description: Lokales Veröffentlichen und Abonnieren von Ereignissen mit einem Webhook über Event Grid in IoT Edge
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/29/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 169b0c8084259ac27b466dbfd3606e465da35d99
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73098635"
---
# <a name="tutorial-publish-subscribe-to-events-locally"></a>Tutorial: Lokales Veröffentlichen und Abonnieren von Ereignissen

Dieser Artikel führt Sie durch alle Schritte, die erforderlich sind, um Ereignisse mithilfe von Event Grid in IoT Edge zu veröffentlichen und zu abonnieren.

> [!NOTE]
> Informationen zu Azure Event Grid-Themen und -Abonnements finden Sie unter [Event Grid-Konzepte](concepts.md).

## <a name="prerequisites"></a>Voraussetzungen 
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein **Azure-Abonnement**: Falls Sie noch kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free). 
* **Azure IoT Hub- und IoT Edge-Gerät**: Befolgen Sie die Schritte im Schnellstart für [Linux](../../iot-edge/quickstart-linux.md)- oder [Windows-Geräte](../../iot-edge/quickstart.md), sofern nicht bereits geschehen.

## <a name="deploy-event-grid-iot-edge-module"></a>Bereitstellen eines IoT Edge-Moduls in Event Grid

Es gibt mehrere Möglichkeiten, wie Sie Module auf einem IoT Edge-Gerät bereitstellen können. Alle sind für Azure Event Grid in IoT Edge geeignet. In diesem Artikel werden die Schritte zum Bereitstellen von Event Grid in IoT Edge über das Azure-Portal beschrieben.

>[!NOTE]
> In diesem Tutorial stellen Sie das Event Grid-Modul ohne Persistenz bereit. Dies bedeutet, dass alle in diesem Tutorial erstellten Themen und Abonnements beim erneuten Bereitstellen des Moduls gelöscht werden. Weitere Informationen zum Einrichten von Persistenz finden Sie in den folgenden Artikeln: [Beibehalten des Status in Linux](persist-state-linux.md) oder [Beibehalten des Status in Windows](persist-state-windows.md). Für Produktionsworkloads empfiehlt es sich, das Event Grid-Modul mit Persistenz zu installieren.


### <a name="select-your-iot-edge-device"></a>Auswählen Ihres IoT Edge-Geräts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Navigieren Sie zu Ihrer IoT Hub-Instanz.
1. Wählen Sie im Menü im Abschnitt **Automatische Geräteverwaltung** die Option **IoT Edge** aus. 
1. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts.
1. Wählen Sie **Module festlegen** aus. Lassen Sie die Seite geöffnet. Sie fahren mit den Schritten im nächsten Abschnitt fort.

### <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Das Azure-Portal verfügt über einen Assistenten, der Sie durch die Erstellung eines Bereitstellungsmanifests führt, damit Sie das JSON-Dokument nicht manuell erstellen müssen.  Der Assistent umfasst drei Schritte: **Hinzufügen von Modulen**, **Angeben von Routen** und **Überprüfen der Bereitstellung**.

### <a name="add-modules"></a>Hinzufügen von Modulen

1. Wählen Sie im Abschnitt **Bereitstellungsmodule** die Option **Hinzufügen** aus.
1. Wählen Sie in der Dropdownliste mit den Modultypen den Eintrag **IoT Edge-Modul** aus.
1. Geben Sie den Namen, das Image und die Optionen für die Containererstellung für den Container an:

   * **Name**: eventgridmodule
   * **Image-URI**: `mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Optionen für Containererstellung**:

    ```json
        {
          "Env": [
            "inbound:clientAuth:clientCert:enabled=false",
            "outbound:webhook:httpsOnly=false"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
    ```    
 1. Klicken Sie unten auf der Seite auf **Speichern**.
 1. Fahren Sie mit dem nächsten Abschnitt fort, um vor der gemeinsamen Bereitstellung das Azure Functions-Modul hinzuzufügen.

    >[!IMPORTANT]
    > In diesem Tutorial stellen Sie das Event Grid-Modul mit deaktivierter Clientauthentifizierung bereit und lassen HTTP-Abonnenten zu. Für Produktionsworkloads empfiehlt es sich, dass Sie die Clientauthentifizierung aktivieren und ausschließlich HTTPS-Abonnenten zulassen. Weitere Informationen zum sicheren Konfigurieren des Event Grid-Moduls finden Sie unter [Sicherheit und Authentifizierung](security-authentication.md).
    > 
    > Wenn Sie eine Azure-VM als Edge-Gerät verwenden, fügen Sie eine Portregel hinzu, um eingehenden Datenverkehr an Port 4438 zuzulassen. Anweisungen zum Hinzufügen der Regel finden Sie unter [Öffnen von Ports für einen virtuellen Computer](../../virtual-machines/windows/nsg-quickstart-portal.md).
    

## <a name="deploy-azure-function-iot-edge-module"></a>Bereitstellen des Azure Functions-IoT Edge-Moduls

In diesem Abschnitt erfahren Sie, wie Sie das Azure Functions-IoT-Modul bereitstellen, das als Event Grid-Mitwirkender fungiert und an das Ereignisse übermittelt werden können.

>[!IMPORTANT]
>In diesem Abschnitt stellen Sie als Beispiel ein Azure Functions-basiertes Abonnementmodul bereit. Es kann sich natürlich um ein beliebiges benutzerdefiniertes IoT-Modul handeln, das auf HTTP POST-Anforderungen lauschen kann.


### <a name="add-modules"></a>Hinzufügen von Modulen

1. Wählen Sie im Abschnitt **Bereitstellungsmodule** erneut die Option **Hinzufügen** aus. 
1. Wählen Sie in der Dropdownliste mit den Modultypen den Eintrag **IoT Edge-Modul** aus.
1. Geben Sie den Namen, das Image und die Optionen für die Containererstellung für den Container an:

   * **Name**: subscriber
   * **Image-URI**: `mcr.microsoft.com/azure-event-grid/iotedge-samplesubscriber-azfunc:latest`
   * **Optionen für Containererstellung**:

       ```json
            {
              "HostConfig": {
                "PortBindings": {
                  "80/tcp": [
                    {
                      "HostPort": "8080"
                    }
                  ]
                }
              }
            }
       ```

1. Klicken Sie unten auf der Seite auf **Speichern**.
1. Klicken Sie auf **Weiter**, um mit dem Abschnitt für Routen fortzufahren.

 ### <a name="setup-routes"></a>Einrichten von Routen

Behalten Sie die Standardrouten bei, und klicken Sie auf **Weiter**, um mit dem Abschnitt für die Überprüfung fortzufahren.

### <a name="submit-the-deployment-request"></a>Übermitteln der Bereitstellungsanforderung

1. Im Abschnitt zur Überprüfung wird das JSON-Bereitstellungsmanifest angezeigt, das basierend auf Ihrer Auswahl in den vorherigen Abschnitten erstellt wurde. Bestätigen Sie, dass beide der in der JSON aufgeführten Module angezeigt werden: **eventgridmodule** und **subscriber**. 
1. Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**. Nach Übermittlung der Bereitstellung wird wieder die Seite **Gerät** angezeigt.
1. Überprüfen Sie im Abschnitt**Module**, dass die beiden Module **eventgrid** und **subscriber** aufgelistet sind. Stellen Sie außerdem sicher, dass die Spalten **In Bereitstellung angegeben** und **Vom Gerät gemeldet** auf **Ja** festgelegt sind.

    Unter Umständen dauert es einen Moment, bis das Modul auf dem Gerät gestartet und an IoT Hub gemeldet wurde. Aktualisieren Sie die Seite, um einen aktualisierten Status anzuzeigen.

## <a name="create-a-topic"></a>Erstellen eines Themas

Als Herausgeber eines Ereignisses müssen Sie ein Event Grid-Thema erstellen. In Azure Event Grid verweist ein Thema auf einen Endpunkt, an den Herausgeber Ereignisse senden können.

1. Erstellen Sie Datei „topic.json“ mit dem folgendem Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        {
          "name": "sampleTopic1",
          "properties": {
            "inputschema": "eventGridSchema"
          }
        }
    ```

1. Führen Sie den folgenden Befehl aus, um ein Event Grid-Thema zu erstellen. Vergewissern Sie sich, dass der HTTP-Statuscode `200 OK` angezeigt wird.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @topic.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```

1. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Thema erfolgreich erstellt wurde. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```

   Beispielausgabe:

   ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic1",
            "name": "sampleTopic1",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/sampleTopic1/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
   ```

## <a name="create-an-event-subscription"></a>Erstellen eines Ereignisabonnements

Abonnenten können sich für Ereignisse registrieren, die in einem Thema veröffentlicht werden. Zum Empfang von Ereignissen müssen Sie ein Event Grid-Abonnement für das gewünschte Thema erstellen.

1. Erstellen Sie eine Datei „subscription.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "http://subscriber:80/api/subscriber"
              }
            }
          }
        }
    ```

    >[!NOTE]
    > Die **endpointType**-Eigenschaft gibt an, dass es sich bei dem Abonnenten um einen **Webhook** handelt.  Die **endpointUrl** gibt die URL an, an der der Abonnent auf Ereignisse lauscht. Diese URL entspricht der aus dem Azure Functions-Beispiel, das Sie zuvor bereitgestellt haben.
2. Führen Sie den folgenden Befehl aus, um ein Abonnement für das Thema zu erstellen. Vergewissern Sie sich, dass der HTTP-Statuscode `200 OK` angezeigt wird.

    ```sh
    curl -k -H "Content-Type: application/json" -X PUT -g -d @subscription.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/eventSubscriptions/sampleSubscription1?api-version=2019-01-01-preview
    ```
3. Führen Sie den folgenden Befehl aus, um zu bestätigen, dass das Abonnement erfolgreich erstellt wurde. Es sollte der HTTP-Statuscode „200 OK“ zurückgegeben werden.

    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/eventSubscriptions/sampleSubscription1?api-version=2019-01-01-preview
    ```

    Beispielausgabe:

   ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/sampleTopic1/eventSubscriptions/sampleSubscription1",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription1",
          "properties": {
            "Topic": "sampleTopic1",
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "http://subscriber:80/api/subscriber"
              }
            }
          }
        }
    ```

## <a name="publish-an-event"></a>Veröffentlichen eines Ereignisses

1. Erstellen Sie eine Datei „event.json“ mit dem folgenden Inhalt. Ausführliche Informationen zur Nutzlast finden Sie in unserer [API-Dokumentation](api.md).

    ```json
        [
          {
            "id": "eventId-func-0",
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
1. Führen Sie den folgenden Befehl aus, um ein Ereignis zu veröffentlichen.

    ```sh
    curl -k -H "Content-Type: application/json" -X POST -g -d @event.json https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1/events?api-version=2019-01-01-preview
    ```

## <a name="verify-event-delivery"></a>Überprüfen der Ereignisübermittlung

1. Stellen Sie über SSH oder RDP eine Verbindung mit Ihrer IoT Edge-VM her.
1. Überprüfen Sie die folgenden Abonnentenprotokolle:

    Führen Sie unter Windows den folgenden Befehl aus:

    ```sh
    docker -H npipe:////./pipe/iotedge_moby_engine container logs subscriber
    ```

   Führen Sie unter Linux den folgenden Befehl aus:

    ```sh
    sudo docker logs subscriber
    ```

    Beispielausgabe:

    ```sh
        Received event data [
            {
              "id": "eventId-func-0",
              "topic": "sampleTopic1",
              "subject": "myapp/vehicles/motorcycles",
              "eventType": "recordInserted",
              "eventTime": "2019-07-28T21:03:07+00:00",
              "dataVersion": "1.0",
              "metadataVersion": "1",
              "data": {
                "make": "Ducati",
                "model": "Monster"
              }
            }
          ]
    ```

## <a name="cleanup-resources"></a>Bereinigen von Ressourcen

* Führen Sie den folgenden Befehl aus, um das Thema und alle zugehörigen Abonnements zu löschen.

    ```sh
    curl -k -H "Content-Type: application/json" -X DELETE https://<your-edge-device-public-ip-here>:4438/topics/sampleTopic1?api-version=2019-01-01-preview
    ```
* Löschen Sie das Abonnentenmodul von Ihrem IoT Edge-Gerät.


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Event Grid-Thema und ein Abonnement erstellt und Ereignisse veröffentlicht. Da Sie nun mit den grundlegenden Schritten vertraut sind, sehen Sie die folgenden Artikel ein: 

- Informationen zum Behandeln von Problemen bei der Verwendung von Azure Event Grid in IoT Edge finden Sie im [Leitfaden zur Problembehandlung](troubleshoot.md).
- Erstellen/Aktualisieren Sie Abonnements mit [Filtern](advanced-filtering.md).
- Richten Sie Persistenz für das Event Grid-Modul unter [Linux](persist-state-linux.md) oder [Windows](persist-state-windows.md) ein.
- Befolgen Sie die Informationen zum Konfigurieren der Clientauthentifizierung in der [Dokumentation](configure-client-auth.md).
- Arbeiten Sie dieses [Tutorial](pub-sub-events-webhook-cloud.md) durch, um Ereignisse an Azure Functions in der Cloud weiterzuleiten.
- Erfahren Sie mehr über das [Reagieren auf Blob Storage-Ereignisse in IoT Edge](react-blob-storage-events-locally.md).

