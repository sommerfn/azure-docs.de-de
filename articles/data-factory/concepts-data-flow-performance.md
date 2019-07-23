---
title: Anleitung zur Leistung und Optimierung von Mapping Data Flow in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, welche Faktoren sich entscheidend auf die Leistung von Datenflüssen in Azure Data Factory auswirken, wenn Sie die Mapping Data Flow-Funktion verwenden.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: bbbc2bc5c47821469ecf15a27195b1bf0c12e6e5
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190629"
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

### <a name="file-naming-options"></a>Optionen für die Benennung von Dateien

* Die Standardmethode zum Schreiben transformierter Daten in ADF-Zuordnungsdatenflüssen besteht darin, in ein Dataset zu schreiben, das einen Blob oder verknüpften ADLS-Dienst enthält. Sie sollten dieses Dataset so festlegen, dass es auf einen Ordner oder Container verweist, nicht auf eine benannte Datei.
* Datenflüsse verwenden Azure Databricks Spark für die Ausführung. Das bedeutet, dass die Ausgabe über mehrere Dateien verteilt wird, entweder basierend auf Spark-Standardpartitionierung oder dem Partitionierungsschema, das explizit ausgewählt wurde.
* Ein sehr häufiger Vorgang in ADF-Datenflüssen ist das Auswählen von „Ausgabe in einzelne Datei“, damit alle PART-Dateien Ihrer Ausgabe in eine einzige Ausgabedatei zusammengeführt werden.
* Dieser Vorgang erfordert jedoch, dass die Ausgabe sich auf eine einzige Partition auf einem einzigen Clusterknoten beschränkt.
* Beachten Sie dies, wenn Sie diese gängige Option auswählen. Wenn Sie viele große Quelldateien in eine einzige Ausgabedateipartition kombinieren, kann dies dazu führen, dass die Clusterknotenressourcen nicht mehr ausreichen.
* Um die Ausschöpfung der Computeknotenressourcen zu vermeiden, können Sie das Standard- oder explizite Partitionierungsschema in ADF beibehalten, das auf Leistung optimiert, und dann eine nachfolgende Kopieraktivität zur Pipeline hinzufügen, durch die alle PART-Dateien des Ausgabeordners in eine neue Einzeldateien zusammengeführt werden. Diese Technik wird im Wesentlichen verwendet, um die Aktion der Transformation von der Dateizusammenführung zu trennen, erzielt aber dasselbe Ergebnis wie das Festlegen der „Ausgabe in eine einzelne Datei“.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die folgenden Artikel zu Datenflüssen in Bezug auf die Leistung:

- [Registerkarte „Optimieren“ für Datenflüsse](concepts-data-flow-optimize-tab.md)
- [Datenflussaktivität](control-flow-execute-data-flow-activity.md)
- [Überwachen der Datenflussleistung](concepts-data-flow-monitoring.md)
