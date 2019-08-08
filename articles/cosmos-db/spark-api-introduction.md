---
title: Einführen in die integrierte operative Analyse in Azure Cosmos DB mit Apache Spark
description: Hier erfahren Sie, wie Sie die integrierte Unterstützung für Azure Spark in Azure Cosmos DB zum Ausführen von operativer Analyse und KI verwenden können.
ms.service: cosmos-db
ms.topic: overview
ms.date: 08/01/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: 3fd7990043543b92eb3daf8ec699ec58db3387ec
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717568"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark-preview"></a>Integrierte operative Analyse in Azure Cosmos DB mit Apache Spark (Vorschauversion) 

Die integrierte Unterstützung für Apache Spark in Azure Cosmos DB ermöglicht Ihnen das Ausführen von Analysen für Ihre in einem Azure Cosmos-Konto gespeicherten Daten aus Apache Spark. Sie ermöglicht die native Unterstützung für die direkte Ausführung von Apache Spark-Aufträgen in Ihren global verteilten Cosmos-Datenbanken. Mit diesen Funktionen können Entwickler, Data Engineers und Data Scientists Azure Cosmos DB als flexible, skalierbare und leistungsstarke Datenplattform zum Ausführen von **OLTP- und OLAP/HTAP**-Workloads verwenden. 

Spark-Compute ist automatisch in allen Azure-Regionen verfügbar, die mit Ihrem Azure Cosmos-Konto verknüpft sind. Spark-Aufträge nutzen die Multimasterfunktion von Azure Cosmos DB und können in lokalen Replikaten in jeder Region schreiben und Abfragen ausführen. 

