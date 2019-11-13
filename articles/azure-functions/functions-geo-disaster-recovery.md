---
title: 'Azure Functions: Georedundante Notfallwiederherstellung und Hochverfügbarkeit | Microsoft-Dokumentation'
description: Verwenden von geografischen Regionen für Redundanz und Failover in Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: jeconnoc
keywords: azure functions, muster, bewährte methode, funktionen
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: jehollan
ms.openlocfilehash: 1d75d58a6df622ffb1b277f75ceedc2c2a66369d
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73576243"
---
# <a name="azure-functions-geo-disaster-recovery"></a>Georedundante Notfallwiederherstellung in Azure Functions

Wenn ganze Azure-Regionen oder Rechenzentren Ausfallzeiten aufweisen, ist es für Compute entscheidend, die Verarbeitung in einer anderen Region fortzusetzen.  In diesem Artikel werden einige der Strategien erläutert, mit denen Sie Funktionen bereitstellen können, um Notfallwiederherstellung zu ermöglichen.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Azure Functions werden in einer bestimmten Region ausgeführt.  Um eine höhere Verfügbarkeit zu erzielen, können Sie dieselben Funktionen in mehreren Regionen bereitstellen.  Wenn mehrere Regionen verwendet werden, können Ihre Funktionen im Muster *aktiv/aktiv* oder *aktiv/passiv* ausgeführt werden.  

* Aktiv/aktiv. Beide Regionen sind aktiv und empfangen Ereignisse (dupliziert oder rotierend). Aktiv/aktiv wird für HTTPS-Funktionen in Kombination mit Azure Front Door empfohlen.
* Aktiv/passiv. Eine Region ist aktiv und empfängt Ereignisse, während sich eine sekundäre Region im Leerlauf befindet.  Wenn ein Failover erforderlich ist, wird die sekundäre Region aktiviert und übernimmt die Verarbeitung.  Dies wird für Nicht-HTTP-Funktionen wie Service Bus und Event Hubs empfohlen.

