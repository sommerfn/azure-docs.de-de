---
title: Einführen in die integrierte operative Analyse in Azure Cosmos DB mit Apache Spark
description: Hier erfahren Sie, wie Sie die integrierte Unterstützung für Azure Spark in Azure Cosmos DB zum Ausführen von operativer Analyse und KI verwenden können.
ms.service: cosmos-db
ms.topic: overview
ms.date: 09/26/2019
author: markjbrown
ms.author: mjbrown
ms.openlocfilehash: 45f70d9a9d2cb450cab1242a080077c771218ba2
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754841"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark"></a>Integrierte operative Analyse in Azure Cosmos DB mit Apache Spark

Mit Azure Cosmos DB können Sie global verteilte Analysen mit geringer Latenz und KI auf Transaktionsdaten ausführen. Mit der nativen Unterstützung für Apache Spark und Jupyter-Notebooks kann Azure Cosmos DB dazu beitragen, dass Erkenntnisse schneller gewonnen werden. Dies liegt daran, dass die Daten erfasst, verarbeitet und anhand des lokalen Datenbankreplikats in einer Azure-Region analysiert werden. Sie können Apache Spark-Abfragen indizierter Daten aus mehreren Modellen, die innerhalb Ihrer Datenpartitionen gespeichert sind, direkt ausführen.

Die integrierte Unterstützung für Apache Spark in Azure Cosmos DB ermöglicht Ihnen das Ausführen von Analysen für Ihre in einem Azure Cosmos-Konto gespeicherten Daten aus Apache Spark. Sie ermöglicht die native Unterstützung für die direkte Ausführung von Apache Spark-Aufträgen in Ihren global verteilten Cosmos-Datenbanken. Mit diesen Funktionen können Entwickler, Data Engineers und Data Scientists Azure Cosmos DB als flexible, skalierbare und leistungsstarke Datenplattform zum Ausführen von **OLTP- und OLAP/HTAP**-Workloads verwenden.

Die Apache Spark-Unterstützung in Azure Cosmos DB bietet die folgenden Vorteile:

* Sie können die kürzeste Zeitspanne bis zur Erkenntnis für die geografisch verteilten Benutzer und Daten erreichen.

* Sie können die Architektur Ihrer Lösung vereinfachen und die [Gesamtbetriebskosten](total-cost-ownership.md) (Total Cost of Ownership, TCO) senken. Das System weist die kleinste Anzahl an Datenverarbeitungskomponenten auf und vermeidet jede unnötige Verschiebung von Daten zwischen ihnen.

* Es erstellt eine [Sicherheits-](secure-access-to-data.md), [Compliance-](compliance.md) und Überwachungsgrenze, die sämtliche unter Verwaltung stehenden Daten umfasst.

* Es bietet "Always-On"- oder [hoch verfügbare](high-availability.md) Endbenutzeranalysen, die von strengen SLAs gestützt werden.

![Apache Spark-Unterstützung in Azure Cosmos DB-Visualisierung](./media/spark-api-introduction/spark-api-visualization.png)
 
Mithilfe der Apache Spark-Unterstützung in Azure Cosmos DB können Sie Lösungen wie KI- und Deep Learning-Modelle, Predictive Analytics, Empfehlungen, IoT, Customer 360, Betrugserkennung, Textstimmungs- und Clickstreamanalyse erstellen und bereitstellen. Diese Lösungen arbeiten direkt mit Ihren Azure Cosmos DB-Daten.

Sie können Batch- und Streaming-ETL-Aufträge in Azure Cosmos DB einrichten, ohne den Datenbankdienst verlassen oder zusätzliche Computedienste hinzufügen zu müssen. Sie können die Compute-Umgebung flexibel skalieren, wenn Sie einen ETL-Auftrag ausführen müssen, und die Skalierung wieder zurückfahren, wenn der Auftrag erledigt ist.

