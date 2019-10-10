---
title: Problembehandlung für Hadoop Distributed File System in Azure HDInsight
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zum Arbeiten mit HDFS und Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 09/30/2019
ms.custom: seodec18
ms.openlocfilehash: 1c5d9f665c9b3e7a439a09f4259f304f8f8b1a0a
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71718326"
---
# <a name="troubleshoot-apache-hadoop-hdfs-by-using-azure-hdinsight"></a>Problembehandlung für Apache Hadoop HDFS mit Azure HDInsight

Lernen Sie die wichtigsten Probleme und ihre Lösungen bei der Arbeit mit HDFS-Nutzlasten (Hadoop Distributed File System) in Apache Ambari kennen. Eine vollständige Liste der Befehle finden Sie im [HDFS Commands Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html) und im [File System Shell Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html).

## <a name="how-do-i-access-local-hdfs-from-inside-a-cluster"></a>Wie greife ich aus einem Cluster heraus auf das lokale HDFS zu?

### <a name="issue"></a>Problem

Greifen Sie von der Befehlszeile und aus dem Anwendungscode auf das lokale HDFS zu, und nicht mithilfe des Azure Blob Storage oder des Azure Data Lake Storage aus dem HDInsight-Cluster.

### <a name="resolution-steps"></a>Lösungsschritte

1. Verwenden Sie an der Eingabeaufforderung `hdfs dfs -D "fs.default.name=hdfs://mycluster/" ...` buchstäblich wie im folgenden Befehl:

    ```output
    hdfs dfs -D "fs.default.name=hdfs://mycluster/" -ls /
    Found 3 items
    drwxr-xr-x   - hdiuser hdfs          0 2017-03-24 14:12 /EventCheckpoint-30-8-24-11102016-01
    drwx-wx-wx   - hive    hdfs          0 2016-11-10 18:42 /tmp
    drwx------   - hdiuser hdfs          0 2016-11-10 22:22 /user
    ```

2. Verwenden Sie den URI `hdfs://mycluster/` im Quellcode buchstäblich wie in der folgenden Beispielanwendung:

    ```Java
    import java.io.IOException;
    import java.net.URI;
    import org.apache.commons.io.IOUtils;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.*;

    public class JavaUnitTests {

        public static void main(String[] args) throws Exception {

            Configuration conf = new Configuration();
            String hdfsUri = "hdfs://mycluster/";
            conf.set("fs.defaultFS", hdfsUri);
            FileSystem fileSystem = FileSystem.get(URI.create(hdfsUri), conf);
            RemoteIterator<LocatedFileStatus> fileStatusIterator = fileSystem.listFiles(new Path("/tmp"), true);
            while(fileStatusIterator.hasNext()) {
                System.out.println(fileStatusIterator.next().getPath().toString());
            }
        }
    }
    ```

3. Führen Sie die kompilierte JAR-Datei (z.B. mit dem Namen `java-unit-tests-1.0.jar`) auf dem HDInsight-Cluster mit dem folgenden Befehl aus:

    ```apache
    hadoop jar java-unit-tests-1.0.jar JavaUnitTests
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.info
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.lck
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.info
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.lck
    ```

## <a name="du"></a>du

Mit dem [-du](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#du)-Befehl wird die Größe der Dateien und Verzeichnisse angezeigt, die im angegebenen Verzeichnis enthalten sind, oder die Länge einer Datei, falls es sich nur um eine Datei handelt.

Mit der Option `-s` wird eine aggregierte Zusammenfassung der angezeigten Dateilängen erzeugt.  
Mit der Option `-h` werden die Dateigrößen formatiert.

Beispiel:

```bash
hdfs dfs -du -s -h hdfs://mycluster/
hdfs dfs -du -s -h hdfs://mycluster/tmp
```

## <a name="rm"></a>rm

Mit dem [-rm](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#rm)-Befehl werden als Argumente angegebene Dateien gelöscht.

Beispiel:

```bash
hdfs dfs -rm hdfs://mycluster/tmp/testfile
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
