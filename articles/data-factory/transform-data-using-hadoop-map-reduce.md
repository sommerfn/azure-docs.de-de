---
title: Transformieren von Daten mit der Hadoop MapReduce-Aktivität in Azure Data Factory
description: Erfahren Sie, wie Sie Daten verarbeiten, indem Sie Hadoop MapReduce-Programme mithilfe einer Azure Data Factory auf einen Azure HDInsight-Cluster anwenden.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 49e00d9a47f92fb30a29e7051cba35f54bde3700
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683848"
---
# <a name="transform-data-using-hadoop-mapreduce-activity-in-azure-data-factory"></a>Transformieren von Daten mit der Hadoop MapReduce-Aktivität in Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-map-reduce.md)
> * [Aktuelle Version](transform-data-using-hadoop-map-reduce.md)

Die HDInsight MapReduce-Aktivität in einer Data Factory-[Pipeline](concepts-pipelines-activities.md) ruft ein MapReduce-Programm für [Ihren eigenen](compute-linked-services.md#azure-hdinsight-linked-service) oder [bedarfsgesteuerten](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight-Cluster auf. Dieser Artikel baut auf dem Artikel zu [Datentransformationsaktivitäten](transform-data.md) auf, der eine allgemeine Übersicht über die Datentransformation und die unterstützten Transformationsaktivitäten bietet.

Wenn Sie noch nicht mit Azure Data Factory vertraut sind, sollten Sie zunächst den Artikel [Einführung in Azure Data Factory](introduction.md) lesen und anschließend das Tutorial zum Thema [Tutorial: Transformieren von Daten](tutorial-transform-data-spark-powershell.md).

Weitere Informationen zum Ausführen von Pig-/Hive-Skripts in einem HDInsight-Cluster über eine Pipeline mithilfe von Pig-/Hive-HDInsight-Aktivitäten finden Sie unter [Pig](transform-data-using-hadoop-pig.md) und [Hive](transform-data-using-hadoop-hive.md).

## <a name="syntax"></a>Syntax

```json
{
    "name": "Map Reduce Activity",
    "description": "Description",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.myorg.SampleClass",
        "jarLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "MyAzureStorage/jars/sample.jar",
        "getDebugInfo": "Failure",
        "arguments": [
            "-SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }
}
```

## <a name="syntax-details"></a>Syntaxdetails

| Eigenschaft          | BESCHREIBUNG                              | Erforderlich |
| ----------------- | ---------------------------------------- | -------- |
| name              | Der Name der Aktivität                     | Ja      |
| description       | Ein Text, der beschreibt, wofür die Aktivität verwendet wird. | Nein       |
| type              | Für die MapReduce-Aktivität ist der Aktivitätstyp „HDInsightMapReduce“. | Ja      |
| linkedServiceName | Verweis auf den HDInsight-Cluster, der als verknüpfter Dienst in Data Factory registriert ist. Weitere Informationen zu diesem verknüpften Dienst finden Sie im Artikel [Von Azure Data Factory unterstützten Compute-Umgebungen](compute-linked-services.md). | Ja      |
| className         | Name der Klasse, die ausgeführt werden soll         | Ja      |
| jarLinkedService  | Verweis auf einen verknüpften Azure Storage-Dienst, der zum Speichern der JAR-Dateien verwendet wird. Wenn Sie diesen verknüpften Dienst nicht angeben, wird der im verknüpften HDInsight-Dienst definierte verknüpfte Azure Storage-Dienst genutzt. | Nein       |
| jarFilePath       | Geben Sie den Pfad der JAR-Dateien an, die im Azure Storage-Speicher gespeichert sind, auf den „jarLinkedService“ verweist. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Ja      |
| jarlibs           | Zeichenfolgenarray des Pfads zu der JAR-Bibliotheksdateien, die in dem Auftrag referenziert wird, der in dem Azure Storage-Speicher gespeichert ist, der in „jarLinkedService“ definiert ist. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Nein       |
| getDebugInfo      | Gibt an, ob die Protokolldateien in den Azure Storage-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von „jarLinkedService“ angegeben wird. Zulässige Werte: „None“, „Always“ oder „Failure“. Standardwert: None (Keine): | Nein       |
| arguments         | Gibt ein Array von Argumenten für einen Hadoop-Auftrag an. Die Argumente werden als Befehlszeilenargumente an jeden Vorgang übergeben. | Nein       |
| defines           | Geben Sie Parameter als Schlüssel-Wert-Paare für Verweise innerhalb des Hive-Skripts an. | Nein       |



## <a name="example"></a>Beispiel
Mit der HDInsight-Aktivität „MapReduce“ können Sie beliebige MapReduce-JAR-Dateien auf einem HDInsight-Cluster ausführen. In der folgenden JSON-Beispieldefinition einer Pipeline wird die HDInsight-Aktivität für die Ausführung einer Mahout-JAR-Datei konfiguriert.

```json
{
    "name": "MapReduce Activity for Mahout",
    "description": "Custom MapReduce to generate Mahout result",
    "type": "HDInsightMapReduce",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
        "jarLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
        "arguments": [
            "-s",
            "SIMILARITY_LOGLIKELIHOOD",
            "--input",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
            "--output",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
            "--maxSimilaritiesPerItem",
            "500",
            "--tempDir",
            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
        ]
    }
}
```
Sie können im Abschnitt **arguments** Argumente für das MapReduce-Programm angeben. Zur Laufzeit werden ein paar zusätzliche Argumente aus dem MapReduce-Framework angezeigt (z.B.: mapreduce.job.tags). Um Ihre Argumente mit den MapReduce-Argumenten zu unterscheiden, sollten Sie erwägen, sowohl Option als auch Wert als Argumente zu verwenden, wie im folgenden Beispiel gezeigt („-s“, „--input“, „--output“ usw. sind Optionen, denen ihre Werte unmittelbar folgen).

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Daten auf andere Weisen transformiert werden:

* [U-SQL-Aktivität](transform-data-using-data-lake-analytics.md)
* [Hive-Aktivität](transform-data-using-hadoop-hive.md)
* [Pig-Aktivität](transform-data-using-hadoop-pig.md)
* [Hadoop-Streamingaktivität](transform-data-using-hadoop-streaming.md)
* [Spark-Aktivität](transform-data-using-spark.md)
* [Benutzerdefinierte .NET-Aktivität](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning-Batchausführungsaktivität](transform-data-using-machine-learning.md)
* [Aktivität „Gespeicherte Prozedur“](transform-data-using-stored-procedure.md)
