---
title: Troubleshooting bei Apache HBase-Leistungsproblemen in Azure HDInsight
description: In diesem Artikel finden Sie eine Reihe von Richtlinien für die Apache HBase-Leistungsoptimierung sowie Tipps zur Steigerung der Leistung in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.openlocfilehash: c67f21a6ed8a7697977bb7737f0e46348efb2530
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "71266503"
---
# <a name="troubleshoot-apache-hbase-performance-issues-on-azure-hdinsight"></a>Troubleshooting bei Apache HBase-Leistungsproblemen in Azure HDInsight

In diesem Artikel wird eine Reihe von Richtlinien für die Apache HBase-Leistungsoptimierung erläutert, und Sie erhalten Tipps zum Optimieren der Leistung in Azure HDInsight. Viele dieser Tipps sind abhängig von der jeweiligen Arbeitsauslastung und dem Lese-/Schreib-/Scan-Muster. Testen Sie die Konfigurationsänderungen gründlich, bevor Sie sie in einer Produktionsumgebung anwenden.

## <a name="hdinsight-hbase-performance-insights"></a>Einblicke in die HDInsight HBase-Leistung

Der größte Engpass in den meisten HBase-Workloads ist das Write-Ahead-Protokoll (WAL). Es hat erhebliche Auswirkungen auf die Schreibleistung. HDInsight HBase weist Modell mit Storage/Compute-Abgrenzung auf, d. h., Daten werden remote auf Azure Storage gespeichert, die Regionsserver werden jedoch auf den VMs gehostet. Bis vor Kurzem wurde auch das Write-Ahead-Protokoll in Azure Storage geschrieben, wodurch dieser Engpass für HDInsight verstärkt wird. Dieses Problem wird durch das Feature [Accelerated Writes](./apache-hbase-accelerated-writes.md) (Beschleunigte Schreibvorgänge) behoben. Dabei wird das Write-Ahead-Protokoll in verwaltete SSD Premium-Datenträger von Azure geschrieben. Dies verbessert die Schreibleistung erheblich und behebt eine Vielzahl von Problemen im Zusammenhang mit einigen schreibintensiven Workloads.

