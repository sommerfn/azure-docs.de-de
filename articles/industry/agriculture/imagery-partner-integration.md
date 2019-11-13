---
title: Integration eines Partnerunternehmens für Bilder
description: Enthält eine Beschreibung der Integration eines Partnerunternehmens für Bilder.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: c76fd151bf70bfff6eed3b45a673f94777e59467
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797299"
---
# <a name="imagery-partner-integration"></a>Integration eines Partnerunternehmens für Bilder

In diesem Artikel wird beschrieben, wie Sie die Komponente „Azure FarmBeats Translator“ nutzen, um Bilddaten an FarmBeats zu senden. Bilddaten aus dem landwirtschaftlichen Bereich können aus unterschiedlichen Quellen stammen, z. B. von Multispektralkameras, Satelliten und Drohnen. Partnerunternehmen, die Bilddaten aus dem landwirtschaftlichen Bereich bereitstellen, können in FarmBeats integriert werden, um Kunden mit maßgeschneiderten Karten für ihre landwirtschaftlichen Betriebe zu versorgen.

Wenn die Daten verfügbar sind, können sie per FarmBeats Accelerator visualisiert werden und ggf. für Datenfusionen und die Erstellung von ML/KI-Modellen (Machine Learning/Künstliche Intelligenz) durch Agrarunternehmen oder Systemintegratoren von Kunden genutzt werden.

Mit FarmBeats haben Sie folgende Möglichkeiten:

- Definieren von benutzerdefinierten Bildtypen, Quelle und Dateiformat mit APIs für erweiterte Typen
- Erfassen von Bilddaten aus unterschiedlichen Quellen mit Scene- und SceneFile-APIs

In den unten angegebenen Informationen geht es darum, wie die unterschiedlichen Arten von Bildern in das FarmBeats-System übertragen werden können.

Wenn Sie den Abschnitt „Drone Imagery“ (Drohnenbilder) auswählen, wird ein Popupfenster geöffnet, um ein hochauflösendes Bild des Orthofotos der Drohne anzuzeigen. Sie können auf die Partnersoftware zugreifen, um Drohnenflüge zu planen und Rohdaten abzurufen. Sie verwenden die Software des Partners weiterhin für die Pfadplanung und das Zusammenfügen von Orthofotos.

Drohnen-Partnerunternehmen müssen es ihren Kunden ermöglichen, das Kundenkonto mit ihrer FarmBeats-Instanz in Azure zu verknüpfen.

Sie müssen in der Software des Drohnenpartners die folgenden Anmeldeinformationen verwenden, um die Verknüpfung mit FarmBeats herzustellen:

- API-Endpunkt
- Mandanten-ID
- Client-ID
- Geheimer Clientschlüssel

## <a name="api-development"></a>API-Entwicklung

