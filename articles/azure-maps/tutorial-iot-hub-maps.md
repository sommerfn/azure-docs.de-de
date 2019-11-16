---
title: 'Tutorial: Implementieren der räumlichen IoT-Analyse mit Azure Maps'
description: 'Tutorial: Es wird beschrieben, wie Sie IoT Hub in Azure Maps-Dienst-APIs integrieren.'
author: walsehgal
ms.author: v-musehg
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: b876b27d0eb24a9eabcffe0d131ea0ef5bb79bad
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74107044"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Tutorial: Implementieren der räumlichen IoT-Analyse mit Azure Maps

Das Nachverfolgen und Erfassen relevanter Ereignisse, die in Raum und Zeit eintreten, ist ein häufiges IoT-Szenario. Beispiele hierfür sind die Bereiche Verwaltung von Fuhrparks, Asset-Nachverfolgung, Mobilität und Smart City-Anwendungen. In diesem Tutorial wird anhand von relevanten Ereignissen Schritt für Schritt ein Lösungsmuster für die Verwendung von Azure Maps-APIs beschrieben, die mit IoT Hub erfasst werden. Hierfür nutzen Sie das von Event Grid bereitgestellte Ereignisabonnementmodell.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen Sie einen IoT Hub.
> * Hochladen des Geofencebereichs in den Azure Maps-Datendienst per Datenupload-API
> * Erstellen einer Funktion in Azure Functions und Implementieren von Geschäftslogik basierend auf einer räumlichen Azure Maps-Analyse
> * Abonnieren von IoT-Gerätetelemetrieereignissen aus der Azure-Funktion per Event Grid
> * Filtern der Telemetrieereignisse per IoT Hub-Nachrichtenrouting
> * Erstellen eines Speicherkontos zum Speichern relevanter Ereignisdaten
> * Simulieren eines IoT-Geräts in einem Fahrzeug
    

## <a name="use-case"></a>Anwendungsfall

Wir stellen die Lösung anhand eines Szenarios dar, bei dem ein Mietwagenunternehmen plant, Ereignisse für seine vermieteten Fahrzeuge zu überwachen und zu protokollieren. Mietwagenunternehmen vermieten Fahrzeuge häufig für eine bestimmte geografische Region und müssen den Standort während der Vermietung nachverfolgen. Jeder Fall, in dem ein Fahrzeug die jeweilige geografische Region verlässt, muss protokolliert werden, damit Richtlinien, Gebühren und andere geschäftliche Aspekte entsprechend angewendet bzw. geregelt werden können.