Die Apache Spark-Unterstützung in Azure Cosmos DB bietet integrierte Machine Learning-Unterstützung in den Apache Spark-Runtimes. Die Runtime enthält Spark MLLib, Microsoft Machine Learning für Spark, Azure Machine Learning und Cognitive Services. Mit diesen Funktionen können Data Scientists, Data Engineers und Datenanalysten Modelle für maschinelles Lernen direkt innerhalb von Cosmos DB erstellen und betreiben, in einem Bruchteil der üblichen Zeit und zu geringen Kosten.


## <a name="key-benefits"></a>Hauptvorteile

### <a name="low-latency-operational-analytics-and-ai"></a>Operative Analyse mit geringer Latenz und KI

Mit Apache Spark in der global verteilten Azure Cosmos-Datenbank erhalten Sie jetzt weltweit mit geringer Wartezeit Erkenntnisse. Azure Cosmos DB ermöglicht **global verteilte operative Analyse mit geringer Latenz** und flexibler Skalierung dank dreier Schlüsseltechnologien:

* Da Ihre Azure Cosmos-Datenbank global verteilt ist, werden alle Daten lokal am Standort der Produzenten der Daten (beispielsweise Benutzer) erfasst. Die Abfragen werden anhand der lokalen Replikate verarbeitet, die sowohl für die Produzenten als auch für die Konsumenten der Daten am nächsten gelegen sind, unabhängig von ihrem Speicherort auf der Welt.

* Ihre sämtlichen Analyseabfragen werden direkt auf den indizierten Daten ausgeführt, die innerhalb der Datenpartitionen gespeichert sind, ohne unnötige Datenverschiebungen.

* Da Spark am gleichen Ort wie Azure Cosmos DB ausgeführt wird, sind weniger Zwischenübersetzungen und Datenverschiebungen erforderlich, was zu besserer Leistung und Skalierbarkeit führt.

* Alle Azure Cosmos DB-Kernfeatures wie Multimaster, automatisches Failover, Verfügbarkeitszonen usw. sind verfügbar für das integrierte Apache Spark in Azure Cosmos DB.

### <a name="unified-serverless-experience-for-apache-spark"></a>Einheitliche serverlose Benutzererfahrung für Apache Spark

Als mehrere Modelle unterstützende Datenbank dehnt Azure Cosmos DB jetzt seine Unterstützung für OSS-APIs aus, indem es eine **einheitliche serverlose Benutzererfahrung für Apache Spark** mit Datenmodellen der Familien Schlüssel-Wert, Dokument, Diagramm und Spalte zur Verfügung stellt. Mithilfe von APIs für MongoDB, Cassandra, Gremlin, Etcd und SQL werden verschiedene Datenmodelle unterstützt – die alle mit den gleichen zugrundeliegenden Daten arbeiten. 

