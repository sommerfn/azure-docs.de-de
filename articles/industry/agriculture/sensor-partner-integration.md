---
title: Integration von Partnern für Sensordaten
description: Beschreibt die Integration von Partnern für Sensordaten.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: e7de815b7254fb071b3094f9ae636b712b38684b
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797231"
---
# <a name="sensor-partner-integration"></a>Integration von Partnern für Sensordaten
Dieser Artikel enthält Informationen zur **Translator**-Komponente von Azure FarmBeats.

Mithilfe dieser Komponente können Partner Sensoren entwickeln, die sich in FarmBeats integrieren lassen. Dabei können sie unsere API nutzen und Daten von kundeneigenen Geräten sowie Telemetriedaten an den FarmBeats-Datenhub senden. Die Daten werden mithilfe des FarmBeats-Accelerators visualisiert. Die Daten können für die Datenfusion und für die Entwicklung von Modellen genutzt werden, die auf Machine Learning oder künstlicher Intelligenz basieren.

## <a name="link-farmbeats-account"></a>Verknüpfen des FarmBeats-Kontos

Nachdem Kunden Geräte/Sensoren erworben und bereitgestellt haben, können sie im Saas-Portal (Software-as-a-Service) des Geräte-Partnerunternehmens auf die Geräte- und Telemetriedaten zugreifen. Geräte-Partnerunternehmen müssen ihren Kunden die Möglichkeit bieten, ihr Konto mit ihrer FarmBeats-Instanz in Azure zu verknüpfen. Die folgenden Anmeldeinformationen müssen vom Kunden/SI angegeben werden:

   - Anzeigename (ein optionales Feld, in dem der Benutzer einen Namen für diese Integration angeben kann)
   - API-Endpunkt
   - Mandanten-ID
   - Client-ID
   - Geheimer Clientschlüssel
   - EventHub-Verbindungszeichenfolge
   - Startdatum

   > [!NOTE]
   > Durch „Startdatum“ werden historische Datenfeeds unterstützt, d. h. Daten, die ab dem vom Benutzer angegebenen Datum erfasst wurden.

## <a name="unlink-farmbeats"></a>Aufheben der FarmBeats-Verknüpfung

Kunden können die Verknüpfung einer vorhandenen FarmBeats-Integration aufheben. Beim Aufheben der FarmBeats-Verknüpfung sollten keine Geräte-/Sensormetadaten gelöscht werden, die im Datenhub des Kunden erstellt wurden. Beim Aufheben der Verknüpfung geschieht Folgendes:

   - Der Telemetriedatenfluss wird beendet.
   - Die Anmeldeinformationen für die Integration des Geräte-Partnerunternehmens werden gelöscht.

## <a name="edit-farmbeats-integration"></a>Bearbeiten der FarmBeats-Integration

Der Kunde kann die FarmBeats-Integration bearbeiten. Der Hauptgrund für die Bearbeitung ist, dass sich der geheime Clientschlüssel oder die Verbindungszeichenfolge ändert, weil sie abgelaufen sind. In diesem Fall kann der Kunde nur die folgenden Felder bearbeiten.

   - Anzeigename (falls zutreffend)
   - Geheimer Clientschlüssel (sollte im Format „2x8***********“ oder mit der Funktion zum Ein-/Ausblenden und nicht als Klartext angezeigt werden)
   - Verbindungszeichenfolge (sollte im Format „2x8***********“ oder mit der Funktion zum Ein-/Ausblenden und nicht als Klartext angezeigt werden)

   > [!NOTE]
   > Die Bearbeitung sollte nicht dazu führen, dass die Erstellung von Metadatenobjekten unterbrochen wird.

## <a name="view-last-telemetry-sent"></a>Anzeigen der zuletzt gesendeten Telemetriedaten

Sie können den Zeitstempel der zuletzt gesendeten **** Telemetriedaten anzeigen. Dies entspricht dem Zeitpunkt, zu dem die neuesten Telemetriedaten erfolgreich an FarmBeats gesendet wurden.