> [!NOTE]
> Die integrierte Unterstützung für Apache Spark in Azure Cosmos DB ist derzeit als eingeschränkte Vorschauversion verfügbar. Um sich für die Vorschauversion zu registrieren, navigieren Sie zur Seite [Für die Vorschau registrieren](https://portal.azure.com/?feature.customportal=false#create/Microsoft.DocumentDB). 

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

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Global verteilte operative Analyse und KI mit geringer Latenz

Mit Apache Spark in der global verteilten Azure Cosmos-Datenbank erhalten Sie jetzt weltweit mit geringer Wartezeit Erkenntnisse. Azure Cosmos DB ermöglicht **global verteilte operative Analyse mit geringer Latenz** und flexibler Skalierung dank dreier Schlüsseltechnologien:

* Da Ihre Azure Cosmos-Datenbank global verteilt ist, werden alle Daten lokal am Standort der Produzenten der Daten (beispielsweise Benutzer) erfasst. Die Abfragen werden anhand der lokalen Replikate verarbeitet, die sowohl für die Produzenten als auch für die Konsumenten der Daten am nächsten gelegen sind, unabhängig von ihrem Speicherort auf der Welt. 

* Ihre sämtlichen Analyseabfragen werden direkt auf den indizierten Daten ausgeführt, die innerhalb der Datenpartitionen gespeichert sind, ohne unnötige Datenverschiebungen. 

* Da Spark am gleichen Ort wie Azure Cosmos DB ausgeführt wird, sind weniger Zwischenübersetzungen und Datenverschiebungen erforderlich, was zu besserer Leistung und Skalierbarkeit führt.

### <a name="unified-serverless-experience-for-apache-spark"></a>Einheitliche serverlose Benutzererfahrung für Apache Spark

Als mehrere Modelle unterstützende Datenbank dehnt Azure Cosmos DB jetzt seine Unterstützung für OSS-APIs aus, indem es eine **einheitliche serverlose Benutzererfahrung für Apache Spark** mit Datenmodellen der Familien Schlüssel-Wert, Dokument, Diagramm und Spalte zur Verfügung stellt. Mithilfe von APIs für MongoDB, Cassandra, Gremlin, Etcd und SQL werden verschiedene Datenmodelle unterstützt – die alle mit den gleichen zugrundeliegenden Daten arbeiten. 

Mit der Apache Spark-Unterstützung in Azure Cosmos DB können Sie Anwendungen, die in Scala, Python und Java geschrieben wurden, nativ unterstützen und verschiedene hoch integrierte Bibliotheken für SQL verwenden. Zu diesen Bibliotheken gehören ([Spark SQL](https://spark.apache.org/sql/)), maschinelles Lernen (Spark [MLlib](https://spark.apache.org/mllib/)), Datenstromverarbeitung ([Spark Structured Streaming](https://spark.apache.org/streaming/)) und Graphverarbeitung (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Diese Tools erleichtern die Nutzung von Apache Spark für eine Vielzahl von Anwendungsfällen. Sie brauchen sich nicht mit der Verwaltung von Spark oder Spark-Clustern auseinanderzusetzen. Sie können die vertrauten Apache Spark-APIs und **Jupyter-Notebooks** für die Analyse und die SQL-API oder eine der OSS NoSQL-APIs wie Cassandra zur gleichzeitigen Transaktionsverarbeitung mit den gleichen zugrundeliegenden Daten verwenden.

### <a name="no-schema-or-index-management"></a>Keine Schema- oder Indexverwaltung

Im Gegensatz zu herkömmlichen Analysedatenbanken brauchen sich bei Azure Cosmos DB Data Engineers und Data Scientists nicht mehr mit der lästigen Schema- und Indexverwaltung abzugeben. Die Datenbank-Engine in Azure Cosmos DB erfordert keine explizite Schema- oder Indexverwaltung und kann automatisch alle von ihr erfassten Daten indizieren, um die Apache Spark-Abfragen schnell zu bedienen. 

### <a name="consistency-choices"></a>Konsistenzoptionen

Da die Apache Spark-Aufträge in den Datenpartitionen Ihrer Azure Cosmos-Datenbank ausgeführt werden, stehen für die Abfragen die [fünf klar definierte Konsistenzoptionen](consistency-levels.md) zur Verfügung. Diese Konsistenzmodelle verleihen die Flexibilität, strenge Konsistenz für Algorithmen für maschinelles Lernen auszuwählen, um die genauesten Ergebnisse zu erzielen, ohne Latenz und Hochverfügbarkeit zu beeinträchtigen. 

### <a name="comprehensive-slas"></a>Umfassende SLAs

Die Apache Spark-Aufträge profitieren von den Azure Cosmos DB-Vorteilen, wie den branchenführenden umfassenden [SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) ohne jeglichen Mehraufwand für die Verwaltung separater Apache Spark-Cluster. Diese SLAs umfassen Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit. 

### <a name="mixed-workloads"></a>Gemischte Workloads

Durch die Integration von Apache Spark in Azure Cosmos DB wird die Trennung zwischen Transaktion und Analyse überbrückt, die für Kunden einen der größten neuralgischen Punkte beim Erstellen cloudnativer Anwendungen im globalen Maßstab darstellte. 

## <a name="scenarios-for-azure-cosmos-db-spark-support"></a>Szenarien für die Spark-Unterstützung von Azure Cosmos DB

### <a name="retail-and-consumer-goods"></a>Einzelhandel und Konsumgüter

Sie können die Spark-Unterstützung in Azure Cosmos DB nutzen, um Empfehlungen und Angebote in Echtzeit bereitzustellen. Mit der Personalisierung in Echtzeit und den Produktempfehlungen können Sie Ihren Kunden helfen, die erforderlichen Artikel zu finden.

* Sie können die integrierte Machine Learning-Unterstützung der Apache Spark-Runtime verwenden, um Empfehlungen für alle Produktkataloge in Echtzeit zu generieren.

* Sie können Clickstreamdaten, Kaufdaten und Kundendaten analysieren, um gezielte Empfehlungen zu geben, die den Lebensdauerwert erhöhen.

* Mit dem globalen Verteilungsfeature von Azure Cosmos DB können große Mengen an Produktdaten, die über Regionen verteilt sind, in Millisekunden analysiert werden.

* Sie können schnell Erkenntnisse zu den geografisch verteilten Benutzern und Daten gewinnen. Sie können die Konvertierungsrate für Aktionen verbessern, indem Sie die richtige Anzeige zur richtigen Zeit für den richtigen Benutzer bereitstellen.

* Sie können die integrierte Spark-Streamingfunktion nutzen, um Livedaten aufzuwerten, indem Sie sie mit statischen Kundendaten kombinieren. Auf diese Weise können Sie personalisiertere und gezieltere Anzeigen in Echtzeit und in Bezug auf die Tätigkeit des Kunden bereitstellen.

Die folgende Abbildung zeigt, wie die Spark-Unterstützung von Azure Cosmos DB zur Optimierung von Preisen und Werbeaktionen verwendet wird:

![Spark-Unterstützung von Azure Cosmos DB zur Optimierung von Preisen und Werbeaktionen](./media/spark-api-introduction/optimize-pricing-and-promotions.png)


Die folgende Abbildung zeigt, wie die Spark-Unterstützung von Azure Cosmos DB in der Empfehlungs-Engine in Echtzeit verwendet wird:

![Spark-Unterstützung von Azure Cosmos DB in der Empfehlungs-Engine in Echtzeit](./media/spark-api-introduction/real-time-recommendation-engine.png)

### <a name="manufacturing-and-iot"></a>Fertigung und IoT

Die integrierte Analyseplattform von Azure Cosmos DB ermöglicht Ihnen die Echtzeitanalyse von IoT-Daten von Millionen von Geräten auf globaler Ebene. Sie können moderne Innovationen wie die Vorhersage von Wetterverhältnissen, Vorhersageanalysen und Energieoptimierungen durchführen.

* Mithilfe von Azure Cosmos DB können Sie Daten wie Echtzeit-Ressourcenmetriken und Wetterfaktoren analysieren und dann Smart Grid-Analysen anwenden, um die Leistung der angeschlossenen Geräte in der Praxis zu optimieren. Die Smart Grid-Analysen sind der Schlüssel zur Kontrolle der Betriebskosten, zur Verbesserung der Netzsicherheit und zur Bereitstellung personalisierter Energiedienstleistungen für Verbraucher.

Die folgende Abbildung zeigt, wie die Spark-Unterstützung von Azure Cosmos DB verwendet wird, um Metriken von IoT-Geräten zu lesen und Smart Grid-Analysen anzuwenden:

![Spark-Unterstützung von Azure Cosmos DB zum Lesen von Metriken von IoT-Geräten](./media/spark-api-introduction/read-metrics-from-iot-devices.png)


### <a name="predictive-maintenance"></a>Predictive Maintenance

* Die Instandhaltung von Ressourcen wie Kompressoren, die in kleinen Bohranlagen auf Hochseeplattformen eingesetzt werden, ist ein komplexes Unterfangen. Diese Ressourcen sind weltweit verteilt und generieren Petabytes an Daten. Mithilfe von Azure Cosmos DB können Sie eine End-to-End-Datenpipeline für die Vorhersage aufbauen, die Spark-Streaming verwendet, um große Mengen an Sensortelemetrie zu verarbeiten, Ressourcenteile zu lagern und Sensorzuordnungsdaten zu speichern.

* Sie können Machine Learning-Modelle erstellen und bereitstellen, um Ressourcenausfälle vor dem Auftreten vorherzusagen und Wartungsaufträge zu erteilen, bevor der Fehler auftritt.

In der folgenden Abbildung wird gezeigt, wie die Spark-Unterstützung von Azure Cosmos DB zum Erstellen eines prädiktiven Wartungssystems verwendet wird:

![Spark-Unterstützung von Azure Cosmos DB zum Erstellen eines prädiktiven Wartungssystems](./media/spark-api-introduction/predictive-maintenance-system.png)

In der folgenden Abbildung wird gezeigt, wie die Spark-Unterstützung von Azure Cosmos DB zum Erstellen eines Echtzeit-Fahrzeugdiagnosesystems verwendet wird:

![Spark-Unterstützung von Azure Cosmos DB zum Erstellen eines Echtzeit-Fahrzeugdiagnosesystems](./media/spark-api-introduction/real-time-vehicle-diagnostic-system.png)

### <a name="gaming"></a>Spiele

* Mit der integrierten Spark-Unterstützung ermöglicht Ihnen Azure Cosmos DB die einfache Erstellung, Skalierung und Bereitstellung fortschrittlicher Analyse- und Machine Learning-Modelle in wenigen Minuten, um das bestmögliche Spieleerlebnis zu schaffen.

* Sie können Spieler-, Kauf- und Verhaltensdaten analysieren, um relevante personalisierte Angebote zu erstellen, um hohe Konvertierungsraten zu erzielen.

* Mit Spark Machine Learning können Sie Telemetriedaten zu Spielen analysieren und entsprechende Erkenntnisse gewinnen. Sie können langsame Ladezeiten und Probleme im Spiel diagnostizieren und verhindern.

In der folgenden Abbildung wird gezeigt, wie die Spark-Unterstützung von Azure Cosmos DB für die Analyse von Spielen verwendet wird:

![Spark-Unterstützung von Azure Cosmos DB bei der Analyse von Spielen](./media/spark-api-introduction/gaming-analytics.png)

## <a name="built-in-jupyter-notebooks-support"></a>Integrierte Unterstützung von Jupyter Notebooks

Azure Cosmos DB unterstützt integrierte Jupyter Notebooks für alle APIs wie Cassandra, MongoDB, SQL, Gremlin und Tabellen. Die Jupyter Notebooks werden in Azure Cosmos-Konten ausgeführt und verbessern die Entwicklererfahrung. Die integrierte Unterstützung für Notebooks für alle Azure Cosmos DB-APIs und -Datenmodelle ermöglichen Ihnen die Ausführung interaktiver Abfragen. Sie können auch Machine Learning-Modelle ausführen und die in Ihren Azure Cosmos-Datenbanken gespeicherten Daten analysieren. Mithilfe von Jupyter Notebooks können Sie im Azure-Portal gespeicherte Daten analysieren, Machine Learning-Modelle erstellen und trainieren und Rückschlüsse auf die Daten ziehen, wie in der folgenden Abbildung gezeigt:

![Unterstützung von Jupyter Notebooks in Azure Cosmos DB](./media/spark-api-introduction/jupyter-notebooks-portal.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Vorzügen von Azure Cosmos DB finden Sie im [Übersichtsartikel](introduction.md).
* [Erste Schritte mit der Azure Cosmos DB-API für MongoDB](mongodb-introduction.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](cassandra-introduction.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](graph-introduction.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](table-introduction.md)




