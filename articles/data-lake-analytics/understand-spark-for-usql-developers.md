---
title: Grundlegendes zu Apache Spark für Azure Data Lake Analytics-U-SQL-Entwickler
description: In diesem Artikel werden Apache Spark-Konzepte zum Unterscheiden zwischen U-SQL-Entwicklern beschrieben.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: understand-apache-spark-for-usql-developers
ms.date: 10/15/2019
ms.openlocfilehash: 594e1055c4c063e4e151fefa3e183e6e799c90b5
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648163"
---
# <a name="understand-apache-spark-for-u-sql-developers"></a>Grundlegendes zu Apache Spark für U-SQL-Entwickler

Microsoft unterstützt verschiedene Analysedienste wie [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) und [Azure HDInsight](../hdinsight/hdinsight-overview.md) sowie Azure Data Lake Analytics. Wir hören von Entwicklern, dass sie beim Erstellen von Analysepipelines ganz eindeutig Open Source-Lösungen bevorzugen. Wir haben diesen Leitfaden erstellt, um U-SQL-Entwicklern Apache Spark zu veranschaulichen und zu erläutern, wie U-SQL-Skripts in Apache Spark transformiert werden können.

Der Leitfaden enthält eine Reihe von Schritten, die ausgeführt werden können, sowie verschiedene Alternativen.

## <a name="steps-to-transform-u-sql-to-apache-spark"></a>Schritte zum Transformieren von U-SQL in Apache Spark

1. Transformieren Ihrer Pipelines für die Auftragsorchestrierung

   Wenn Sie [Azure Data Factory](../data-factory/introduction.md) zum Orchestrieren Ihrer Azure Data Lake Analytics-Skripts verwenden, müssen Sie diese für das Orchestrieren der neuen Spark-Programme anpassen.
2. Verstehen der Unterschiede zwischen der Datenverwaltung in U-SQL und Spark

   Wenn Sie Ihre Daten aus [Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md) in [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) verschieben möchten, müssen Sie sowohl die Dateidaten als auch die im Katalog verwalteten Daten kopieren. Beachten Sie, dass Azure Data Lake Analytics nur Azure Data Lake Storage Gen1 unterstützt. Informationen dazu finden Sie unter [Grundlegendes zu Spark-Datenformaten](understand-spark-data-formats.md).
3. Transformieren Ihrer U-SQL-Skripts in Spark

   Vor dem Transformieren Ihrer U-SQL-Skripts müssen Sie einen Analysedienst auswählen. Nachfolgend sind einige der verfügbaren Computedienste aufgelistet:
      - [Azure Data Factory-Datenfluss](../data-factory/concepts-data-flow-overview.md): Zuordnungsdatenflüsse sind visuell gestaltete Datentransformationen, mit denen Datentechniker eine grafische Datentransformationslogik entwickeln können, ohne Code schreiben zu müssen. Obwohl nicht zum Ausführen von komplexem Benutzercode geeignet, können sie auf einfache Weise herkömmliche SQL-ähnliche Datenflusstransformationen darstellen.
      - [Azure HDInsight Hive](../hdinsight/hadoop/apache-hadoop-using-apache-hive-as-an-etl-tool.md): Apache Hive in HDInsight eignet sich für ETL-Vorgänge (Extrahieren, Transformieren und Laden). Das bedeutet, dass Sie Ihre U-SQL-Skripts in Apache Hive übersetzen.
      - Apache Spark-Engines wie [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) oder [Azure Databricks](../azure-databricks/what-is-azure-databricks.md): Das bedeutet, dass Sie Ihre U-SQL-Skripts in Spark übersetzen. Weitere Informationen finden Sie unter [Grundlegendes zu Spark-Datenformaten](understand-spark-data-formats.md).

> [!CAUTION]
> [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) und [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) sind Clusterdienste und keine serverlosen Aufträge wie Azure Data Lake Analytics. Sie müssen berücksichtigen, wie die Cluster bereitgestellt werden, um das entsprechende Preis-Leistungs-Verhältnis zu erzielen, und wie deren Lebensdauer zu verwalten ist, um Kosten zu minimieren. Diese Dienste weisen verschiedene Leistungsmerkmale mit Benutzercode in .NET auf, sodass Sie entweder Wrapper schreiben oder Ihren Code in einer unterstützten Sprache umschreiben müssen. Weitere Informationen finden Sie unter [Grundlegendes zu Spark-Datenformaten](understand-spark-data-formats.md), [Grundlegendes zu Apache Spark-Codekonzepten für U-SQL-Entwickler](understand-spark-code-concepts.md) und [.Net für Apache Spark](https://dotnet.microsoft.com/apps/data/spark).

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Spark-Datenformaten für U-SQL-Entwickler](understand-spark-data-formats.md)
- [Grundlegendes zu Spark-Codekonzepten für U-SQL-Entwickler](understand-spark-code-concepts.md)
- [Upgrade von Big Data-Analyselösungen von Azure Data Lake Storage Gen1 auf Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md)
- [.NET für Apache Spark](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Transformieren von Daten mit der Hadoop Hive-Aktivität in Azure Data Factory](../data-factory/transform-data-using-hadoop-hive.md)
- [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](../data-factory/transform-data-using-spark.md)
- [Was ist Apache Spark in Azure HDInsight?](../hdinsight/spark/apache-spark-overview.md)
