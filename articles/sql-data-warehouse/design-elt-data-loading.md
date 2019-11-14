---
title: Entwerfen von ELT anstelle von ETL
description: Entwerfen Sie einen Extrahieren, Transformieren und Laden (ETL)- anstelle eines ETL-Prozesses zum Herunterladen von Daten oder für Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load-data
ms.date: 11/07/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 9220bf0cf94eaae6ddc945e83deac2a6041158d2
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748507"
---
# <a name="data-loading-strategies-for-azure-sql-data-warehouse"></a>Datenladestrategien für Azure SQL Data Warehouse

In herkömmlichen SMP-Data Warehouses wird zum Laden von Daten ein ETL-Prozess (Extrahieren, Transformieren und Laden) verwendet. Azure SQL Data Warehouse ist eine Architektur mit massiver Parallelverarbeitung (MPP), die die Vorteile der Skalierbarkeit und Flexibilität von Compute-und Speicherressourcen nutzt. Ein ETL-Prozess (Extrahieren, Transformieren und Laden) kann die Vorteile von MPP nutzen und Ressourcen entfernen, die vor dem Laden zum Transformieren der Daten erforderlich sind. SQL Data Warehouse unterstützt zwar zahlreiche Lademethoden (unter anderem beliebte SQL Server-Optionen wie BCP und die SQL-BulkCopy-API), die schnellste und am besten skalierbare Möglichkeit zum Laden von Daten stellen jedoch externe PolyBase-Tabellen und die [COPY-Anweisung](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Vorschauversion) dar.  Mit PolyBase und der COPY-Anweisung können Sie über die T-SQL-Sprache auf externe Daten zugreifen, die in Azure Blob Storage oder in Azure Data Lake Storage gespeichert sind. Wir empfehlen die Verwendung der COPY-Anweisung, um beim Laden von Daten in SQL Data Warehouse so flexibel wie möglich zu sein. 

> [!NOTE]  
> Die COPY-Anweisung befindet sich momentan in der Public Preview-Phase. Wenn Sie Feedback abgeben möchten, senden Sie eine E-Mail an die folgende Verteilerliste: sqldwcopypreview@service.microsoft.com.
>
        
 
> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]


## <a name="what-is-elt"></a>Was ist ELT?

ELT (Extrahieren, Laden und Transformieren) ist ein Prozess, bei dem Daten aus einem Quellsystem extrahiert, in ein Data Warehouse geladen und anschließend transformiert werden. 

Die Implementierung von ELT für SQL Data Warehouse umfasst folgende grundlegende Schritte:

1. Extrahieren Sie die Quelldaten in Textdateien.
2. Legen Sie die Daten in Azure Blob Storage oder Azure Data Lake Store ab.
3. Bereiten Sie die Daten für das Laden vor.
4. Laden Sie die Daten mithilfe von PolyBase oder per COPY-Befehl in SQL Data Warehouse-Stagingtabellen. 
5. Transformieren Sie die Daten.
6. Fügen Sie die Daten in Produktionstabellen ein.


Ein Tutorial für das Laden mit PolyBase finden Sie unter [Verwenden von PolyBase zum Laden von Daten aus Azure Blob Storage in Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Weitere Informationen finden Sie im Blog [Lademuster](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/). 


## <a name="1-extract-the-source-data-into-text-files"></a>1. Extrahieren der Quelldaten in Textdateien

Das Abrufen von Daten aus dem Quellsystem hängt vom Speicherort ab.  Das Ziel besteht darin, die Daten in durch Trennzeichen getrennte Text- oder CSV-Dateien zu verschieben, die von PolyBase und der COPY-Anweisung unterstützt werden. 

### <a name="polybase-and-copy-external-file-formats"></a>Externe PolyBase- und COPY-Dateiformate

Mit PolyBase und der COPY-Anweisung können Sie Daten aus UTF-8- und UTF-16-codierten, durch Trennzeichen getrennten Text- oder CSV-Dateien laden. Neben durch Trennzeichen getrennten Text- oder CSV-Dateien können auch Daten auch aus Hadoop-Dateiformaten wie ORC oder Parquet geladen werden. Mit PolyBase und der COPY-Anweisung können auch Daten aus Dateien geladen werden, die mit Gzip und Snappy komprimiert wurden. Erweitertes ASCII, Formate mit fester Breite und geschachtelte Formate wie WinZip oder XML werden nicht unterstützt. Beim Exportieren aus SQL Server können Sie die Daten mit dem [Befehlszeilentool bcp](/sql/tools/bcp-utility?view=azure-sqldw-latest) in durch Trennzeichen getrennte Textdateien exportieren. 

## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Laden der Daten in Azure Blob Storage oder Azure Data Lake Storage

Wenn Sie Daten in Azure Storage platzieren möchten, können Sie sie in [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) oder in [Azure Data Lake Store Gen2](../data-lake-store/data-lake-store-overview.md) verschieben. In beiden Fällen sollten die Daten in Textdateien gespeichert werden. PolyBase und die COPY-Anweisung können Daten von beiden Orten laden.

Tools und Dienste, mit denen Sie Daten in Azure Storage verschieben können:

- Der [Azure ExpressRoute](../expressroute/expressroute-introduction.md)-Dienst verbessert Netzwerkdurchsatz, Leistung und Vorhersagbarkeit. ExpressRoute ist ein Dienst, der Ihre Daten über eine dedizierte private Verbindung zu Azure weiterleitet. Bei ExpressRoute-Verbindungen werden Daten nicht über das öffentliche Internet weitergeleitet. Die Verbindungen bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit, niedrigere Latenzzeiten und mehr Sicherheit als herkömmliche Verbindungen über das öffentliche Internet.
- Das Hilfsprogramm [AZCopy](../storage/common/storage-moving-data.md) verschiebt Daten über das öffentliche Internet in Azure Storage. Dies funktioniert, wenn Ihre Datenmengen weniger als 10 TB umfassen. Wenn Sie Ladevorgänge in regelmäßigen Abständen mit AZCopy ausführen möchten, testen Sie die Netzwerkgeschwindigkeit, um festzustellen, ob sie geeignet ist. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) verfügt über ein Gateway, das Sie auf dem lokalen Server installieren können. Anschließend können Sie eine Pipeline erstellen, um Daten vom lokalen Server in Azure Storage zu verschieben. Informationen zur Verwendung der Data Factory mit SQL Data Warehouse finden Sie unter [Laden von Daten in Azure SQL Data Warehouse mit Azure Data Factory](/azure/data-factory/load-azure-sql-data-warehouse).


