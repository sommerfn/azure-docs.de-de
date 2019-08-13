---
title: Längere Verarbeitungszeit für Apache Spark-Streamingaufträge in Azure HDInsight
description: Längere Verarbeitungszeit für Apache Spark-Streamingaufträge in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 7c7baaaa5fd08a102b3b55c38fb2c4bf892480c5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679407"
---
# <a name="scenario-apache-spark-streaming-jobs-take-longer-than-usual-to-process-in-azure-hdinsight"></a>Szenario: Längere Verarbeitungszeit für Apache Spark-Streamingaufträge in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie haben beobachtet, dass einige der Apache Spark-Streamingaufträge langsam sind oder ihre Verarbeitung länger als üblich dauert. Bei Spark-Streaminganwendungen entspricht jeder Batch von Nachrichten einem Auftrag, der an Spark übermittelt wird. Normalerweise nimmt die Verarbeitung eines Auftrags X Sekunden in Anspruch. Sie kann aber gelegentlich 2–3 Minuten länger als üblich dauern.

## <a name="cause"></a>Ursache

Eine mögliche Ursache ist, dass der Apache Kafka-Producer mehr als 2 Minuten benötigt, um den Schreibvorgang in den Kafka-Cluster abzuschließen. Zum weiteren Debuggen des Kafka-Problems können Sie dem Code eine Protokollierung hinzufügen, die einen Kafka-Producer zum Senden von Nachrichten verwendet, und diese mit den Protokollen aus dem Kafka-Cluster vergleichen.

Eine weitere mögliche Ursache besteht darin, dass nachfolgende, sehr kleine Batches durch häufige Lese- und Schreibvorgänge in WASB verzögert werden können. Die WASB-Implementierung von `Filesystem.listStatus` ist aufgrund eines `O(n!)`-Algorithmus zum Entfernen von Duplikaten sehr langsam. Sie verbraucht aufgrund der zusätzlichen Konvertierung von `BlobListItem` in `FileMetadata` in `FileStatus` zu viel Arbeitsspeicher. Der Algorithmus benötigt z. B. mehr als 30 Minuten, um 700.000 Dateien aufzulisten. Wenn `ListBlobs` also von SparkSQL für jeden Mikrobatch aggressiv aufgerufen wird, führt dies dazu, dass nachfolgende Mikrobatches verzögert werden. Dies macht sich als lange Planungsverzögerung bemerkbar. Mit [diesem Patch](https://issues.apache.org/jira/browse/HADOOP-15547) wird dieses Problem behoben. Wenn dieser aber in Ihrer Umgebung fehlt, treten bei `ListBlobs` hohe Wartezeiten auf. Selbst wenn Sie stündlich Dateien löschen, muss die Auflistung am Back-End außerdem alle Zeilen (einschließlich der gelöschten) durchlaufen, da die automatische Speicherbereinigung noch nicht abgeschlossen ist. Der Patch wird sicherlich einen Teil des Problems lösen, das Problem mit der automatischen Speicherbereinigung wird aber weiterhin zu einer Verzögerung bei der Streamverarbeitung von Batches führen.

## <a name="resolution"></a>Lösung

Wenden Sie die Korrektur [HADOOP-15547](https://issues.apache.org/jira/browse/HADOOP-15547) an. Wenn dies nicht möglich ist, können Sie HDFS als Position für den Prüfpunkt verwenden. Legen Sie `checkpointDirectory` z. B. auf `hdfs://mycluster/checkpoint` fest.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
