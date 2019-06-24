---
title: Reagieren auf Schlüssel-Wert-Ereignisse von Azure App Configuration | Microsoft-Dokumentation
description: Verwenden Sie Azure Event Grid, um App Configuration-Ereignisse zu abonnieren.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 601124aef37d2b285db71130f5c63b3620c7768f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735879"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Reagieren auf Azure App Configuration-Ereignisse

Azure App Configuration-Ereignisse ermöglichen es Anwendungen, auf Schlüssel-Wert-Änderungen zu reagieren – ganz ohne komplizierten Code oder teure und ineffiziente Abrufdienste. Stattdessen werden die Ereignisse per Push über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Abonnenten wie [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) oder sogar Ihren eigenen benutzerdefinierten HTTP-Listener übertragen, und Sie zahlen nur für das, was Sie tatsächliche verwenden.

Azure App Configuration-Ereignisse werden an den Azure Event Grid-Dienst gesendet, der dank umfangreicher Wiederholungsrichtlinien und der Zustellung unzustellbarer Nachrichten zuverlässige Zustelldienste für Ihre Anwendungen bietet. Weitere Informationen finden Sie unter [Event Grid – Übermittlung und Wiederholung von Nachrichten](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Zu gängigen Szenarien mit App-Konfigurationsereignissen zählen beispielsweise das Aktualisieren der Anwendungskonfiguration, das Auslösen von Bereitstellungen sowie andere konfigurationsorientierte Workflows. Wenn Änderungen selten sind, aber Ihr Szenario die sofortige Reaktion erfordert, kann die ereignisbasierte Architektur besonders effizient sein.

Unter [Schnellstart: Weiterleiten von Azure App Configuration-Ereignissen an einen Webendpunkt mit der Azure-Befehlszeilenschnittstelle](./howto-app-configuration-event.md) finden Sie ein kurzes Beispiel. 

![Event Grid-Modell](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Verfügbare Azure App Configuration-Ereignisse
Event Grid verwendet [Ereignisabonnements](../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten. Azure App Configuration-Ereignisabonnements können zwei Arten von Ereignissen enthalten:  

> |Veranstaltungsname|BESCHREIBUNG|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Wird ausgelöst, wenn ein Schlüssel-Wert-Paar erstellt oder ersetzt wird.|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Wird ausgelöst, wenn ein Schlüssel-Wert-Paar gelöscht wird.|

## <a name="event-schema"></a>Ereignisschema
Azure App Configuration-Ereignisse enthalten alle Informationen, die Sie zur Reaktion auf Änderungen in Ihren Daten benötigen. Ein App -Konfigurationsereignis erkennen Sie daran, dass die eventType-Eigenschaft mit „Microsoft.AppConfiguration“ beginnt. Weitere Informationen zur Verwendung von Event Grid-Ereigniseigenschaften finden Sie unter [Event Grid-Ereignisschema](../event-grid/event-schema.md).  

> |Eigenschaft|Typ|BESCHREIBUNG|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |topic|Zeichenfolge|Vollständige Azure Resource Manager-ID der App-Konfiguration, die das Ereignis ausgibt.|
> |subject|Zeichenfolge|Die URL des Schlüssel-Wert-Paars, das Gegenstand des Ereignisses ist.|
> |eventTime|Zeichenfolge|Datum und Uhrzeit der Ereignisgenerierung im ISO 8601-Format.|
> |eventType|Zeichenfolge|„Microsoft.AppConfiguration.KeyValueModified“ oder „Microsoft.AppConfiguration.KeyValueDeleted“.|
> |id|Zeichenfolge|Ein eindeutiger Bezeichner für dieses Ereignis.|
> |dataVersion|Zeichenfolge|Die Schemaversion des Datenobjekts.|
> |metadataVersion|Zeichenfolge|Die Schemaversion der Eigenschaften oberster Ebene.|
> |data|object|Eine Sammlung Azure App Configuration-spezifischer Ereignisdaten.|
> |data.key|Zeichenfolge|Der Schlüssel des Schlüssel-Wert-Paars, das geändert oder gelöscht wurde.|
> |data.label|Zeichenfolge|Die Bezeichnung (sofern vorhanden) des Schlüssel-Wert-Paars, das geändert oder gelöscht wurde.|
> |data.etag|Zeichenfolge|Für `KeyValueModified`: Das ETag des neuen Schlüssel-Wert-Paars. Für `KeyValueDeleted`: Das ETag des gelöschten Schlüssel-Wert-Paars.|

Hier sehen Sie ein Beispiel für ein KeyValueModified-Ereignis:
```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

Weitere Informationen finden Sie unter [Azure Event Grid-Ereignisschema für Azure App Configuration](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Methoden zum Nutzen von Ereignissen
Anwendungen, die App-Konfigurationsereignisse behandeln, sollten einige empfohlene Methoden verwenden:
> [!div class="checklist"]
> * Da mehrere Abonnements zum Weiterleiten von Ereignissen an den gleichen Ereignishandler konfiguriert werden können, ist es wichtig, nicht davon auszugehen, dass Ereignisse von einer bestimmten Quelle stammen, sondern das Thema der Nachricht zu überprüfen, um sicherzugehen, dass sie von der erwarteten App-Konfiguration stammt.
> * Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
> * Da Nachrichten in falscher Reihenfolge und mit Verzögerung eintreffen können, verwenden Sie die etag-Felder, um zu verstehen, ob Ihre Informationen zu Objekten weiterhin auf dem neuesten Stand ist.  Verwenden Sie auch die sequencer-Felder, um die Reihenfolge der Ereignisse für ein bestimmtes Objekt zu verstehen.
> * Verwenden Sie das subject-Feld, um auf das geänderte Schlüssel-Wert-Paar zuzugreifen.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Event Grid, und probieren Sie Azure App Configuration-Ereignisse aus:

- [Einführung in Azure Event Grid](../event-grid/overview.md)
- [Schnellstart: Weiterleiten von Azure App Configuration-Ereignissen an einen Webendpunkt mit der Azure-Befehlszeilenschnittstelle](./howto-app-configuration-event.md)