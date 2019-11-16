---
title: Was ist Azure Cache for Redis?
description: Erfahren Sie, was Azure Cache for Redis ist und wie dieser Cache üblicherweise verwendet wird.
author: yegu-ms
ms.service: cache
ms.topic: overview
ms.date: 03/26/2018
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 87e7505bddfce431b5e859fbbeee79f75867cfc9
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74122658"
---
# <a name="azure-cache-for-redis-description"></a>Beschreibung von Azure Cache for Redis

Azure Cache for Redis basiert auf der beliebten Software [Redis](https://redis.io/). Er wird in der Regel als Cache zum Verbessern der Leistung und Skalierbarkeit von Systemen verwendet, die stark von Back-End-Datenspeichern abhängig sind. Die Leistung wird durch vorübergehendes Kopieren häufig verwendeter Daten in einen schnellen Speicher verbessert, der sich in der Nähe der Anwendung befindet. Bei [Azure Cache for Redis](https://redis.io/) befindet sich dieser schnelle Speicher im Speicher mit Azure Cache for Redis, anstatt von einer Datenbank vom Datenträger geladen zu werden.

Azure Cache for Redis kann auch als In-Memory-Datenstrukturspeicher, verteilte nicht relationale Datenbank und Nachrichtenbroker verwendet werden. Die Anwendungsleistung wird durch die Nutzung der Leistung des Redis-Moduls mit niedriger Latenz und hohem Durchsatz verbessert.

Azure Cache for Redis bietet Ihnen Zugriff auf einen sicheren, dedizierten Redis Cache. Azure Cache for Redis wird von Microsoft verwaltet und in Azure gehostet, und jede Anwendung innerhalb oder außerhalb von Azure kann darauf zugreifen.

## <a name="using-azure-cache-for-redis"></a>Verwenden von Azure Cache for Redis

Es gibt viele allgemeine Muster, bei denen Azure Cache for Redis verwendet wird, um die Anwendungsarchitektur zu unterstützen oder die Anwendungsleistung zu verbessern. Nachfolgend sind einige der am häufigsten verwendeten Muster aufgeführt:

| Muster      | BESCHREIBUNG                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Da eine Datenbank groß sein kann, ist das Laden einer gesamten Datenbank in einen Cache als Ansatz nicht empfehlenswert. Das [cachefremde](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) Muster wird üblicherweise verwendet, um die Datenelemente nur bei Bedarf in den Cache zu laden. Wenn das System Änderungen an den Back-End-Daten vornimmt, kann es zu diesem Zeitpunkt auch den Cache aktualisieren, der mit anderen Clients verteilt wird. Darüber hinaus kann das System ein Ablaufdatum für Datenelemente festlegen oder eine Entfernungsrichtlinie verwenden, die bewirkt, dass Datenaktualisierungen erneut in den Cache geladen werden.|
| [Inhaltszwischenspeicherung](cache-aspnet-output-cache-provider.md) | Die meisten Webseiten werden anhand von Vorlagen mit Kopfzeilen, Fußzeilen, Symbolleisten, Menüs usw. generiert. Sie werden im Grunde nicht häufig geändert und sollten nicht dynamisch generiert werden. Im Vergleich zu Back-End-Datenspeichern bietet die Verwendung eines In-Memory-Cache wie Azure Cache for Redis Ihren Webservern schnellen Zugriff auf diese Art von statischen Inhalten. Dieses Muster verkürzt die Verarbeitungszeit und reduziert die Serverlast, die zum dynamischen Generieren von Inhalten erforderlich wären. Dadurch können Webserver schneller reagieren, und Sie können die Anzahl der zum Verarbeiten von Lasten erforderlichen Server reduzieren. Zur Unterstützung dieses Musters mit ASP.NET bietet Azure Cache for Redis den Redis-Ausgabecacheanbieter.|
| [Zwischenspeicherung der Benutzersitzung](cache-aspnet-session-state-provider.md) | Dieses Muster wird häufig bei Einkaufswagen und anderen Benutzerverlaufsinformationen verwendet, denen eine Webanwendung möglicherweise Benutzercookies zuordnen möchte. Die Speicherung von zu vielen Inhalten in einem Cookie kann sich negativ auf die Leistung auswirken, weil die Cookiegröße zunimmt und mit jeder Anforderung übergeben und überprüft wird. Eine typische Lösung besteht darin, das Cookie als Schlüssel zum Abfragen der Daten in einer Back-End-Datenbank zu verwenden. Die Verwendung eines In-Memory-Cache wie Azure Cache for Redis, um einem Benutzer Informationen zuzuordnen, ist wesentlich schneller als die Interaktion mit einer vollständigen relationalen Datenbank. |
| Einreihen von Aufträgen in eine Warteschlange und Message Queuing | Wenn Anwendungen Anforderungen erhalten, benötigen die mit der Anforderung verbundenen Vorgänge oft zusätzliche Zeit für die Ausführung. Es ist ein gängiges Muster, länger ausgeführte Vorgänge zu verzögern, indem sie einer Warteschlange hinzugefügt werden, die zu einem späteren Zeitpunkt und möglicherweise von einem anderen Server verarbeitet wird. Diese Methode der Verzögerung von Arbeit wird als Aufgaben-Queuing bezeichnet. Es gibt viele Softwarekomponenten, die Aufgabenwarteschlangen unterstützen. Azure Cache for Redis dient dazu auch als verteilte Warteschlange.|
| Verteilte Transaktionen | Anwendungen müssen oft eine Reihe von Befehlen für einen Back-End-Datenspeicher als einzelnen Vorgang (atomisch) ausführen können. Alle Befehle müssen erfolgreich ausgeführt oder jeweils mittels Rollback in den ursprünglichen Zustand zurückversetzt werden. Azure Cache for Redis unterstützt das Ausführen eines Batches von Befehlen als einzelnen Vorgang in Form von [Transaktionen](https://redis.io/topics/transactions). |

## <a name="azure-cache-for-redis-offerings"></a>Azure Cache for Redis-Angebote

Azure Cache for Redis ist in folgenden Tarifen verfügbar:

| Tarif | BESCHREIBUNG |
|---|---|
Basic | Ein Cache mit einem einzelnen Knoten. Dieser Tarif unterstützt mehrere Arbeitsspeichergrößen (250 MB - 53 GB). Dieser Tarif eignet sich ideal zu Entwicklungs-/Testzwecken und für nicht kritische Workloads. Für den Basic-Tarif gilt keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA). |
| Standard | Ein replizierter Cache in einer Primär-/Sekundärkonfiguration mit zwei Knoten, die von Microsoft verwaltet wird, mit Hochverfügbarkeits-SLA (99,9 %) |
| Premium | Die Dienstebene „Premium“ ist die Ebene für Unternehmen. Caches im Premium-Tarif unterstützen mehr Funktionen und bieten einen höheren Durchsatz bei niedrigeren Latenzen. Caches im Premium-Tarif werden auf leistungsfähigerer Hardware bereitgestellt und bieten im Vergleich zu den Tarifen „Basic“ und „Standard“ eine bessere Leistung. Dieser Vorteil bedeutet, dass der Durchsatz für einen Cache derselben Größe in der Premium-Dienstebene höher ist als in der Standard-Dienstebene. |

> [!TIP]
> Weitere Informationen zur Größe, zum Durchsatz und zur Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Cache for Redis](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use).
>

