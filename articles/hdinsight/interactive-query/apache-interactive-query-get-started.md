---
title: Was ist Interactive Query in Azure HDInsight?
description: Eine Einführung in Interactive Query in Azure HDInsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: overview
ms.date: 06/14/2019
ms.openlocfilehash: ea17ddeab21c371f41cc57115df4dd91277c3c42
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151199"
---
# <a name="what-is-interactive-query-in-azure-hdinsight"></a>Interactive Query in Azure HDInsight

Interactive Query (auch Apache Hive LLAP oder [Low Latency Analytical Processing](https://cwiki.apache.org/confluence/display/Hive/LLAP) genannt) ist ein Azure HDInsight-[Clustertyp](../hdinsight-hadoop-provision-linux-clusters.md#cluster-types). Interactive Query unterstützt speicherinternes Zwischenspeichern, wodurch Apache Hive-Abfragen schneller und viel interaktiver werden. Kunden verwenden Interactive Query, um Daten, die in Azure Storage und Azure Data Lake Storage gespeichert sind, äußerst schnell abzufragen. Interactive Query erleichtert Entwicklern und Data Scientists die Arbeit mit Big Data mit den BI-Tools, die sie am meisten lieben. HDInsight Interactive Query unterstützt verschiedene Tools für den mühelosen Zugriff auf Big Data.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Ein Interactive Query-Cluster unterscheidet sich von einem Apache Hadoop-Cluster. Er enthält nur den Hive-Dienst.

Sie können im Interactive Query-Cluster nur über die Apache Ambari-Hive-Ansicht, Beeline und den Microsoft Hive Open Database Connectivity-Treiber (Hive ODBC) auf den Hive-Dienst zugreifen. Sie können nicht über die Hive-Konsole, Templeton, die klassische Azure-Befehlszeilenschnittstelle oder Azure PowerShell darauf zugreifen.

## <a name="create-an-interactive-query-cluster"></a>Erstellen eines Interactive Query-Clusters

Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Einrichten von Clustern in HDInsight mit Hadoop, Spark, Kafka usw](../hdinsight-hadoop-provision-linux-clusters.md). Wählen Sie den Clustertyp „Interactive Query“ aus.

## <a name="execute-apache-hive-queries-from-interactive-query"></a>Ausführen von Apache Hive-Abfragen über Interactive Query

Um Hive-Abfragen auszuführen, können Sie folgende Optionen nutzen:

* Verwenden von Microsoft Power BI

    Siehe [Visualisieren von Interactive Query-Hive-Daten mit Power BI mithilfe direkter Abfragen in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md). Siehe [Visualisieren von Apache Hive-Daten mit Microsoft Power BI mithilfe von ODBC in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).

* Verwenden von Visual Studio

    Siehe [Verbinden mit Azure HDInsight und Ausführen von Apache Hive-Abfragen mithilfe von Data Lake-Tools für Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#run-interactive-apache-hive-queries)

* Verwenden von Visual Studio Code

    Weitere Informationen finden Sie unter [Verwenden von Visual Studio Code für Apache Hive, LLAP oder pySpark](../hdinsight-for-vscode.md).
* Ausführen von Apache Hive mithilfe der Apache Ambari-Hive-Ansicht.
  
    Siehe [Verwenden der Apache Ambari-Hive-Ansicht mit Apache Hadoop in HDInsight](../hadoop/apache-hadoop-use-hive-ambari-view.md)

* Ausführen von Apache Hive mithilfe von Beeline
  
    Informationen finden Sie unter [Verwenden des Apache Beeline-Clients mit Apache Hive](../hadoop/apache-hadoop-use-hive-beeline.md).
  
    Sie können Beeline entweder über den Hauptknoten oder von einem leeren Edgeknoten aus verwenden. Am besten verwenden Sie Beeline von einem leeren Edgeknoten aus. Informationen zum Erstellen eines HDInsight-Clusters mit einem leeren Edgeknoten finden Sie unter [Verwenden leerer Edgeknoten in Hadoop-Clustern in HDInsight](../hdinsight-apps-use-edge-node.md).
* Ausführen von Apache Hive mithilfe von Hive ODBC
  
    Informationen finden Sie unter [Verbinden von Excel mit Apache Hadoop in Azure HDInsight mithilfe des Microsoft Hive ODBC-Treibers](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).

So suchen Sie die Verbindungszeichenfolge für die Java Database Connectivity (JDBC):

1. Melden Sie sich bei Apache Ambari mit der folgenden URL an: `https://<cluster name>.AzureHDInsight.net`.
2. Wählen Sie im Menü auf der linken Seite die Option **Hive** aus.
3. Wählen Sie das Symbol für die Zwischenablage zum Kopieren der URL aus:

   ![HDInsight Hadoop Interactive Query LLAP JDBC](./media/apache-interactive-query-get-started/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Interactive Query-Cluster in HDInsight erstellen](../hdinsight-hadoop-provision-linux-clusters.md).
* Erfahren Sie, wie Sie [Big Data mit Power BI in Azure HDInsight visualisieren](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* Erfahren Sie, wie Sie [Apache Zeppelin zum Ausführen von Apache Hive-Abfragen in Azure HDInsight verwenden](../interactive-query/hdinsight-connect-hive-zeppelin.md).
