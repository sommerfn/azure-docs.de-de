---
title: Anleitung für Entwickler – IoT Plug & Play (Vorschau) | Microsoft-Dokumentation
description: Beschreibung der Gerätemodellierung für IoT Plug & Play-Entwickler
author: dominicbetts
ms.author: dobett
ms.date: 07/05/2019
ms.topic: conceptual
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 6d8e0e9e675b88c69b74cdad261280f5dcaf7161
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581621"
---
# <a name="iot-plug-and-play-preview-modeling-developer-guide"></a>Anleitung für Entwickler: Modellierung mit IoT Plug & Play (Vorschau)

Mit IoT Plug & Play (Vorschau) können Sie Geräte erstellen, deren Funktionen für Azure IoT-Anwendungen verfügbar gemacht werden. Bei IoT Plug & Play-Geräten ist keine manuelle Konfiguration erforderlich, wenn ein Kunde sie mit IoT Plug & Play-fähigen Anwendungen verbindet. IoT Central ist ein Beispiel für eine IoT Plug & Play-fähige Anwendung.

Zum Erstellen eines IoT Plug & Play-Geräts müssen Sie eine Gerätebeschreibung erstellen. Die Beschreibung erfolgt mit der einfachen Definitionssprache Digital Twins Definition Language (DTDL).

## <a name="device-capability-model"></a>Gerätefunktionsmodell

Mit DTDL erstellen Sie ein _Gerätefunktionsmodell_, um die Komponenten des Geräts zu beschreiben. Ein typisches IoT-Gerät besteht aus folgenden Komponenten:

- Benutzerdefinierte Komponenten, die Ihr Gerät einzigartig machen
- Standardkomponenten, die bei allen Geräten identisch sind

Diese Komponenten werden in einem Gerätefunktionsmodell als _Schnittstellen_ bezeichnet. Schnittstellen definieren die Details der einzelnen mit Ihrem Gerät implementierten Komponenten.

Das folgende Beispiel zeigt das Gerätefunktionsmodell für einen Thermostat:

```json
{
  "@id": "urn:example:Thermostat_T_1000:1",
  "@type": "CapabilityModel",
  "implements": [
    {
      "name": "thermostat",
      "schema": "urn:example:Thermostat:1"
    },
    {
      "name": "urn_azureiot_deviceManagement_DeviceInformation",
      "schema": "urn:azureiot:deviceManagement:DeviceInformation:1"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/CapabilityModel.json"
}
```

Ein Funktionsmodell umfasst einige Pflichtfelder:

- `@id`: eine eindeutige ID im Format eines einfachen Uniform Resource Name
- `@type`: deklariert dieses Objekt als Funktionsmodell
- `@context`: gibt die für das Funktionsmodell verwendete DTDL-Version an
- `implements`: listet die Schnittstellen auf, die das Gerät implementiert

Jeder Eintrag in der Liste der Schnittstellen im Abschnitt „implements“ enthält jeweils Folgendes:

- `name`: der Programmiername der Schnittstelle
- `schema`: die Schnittstelle, die mit dem Funktionsmodell implementiert wird

Es gibt weitere optionale Felder, über die Sie dem Funktionsmodell weitere Details hinzufügen können, z. B. den Anzeigenamen und eine Beschreibung. Schnittstellen, die in einem Funktionsmodell deklariert sind, können als Komponenten des Geräts betrachtet werden. In der Public Preview enthält die Schnittstellenliste möglicherweise nur einen Eintrag pro Schema.

## <a name="interface"></a>Schnittstelle

Mit DTDL beschreiben Sie die Funktionen des Geräts über Schnittstellen. Schnittstellen beschreiben die _Eigenschaften_, _Telemetriedaten_ und _Befehle_, die eine Komponente des Geräts implementiert:

- `Properties`. Eigenschaften sind Datenfelder, die den Zustand des Geräts darstellen. Verwenden Sie Eigenschaften, um den dauerhaften Zustand des Geräts darzustellen, z. B. den Ein/Aus-Status einer Kühlmittelpumpe. Eigenschaften können außerdem grundlegende Geräteeigenschaften darstellen, z. B. die Firmwareversion des Geräts. Sie können Eigenschaften als schreibgeschützt oder beschreibbar deklarieren.
- `Telemetry`. Telemetriefelder stellen Messungen von Sensoren dar. Immer wenn das Gerät eine Sensormessung vornimmt, sollte ein Telemetrieereignis mit den Sensordaten gesendet werden.
- `Commands`. Befehle stellen Methoden dar, die Benutzer des Geräts auf dem Gerät ausführen können, beispielsweise ein Befehl zum Zurücksetzen oder ein Befehl zum Ein- oder Ausschalten eines Lüfters.

Das folgende Beispiel zeigt die Schnittstelle für einen Thermostat:

```json
{
  "@id": "urn:example:Thermostat:1",
  "@type": "Interface",
  "contents": [
    {
      "@type": "Telemetry",
      "name": "temperature",
      "schema": "double"
    }
  ],
  "@context": "http://azureiot.com/v1/contexts/IoTModel.json"
}
```