Nach der Erstellung können Sie Ihren Cache auf einen höheren Tarif skalieren. Die Herunterskalierung auf einen niedrigeren Tarif wird nicht unterstützt. Ausführliche Anweisungen zur Skalierung finden Sie unter [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Skalieren von Azure Cache for Redis) und [How to automate a scaling operation](cache-how-to-scale.md#how-to-automate-a-scaling-operation) (Automatisieren eines Skalierungsvorgangs).

### <a name="feature-comparison"></a>Funktionsvergleiche

Auf der Seite [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/) finden Sie einen detaillierten Vergleich der einzelnen Tarife. In der folgenden Tabelle werden einige der unterstützten Features nach Tarif aufgeführt:

| Featurebeschreibung | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis-Cluster](cache-how-to-premium-clustering.md) |✔|-|-|
| [Sicherheit über Firewallregeln](cache-configure.md#firewall) |✔|✔|✔|
| Verschlüsselung während der Übertragung |✔|✔|✔|
| [Verbesserte Sicherheit und Isolation mit VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/Export](cache-how-to-import-export-data.md) |✔|-|-|
| [Geplante Updates](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Georeplikation](cache-how-to-geo-replication.md) |✔|-|-|
| [Neustart](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Nächste Schritte

* [ASP.NET-Web-App-Schnellstart](cache-web-app-howto.md): Erstellen Sie eine einfache ASP.NET-Web-App, die Azure Cache for Redis verwendet.
* [.NET-Schnellstart](cache-dotnet-how-to-use-azure-redis-cache.md): Erstellen Sie eine .NET-App, die Azure Cache for Redis verwendet.
* [.NET Core-Schnellstart](cache-dotnet-core-quickstart.md): Erstellen Sie eine .NET Core-App, die Azure Cache for Redis verwendet.
* [Node.js-Schnellstart](cache-nodejs-get-started.md): Erstellen Sie eine einfache Node.js-App, die Azure Cache for Redis verwendet.
* [Java-Schnellstart](cache-java-get-started.md): Erstellen Sie eine einfache Java-App, die Azure Cache for Redis verwendet.
* [Python-Schnellstart](cache-python-get-started.md): Erstellen Sie eine Python-App, die Azure Cache for Redis verwendet.
