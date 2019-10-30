---
title: Problembehandlung bei Redis-Client | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie häufige clientseitige Probleme bei Azure Cache for Redis beheben
services: cache
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 10/18/2019
ms.author: yegu
ms.openlocfilehash: 51039d7e586e598d443af737bcd7567fb59e1d63
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795250"
---
# <a name="troubleshoot-azure-cache-for-redis-client-side-issues"></a>Behandeln von clientseitigen Problemen bei Azure Cache for Redis

In diesem Abschnitt wird das Behandeln von Problemen beschrieben, die wegen eines Zustands des Redis-Clients auftreten, den Ihre Anwendung verwendet.

- [Hohe Arbeitsspeicherauslastung auf dem Redis-Client](#memory-pressure-on-redis-client)
- [Sprunghafter Anstieg des Datenverkehrsvolumens](#traffic-burst)
- [Hohe Auslastung der Client-CPU](#high-client-cpu-usage)
- [Clientseitige Bandbreitenbegrenzung](#client-side-bandwidth-limitation)
- [Große Anforderungen/große Antworten](#large-request-or-response-size)

## <a name="memory-pressure-on-redis-client"></a>Hohe Arbeitsspeicherauslastung auf dem Redis-Client

Hohe Arbeitsspeicherauslastung auf dem Clientcomputer führt zu Leistungsproblemen aller Art, die die Verarbeitung von Antworten aus dem Cache verzögern können. Wenn es zu hoher Arbeitsspeicherauslastung kommt, kann das System beginnen, Daten auf den Datenträger auszulagern. Diese sogenannten _Seitenfehler_ bewirken eine deutliche Verlangsamung des Systems.

So erkennen sie hohe Speicherauslastung auf dem Client

- Überwachen Sie die Speicherauslastung auf dem Computer und stellen Sie sicher, dass sie den verfügbaren Speicher nicht überschreitet.
- Überwachen Sie den Leistungsindikator `Page Faults/Sec` des Clients. Während des normalen Betriebs kommt es auf den meisten Systeme zu ein paar Seitenfehlern. Auftretende Spitzen bei Seitenfehlern, die mit Anforderungstimeouts korrespondieren, können auf hohe Arbeitsspeicherauslastung hindeuten.

Hoher Arbeitsspeicherauslastung auf dem Client kann auf mehrere Arten gemindert werden:

- Analysieren Sie die Muster Ihrer Arbeitsspeichernutzung, um die Auslastung auf dem Client zu verringern.
- Führen Sie ein Upgrade Ihrer Client-VM auf eine größere VM mit mehr Arbeitsspeicher aus.

## <a name="traffic-burst"></a>Sprunghafter Anstieg des Datenverkehrsvolumens

Plötzliche Anstiege des Datenverkehrsvolumens in Verbindung mit unzureichenden `ThreadPool` -Einstellungen können zu Verzögerungen beim Verarbeiten der vom Redis-Server bereits gesendeten, aber auf der Clientseite noch nicht genutzten Daten führen.

Überwachen Sie die Änderungen bei den `ThreadPool`-Statistiken im zeitlichen Verlauf unter Verwendung [eines Beispiel-`ThreadPoolLogger`-](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Sie können `TimeoutException`-Meldungen von StackExchange.Redis wie unten gezeigt verwenden, um weitere Untersuchungen anzustellen:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

In der oben aufgeführten Ausnahme zeigen sich mehrere interessante Probleme:

- Beachten Sie, dass in den Abschnitten `IOCP` und `WORKER` der `Busy`-Wert größer als der `Min`-Wert ist. Dieser Unterschied bedeutet, dass die `ThreadPool`-Einstellungen angepasst werden müssen.
- Außerdem wird `in: 64221` angegeben. Dieser Wert bedeutet, dass 64.211 Bytes auf der Kernelsocketebene des Clients empfangen, aber von der Anwendung noch nicht gelesen wurden. Dieser Unterschied bedeutet in der Regel, dass Ihre Anwendung (z. B. StackExchange.Redis) Daten aus dem Netzwerk nicht so schnell liest, wie sie vom Server gesendet werden.

Sie können Ihre [`ThreadPool`-Einstellungen konfigurieren](https://gist.github.com/JonCole/e65411214030f0d823cb), um sicherzustellen, dass Ihr Threadpool bei Datenverkehrsspitzen schnell hochskaliert wird.

## <a name="high-client-cpu-usage"></a>Hohe Auslastung der Client-CPU

Hohe Auslastung der Client-CPU weist darauf hin, dass das System mit der angeforderten Arbeitsmenge nicht Schritt halten kann. Obgleich der Client die Antwort schnell gesendet hat, schafft es der Client eventuell nicht, die Antwort rechtzeitig zu verarbeiten.

Überwachen Sie die systemweite CPU-Auslastung des Clients mithilfe der im Azure-Portal zur Verfügung stehenden Metriken oder mithilfe von Leistungsindikatoren auf dem Computer. Achten Sie darauf, dass Sie nicht die *Prozess*-CPU überwachen. Für einen einzelnen Prozess kann die CPU-Auslastung gering sein, während aber die systemweite CPU-Auslastung hoch sein kann. Beobachten Sie die Spitzen bei der CPU-Auslastung, denn sie korrespondieren mit Timeouts. Eine hohe CPU-Auslastung kann auch hohe Werte für `in: XXX` in den `TimeoutException`-Fehlermeldungen verursachen, wie im Abschnitt [Sprunghafter Anstieg des Datenverkehrsvolumens](#traffic-burst) beschrieben.

> [!NOTE]
> Ab StackExchange.Redis 1.1.603 ist in den `TimeoutException`-Fehlermeldungen die Metrik `local-cpu` enthalten. Verwenden Sie immer die neueste Version des [NuGet-Pakets für StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/). Es werden ständig Fehler im Code behoben, um ihn widerstandsfähiger gegen Timeouts zu machen. Das Nutzen der neuesten Version ist also wichtig.
>

So mindern Sie eine hohe CPU-Auslastung beim Client

- Untersuchen Sie, was CPU-Spitzen verursacht.
- Führen Sie ein Upgrade Ihres Clients auf eine größere VM mit höherer CPU-Kapazität aus.

## <a name="client-side-bandwidth-limitation"></a>Clientseitige Bandbreitenbegrenzung

Je nach Architektur der Clientcomputer bestehen möglicherweise Einschränkungen im Hinblick auf die verfügbare Netzwerkbandbreite. Wenn auf dem Client durch Überladen der Netzwerkkapazität die verfügbare Bandbreite überschritten wird, werden die Daten auf der Clientseite nicht so schnell verarbeitet, wie sie vom Server gesendet werden. Diese Situation kann zu Timeouts führen.

Überwachen Sie die Änderungen bei der Bandbreitennutzung im zeitlichen Verlauf unter Verwendung eines [Beispiel-`BandwidthLogger`](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Dieser Code wird in einigen Umgebungen mit eingeschränkten Berechtigungen (z.B. Azure-Websites) möglicherweise nicht erfolgreich ausgeführt.

Verringern Sie zur Minderung den Netzwerkbandbreitenverbrauch, oder vergrößern Sie die Client-VM auf eine mit höherer Netzwerkkapazität.

## <a name="large-request-or-response-size"></a>Große Anforderungen/große Antworten

Eine große Anforderung oder eine große Antwort kann Timeouts verursachen. Nehmen Sie beispielsweise an, dass auf Ihrem Client ein Timeoutwert von einer Sekunde konfiguriert ist. Ihre Anwendung fordert (unter Verwendung derselben physischen Verbindung) zwei Schlüssel gleichzeitig an (z.B. „A“ und „B“). Die meisten Clients unterstützen das „Pipelining“ für Anforderungen, wobei bei Anforderungen „A“ und „B“ nacheinander gesendet werden, ohne dass auf ihre Antworten gewartet wird. Der Server sendet die Antworten in der gleichen Reihenfolge zurück. Wenn die Antwort „A“ groß ist, kann sie den Großteil der Timeoutzeit für spätere Anforderungen verbrauchen.

Im folgenden Beispiel werden die Anforderungen „A“ und „B“ schnell an den Server gesendet. Der Server beginnt schnell mit dem Senden der Antworten „A“ und „B“. Aufgrund der Datenübertragungszeiten muss Antwort „B“ hinter Antwort „A“ warten und hat ein Timeout, obwohl der Server schnell geantwortet hat.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Dieses Anforderungs-/Antwortszenario ist schwer zu messen. Sie könnten Ihren Clientcode für die Nachverfolgung von großen Anforderungen und Antworten instrumentieren.

Lösungen für große Antworten variieren, umfass aber unter anderem:

1. Optimieren Sie Ihre Anwendung für eine große Anzahl von kleinen Werten, statt für wenige große Werte.
    - Die bevorzugte Lösung besteht darin, Ihre Daten in verknüpfte kleinere Werte aufzuteilen.
    - Ausführliche Informationen, warum kleinere Werte empfohlen werden, finden Sie im Beitrag [What is the ideal value size range for redis? Is 100 KB too large?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) (Welche Größe ist für Werte bei Redis ideal? Sind 100 KB zu viel?).
1. Erhöhen Sie die Größe Ihres virtuellen Computers, um höhere Bandbreitenkapazitäten zu erhalten.
    - Mehr Bandbreite auf ihrer Client- oder Server-VM kann die Datenübertragungszeiten für größere Antworten verringern.
    - Vergleichen Sie Ihre aktuelle Netzwerknutzung auf beiden Computern mit den Limits Ihrer aktuellen VM-Größe. Mehr Bandbreite nur auf dem Server oder nur auf dem Client ist möglicherweise nicht ausreichend.
1. Erhöhen Sie die Anzahl von Verbindungsobjekten, die Ihre Anwendung verwendet.
    - Verwenden Sie einen Roundrobinansatz, um Anforderungen über verschiedene Verbindungsobjekte auszuführen.

## <a name="additional-information"></a>Zusätzliche Informationen

- [Behandeln von serverseitigen Problemen bei Azure Cache for Redis](cache-troubleshoot-server.md)
- [Wie kann ich die Leistung meines Caches messen und testen?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
