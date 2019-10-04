---
title: Bewährte Methoden für Apache Spark in Azure HDInsight
description: Erhalten Sie Informationen zu bewährten Methoden für die Verwendung von Apache Spark in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 20033e52e862f086d1491c06d38cdf4f2c57ba8d
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106882"
---
# <a name="apache-spark-best-practices"></a>Bewährte Methoden für Apache Spark

Dieser Artikel enthält verschiedene bewährte Methoden für die Verwendung von Apache Spark für Azure HDInsight.

## <a name="how-do-i-run-or-submit-spark-jobs"></a>Wie kann ich Spark-Aufträge ausführen oder übermitteln?

| Option | Dokumente |
|---|---|
| VSCode | [Verwenden von Spark- und Hive-Tools für Visual Studio Code](../hdinsight-for-vscode.md) |
| Jupyter-Notebooks | [Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight](./apache-spark-load-data-run-query.md) |
| IntelliJ | [Tutorial: Erstellen von Apache Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ](./apache-spark-intellij-tool-plugin.md) |
| IntelliJ | [Tutorial: Erstellen einer Scala Maven-Anwendung für Apache Spark in HDInsight mithilfe von IntelliJ](./apache-spark-create-standalone-application.md) |
| Zeppelin-Notebooks | [Verwenden von Apache Zeppelin-Notebooks mit Apache Spark-Cluster in Azure HDInsight](./apache-spark-zeppelin-notebook.md) |
| Remoteauftragsübermittlung mit Livy | [Übermitteln von Remoteaufträgen an einen HDInsight Spark-Cluster mithilfe der Apache Spark-REST-API](./apache-spark-livy-rest-interface.md) |

## <a name="how-do-i-monitor-and-debug-spark-jobs"></a>Wie kann ich Spark-Aufträge überwachen und debuggen?

| Option | Dokumente |
|---|---|
| Azure Toolkit für IntelliJ | [Debuggen von fehlgeschlagenen Spark-Aufträgen mit dem Azure-Toolkit für IntelliJ (Vorschau)](apache-spark-intellij-tool-failure-debug.md) |
| Azure-Toolkit für IntelliJ über SSH | [Lokales oder Remotedebuggen von Apache Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ per SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md) |
| Azure-Toolkit für IntelliJ über VPN | [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Apache Spark-Anwendungen über VPN](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) |
| Auftragsdiagramm im Apache Spark-Verlaufsserver | [Verwenden des erweiterten Apache Spark-Verlaufsservers zum Debuggen und Diagnostizieren von Apache Spark-Anwendungen](./apache-azure-spark-history-server.md) |

## <a name="how-do-i-make-my-spark-jobs-run-more-efficiently"></a>Wie kann ich meine Spark-Aufträge effizienter gestalten?

| Option | Dokumente |
|---|---|
| E/A-Cache | [Verbessern der Leistung von Apache Spark-Workloads per Azure HDInsight IO Cache (Vorschauversion)](./apache-spark-improve-performance-iocache.md) |
| Konfigurationsoptionen | [Optimieren von Apache Spark-Aufträgen](./apache-spark-perf.md) |

## <a name="how-do-i-connect-to-other-azure-services"></a>Wie kann ich Verbindungen mit anderen Azure-Diensten herstellen?

| Option | Dokumente |
|---|---|
| Apache Hive in HDInsight | [Integrieren von Apache Spark und Apache Hive per Hive Warehouse Connector](../interactive-query/apache-hive-warehouse-connector.md) |
| Apache HBase in HDInsight | [Verwenden von Apache Spark zum Lesen und Schreiben von Apache HBase-Daten](../hdinsight-using-spark-query-hbase.md) |
| Apache Kafka in HDInsight | [Tutorial: Verwenden von strukturiertem Apache Spark-Streaming mit Apache Kafka in HDInsight](../hdinsight-apache-kafka-spark-structured-streaming.md) |
| Azure Cosmos DB | [Azure Cosmos DB: Implementieren einer Lambda-Architektur auf der Azure Platform](../../cosmos-db/lambda-architecture.md) |

## <a name="what-are-my-storage-options"></a>Welche Speicheroptionen stehen mir zur Verfügung?

| Option | Dokumente |
|---|---|
| Data Lake Storage Gen2 | [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-storage-gen2.md) |
| Data Lake Storage Gen1 | [Verwenden von Data Lake Storage Gen1 mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-data-lake-store.md) |
| Azure Blob Storage | [Verwenden von Azure Storage mit Azure HDInsight-Clustern](../hdinsight-hadoop-use-blob-storage.md) |

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren von Apache Spark-Einstellungen](apache-spark-settings.md)
* [Optimieren von Apache Spark-Aufträgen in HDInsight](apache-spark-perf.md)