## <a name="3-prepare-the-data-for-loading"></a>3. Vorbereiten der Daten für das Laden

Möglicherweise müssen Sie die Daten in Ihrem Speicherkonto vorbereiten und bereinigen, bevor Sie sie in SQL Data Warehouse laden. Die Datenvorbereitung kann durchgeführt werden, während sich Ihre Daten in der Quelle befinden, während Sie die Daten in Textdateien exportieren oder nachdem sich die Daten in Azure Storage befinden.  Am einfachsten ist es, so früh wie möglich im Prozess mit den Daten zu arbeiten.  

### <a name="define-external-tables"></a>Definieren externer Tabellen

Bei Verwendung von PolyBase müssen vor dem Laden externe Tabellen in Ihrem Data Warehouse definiert werden. Bei Verwendung der COPY-Anweisung sind keine externen Tabellen erforderlich. PolyBase verwendet externe Tabellen, um Daten in Azure Storage zu definieren und auf diese zuzugreifen. Eine externe Tabelle ähnelt einer Datenbanksicht. Die externe Tabelle enthält das Tabellenschema und verweist auf Daten, die außerhalb des Data Warehouse gespeichert sind. 

Die Definition externer Tabellen umfasst die Angabe der Datenquelle, des Formats der Textdateien und der Tabellendefinitionen. Folgende T-SQL-Syntaxthemen sind für Sie relevant:
- [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?view=azure-sqldw-latest)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql?view=azure-sqldw-latest)

Beim Laden von Parquet lautet die Datentypzuordnung mit SQL DW wie folgt:

| **Parquet-Datentyp** |                      **SQL-Datentyp**                       |
| :-------------------: | :----------------------------------------------------------: |
|        tinyint        |                           tinyint                            |
|       smallint        |                           smallint                           |
|          int          |                             int                              |
|        bigint         |                            bigint                            |
|        boolean        |                             bit                              |
|        double         |                            float                             |
|         float         |                             real                             |
|        double         |                            money                             |
|        double         |                          smallmoney                          |
|        Zeichenfolge         |                            nchar                             |
|        Zeichenfolge         |                           nvarchar                           |
|        Zeichenfolge         |                             char                             |
|        Zeichenfolge         |                           varchar                            |
|        binary         |                            binary                            |
|        binary         |                          varbinary                           |
|       timestamp       |                             date                             |
|       timestamp       |                        smalldatetime                         |
|       timestamp       |                          datetime2                           |
|       timestamp       |                           datetime                           |
|       timestamp       |                             time                             |
|       date            |                             date                             |
|        decimal        |                            decimal                           |

Ein Beispiel für die Erstellung externer Objekte finden Sie im Schritt [Erstellen externer Tabellen](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) im Tutorial zu Ladevorgängen.

### <a name="format-text-files"></a>Formatieren von Textdateien

Bei Verwendung von PolyBase müssen die definierten externen Objekte die Zeilen der Textdateien mit der externen Tabelle und der Definition des Dateiformats abgleichen. Die Daten in den einzelnen Zeilen der Textdatei müssen mit der Tabellendefinition übereinstimmen.
So formatieren Sie die Textdateien

