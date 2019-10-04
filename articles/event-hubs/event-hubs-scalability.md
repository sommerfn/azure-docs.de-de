---
title: Skalierbarkeit – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Skalieren von Azure Event Hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 06/18/2019
ms.author: shvija
ms.openlocfilehash: c46b333f2cc304cc12ddf78670b60940c7bc0db3
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827678"
---
# <a name="scaling-with-event-hubs"></a>Skalierung mit Event Hubs

Es gibt zwei Faktoren, die die Skalierung mit Event Hubs beeinflussen.
*   Durchsatzeinheiten
*   Partitionen

## <a name="throughput-units"></a>Durchsatzeinheiten

Die Durchsatzkapazität von Event Hubs wird durch *Durchsatzeinheiten* gesteuert. Durchsatzeinheiten werden vorab als Kapazitätseinheiten erworben. Ein einzelner Durchsatz ermöglicht Folgendes:

* Eingehende Daten: Bis zu 1 MB pro Sekunde oder 1.000 Ereignisse pro Sekunde, je nachdem, was zuerst eintritt.
* Ausgehende Daten: Bis zu 2 MB pro Sekunde oder 4.096 Ereignisse pro Sekunde.

Bei Überschreitung der Kapazität der erworbenen Durchsatzeinheiten wird der Eingang eingeschränkt und [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) zurückgegeben. Der Ausgang erstellt zwar keine Drosselungsausnahmen, die Kapazität der erworbenen Durchsatzeinheiten ist allerdings dennoch beschränkt. Wenn Sie Ausnahmen für die Veröffentlichungsrate erhalten oder einen größeren Ausgang erwarten, überprüfen Sie, wie viele Durchsatzeinheiten Sie für den Namespace erworben haben. Sie können Durchsatzeinheiten auf dem Blatt **Skalierung** der Namespaces im [Azure-Portal](https://portal.azure.com) verwalten. Mithilfe der [Event Hubs-APIs](event-hubs-api-overview.md) können Durchsatzeinheiten auch programmgesteuert verwaltet werden.

Durchsatzeinheiten werden im Voraus erworben und auf Stundenbasis abgerechnet. Nach dem Erwerb werden Durchsatzeinheiten für mit einem Minimum von einer Stunde in Rechnung gestellt. Bis zu 20 Durchsatzeinheiten können für einen Event Hubs-Namespace erworben und in allen Event Hubs dieses Namespace gemeinsam verwendet werden.

Das Feature **Automatische Vergrößerung** von Event Hubs wird automatisch durch Erhöhung der Anzahl von Durchsatzeinheiten zentral hochskaliert, um den Nutzungsanforderungen gerecht zu werden. Zusätzliche Durchsatzeinheiten verhindern Drosselungsszenarien, bei denen Folgendes vorliegt:

- Die Datenerfassungsrate überschreitet die festgelegten Durchsatzeinheiten.
- Die Raten für ausgehende Datenanforderungen überschreiten die festgelegten Durchsatzeinheiten.

Der Event Hubs-Dienst erhöht den Durchsatz, wenn die Last den minimalen Schwellenwert überschreitet, ohne dass Anforderungen mit ServerBusy-Fehlern auftreten. 

Weitere Informationen zum Feature für die automatische Vergrößerung finden Sie unter [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Partitionen
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]

### <a name="partition-key"></a>Partitionsschlüssel

Mit einem [Partitionsschlüssel](event-hubs-programming-guide.md#partition-key) können Sie eingehende Ereignisdaten spezifischen Partitionen zuordnen und die Daten so organisieren. Der Partitionsschlüssel ist ein vom Absender bereitgestellter Wert, der an einen Event Hub übergeben wird. Er wird über eine statische Hashfunktion verarbeitet, die die Partitionszuweisung erstellt. Wenn Sie beim Veröffentlichen eines Ereignisses keinen Partitionsschlüssel angeben, wird eine Roundrobinzuordnung verwendet.

Dem Ereignisherausgeber ist nur der Partitionsschlüssel bekannt, nicht die Partition, auf der die Ereignisse veröffentlicht werden. Dieses Entkoppeln von Schlüssel und Partition entbindet den Absender davon, zu viel über die Downstreamverarbeitung wissen zu müssen. Eine gerätebezogene oder für einen Benutzer eindeutige Identität stellt einen guten Partitionsschlüssel dar, es können aber auch andere Attribute wie z. B. Geografie zum Gruppieren von verwandten Ereignissen in einer einzelnen Partition verwendet werden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md)
- [Event Hubs-Dienst: Übersicht](event-hubs-what-is-event-hubs.md)