Verwenden Sie das [Premium-Blockblob-Speicherkonto](https://azure.microsoft.com/blog/azure-premium-block-blob-storage-is-now-generally-available/) als Remotespeicher, um wesentliche Verbesserungen bei Schreibvorgängen zu erzielen. Diese Option kann nur verwendet werden, wenn das Feature Write-Ahead-Protokoll aktiviert ist.

## <a name="compaction"></a>Komprimierung

Komprimierung ist nach allgemeiner Auffassung in der Community ein weiterer potenzieller Engpass.  Standardmäßig ist die umfassende Komprimierung in HDInsight HBase-Clustern deaktiviert. Dies liegt daran, dass es sich um einen ressourcenintensiven Prozess handelt und wir Kunden uneingeschränkte Flexibilität bieten möchten, diesen entsprechend ihren Workload-Merkmalen zu planen (d. h. außerhalb der Spitzenzeiten). Da unser Speicher remote ist (gestützt auf Azure Storage) und nicht auf dem lokalen HDFS (Hadoop Distributed File System) aufbaut, was in den meisten lokalen Instanzen üblich ist, spielt der Ort der Daten keine Rolle – eines der Hauptziele der umfassenden Komprimierung.

Es wird davon ausgegangen, dass der Kunde umfassende Komprimierungsvorgänge entsprechend den jeweiligen Anforderungen selbst plant. Wird eine derartige Pflege unterlassen, kann die Komprimierung die Leseleistung langfristig erheblich beeinträchtigen.

Insbesondere bei Scanvorgängen können mittlere Wartezeiten von weit mehr als 100 ms Grund zur Besorgnis sein. Überprüfen Sie, ob umfassende Komprimierungsvorgänge ordnungsgemäß geplant wurden.

## <a name="know-your-apache-phoenix-workload"></a>Informieren Sie sich über Ihre Apache Phoenix-Workload

Durch die Beantwortung der folgenden Fragen können Sie Ihre Apache Phoenix-Workload besser verstehen:

* Werden alle Ihre „Lesevorgänge“ in Scans übersetzt?
    * Falls ja, welche Merkmale haben diese Scans?
    * Haben Sie Ihr Phoenix-Tabellenschema für diese Scans optimiert, einschließlich der entsprechenden Indizierung?
* Haben Sie mit der `EXPLAIN`-Anweisung die Abfragepläne erkundet, die von Ihren Lesevorgängen generiert werden?
* Sind Ihre Schreibvorgänge „upsert-select“-Ausführungen?
    * Wenn dies der Fall ist, führen sie ebenfalls Scans aus. Als Latenz für Scans wird durchschnittlich 100 ms erwartet, im Gegensatz zu 10 ms für Point Gets in HBase.  

## <a name="test-methodology-and-metrics-monitoring"></a>Testmethoden und Überwachen von Metriken

Wenn Sie die Leistung anhand von Benchmarks wie YCSB, JMeter oder Pherf testen und optimieren, stellen Sie Folgendes sicher:

1. Die Clientcomputer werden zu keinem Engpass (CPU-Auslastung auf Clientcomputern prüfen).

1. Clientseitige Konfigurationen (wie z. B. die Anzahl der Threads usw.) werden entsprechend angepasst, um die Clientbandbreite komplett zu nutzen.

1. Testergebnisse werden genau und systematisch aufgezeichnet.

Wenn Ihre Abfragen plötzlich viel schlechter als früher ausgeführt werden, suchen Sie nach möglichen Fehlern im Anwendungscode: Werden plötzlich große Mengen von ungültigen Daten generiert, was natürlich die Leselatenz erhöht?

## <a name="migration-issues"></a>Migrationsprobleme

Stellen Sie bei der Migration zu Azure HDInsight sicher, dass sie systematisch und genau abgewickelt wird, vorzugsweise über Automatisierung. Vermeiden Sie eine manuelle Migration. Stellen Sie Folgendes sicher:

1. Tabellenattribute wie Komprimierung, Bloom-Filter usw. sind genau zu migrieren.

1. Bei Phoenix-Tabellen sind die Salting-Einstellungen ordnungsgemäß der neuen Clustergröße zuzuordnen. Es wird empfohlen, dass die Anzahl der Salt-Buckets einem Vielfachen der Anzahl von Workerknoten im Cluster entspricht, wobei das jeweilige Vielfache von der Menge beobachteten Hotspots abhängig ist.  

## <a name="server-side-config-tunings"></a>Serverseitige Konfigurationsoptimierung

In HDInsight HBase werden HFiles im Remotespeicher gespeichert. Bei einem Cacheausfall sind die Kosten für Lesevorgänge daher definitiv höher als in lokalen System, in denen dank der Netzwerklatenz Daten vom lokalen HDFS (Hadoop Distributed File System) gesichert werden. In den meisten Szenarien wird dieses Problem durch die intelligente Nutzung von HBase-Caches (Blockcache und Bucketcache) umgangen. Gelegentlich kann dies aber auch für den Kunden ein Problem darstellen. Die Verwendung eines Premium-Blockblobkontos hat hier etwas Abhilfe geschaffen. Mit dem WASB-Blob (Windows Azure Storage Driver), der sich beim Abrufen von Daten in Blocks auf bestimmte Eigenschaften wie `fs.azure.read.request.size` stützt, je nach seiner Feststellung des Lesemodus (sequenziell oder zufällig), sind auch künftig höhere Latenzen bei Lesevorgängen nicht auszuschließen. In empirischen Experimenten wurde festgestellt, dass durch Festlegen der Leseanforderungsblockgröße (`fs.azure.read.request.size`) auf 512 KB und Festlegen der Blockgröße der HBase-Tabellen auf denselben Wert in der Praxis das beste Ergebnis erhalten wird.

HDInsight HBase stellt für die meisten Cluster mit großen Knoten `bucketcache` als Datei auf dem lokalen SSD bereit, die mit der VM verbunden ist, auf der `regionservers` ausgeführt wird. Gelegentlich kann mit Verwendung eines Off-Heap-Cache eine gewisse Verbesserung erzielt werden. Dabei gilt jedoch die Beschränkung, dass der verfügbare Speicher verwendet wird und er potenziell kleiner als ein dateibasierter Cache ist, sodass dies offensichtlich nicht immer die beste Wahl ist.

Wir haben einige andere spezifische Parameter optimiert, was anscheinend in unterschiedlichem Maß zu einer Verbesserung beigetragen hat:

1. Erhöhen der Größe von `memstore` von den standardmäßigen 128 MB auf 256 MB – diese Einstellung empfiehlt sich typischerweise für schreibintensive Szenarien.

1. Erhöhen der Anzahl der für die Komprimierung reservierten Threads vom Standardwert 1 auf 4. Diese Einstellung ist relevant, wenn häufig geringfügige Komprimierungsvorgänge ausgeführt werden.

1. Die `memstore`-Leerung sollte nicht aufgrund von Speicherlimits blockiert werden. `Hbase.hstore.blockingStoreFiles` kann auf 100 erhöht werden, um einen solchen Puffer zu erhalten.

1. Zum Steuern von Leerungen können die Standardwerte wie folgt angepasst werden:

    1. `Hbase.regionserver.maxlogs` kann von 32 (Vermeiden von Leerungen aufgrund der WAL-Limits) auf 140 heraufgesetzt werden.

    1. `Hbase.regionserver.global.memstore.lowerLimit` = 0,55.

    1. `Hbase.regionserver.global.memstore.upperLimit` = 0,60.

1. Phoenix-spezifische Konfigurationen für die Optimierung von Threadpools:

    1. `Phoenix.query.queuesize` kann auf 10.000 erhöht werden.

    1. `Phoenix.query.threadpoolsize` kann auf 512 erhöht werden.

1. Weitere Phoenix-spezifische Konfigurationen:

    1. `Phoenix.rpc.index.handler.count` kann auf 50 festgelegt werden, wenn umfassende oder viele Indexsuchvorgänge ausgeführt werden.

    1. `Phoenix.stats.updateFrequency` kann vom Standardwert von 15 Minuten auf 1 Stunde heraufgesetzt werden.

    1. `Phoenix.coprocessor.maxmetadatacachetimetolivems` kann von 30 Minuten auf 1 Stunde heraufgesetzt werden.

    1. `Phoenix.coprocessor.maxmetadatacachesize` kann von 20 MB auf 50 MB heraufgesetzt werden.

1. RPC-Zeitlimits: HBase-RPC-Zeitlimit, HBase-Client-Scannerzeitlimit und Phoenix-Abfragezeitlimit können auf 3 Minuten vergrößert werden. Hierbei ist unbedingt zu beachten, dass der `hbase.client.scanner.caching`-Parameter auf einen Wert festgelegt wird, der auf Server- und Clientseite übereinstimmt. Andernfalls bewirkt diese Einstellung Fehler in Bezug auf `OutOfOrderScannerException` auf der Clientseite. Diese Einstellung sollte für umfassende Scans auf einen niedrigen Wert festgelegt werden. Wir legen diesen Wert auf 100 fest.

## <a name="other-considerations"></a>Weitere Überlegungen

Einige weitere Parameter, die bei der Optimierung berücksichtigt werden sollten:

1. `Hbase.rs.cacheblocksonwrite`: Diese Einstellung ist für HDI standardmäßig auf „true“ festgelegt.

1. Einstellungen, mit denen weniger umfassende Komprimierungsvorgänge auf einen späteren Zeitpunkt verschoben werden kann.

1. Experimentelle Einstellungen, z. B. das Anpassen des Prozentsatzes von Warteschlangen, der für Lese-und Schreibanforderungen reserviert ist.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

- Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

- Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
