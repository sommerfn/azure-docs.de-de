---
title: Integrieren von Apache Spark und Apache Hive per Hive Warehouse Connector
description: Es wird beschrieben, wie Sie Apache Spark und Apache Hive unter Azure HDInsight per Hive Warehouse Connector integrieren.
ms.service: hdinsight
author: nakhanha
ms.author: nakhanha
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: b245661ab8f26c1f529a049d326d2c72838c7a17
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056725"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Integrieren von Apache Spark und Apache Hive per Hive Warehouse Connector

Bei Apache Hive Warehouse Connector (HWC) handelt es sich um eine Bibliothek, die Ihnen die Verwendung von Apache Spark und Apache Hive vereinfacht. Sie unterstützt Aufgaben wie das Verschieben von Daten zwischen Spark-Datenrahmen und Hive-Tabellen und außerdem das Leiten von Spark-Streamingdaten an Hive-Tabellen. Hive Warehouse Connector fungiert quasi als Brücke zwischen Spark und Hive. Für die Entwicklung werden Scala, Java und Python unterstützt.

Mit Hive Warehouse Connector können Sie die einzigartigen Features von Hive und Spark nutzen, um leistungsfähige Big Data-Anwendungen zu entwickeln. Apache Hive verfügt über Unterstützung für Datenbanktransaktionen, die atomisch, konsistent, isoliert und dauerhaft sind (Atomic, Consistent, Isolated and Durable, ACID). Weitere Informationen zu ACID und Transaktionen in Hive finden Sie im Artikel zu [Hive-Transaktionen](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Darüber hinaus verfügt Hive über ausführliche Sicherheitskontrollen per Apache Ranger und Low Latency Analytical Processing (Analytische Verarbeitung mit geringer Latenz), die in Apache Spark nicht verfügbar sind.

Apache Spark enthält eine API für strukturierte Streams, mit der Streamingfunktionen ermöglicht werden, die in Apache Hive nicht verfügbar sind. Ab HDInsight 4.0 weisen Apache Spark 2.3.1 und Apache Hive 3.1.0 separate Metastores auf, sodass die Interoperabilität erschwert werden kann. Der Hive Warehouse-Connector vereinfacht die gemeinsame Nutzung von Spark und Hive. Die HWC-Bibliothek lädt Daten von LLAP-Daemons parallel in Spark-Executors. Dies ist effizienter und besser skalierbar als die Verwendung einer JDBC-Standardverbindung von Spark zu Hive.

![Architecture](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Einige Vorgänge, die von Hive Warehouse Connector unterstützt werden, sind:

* Beschreiben einer Tabelle
* Erstellen einer Tabelle für Daten im ORC-Format
* Auswählen von Hive-Daten und Abrufen eines Datenrahmens
* Schreiben eines Datenrahmens in Hive als Batchvorgang
* Ausführen einer Hive-Aktualisierungsanweisung
* Lesen von Tabellendaten aus Hive, Durchführen der Transformation für Spark und Schreiben der Daten in eine neue Hive-Tabelle
* Schreiben eines Datenrahmens oder Spark-Streams in Hive per HiveStreaming

## <a name="hive-warehouse-connector-setup"></a>Hive Warehouse Connector-Setup

Führen Sie diese Schritte aus, um Hive Warehouse Connector zwischen einem Spark- und einem Interactive Query-Cluster in Azure HDInsight einzurichten:

1. Erstellen Sie einen HDInsight Spark 4.0-Cluster, indem Sie ein Speicherkonto und ein benutzerdefiniertes virtuelles Azure-Netzwerk verwenden. Informationen zur Erstellung eines Clusters in einem virtuellen Azure-Netzwerk finden Sie unter [Hinzufügen von HDInsight zu einem vorhandenen virtuellen Netzwerk](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md#existingvnet).
1. Erstellen Sie einen Cluster vom Typ „HDInsight Interactive Query (LLAP) 4.0“, indem Sie im Azure-Portal dasselbe Speicherkonto und virtuelle Azure-Netzwerk wie für den Spark-Cluster verwenden.
1. Kopieren Sie den Inhalt der Datei `/etc/hosts` auf „headnode0“ Ihres Interactive Query-Clusters in die Datei `/etc/hosts` auf „headnode0“ Ihres Spark-Clusters. Durch diesen Schritt wird es ermöglicht, dass Ihr Spark-Cluster IP-Adressen der Knoten im Interactive Query-Cluster auflöst. Zeigen Sie den Inhalt der aktualisierten Datei mit `cat /etc/hosts` an. Die Ausgabe sollte etwa wie im folgenden Screenshot aussehen.

    ![Anzeigen der Datei „hosts“](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Konfigurieren Sie die Einstellungen des Spark-Clusters, indem Sie die folgenden Schritte ausführen: 
    1. Wechseln Sie zum Azure-Portal, wählen Sie „HDInsight-Cluster“ aus, und klicken Sie dann auf Ihren Clusternamen.
    1. Wählen Sie auf der rechten Seite unter **Clusterdashboards** die Option **Ambari-Homepage**.
    1. Klicken Sie auf der Ambari-Webbenutzeroberfläche auf **SPARK2** > **CONFIGS** > **Custom spark2-defaults**.

        ![Spark2 Ambari-Konfiguration](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. Legen Sie `spark.hadoop.hive.llap.daemon.service.hosts` auf den gleichen Wert wie die Eigenschaft **LLAP app name** unter **Advanced hive-interactive-env** fest. Zum Beispiel, `@llap0`

    1. Legen Sie `spark.sql.hive.hiveserver2.jdbc.url` auf die JDBC-Verbindungszeichenfolge fest, mit der eine Verbindung mit Hiveserver2 im Interactive Query-Cluster hergestellt wird. Die Verbindungszeichenfolge für Ihren Cluster sieht wie der unten angegebene URI aus. `CLUSTERNAME` ist der Name Ihres Spark-Clusters, und die Parameter `user` und `password` werden auf die richtigen Werte für Ihren Cluster festgelegt.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > Die JDBC-URL sollte die Anmeldeinformationen für die Verbindungsherstellung mit Hiveserver2 enthalten, z. B. Benutzername und Kennwort.

    1. Legen Sie `spark.datasource.hive.warehouse.load.staging.dir` auf ein geeignetes HDFS-kompatibles Stagingverzeichnis fest. Wenn Sie zwei unterschiedliche Cluster verwenden, sollte das Stagingverzeichnis ein Ordner im Stagingverzeichnis Ihres Speicherkontos im LLAP-Cluster sein, damit HiveServer2 Zugriff darauf hat. Ein Beispiel ist `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp`, wobei `STORAGE_ACCOUNT_NAME` der Name des vom Cluster verwendeten Speicherkontos und `STORAGE_CONTAINER_NAME` der Name des Speichercontainers ist.

    1. Legen Sie `spark.datasource.hive.warehouse.metastoreUri` mit dem Wert für den Metastore-URI des Interactive Query-Clusters fest. Sie können den metastoreUri für Ihren LLAP-Cluster ermitteln, indem Sie auf der Ambari-Benutzeroberfläche für Ihren LLAP-Cluster unter **Hive** > **ADVANCED** > **General** nach der **hive.metastore.uris**-Eigenschaft suchen. Der Wert sollte etwa wie der folgende URI aussehen:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Legen Sie `spark.security.credentials.hiveserver2.enabled` für den YARN-Clientbereitstellungsmodus auf `false` fest.
    1. Legen Sie `spark.hadoop.hive.zookeeper.quorum` auf das Zookeeper-Quorum Ihres LLAP-Clusters fest. Sie können das Zookeeper-Quorum für Ihren LLAP-Cluster ermitteln, indem Sie auf der Ambari-Benutzeroberfläche für Ihren LLAP-Cluster unter **Hive** > **ADVANCED** > **Advanced hive-site** nach der **hive.zookeeper.quorum**-Eigenschaft suchen. Der Wert sieht in etwa wie die folgende Zeichenfolge aus:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Führen Sie die Schritte im Abschnitt [Herstellen der Verbindung und Ausführen von Abfragen](#connecting-and-running-queries) aus, um die Konfiguration von Hive Warehouse Connector zu testen.

## <a name="using-the-hive-warehouse-connector"></a>Verwenden von Hive Warehouse Connector

### <a name="connecting-and-running-queries"></a>Herstellen der Verbindung und Ausführen von Abfragen

Sie können zwischen einigen unterschiedlichen Methoden wählen, um eine Verbindung mit Ihrem Interactive Query-Cluster herzustellen und Abfragen auszuführen, indem Sie Hive Warehouse Connector verwenden. Beispielsweise werden die folgenden Tools unterstützt:

* [spark-shell](../spark/apache-spark-shell.md)
* PySpark
* spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Alle Beispiele in diesem Artikel werden mit spark-shell ausgeführt.

Führen Sie die folgenden Schritte aus, um eine spark-shell-Sitzung zu starten:

1. Stellen Sie eine SSH-Verbindung mit dem Hauptknoten Ihres Clusters her. Informationen zum Herstellen einer SSH-Verbindung mit Ihrem Cluster finden Sie unter [Herstellen einer Verbindung mit HDInsight (Apache Hadoop) per SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Wechseln Sie in das richtige Verzeichnis, indem Sie `cd /usr/hdp/current/hive_warehouse_connector` eingeben, oder geben Sie den vollständigen Pfad zu allen JAR-Dateien an, die im spark-shell-Befehl als Parameter verwendet werden.
1. Geben Sie den folgenden Befehl ein, um spark-shell zu starten:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Eine Begrüßungsnachricht und eine `scala>`-Eingabeaufforderung zum Eingeben von Befehlen werden angezeigt.

1. Nachdem spark-shell gestartet wurde, kann mit den folgenden Befehlen eine Hive Warehouse Connector-Instanz gestartet werden:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Herstellen einer Verbindung und Ausführen von Abfragen für ESP-Cluster (Enterprise-Sicherheitspaket)

Das Enterprise-Sicherheitspaket (ESP) umfasst Unternehmensfunktionen wie Active Directory-basierte Authentifizierung, Mehrbenutzerunterstützung und rollenbasierte Zugriffssteuerung für Apache Hadoop-Cluster in Azure HDInsight. Weitere Informationen zum ESP finden Sie unter [Einführung in die Apache Hadoop-Sicherheit mit dem Enterprise-Sicherheitspaket](../domain-joined/apache-domain-joined-introduction.md).

1. Führen Sie unter [Herstellen der Verbindung und Ausführen von Abfragen](#connecting-and-running-queries) die Schritte 1 und 2 aus.
1. Geben Sie `kinit` ein, und melden Sie sich mit einem Domänenbenutzer an.
1. Starten Sie spark-shell mit der vollständigen Liste der Konfigurationsparameter. Dies ist unten dargestellt. Alle Werte in Großbuchstaben, die in spitze Klammern gesetzt sind, müssen gemäß Ihrem Cluster angegeben werden. Falls Sie die Werte ermitteln möchten, die für die unten angegebenen Parameter eingegeben werden müssen, hilft Ihnen der Abschnitt [Hive Warehouse Connector-Setup](#hive-warehouse-connector-setup) weiter:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Erstellen von Spark-Datenrahmen über Hive-Abfragen

Die Ergebnisse aller Abfragen mit Verwendung der HWC-Bibliothek werden als Datenrahmen zurückgegeben. In den folgenden Beispielen wird veranschaulicht, wie Sie eine einfache Abfrage erstellen.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Bei den Ergebnissen der Abfrage handelt es sich um Spark-Datenrahmen, die mit Spark-Bibliotheken wie MLIB und SparkSQL verwendet werden können.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Schreiben von Spark-Datenrahmen in Hive-Tabellen

Spark verfügt nicht über native Unterstützung für das Schreiben in verwaltete ACID-Tabellen von Hive. Mithilfe von HWC können Sie aber alle Datenrahmen in eine Hive-Tabelle schreiben. Diese Funktionalität wird im folgenden Beispiel veranschaulicht:

1. Erstellen Sie eine Tabelle mit dem Namen `sampletable_colorado`, und geben Sie Spalten dafür an, indem Sie den folgenden Befehl verwenden:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Filtern Sie die Tabelle `hivesampletable`, in der die Spalte `state` den Eintrag `Colorado` enthält. Diese Abfrage der Hive-Tabelle wird als Spark-Datenrahmen zurückgegeben. Der Datenrahmen wird dann mit der Funktion `write` in der Hive-Tabelle `sampletable_colorado` gespeichert.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

Die sich ergebende Tabelle ist unten im Screenshot dargestellt.

![Anzeigen der sich ergebenden Tabelle](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Schreibvorgänge per strukturiertem Stream

Mit Hive Warehouse Connector können Sie das Spark-Streaming nutzen, um Daten in Hive-Tabellen zu schreiben.

Führen Sie die unten angegebenen Schritte aus, um ein Hive Warehouse Connector-Beispiel zu erstellen, mit dem Daten aus einem Spark-Stream über den localhost-Port 9999 in einer Hive-Tabelle erfasst werden.

1. Öffnen Sie in Ihrem Spark-Cluster ein Terminal.
1. Beginnen Sie den Spark-Stream mit dem folgenden Befehl:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Generieren Sie Daten für den von Ihnen erstellten Spark-Stream, indem Sie die folgenden Schritte ausführen:
    1. Öffnen Sie in demselben Spark-Cluster ein weiteres Terminal.
    1. Geben Sie an der Eingabeaufforderung Folgendes ein: `nc -lk 9999`. Für diesen Befehl wird das Hilfsprogramm netcat verwendet, um Daten über die Befehlszeile an den angegebenen Port zu senden.
    1. Geben Sie die Wörter, die vom Spark-Stream erfasst werden sollen, gefolgt von einem Wagenrücklauf ein.
        ![Eingeben von Daten in den Spark-Stream](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Erstellen Sie eine neue Hive-Tabelle für die Streamingdaten. Geben Sie für spark-shell die folgenden Befehle ein:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Schreiben Sie die Streamingdaten mit dem folgenden Befehl in die neu erstellte Tabelle:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > Aufgrund eines bekannten Problems in Apache Spark müssen die Optionen `metastoreUri` und `database` derzeit manuell festgelegt werden. Weitere Informationen zu diesem Problem finden Sie unter [SPARK-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Sie können die in die Tabelle eingefügten Daten mit dem folgenden Befehl anzeigen:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>Schützen von Daten in Spark-ESP-Clustern

1. Erstellen Sie die Tabelle `demo` mit einigen Beispieldaten, indem Sie die folgenden Befehle eingeben:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Zeigen Sie den Inhalt der Tabelle mit dem folgenden Befehl an. Vor dem Anwenden der Richtlinie wird in der Tabelle `demo` die gesamte Spalte angezeigt.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![Tabelle „demo“ vor dem Anwenden der Ranger-Richtlinie](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Wenden Sie eine Richtlinie für die Spaltenmaskierung an, bei der nur die letzten vier Zeichen der Spalte angezeigt werden.  
    1. Navigieren Sie zur Ranger-Administratoroberfläche unter `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Klicken Sie unter **Hive** auf den Hive-Dienst für Ihren Cluster.
        ![Tabelle „demo“ vor dem Anwenden der Ranger-Richtlinie](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Klicken Sie auf die Registerkarte für die **Maskierung** und dann auf **Neue Richtlinie hinzufügen**. ![Liste mit Richtlinien](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)
    1. Geben Sie den gewünschten Namen für die Richtlinie ein. Wählen Sie Folgendes aus. Datenbank: **Standard**, Hive-Tabelle: **demo**, Hive-Spalte: **name**, Benutzer: **rsadmin2**, Zugriffstypen: **Auswählen** und **Teilmaske: Letzte 4 anzeigen** (Menü **Maskierungsfunktion auswählen**). Klicken Sie auf **Hinzufügen**.
                ![Liste mit Richtlinien](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Zeigen Sie den Inhalt der Tabelle erneut an. Nachdem die Ranger-Richtlinie angewendet wurde, werden nur die letzten vier Zeichen der Spalte angezeigt.

    ![Tabelle „demo“ nach dem Anwenden der Ranger-Richtlinie](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Interactive Query mit HDInsight](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Beispiele für die Interaktion mit Hive Warehouse Connector per Zeppelin, Livy, spark-submit und pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
