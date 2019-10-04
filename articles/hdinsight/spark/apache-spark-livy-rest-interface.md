---
title: Übermittlung von Aufträgen an einen Spark-Cluster in Azure HDInsight mithilfe von Livy Spark
description: Erfahren Sie, wie Sie die Apache Spark-REST-API verwenden, um Spark-Aufträge remote an einen Azure HDInsight-Cluster übermitteln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: f5b3500e1e700abf894fc4e21fb540eb258d5e35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066055"
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>Übermitteln von Remoteaufträgen an einen HDInsight Spark-Cluster mithilfe der Apache Spark-REST-API

Erfahren Sie, wie Sie [Apache Livy](https://livy.incubator.apache.org/), die [Apache Spark](https://spark.apache.org/)-REST-API, verwenden, um Remoteaufträge an einen Azure HDInsight Spark-Cluster zu übermitteln. Eine ausführliche Dokumentation finden Sie unter [https://livy.incubator.apache.org/](https://livy.incubator.apache.org/).

Mit Livy können Sie interaktive Spark-Shells ausführen oder Batchaufträge zur Ausführung in Spark übermitteln. In diesem Artikel wird die Übermittlung von Batchaufträgen mithilfe von Livy behandelt. Die Codeausschnitte in diesem Artikel verwenden cURL für an den Livy Spark-Endgerät gerichtete REST-API-Aufrufe.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* [cURL](https://curl.haxx.se/). Dieser Artikel zeigt mit cURL, wie Sie REST-API-Aufrufe für einen HDInsight Spark-Cluster ausführen.

## <a name="submit-an-apache-livy-spark-batch-job"></a>Übermitteln eines Apache Livy Spark-Batchvorgangs

Vor dem Übermitteln eines Batchauftrags muss die JAR-Anwendungsdatei an den Clusterspeicher hochgeladen werden, der dem Cluster zugeordnet ist. Hierzu können Sie das Befehlszeilenprogramm [AzCopy](../../storage/common/storage-use-azcopy.md) verwenden. Die Daten können aber auch mit verschiedenen anderen Clients hochgeladen werden. Weitere Informationen finden Sie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](../hdinsight-upload-data.md).

```cmd
curl -k --user "<hdinsight user>:<user password>" -v -H "Content-Type: application/json" -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches' -H "X-Requested-By: admin"
```

### <a name="examples"></a>Beispiele

* In diesem Beispiel befindet sich die JAR-Datei im Clusterspeicher (WASB):

    ```cmd  
    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

* In diesem Beispiel möchten Sie den Namen der JAR-Datei und den Klassennamen als Teil der Eingabedatei „input.txt“ übergeben:

    ```cmd
    curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Abrufen von Informationen zu im Cluster ausgeführten Livy Spark-Batches

Syntax:

```cmd
curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"
```

### <a name="examples"></a>Beispiele

* In diesem Beispiel sollen alle im Cluster ausgeführten Livy Spark-Batches abgerufen werden:

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches" 
    ```

* In diesem Beispiel möchten Sie einen spezifischen Batch mit einer bestimmten Batch-ID abrufen:

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"
    ```

## <a name="delete-a-livy-spark-batch-job"></a>Löschen eines Livy Spark-Batchvorgangs

```cmd
curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"
```

### <a name="example"></a>Beispiel

Löschen des Batch-Auftrags mit der Batch-ID `5`

```cmd
curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/5"
```

## <a name="livy-spark-and-high-availability"></a>Livy Spark und hohe Verfügbarkeit

Livy bietet hohe Verfügbarkeit für im Cluster ausgeführte Spark-Aufträge. Hier sind einige Beispiele angegeben:

* Wenn der Livy-Dienst nach der Übermittlung eines Remoteauftrags an einen Spark-Cluster abstürzt, wird der Auftrag weiterhin im Hintergrund ausgeführt. Wenn Livy wieder funktioniert, stellt es den Status des Auftrags wieder her und erstattet Bericht.
* Jupyter Notebooks für HDInsight wird am Back-End durch Livy unterstützt. Wenn ein Notebook einen Spark-Auftrag ausführt und der Livy-Dienst neu gestartet wird, führt das Notebook weiterhin die Codezellen aus. 

## <a name="show-me-an-example"></a>Beispiele

In diesem Abschnitt erfahren Sie anhand von Beispielen, wie Sie mit Livy Spark einen Batchvorgang übermitteln, den Status des Vorgangs überwachen und den Vorgang anschließend löschen. Dabei verwenden wir die im Artikel [Erstellen einer eigenständigen Scala-Anwendung zur Ausführung in HDInsight Spark-Clustern](apache-spark-create-standalone-application.md)entwickelte Anwendung. Bei den folgenden Schritten wird von Folgendem ausgegangen:

* Die JAR-Anwendungsdatei wurde bereits in das dem Cluster zugeordnete Speicherkonto kopiert.
* Auf dem Computer, auf dem die Schritte ausgeführt werden, ist cURL installiert.

Führen Sie die folgenden Schritte aus:

1. Überprüfen Sie zunächst, ob Livy Spark im Cluster ausgeführt wird. Dazu können Sie eine Liste mit ausgeführten Batches abrufen. Bei der erstmaligen Ausführung eines Auftrags mithilfe von Livy wird Null zurückgegeben.

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
    ```

    Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:47:53 GMT
    < Content-Length: 34
    <
    {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Beachten Sie, dass in der letzten Zeile der Ausgabe **total:0** angegeben ist. Das bedeutet, dass keine Batches ausgeführt werden.

2. Übermitteln wir nun einen Batchauftrag. Im folgenden Codeausschnitt werden mithilfe einer Eingabedatei namens „input.txt“ der JAR-Name und der Klassenname als Parameter übergeben. Beim Ausführen der Schritte auf einem Windows-Computer wird die Verwendung einer Eingabedatei empfohlen.

    ```cmd
    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

    Die Parameter in der Datei **input.txt** sind wie folgt definiert:

    ```text
    { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
    ```

    Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    ```output
    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /0
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:51:30 GMT
    < Content-Length: 36
    <
    {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Beachten Sie die Angabe **state:starting**in der letzten Zeile der Ausgabe. Und die Angabe **id:0**. **0** ist hier die Batch-ID.

3. Anhand der Batch-ID können Sie nun den Status dieses speziellen Batchs abrufen.

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
    ```

    Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:54:42 GMT
    < Content-Length: 509
    <
    {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Die Ausgabe enthält nun **state:success**. Der Auftrag wurde also erfolgreich abgeschlossen.

4. Wenn Sie möchten, können Sie den Batch jetzt löschen.

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
    ```

    Die Ausgabe sollte in etwa wie folgender Codeausschnitt aussehen:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Sat, 21 Nov 2015 18:51:54 GMT
    < Content-Length: 17
    <
    {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Die letzte Zeile der Ausgabe zeigt, dass der Batch erfolgreich gelöscht wurde. Beim Löschen eines ausgeführten Auftrags wird dieser gleichzeitig beendet. Beim Löschen eines (erfolgreich oder anderweitig) abgeschlossenen Auftrags werden die Auftragsinformationen vollständig gelöscht.

## <a name="updates-to-livy-configuration-starting-with-hdinsight-35-version"></a>Aktualisierungen der Livy-Konfiguration ab Version 3.5 von HDInsight

HDInsight-Cluster mit Version 3.5 und höheren Versionen deaktivieren standardmäßig die Verwendung der lokalen Dateipfade für den Zugriff auf Beispieldatendateien oder JAR-Dateien. Wir empfehlen Ihnen stattdessen die Verwendung des `wasb://`-Pfads, um aus dem Cluster auf Beispieldatendateien oder JAR-Dateien zuzugreifen.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Übermitteln von Livy-Aufträgen für einen Cluster in einem virtuellen Azure-Netzwerk

Wenn Sie aus einer Azure Virtual Network-Instanz eine Verbindung mit einem HDInsight Spark-Cluster herstellen, können Sie direkt eine Verbindung mit Livy im Cluster herstellen. In diesem Fall lautet die URL für den Livy-Endpunkt `http://<IP address of the headnode>:8998/batches`. **8998** ist der Port, an dem Livy im Clusterhauptknoten ausgeführt wird. Weitere Informationen zum Zugreifen auf Dienste über nicht öffentliche Ports finden Sie unter [Ports für Apache Hadoop-Dienste in HDInsight](../hdinsight-hadoop-port-settings-for-services.md).

## <a name="next-steps"></a>Nächste Schritte

* [Apache Livy-REST-API-Dokumentation](https://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)