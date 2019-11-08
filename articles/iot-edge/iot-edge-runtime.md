---
title: Verwalten von Geräten mithilfe der Runtime –Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, wie die IoT Edge-Runtime Module, Sicherheit, Kommunikation und Berichterstellung auf Ihren Geräten verwaltet.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/01/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 94e33c855327e70f486746bcd781491823324dec
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490418"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Grundlegendes zur Azure IoT Edge-Runtime und ihrer Architektur

Die Azure IoT Edge-Runtime ist eine Sammlung von Programmen, die aus einem Gerät ein IoT Edge-Gerät machen. Die Sammlung der IoT Edge-Runtime-Komponenten ermöglicht IoT Edge-Geräten den Empfang von Code für die Ausführung im Edgebereich und das Kommunizieren der Ergebnisse. 

Die IoT Edge-Runtime führt auf IoT Edge-Geräten die folgenden Funktionen aus:

* Installieren und Aktualisieren von Workloads auf dem Gerät
* Aufrechterhalten von Azure IoT Edge-Sicherheitsstandards auf dem Gerät
* Sicherstellen, dass die [IoT Edge-Module](iot-edge-modules.md) immer ausgeführt werden
* Melden der Modulintegrität an die Cloud für die Remoteüberwachung
* Verwalten der Kommunikation zwischen nachgeschalteten Geräten und IoT Edge-Geräten.
* Verwalten der Kommunikation zwischen Modulen auf dem IoT Edge-Gerät.
* Verwalten der Kommunikation zwischen dem IoT Edge-Gerät und der Cloud.

![Die IoT Edge-Runtime übermittelt an IoT Hub Einblicke und Informationen zur Modulintegrität](./media/iot-edge-runtime/Pipeline.png)

Die Aufgaben der IoT Edge-Laufzeit fallen in zwei Kategorien: Kommunikation und Modulverwaltung. Diese beiden Rollen werden von zwei Komponenten ausgeführt, die Bestandteil der IoT Edge-Runtime sind. Der *IoT Edge-Hub* ist für die Kommunikation verantwortlich, während der *IoT Edge-Agent* die Module bereitstellt und überwacht. 

Der IoT Edge-Hub und der IoT Edge-Agent sind Module wie jedes andere auf einem IoT Edge-Gerät ausgeführte Modul. Manchmal werden sie auch als die *Runtimemodule* bezeichnet. 

## <a name="iot-edge-hub"></a>IoT Edge-Hub

Der IoT Edge-Hub ist eines der beiden Module, aus denen die Azure IoT Edge-Runtime besteht. Er fungiert als lokaler Proxy für IoT Hub, indem er die gleichen Protokollendpunkte wie IoT Hub verfügbar macht. Diese Konsistenz bedeutet, dass Clients (ob Geräte oder Module) auf die gleiche Weise eine Verbindung mit der IoT Edge-Laufzeit wie mit IoT Hub herstellen können. 

>[!NOTE]
> Der IoT Edge-Hub unterstützt Clients, die sich über MQTT oder AMQP verbinden. Clients, die HTTP verwenden, werden jedoch nicht unterstützt. 

Der IoT Edge-Hub ist keine vollständige Version des lokal ausgeführten IoT Hub. Einige Aufgaben werden vom IoT Edge-Hub automatisch an IoT Hub delegiert. Beispielsweise leitet der IoT Edge-Hub Authentifizierungsanforderungen an IoT Hub weiter, wenn ein Gerät zum ersten Mal eine Verbindung herzustellen versucht. Nachdem die erste Verbindung hergestellt wurde, werden die Sicherheitsinformationen vom IoT Edge-Hub lokal zwischengespeichert. Nachfolgende Verbindungen von diesem Gerät sind zulässig, ohne dass eine Authentifizierung bei der Cloud erforderlich ist. 

