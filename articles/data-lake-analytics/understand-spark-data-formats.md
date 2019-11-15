---
title: Grundlegendes zu Apache Spark-Datenformaten für Azure Data Lake Analytics-U-SQL-Entwickler.
description: In diesem Artikel werden Apache Spark-Konzepte beschrieben, damit U-SQL-Entwickler die Unterschiede zwischen U-SQL- und Spark-Datenformaten verstehen.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: understand-apache-spark-data-formats
ms.date: 01/31/2019
ms.openlocfilehash: 36f39503ca32f1ee4b422ae7b1cf9abf48716f07
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648167"
---
# <a name="understand-differences-between-u-sql-and-spark-data-formats"></a>Grundlegendes zu den Unterschieden zwischen U-SQL- und Spark-Datenformaten

Wenn Sie [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) oder [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) verwenden möchten, empfiehlt es sich, Ihre Daten von [Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md) zu [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) zu migrieren.

Zusätzlich zum Verschieben der Dateien sollten Sie die in U-SQL-Tabellen gespeicherten Daten für Spark verfügbar machen.

## <a name="move-data-stored-in-azure-data-lake-storage-gen1-files"></a>Verschieben von in Azure Data Lake Storage Gen1-Dateien gespeicherten Dateien

In Dateien gespeicherte Daten können auf verschiedene Weise verschoben werden:

- Schreiben Sie eine [Azure Data Factory](../data-factory/introduction.md)-Pipeline, um die Daten aus dem [Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md)-Konto in das [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)-Konto zu kopieren.
- Schreiben Sie einen Spark-Auftrag, mit dem die Daten aus dem [Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md)-Konto gelesen und dann in das [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)-Konto geschrieben werden. Je nach Anwendungsfall können Sie ein anderes Format verwenden, z. B. Parquet, wenn das ursprüngliche Dateiformat nicht beibehalten werden muss.

Es empfiehlt sich, den Artikel [Upgrade von Big Data-Analyselösungen von Azure Data Lake Storage Gen1 auf Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md) zu lesen.

## <a name="move-data-stored-in-u-sql-tables"></a>Verschieben von in U-SQL-Tabellen gespeicherten Daten

U-SQL-Tabellen können in Spark nicht interpretiert werden. Wenn Sie Daten in U-SQL-Tabellen gespeichert haben, führen Sie einen U-SQL-Auftrag aus, mit dem die Tabellendaten extrahiert und in einem Format gespeichert werden, das in Spark erkannt wird. Am besten eignet sich das Erstellen einer Gruppe von Parquet-Dateien entsprechend dem Ordnerlayout des Hive-Metastores.

Die Ausgabe kann in U-SQL mit dem integrierten Parquet-Outputter und mithilfe der dynamischen Ausgabepartitionierung mit Dateigruppen zum Erstellen der Partitionsordner erreicht werden. Unter [Process more files than ever and use Parquet](https://blogs.msdn.microsoft.com/azuredatalake/2018/06/11/process-more-files-than-ever-and-use-parquet-with-azure-data-lake-analytics) (Verarbeiten größerer Datenmengen durch Verwendung von Parquet) finden Sie ein Beispiel zum Erstellen dieser in Spark nutzbaren Daten.

Nach dieser Transformation kopieren Sie die Daten entsprechend der Beschreibung im Abschnitt [Verschieben von in Azure Data Lake Storage Gen1-Dateien gespeicherten Dateien](#move-data-stored-in-azure-data-lake-storage-gen1-files).

## <a name="caveats"></a>Einschränkungen

- Datensemantik: Beim Kopieren von Dateien erfolgt der Kopiervorgang auf Byteebene. Daher sollten die gleichen Daten im [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)-Konto angezeigt werden. Beachten Sie jedoch, dass einige Zeichen in Spark unterschiedlich interpretiert werden können. Beispielsweise kann in einer CSV-Datei ein anderer Standardwert für ein Zeilentrennzeichen verwendet werden.
    Wenn Sie typisierte Daten (aus Tabellen) kopieren, können in Parquet und Spark außerdem möglicherweise eine unterschiedliche Genauigkeit und Anzahl der Dezimalstellen für einige der typisierten Werte festgelegt sein (z. B. ein Gleitkommawert) und NULL-Werte unterschiedlich verarbeitet werden. In U-SQL wird z. B. die C#-Semantik für NULL-Werte verwendet, in Spark dagegen eine dreiwertige Logik.

- Datenorganisation (Partitionierung): U-SQL-Tabellen umfassen eine zweistufige Partitionierung. Die äußere Ebene (`PARTITIONED BY`) ist nach Wert definiert und wird größtenteils dem Hive/Spark-Partitionsschema zugeordnet. Sie müssen sicherstellen, dass die NULL-Werte dem richtigen Ordner zugeordnet werden. Die innere Ebene (`DISTRIBUTED BY`) in U-SQL bietet 4 Verteilungsschemas: Roundrobin, Bereich, Hash und direkter Hash.
    Hive/Spark-Tabellen unterstützen nur die Wert- oder die Hashpartitionierung, wobei eine andere Hashfunktion als bei U-SQL verwendet wird. Bei der Ausgabe der U-SQL-Tabellendaten können Sie wahrscheinlich nur die Wertpartitionierung für Spark zuordnen und müssen abhängig von den endgültigen Spark-Abfragen möglicherweise eine weitere Optimierung des Datenlayouts vornehmen.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Spark-Codekonzepten für U-SQL-Entwickler](understand-spark-code-concepts.md)
- [Upgrade von Big Data-Analyselösungen von Azure Data Lake Storage Gen1 auf Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md)
- [.NET für Apache Spark](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](../data-factory/transform-data-using-spark.md)
- [Transformieren von Daten mit der Hadoop Hive-Aktivität in Azure Data Factory](../data-factory/transform-data-using-hadoop-hive.md)
- [Was ist Apache Spark in Azure HDInsight?](../hdinsight/spark/apache-spark-overview.md)
