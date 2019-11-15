---
title: Failover und Patching – Azure Cache for Redis | Microsoft-Dokumentation
description: Informationen zum Failover, Patchen und Updateprozess für Azure Cache for Redis.
services: cache
author: asasine
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: adsasine
ms.openlocfilehash: 22c48441795e8aff9aba6540f15130452bcec2f7
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819171"
---
# <a name="failover-and-patching-for-azure-cache-for-redis"></a>Failover und Patching für Azure Cache for Redis

Zum Erstellen resilienter und erfolgreicher Clientanwendungen ist es wichtig, das Failover im Zusammenhang mit dem Azure Cache for Redis-Dienst zu verstehen. Ein Failover kann Teil geplanter Verwaltungsvorgänge sein oder durch ungeplante Hardware- oder Netzwerkausfälle verursacht werden. Ein Cachefailover erfolgt häufig, wenn der Verwaltungsdienst die Azure Cache for Redis-Binärdateien patcht. In diesem Artikel wird erläutert, was ein Failover ist, wie es beim Patchen erfolgt und wie eine resiliente Clientanwendung erstellt wird.

## <a name="what-is-a-failover"></a>Was ist ein Failover?

Wir beginnen mit einer Übersicht über das Failover für Azure Cache for Redis.

### <a name="a-quick-summary-of-cache-architecture"></a>Kurze Zusammenfassung der Cachearchitektur

Ein Cache wird von mehreren virtuellen Computern mit separaten privaten IP-Adressen erstellt. Jeder virtuelle Computer, der auch als Knoten bezeichnet wird, ist mit einem freigegebenen Load Balancer mit einer einzelnen virtuellen IP-Adresse verbunden. Jeder Knoten führt den Redis-Serverprozess aus und ist über den Hostnamen und die Redis-Ports zugänglich. Jeder Knoten gilt entweder als Master- oder als Replikatknoten. Wenn eine Clientanwendung eine Verbindung mit einem Cache herstellt, durchläuft der zugehörige Datenverkehr diesen Load Balancer und wird automatisch an den Masterknoten weitergeleitet.

In einem Basic-Cache ist der einzelne Knoten immer auch der Masterknoten. Ein Standard- oder Premium-Cache umfasst zwei Knoten: ein ausgewählter Masterknoten und ein Replikatknoten. Da Standard- und Premium-Caches mehrere Knoten umfassen, ist ein Knoten möglicherweise nicht verfügbar, während auf dem anderen weiterhin Anforderungen verarbeitet werden. Gruppierte Caches bestehen aus vielen Shards, die jeweils unterschiedliche Master- und Replikatknoten umfassen. Ein Shard kann ausgefallen sein, während die anderen verfügbar bleiben.

> [!NOTE]
> Ein Basic-Cache verfügt nicht über mehrere Knoten und für seine Verfügbarkeit gibt es keine Vereinbarung zum Service Level (SLA). Basic-Caches werden nur zu Entwicklungs- und Testzwecken empfohlen. Verwenden Sie einen Standard- oder Premium-Cache für eine Bereitstellung mit mehreren Knoten, um die Verfügbarkeit zu erhöhen.

### <a name="explanation-of-a-failover"></a>Erläuterung eines Failovers

Ein Failover erfolgt, wenn sich ein Replikatknoten als Masterknoten höher stuft und vorhandene Verbindungen des alten Masterknotens geschlossen werden. Nachdem der Masterknoten wieder verfügbar ist, wird die Änderung der Rollen erkannt, und er stuft sich als Replikatknoten tiefer. Anschließend wird eine Verbindung mit dem neuen Masterknoten hergestellt, und die Daten werden synchronisiert. Ein Failover kann geplant oder ungeplant sein.

Ein *geplantes Failover* erfolgt bei Systemupdates, z. B. beim Patchen von Redis oder bei Betriebssystemupgrades, und bei Verwaltungsvorgängen, z. B. Skalierung und Neustart. Da die Knoten eine Vorabbenachrichtigung zum Update erhalten, können die Rollen getauscht werden, und der Load Balancer kann schnell entsprechend der Änderung aktualisiert werden. Ein geplantes Failover wird normalerweise in weniger als einer Sekunde abgeschlossen.

