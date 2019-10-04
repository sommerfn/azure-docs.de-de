---
title: Anleitung zur Leistung und Optimierung von Mapping Data Flow in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, welche Faktoren sich entscheidend auf die Leistung von Datenflüssen in Azure Data Factory auswirken, wenn Sie die Mapping Data Flow-Funktion verwenden.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 09/22/2019
ms.openlocfilehash: e4b3e08c0cc7fc1ead2aed551c228c6a1165c3b6
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180861"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory Mapping Data Flow bietet eine codefreie Browserschnittstelle zum Entwerfen, Bereitstellen und Orchestrieren von Datentransformationen in beliebigen Größenordnungen.

> [!NOTE]
> Falls Sie mit dem grundlegenden Konzept von ADF Mapping Data Flow noch nicht vertraut sind, lesen Sie unter [Datenflüsse – Übersicht](concepts-data-flow-overview.md) nach, bevor Sie sich mit diesem Artikel beschäftigen.
>

> [!NOTE]
> Stellen Sie beim Entwerfen und Testen von Datenflüssen auf der Benutzeroberfläche von ADF sicher, dass das Debuggen aktiviert ist, damit Sie Ihre Datenflüsse in Echtzeit ausführen können, ohne auf die Aktivierung eines Clusters warten zu müssen.
>

![Schaltfläche „Debuggen“](media/data-flow/debugb1.png "Debuggen")

## <a name="monitor-data-flow-performance"></a>Überwachen der Datenflussleistung

Beim Entwerfen Ihrer Zuordnungsdatenflüsse im Browser können Sie jede einzelne Transformation Komponententests unterziehen, indem Sie für jede Transformation auf die Datenvorschauregisterkarte im Einstellungsbereich unten klicken. Als nächstes sollten Sie Ihren Datenfluss im Pipeline-Designer von Ende zu Ende testen. Fügen Sie eine Aktivität zum Ausführen der Datenflussaktivität (Execute Data Flow) aus, und verwenden Sie die Schaltfläche „Debug“ zum Testen der Leistung des Datenflusses. Im unteren Bereich des Pipelinefensters wird unter den Aktionen ein Brillensymbol angezeigt:

![Datenflussmonitor](media/data-flow/mon002.png "Datenflussmonitor 2")

Wenn Sie auf das Symbol klicken, werden der Ausführungsplan und das nachfolgende Leistungsprofil des Datenflusses angezeigt. Anhand dieser Informationen können Sie die Leistung des Datenflusses für Datenquellen verschiedener Größen schätzen. Beachten Sie, dass Sie für die Zeit zur Einrichtung der Clusterauftragsausführung in Ihren Berechnungen der Gesamtleistung von 1 Minute ausgehen können. Wenn Sie die Standard-Azure Integration Runtime verwenden, müssen Sie möglicherweise weitere 5 Minuten für das Hochfahren des Clusters hinzurechnen.

![Datenflussüberwachung](media/data-flow/mon003.png "Datenflussmonitor 3")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Optimierung für Azure SQL-Datenbank und Azure SQL Data Warehouse

![Bereich für die Quelle](media/data-flow/sourcepart3.png "Bereich für die Quelle")

### <a name="partition-your-source-data"></a>Partitionieren der Quelldaten

* Navigieren Sie zu „Optimieren“, und wählen Sie „Quelle“ aus. Legen Sie eine bestimmte Spalte oder einen Typ in einer Abfrage fest.
* Wenn Sie „Spalte“ ausgewählt haben, wählen Sie noch die Partitionsspalte aus.
* Legen Sie auch die maximale Anzahl von Verbindungen mit Ihrer Azure SQL-Datenbank-Instanz fest. Sie können eine höhere Einstellung testen, um parallele Verbindungen mit Ihrer Datenbank nutzen zu können. In einigen Fällen erreichen Sie jedoch mit einer begrenzten Anzahl von Verbindungen eine höhere Leistung.
* Ihre Quelldatenbanktabellen müssen nicht partitioniert werden.
* Indem Sie in Ihrer Quelltransformation eine Abfrage festlegen, die dem Partitionierungsschema der Datenbanktabelle entspricht, ermöglichen Sie der Quelldatenbank-Engine die Nutzung der Partitionsentfernung.
* Ist Ihre Quelle nicht bereits partitioniert, wird von ADF die Datenpartitionierung in der Spark-Transformationsumgebung weiterhin basierend auf dem in der Quelltransformation ausgewählten Schlüssel verwendet.

### <a name="set-batch-size-and-query-on-source"></a>Festlegen der Batchgröße und Abfragen der Quelle

