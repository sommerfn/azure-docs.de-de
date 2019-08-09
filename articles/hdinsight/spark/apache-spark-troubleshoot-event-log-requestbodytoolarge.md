---
title: NativeAzureFileSystem ... RequestBodyTooLarge ist im Protokoll für die Apache Spark-Streaming-App in Azure HDInsight enthalten.
description: NativeAzureFileSystem ... RequestBodyTooLarge ist im Protokoll für die Apache Spark-Streaming-App in Azure HDInsight enthalten.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 571e02e79714d2e713d4173936120e78e4b067de
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700198"
---
# <a name="scenario-nativeazurefilesystem--requestbodytoolarge-appears-in-log-for-apache-spark-streaming-app-in-azure-hdinsight"></a>Szenario: „NativeAzureFileSystem ... RequestBodyTooLarge“ ist im Protokoll für die Apache Spark-Streaming-App in Azure HDInsight enthalten.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Der Fehler `NativeAzureFileSystem ... RequestBodyTooLarge` wird in das Treiberprotokoll für eine Apache Spark-Streaming-App geschrieben.

## <a name="cause"></a>Ursache

Die Größe Ihrer Spark-Ereignisprotokolldatei übersteigt vermutlich die Dateilänge für WASB.

In Spark 2.3 generiert jede Spark-App eine Spark-Ereignisprotokolldatei. Die Spark-Ereignisprotokolldatei für eine Spark-Streaming-App wird während der App-Ausführung immer größer. Heutzutage gilt für eine Datei in WASB ein Blocklimit von 50.000, und die Standardblockgröße beträgt 4 MB. Also beträgt die maximale Dateigröße in der Standardkonfiguration 195 GB. In Azure Storage wurde jedoch die maximale Blockgröße auf 100 MB angehoben und dadurch die Beschränkung für einzelne Dateien auf 4,75 TB erhöht. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungszielen für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

## <a name="resolution"></a>Lösung

Für diesen Fehler gibt es drei Lösungen:

* Vergrößern Sie die Blockgröße auf bis zu 100 MB. Ändern Sie auf der Ambari-Benutzeroberfläche die HDFS-Konfigurationseigenschaft `fs.azure.write.request.size` (oder erstellen Sie sie im Abschnitt `Custom core-site`). Legen Sie die Eigenschaft auf einen größeren Wert fest, z. B.: 33554432. Speichern Sie die aktualisierte Konfiguration, und starten Sie die betroffenen Komponenten neu.

* Beenden Sie den Spark-Streamingauftrag regelmäßig, und übermitteln Sie ihn erneut.

* Verwenden Sie HDFS zum Speichern der Spark-Ereignisprotokolle. Die Verwendung von HDFS für die Speicherung kann zu einem Verlust von Spark-Ereignisdaten während der Clusterskalierung oder bei Azure-Upgrades führen.

    1. Nehmen Sie auf der Ambari-Benutzeroberfläche Änderungen an `spark.eventlog.dir` und `spark.history.fs.logDirectory` vor:

        ```
        spark.eventlog.dir = hdfs://mycluster/hdp/spark2-events
        spark.history.fs.logDirectory = "hdfs://mycluster/hdp/spark2-events"
        ```

    1. Erstellen Sie Verzeichnisse in HDFS:

        ```
        hadoop fs -mkdir -p hdfs://mycluster/hdp/spark2-events
        hadoop fs -chown -R spark:hadoop hdfs://mycluster/hdp
        hadoop fs -chmod -R 777 hdfs://mycluster/hdp/spark2-events
        hadoop fs -chmod -R o+t hdfs://mycluster/hdp/spark2-events
        ```

    1. Starten Sie alle betroffenen Dienste über die Ambari-Benutzeroberfläche neu.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