Mit der Apache Spark-Unterstützung in Azure Cosmos DB können Sie Anwendungen, die in Scala, Python und Java geschrieben wurden, nativ unterstützen und verschiedene hoch integrierte Bibliotheken für SQL verwenden. Zu diesen Bibliotheken gehören ([Spark SQL](https://spark.apache.org/sql/)), maschinelles Lernen (Spark [MLlib](https://spark.apache.org/mllib/)), Datenstromverarbeitung ([Spark Structured Streaming](https://spark.apache.org/streaming/)) und Graphverarbeitung (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Diese Tools erleichtern die Nutzung von Apache Spark für eine Vielzahl von Anwendungsfällen. Sie brauchen sich nicht mit der Verwaltung von Spark oder Spark-Clustern auseinanderzusetzen. Sie können die vertrauten Apache Spark-APIs und **Jupyter-Notebooks** für die Analyse und die SQL-API oder eine der OSS NoSQL-APIs wie Cassandra zur gleichzeitigen Transaktionsverarbeitung mit den gleichen zugrundeliegenden Daten verwenden.

### <a name="no-schema-or-index-management"></a>Keine Schema- oder Indexverwaltung

Im Gegensatz zu herkömmlichen Analysedatenbanken brauchen sich bei Azure Cosmos DB Data Engineers und Data Scientists nicht mehr mit der lästigen Schema- und Indexverwaltung abzugeben. Die Datenbank-Engine in Azure Cosmos DB erfordert keine explizite Schema- oder Indexverwaltung und kann automatisch alle von ihr erfassten Daten indizieren, um die Apache Spark-Abfragen schnell zu bedienen.

### <a name="consistency-choices"></a>Konsistenzoptionen

Da die Apache Spark-Aufträge in den Datenpartitionen Ihrer Azure Cosmos-Datenbank ausgeführt werden, stehen für die Abfragen die [fünf klar definierte Konsistenzoptionen](consistency-levels.md) zur Verfügung. Diese Konsistenzmodelle verleihen die Flexibilität, strenge Konsistenz für Algorithmen für maschinelles Lernen auszuwählen, um die genauesten Ergebnisse zu erzielen, ohne Latenz und Hochverfügbarkeit zu beeinträchtigen.

### <a name="comprehensive-slas"></a>Umfassende SLAs

Die Apache Spark-Aufträge profitieren von den Azure Cosmos DB-Vorteilen, wie den branchenführenden umfassenden [SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) ohne jeglichen Mehraufwand für die Verwaltung separater Apache Spark-Cluster. Diese SLAs umfassen Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit. 

### <a name="mixed-operational-and-analytical-workloads-with-complete-isolation"></a>Gemischte operative und analytische Workloads mit kompletter Isolation

Durch die Integration von Apache Spark in Azure Cosmos DB wird die Trennung zwischen Transaktion und Analyse überbrückt, die für Kunden einen der größten neuralgischen Punkte beim Erstellen cloudnativer Anwendungen im globalen Maßstab darstellte. Die OLTP-und OLAP-Workloads werden parallel ausgeführt und beeinträchtigen einander nicht.

### <a name="low-latency-analytical-and-transactional-storage"></a>Analytische und transaktionale Speicherung mit geringer Latenz

Azure Cosmos DB speichert Daten nativ auf zwei voneinander abgegrenzten Speicherebenen: im transaktionalen Speicher (zeilenorientiert) und im analytischen Speicher (spaltenorientiert, im Apache Parquet-Dateiformat). Die Daten werden auf jeder Ebene global repliziert, und Benutzer können Daten auf diesen Ebenen entsprechend den Aufbewahrungsrichtlinien unabhängig verwalten.

Diese Speicherarchitektur ermöglicht sowohl unternehmenskritische transaktionale als auch analytische Workloads für dieselben global verteilten Daten. Mit Cosmos DB sind keine ETL-Vorgänge erforderlich, und es müssen keine unnötigen Datenverschiebungen ausgeführt werden. Sie können einfach transaktionale und analytische Workloads für dieselben zugrunde liegenden Daten ausführen. Bei den transaktionalen Workloads können die vertrauten APIs für transaktionalen Zugriff verwendet werden, einschließlich von SQL, Cassandra, MongoDB, Gremlin und etcd. Die Analyse- und KI-Workloads hingegen können die integrierten Apache Spark SQL- und ML-Frameworks und die gesamte Apache Spark-Toolkette verwenden.

### <a name="snapshots-and-historical-analytical-queries"></a>Momentaufnahmen und historische analytische Abfragen

Sie können bedarfsgesteuerte oder geplante Momentaufnahmen der auf der analytischen Ebene gespeicherten komprimierten Spaltendaten erstellen, um direkt analytische Abfragen für eine bestimmte Momentaufnahme auszuführen. Dieses Feature ermöglicht analytische Rückblende- oder Zeitreise-Abfragen, Rollbacks, vollständige historische Audit-Trails sowie reproduzierbare Machine Learning- und KI-Experimente.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Vorzügen von Azure Cosmos DB finden Sie im [Übersichtsartikel](introduction.md).
* [Erste Schritte mit der Azure Cosmos DB-API für MongoDB](mongodb-introduction.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](cassandra-introduction.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](graph-introduction.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](table-introduction.md)