Ein *ungeplantes Failover* kann aufgrund eines Hardwarefehlers, eines Netzwerkfehlers oder anderer unerwarteter Ausfälle des Masterknotens erfolgen. Der Replikatknoten stuft sich als Masterknoten höher, der Prozess dauert jedoch länger. Ein Replikatknoten muss zunächst erkennen, dass der zugehörige Masterknoten nicht verfügbar ist, bevor der Failoverprozess initiiert werden kann. Der Replikatknoten muss außerdem überprüfen, ob dieser ungeplante Ausfall nicht vorübergehend oder lokal ist, um ein unnötiges Failover zu vermeiden. Diese Verzögerung bei der Erkennung bedeutet, dass ein ungeplantes Failover normalerweise innerhalb von 10 bis 15 Sekunden abgeschlossen ist.

## <a name="how-does-patching-occur"></a>Wie erfolgt das Patchen?

Der Azure Cache for Redis-Dienst aktualisiert den Cache regelmäßig mit den neuesten Plattformfunktionen und Korrekturen. Zum Patchen eines Caches führt der Dienst die folgenden Schritte aus:

1. Der Verwaltungsdienst wählt einen zu patchenden Knoten aus.
1. Wenn der ausgewählte Knoten ein Masterknoten ist, stuft sich der zugehörige Replikatknoten höher. Diese Höherstufung gilt als geplantes Failover.
1. Der ausgewählte Knoten wird neu gestartet, um die neuen Änderungen zu übernehmen, und ist als Replikatknoten wieder verfügbar.
1. Der Replikatknoten stellt eine Verbindung mit dem Masterknoten her und synchronisiert die Daten.
1. Wenn die Datensynchronisierung abgeschlossen ist, wird der Patchprozess für die verbleibenden Knoten wiederholt.

Da das Patchen ein geplantes Failover ist, stuft sich der Replikatknoten schnell als Masterknoten höher und beginnt mit der Verarbeitung von Anforderungen und neuen Verbindungen. Basic-Caches verfügen über keinen Replikatknoten und sind bis zum Abschluss des Updates nicht verfügbar. Jeder Shard eines gruppierten Caches wird separat gepatcht und schließt keine Verbindungen mit einem anderen Shard.

> [!IMPORTANT]
> Knoten werden einzeln gepatcht, um Datenverluste zu vermeiden. Bei Basic-Caches kommt es zu Datenverlust. Bei gruppierten Caches werden die Shards jeweils einzeln gepatcht.

Mehrere Caches in derselben Ressourcengruppe und Region werden ebenfalls einzeln gepatcht.  Caches, die sich in unterschiedlichen Ressourcengruppen oder Regionen befinden, können gleichzeitig gepatcht werden.

