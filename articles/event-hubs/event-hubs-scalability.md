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
ms.openlocfilehash: 3eb20013a6b3afaddce10f2e4652add0edf22a9a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276527"
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

Mit Partitionen können Sie die Skalierung für Ihre Downstreamverarbeitung durchführen. Aufgrund des partitionierten Consumermodells, das Event Hubs mit Partitionen bietet, können Sie die horizontale Skalierung bei gleichzeitiger Verarbeitung Ihrer Ereignisse vornehmen. Ein Event Hub kann über bis zu 32 Partitionen verfügen.

Es wird empfohlen, Durchsatzeinheiten und Partitionen 1:1 aufeinander abzustimmen, um eine optimale Skalierung zu erreichen. Eine einzelne Partition weist einen garantierten Ein- und Ausgang von bis zu einer Durchsatzeinheit auf. Sie können zwar einen höheren Durchsatz auf einer Partition erzielen, aber die Leistung ist nicht garantiert. Aus diesem Grund wird dringend empfohlen, dass die Anzahl der Partitionen in einem Event Hub größer oder gleich der Anzahl der Durchsatzeinheiten ist.

Angesichts des Gesamtdurchsatzes, den Sie planen zu benötigen, kennen Sie die Anzahl der benötigten Durchsatzeinheiten und die minimale Anzahl der Partitionen, aber wie viele Partitionen sollten Sie verwenden? Wählen Sie die Anzahl der Partitionen basierend auf der zu erreichenden Downstreamparallelität und Ihren zukünftigen Durchsatzanforderungen. Für die Anzahl der Partitionen, die Sie innerhalb eines Event Hubs verwenden, fällt keine Gebühr an.

Ausführliche Informationen zu Event Hubs-Preisen finden Sie unter [Event Hubs-Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie unter den folgenden Links:

- [Automatisches Skalieren von Durchsatzeinheiten](event-hubs-auto-inflate.md)
- [Event Hubs-Dienst: Übersicht](event-hubs-what-is-event-hubs.md)
