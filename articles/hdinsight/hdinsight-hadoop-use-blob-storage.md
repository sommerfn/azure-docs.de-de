---
title: 'Abfragen von Daten aus HDFS-kompatiblem Azure-Speicher: Azure HDInsight'
description: Es wird beschrieben, wie Sie Daten in Azure-Speicher und Azure Data Lake Storage abfragen, um die Ergebnisse Ihrer Analyse zu speichern.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: 1e115c59cab4c340f927da516b5f937abf42e985
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73839661"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Verwenden von Azure Storage mit Azure HDInsight-Clustern

Zum Analysieren von Daten im HDInsight-Cluster können Sie die Daten in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) oder einer Kombination dieser beiden speichern. Diese Speichervarianten ermöglichen das sichere Löschen von HDInsight-Clustern, die für Berechnungen verwendet werden, ohne dass Benutzerdaten verloren gehen.

Apache Hadoop unterstützt eine Variante des Standarddateisystems. Das Standarddateisystem gibt ein Standardschema und eine Standardautorität vor. Es kann auch zur Auflösung relativer Pfade verwendet werden. Bei der Erstellung des HDInsight-Clusters können Sie einen Blobcontainer in Azure Storage als Standarddateisystem angeben. Mit HDInsight 3.6 können Sie Azure Storage oder Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen2 als Standarddateisystem auswählen (mit einigen Ausnahmen). Informationen zur Unterstützung von Data Lake Storage Gen1 als Standardspeicher und als verknüpfter Speicher finden Sie unter [Verfügbarkeit für HDInsight-Cluster](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

In diesem Artikel erfahren Sie, wie Azure Storage mit HDInsight-Clustern funktioniert. Informationen zur Funktionsweise von Data Lake Storage Gen1 mit HDInsight-Clustern finden Sie unter [Verwenden von Azure Data Lake Storage mit Azure HDInsight-Clustern](hdinsight-hadoop-use-data-lake-store.md). Weitere Informationen zum Erstellen eines HDInsight-Clusters finden Sie unter [Erstellen von Apache Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!IMPORTANT]  
> Speicherkonten vom Typ **BlobStorage** können nur als sekundärer Speicher für HDInsight-Cluster verwendet werden.

| Art des Speicherkontos | Unterstützte Dienste | Unterstützte Leistungsstufen | Unterstützte Zugriffsebenen |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (universell v2)  | Blob     | Standard                    | Heiß, Kalt, Archiv\*   |
| Speicher (general-purpose v1)   | Blob     | Standard                    | –                    |
| BlobStorage                    | Blob     | Standard                    | Heiß, Kalt, Archiv\*   |

Die Verwendung des Standard-Blobcontainers zum Speichern von Geschäftsdaten wird nicht empfohlen. Stattdessen empfiehlt es sich, den Standard-Blobcontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Der Standardcontainer enthält Anwendungs- und Systemprotokolle. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

Das Freigeben eines Blobcontainers als Standarddateisystem für mehrere Cluster wird nicht unterstützt.

> [!NOTE]  
> Die Archivspeicherebene ist eine Offlineebene, die eine Abrufwartezeit von mehreren Stunden aufweist, und wird für die Verwendung mit HDInsight nicht empfohlen. Weitere Informationen finden Sie unter [Zugriffsebene „Archiv“](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

## <a name="access-files-from-the-cluster"></a>Zugreifen auf Dateien aus dem Cluster

Es gibt mehrere Möglichkeiten, wie Sie auf die Dateien in Data Lake Storage über einen HDInsight-Cluster zugreifen können. Das URI-Schema bietet unverschlüsselten Zugriff (mit dem Präfix *wasb:* ) wie auch SSL-verschlüsselten Zugriff (mit *wasbs*). Wir empfehlen die Verwendung von *wasbs* , und zwar auch für den Zugriff auf Daten, die sich in Azure in derselben Region befinden.

* **Verwenden des vollqualifizierten Namens** Bei diesem Ansatz geben Sie den vollständigen Pfad zu der Datei an, auf die Sie zugreifen möchten.

    ```
    wasb://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    wasbs://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    ```

* **Verwenden des verkürzten Pfadformats** Bei diesem Ansatz ersetzen Sie den Pfad bis zum Clusterstamm durch:

    ```
    wasb:///<file.path>/
    wasbs:///<file.path>/
    ```

* **Verwenden des relativen Pfads** Bei diesem Ansatz geben Sie nur den relativen Pfad zu der Datei an, auf die Sie zugreifen möchten.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Beispiele für Datenzugriff

Die Beispiele basieren auf einer [ssh-Verbindung](./hdinsight-hadoop-linux-use-ssh-unix.md) mit dem Hauptknoten des Clusters. In den Beispielen werden alle drei URI-Schemas verwendet. Ersetzen Sie `CONTAINERNAME` und `STORAGEACCOUNT` durch die entsprechenden Werte.

#### <a name="a-few-hdfs-commands"></a>Ein paar HDFS-Befehle

1. Erstellen einer einfachen Datei im lokalen Speicher.

    ```bash
    touch testFile.txt
    ```

1. Erstellen von Verzeichnissen im Clusterspeicher.

    ```bash
    hdfs dfs -mkdir wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -mkdir wasbs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Kopieren von Daten aus dem lokalen Speicher in den Clusterspeicher.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  wasbs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Auflisten von Verzeichnisinhalten im Clusterspeicher.

    ```bash
    hdfs dfs -ls wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -ls wasbs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

> [!NOTE]  
> Wenn Blobs außerhalb von HDInsight verwendet werden, wird das WASB-Format von den meisten Dienstprogrammen nicht erkannt. Diese erwartet vielmehr ein grundlegendes Pfadformat wie `example/jars/hadoop-mapreduce-examples.jar`.

#### <a name="creating-a-hive-table"></a>Erstellen einer Hive-Tabelle

Zu Zwecken der Veranschaulichung werden drei Speicherorte gezeigt. Verwenden Sie für die tatsächliche Ausführung nur einen der `LOCATION`-Einträge.

```hql
DROP TABLE myTable;
CREATE EXTERNAL TABLE myTable (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE
LOCATION 'wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/example/data/';
LOCATION 'wasbs:///example/data/';
LOCATION '/example/data/';
```

## <a name="identify-storage-path-from-abmari"></a>Identifizieren des Speicherpfads von Abmari

* Um den vollständigen Pfad zum konfigurierten Standardspeicher zu ermitteln, navigieren Sie zu:

    **HDFS** > **Configs**, und geben Sie in das Filtereingabefeld `fs.defaultFS` ein.

* Um zu überprüfen, ob der WASB-Speicher als sekundärer Speicher konfiguriert ist, navigieren Sie zu:

    **HDFS** > **Configs**, und geben Sie in das Filtereingabefeld `blob.core.windows.net` ein.

## <a name="blob-containers"></a>Blobcontainer

Um Blobs zu verwenden, erstellen Sie zuerst ein Azure-[Speicherkonto](../storage/common/storage-create-storage-account.md). Hierzu liegen Sie eine Azure-Region fest, in der das Speicherkonto erstellt wird. Cluster und Speicherkonto müssen sich in der gleichen Region befinden. Die SQL Server-Datenbanken für den Hive- und Apache Oozie-Metastore müssen sich ebenfalls in der gleichen Region befinden.

Ein Blob gehört unabhängig davon, wo es sich befindet, stets zu einem Container Ihres Azure-Speicherkontos. Dieser Container kann ein außerhalb von HDInsight erstelltes Blob sein, oder es handelt sich um einen Container, der für einen HDInsight-Cluster erstellt wird.

Der standardmäßige Blobcontainer speichert clusterspezifische Informationen wie etwa Auftragsverlauf und Protokolle. Geben Sie einen Standard-Blob-Container nicht für mehrere HDInsight-Cluster frei. Dies kann zu einer Beschädigung des Auftragsverlaufs führen. Es wird empfohlen, unterschiedliche Container für die einzelnen Cluster zu verwenden und freigegebene Daten nicht im Standardspeicherkonto, sondern in einem verknüpften Speicherkonto abzulegen, das in der Bereitstellung aller relevanten Cluster angegeben wird. Weitere Informationen zum Konfigurieren verknüpfter Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md). Einen Standardspeichercontainer können Sie jedoch auch wiederverwenden, wenn der ursprüngliche HDInsight-Cluster gelöscht wurde. Bei HBase-Clustern können Sie das HBase-Tabellenschema sowie die darin enthaltenen Daten sogar beibehalten, indem Sie einen neuen HBase-Cluster mit dem Standardblobcontainer erstellen, der von einem gelöschten HBase-Cluster verwendet wurde.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Interaktion mit Azure-Speicher

Microsoft bietet die folgenden Tools für die Arbeit mit Azure Storage:

| Tool | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure-Befehlszeilenschnittstelle](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Verwenden zusätzlicher Speicherkonten

Beim Erstellen eines HDInsight-Clusters geben Sie das Azure Storage-Konto an, dem Sie es zuordnen möchten. Zusätzlich zu diesem Speicherkonto können Sie während des Erstellungsprozesses oder nach der Erstellung eines Clusters weitere Speicherkonten aus dem gleichen oder einem anderen Azure-Abonnement hinzufügen. Informationen zum Hinzufügen zusätzlicher Speicherkonten finden Sie unter [Erstellen von HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde beschrieben, wie Sie HDFS-kompatiblen Azure Storage mit HDInsight verwenden. Dadurch können Sie skalierbare Datenerfassungslösungen mit langfristiger Archivierung aufbauen und HDInsight verwenden, um die Informationen innerhalb der gespeicherten strukturierten und unstrukturierten Daten zu entsperren.

Weitere Informationen finden Sie unter

* [Erste Schritte mit Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Erste Schritte mit Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Hochladen von Daten in HDInsight](hdinsight-upload-data.md)
* [Verwenden von Apache Hive mit HDInsight](hadoop/hdinsight-use-hive.md)
* [Verwenden von Azure Storage Shared Access Signatures zum Einschränken des Zugriffs auf Daten mit HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Verwenden von Azure Data Lake Storage Gen2 mit Azure HDInsight-Clustern](hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Tutorial: Extrahieren, Transformieren und Laden von Daten mithilfe von Interactive Query in Azure HDInsight](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
