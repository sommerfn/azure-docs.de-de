---
title: Azure Service Bus-Diagnoseprotokolle | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Diagnoseprotokolle für Service Bus in Azure einrichten.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 41e0bdc1f04c9491ebe939f46b59ae4eb2bc7ab6
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592460"
---
# <a name="enable-diagnostic-logs-for-service-bus"></a>Aktivieren von Diagnoseprotokollen für Service Bus

Wenn Sie mit der Verwendung des Azure Service Bus-Namespace beginnen, möchten Sie ggf. kontrollieren, wie und wann Ihr Namespace erstellt, gelöscht oder aufgerufen wird. Dieser Artikel enthält eine Übersicht über alle verfügbaren Betriebs-/Diagnoseprotokolle.

Azure Service Bus unterstützt derzeit Aktivitäts-/Betriebsprotokolle, die **Verwaltungsvorgänge** erfassen, welche für den Azure Service Bus-Namespace ausgeführt werden. Diese Protokolle erfassen insbesondere den Vorgangstyp, darunter das Erstellen von Warteschlangen, verwendete Ressourcen und den Status des Vorgangs.

## <a name="operational-logs-schema"></a>Schema „Betriebsprotokolle“

Alle Protokolle werden im JSON-Format (JavaScript Object Notation) an den folgenden beiden Speicherorten gespeichert.

- **AzureActivity** – Zeigt Protokolle aus Vorgängen/Aktionen an, die für Ihren Namespace im Portal oder über Azure Resource Manager-Vorlagenbereitstellungen ausgeführt werden.
- **AzureDiagnostics** – Zeigt Protokolle aus Vorgängen/Aktionen an, die für Ihren Namespace mithilfe der API oder über Verwaltungsclients im Sprach-SDK ausgeführt werden.

JSON-Zeichenfolgen im Betriebsprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

| NAME | BESCHREIBUNG |
| ------- | ------- |
| ActivityId | Interne ID, die zum Identifizieren der angegebenen Aktivität verwendet wird |
| EventName | Vorgangsname |
| resourceId | Azure Resource Manager-Ressourcen-ID |
| SubscriptionId | Abonnement-ID |
| EventTimeString | Vorgangsdauer |
| EventProperties | Vorgangseigenschaften |
| Status | Vorgangsstatus |
| Caller | Aufrufer des Vorgangs (Azure-Portal oder Verwaltungsclient) |
| Category | OperationalLogs |

Hier ein Beispiel für eine JSON-Zeichenfolge im Betriebsprotokoll:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="what-eventsoperations-are-captured-in-operational-logs"></a>Welche Ereignisse/Vorgänge werden in Betriebsprotokollen erfasst?

Betriebsprotokolle erfassen alle Verwaltungsvorgänge, die für den Azure Service Bus-Namespace ausgeführt werden. Datenvorgänge werden aufgrund der hohen Menge an Datenvorgängen, die für Azure Service Bus durchgeführt werden, nicht erfasst.

> [!NOTE]
> Damit Sie Datenvorgänge besser nachverfolgen können, empfehlen wir Ihnen die clientseitige Nachverfolgung.

Die folgenden Verwaltungsvorgänge werden in Betriebsprotokollen erfasst: 

| `Scope` | Vorgang|
|-------| -------- |
| Namespace | <ul> <li> Erstellen des Namespace</li> <li> Aktualisieren des Namespace </li> <li> Löschen des Namespace </li>  </ul> | 
| Warteschlange | <ul> <li> Erstellen einer Warteschlange</li> <li> Aktualisieren einer Warteschlange</li> <li> Löschen einer Warteschlange </li> </ul> | 
| Thema | <ul> <li> Erstellen eines Themas </li> <li> Aktualisieren eines Themas </li> <li> Löschen eines Themas </li> </ul> |
| Subscription | <ul> <li> Erstellen des Abonnements </li> <li> Aktualisieren des Abonnements </li> <li> Löschen des Abonnements </li> </ul> |

> [!NOTE]
> **Lesevorgänge** werden in Betriebsprotokollen derzeit nicht nachverfolgt.

## <a name="how-to-enable-operational-logs"></a>Wie werden Betriebsprotokolle aktiviert?

Betriebsprotokolle sind standardmäßig deaktiviert. Führen Sie die folgenden Schritte aus, um die Diagnoseprotokolle zu aktivieren:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Azure Service Bus-Namespace, und klicken Sie unter **Überwachung** auf **Diagnoseeinstellungen**.

   ![Blatt „Navigation zu Diagnoseprotokollen“](./media/service-bus-diagnostic-logs/image1.png)

2. Klicken Sie auf **Diagnoseeinstellung hinzufügen**, um die Diagnoseeinstellungen zu konfigurieren.  

   ![Aktivieren der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image2.png)

3. Konfigurieren der Diagnoseeinstellungen
   1. Geben Sie einen **Namen** ein, um die Diagnoseeinstellungen zu identifizieren.
   2. Wählen Sie ein Ziel für die Diagnose aus.
      - Wenn Sie **Speicherkonto** auswählen, müssen Sie das Speicherkonto konfigurieren, in dem die Diagnosen gespeichert werden.
      - Wenn Sie **Event Hubs** auswählen, müssen Sie den entsprechenden Event Hub konfigurieren, in den die Diagnoseeinstellungen gestreamt werden.
      - Wenn Sie **Log Analytics** auswählen, müssen Sie angeben, an welche Instanz von Log Analytics die Diagnosen gesendet werden.
    3. Aktivieren Sie **OperationalLogs**.

       ![Ändern des Status der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image3.png)

4. Klicken Sie auf **Speichern**.


Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseprotokolle** angezeigt.

Weitere Informationen zum Konfigurieren der Diagnose finden Sie in der [Übersicht über Azure-Diagnoseprotokolle](../azure-monitor/platform/diagnostic-logs-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Unter den folgenden Links erfahren Sie mehr über Service Bus:

* [Einführung in Servicebus](service-bus-messaging-overview.md)
* [Erste Schritte mit Service Bus](service-bus-dotnet-get-started-with-queues.md)