Da die vollständige Datensynchronisierung ausgeführt wird, bevor der Prozess wiederholt wird, ist es unwahrscheinlich, dass bei der Verwendung eines Standard- oder Premium-Caches Datenverluste auftreten. Sie können Datenverlusten weiter vorbeugen, indem Sie die Daten [exportieren](cache-how-to-import-export-data.md#export) und die [Datenpersistenz](cache-how-to-premium-persistence.md) aktivieren.

## <a name="additional-cache-load"></a>Zusätzliche Cacheauslastung

Bei jedem Failover müssen in den Standard- und Premium-Caches Daten von einem auf den anderen Knoten repliziert werden. Diese Replikation führt zu einer gewissen Erhöhung der Auslastung von Serverspeicher und Server-CPU. Wenn die Cache-Instanz bereits stark ausgelastet ist, können Clientanwendungen eine höhere Latenz aufweisen. Im Extremfall treten in Clientanwendungen möglicherweise Timeoutausnahmen auf. Um die Auswirkungen dieser zusätzlichen Auslastung zu verringern, [konfigurieren](cache-configure.md#memory-policies) Sie die Einstellung `maxmemory-reserved` des Caches.

## <a name="how-does-a-failover-affect-my-client-application"></a>Wie wirkt sich ein Failover auf meine Clientanwendung aus?

Die Anzahl der in der Clientanwendung erkannten Fehler hängt davon ab, wie viele Vorgänge zum Zeitpunkt des Failovers für die entsprechende Verbindung ausstehend waren. Bei jeder Verbindung, die über den Knoten weitergeleitet werden, dessen Verbindungen geschlossen wurden, treten Fehler auf. Viele Clientbibliotheken können unterschiedliche Arten von Fehlern auslösen, wenn Verbindungen unterbrochen werden, einschließlich Timeoutausnahmen, Verbindungsausnahmen oder Socketausnahmen. Anzahl und Typ der Ausnahmen hängen davon ab, wo sich die Anforderung im Codepfad befindet, wenn die Verbindungen im Cache geschlossen werden. Beispielsweise tritt bei einem Vorgang, bei dem eine Anforderung gesendet wird, für die zum Zeitpunkt des Failovers jedoch noch keine Antwort zurückgegeben wurde, möglicherweise eine Timeoutausnahme auf. Bei neuen Anforderungen für das geschlossene Verbindungsobjekt treten Verbindungsausnahmen auf, bis die Verbindung erfolgreich wiederhergestellt wird.

Die meisten Clientbibliotheken versuchen, erneut eine Verbindung mit dem Cache herzustellen, wenn sie entsprechend konfiguriert sind. Durch unvorhergesehene Fehler können die Bibliotheksobjekte jedoch gelegentlich in einen nicht wiederherstellbaren Zustand gesetzt werden. Wenn Fehler länger als eine vorkonfigurierte Zeitspanne andauern, sollte das Verbindungsobjekt neu erstellt werden. In Microsoft .NET und anderen objektorientierten Sprachen kann die Neuerstellung der Verbindung ohne Neustart der Anwendung durch Verwendung eines [Lazy\<T\>-Musters](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#reconnecting-with-lazyt-pattern) erreicht werden.

### <a name="how-do-i-make-my-application-resilient"></a>Wie mache ich meine Anwendung resilient?

Da Failover nicht vollständig vermieden werden können, müssen Sie Ihre Clientanwendungen so konfigurieren, dass sie für Verbindungsunterbrechungen und fehlerhafte Anforderungen resilient sind. Obwohl die meisten Clientbibliotheken automatisch erneut eine Verbindung mit dem Cacheendpunkt herstellen, versuchen nur wenige, fehlerhafte Anforderungen zu wiederholen. Je nach Anwendungsszenario ist es möglicherweise sinnvoll, Wiederholungslogik mit Backoff zu verwenden.

Verwenden Sie zum Testen der Resilienz einer Clientanwendung einen [Neustart](cache-administration.md#reboot) als manuellen Auslöser für Verbindungsunterbrechungen. Außerdem wird empfohlen, dass Sie [Updates](cache-administration.md#schedule-updates) für einen Cache planen. Legen Sie für den Verwaltungsdienst fest, dass während der angegebenen wöchentlichen Zeitfenster Redis-Laufzeitpatches angewandt werden. Bei diesen Zeitfenstern handelt es sich normalerweise um Zeiträume, in denen der Datenverkehr der Clientanwendung gering ist.

### <a name="client-network-configuration-changes"></a>Änderungen der Clientnetzwerkkonfiguration

Bestimmte Änderungen der clientseitigen Netzwerkkonfiguration können Fehler vom Typ „Keine Verbindung verfügbar“ auslösen. Dies betrifft beispielsweise folgende Änderungen:

- Austauschen der virtuellen IP-Adresse einer Clientanwendung zwischen Staging- und Produktionsslots
- Skalieren der Größe oder Anzahl der Instanzen der Anwendung

Diese Änderungen können zu einem Verbindungsproblem führen, das weniger als eine Minute dauert. Dies führt dazu, dass nicht nur die Verbindung zwischen der Clientanwendung und dem Azure Cache for Redis-Dienst, sondern wahrscheinlich auch die Verbindung mit anderen externen Netzwerkressourcen getrennt wird.

## <a name="next-steps"></a>Nächste Schritte

- [Planen von Updates](cache-administration.md#schedule-updates) für den Cache
- Testen der Anwendungsresilienz durch einen [Neustart](cache-administration.md#reboot)
- [Konfigurieren](cache-configure.md#memory-policies) von Speicherreservierungen und -richtlinien
