---
title: Problembehandlung für Spark in Azure HDInsight
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Arbeiten mit Apache Spark und Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: c88136fee7a75b8f3b8e504b1ff1e6673a31bcf7
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543175"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Problembehandlung bei Apache Spark mit Azure HDInsight

Lernen Sie die wichtigsten Probleme und ihre Lösungen bei der Arbeit mit [Apache Spark](https://spark.apache.org/)-Nutzlasten in [Apache Ambari](https://ambari.apache.org/) kennen.

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über Apache Ambari in Clustern?

### <a name="resolution-steps"></a>Lösungsschritte

Spark-Konfigurationswerte können optimiert werden, um eine OutofMemoryError-Ausnahme der Apache Spark-Anwendung zu vermeiden. Die folgenden Schritte zeigen die Standardwerte der Spark-Konfiguration in Azure HDInsight: 

1. Wählen Sie in der Liste der Cluster **Spark2** aus.

    ![Cluster aus der Liste wählen](./media/apache-troubleshoot-spark/update-config-1.png)

2. Wählen Sie die Registerkarte **Configs** aus.

    ![Registerkarte „Configs“ auswählen](./media/apache-troubleshoot-spark/update-config-2.png)

3. Wählen Sie in der Liste der Konfigurationen **Custom-spark2-defaults** aus.

    ![„custom-spark-defaults“ auswählen](./media/apache-troubleshoot-spark/update-config-3.png)

4. Suchen Sie nach der Werteinstellung, die Sie anpassen müssen, z.B. **spark.executor.memory**. In diesem Fall ist der Wert **4608m** hoch.

    ![Feld „spark.executor.memory“ wählen](./media/apache-troubleshoot-spark/update-config-4.png)

5. Legen Sie den Wert auf die empfohlene Einstellung fest. Für diese Einstellung wird der Wert **2048m** empfohlen.

    ![Wert in „2048m“ ändern](./media/apache-troubleshoot-spark/update-config-5.png)

6. Speichern Sie den Wert und anschließend die Konfiguration. Wählen Sie auf der Symbolleiste **Save** (Speichern) aus.

    ![Einstellung und Konfiguration speichern](./media/apache-troubleshoot-spark/update-config-6a.png)

    Sie werden benachrichtigt, wenn eine Konfiguration Ihre Aufmerksamkeit erfordert. Notieren Sie sich die Elemente, und wählen Sie anschließend **Proceed Anyway** (Dennoch fortfahren) aus. 

    ![„Proceed Anyway“ (Dennoch fortfahren) auswählen](./media/apache-troubleshoot-spark/update-config-6b.png)

    Schreiben Sie eine Notiz zu den Konfigurationsänderungen, und wählen Sie **Save** (Speichern) aus.

    ![Notiz zu den von Ihnen vorgenommenen Änderungen eingeben](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Wenn eine Konfiguration gespeichert ist, werden Sie aufgefordert, den Dienst neu starten. Wählen Sie **Neu starten** aus.

    ![„Neu starten“ auswählen](./media/apache-troubleshoot-spark/update-config-7a.png)

    Bestätigen Sie den Neustart.

    ![„Confirm Restart All“ (Bestätigen: Alle neu starten) auswählen](./media/apache-troubleshoot-spark/update-config-7b.png)

    Sie können die ausgeführten Prozesse überprüfen.

    ![Ausgeführte Prozesse überprüfen](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Sie können Konfigurationen hinzufügen. Wählen Sie in der Liste der Konfigurationen **Custom-spark2-defaults** aus, und wählen Sie dann **Add Property** (Eigenschaft hinzufügen) aus.

    ![„Add Property“ (Eigenschaft hinzufügen) auswählen](./media/apache-troubleshoot-spark/update-config-8.png)

9. Definieren Sie eine neue Eigenschaft. Sie können eine einzelne Eigenschaft mithilfe eines Dialogfelds für bestimmte Einstellungen definieren (z.B. den Datentyp). Sie können auch mehrere Eigenschaften definieren, indem Sie eine Eigenschaft pro Zeile angeben. 

    In diesem Beispiel wird die Eigenschaft **spark.driver.memory** mit dem Wert **4g** definiert.

    ![Neue Eigenschaft definieren](./media/apache-troubleshoot-spark/update-config-9.png)

10. Speichern Sie die Konfiguration, und starten Sie den Dienst wie in Schritt 6 und 7 beschrieben neu.

Diese Änderungen betreffen den gesamten Cluster, sie können jedoch beim Senden des Spark-Auftrags überschrieben werden.

### <a name="additional-reading"></a>Zusätzliche Lektüre

[Übermitteln von Apache Spark-Aufträgen in HDInsight-Clustern](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über ein Jupyter Notebook in Clustern?

### <a name="resolution-steps"></a>Lösungsschritte

1. Wenn Sie ermitteln möchten, welche Spark-Konfigurationen auf welche Werte festgelegt werden müssen, lesen Sie den Artikel „Wodurch wird eine OutOfMemoryError-Ausnahme in einer Apache Spark-Anwendung verursacht?“

2. Geben Sie in der ersten Zelle im Jupyter-Notebook nach der Anweisung **%%configure** die Spark-Konfigurationen in einem gültigen JSON-Format ein. Ändern Sie die tatsächlichen Werte entsprechend den jeweiligen Anforderungen:

    ![Konfiguration hinzufügen](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Zusätzliche Lektüre

[Übermitteln von Apache Spark-Aufträgen in HDInsight-Clustern](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über Apache Livy in Clustern?

### <a name="resolution-steps"></a>Lösungsschritte

1. Wenn Sie ermitteln möchten, welche Spark-Konfigurationen auf welche Werte festgelegt werden müssen, lesen Sie den Artikel „Wodurch wird eine OutOfMemoryError-Ausnahme in einer Apache Spark-Anwendung verursacht?“ 

2. Senden Sie die Spark-Anwendung mit einem REST-Client wie cURL an Livy. Führen Sie einen Befehl aus, der dem folgenden ähnelt: Ändern Sie die tatsächlichen Werte entsprechend den jeweiligen Anforderungen:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Zusätzliche Lektüre

[Übermitteln von Apache Spark-Aufträgen in HDInsight-Clustern](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Wie konfiguriere ich eine Apache Spark-Anwendung über spark-submit in Clustern?

### <a name="resolution-steps"></a>Lösungsschritte

1. Wenn Sie ermitteln möchten, welche Spark-Konfigurationen auf welche Werte festgelegt werden müssen, lesen Sie den Artikel „Wodurch wird eine OutOfMemoryError-Ausnahme in einer Apache Spark-Anwendung verursacht?“

2. Starten Sie spark-shell mit einem Befehl, der dem folgenden ähnelt. Ändern Sie den tatsächlichen Wert der Konfigurationen entsprechend den jeweiligen Anforderungen: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Zusätzliche Lektüre

[Übermitteln von Apache Spark-Aufträgen in HDInsight-Clustern](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* [Spark memory management overview](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview) (Übersicht über die Spark-Speicherverwaltung).

* [Debugging Spark application on HDInsight clusters](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/) (Debuggen der Spark-Anwendung in HDInsight-Clustern).

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
