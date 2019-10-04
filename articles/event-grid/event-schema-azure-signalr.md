---
title: Azure Event Grid-Ereignisschema für Azure SignalR
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für SignalR-Ereignisse bereitgestellt werden
services: event-grid
author: chenyl
ms.service: event-grid
ms.topic: reference
ms.date: 06/11/2019
ms.author: chenyl
ms.openlocfilehash: 3b072ff2b680ad6d144c7441190ab2df9870f5d0
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788513"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>Azure Event Grid-Ereignisschema für SignalR

In diesem Artikel werden die Eigenschaften und das Schema für SignalR-Ereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).


## <a name="available-event-types"></a>Verfügbare Ereignistypen

SignalR Service gibt die folgenden Ereignistypen aus:

| Ereignistyp | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | Wird ausgelöst, wenn eine Clientverbindung verbunden wird. |
| Microsoft.SignalRService.ClientConnectionDisconnected | Wird ausgelöst, wenn eine Clientverbindung getrennt wird. |

## <a name="example-event"></a>Beispielereignis

Das folgende Beispiel zeigt das Schema eines Ereignisses vom Typ „Clientverbindung verbunden“: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Das Schema für ein Ereignis vom Typ „Clientverbindung getrennt“ ist ähnlich: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| topic | Zeichenfolge | Vollständiger Ressourcenpfaf zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | Zeichenfolge | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| data | object | SignalR Service-Ereignisdaten. |
| dataVersion | string | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | string | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist die folgenden Eigenschaften auf:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| timestamp | Zeichenfolge | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| hubName | Zeichenfolge | Der Hub, zu dem die Clientverbindung gehört. |
| connectionId | Zeichenfolge | Der eindeutige Bezeichner für die Clientverbindung. |
| userId | Zeichenfolge | Der im Anspruch definierte Benutzerbezeichner. |
| errorMessage | Zeichenfolge | Der Fehler, der bewirkt, dass die Verbindung getrennt wird. |

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
