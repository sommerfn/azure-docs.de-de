---
title: IllegalArgumentException für Apache Spark-Aktivität in Azure HDInsight
description: IllegalArgumentException für Apache Spark-Aktivität in Azure HDInsight für Azure Data Factory
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 54e07a840aac1e754db68d9a14403750757f4bcf
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620606"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Szenario: IllegalArgumentException für Apache Spark-Aktivität in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Sie erhalten die folgende Ausnahme, wenn Sie versuchen, eine Spark-Aktivität in einer Azure Data Factory-Pipeline auszuführen:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Ursache

Ein Spark-Auftrag kann nicht ausgeführt werden, wenn sich die JAR-Datei der Anwendung nicht im standardmäßigen oder primären Speicher des Spark-Clusters befindet.

Dies ist ein bekanntes Problem beim Open-Source-Framework von Spark, das in diesem Fehler nachverfolgt wird: [Fehler beim Spark-Auftrag, wenn fs.defaultFS und die JAR-Datei der Anwendung unterschiedliche URLs aufweisen](https://issues.apache.org/jira/browse/SPARK-22587).

Das Problem wurde in Spark 2.3.0 gelöst.

## <a name="resolution"></a>Lösung

Stellen Sie sicher, dass die JAR-Datei der Anwendung im standardmäßigen/primären Speicher für den HDInsight-Cluster gespeichert wird. Stellen Sie bei Azure Data Factory sicher, dass der über ADF verknüpfte Dienst auf den HDInsight-Standardcontainer verweist, nicht auf einen sekundären Container.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