## <a name="translator-development"></a>Translator-Entwicklung

**REST-API-basierte Integration**

Die FarmBeats-Funktionen zur Integration von Sensordaten werden über die REST-API verfügbar gemacht. Die Funktionen umfassen die Definition von Metadaten sowie die Bereitstellung und Verwaltung von Geräten und Sensoren.

**Telemetriedatenerfassung**

Die Telemetriedaten werden einer kanonischen Nachricht zugeordnet, die zur Verarbeitung auf Azure Event Hub veröffentlicht wird. Der Azure Event Hub-Dienst ermöglicht die Erfassung von Echtzeitdaten (Telemetrie) von verbundenen Geräten und Anwendungen.

**API-Entwicklung**

Die APIs enthalten eine technische Swagger-Dokumentation. Weitere Informationen zu den APIs und den entsprechenden Anforderungen/Antworten finden Sie unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger)

**Authentifizierung**

FarmBeats nutzt die Active Directory-Authentifizierung von Microsoft Azure. Der Azure App Service bietet eine integrierte Authentifizierungs- und Autorisierungsunterstützung.

Weitere Informationen finden Sie unter [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).

Für den Datenhub von FarmBeats wird eine Bearerauthentifizierung verwendet, für die die folgenden Anmeldeinformationen benötigt werden:
   - Client-ID
   - Geheimer Clientschlüssel
   - Mandanten-ID

Mithilfe der obigen Anmeldeinformationen kann der Aufrufer ein Zugriffstoken anfordern. Dieses muss in den nachfolgenden API-Anforderungen wie folgt im Headerabschnitt gesendet werden:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Im Folgenden ein Beispiel für Python-Code, durch den das Zugriffstoken bereitgestellt wird, das für nachfolgende API-Aufrufe an FarmBeats verwendet werden kann: 

```python
import azure 

from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net" [Azure website](https://<yourdatahub>.azurewebsites.net)
CLIENT_ID = "<Your Client ID>"   
CLIENT_SECRET = "<Your Client Secret>"   
TENANT_ID = "<Your Tenant ID>" 
AUTHORITY_HOST = 'https://login.microsoftonline.com' 
AUTHORITY = AUTHORITY_HOST + '/' + TENANT_ID 
#Authenticating with the credentials 
context = adal.AuthenticationContext(AUTHORITY) 
token_response = context.acquire_token_with_client_credentials(ENDPOINT, CLIENT_ID, CLIENT_SECRET) 
#Should get an access token here 
access_token = token_response.get('accessToken') 
```


**HTTP-Anforderungsheader**

Hier sind die gängigsten Anforderungsheader aufgeführt, die bei einem für den FarmBeats-Datenhub bestimmten API-Aufruf angegeben werden müssen:


**Kopfzeile** | **Beschreibung und Beispiel**
--- | ---
Content-Type | Das Anforderungsformat (Content-Type: application/<format>) für die FarmBeats-Datenhub-API ist JSON. Content-Type: application/json
Authorization | Gibt das Zugriffstoken an, das zur Autorisierung eines API-Aufrufs erforderlich ist: Bearer <Access-Token>
Accept | Das Antwortformat. Für FarmBeats-Datenhub-APIs ist das Format: json Accept: application/json.

**API-Anforderungen**

Um eine REST (Representational State Transfer)-API-Anforderung auszuführen, kombinieren Sie die HTTP (GET, POST oder PUT)-Methode, die URL für den API-Dienst, den URI (Uniform Resource Identifier) für eine Ressource, die abgefragt, aktualisiert oder gelöscht bzw. an die Daten übermittelt werden sollen, und mindestens einen HTTP-Anforderungsheader. Die URL für den API-Dienst entspricht dem vom Kunden bereitgestellten API-Endpunkt. Hier ein Beispiel: https://\<<Datenhub-Websitename>.azurewebsites.net

