---
title: Erfassen historischer Telemetriedaten
description: Hier erfahren Sie, wie Sie historische Telemetriedaten erfassen.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 0ff9e055ecc0c4f58e4b3df0494debbe3f4cd8a4
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797283"
---
# <a name="ingest-historical-telemetry-data"></a>Erfassen historischer Telemetriedaten

In diesem Artikel erfahren Sie, wie Sie historische Sensordaten in Azure FarmBeats erfassen.

Die Erfassung historischer Daten aus dem Internet der Dinge (Internet of Things, IoT) für Ressourcen wie Geräte und Sensoren ist ein gängiges FarmBeats-Szenario. Sie erstellen Metadaten für Geräte und Sensoren und erfassen dann die historischen Daten in einem kanonischen Format in FarmBeats.

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel muss FarmBeats installiert sein, und Sie müssen bereits historische IoT-Daten gesammelt haben.

## <a name="enable-partner-access"></a>Aktivieren von Partnerzugriff

Sie müssen die Partnerintegration für Ihre Azure FarmBeats-Instanz aktivieren. Durch diesen Schritt wird ein Client erstellt, der als Gerätepartner Zugriff auf Ihre Azure FarmBeats-Instanz hat und folgende Werte bereitstellt, die in den weiteren Schritten benötigt werden.

