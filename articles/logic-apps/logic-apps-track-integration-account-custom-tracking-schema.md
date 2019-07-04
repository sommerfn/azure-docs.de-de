---
title: Benutzerdefinierte Nachverfolgungsschemas für B2B-Nachrichten – Azure Logic Apps | Microsoft-Dokumentation
description: Erstellen benutzerdefinierter Nachverfolgungsschemas zur Überwachung von B2B-Nachrichten in Integrationskonten für Azure Logic Apps mit Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.date: 01/27/2017
ms.openlocfilehash: 76a9ece9e925543e856136a798a60038316caad9
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203042"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>Erstellen benutzerdefinierter Nachverfolgungsschemas zur Überwachung von End-to-End-Workflows in Azure Logic Apps

Es gibt eine integrierte Nachverfolgung, die Sie für die verschiedenen Teile des B2B-Workflows aktivieren können, z.B. für die Überwachung von AS2- oder X12-Nachrichten. Wenn Sie Workflows erstellen, die eine Logik-App, BizTalk Server, SQL Server oder andere Ebenen enthalten, dann können Sie die benutzerdefinierte Nachverfolgung aktivieren, bei der Ereignisse vom Anfang bis zum Ende des Workflows protokolliert werden. 

Dieser Artikel enthält benutzerdefinierten Code, den Sie in den Ebenen außerhalb Ihrer Logik-App verwenden können. 

## <a name="custom-tracking-schema"></a>Benutzerdefiniertes Nachverfolgungsschema

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": "",
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {                
         }
      }
   ]
}
```

| Eigenschaft | Erforderlich | Typ | BESCHREIBUNG |
| --- | --- | --- | --- |
| sourceType | Ja |   | Typ der Ausführungsquelle Zulässige Werte sind **Microsoft.Logic/workflows** und **custom** (benutzerdefiniert). |
| source | Ja |   | Wenn der Quelltyp **Microsoft.Logic/workflows** ist, müssen die Quellinformationen diesem Schema folgen. Wenn der Quelltyp **custom** ist, ist das Schema ist ein JToken. |
| systemId | Ja | string | System-ID der Logik-App |
| runId | Ja | string | Ausführungs-ID der Logik-App |
| operationName | Ja | string | Name des Vorgangs (z.B. Aktion oder Trigger) |
| repeatItemScopeName | Ja | string | Elementnamen wiederholen, wenn sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. |
| repeatItemIndex | Ja | Integer | Gibt an, ob sich die Aktion innerhalb einer `foreach`/`until`-Schleife befindet. Gibt den Index des Wiederholungselements an. |
| trackingId | Nein | string | Überwachungs-ID zum Korrelieren der Nachrichten |
| correlationId | Nein | string | Korrelations-ID zum Korrelieren der Nachrichten |
| clientRequestId | Nein | string | Kann vom Client zum Korrelieren von Nachrichten ausgefüllt werden. |
| eventLevel | Ja |   | Ebene des Ereignisses. |
| eventTime | Ja |   | Zeit des Ereignisses im UTC-Format YYYY-MM-DDTHH:MM:SS.00000Z. |
| recordType | Ja |   | Typ des Nachverfolgungsdatensatzes Der zulässige Wert ist **custom**. |
| record | Ja |   | Benutzerdefinierter Datensatztyp Das zulässige Format ist JToken. |
||||

## <a name="b2b-protocol-tracking-schemas"></a>B2B-Protokollnachverfolgungsschemas

Informationen zu B2B-Protokollnachverfolgungsschemas finden Sie unter:

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [X12-Nachverfolgungsschemas](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit der [Überwachung von B2B-Nachrichten](logic-apps-monitor-b2b-message.md) vertraut.
* Informieren Sie sich über das [Nachverfolgen von B2B-Nachrichten in Azure Monitor-Protokollen](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).