Der IoT Edge-Hub optimiert die Anzahl der tatsächlich hergestellten Verbindungen mit der Cloud, um die von der IoT Edge-Lösung genutzte Bandbreite zu reduzieren. Der IoT Edge-Hub fasst logische Verbindungen von Clients wie Modulen oder nachgeschalteten Geräten zu einer einzelnen physischen Verbindung mit der Cloud zusammen. Die Details dieses Vorgangs sind für den Rest der Lösung transparent. Clients können nicht erkennen, dass statt einer eigenen Verbindung mit der Cloud für alle Clients eine gemeinsame Verbindung verwendet wird. 

![Der IoT Edge-Hub ist ein Gateway zwischen physischen Geräten und IoT Hub](./media/iot-edge-runtime/Gateway.png)

Der IoT Edge-Hub kann ermitteln, ob er mit IoT Hub verbunden ist. Wenn die Verbindung unterbrochen wird, speichert der IoT Edge-Hub Nachrichten oder Zwillingsaktualisierungen lokal. Sobald eine Verbindung wieder hergestellt wurde, werden alle Daten synchronisiert. Der Speicherort für diesen temporären Cache wird durch eine Eigenschaft des Modulzwillings des IoT Edge-Hubs festgelegt. Die Größe des Caches wird nicht begrenzt, solange die Speicherkapazität des Geräts ausreicht. Weitere Informationen finden Sie unter [Offlinefunktionen](offline-capabilities.md).

### <a name="module-communication"></a>Modulkommunikation

Der IoT Edge-Hub ermöglicht die Kommunikation zwischen Modulen. Die Verwendung des IoT Edge-Hubs als Nachrichtenbroker sorgt dafür, dass die Module voneinander unabhängig bleiben. Module müssen nur die Eingänge, an denen sie Nachrichten akzeptieren, und die Ausgänge, an die sie Nachrichten schreiben, angeben. Ein Lösungsentwickler kann diese Eingänge und Ausgänge zusammenfügen, damit die Module Daten in der speziellen Reihenfolge für diese Lösung verarbeiten. 

![Der IoT Edge-Hub ermöglicht die Kommunikation zwischen Modulen.](./media/iot-edge-runtime/module-endpoints.png)

Ein Modul ruft zum Senden von Daten an den IoT Edge-Hub die SendEventAsync-Methode auf. Das erste Argument gibt an, an welchen Ausgang die Nachricht gesendet werden soll. Mit dem folgenden Pseudocode wird eine Nachricht an **output1** gesendet:

   ```csharp
   ModuleClient client = await ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync("output1", message); 
   ```

Um eine Nachricht zu empfangen, registrieren Sie einen Rückruf, der eingehende Nachrichten an einem speziellen Eingang verarbeitet. Mit dem folgenden Pseudocode wird die Funktion „messageProcessor“ registriert, die für die Verarbeitung aller an **input1** empfangenen Nachrichten verwendet werden soll:

   ```csharp
   await client.SetInputMessageHandlerAsync("input1", messageProcessor, userContext);
   ```

Weitere Informationen zur ModuleClient-Klasse und ihre Kommunikationsmethoden finden Sie in der API-Referenz für Ihre bevorzugte SDK-Sprache: [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet), [C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h), [Python](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device.iothubmoduleclient?view=azure-python), [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable) oder [Node.js](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest).

Der Lösungsentwickler muss die Regeln angeben, die festlegen, wie Nachrichten vom IoT Edge-Hub zwischen Modulen übergeben werden. Routingregeln werden in der Cloud definiert und an den IoT Edge-Hub in dessen Modulzwilling übertragen. Zum Definieren von Routen zwischen Modulen in Azure IoT Edge wird die gleiche Syntax wie für IoT Hub-Routen verwendet. Weitere Informationen finden Sie unter [Informationen zum Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md).   

![Das Routing zwischen Modulen erfolgt über den IoT Edge-Hub](./media/iot-edge-runtime/module-endpoints-with-routes.png)

## <a name="iot-edge-agent"></a>IoT Edge-Agent

Der IoT Edge-Agent ist das zweite Modul, aus dem die Azure IoT Edge-Laufzeit besteht. Er ist für das Instanziieren von Modulen verantwortlich, stellt sicher, dass sie weiterhin ausgeführt werden, und meldet den Status der Module an IoT Hub. Diese Konfigurationsdaten werden als Eigenschaft des Modulzwillings des IoT Edge-Agents geschrieben. 