Die APIs enthalten technische Swagger-Dokumentation. Unter [Swagger](https://aka.ms/FarmBeatsDatahubSwagger) finden Sie Informationen zu den APIs und zu den entsprechenden Anforderungen und Antworten.

## <a name="authentication"></a>Authentication

Für FarmBeats wird [Active Directory]((https://docs.microsoft.com/azure/app-service/overview-authentication-authorization)) von Microsoft Azure verwendet. Die Authentifizierungsfunktion von Azure App Service verfügt über eine integrierte Authentifizierungs- und Autorisierungsunterstützung. 

Weitere Informationen finden Sie unter [Azure Active Directory](https://docs.microsoft.com/azure/app-service/overview-authentication-authorization).   

Für den Datenhub von FarmBeats wird eine Bearerauthentifizierung verwendet, für die die folgenden Anmeldeinformationen benötigt werden:

- Client-ID
- Geheimer Clientschlüssel
- Mandanten-ID

Mithilfe der obigen Anmeldeinformationen kann der Aufrufer ein Zugriffstoken anfordern. Dieses muss in den nachfolgenden API-Anforderungen wie folgt im Headerabschnitt gesendet werden:

```
headers = {"Authorization": "Bearer " + access_token, …} 
```

Unten ist ein Python-Codebeispiel angegeben, mit dem das Zugriffstoken abgerufen wird. Anschließend können Sie das Token für nachfolgende API-Aufrufe von FarmBeat verwenden:   
Importieren von Azure 

```python
from azure.common.credentials import ServicePrincipalCredentials 
import adal 
#FarmBeats API Endpoint 
ENDPOINT = "https://<yourdatahub>.azurewebsites.net"   
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

## <a name="http-request-headers"></a>HTTP-Anforderungsheader

Hier sind die gängigsten Anforderungsheader aufgeführt, die bei einem für den FarmBeats-Datenhub bestimmten API-Aufruf angegeben werden müssen:

**Kopfzeile** | **Beschreibung und Beispiel**
--- | ---
Content-Type  | Das Anforderungsformat (Content-Type: application/<format>) für die FarmBeats-Datenhub-API ist JSON. Content-Type: application/json
Authorization | Gibt das Zugriffstoken an, das zum Durchführen eines API-Aufrufs erforderlich ist. Autorisierung: Bearer <Access-Token>
Akzeptieren  | Das Antwortformat. Für FarmBeats-Datenhub-APIs ist das Format JSON. Accept: application/json


## <a name="api-requests"></a>API-Anforderungen

Zum Senden einer REST-API-Anforderung kombinieren Sie die HTTP-Methode (GET/POST/PUT), die URL für den API-Dienst, den Ressourcen-URI (zum Abfragen, Übermitteln von Daten, Aktualisieren oder Löschen) und mindestens einen HTTP-Anforderungsheader miteinander.

Optional können Sie Abfrageparameter in GET-Aufrufe einfügen, um Daten zu filtern, deren Größe zu beschränken und diese in den Antworten zu sortieren.

Mit der unten angegebenen Beispielanforderung wird die Liste mit den Geräten abgerufen:

```
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H
"Content-Type: application/json" -H
"Authorization: Bearer <Access-Token>”
```

Für die meisten GET-, POST- und PUT-Aufrufe ist ein JSON-Anforderungstext erforderlich.

Die folgende Beispielanforderung dient zum Erstellen eines Geräts (unter Verwendung von JSON-Eingabecode mit dem Anforderungstext).


```json
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  
"accept: application/json" -H  
"Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d
"{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  
\"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

## <a name="data-format"></a>Datenformat

JSON (JavaScript Object Notation) ist ein gängiges sprachunabhängiges Datenformat, das eine einfache Textdarstellung beliebiger Datenstrukturen ermöglicht. Weitere Informationen finden Sie unter [JSON.org](https://JSON.org).

## <a name="ingesting-imagery-into-farmbeats"></a>Erfassen von Bildern in FarmBeats

Wenn das Partnerunternehmen die Anmeldeinformationen für die Verbindungsherstellung mit dem FarmBeats-Datenhub erhalten hat, führt es in der Translator-Komponente die folgenden Schritte aus:

1.  Erstellen eines neuen erweiterten Typs für die folgenden Felder (gemäß dem Typ der hochzuladenden Bilder):

    - Scene Source (Szenenquelle): Beispiel: <drone_partner_name>
    - Scene Type (Szenentyp): Zum Beispiel, <drone>
    - Scene File Type (Szenendateityp): Zum Beispiel, <chlorophyll index>
    - Scene File Content Type (Szenendatei-Inhaltstyp): Beispiel: <image/tiff>

2.  Aufrufen der Farms-API zum Abrufen der Liste mit den landwirtschaftlichen Betrieben aus dem Azure FarmBeats-System.
3.  Bereitstellen einer Option für den Kunden, mit der in der Liste mit den landwirtschaftlichen Betrieben ein einzelner Betrieb ausgewählt werden kann.

    Im Partnersystem muss der landwirtschaftliche Betrieb in der Partnersoftware angezeigt werden, damit die Pfadplanung und der Drohnenflug sowie die Bilderfassung durchgeführt werden können.

4.  Aufrufen der Scene-API und Angeben der erforderlichen Details für die Erstellung einer neuen Szene mit eindeutiger SceneID.
5.  Empfangen einer Blob-SAS-URL für den Upload der erforderlichen Bilder in den FarmBeats-Datenhub (im Kontext des ausgewählten landwirtschaftlichen Betriebs) des FarmBeats-Systems.

Hier ist ein detaillierter Ablauf der API-Aufrufe angegeben:

### <a name="step-1-extendedtype"></a>Schritt 1: ExtendedType

Checken Sie die ExtendedType-API ein, wenn der Typ und die Dateiquelle in FarmBeats verfügbar sind. Hierfür können Sie einen GET-Aufruf für die /ExtendedType-API durchführen.

Hier sind die vom System definierten Werte aufgeführt:

```json
{
  "items": [
    {
      "id": "ae2aa527-7d27-42e2-8fcf-841937c651d7",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:47:42.5910344Z",
      "key": "SceneFileType",
      "value": [
        "evi",
        "ndvi",
        "ndwi",
        "tci",
        "soil-moisture",
        "sensor-placement",
        "sentinel-b01",
        "sentinel-b02",
        "sentinel-b03",
        "sentinel-b04",
        "sentinel-b05",
        "sentinel-b06",
        "sentinel-b07",
        "sentinel-b08",
        "sentinel-b8a",
        "sentinel-b09",
        "sentinel-b10",
        "sentinel-b11",
        "sentinel-b12",
        "cloud-mask",
        "farm-mask",
        "image/tiff",
        "orthomosaic"
      ],
      "description": "List of scene file types available in system. User can add more values."
    },
    {
      "id": "b7824d2e-3e43-46d3-8fd4-ec7ba8918d84",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:44.8914691Z",
      "key": "SceneFileContentType",
      "value": [
        "image/tiff",
        "image/png",
        "image/jpeg",
        "text/csv",
        "text/plain",
        "text/tab-separated-values",
        "application/json",
        "application/octet-stream",
        "orthomosaic"
      ],
      "description": "List of scene file content types available in system. User can add more values."
    },
    {
      "id": "21d2924d-8b3e-4ce4-a7f8-318e85c0f7f2",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:37.5096445Z",
      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P",
        "DJI"
      ],
      "description": "List of scene sources available in system. User can add more values."
    },
    {
      "id": "6e09ca69-cacf-4b53-b63a-53c659aae4a4",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-10-25T09:45:34.7483899Z",
      "key": "SceneType",
      "value": [
        "base-bands",
        "sensor-placement",
        "soil-moisture",
        "evi",
        "ndwi",
        "ndvi",
        "drone",
        "orthomosaic-drone"
      ],
      "description": "List of scene types available in system. User can add more values."
    },
    {
      "id": "55d8436b-3a86-4bea-87ab-e1b51226525f",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureType",
      "value": [
        "AmbientTemperature",
        "CO2",
        "Depth",
        "ElectricalConductivity",
        "LeafWetness",
        "Length",
        "LiquidLevel",
        "Nitrate",
        "O2",
        "PH",
        "Phosphate",
        "PointInTime",
        "Potassium",
        "Pressure",
        "RainGauge",
        "RelativeHumidity",
        "Salinity",
        "SoilMoisture",
        "SoilTemperature",
        "SolarRadiation",
        "State",
        "TimeDuration",
        "UVRadiation",
        "UVIndex",
        "Volume",
        "WindDirection",
        "WindRun",
        "WindSpeed",
        "Evapotranspiration",
        "PAR"
      ],
      "description": "List of sensor measure types available in system. User can add more values."
    },
    {
      "id": "0dfd6e6b-df58-428f-9ab8-a0674bfdcbe5",
      "createdAt": "2019-06-17T07:32:30.1135716Z",
      "lastModifiedAt": "2019-06-17T07:32:30.1135716Z",
      "key": "SensorMeasureUnit",
      "value": [
        "NoUnit",
        "Celsius",
        "Fahrenheit",
        "Kelvin",
        "Rankine",
        "Pascal",
        "Mercury",
        "PSI",
        "MilliMeter",
        "CentiMeter",
        "Meter",
        "Inch",
        "Feet",
        "Mile",
        "KiloMeter",
        "MilesPerHour",
        "MilesPerSecond",
        "KMPerHour",
        "KMPerSecond",
        "MetersPerHour",
        "MetersPerSecond",
        "Degree",
        "WattsPerSquareMeter",
        "KiloWattsPerSquareMeter",
        "MilliWattsPerSquareCentiMeter",
        "MilliJoulesPerSquareCentiMeter",
        "VolumetricWaterContent",
        "Percentage",
        "PartsPerMillion",
        "MicroMol",
        "MicroMolesPerLiter",
        "SiemensPerSquareMeterPerMole",
        "MilliSiemensPerCentiMeter",
        "Centibar",
        "DeciSiemensPerMeter",
        "KiloPascal",
        "VolumetricIonContent",
        "Liter",
        "MilliLiter",
        "Seconds",
        "UnixTimestamp",
        "MicroMolPerMeterSquaredPerSecond",
        "InchesPerHour"
      ],
      "description": "List of sensor measure units available in system. User can add more values."
    }
  ]
}
```
Dies ist ein einmalig durchzuführendes Setup. Der Bereich dieses neuen Szenentyps ist auf das Abonnement beschränkt, unter dem das FarmBeats-Projekt bereitgestellt wird.

Beispiel: Zum Hinzufügen von „SceneSource: SlantRange“ verwenden Sie einen PUT-Vorgang für die ID von „/ExtendedType“ mit dem folgenden Schlüssel: „SceneSource“-Eingabenutzlast:

```json
{

      "key": "SceneSource",
      "value": [
        "sentinel-l1c",
        "sentinel-l2a",
        "farmbeats-model",
        "dji",
        "SlantRange-3P"
      ],
      "description": "List of scene sources available in system. User can add more values."
}

```

Das grün gekennzeichnete Feld wurden den vom System definierten Szenenquellenwerten neu hinzugefügt.

### <a name="step-2-get-farmdetails"></a>Schritt 2: Abrufen von FarmDetails

Die Szenen (TIFF- oder CSV-Dateien) weisen einen Kontext in Bezug auf landwirtschaftliche Betriebe auf. Sie müssen die Details zum landwirtschaftlichen Betrieb abrufen, indem Sie für die /Farm-API einen GET-Vorgang durchführen. Die API gibt die Liste mit den landwirtschaftlichen Betrieben zurück, die in FarmBeats verfügbar sind, und Sie können den Betrieb auswählen, für den Sie die Daten erfassen möchten.

Abrufen der /Farm-Antwort:

```json
{
  "id": "07f3e09c-89aa-4619-8d50-e57fb083d8f9",
  "createdAt": "2019-11-01T13:55:41.8804663Z",
  "lastModifiedAt": "2019-11-01T13:55:41.8804663Z",
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          78.34252251428694,
          17.402556352862675
        ],
        [
          78.34278000635095,
          17.407920852463022
        ],
        [
          78.34883106989963,
          17.40878079576528
        ],
        [
          78.3498181228054,
          17.403539173730607
        ],
        [
          78.34805859369493,
          17.39977166504127
        ],
        [
          78.34252251428694,
          17.402556352862675
        ]
      ]
    ]
  },
  "name": "Samplefarm",
  "properties": {
    "crops": "Corn",
    "address": "Sample street"
  }
}
 ```

### <a name="step-3-create-ascene-id-post-call"></a>Schritt 3: Erstellen einer Szenen-ID (POST-Aufruf)

Erstellen Sie eine neue Szene (TIFF- oder CSV-Datei) mit den entsprechenden Informationen, und geben Sie das Datum, die Sequenz und die ID des landwirtschaftlichen Betriebs für die Zuordnung zur Szene an. Die der Szene zugeordneten Metadaten können unter „Eigenschaften“ definiert werden, z. B. Dauer und Typ der Messung.

Hierdurch wird eine neue Szenen-ID erstellt, die dem landwirtschaftlichen Betrieb zugeordnet wird. Nach der Erstellung der Szenen-ID kann der Benutzer hiermit eine neue Datei (TIFF oder CSV) erstellen und den Inhalt der Datei speichern.

Beispiel: Eingabenutzlast für den POST-Aufruf zur /Scene-API

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```

API-Antwort:

```json
{
  "id": "a0505928-c480-491b-ba31-d38285a28c1d",
  "createdAt": "2019-10-04T16:19:12.4838584Z",
  "lastModifiedAt": "2019-10-04T16:19:12.4838584Z",
  "type": "new type",
  "source": "SlantRange-3P",
  "farmId": "d41a33e7-b73e-480e-9279-0fcb3207332b",
  "date": "2019-10-04T16:13:39.064Z",
  "sequence": 5,
  "name": "test scene",
  "description": "test scene description",
  "properties": {}
}

```

**Create/SceneFile**

Die in Schritt 3 zurückgegebene Szenen-ID ist die Eingabe für die Szenendatei (SceneFile). Die Szenendatei gibt ein SAS-URL-Token zurück, das 24 Stunden lang gültig ist.

Falls der Benutzer eine programmgesteuerte Option zum Hochladen eines Bilddatenstroms benötigt, kann er das Blobspeicher-SDK verwenden, um mit der Szenendatei-ID, dem Speicherort und der URL eine Methode zu definieren.

Beispiel: Eingabenutzlast für den POST-Aufruf zur /SceneFile-API:

```json
{
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {
    "additionalProp1": {},
    "additionalProp2": {},
    "additionalProp3": {}
  }
}
```
API-Antwort:

```json
{
  "uploadSASUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840?sv=2018-03-28&sr=b&sig=%2F1426JkDcIFE5g3d%2BjOevCVMIn%2FJo9YKwBn3La5zL8Y%3D&se=2019-10-05T16%3A23%3A57Z&sp=w",
  "id": "e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "createdAt": "2019-10-04T16:23:57.1192916Z",
  "lastModifiedAt": "2019-10-04T16:23:57.1192916Z",
  "blobUrl": "https://storagej2lho.blob.core.windows.net/farm-scene/2019/a0505928-c480-491b-ba31-d38285a28c1d/e91139a7-4ebd-4e2f-b17c-c677822dc840",
  "sceneId": "a0505928-c480-491b-ba31-d38285a28c1d",
  "type": "newtype",
  "contentType": "image/tiff",
  "name": "test scene file",
  "description": "test scene file description",
  "properties": {}
}


```

Der POST-Aufruf der /SceneFile-API gibt eine SAS-Upload-URL zurück, die genutzt werden kann, um die CSV- oder TIFF-Datei über die Azure Blob Storage-Clientbibliothek hochzuladen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur REST-API-basierten Integration finden Sie unter [REST-API](references-for-farmbeats.md#rest-api).