Eine Schnittstelle umfasst einige Pflichtfelder:

- `@id`: eine eindeutige ID im Format eines einfachen Uniform Resource Name
- `@type`: deklariert dieses Objekt als Schnittstelle
- `@context`: gibt die für die Schnittstelle verwendete DTDL-Version an
- `contents`: listet die Eigenschaften, Telemetriedaten und Befehle auf, die das Gerät ausmachen

In diesem einfachen Beispiel gibt es nur ein einziges Telemetriefeld. Eine minimale Feldbeschreibung umfasst Folgendes:

- `@type`: gibt den Typ der Funktion an (`Telemetry`, `Property` oder `Command`)
- `name`: gibt den Namen des Telemetriewerts an
- `schema`: gibt den Datentyp für die Telemetriedaten an Dieser Wert kann ein primitiver Typ sein, z. B. „double“, „integer“, „boolean“ oder „string“. Komplexe Objekttypen, Arrays und Zuordnungen werden ebenfalls unterstützt.

Über andere optionale Felder, z. B. Anzeigename und Beschreibung, können Sie der Schnittstelle und den Funktionen weitere Details hinzufügen.

### <a name="properties"></a>Properties

Standardmäßig sind Eigenschaften schreibgeschützt. Schreibgeschützte Eigenschaften bedeutet, dass das Gerät Aktualisierungen der Eigenschaftswerte an Ihren IoT-Hub sendet. Der IoT-Hub kann den Wert einer schreibgeschützten Eigenschaft nicht festlegen.

Sie können eine Eigenschaft auch als beschreibbar für eine Schnittstelle markieren. Ein Gerät kann eine Aktualisierung einer beschreibbaren Eigenschaft vom IoT-Hub empfangen sowie Aktualisierungen der Eigenschaftswerte an den Hub senden.

Geräte müssen nicht verbunden sein, um Eigenschaftswerte festzulegen. Die aktualisierten Werte werden übertragen, wenn das Gerät das nächste Mal mit dem Hub verbunden wird. Dieses Verhalten gilt sowohl für schreibgeschützte als auch für beschreibbare Eigenschaften.

Verwenden Sie keine Eigenschaften, um Telemetriedaten von Ihrem Gerät zu senden. Beispielsweise bedeutet die schreibgeschützte `temperatureSetting=80`-Eigenschaft, dass die Gerätetemperatur auf 80 festgelegt wurde und das Gerät versucht, diese Temperatur zu erreichen oder zu halten.

Bei beschreibbaren Eigenschaften gibt die Geräteanwendung den Statuscode, die Version und die Beschreibung eines gewünschten Zustands zurück, um anzugeben, ob der Eigenschaftswert empfangen und angewandt wurde.

### <a name="telemetry"></a>Telemetrie

Standardmäßig leitet IoT Hub alle Telemetrienachrichten von Geräten an den [ integrierten dienstseitigen Endpunkt (**messages/events**)](../iot-hub/iot-hub-devguide-messages-read-builtin.md) weiter, der mit [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/) kompatibel ist.

Über [benutzerdefinierte Endpunkte und Routingregeln von IoT Hub](../iot-hub/iot-hub-devguide-messages-d2c.md) können Sie Telemetriedaten an andere Ziele wie Blob Storage oder andere Event Hubs senden. Routingregeln verwenden Nachrichteneigenschaften zum Auswählen von Nachrichten.

### <a name="commands"></a>Befehle

Befehle sind entweder synchron oder asynchron. Ein synchroner Befehl muss standardmäßig innerhalb von 30 Sekunden ausgeführt werden, und das Gerät muss beim Eingehen des Befehls verbunden sein. Wenn das Gerät nicht rechtzeitig antwortet oder nicht verbunden ist, wird der Befehl nicht ausgeführt.

Verwenden Sie asynchrone Befehle für Vorgänge mit langer Ausführungszeit. Das Gerät sendet Statusinformationen unter Verwendung von Telemetrienachrichten. Diese Statusmeldungen enthalten die folgenden Headereigenschaften:

- `iothub-command-name`: der Befehlsname, z. B. `UpdateFirmware`
- `iothub-command-request-id`: die Anforderungs-ID, die serverseitig generiert und beim ersten Aufruf an das Gerät gesendet wurde
- `iothub-interface-id`:  die ID der Schnittstelle, für die dieser Befehl definiert ist, z. B. `urn:example:AssetTracker:1`
 `iothub-interface-name`: der Instanzname der Schnittstelle, z. B. `myAssetTracker`
- `iothub-command-statuscode`: der vom Gerät zurückgegebene Statuscode, z. B. `202`

## <a name="register-a-device"></a>Registrieren eines Geräts

Mit IoT Plug & Play lassen sich die Funktionen des Geräts ganz einfach veröffentlichen. Bei Verwendung von IoT Plug & Play müssen Sie das Gerätefunktionsmodell registrieren, nachdem Ihr Gerät mit IoT Hub verbunden wurde. Die Registrierung ermöglicht es Kunden, die IoT Plug & Play-Funktionen des Geräts zu verwenden.