Der [IoT Edge-Sicherheits-Daemon](iot-edge-security-manager.md) startet den IoT Edge-Agent beim Starten des Geräts. Der Agent ruft den Modulzwilling von IoT Hub ab und überprüft das Bereitstellungsmanifest. Das Bereitstellungsmanifest ist eine JSON-Datei, die die Module deklariert, die gestartet werden müssen. 

Jedes Element im Bereitstellungsmanifest enthält spezifische Informationen zu einem Modul und wird vom IoT Edge-Agent zum Steuern des Lebenszyklus des Moduls verwendet. Einige der wichtigeren Eigenschaften lauten: 

* **settings.image:** der Container, der vom IoT Edge-Agent zum Starten des Moduls verwendet wird. Der IoT Edge-Agent muss mit den Anmeldeinformationen für die Containerregistrierung konfiguriert werden, wenn das Image mit einem Kennwort geschützt ist. Anmeldeinformationen für die Containerregistrierung können remote mit dem Bereitstellungsmanifest konfiguriert werden oder auf dem IoT Edge-Gerät selbst durch Aktualisieren der Datei `config.yaml` im IoT Edge-Programmordner.
* **settings.createOptions** – Eine Zeichenfolge, die beim Starten des Containers eines Moduls direkt an den Moby-Container-Daemon übergeben wird. Das Hinzufügen von Optionen in dieser Eigenschaft ermöglicht erweiterte Konfigurationen, z.B. Portweiterleitung oder das Bereitstellen von Volumes im Container eines Moduls.  
* **status:** der Status, in den der IoT Edge-Agent das Modul versetzt. Dieser Wert wird in der Regel auf *running* festgelegt, da die meisten Personen möchten, dass der IoT Edge-Agent alle Module auf dem Gerät sofort startet. Sie können jedoch festlegen, dass der Anfangsstatus eines Moduls „Beendet“ lautet, und den IoT Edge-Agent zu einem zukünftigen Zeitpunkt zum Starten eines Moduls auffordern. Der IoT Edge-Agent meldet der Cloud den Status jedes Moduls in den gemeldeten Eigenschaften. Weicht die gemeldete Eigenschaft von der gewünschten Eigenschaft ab, ist dies ein Indikator für ein fehlerhaftes Gerät. Die unterstützten Status lauten:
   * Herunterladen
   * Wird ausgeführt
   * Fehlerhaft
   * Fehler
   * Beendet
* **restartPolicy:** legt fest, wie der IoT Edge-Agent ein Modul neu startet. Mögliche Werte sind:
   * `never` – Der IoT Edge-Agent führt niemals einen Neustart des Moduls aus.
   * `on-failure` – Wenn das Modul abstürzt, wird es vom IoT Edge-Agent neu gestartet. Wenn das Modul ordnungsgemäß heruntergefahren wird, führt der IoT Edge-Agent keinen Neustart des Moduls aus.
   * `on-unhealthy` – Wenn das Modul abstürzt oder als fehlerhaft betrachtet wird, startet es der IoT Edge-Agent neu.
   * `always` – Wenn das Modul abstürzt, als fehlerhaft betrachtet oder auf irgendeine Weise heruntergefahren wird, startet es der IoT Edge-Agent neu. 

Der IoT Edge-Agent sendet eine Runtimeantwort an IoT Hub. Im Folgenden sehen Sie eine Liste der möglichen Antworten:
  * 200 – OK
  * 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
  * 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
  * 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
  * 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
  * 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

Weitere Informationen finden Sie unter [Informationen zum Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md).   

### <a name="security"></a>Sicherheit

Der IoT Edge-Agent hat eine wichtige Funktion für die Sicherheit eines IoT Edge-Geräts. Er überprüft beispielsweise das Image eines Moduls, bevor er es startet. 

Weitere Informationen zum Azure IoT Edge-Sicherheitsframework finden Sie in der Dokumentation zu [IoT Edge Security Manager](iot-edge-security-manager.md).

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Azure IoT Edge-Modulen](iot-edge-modules.md)