- API-Endpunkt: Hierbei handelt es sich um die Datenhub-URL (beispielsweise „https://<datahub>.azurewebsites.net“).
- Mandanten-ID
- Client-ID
- Geheimer Clientschlüssel
- Event Hub-Verbindungszeichenfolge

Führen Sie die folgenden Schritte aus, um diese Werte zu generieren:

>[!NOTE]
> Die folgenden Schritte müssen von einem Administrator ausgeführt werden.

1. Laden Sie [dieses Skript](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie es auf Ihrem lokalen Laufwerk. Die ZIP-Datei enthält zwei Dateien.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie Cloud Shell. (Die entsprechende Option finden Sie rechts oben auf der Leiste des Portals.)  

    ![FarmBeats-Projekt](./media/for-tutorials/navigation-bar-1.png)

3. Vergewissern Sie sich, dass die Umgebung auf **PowerShell** festgelegt ist.

    ![FarmBeats-Projekt](./media/for-tutorials/power-shell-new-1.png)

4. Laden Sie die beiden Dateien, die Sie weiter oben im ersten Schritt heruntergeladen haben, in Ihre Cloud Shell-Instanz hoch.  

    ![FarmBeats-Projekt](./media/for-tutorials/power-shell-two-1.png)

5. Navigieren Sie zu dem Verzeichnis, in das die Dateien hochgeladen wurden (standardmäßig das Basisverzeichnis „/home/<Benutzername>/“).
6. Führen Sie das Skript mithilfe des folgenden Befehls aus:  

    ```azurepowershell-interactive
    PS> ./generateCredentials.ps1
    ```

7. Folgen Sie den Anweisungen auf dem Bildschirm, um die Prozedur abzuschließen.

    Sollten Sie keinen Zugriff auf FarmBeats oder Ihr Azure-Abonnement haben, wenden Sie sich an den FarmBeats-Administrator.

## <a name="create-devicesensor-metadata"></a>Erstellen von Geräte-/Sensormetadaten

 Nachdem Sie nun über die erforderlichen Anmeldeinformationen verfügen, können Sie das Gerät und die Sensoren definieren, indem Sie mithilfe von FarmBeats-APIs die Metadaten erstellen.

 Der FarmBeats-Datenhub bietet folgende APIs für die Erstellung und Verwaltung von Geräte-/Sensormetadaten:   

- /**DeviceModel**: Die Metadaten des Geräts. Hierzu zählen etwa der Hersteller und die Art des Geräts (Gateway oder Knoten).  
- /**Device**: Ein physisches Gerät des landwirtschaftlichen Betriebs.  
- /**SensorModel**: Die Metadaten des Sensors. Hierzu zählen beispielsweise der Hersteller, die Art des Sensors (analog oder digital) und die Art der Messung (etwa Umgebungstemperatur oder Druck).
- /**Sensor**: Ein physischer Sensor, der Werte erfasst. Ein Sensor ist in der Regel mit einem Gerät mit einer Geräte-ID verbunden.  


|        Gerätemodus   |  Vorschläge   |
| ------- | -------             |
|     Type (Node, Gateway)        |          Ein Stern      |
|          Hersteller            |         Zwei Sterne     |
|  ProductCode                    |  Produktcode oder Modellname/-nummer des Geräts. Beispiel: EnviroMonitor#6800.  |
|            Ports          |     Portname und -typ (digital/analog)
|     NAME                 |  Name zur Identifizierung der Ressource. Beispiel: Modellname/Produktname.
      BESCHREIBUNG     | Aussagekräftige Beschreibung des Modells
|    Properties          |    Zusätzliche Eigenschaften des Herstellers   |
|    **Device**             |                      |
|   DeviceModelId     |     ID des zugeordneten Gerätemodells  |
|  HardwareId          | Eindeutige ID für das Gerät (beispielsweise MAC-Adresse usw.)
|  ReportingInterval        |   Berichtsintervall in Sekunden
|  Location            |  Breitengrad (-90 bis +90)/Längengrad (-180 bis +180)/Höhe (in Meter) des Geräts   
|ParentDeviceId       |    ID des übergeordneten Geräts, mit dem dieses Gerät verbunden ist. Beispielsweise ein Knoten, der mit einem Gateway verbunden ist. Ein Knoten besitzt den parentDeviceId-Wert des Gateways.  |
|    NAME            | Ein Name zum Identifizieren der Ressource. Gerätepartner müssen einen Namen senden, der mit dem Gerätenamen auf der Partnerseite konsistent ist. Ist der Partnergerätename benutzerdefiniert, muss der gleiche benutzerdefinierte Name in FarmBeats angegeben werden.|
|     BESCHREIBUNG       |      Eine aussagekräftige Beschreibung  |
|     Properties    |  Zusätzliche Eigenschaften des Herstellers
|     **Sensormodul**        |          |
|       Type (Analog, Digital)          |      Art des Sensors (analog oder digital)       |
|          Hersteller            |       Der Sensorhersteller     |
|     ProductCode| Code oder Modellname/-nummer des Produkts. Beispiel: RS-CO2-N01 |
|       SensorMeasures > Name       | Name des Sensormessung. Es werden nur Kleinbuchstaben unterstützt. Geben Sie bei Messungen mit unterschiedlicher Tiefe die Tiefe an. Beispiel: soil_moisture_15cm. Dieser Name muss mit den Telemetriedaten übereinstimmen.  |
|          SensorMeasures > DataType       |Art der Telemetriedaten. Aktuell wird „double“ unterstützt.|
|    SensorMeasures > Type    |Art der Messung der Sensortelemetriedaten. Systemdefinierte Typen: AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration, PAR. Informationen zum Hinzufügen weiterer Typen finden Sie unter der API „/ExtendedType“.|
|        SensorMeasures > Unit              | Einheit der Sensortelemetriedaten. Systemdefinierte Einheiten: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchesPerHour. Informationen zum Hinzufügen weiterer Einheiten finden Sie unter der API „/ExtendedType“.|
|    SensorMeasures > aggregationType    |  Mögliche Werte: „none“, „average“, „maximum“, „minimum“ oder „StandardDeviation“  |
|          NAME            | Name zur Identifizierung der Ressource. Beispiel: Modellname/Produktname.  |
|    BESCHREIBUNG        | Aussagekräftige Beschreibung des Modells  |
|   Properties       |  Zusätzliche Eigenschaften des Herstellers  |
|    **Sensor**      |          |
| HardwareId          |   Eindeutige, vom Hersteller festgelegte ID für den Sensor |
|  SensorModelId     |    ID des zugeordneten Sensormodells   |
| location          |  Breitengrad (-90 bis +90)/Längengrad (-180 bis +180)/Höhe (in Meter) des Sensors|
|   Port > name        |  Name und Art des Ports, über den der Sensor mit dem Gerät verbunden ist. Hierbei muss es sich um den gleichen Namen handeln, der auch im Gerätemodell definiert ist. |
|    DeviceID  |    ID des Geräts, mit dem der Sensor verbunden ist     |
| NAME            |   Name zur Identifizierung der Ressource. Beispiel: Sensorname/Produktname und Modellnummer/Produktcode.|
|    BESCHREIBUNG      | Eine aussagekräftige Beschreibung |
|    Properties        |Zusätzliche Eigenschaften des Herstellers |

Weitere Informationen zu Objekten finden Sie unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

**API-Anforderung zum Erstellen von Metadaten**

Für eine API-Anforderung kombinieren Sie die HTTP-Methode (POST), die URL des API-Diensts und den URI einer abzufragenden Ressource, übermitteln Daten zum Erstellen oder Löschen einer Anforderung und fügen mindestens einen HTTP-Anforderungsheader hinzu. Die URL des API-Diensts ist der API-Endpunkt, also die Datenhub-URL (https://<yourdatahub>.azurewebsites.net).  

**Authentifizierung:**

Der FarmBeats-Datenhub verwendet die Bearerauthentifizierung. Für diese sind folgende Anmeldeinformationen erforderlich, die wir im obigen Abschnitt generiert haben:

- Client-ID
- Geheimer Clientschlüssel
- Mandanten-ID  

Mithilfe der obigen Anmeldeinformationen kann der Aufrufer ein Zugriffstoken anfordern. Dieses muss in den nachfolgenden API-Anforderungen wie folgt im Headerabschnitt gesendet werden:

headers = *{"Authorization": "Bearer " + access_token, …}*

**HTTP-Anforderungsheader**:

Im Anschluss finden Sie die gängigsten Anforderungsheader, die bei einem für den FarmBeats-Datenhub bestimmten API-Aufruf angegeben werden müssen:

- Content-Type: application/json
- Autorisierung: Bearer <Zugriffstoken>
- Accept: application/json

**Eingabenutzlast für die Metadatenerstellung**:

**DeviceModel**


```json
{
  "type": "Node",
  "manufacturer": "string",
  "productCode": "string",
  "ports": [
    {
      "name": "string",
      "type": "Analog"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

Device
```json
{
  "deviceModelId": "string",
  "hardwareId": "string",
  "farmId": "string",
  "reportingInterval": 0,
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "parentDeviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

SensorModel

```json
{
  "type": "Analog",
  "manufacturer": "string",
  "productCode": "string",
  "sensorMeasures": [
    {
      "name": "string",
      "dataType": "Double",
      "type": "string",
      "unit": "string",
      "aggregationType": "None",
      "depth": 0,
      "description": "string"
    }
  ],
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Sensor

```json
{
  "hardwareId": "string",
  "sensorModelId": "string",
  "location": {
    "latitude": 0,
    "longitude": 0,
    "elevation": 0
  },
  "depth": 0,
  "port": {
    "name": "string",
    "type": "Analog"
  },
  "deviceId": "string",
  "name": "string",
  "description": "string",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}

```
Die folgende Beispielanforderung dient zum Erstellen eines Geräts (unter Verwendung einer JSON-Eingabe als Nutzlast mit dem Anforderungstext).  

```
curl -X POST "https://<datahub>.azurewebsites.net/Device" -H  
"accept: application/json" -H  "Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>" -d "
{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  
\"reportingInterval\": 900,  \"name\": \"Device123\",  
\"description\": \"Test Device 123\",}"*
```

> [!NOTE]
> Die APIs geben eindeutige IDs für jede erstellte Instanz zurück. Diese IDs werden zum Senden der entsprechenden Telemetrienachrichten benötigt.

**Senden von Telemetriedaten**

Nachdem Sie die Geräte und Sensoren in FarmBeats erstellt haben, können Sie nun die zugehörigen Telemetrienachrichten senden.  

**Erstellen des Telemetrieclients**

Die Telemetriedaten müssen zur Verarbeitung an Azure Event Hub gesendet werden. Der Azure Event Hub-Dienst ermöglicht die Erfassung von Echtzeitdaten (Telemetrie) von verbundenen Geräten und Anwendungen. Wenn Sie Telemetriedaten an FarmBeats senden möchten, müssen Sie einen Client erstellen, der Nachrichten an einen Event Hub in FarmBeats sendet. Weitere Informationen zum Senden von Telemetriedaten finden Sie unter [Senden von Ereignissen an oder Empfangen von Ereignissen aus Event Hubs mithilfe von .NET Core](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

**Senden von Telemetrienachrichten als Client**

Nachdem Sie eine Verbindung als Event Hub-Client eingerichtet haben, können Sie Nachrichten im JSON-Format an den Event Hub senden.  
Konvertieren Sie das Format der historischen Sensordaten in ein kanonisches, für Azure FarmBeats geeignetes Format. Das kanonische Nachrichtenformat sieht wie folgt aus:  


 ```
  {   
      “deviceid”: “<id of the Device created>”,   
      "timestamp": "<timestamp in ISO 8601 format>",     
      "version" : "1",   
      "sensors":
      [     
      {        
          "id": "<id of the sensor created>”       
          "sensordata": [         
          {            
              "timestamp": "< timestamp in ISO 8601 format >",           
              "<sensor measure name (as defined in the Sensor Model)>": value          
    },          
    {            
    "timestamp": "<timestamp in ISO 8601 format>",           
     "<sensor measure name (as defined in the Sensor Model)>": value          
    }        
    ]      
    }  
    }
```


Nachdem Sie die entsprechenden Geräte und Sensoren hinzugefügt haben, rufen Sie die Geräte-ID und die Sensor-ID aus der Telemetrienachricht ab, wie im vorherigen Abschnitt beschrieben.

Beispiel für eine Telemetrienachricht:


 ```json
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version": "1",
  "sensors": [
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 15
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 42
        }
      ]
    },
    {
      "id": "d8e7beb4-72de-4eed-9e00-45e09043a0b3",
      "sensordata": [
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_max": 20
        },
        {
          "timestamp": "2019-06-22T06:55:02.7279559Z",
          "hum_min": 89
        }
      ]
    }
  ]
}
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu REST-API-basierten Integrationsdetails finden Sie unter [REST:API](references-for-farmbeats.md#rest-api).