In unserem Anwendungsfall sind die Mietwagen mit IoT-Geräten ausgestattet, die regelmäßig Telemetriedaten an Azure IoT Hub senden. Die Telemetriedaten umfassen den aktuellen Standort und geben an, ob der Motor des Fahrzeugs läuft. Das Schema für den Gerätestandort basiert auf dem [IoT Plug & Play-Schema für Geodaten](https://github.com/Azure/IoTPlugandPlay/blob/master/Schemas/geospatial.md). Das Gerätetelemetrieschema des Mietwagens sieht wie folgt aus:

```JSON
{
    "data": {
        "properties": {
            "Engine": "ON"
        },
        "systemProperties": {
            "iothub-content-type": "application/json",
            "iothub-content-encoding": "utf-8",
            "iothub-connection-device-id": "ContosoRentalDevice",
            "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
            "iothub-connection-auth-generation-id": "636959817064335548",
            "iothub-enqueuedtime": "2019-06-18T00:17:20.608Z",
            "iothub-message-source": "Telemetry"
        },
        "body": { 
            "location": { 
                "type": "Point",
                "coordinates": [ -77.025988698005662, 38.9015330523316 ]
            } 
        } 
    }
}
```

Die Gerätetelemetrie im Fahrzeug kann genutzt werden, um das Ziel zu erreichen. Unser Ziel besteht darin, bei jedem Ereignis, das auf eine Änderung des Fahrzeugstandorts hinweist, Geofencingregeln und eine geeignete Nachbehandlung auszuführen. Hierzu abonnieren wir die Gerätetelemetrieereignisse von IoT Hub per Event Grid, damit die gewünschte Kundengeschäftslogik nur dann ausgeführt werden kann, wenn dies angebracht ist. Es gibt mehrere Möglichkeiten, Event Grid zu abonnieren. In diesem Tutorial verwenden wir Azure Functions. Azure Functions reagiert auf in Event Grid veröffentlichte Ereignisse und implementiert die Logik für Mietwagenunternehmen basierend auf der räumlichen Azure Maps-Analyse. Die Azure-Funktion umfasst die Überprüfung, ob das Fahrzeug den Geofencebereich verlassen hat. Wenn ja, werden weitere Informationen gesammelt, z. B. die Adresse des aktuellen Standorts. Mit der Funktion wird auch Logik zum Speichern von aussagekräftigen Ereignisdaten in einem Datenblobspeicher implementiert, sodass sowohl die Analysten des Mietwagenunternehmens als auch Kunden eine genaue Beschreibung der Ereignisumstände erhalten.

Das folgende Diagramm enthält eine allgemeine Übersicht über das System.

 
  <center>

  ![Systemübersicht](./media/tutorial-iot-hub-maps/system-diagram.png)</center>

In der Abbildung unten ist der Geofencebereich blau und die Route des Mietwagens als grüne Linie dargestellt.

  ![Geofenceroute](./media/tutorial-iot-hub-maps/geofence-route.png)


## <a name="prerequisites"></a>Voraussetzungen 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie zunächst im Azure-Portal eine Ressourcengruppe erstellen. Führen Sie die folgenden Schritte aus, um eine Ressourcengruppe zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie **Ressourcengruppen**.
    
   ![Ressourcengruppen](./media/tutorial-iot-hub-maps/resource-group.png)

3. Wählen Sie unter „Ressourcengruppe“ die Option **Hinzufügen**.
    
   ![Hinzufügen von Ressourcengruppen](./media/tutorial-iot-hub-maps/add-resource-group.png) 

4. Geben Sie die folgenden Eigenschaftswerte ein:
    * **Abonnement:** Wählen Sie Ihr Azure-Abonnement aus.
    * **Ressourcengruppe:** Geben Sie „ContosoRental“ als Ressourcengruppennamen ein.
    * **Region:** Wählen Sie eine Region für die Ressourcengruppe aus.  

    ![Details zur Ressourcengruppe](./media/tutorial-iot-hub-maps/resource-details.png)

    Klicken Sie auf **Bewerten + erstellen**, und wählen Sie auf der nächsten Seite die Option **Erstellen**.

### <a name="create-an-azure-maps-account"></a>Erstellen eines Azure Maps-Kontos 

Zum Implementieren von Geschäftslogik basierend auf der räumlichen Azure Maps-Analyse müssen wir in der erstellten Ressourcengruppe ein Azure Maps-Konto erstellen. Befolgen Sie die Anleitung unter [Verwalten Ihres Azure Maps-Kontos](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), um ein Azure Maps-Kontoabonnement mit S1-Tarif zu erstellen. Führen Sie außerdem die Schritte unter [Suchen nach Points of Interest in der Nähe mit Azure Maps](./tutorial-search-location.md#getkey) aus, um den primären Abonnementschlüssel für Ihr Konto abzurufen.


### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Zum Protokollieren von Ereignisdaten erstellen wir das universelle Konto **v2storage** in der Ressourcengruppe „ContosoRental“, um Daten als Blobs zu speichern. Befolgen Sie zum Erstellen eines Speicherkontos die Anleitung unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal). Als Nächstes müssen wir einen Container zum Speichern von Blobs erstellen. Führen Sie hierzu die folgenden Schritte aus:

1. Navigieren Sie in Ihrem Speicherkonto zu „Blobs“.

    ![Blobs](./media/tutorial-iot-hub-maps/blobs.png)

2. Klicken Sie oben links auf die Containerschaltfläche, geben Sie Ihrem Container den Namen „contoso-rental-logs“, und klicken Sie auf „OK“.

    ![blob-container](./media/tutorial-iot-hub-maps/blob-container.png)

3. Navigieren Sie in Ihrem Speicherkonto zum Blatt **Zugriffsschlüssel**, und kopieren Sie den Kontonamen und den Zugriffsschlüssel, da wir diese Angaben später noch benötigen.

    ![access-keys](./media/tutorial-iot-hub-maps/access-keys.png)


Nachdem wir jetzt über ein Speicherkonto und einen Container zum Protokollieren von Ereignisdaten verfügen, erstellen wir als Nächstes eine IoT Hub-Instanz.

### <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Eine IoT Hub-Instanz ist ein verwalteter Dienst in der Cloud, der als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen einer IoT-Anwendung und den damit verwalteten Geräten dient. Zum Weiterleiten von Gerätetelemetrienachrichten an eine Event Grid-Instanz erstellen wir in der Ressourcengruppe „ContosoRental“ eine IoT Hub-Instanz und richten eine Nachrichtenroutenintegration ein. Darin filtern wir Nachrichten basierend auf dem Motorstatus des Fahrzeugs und senden Gerätetelemetrienachrichten jeweils an die Event Grid-Instanz, wenn sich das Fahrzeug bewegt. 

> [!Note] 
> Die IoT Hub-Funktionalität zum Veröffentlichen von Gerätetelemetrieereignissen über Event Grid befindet sich in der öffentlichen Vorschauphase (Public Preview). Features in der öffentlichen Vorschauphase sind in allen Regionen verfügbar, mit Ausnahme von **„USA, Osten“, „USA, Westen“, „Europa, Westen“, „Azure Government“, „Azure China 21Vianet“** und **„Azure Deutschland“** . 

Erstellen Sie eine IoT Hub-Instanz, indem Sie die Schritte im Abschnitt [Erstellen eines IoT Hubs](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub) ausführen.


### <a name="register-a-device"></a>Registrieren eines Geräts 

Zum Herstellen einer Verbindung mit der IoT Hub-Instanz muss ein Gerät registriert werden. Führen Sie die folgenden Schritte aus, um ein Gerät bei IoT Hub zu registrieren:

1. Klicken Sie auf Ihrer IoT Hub-Instanz auf das Blatt „IoT-Geräte“ und auf „Neu“.

    ![add-device](./media/tutorial-iot-hub-maps/add-device.png)

2. Geben Sie Ihrem IoT-Gerät auf der Seite für die Geräteerstellung einen Namen, und klicken Sie auf „Speichern“.
    
    ![register-device](./media/tutorial-iot-hub-maps/register-device.png)


## <a name="upload-geofence"></a>Hochladen der Geofencedaten

Wir nutzen die [Postman-Anwendung](https://www.getpostman.com) zum [Hochladen der Geofencedaten](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) in den Azure Maps-Dienst, indem wir die Data Upload-API von Azure Maps verwenden. Alle Ereignisse, bei denen sich das Fahrzeug außerhalb dieses Geofencebereichs befindet, werden protokolliert.

Öffnen Sie die Postman-App, und führen Sie die unten angegebenen Schritte aus, um die Geofencedaten mit der Data Upload-API von Azure Maps hochzuladen.  

1. Klicken Sie in der Postman-App auf „New“ | „Create new“ („Neu“ | „Neu erstellen“), und wählen Sie anschließend „Request“ (Anfordern). Geben Sie einen Anforderungsnamen für das Hochladen von Geofencedaten ein, wählen Sie eine Sammlung oder einen Ordner zum Speichern aus, und klicken Sie auf „Save“ (Speichern).

    ![Hochladen von Geofences mit Postman](./media/tutorial-iot-hub-maps/postman-new.png)

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die POST-HTTP-Methode aus, und geben Sie die folgende URL ein, um eine POST-Anforderung zu senden.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    Der „geojson“-Wert für den Parameter `dataFormat` im URL-Pfad gibt das Format der hochzuladenden Daten an.

3. Klicken Sie auf **Params**, und geben Sie die folgenden Schlüssel-Wert-Paare ein, die für die POST-Anforderungs-URL verwendet werden sollen. Ersetzen Sie den Abonnementschlüsselwert durch Ihren primären Azure Maps-Abonnementschlüssel.
   
    ![Schlüssel-Wert-Parameter: Postman](./media/tutorial-iot-hub-maps/postman-key-vals.png)

4. Klicken Sie auf **Text**, und wählen Sie dann das Rohdaten-Eingabeformat (**raw**) aus. Wählen Sie in der Dropdownliste anschließend **JSON (application/text)** als Eingabeformat aus. Öffnen Sie die JSON-Datendatei [hier](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4), und kopieren Sie den JSON-Code in Postman als hochzuladende Daten in den body-Abschnitt. Klicken Sie anschließend auf **Send** (Senden).
    
    ![Bereitstellen von Daten](./media/tutorial-iot-hub-maps/post-json-data.png)
    
5. Sehen Sie sich die Antwortheader an. Bei einer erfolgreichen Anforderung enthält der Adressheader (**Location**) den Status-URI, um den aktuellen Status der Uploadanforderung zu überprüfen. Der Status-URI hat das folgende Format. 

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


Als Nächstes erstellen wir eine Azure-Funktion in der Ressourcengruppe „ContosoRental“ und richten dann eine Nachrichtenroute in IoT Hub ein, um Gerätetelemetrienachrichten zu filtern.


## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Erstellen einer Azure-Funktion und Hinzufügen eines Event Grid-Abonnements

Azure Functions ist ein serverloser Computedienst, mit dem wir Code bedarfsgesteuert ausführen können, ohne eine Computeinfrastruktur explizit bereitstellen oder verwalten zu müssen. Weitere Informationen zu Azure Functions finden Sie in der Dokumentation zu [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview). Für die Logik, die wir in der Funktion implementieren, werden die Standortdaten der Gerätetelemetrie im Fahrzeug genutzt, um den Geofencestatus zu bewerten. Falls ein Fahrzeug den Geofencebereich verlässt, sammelt die Funktion weitere Informationen, z. B. die Adresse des Standorts, über die [Get Search Address Reverse-API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Hiermit wird eine bestimmte Standortkoordinate in eine für Menschen lesbare Adresse übersetzt. Alle relevanten Ereignisinformationen werden dann im Blobspeicher gespeichert. In Schritt 5 unten wird auf den ausführbaren Code verwiesen, mit dem diese Logik implementiert wird. Führen Sie die unten angegebenen Schritte aus, um eine Azure-Funktion zu erstellen, mit der Datenprotokolle an den Blobcontainer im Speicherkonto gesendet werden, und fügen Sie ein Event Grid-Abonnement hinzu.

1. Wählen Sie im Dashboard des Azure-Portals die Option zum Erstellen einer Ressource aus. Wählen Sie in der Liste mit den verfügbaren Ressourcentypen die Option **Compute** und dann **Funktions-App**.

    ![create-resource](./media/tutorial-iot-hub-maps/create-resource.png)

2. Geben Sie Ihrer Funktions-App auf der Seite für die Erstellung von Funktions-Apps einen Namen, wählen Sie unter **Ressourcengruppe** die Option **Vorhandene verwenden**, und wählen Sie in der Dropdownliste die Option „ContosoRental“ aus. Wählen Sie „.NET Core“ als Laufzeitstapel, unter **Speicher** die Option **Vorhandene verwenden** und in der Dropdownliste dann „contosorentaldata“ aus. Klicken Sie anschließend auf **Erstellen**.
    
    ![create-app](./media/tutorial-iot-hub-maps/rental-app.png)

3. Nach der Erstellung der App müssen wir ihr eine Funktion hinzufügen. Navigieren Sie zur Funktions-App, und klicken Sie auf **Neue Funktion**, um eine Funktion hinzuzufügen. Wählen Sie als Entwicklungsumgebung die Option **Im Portal** und dann **Weiter**.

    ![create-function](./media/tutorial-iot-hub-maps/function.png)

4. Wählen Sie **More templates** (Weitere Vorlagen), und klicken Sie auf **Finish and view templates** (Fertig stellen und Vorlagen anzeigen). 

5. Wählen Sie die Vorlage mit einem **Azure Event Grid-Trigger** aus. Installieren Sie die Erweiterungen, wenn Sie dazu aufgefordert werden, geben Sie der Funktion einen Namen, und wählen Sie **Erstellen**.

    ![function-template](./media/tutorial-iot-hub-maps/eventgrid-funct.png)

6. Kopieren Sie den [C#-Code](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) in Ihre Funktion, und klicken Sie auf **Speichern**.
 
7. Ersetzen Sie im C#-Skript die folgenden Parameter:
    * Ersetzen Sie **SUBSCRIPTION_KEY** durch den primären Abonnementschlüssel Ihres Azure Maps-Kontos.
    * Ersetzen Sie **UDID** durch die udId des hochgeladenen Geofencebereichs. 
    * Mit der Funktion **CreateBlobAsync** im Skript wird im Datenspeicherkonto ein Blob pro Ereignis erstellt. Ersetzen Sie **ACCESS_KEY**, **ACCOUNT_NAME** und **STORAGE_CONTAINER_NAME** durch den Zugriffsschlüssel bzw. Kontonamen Ihres Speicherkontos und durch den Datenspeichercontainer.

10. Klicken Sie auf **Event Grid-Abonnement hinzufügen**.
    
    ![add-event-grid](./media/tutorial-iot-hub-maps/add-egs.png)

11. Geben Sie die Abonnementdetails unter dem Namen **DETAILS ZUM EREIGNISABONNEMENT** Ihres Abonnements ein, und wählen Sie als Ereignisschema die Option „Event Grid-Schema“. Wählen Sie unter **THEMENDETAILS** die Option „Azure IoT Hub-Konten“ als Thementyp aus, und wählen Sie dasselbe Abonnement aus, das Sie zum Erstellen der Ressourcengruppe verwendet haben. Wählen Sie „ContosoRental“ als „Ressourcengruppe“ und dann die von Ihnen erstellte IoT Hub-Instanz als „Ressource“ aus. Wählen Sie **Gerätetelemetrie** als Ereignistyp aus. Nach Auswahl dieser Optionen sehen Sie, dass der „Thementyp“ automatisch in „IoT Hub“ geändert wurde.

    ![event-grid-subscription](./media/tutorial-iot-hub-maps/af-egs.png)
 

## <a name="filter-events-using-iot-hub-message-routing"></a>Filtern von Ereignissen per IoT Hub-Nachrichtenrouting

Nachdem Sie der Azure-Funktion ein Event Grid-Abonnement hinzugefügt haben, wird auf dem Blatt **Nachrichtenrouting** von IoT Hub eine Standardnachrichtenroute zu Event Grid angezeigt. Das Nachrichtenrouting ermöglicht es Ihnen, verschiedene Datentypen, z. B. Gerätetelemetrienachrichten, Gerätelebenszyklusereignisse und Änderungsereignisse für Gerätezwillinge, an verschiedene Endpunkte weiterzuleiten. Weitere Informationen zum IoT Hub-Nachrichtenrouting finden Sie unter [Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

![hub-EG-route](./media/tutorial-iot-hub-maps/hub-route.png)

In unserem Beispielszenario möchten wir alle Nachrichten herausfiltern, bei denen der Mietwagen bewegt wird. Zum Veröffentlichen dieser Gerätetelemetrieereignisse für Event Grid verwenden wir die Routingabfrage zum Filtern der Ereignisse, für die die `Engine`-Eigenschaft auf **„ON“** festgelegt ist. Es gibt verschiedene Möglichkeiten zum Abfragen von IoT-Gerät-zu-Cloud-Nachrichten. Weitere Informationen zur Syntax für das Nachrichtenrouting finden Sie unter [Abfragesyntax für das IoT Hub-Nachrichtenrouting](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax). Klicken Sie zum Erstellen einer Routingabfrage auf die Route **RouteToEventGrid**, und ersetzen Sie die **Routingabfrage** durch **„Engine='ON'“** . Klicken Sie anschließend auf **Speichern**. IoT Hub veröffentlicht jetzt nur Gerätetelemetriedaten, wenn „Engine“ auf „ON“ festgelegt ist.

![hub-EG-filter](./media/tutorial-iot-hub-maps/hub-filter.png)


## <a name="send-telemetry-data-to-iot-hub"></a>Senden von Telemetriedaten an IoT Hub

Da unsere Azure-Funktion jetzt betriebsbereit ist, senden wir Telemetriedaten an die IoT Hub-Instanz, von wo aus diese an Event Grid weitergeleitet werden. Wir verwenden eine C#-Anwendung, um Standortdaten für ein Gerät in einem Mietwagen zu simulieren. Zum Ausführen der Anwendung benötigen Sie auf Ihrem Entwicklungscomputer .NET Core SDK 2.1.0 oder höher. Führen Sie die folgenden Schritte aus, um simulierte Telemetriedaten an IoT Hub zu senden.

1. Laden Sie das C#-Projekt [rentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) herunter. 

2. Öffnen Sie die Datei „simulatedCar.cs“ in einem Text-Editor Ihrer Wahl, und ersetzen Sie den Wert von `connectionString` durch den Wert, den Sie beim Registrieren des Geräts gespeichert haben. Speichern Sie anschließend die Änderungen der Datei.
 
3. Navigieren Sie in Ihrem lokalen Terminalfenster zum Stammordner des C#-Projekts, und führen Sie den folgenden Befehl aus, um die erforderlichen Pakete für die simulierte Geräteanwendung zu installieren:
    
    ```cmd/sh
    dotnet restore
    ```

4. Führen Sie in demselben Terminal den folgenden Befehl aus, um die Anwendung für die Mietwagensimulation zu erstellen und auszuführen:

    ```cmd/sh
    dotnet run
    ```

  Ihr lokales Terminal sollte wie folgt aussehen.

  ![Terminalausgabe](./media/tutorial-iot-hub-maps/terminal.png)

Wenn Sie den Blobspeichercontainer jetzt öffnen, sollten vier Blobs für Standorte angezeigt werden, an denen sich das Fahrzeug außerhalb des Geofencebereichs befunden hat.

![Blobeingabe](./media/tutorial-iot-hub-maps/blob.png)

In der Karte unten werden vier Punkte angezeigt, an denen sich das Fahrzeug außerhalb des Geofencebereichs befunden hat. Diese Protokollierung erfolgt in regelmäßigen Intervallen.

![Anzeige von Verstößen auf Karte](./media/tutorial-iot-hub-maps/violation-map.png)

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Get Search Address Reverse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Get Geofence](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Eine vollständige Liste mit Azure Maps-REST-APIs finden Sie unter:

* [Azure Maps-REST-APIs](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Weitere Informationen zu IoT Plug & Play finden Sie in dem folgenden Artikel:

* [Dokumentation zu IoT Plug & Play (Vorschauversion)](https://docs.microsoft.com/azure/iot-pnp)

Eine Liste mit Geräten, die über eine Azure-Zertifizierung für IoT verfügen, finden Sie unter:

* [Geräte mit Azure-Zertifizierung](https://catalog.azureiotsolutions.com/)

Weitere Informationen zum Senden von Gerät-zu-Cloud-Telemetriedaten (und umgekehrt) finden Sie unter:

* [Senden von Telemetriedaten von einem Gerät](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