In dieser Anleitung wird erläutert, wie Sie ein Gerät mithilfe des Azure IoT-Geräte-SDK für C registrieren.

Für jede Schnittstelle, die mit Ihrem Gerät implementiert wird, müssen Sie eine Schnittstelle erstellen und mit der zugehörigen Implementierung verbinden.

Für die weiter oben gezeigte Thermostatschnittstelle erstellen Sie bei Verwendung des C SDK Ihre Thermostatschnittstelle und verbinden sie mit der zugehörigen Implementierung:

```c
DIGITALTWIN_INTERFACE_HANDLE thermostatInterfaceHandle;

DIGITALTWIN_CLIENT_RESULT result = DigitalTwin_InterfaceClient_Create(
    "thermostat",
    "urn:example:Thermostat:1",
    null, null,
    &thermostatInterfaceHandle);

result = DigitalTwin_Interface_SetCommandsCallbacks(
    thermostatInterfaceHandle,
    commandsCallbackTable);

result = DigitalTwin_Interface_SetPropertiesUpdatedCallbacks(
    thermostatInterfaceHandle,
    propertiesCallbackTable);

```

Wiederholen Sie diesen Code für jede Schnittstelle, die mit Ihrem Gerät implementiert wird.

Nachdem Sie eine Schnittstelle erstellt haben, registrieren Sie das Gerätefunktionsmodell und die Schnittstellen bei Ihrem IoT-Hub:

```c
DIGITALTWIN_INTERFACE_CLIENT_HANDLE interfaces[2];
interfaces[0] = thermostatInterfaceHandle;
interfaces[1] = deviceInfoInterfaceHandle;

result = DigitalTwin_DeviceClient_RegisterInterfacesAsync(
    digitalTwinClientHandle, // The handle for the connection to Azure IoT
    "urn:example:Thermostat_T_1000:1",
    interfaceHandleList, 2,
    null, null);
```

## <a name="use-a-device"></a>Verwenden eines Geräts

Mithilfe von IoT Plug & Play können Sie Geräte verwenden, deren Funktionen bei Ihrem IoT-Hub registriert wurden. Beispielsweise können Sie direkt auf die Eigenschaften und Befehle eines Geräts zugreifen.

Zur Verwendung eines mit dem IoT-Hub verbundenen IoT Plug & Play-Geräts können Sie entweder die IoT Hub-REST-API oder eines der IoT-Sprach-SDKs nutzen. In den folgenden Beispielen wird die IoT Hub-REST-API verwendet.

Zum Abrufen des Werts einer Geräteeigenschaft, z. B. der Firmwareversion (`fwVersion`) in der `DeviceInformation`-Schnittstelle im Thermostat, verwenden Sie die Digital Twins-REST-API.

Wenn das Thermostatgerät den Namen `t-123` trägt, können Sie alle mit dem Gerät implementierten Eigenschaften mit einem GET-Aufruf der REST-API abrufen:

```REST
GET /digitalTwins/t-123/interfaces
```

Generell wird über folgende REST-API-Vorlage auf alle Eigenschaften zugegriffen, wobei `{device-id}` der Bezeichner für das Gerät ist:

```REST
GET /digitalTwins/{device-id}/interfaces
```

Wenn Sie den Namen der Schnittstelle kennen und Eigenschaften für eine bestimmte Schnittstelle abrufen möchten, können Sie die Anforderung auf diese spezifische Schnittstelle nach deren Namen einschränken:

```REST
GET /digitalTwins/t-123/interfaces/info
```

Generell kann über folgende REST-API-Vorlage auf die Eigenschaften einer spezifischen Schnittstelle zugegriffen werden, wobei `device-id` der Bezeichner für das Gerät und `{interface-name}` der Name der Schnittstelle ist:

```REST
GET /digitalTwins/{device-id}/interfaces/{interface-name}
```

Sie können Befehle des IoT Plug & Play-Geräts direkt aufrufen. Wenn die Schnittstelle `Thermostat` im Gerät `t-123` den Befehl `restart` enthält, können Sie diesen mit einem POST-Aufruf der REST-API aufrufen:

```REST
POST /digitalTwins/t-123/interfaces/thermostat/commands/restart
```

Generell können Befehle über folgende REST-API-Vorlage aufgerufen werden:

- `device-id`: der Bezeichner für das Gerät
- `interface-name`: der Name der Schnittstelle aus dem Abschnitt „implements“ im Gerätefunktionsmodell
- `command-name`: der Name des Befehls

```REST
/digitalTwins/{device-id}/interfaces/{interface-name}/commands/{command-name}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der Gerätemodellierung vertraut gemacht haben, finden Sie weitere Informationen in folgenden Artikeln:

- [Digital Twin Definition Language (DTDL)](https://aka.ms/DTDL)
- [C-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
- [IoT-REST-API](https://docs.microsoft.com/rest/api/iothub/device)