![Quelle](media/data-flow/source4.png "Quelle")

* Durch das Festlegen der Batchgröße weisen Sie ADF an, Daten im Speicher und nicht zeilenweise zu speichern. Dies ist eine optionale Einstellung. Sie könnte dazu führen, dass Sie nicht genügend Ressourcen auf den Serverknoten haben, wenn diese nicht ordnungsgemäß dimensioniert sind.
* Das Festlegen einer Abfrage ermöglicht Ihnen das Filtern der Zeilen direkt in der Quelle – bevor Sie überhaupt den Datenfluss für die Verarbeitung erreichen. Dies kann die anfängliche Datenerfassung beschleunigen.
* Wenn Sie eine Abfrage verwenden, können Sie optional Abfragehinweise für Ihre Azure SQL-Datenbank-Instanz hinzufügen, d. h. READ UNCOMMITTED.

### <a name="set-isolation-level-on-source-transformation-settings-for-sql-datasets"></a>Festlegen der Isolationsstufe unter den Einstellungen der Quelltransformation für SQL-Datasets

* READ UNCOMMITTED bietet schnellere Abfrageergebnisse zur Quelltransformation.

![Isolationsstufe](media/data-flow/isolationlevel.png "Isolationsstufe")

### <a name="set-sink-batch-size"></a>Festlegen der Batchgröße für die Senke

![Senke](media/data-flow/sink4.png "Senke")

* Um die zeilenweise Verarbeitung Ihrer Datenflüsse zu vermeiden, legen Sie die „Batchgröße“ in den Senkeneinstellungen für Azure SQL-Datenbank fest. Dadurch wird ADF angewiesen, Schreibvorgänge in der Datenbank in Batches basierend auf der angegebenen Größe zu verarbeiten.

### <a name="set-partitioning-options-on-your-sink"></a>Festlegen von Partitionierungsoptionen für die Senke

* Auch wenn Sie Ihre Daten in den Azure SQL-Datenbank-Zieltabellen nicht partitionieren, sollten Sie die Partitionierung auf der Registerkarte „Optimieren“ festlegen.
* Häufig führt schon die Festlegung der Roundrobin-Partitionierung in ADF dazu, dass Ladevorgänge in den Spark-Ausführungsclustern sehr viel schneller ablaufen, ohne dass alle Verbindungen von einem einzelnen Knoten bzw. einer Partition stammen müssen.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Vergrößern der Compute-Engine in Azure Integration Runtime

![Neue IR](media/data-flow/ir-new.png "Neue IR")

* Erhöhen Sie die Anzahl der Kerne. Damit erhöhen Sie auch die Anzahl der Knoten, sodass Ihnen mehr Verarbeitungsleistung zum Abfragen und Schreiben in Ihrer Azure SQL-Datenbank-Instanz zur Verfügung steht.
* Versuchen Sie auch, die Optionen „Compute-optimiert“ und „Arbeitsspeicheroptimiert“ auf zusätzliche Ressourcen für Ihre Serverknoten anzuwenden.

### <a name="unit-test-and-performance-test-with-debug"></a>Komponententests und Leistungstests mit Debuggen

* Wenn Komponententests für Datenflüsse durchgeführt werden, legen Sie die Schaltfläche "Data Flow Debug" (Datenfluss debuggen) auf ON (EIN) fest.
* Verwenden Sie im Datenfluss-Designer die Registerkarte „Data Preview“ (Datenvorschau) für Transformationen zum Anzeigen der Ergebnisse Ihrer Transformationslogik.
* Führen Sie Komponententests für Ihre Datenflüsse aus der Pipeline-Designer aus, indem Sie eine Datenflussaktivität in den Pipelineentwurfscanvas platzieren und die Schaltfläche „Debug“ zum Starten des Tests verwenden.
* Das Testen im Debugmodus funktioniert für eine aktive, warmgelaufene Clusterumgebung, ohne auf das Just-In-Time-Hochfahren des Clusters warten zu müssen.
* Während des Debuggens der Datenvorschau innerhalb der Umgebung des Datenfluss-Designers können Sie die Datenmenge, mit der Sie die einzelnen Quellen testen, begrenzen, indem Sie den Zeilengrenzwert über den Link zu den Debugeinstellungen auf der Benutzeroberfläche des Datenfluss-Designers festlegen. Beachten Sie, dass Sie zuerst den Debugmodus aktivieren müssen.

![Debugeinstellungen](media/data-flow/debug-settings.png "Debugeinstellungen")

