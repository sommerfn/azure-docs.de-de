---
title: 'Tutorial: Erstellen eines Geofence mit Azure Maps'
description: 'Tutorial: Richten Sie mit Azure Maps einen Geofence ein.'
author: walsehgal
ms.author: v-musehg
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 62005546c653796773083eaf625820ab532a8a2c
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74107102"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutorial: Einrichten eines Geofence mit Azure Maps

In diesem Tutorial werden die grundlegenden Schritte beschrieben, die zum Einrichten eines Geofence mit Azure Maps ausgeführt werden müssen. Im Szenario dieses Tutorials unterstützen wir Baustellenleiter bei der Überwachung von Fällen, in denen potenziell gefährliche Ausrüstung den gekennzeichneten Baustellenbereich verlässt. Auf einer Baustelle wird teure Ausrüstung verwendet, und es gelten bestimmte gesetzliche Vorschriften. Eine häufige Anforderung ist, dass die Ausrüstung innerhalb des Baustellenbereichs verbleiben muss und diesen ohne vorherige Erlaubnis nicht verlassen darf.

Wir nutzen die Datenupload-API von Azure Maps, um einen Geofence zu speichern, und die Geofence-API von Azure Maps, um den Standort der Ausrüstung relativ zum Geofence zu überprüfen. Wir verwenden Azure Event Grid, um die Ergebnisse für den Geofence zu streamen und basierend darauf eine Benachrichtigung einzurichten.
Weitere Informationen zu Event Grid finden Sie unter [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).
In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hochladen des Geofencebereichs in den Azure Maps-Datendienst per Datenupload-API
> *   Einrichten einer Event Grid-Instanz zum Verarbeiten von Geofence-Ereignissen
> *   Einrichten eines Geofence-Ereignishandlers
> *   Einrichten von Warnungen als Antwort auf Geofence-Ereignisse mit Logic Apps
> *   Verwenden von Azure Maps-Geofencedienst-APIs zum Nachverfolgen, ob sich eine bestimmte Baustellenressource innerhalb des Baustellenbereichs befindet