- Wenn Ihre Daten aus einer nicht relationalen Quelle stammen, müssen Sie sie in Zeilen und Spalten transformieren. Unabhängig davon, ob die Daten aus einer relationalen oder nicht relationalen Quelle stammen, müssen die Daten so transformiert werden, dass sie mit den Spaltendefinitionen der Tabelle übereinstimmen, in die die Daten geladen werden sollen. 
- Formatieren Sie die Daten in der Textdatei so, dass sie mit den Spalten und Datentypen in der SQL Data Warehouse-Zieltabelle übereinstimmen. Eine Nichtübereinstimmung zwischen den Datentypen in den externen Textdateien und der Data Warehouse-Tabelle führt dazu, dass Zeilen beim Laden zurückgewiesen werden.
- Trennen Sie Felder in der Textdatei mit einem Abschlusszeichen.  Stellen Sie sicher, dass Sie ein Zeichen oder eine Zeichenfolge verwenden, die nicht in Ihren Quelldaten enthalten ist. Verwenden Sie das durch [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql) angegebene Abschlusszeichen.


## <a name="4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase-or-the-copy-statement"></a>4. Laden der Daten in SQL Data Warehouse-Stagingtabellen mithilfe von PolyBase oder per COPY-Anweisung

Es hat sich bewährt, die Daten in eine Stagingtabelle zu laden. Stagingtabellen ermöglichen das Behandeln von Fehlern, ohne die Produktionstabellen zu beeinträchtigen. Eine Stagingtabelle bietet Ihnen außerdem die Möglichkeit, Datentransformationen mit SQL Data Warehouse-MPP auszuführen, bevor Daten in Produktionstabellen eingefügt werden. Die Tabelle muss vorab erstellt werden, wenn Daten mithilfe von COPY in eine Stagingtabelle geladen werden.

### <a name="options-for-loading-with-polybase-and-copy-statement"></a>Optionen für das Laden per PolyBase und COPY-Anweisung

Um Daten mit PolyBase zu laden, können Sie eine der folgenden Ladeoptionen nutzen:

- [PolyBase mit T-SQL](load-data-from-azure-blob-storage-using-polybase.md) funktioniert gut, wenn sich Ihre Daten in Azure Blob Storage oder Azure Data Lake Store befinden. Es bietet Ihnen die größtmögliche Kontrolle über den Ladevorgang, erfordert aber auch die Definition externer Datenobjekte. Die anderen Methoden definieren diese Objekte im Hintergrund, wenn Sie Quelltabellen zu Zieltabellen zuordnen.  Zum Orchestrieren von T-SQL-Ladevorgängen können Sie Azure Data Factory-, SSIS- oder Azure-Funktionen verwenden. 
- [PolyBase mit SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) funktioniert gut, wenn sich die Quelldaten in SQL Server befinden (entweder lokal in SQL Server oder in der Cloud). SSIS definiert die Zuordnung von Quell- zu Zieltabellen und orchestriert zudem die Workload. Wenn Sie bereits über SSIS-Pakete verfügen, können Sie die Pakete so ändern, dass sie mit dem neuen Data Warehouse-Ziel funktionieren. 
- [PolyBase und COPY-Anweisung mit Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) ist ein weiteres Orchestrierungstool.  Es definiert eine Pipeline und plant Aufträge. 
- [PolyBase mit Azure Databricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) überträgt Daten aus einer SQL Data Warehouse-Tabelle in einen Databricks-Datenrahmen und/oder schreibt Daten aus einem Databricks-Datenrahmen in eine SQL Data Warehouse-Tabelle, die PolyBase verwendet.

### <a name="other-loading-options"></a>Weitere Ladeoptionen

Neben PolyBase und der COPY-Anweisung können Sie auch [bcp](/sql/tools/bcp-utility?view=azure-sqldw-latest) oder die [SQLBulkCopy-API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx) verwenden. Mit „bcp“ werden Daten direkt in SQL Data Warehouse geladen, ohne Azure Blob Storage zu durchlaufen. Daher ist es nur für kleine Workloads konzipiert. Beachten Sie, dass die Ladeleistung dieser Optionen geringer ist als bei PolyBase und der COPY-Anweisung. 


## <a name="5-transform-the-data"></a>5. Transformieren der Daten

Führen Sie während der Bereitstellung der Daten in der Stagingtabelle entsprechende Transformationen durch, die für Ihre Workload erforderlich sind. Anschließend verschieben Sie die Daten in eine Produktionstabelle.


## <a name="6-insert-the-data-into-production-tables"></a>6. Einfügen der Daten in Produktionstabellen

Die SELECT-Anweisung „INSERT INTO...“ verschiebt die Daten aus der Stagingtabelle in die permanente Tabelle. 

Versuchen Sie beim Entwerfen eines ETL-Prozesses, den Prozess für ein kleines Testbeispiel auszuführen. Versuchen Sie, 1000 Zeilen aus der Tabelle in eine Datei zu extrahieren, sie nach Azure zu verschieben und sie dann in eine Stagingtabelle zu laden. 


## <a name="partner-loading-solutions"></a>Ladelösungen von Partnern

Viele unserer Partner stellen Ladelösungen bereit. Weitere Informationen finden Sie in der Liste mit unseren [Lösungspartnern](sql-data-warehouse-partner-business-intelligence.md). 


## <a name="next-steps"></a>Nächste Schritte

Eine Anleitung zum Laden finden Sie unter [Anleitung zum Laden von Daten](guidance-for-loading-data.md).


