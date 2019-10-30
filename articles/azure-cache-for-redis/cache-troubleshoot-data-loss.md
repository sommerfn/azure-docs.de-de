---
title: Problembehandlung bei Azure Cache for Redis | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Probleme mit Datenverlusten bei Azure Cache for Redis beheben.
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
ms.date: 10/17/2019
ms.author: yegu
ms.openlocfilehash: 507a8aef36e4ea5db0b4fdecb54876158398d96a
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795246"
---
# <a name="troubleshoot-data-loss"></a>Problembehandlung bei Datenverlusten

In diesem Abschnitt wird erläutert, wie Sie tatsächliche oder vermeintliche Datenverluste, die in Azure Cache for Redis auftreten können, diagnostizieren.

- [Partieller Verlust von Schlüsseln](#partial-loss-of-keys)
- [Größerer oder kompletter Verlust von Schlüsseln](#major-or-complete-loss-of-keys)

> [!NOTE]
> Etliche Problembehandlungsschritte in diesem Leitfaden enthalten Anleitungen zum Ausführen von Redis-Befehlen und zum Überwachen verschiedener Leistungsmetriken. Weitere Informationen und Anleitungen finden Sie in den Artikeln im Abschnitt [Weitere Informationen](#additional-information) .
>

## <a name="partial-loss-of-keys"></a>Partieller Verlust von Schlüsseln

Redis löscht Schlüssel nicht nach dem Zufallsprinzip, nachdem sie im Arbeitsspeicher gespeichert wurden. Schlüssel werden jedoch als Reaktion auf Ablauf- oder Entfernungsrichtlinien sowie durch explizite Befehle zum Löschen von Schlüsseln entfernt. Außerdem sind Schlüssel, die in Azure Cache for Redis (Tarife „Standard“ oder „Premium“) in den Masterknoten geschrieben wurden, möglicherweise nicht sofort auf einem Replikat verfügbar. Daten werden asynchron und auf nicht blockierende Weise vom Master auf das Replikat repliziert.

Wenn Sie feststellen, dass Schlüssel aus dem Cache verschwunden sind, können Sie Folgendes überprüfen, um nach der Ursache zu suchen:

| Ursache | BESCHREIBUNG |
|---|---|
| [Ablauf von Schlüsseln](#key-expiration) | Schlüssel werden aufgrund der für sie festgelegten Timeouts entfernt. |
| [Entfernen von Schlüsseln](#key-eviction) | Schlüssel werden bei hoher Arbeitsspeicherauslastung entfernt. |
| [Löschen von Schlüsseln](#key-deletion) | Schlüssel werden durch explizite Löschbefehle entfernt. |
| [Asynchrone Replikation](#async-replication) | Schlüssel sind aufgrund von Verzögerungen bei der Datenreplikation auf einem Replikat nicht verfügbar. |

### <a name="key-expiration"></a>Ablauf von Schlüsseln

Redis entfernt einen Schlüssel automatisch, wenn diesem ein Timeout zugewiesen wurde und dieser Zeitraum abgelaufen ist. Weitere Details zum Ablauf von Redis-Schlüsseln finden Sie in der Dokumentation zum Befehl [EXPIRE](http://redis.io/commands/expire). Timeoutwerte können auch über [SET](http://redis.io/commands/set), [SETEX](https://redis.io/commands/setex), [GETSET](https://redis.io/commands/getset) und andere \*STORE-Befehle festgelegt werden.

Mit dem Befehl [INFO](http://redis.io/commands/info) können Sie Statistiken abrufen, die angeben, wie viele Schlüssel abgelaufen sind. Im Abschnitt *Statistik* wird die Gesamtzahl abgelaufener Schlüssel angezeigt. Der Abschnitt *Keyspace* enthält zusätzliche Informationen zur Anzahl der Schlüssel mit Timeouts und dem durchschnittlichen Timeoutwert.

```
# Stats

expired_keys:46583

# Keyspace

db0:keys=3450,expires=2,avg_ttl=91861015336
```

Darüber hinaus können Sie die Diagnosemetriken für Ihren Cache überprüfen, um festzustellen, ob es eine Korrelation zwischen dem fehlendem Schlüssel und einer Spitze bei den abgelaufenen Schlüsseln gibt. Informationen zur Verwendung von Keyspace-Benachrichtigungen oder zum Debuggen dieser Arten von Problemen mit dem MONITOR-Befehl finden Sie im [Anhang](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix).

### <a name="key-eviction"></a>Entfernen von Schlüsseln

Redis benötigt Platz im Arbeitsspeicher zum Speichern von Daten. Bei Bedarf werden Schlüssel gelöscht, um verfügbaren Speicher freizugeben. Wenn sich die Werte **used_memory** oder **used_memory_rss** im Befehl [INFO](http://redis.io/commands/info) dem für die Einstellung **maxmemory** konfigurierten Wert nähern, beginnt Redis aufgrund der [Cacherichtlinie](http://redis.io/topics/lru-cache) mit dem Entfernen von Schlüsseln.

Mit dem Befehl [INFO](http://redis.io/commands/info) können Sie die Anzahl der entfernten Schlüssel überwachen.

```
# Stats

evicted_keys:13224
```

Darüber hinaus können Sie die Diagnosemetriken für Ihren Cache überprüfen, um festzustellen, ob es eine Korrelation zwischen dem fehlendem Schlüssel und einer Spitze bei den entfernten Schlüsseln gibt. Informationen zur Verwendung von Keyspace-Benachrichtigungen oder zum Debuggen dieser Arten von Problemen mit dem MONITOR-Befehl finden Sie im [Anhang](https://gist.github.com/JonCole/4a249477142be839b904f7426ccccf82#appendix).

### <a name="key-deletion"></a>Löschen von Schlüsseln

Redis-Clients können den Befehl [DEL](http://redis.io/commands/del) oder [HDEL](http://redis.io/commands/hdel) ausgeben, um Schlüssel explizit aus Redis zu entfernen. Mit dem Befehl [INFO](http://redis.io/commands/info) können Sie die Anzahl der Löschvorgänge verfolgen. Aufrufe von DEL- oder HDEL-Befehlen werden im Abschnitt *Befehlsstatistik* aufgelistet.

```
# Commandstats

cmdstat_del:calls=2,usec=90,usec_per_call=45.00

cmdstat_hdel:calls=1,usec=47,usec_per_call=47.00
```

### <a name="async-replication"></a>Asynchrone Replikation

Azure Cache for Redis ist sowohl im Tarif „Standard“ als auch im Tarif „Premium“ mit einem Masterknoten und mindestens einem Replikat konfiguriert. Daten werden über einen Prozess im Hintergrund asynchron aus dem Masterknoten in ein Replikat kopiert. Auf der Website [redis.io](http://redis.io/topics/replication) wird beschrieben, wie die Redis-Datenreplikation im Allgemeinen funktioniert. Bei Szenarien, in denen Clients häufig in Redis schreiben, kann aufgrund der Tatsache, dass eine sofortige Datenreplikation garantiert wird, ein partieller Datenverlust auftreten. Wenn beispielsweise der Masterknoten heruntergefahren wird, _nachdem_ ein Client einen Schlüssel in diesen geschrieben hat und _bevor_ der Hintergrundprozess die Möglichkeit hatte, diesen Schlüssel an das Replikat zu senden, geht der Schlüssel verloren, wenn das Replikat als neuer Masterknoten verwendet wird.

## <a name="major-or-complete-loss-of-keys"></a>Größerer oder kompletter Verlust von Schlüsseln

Wenn Sie feststellen, dass die meisten oder alle Schlüssel im Cache nicht mehr vorhanden sind, können Sie Folgendes überprüfen, um nach der Ursache zu suchen:

| Ursache | BESCHREIBUNG |
|---|---|
| [Leeren von Schlüsseln](#key-flushing) | Die Schlüssel wurden manuell gelöscht. |
| [Falsche Datenbankauswahl](#incorrect-database-selection) | Redis ist auf die Verwendung einer nicht standardmäßigen Datenbank eingestellt. |
| [Redis-Instanzausfall](#redis-instance-failure) | Schlüssel werden durch explizite Löschbefehle entfernt. |

### <a name="key-flushing"></a>Leeren von Schlüsseln

Clients können den Befehl [FLUSHDB](http://redis.io/commands/flushdb) aufrufen, um alle Schlüssel in einer **einzelnen** Datenbank zu entfernen. Mit dem Befehl [FLUSHALL](http://redis.io/commands/flushall) werden alle Schlüssel aus **allen** Datenbanken in einem Redis-Cache entfernt. Mit dem Befehl [INFO](http://redis.io/commands/info) können Sie feststellen, ob Schlüssel geleert wurden. Im Abschnitt *Befehlsstatistik* wird angezeigt, ob einer der FLUSH-Befehle aufgerufen wurde.

```
# Commandstats

cmdstat_flushall:calls=2,usec=112,usec_per_call=56.00

cmdstat_flushdb:calls=1,usec=110,usec_per_call=52.00
```

### <a name="incorrect-database-selection"></a>Falsche Datenbankauswahl

Azure Cache for Redis verwendet standardmäßig die Datenbank **db0**. Wenn Sie zu einer anderen Datenbank wechseln (z.B. db1) und versuchen, Schlüssel daraus zu lesen, kann Redis sie nicht finden, da jede Datenbank eine logisch separate Einheit mit unterschiedlichen Datasets bildet. Wählen Sie mit dem Befehl [SELECT](http://redis.io/commands/select) andere verfügbare Datenbanken aus, und suchen Sie dort jeweils nach Schlüsseln.

### <a name="redis-instance-failure"></a>Redis-Instanzausfall

Redis ist ein In-Memory-Datenspeicher. Die Daten werden auf den physischen oder virtuellen Computern aufbewahrt, die Redis hosten. Eine Azure Cache for Redis-Instanz im Tarif „Basic“ wird nur auf einem einzelnen virtuellen Computer (VM) ausgeführt. Wenn dieser virtuelle Computer ausfällt, gehen alle Daten verloren, die Sie im Cache gespeichert haben. Caches der Tarife „Standard“ und „Premium“ bieten eine wesentlich höhere Resilienz vor Datenverlusten, da zwei virtuelle Computer in einer replizierten Konfiguration verwendet werden. Wenn der Masterknoten bei einem solchen Cache ausfällt, übernimmt der Replikatknoten automatisch die Bereitstellung der Daten. Diese virtuellen Computer befinden sich in separaten Fehler- und Updatedomänen, um das Risiko zu minimieren, dass beide gleichzeitig nicht verfügbar sind. Beim Ausfall eines größeren Rechenzentrums können die virtuellen Computer jedoch auch zusammen ausfallen. In diesen seltenen Fällen gehen Ihre Daten verloren.

Sie sollten die Verwendung von [Redis-Datenpersistenz](http://redis.io/topics/persistence) und [Georeplikation](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-how-to-geo-replication) in Erwägung ziehen, um den Schutz Ihrer Daten vor derartigen Infrastrukturausfällen zu erhöhen.

## <a name="additional-information"></a>Zusätzliche Informationen

- [Behandeln von serverseitigen Problemen bei Azure Cache for Redis](cache-troubleshoot-server.md)
- [Welches Azure Cache for Redis-Angebot und welche Redis Cache-Größe sollte ich verwenden?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Überwachen von Azure Cache for Redis](cache-how-to-monitor.md)
- [Wie führe ich Redis-Befehle aus?](cache-faq.md#how-can-i-run-redis-commands)