Optional können Sie Abfrageparameter in GET-Aufrufe einfügen, um Daten in den Antworten zu filtern, zu sortieren und deren Größe zu beschränken.

Mit der unten angegebenen Beispielanforderung wird die Liste mit den Geräten abgerufen:

```
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>”
```
Für die meisten GET-, POST- und PUT-Aufrufe ist ein JSON-Anforderungstext erforderlich.

Die folgende Beispielanforderung dient zum Erstellen eines Geräts (unter Verwendung von JSON-Eingabecode mit dem Anforderungstext).

```
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Datenformat

JSON (JavaScript Object Notation) ist ein gängiges sprachunabhängiges Datenformat, das eine einfache Textdarstellung beliebiger Datenstrukturen ermöglicht. Weitere Informationen finden Sie unter [json.org](http://json.org).

## <a name="metadata-specifications"></a>Metadatenspezifikationen

Der FarmBeats-Datenhub bietet folgende APIs, die Geräte-Partnerunternehmen die Erstellung und Verwaltung von Geräte-/Sensormetadaten ermöglichen:  

- /**DeviceModel**: Das Gerätemodell entspricht den Metadaten des Geräts, beispielsweise dem Hersteller und Gerätetyp (Gateway oder Knoten).  
- /**Device**: Ein physisches Gerät des landwirtschaftlichen Betriebs.
- /**SensorModel**: Das Sensormodell entspricht den Metadaten des Sensors. Hierzu zählen beispielsweise der Hersteller, der Sensortyp (analog oder digital) und die Sensormessung (wie Umgebungstemperatur, Luftdruck usw.).
- /**Sensor**: Ein physischer Sensor, der Werte aufzeichnet. Ein Sensor ist in der Regel mit einem Gerät mit einer Geräte-ID verbunden.

  Gerätemodell| DeviceModel entspricht den Metadaten des Geräts, beispielsweise dem Hersteller und Gerätetyp (Gateway oder Knoten).
  --- | ---
  Type (Knoten, Gateway)  | Ein Stern |
  Hersteller  | Zwei Sterne |
  ProductCode  | Produktcode oder Modellname/-nummer des Geräts. Beispiel: EnviroMonitor#6800 |
  Ports  | Portname und -typ (digital/analog)  |
  Name  | Der Name zur Identifizierung der Ressource. Beispiel: Modellname/Produktname |
  Description  | Aussagekräftige Beschreibung des Modells |
  Properties  | Zusätzliche Eigenschaften des Herstellers |
  **Device** | **Ein physisches Gerät des landwirtschaftlichen Betriebs. Jedes Gerät verfügt über eine eindeutige Geräte-ID.** |
DeviceModelId  |ID des zugeordneten Gerätemodells |
HardwareId   |Eindeutige ID für das Gerät (beispielsweise MAC-Adresse)  |
reportingInterval |Berichtsintervall in Sekunden |
Location    |Breitengrad (-90 bis +90)/Längengrad (-180 bis +180)/Höhe (in Metern) des Geräts |
ParentDeviceId | Die ID des übergeordneten Geräts, mit dem dieses Gerät verbunden ist. Beispiel: Ein mit einem Gateway verbundener Knoten besitzt den parentDeviceID-Wert als Gateway. |
  Name  | Der Name zur Identifizierung der Ressource.  Gerätepartner müssen einen Namen senden, der mit dem Gerätenamen auf der Partnerseite übereinstimmt. Wenn der Gerätename auf der Seite des Gerätepartners benutzerdefiniert ist, muss derselbe benutzerdefinierte Name in FarmBeats angegeben werden.  |
  Description  | Eine aussagekräftige Beschreibung  |
  Properties  |Zusätzliche Eigenschaften des Herstellers  |
  **Sensormodell** | SensorModel entspricht den Metadaten des Sensors. Hierzu zählen beispielsweise der Hersteller, der Sensortyp (analog oder digital) und die Sensormessung (wie Umgebungstemperatur, Luftdruck usw.). |
  Type (analog, digital)  |Gibt an, ob der Sensor analog oder digital ist.|
  manufacturer  | Name des Herstellers |
  ProductCode  | Produktcode oder Modellname/-nummer. Beispiel: RS-CO2-N01  |
  SensorMeasures > Name  | Der Name der Sensormessung. Es werden nur Kleinbuchstaben unterstützt. Geben Sie bei Messungen mit unterschiedlicher Tiefe die Tiefe an. Beispiel: soil_moisture_15cm. Dieser Name muss mit den Telemetriedaten übereinstimmen. |
  sensorMeasures > DataType  | Die Art der Telemetriedaten. Aktuell wird „double“ unterstützt.  |
  sensorMeasures > Type  | Die Art der Messung der Sensortelemetriedaten. Systemdefinierte Typen: AmbientTemperature, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration, PAR. Informationen zum Hinzufügen weiterer Typen finden Sie unter der API „/ExtendedType“.
  sensorMeasures > Unit | Die Einheit der Sensortelemetriedaten. Systemdefinierte Einheiten: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond, InchesPerHour. Informationen zum Hinzufügen weiterer Einheiten finden Sie unter der API „/ExtendedType“.
  SensorMeasures > aggregationType  | „none“, „average“, „maximum“, „minimum“ oder „StandardDeviation“
  SensorMeasures > depth  | Die Tiefe des Sensors in Zentimetern (beispielsweise zur Messung der Bodenfeuchte in einer Tiefe von 10 cm)
  sensorMeasures > description  | Aussagekräftige Beschreibung der Maßeinheit
  name  | Der Name zur Identifizierung der Ressource. Beispiel: Modellname/Produktname
  description  | Aussagekräftige Beschreibung des Modells
  properties  | Zusätzliche Eigenschaften des Herstellers
  **Sensor**  |
  hardwareId  | Eindeutige, vom Hersteller festgelegte ID für den Sensor
  sensorModelId  | Die ID des zugeordneten Sensormodells.
  location  | Breitengrad (-90 bis +90)/Längengrad (-180 bis +180)/Höhe (in Metern) des Sensors
  port > name  |Name und Typ des Ports, über den der Sensor mit dem Gerät verbunden ist. Hierbei muss es sich um den gleichen Namen handeln, der auch im Gerätemodell definiert ist.
  deviceId  | Die ID des Geräts, mit dem der Sensor verbunden ist.
  name  | Der Name zur Identifizierung der Ressource. Beispiel: Sensorname/Produktname und Modellnummer/Produktcode
  description  | Eine aussagekräftige Beschreibung
  properties  | Zusätzliche Eigenschaften des Herstellers

 Informationen zu den einzelnen Objekten und ihren Eigenschaften finden Sie unter [Swagger](httpa://aka.ms/FarmBeatsDatahubSwagger).

 > [!NOTE]
 > Die APIs geben eindeutige IDs für jede erstellte Instanz zurück. Diese ID muss vom Translator für die Geräteverwaltung und die Synchronisierung von Metadaten beibehalten werden.


**Synchronisierung von Metadaten**

Der Translator sollte Updates für die Metadaten senden. Beispielsweise ist eine Aktualisierung erforderlich, wenn der Name oder Standort des Geräts/Sensors geändert wurde.

Der Translator sollte in der Lage sein, neue Geräte und/oder Sensoren hinzuzufügen, die vom Benutzer nach der Verknüpfung mit FarmBeats installiert wurden. Dasselbe gilt, wenn ein Gerät oder Sensor vom Benutzer aktualisiert wurde. In diesem Fall sollte auch das entsprechende Gerät bzw. der entsprechenden Sensor in FarmBeats aktualisiert werden. Ein typisches Szenario für eine derartige Geräte- oder Sensoraktualisierung liegt vor, wenn der Gerätestandort geändert wird oder einem Knoten Sensoren hinzugefügt werden.


> [!NOTE]
> Geräte-/Sensormetadaten können nicht gelöscht werden.
>
> Zum Aktualisieren der Metadaten ist es zwingend erforderlich, „/Get/{id}“ für das Gerät/den Sensor aufzurufen, die geänderten Eigenschaften zu aktualisieren und dann „/Put/{id}“ auszuführen, damit keine vom Benutzer festgelegten Eigenschaften verloren gehen.

### <a name="adding-new-typesunit"></a>Hinzufügen neuer Typen/Einheiten

FarmBeats unterstützt das Hinzufügen neuer Messungstypen und Maßeinheiten für Sensoren. Weitere Informationen zur API „/ExtendedType“ finden Sie unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger).

## <a name="telemetry-specifications"></a>Telemetriespezifikationen

Die Telemetriedaten werden einer kanonischen Nachricht zugeordnet, die zur Verarbeitung auf Azure Event Hub veröffentlicht wird. Der Azure Event Hub-Dienst ermöglicht die Erfassung von Echtzeitdaten (Telemetrie) von verbundenen Geräten und Anwendungen.

## <a name="send-telemetry-data-to-farmbeats"></a>Senden von Telemetriedaten an FarmBeats

Wenn Sie Telemetriedaten an FarmBeats senden möchten, müssen Sie einen Client erstellen, der Nachrichten an einen Event Hub in FarmBeats sendet. Weitere Informationen zu Telemetriedaten finden Sie unter [Senden von Telemetriedaten an Event Hub](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

Im Folgenden ein Beispiel für Python-Code, mit dem Telemetriedaten als Client an einen angegebenen Event Hub gesendet werden:

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

Die kanonische Nachricht hat das folgende Format:

```
{
“deviceid”: “<id of the Device created>”,
 "timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
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

Alle Schlüsselnamen in den Telemetriedaten im JSON-Format sollten klein geschrieben werden, z. B. „deviceid“, „sensordata“ usw.

Beispiel für eine Telemetrienachricht:


```
{
  "deviceid": "7f9b4b92-ba45-4a1d-a6ae-c6eda3a5bd12",
  "timestamp": "2019-06-22T06:55:02.7279559Z",
  "version" : "1",
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

## <a name="troubleshooterror-management"></a>Problembehandlung/Fehlerverwaltung

**Problembehandlungsoption/Support**

Falls der Kunde keine Gerätedaten und/oder Telemetriedaten in der angegebenen FarmBeats-Instanz empfangen kann, muss der Gerätepartner Support leisten und einen Mechanismus zur Problembehandlung bereitstellen.

**Aufbewahrung von Telemetriedaten**

Die Telemetriedaten sollten zudem über einen vordefinierten Zeitraum aufbewahrt werden, damit sie zum Debuggen verwendet oder erneut gesendet werden können, falls ein Fehler oder Datenverlust auftritt.

**Fehlerverwaltung/Fehlerbenachrichtigung**

Bei einem Fehler, der sich auf die Geräte-/Sensormetadaten/Datenintegration oder den Telemetriedatenfluss im System des Gerätepartners auswirkt, sollte der Kunde davon unterrichtet werden. Darüber hinaus sollte ein Mechanismus zur Fehlerbehebung konzipiert und implementiert werden.

**Verbindungscheckliste**

Gerätehersteller/-partner nutzen möglicherweise folgenden Integritätstest bzw. folgende Checkliste, um sicherzustellen, dass die vom Kunden bereitgestellten Anmeldeinformationen korrekt sind.

   - Überprüfen Sie, ob mit den angegebenen Anmeldeinformationen ein Zugriffstoken empfangen wird.
   - Überprüfen Sie, ob ein API-Aufruf mit dem empfangenen Zugriffstoken erfolgreich ist.
   - Überprüfen Sie, ob die EventHub-Clientverbindung hergestellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur REST-API finden Sie unter [REST-API](references-for-farmbeats.md#rest-api).