* Wenn Sie Ihre Datenflüsse über die Debugausführung für die Pipeline testen, können Sie die Anzahl der für den Test verwendeten Zeilen begrenzen, indem Sie die Samplinggröße für jede Ihrer Quellen festlegen. Achten Sie darauf, dass Sie das Sampling deaktivieren, wenn Sie Ihre Pipelines nach einem regelmäßigen, operationalisierten Zeitplan planen.

![Zeilensampling](media/data-flow/source1.png "Zeilensampling")

### <a name="disable-indexes-on-write"></a>Deaktivieren von Indizes beim Schreiben
* Verwenden Sie vor der Datenflussaktivität eine Aktivität mit einer gespeicherten Prozedur in Ihrer ADF-Pipeline, die Indizes in Ihren Zieltabellen deaktiviert, in die von der Senke aus geschrieben wird.
* Fügen Sie nach der Datenflussaktivität eine weitere Aktivität mit einer gespeicherten Prozedur hinzu, die diese Indizes aktiviert.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Erhöhen der Größe Ihrer Azure SQL-Datenbank-Instanz
* Planen Sie die Anpassung der Größe Ihrer Quelle und Senke in Azure SQL-Datenbank vor der Ausführung Ihrer Pipeline, um den Durchsatz zu erhöhen und die Drosselung durch Azure beim Erreichen der DTU-Grenzwerte zu minimieren.
* Nach Abschluss der Pipelineausführung können Sie die Größe Ihrer Datenbanken für die normale Ausführung wiederherstellen.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Optimierung für Azure SQL Data Warehouse

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Verwenden eines Stagingprozesses zum Laden von Daten per Massenvorgang über Polybase

* Um eine zeilenweise Verarbeitung Ihrer Datenflüsse zu vermeiden, legen Sie die Option „Staging“ in den Einstellungen für die Senke so fest, dass ADF Polybase nutzen kann, um zeilenweise Einfügungen in Data Warehouse zu vermeiden. Dadurch wird ADF angewiesen, Polybase zu verwenden, damit Daten in einem Massenvorgang geladen werden können.
* Beim Ausführen Ihrer Datenflussaktivität aus einer Pipeline bei aktiviertem Staging müssen Sie den Blob-Speicherort für Ihre Stagingdaten für das Massenladen auswählen.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Erhöhen der Größe Ihrer Azure SQL DW-Instanz

* Planen Sie die Anpassung der Größe Ihrer Quelle und Senke in Azure SQL DW vor der Ausführung Ihrer Pipeline, um den Durchsatz zu erhöhen und die Drosselung durch Azure beim Erreichen der DWU-Grenzwerte zu minimieren.

* Nach Abschluss der Pipelineausführung können Sie die Größe Ihrer Datenbanken für die normale Ausführung wiederherstellen.

## <a name="optimize-for-files"></a>Optimieren für Dateien

* Sie können steuern, wie viele Partitionen von ADF verwendet werden. Für jede Quelle-und-Senke-Transformation sowie jede einzelne Transformation können Sie ein Partitionierungsschema festlegen. Für kleinere Dateien stellen Sie möglicherweise fest, dass das Auswählen von „Einzelne Partition“ manchmal besser und schneller funktioniert, als Spark anzuweisen, Ihre kleinen Dateien zu partitionieren.
* Wenn Sie nicht über genügend Informationen zu Ihren Quelldaten verfügen, können Sie „Round-Robin“-Partitionierung auswählen und die Anzahl der Partitionen festlegen.
* Wenn Sie Ihre Daten untersuchen und feststellen, dass Sie über Spalten verfügen, die sich gut als Hashschlüssel eignen, verwenden Sie die Option für die Hashpartitionierung.
* Beachten Sie beim Debuggen in der Datenvorschau sowie beim Debuggen der Pipeline, dass das Limit und die Stichprobengrößen für dateibasierte Quelldatasets nur für die Anzahl zurückgegebener Zeilen gelten (nicht für die Anzahl gelesener Zeilen). Dieser Punkt ist wichtig, da er sich auf die Leistung der Debugausführungen auswirken und ggf. dazu führen kann, dass der Ablauf nicht erfolgreich ist.
* Zur Erinnerung: Debugcluster sind standardmäßig kleine Cluster mit einem einzelnen Knoten. Verwenden Sie daher zum Debuggen temporäre kleine Dateien. Navigieren Sie zu den Debugeinstellungen, und verweisen Sie unter Verwendung einer temporären Datei auf eine kleine Teilmenge Ihrer Daten.

![Debugeinstellungen](media/data-flow/debugsettings3.png "Debugeinstellungen")

### <a name="file-naming-options"></a>Optionen für die Benennung von Dateien