Weitere Informationen zu Bereitstellungen in mehreren Regionen finden Sie unter [Ausführen von Apps in mehreren Regionen](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

## <a name="activeactive-for-https-functions"></a>Aktiv/aktiv für HTTPS-Funktionen

Um Bereitstellungen von Funktionen vom Typ aktiv/aktiv zu erreichen, ist eine Komponente erforderlich, die die Ereignisse zwischen beiden Regionen koordinieren kann.  Für HTTPS-Funktionen erfolgt diese Koordinierung mithilfe von [Azure Front Door](../frontdoor/front-door-overview.md).  Azure Front Door kann HTTPS-Anforderungen zwischen mehreren regionalen Funktionen weiterleiten und Round-Robin-Vorgänge ausführen.  Außerdem wird regelmäßig die Integrität der einzelnen Endpunkte überprüft.  Wenn eine regionale Funktion nicht mehr auf Integritätsüberprüfungen reagiert, wird sie von Azure Front Door aus der Rotation entfernt, und Datenverkehr wird nur an fehlerfreie Funktionen weitergeleitet.  

![Architektur für Azure Front Door und Funktionen](media/functions-geo-dr/front-door.png)  

## <a name="activeactive-for-non-https-functions"></a>Aktiv/aktiv für Nicht-HTTPS-Funktionen

Sie können trotzdem Bereitstellungen vom Typ aktiv/aktiv für Nicht-HTTPS-Funktionen erreichen.  Sie müssen jedoch in Betracht ziehen, wie die beiden Regionen miteinander interagieren oder koordiniert werden.  Wenn Sie dieselbe Funktions-App in zwei Regionen bereitstellen würden, die jeweils die gleiche Service Bus-Warteschlange auslösen, würden sie als konkurrierende Consumer beim Auflösen der betreffenden Warteschlange fungieren.  Das bedeutet, dass jede Nachricht nur von einer der Instanzen verarbeitet wird, aber auch, dass es immer noch einen Single Point of Failure für den einzelnen Service Bus gibt.  Wenn Sie zwei Service Bus-Warteschlangen bereitstellen (eine in einer primären Region, eine in einer sekundären Region) und die beiden Funktions-Apps auf ihre Regionswarteschlange verweisen, liegt die Herausforderung darin, wie die Warteschlangennachrichten zwischen den beiden Regionen verteilt werden.  Dies bedeutet häufig, dass jeder Verleger versucht, eine Nachricht in *beiden* Regionen zu veröffentlichen, und jede Nachricht wird von beiden aktiven Funktions-Apps verarbeitet.  Obwohl dadurch ein Muster vom Typ aktiv/aktiv erstellt wird, entstehen andere Herausforderungen bei der Duplizierung von Compute und dabei, wann oder wie Daten konsolidiert werden.  Aus diesen Gründen wird empfohlen, dass Nicht-HTTPS-Trigger das Muster vom Typ aktiv/passiv verwenden.

## <a name="activepassive-for-non-https-functions"></a>Aktiv/passiv für Nicht-HTTPS-Funktionen

Aktiv/passiv bietet eine Möglichkeit, dass nur eine einzige Funktion jede Nachricht verarbeitet, stellt aber einen Mechanismus zur Verfügung, um im Notfall ein Failover in eine Sekundärregion durchzuführen.  Azure Functions funktioniert zusammen mit [georedundanter Wiederherstellung von Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md) und [georedundanter Wiederherstellung von Azure Event Hubs](../event-hubs/event-hubs-geo-dr.md).

Wenn Sie beispielsweise Azure Event Hubs-Trigger verwenden, würde das Muster vom Typ aktiv/passiv Folgendes umfassen:

* Azure Event Hub ist für eine primäre und eine sekundäre Region bereitgestellt.
* Georedundante Notfallwiederherstellung ist aktiviert, um den primären und sekundären Event Hub zu koppeln.  Dadurch wird auch ein „Alias“ erstellt, den Sie verwenden können, um eine Verbindung mit Event Hubs herzustellen und vom primären zum sekundären Event Hub zu wechseln, ohne die Verbindungsinformationen zu ändern.
* Funktions-Apps sind für eine primäre und eine sekundäre Region bereitgestellt.
* Die Funktions-Apps werden für die *direkte* (kein Alias) Verbindungszeichenfolge für den jeweiligen Event Hub ausgelöst. 
* Verleger für den Event Hub sollten ihre Veröffentlichungen über die Aliasverbindungszeichenfolge ausführen. 

![Beispielarchitektur für aktiv/passiv](media/functions-geo-dr/active-passive.png)

Vor dem Failover leiten Verleger, die an den freigegebenen Alias senden, Nachrichten an den primären Event Hub weiter.  Die primäre Funktions-App lauscht exklusiv auf den primären Event Hub.  Die sekundäre Funktions-App ist passiv und befindet sich im Leerlauf.  Sobald das Failover eingeleitet wird, leiten die Verleger, die an den freigegebenen Alias senden, nun an den sekundären Event Hub weiter.  Die sekundäre Funktions-App wird nun aktiv und beginnt automatisch mit dem Auslösen.  Ein effektives Failover zu einer sekundären Region kann vollständig vom Event Hub gesteuert werden, wobei die Funktionen nur aktiv werden, wenn der jeweilige Event Hub aktiv ist.

Machen Sie sich mit weiteren Informationen und Überlegungen zum Failover mit [Service Bus](../service-bus-messaging/service-bus-geo-dr.md) und [Event Hubs](../event-hubs/event-hubs-geo-dr.md) vertraut.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen von Azure Front Door](../frontdoor/quickstart-create-front-door.md)
* [Überlegungen zum Event Hubs-Failover](../event-hubs/event-hubs-geo-dr.md#considerations)
