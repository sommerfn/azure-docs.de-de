---
title: RpcTimeoutException für Apache Spark-Thrift-Server in Azure HDInsight
description: Bei der Verarbeitung großer Datasets mit Apache Spark-Thrift-Server werden 502-Fehler angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 0d4f2f9c8392e5b81f5abb8c67548baefae611c9
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620602"
---
# <a name="scenario-rpctimeoutexception-for-apache-spark-thrift-server-in-azure-hdinsight"></a>Szenario: RpcTimeoutException für Apache Spark-Thrift-Server in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Spark-Anwendung löst die Ausnahme `org.apache.spark.rpc.RpcTimeoutException` mit der Meldung `Futures timed out` aus, wie im folgenden Beispiel gezeigt:

```
org.apache.spark.rpc.RpcTimeoutException: Futures timed out after [120 seconds]. This timeout is controlled by spark.rpc.askTimeout
 at org.apache.spark.rpc.RpcTimeout.org$apache$spark$rpc$RpcTimeout$$createRpcTimeoutException(RpcTimeout.scala:48)
```

Die Fehler `OutOfMemoryError` und `overhead limit exceeded` können auch in `sparkthriftdriver.log` enthalten sein, wie im folgenden Beispiel gezeigt:

```
WARN  [rpc-server-3-4] server.TransportChannelHandler: Exception in connection from /10.0.0.17:53218
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

## <a name="cause"></a>Ursache

Diese Fehler werden durch fehlende Speicherressourcen während der Datenverarbeitung verursacht. Wenn der Java-Garbage Collection-Prozess startet, kann dies dazu führen, dass die Spark-Anwendung nicht mehr reagiert. Es kommt bei den Abfragen zu Timeouts, und die Verarbeitung wird beendet. Der Fehler `Futures timed out` weist auf einen überlasteten Cluster hin.

## <a name="resolution"></a>Lösung

Erhöhen Sie die Clustergröße, indem Sie zusätzliche Workerknoten hinzufügen oder die Arbeitsspeicherkapazität der vorhandenen Clusterknoten erhöhen. Sie können auch die Datenpipeline anpassen, um die Menge der gleichzeitig verarbeiteten Daten zu verringern.

`spark.network.timeout` steuert das Timeout für alle Netzwerkverbindungen. Wenn Sie das Netzwerktimeout erhöhen, wird mehr Zeit für die Beendigung einiger kritischer Vorgänge bereitgestellt – dadurch wird das Problem allerdings nicht vollständig gelöst.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie über den [Azure-Communitysupport](https://azure.microsoft.com/support/community/) Antworten von Azure-Experten.

* Setzen Sie sich mit [@AzureSupport](https://twitter.com/azuresupport) in Verbindung – dem offiziellen Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber erhält die Azure-Community Kontakt mit den richtigen Ressourcen: Antworten, Support und Experten.

* Wenn Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführliche Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