* Die Standardmethode zum Schreiben transformierter Daten in ADF-Zuordnungsdatenflüssen besteht darin, in ein Dataset zu schreiben, das einen Blob oder verknüpften ADLS-Dienst enthält. Sie sollten dieses Dataset so festlegen, dass es auf einen Ordner oder Container verweist, nicht auf eine benannte Datei.
* Datenflüsse verwenden Azure Databricks Spark für die Ausführung. Das bedeutet, dass die Ausgabe über mehrere Dateien verteilt wird, entweder basierend auf Spark-Standardpartitionierung oder dem Partitionierungsschema, das explizit ausgewählt wurde.
* Ein sehr häufiger Vorgang in ADF-Datenflüssen ist das Auswählen von „Ausgabe in einzelne Datei“, damit alle PART-Dateien Ihrer Ausgabe in eine einzige Ausgabedatei zusammengeführt werden.
* Dieser Vorgang erfordert jedoch, dass die Ausgabe sich auf eine einzige Partition auf einem einzigen Clusterknoten beschränkt.
* Beachten Sie dies, wenn Sie diese gängige Option auswählen. Wenn Sie viele große Quelldateien in eine einzige Ausgabedateipartition kombinieren, kann dies dazu führen, dass die Clusterknotenressourcen nicht mehr ausreichen.
* Um die Ausschöpfung der Computeknotenressourcen zu vermeiden, können Sie das Standard- oder explizite Partitionierungsschema in ADF beibehalten, das auf Leistung optimiert, und dann eine nachfolgende Kopieraktivität zur Pipeline hinzufügen, durch die alle PART-Dateien des Ausgabeordners in eine neue Einzeldateien zusammengeführt werden. Diese Technik wird im Wesentlichen verwendet, um die Aktion der Transformation von der Dateizusammenführung zu trennen, erzielt aber dasselbe Ergebnis wie das Festlegen der „Ausgabe in eine einzelne Datei“.

### <a name="looping-through-file-lists"></a>Durchlaufen von Dateilisten

In den meisten Fällen werden Datenflüsse in ADF besser über eine Pipeline ausgeführt, die der Datenfluss-Quelltransformation das Durchlaufen mehrerer Dateien ermöglicht. Anders ausgedrückt: Es empfiehlt sich, innerhalb Ihrer Quelle Platzhalter oder Dateilisten im Datenfluss zu verwenden, anstatt in der Pipeline mittels „ForEach“ eine umfangreiche Dateiliste zu durchlaufen und bei jeder Iteration eine Aktivität zum Ausführen des Datenflusses aufzurufen. Der Datenflussprozess wird schneller ausgeführt, wenn das Durchlaufen innerhalb des Datenflusses erfolgen kann.

Ein Beispiel: Angenommen, in einem Ordner in Blob Storage befindet sich eine Liste mit zu verarbeitenden Datendateien vom Juli 2019. In diesem Fall ist es sinnvoller, einmalig in der Pipeline eine Aktivität zum Ausführen des Datenflusses aufzurufen und in der Quelle einen Platzhalter zu verwenden:

```DateFiles/*_201907*.txt```

Dadurch erzielen Sie eine bessere Leistung als bei einem Suchvorgang für den Blobspeicher in einer Pipeline, bei der dann alle entsprechenden Dateien mittels „ForEach“ mit einer eingeschlossenen Aktivität zum Ausführen des Datenflusses durchlaufen werden.

### <a name="increase-the-size-of-your-debug-cluster"></a>Erhöhen der Größe Ihres Debugclusters

Standardmäßig wird beim Aktivieren des Debugmodus die standardmäßige Azure Integration Runtime verwendet, die automatisch für jede Data Factory erstellt wird. Diese standardmäßige Azure Integration Runtime ist auf acht Kerne festgelegt, d. h. vier für einen Treiberknoten und vier für einen Workerknoten, wobei die allgemeinen Eigenschaften für „Compute allgemein“ verwendet werden. Beim Testen mit größeren Datenmengen können Sie Ihren Debugcluster vergrößern, indem Sie eine neue Azure Integration Runtime mit umfangreicheren Konfigurationen erstellen und diese neue Azure Integration Runtime auswählen, wenn Sie das Debuggen aktivieren. Dadurch wird ADF angewiesen, diese Azure Integration Runtime für die Datenvorschau und das Debuggen der Pipeline mit Datenflüssen zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die folgenden Artikel zu Datenflüssen in Bezug auf die Leistung:

- [Registerkarte „Optimieren“ für Datenflüsse](concepts-data-flow-optimize-tab.md)
- [Datenflussaktivität](control-flow-execute-data-flow-activity.md)
- [Überwachen der Datenflussleistung](concepts-data-flow-monitoring.md)