## <a name="prerequisites"></a>Voraussetzungen

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Befolgen Sie für die Schritte in diesem Tutorial die Anleitung unter [Verwalten Ihres Azure Maps-Kontos](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), um ein Azure Maps-Kontoabonnement mit S1-Tarif zu erstellen. Führen Sie außerdem die Schritte unter [Suchen nach Points of Interest in der Nähe mit Azure Maps](./tutorial-search-location.md#getkey) aus, um den primären Abonnementschlüssel für Ihr Konto abzurufen.

## <a name="upload-geofences"></a>Hochladen von Geofences

Wir nutzen die Anwendung Postman, um den Geofence für die Baustelle mit der Datenupload-API hochzuladen. Für dieses Tutorial setzen wir voraus, dass es einen festgelegten Baustellenbereich gibt, bei dem es sich um einen festen Parameter handelt, gegen den von der Baustellenausrüstung nicht verstoßen werden sollte. Verstöße gegen diese Begrenzung stellen eine schwerwiegende Übertretung dar, die der Baustellenleitung gemeldet wird. Es kann auch eine optimierte Gruppe mit zusätzlichen Geofences verwendet werden, um innerhalb des gesamten Baustellenbereichs je nach Planung kleinere Unterbereiche nachzuverfolgen. Wir setzen voraus, dass der übergeordnete Geofence über den untergeordneten Bereich „subsite1“ mit einem bestimmten Ablaufzeitpunkt verfügt, nach dem dieser nicht mehr gültig ist. Sie können je nach Ihren Anforderungen weitere geschachtelte Geofences erstellen. Beispielsweise kann „subsite1“ der Bereich sein, in dem während der Wochen 1 bis 4 des Zeitplans gearbeitet wird, und „subsite2“ der Bereich, in dem während der Wochen 5 bis 7 gearbeitet wird. Alle diese Fences können zu Beginn des Projekts als einzelnes Dataset geladen und verwendet werden, um Regeln anhand von Zeit und Raum nachzuverfolgen. Weitere Informationen zum Datenformat von Geofences finden Sie unter [Geofencing GeoJSON data](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) (Geofencing: GeoJSON-Daten). Weitere Informationen zum Hochladen von Daten in den Azure Maps-Dienst finden Sie unter [Data – Upload Preview](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) (Datenupload: Vorschau).

Öffnen Sie die Postman-App, und führen Sie die folgenden Schritte aus, um den Geofence der Baustelle mit der Datenupload-API von Azure Maps hochzuladen.

1. Öffnen Sie die App Postman, und klicken Sie auf „New“ > „Create new“ („Neu“ > „Neu erstellen“). Wählen Sie anschließend die Option „Request“ (Anfordern). Geben Sie einen Anforderungsnamen für das Hochladen von Geofencedaten ein, wählen Sie eine Sammlung oder einen Ordner zum Speichern aus, und klicken Sie auf „Save“ (Speichern).

    ![Hochladen von Geofences mit Postman](./media/tutorial-geofence/postman-new.png)

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die POST-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine POST-Anforderung zu senden.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    Der Parameter GEOJSON im URL-Pfad steht für das Format der Daten, die hochgeladen werden.

3. Klicken Sie auf **Params**, und geben Sie die folgenden Schlüssel-Wert-Paare ein, die für die POST-Anforderungs-URL verwendet werden sollen. Ersetzen Sie den Abonnementschlüsselwert durch Ihren primären Azure Maps-Abonnementschlüssel.
   
    ![Schlüssel-Wert-Parameter: Postman](./media/tutorial-geofence/postman-key-vals.png)

4. Klicken Sie auf **Text**, und wählen Sie dann das Rohdaten-Eingabeformat aus. Wählen Sie in der Dropdownliste anschließend „JSON“ als Eingabeformat. Geben Sie den folgenden JSON-Code als hochzuladende Daten ein:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Klicken Sie auf „Senden“, und sehen Sie sich den Antwortheader an. Bei einer erfolgreichen Anforderung enthält der Adressheader (**Location**) den Status-URI, um den aktuellen Status der Uploadanforderung zu überprüfen. Der Status-URI hat das folgende Format. 

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Kopieren Sie Ihren Status-URI, und fügen Sie den Parameter `subscription-key` an, der als Wert den Abonnementschlüssel Ihres Azure Maps-Kontos enthält. Das Format des Status-URI sollte wie folgt aussehen:

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Um den Wert von `udId` abzurufen, öffnen Sie in der Postman-App eine neue Registerkarte, wählen auf der Registerkarte „Builder“ (Generator) die HTTP-Methode „GET“ aus und führen eine GET-Anforderung für den Status-URI aus. Wenn der Datenupload erfolgreich ausgeführt wurde, wird im Antworttext ein Wert für „udId“ ausgegeben. Kopieren Sie die udId zur späteren Verwendung.

   ```JSON
   {
    "udid" : "{udId}"
   }
   ```

## <a name="set-up-an-event-handler"></a>Einrichten eines Ereignishandlers

Um den Baustellenleiter über Betreten- und Verlassen-Ereignisse des Bereichs („enter“ und „exit“) zu benachrichtigen, sollten wir einen Ereignishandler erstellen, der die Benachrichtigungen empfängt.

Wir erstellen zwei [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps)-Dienste zum Verarbeiten von enter- und exit-Ereignissen. Außerdem erstellen wir Ereignisauslöser in den Logik-Apps, die von diesen Ereignissen ausgelöst werden. Die Idee besteht darin, jeweils Benachrichtigungen (in diesem Fall in Form von E-Mails) an den Baustellenleiter zu senden, wenn Ausrüstung in den Baustellenbereich gelangt bzw. diesen verlässt. In der folgenden Abbildung ist die Erstellung einer Logik-App für das Geofence-Ereignis „enter“ dargestellt. Auf ähnliche Weise können Sie auch eine Logik-App für das Ereignis „exit“ erstellen.
Sie können auch die [Informationen zu allen unterstützten Ereignishandlern anzeigen](https://docs.microsoft.com/azure/event-grid/event-handlers).

1. Erstellen einer Logik-App im Azure-Portal

   ![Erstellen von Logik-Apps](./media/tutorial-geofence/logic-app.png)

2. Wählen Sie einen HTTP-Anforderungstrigger und dann als Aktion im Outlook-Connector die Option zum Senden einer E-Mail aus.
  
   ![Logic Apps-Schema](./media/tutorial-geofence/logic-app-schema.png)

3. Speichern Sie die Logik-App, um den HTTP-URL-Endpunkt zu generieren, und kopieren Sie die HTTP-URL.

   ![Logic Apps-Endpunkt](./media/tutorial-geofence/logic-app-endpoint.png)


## <a name="create-an-azure-maps-events-subscription"></a>Erstellen eines Abonnements für Azure Maps-Ereignisse

Azure Maps unterstützt drei Ereignistypen. Sie können sich die von Azure Maps unterstützten Ereignistypen [hier](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps) ansehen. Wir erstellen zwei verschiedene Abonnements: eins für enter- und eins für exit-Ereignisse.

Führen Sie die unten angegebenen Schritte aus, um ein Ereignisabonnement für die enter-Ereignisse von Geofences zu erstellen. Sie können exit-Ereignisse von Geofences auf ähnliche Weise abonnieren.

1. Navigieren Sie über [diesen Link im Portal](https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/dashboard/) zu Ihrem Azure Maps-Konto, und wählen Sie die Registerkarte „Ereignisse“.

   ![Azure Maps-Ereignisse](./media/tutorial-geofence/events-tab.png)

2. Wählen Sie zum Erstellen eines Ereignisabonnements auf der Seite „Ereignisse“ die Option „Ereignisabonnement“.

   ![Azure Maps-Ereignisabonnement](./media/tutorial-geofence/create-event-subscription.png)

3. Geben Sie dem Ereignisabonnement einen Namen, und abonnieren Sie den Ereignistyp „enter“. Wählen Sie nun als Endpunkttyp „Webhook“ aus, und kopieren Sie Ihren HTTP-URL-Endpunkt der Logik-App in „Endpunkt“.

   ![Ereignisabonnement](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Verwenden der Geofence-API

Sie können die Geofence-API nutzen, um zu überprüfen, ob sich ein Gerät (**device**) innerhalb oder außerhalb eines Geofencebereichs befindet. (Hinweis: Die Ausrüstung ist Teil des Status.) Wir gehen wie folgt vor, um ein besseres Verständnis der Geofence-GET-API zu entwickeln. Wir führen dafür Abfragen für verschiedene Standorte durch, an denen sich bestimmte Ausrüstung im Laufe der Zeit befunden hat. In der folgenden Abbildung sind fünf Standorte von bestimmter Ausrüstung der Baustelle mit einer eindeutigen Geräte-ID (**device id**) in chronologischer Reihenfolge dargestellt. Jeder dieser fünf Standorte wird genutzt, um zu bewerten, wie sich der Geofencestatus „enter“ und „exit“ basierend auf dem Fence jeweils ändert. Bei einer Statusänderung löst der Geofencedienst ein Ereignis aus, das von Event Grid an die Logik-App gesendet wird. Der Baustellenleiter erhält dann per E-Mail die entsprechende Benachrichtigung zum enter- bzw. exit-Vorgang.

> [!Note]
> Das obige Szenario und Verhalten basiert auf derselben Geräte-ID (**device id**) und spiegelt somit die fünf verschiedenen Standorte wider, die in der Abbildung unten dargestellt sind.

![Geofencekarte](./media/tutorial-geofence/geofence.png)

Öffnen Sie in der App Postman eine neue Registerkarte in derselben Sammlung, die Sie oben erstellt haben. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-GET-Methode aus:

Im Folgenden sind fünf HTTP-GET-Anforderungen der Geofencing-API angegeben, die jeweils über andere Standortkoordinaten der Ausrüstung in chronologischer Reihenfolge verfügen. Auf jede Anforderung folgt jeweils der Antworttext.
 
1. Standort 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Geofence-Abfrage 1](./media/tutorial-geofence/geofence-query1.png)

   In der obigen Antwort bedeutet der negative Abstand vom Geofencehauptbereich, dass sich die Ausrüstung innerhalb des Geofence befindet. Der positive Abstand vom Geofence des Unterbereichs bedeutet, dass sich die Ausrüstung außerhalb dieses Unterbereichs befindet. 

2. Standort 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Geofence-Abfrage 2](./media/tutorial-geofence/geofence-query2.png)

   Wenn Sie sich die obige JSON-Antwort genau ansehen, erkennen Sie, dass sich die Ausrüstung außerhalb des Unterbereichs und innerhalb des Hauptbereichs befindet. Es wird kein Ereignis ausgelöst und keine E-Mail gesendet.

3. Standort 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence-Abfrage 3](./media/tutorial-geofence/geofence-query3.png)

   Eine Zustandsänderung ist eingetreten, und jetzt befindet sich die Ausrüstung sowohl innerhalb des Hauptbereichs als auch innerhalb des Unterbereichs. Ein Ereignis wird veröffentlicht, und eine Benachrichtigungs-E-Mail wird an den Baustellenleiter gesendet.

