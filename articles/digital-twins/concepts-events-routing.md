---
title: 'Weiterleiten von Ereignissen und Nachrichten: Azure Digital Twins | Microsoft-Dokumentation'
description: Übersicht über das Weiterleiten von Ereignissen und Nachrichten an Dienstendpunkte mit Azure Digital Twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: f2479d9f3e278d23d62275b667f78d1fd70dd151
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73889706"
---
# <a name="routing-events-and-messages"></a>Weiterleiten von Ereignissen und Nachrichten

IoT-Lösungen vereinigen oft mehrere leistungsstarke Dienste, zu denen Speicher, Analysen usw. zählen. In diesem Artikel wird erläutert, wie Sie Azure Digital Twins-Apps mit Analyse-, KI- und Speicherdiensten von Azure verbinden, um detailliertere Erkenntnisse und Funktionen für die Apps bereitzustellen.

## <a name="route-types"></a>Routentypen  

Azure Digital Twins bietet zwei Möglichkeiten zum Verbinden von IoT-Ereignissen mit anderen Azure-Dienste oder Geschäftsanwendungen:

* **Weiterleiten von Azure Digital Twins-Ereignissen**: Objektänderungen im Raumgraphen, der Empfang von Telemetriedaten oder eine benutzerdefinierte Funktion, die basierend auf vordefinierten Bedingungen eine Benachrichtigung erstellt, können Azure Digital Twins-Ereignisse auslösen. Benutzer können diese Ereignisse zur weiteren Verarbeitung an [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure Service Bus-Themen](https://azure.microsoft.com/services/service-bus/) oder [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) senden.

* **Weiterleiten von Gerätetelemetrie**: Azure Digital Twins kann nicht nur Ereignisse, sondern auch nicht formatierte Gerätetelemetrienachrichten an Event Hubs weiterleiten, um weitere Informationen und Analysen bereitzustellen. Diese Nachrichtentypen werden nicht von Azure Digital Twins verarbeitet. Sie werden nur an den Event Hub weitergeleitet.

Benutzer können mindestens einen ausgehenden Endpunkt angeben, um Ereignisse zu senden oder Nachrichten weiterzuleiten. Ereignisse und Nachrichten werden gemäß diesen vordefinierten Routingeinstellungen an die Endpunkte gesendet. Anders ausgedrückt: Benutzer können einen Endpunkt zum Empfangen von Graphvorgangsereignissen, einen weiteren zum Empfangen von Gerätetelemetrieereignissen usw. angeben.

[![Weiterleiten von Azure Digital Twins-Ereignissen](media/concepts/digital-twins-events-routing.png)](media/concepts/digital-twins-events-routing.png#lightbox)

Bei der Weiterleitung an Event Hubs wird die Reihenfolge beibehalten, in der Telemetrienachrichten gesendet werden. Sie gehen daher in der Reihenfolge, in der sie ursprünglich empfangen wurden, auf dem Endpunkt ein. 

Event Grid und Service Bus garantieren nicht, dass Endpunkte die Ereignisse in der gleichen Reihenfolge empfangen, in der sie aufgetreten sind. Das Ereignisschema enthält jedoch einen Zeitstempel, mit dem die Reihenfolge ermittelt werden kann, nachdem die Ereignisse auf dem Endpunkt eingegangen sind.

## <a name="route-implementation"></a>Implementieren von Routen

Der Azure Digital Twins-Dienst unterstützt derzeit die folgenden Endpunkttypen (**EndpointTypes**):

* **EventHub** ist der Verbindungszeichenfolgen-Endpunkt von Event Hub.
* **ServiceBus** ist der Verbindungszeichenfolgen-Endpunkt von Service Bus.
* **EventGrid** ist der Verbindungszeichenfolgen-Endpunkt von Event Grid.

Azure Digital Twins unterstützt derzeit die folgenden Ereignistypen (**EventTypes**), die an den ausgewählten Endpunkt gesendet werden:

* **DeviceMessages** sind Telemetrienachrichten, die von Benutzergeräten gesendet und vom System weitergeleitet werden.
* **TopologyOperation** ist ein Vorgang, durch den der Graph oder seine Metadaten geändert werden. Ein Beispiel hierfür ist das Hinzufügen oder Löschen einer Entität, z. B. eines Gebäudebereichs.
* **SpaceChange** ist eine Änderung des berechneten Werts eines Gebäudebereichs aufgrund einer Gerätetelemetrienachricht.
* **SensorChange** ist eine Änderung des berechneten Werts eines Sensors aufgrund einer Gerätetelemetrienachricht.
* **UdfCustom** ist eine benutzerdefinierte Benachrichtigung von einer benutzerdefinierten Funktion.

> [!IMPORTANT]  
> Nicht jedes **EndpointTypes**-Element unterstützt alle **EventTypes**-Elemente.
> In der folgenden Tabelle sind die für jeden **EndpointType** zulässigen **EventTypes** aufgeführt.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| EventHub|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Weitere Informationen zum Erstellen von Endpunkten und Beispiele für Ereignisschemata finden Sie unter [Ausgehende Daten und Endpunkte](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den Einschränkungen der Vorschauversion von Azure Digital Twins finden Sie unter [Diensteinschränkungen der öffentlichen Vorschauversion](concepts-service-limits.md).

- Wenn Sie ein Azure Digital Twins-Beispiel ausprobieren möchten, fahren Sie mit [Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins](quickstart-view-occupancy-dotnet.md) fort.