4. Standort 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Geofence-Abfrage 4](./media/tutorial-geofence/geofence-query4.png)

   Wenn Sie sich die entsprechende Antwort genau ansehen, erkennen Sie, dass hier kein Ereignis veröffentlicht wird, obwohl die Ausrüstung den Geofence des Unterbereichs verlassen hat. Anhand des angegebenen Zeitpunkts des Benutzers in der GET-Anforderung können Sie sehen, dass der Geofence des Unterbereichs bezogen auf diesen Zeitpunkt abgelaufen ist und sich die Ausrüstung noch innerhalb des Geofencehauptbereichs befindet. Im Antworttext unter `expiredGeofenceGeometryId` sehen Sie auch die Geometrie-ID des Geofence-Unterbereichs.


5. Standort 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence-Abfrage 5](./media/tutorial-geofence/geofence-query5.png)

   Sie können sehen, dass die Ausrüstung den Geofence des Hauptbereichs der Baustelle verlassen hat. Ein Ereignis wird veröffentlicht, es handelt sich um einen schweren Verstoß, und eine E-Mail mit einer kritischen Warnung wird an den Baustellenleiter gesendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie einen Geofence einrichten, indem Sie ihn mit der Datenupload-API in den Datendienst von Azure Maps hochladen. Sie haben auch erfahren, wie Sie Azure Maps Event Grid nutzen, um Geofence-Ereignisse zu abonnieren und zu verarbeiten. 

* Unter [Behandeln von Inhaltstypen in Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type) finden Sie Informationen dazu, wie Sie Logic Apps zum Analysieren von JSON-Code nutzen, um eine komplexere Logik zu erstellen.
* Weitere Informationen zu Ereignishandlern in Event Grid finden Sie unter [Ereignishandler in Azure Event Grid](https://docs.microsoft.com/azure/event-grid/event-handlers).
