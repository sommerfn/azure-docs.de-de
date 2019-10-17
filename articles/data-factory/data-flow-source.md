---
title: 'Quelltransformation in Mapping Data Flow: Azure Data Factory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie eine Quelltransformation in Mapping Data Flow einrichten.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: c3c24e9dc674ac29c8ca4d0d445cc3f572cda71e
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029215"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Quelltransformation für Mapping Data Flow 



Mit einer Quelltransformation wird die Datenquelle für den Datenfluss konfiguriert. Beim Entwerfen von Datenflüssen ist der erste Schritt immer das Konfigurieren einer Quelltransformation. Um eine Quelle hinzuzufügen, klicken Sie im Datenfluss-Zeichenbereich auf das Feld **Quelle hinzufügen**.

Jeder Datenfluss erfordert mindestens eine Quelltransformation, aber Sie können so viele Quellen wie erforderlich hinzufügen, um Ihre Datentransformationen abzuschließen. Sie können diese Quellen zusammen mit einer Join-, Lookup- oder Union-Transformation verwenden.

Jede Quelltransformation ist genau einem Data Factory-Dataset zugeordnet. Das Dataset definiert die Form und Position der Daten, in die geschrieben oder aus denen gelesen werden soll. Wenn Sie ein dateibasiertes Dataset verwenden, können Sie Platzhalter und Dateilisten in Ihrer Quelle verwenden, um mit mehreren Dateien gleichzeitig zu arbeiten.

## <a name="supported-connectors-in-mapping-data-flow"></a>Unterstützte Connectors bei der Zuordnung des Datenflusses

Die Zuordnung des Datenflusses folgt einem Ansatz zum Extrahieren, Laden und Transformieren (ELT) und funktioniert mit *Stagingdatasets* in Azure. Derzeit können die folgenden Datasets in einer Quelltransformation verwendet werden:
    
* Azure Blob Storage
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL Data Warehouse
* Azure SQL-Datenbank

Azure Data Factory hat Zugriff auf über 80 native Connectors. Um Daten aus diesen anderen Quellen in Ihren Datenfluss einzubeziehen, verwenden Sie die Kopieraktivität, um die Daten in einen der unterstützten Stagingbereiche zu laden.

## <a name="source-settings"></a>Quelleinstellungen

Nachdem Sie eine Quelle hinzugefügt haben, konfigurieren Sie sie über die Registerkarte **Quelleinstellungen**. Hier können Sie das Dataset auswählen oder erstellen, auf das Ihre Quelle verweist. Sie können auch das Schema und die Samplingoptionen für Ihre Daten auswählen.

![Registerkarte „Quelleinstellungen“](media/data-flow/source1.png "Registerkarte „Quelleinstellungen“")

**Schemaabweichung**: [Schemaabweichung](concepts-data-flow-schema-drift.md) ist die Fähigkeit von Data Factory, flexible Schemas in Ihren Datenflüssen nativ zu verarbeiten, ohne explizit Spaltenänderungen definieren zu müssen.

* Aktivieren Sie das Kontrollkästchen **Schemaabweichung zulassen**, wenn die Quellspalten häufig geändert werden. Diese Einstellung ermöglicht es, dass alle eingehenden Quellfelder durch die Transformationen zur Senke fließen.

* Mit der Option **Abweichende Spaltentypen ableiten** wird Data Factory angewiesen, Datentypen für jede neue erkannte Spalte zu ermitteln und zu definieren. Wenn dieses Feature deaktiviert ist, weisen alle abweichende Spalten den Typ „String“ auf.

**Schema überprüfen**: Ist „Schema überprüfen“ aktiviert, führt das Ausführen des Datenflusses zu einem Fehler, wenn die eingehenden Daten nicht mit dem definierten Schema des Datasets übereinstimmen.

**Anzahl zu überspringender Zeilen**: Das Feld „Anzahl zu überspringender Zeilen“ gibt an, wie viele Zeilen am Anfang des Datasets ignoriert werden sollen.

**Stichprobenentnahme**: Aktivieren Sie „Stichprobenentnahme“, um die Anzahl der Zeilen aus der Quelle zu beschränken. Verwenden Sie diese Einstellung, wenn Sie für das Debuggen Stichproben der Daten an der Quelle erstellen möchten.

Um zu überprüfen, ob die Quelle ordnungsgemäß konfiguriert ist, aktivieren Sie den Debugmodus, und rufen Sie eine Datenvorschau ab. Weitere Informationen finden Sie unter [Debugmodus](concepts-data-flow-debug-mode.md).

> [!NOTE]
> Wenn der Debugmodus aktiviert ist, überschreibt die Zeilenlimit-Konfiguration in den Debugeinstellungen bei der Datenvorschau die Stichprobenentnahme-Einstellung in der Quelle.

## <a name="file-based-source-options"></a>Dateibasierte Quelloptionen

Wenn Sie ein dateibasiertes Dataset verwenden, wie z.B. Azure Blob Storage oder Azure Data Lake Storage, können Sie auf der Registerkarte **Quelloptionen** verwalten, wie die Quelle Dateien liest.

![Quelloptionen](media/data-flow/sourceOPtions1.png "Quelloptionen")

**Platzhalterpfad**: Mithilfe eines Platzhaltermusters wird ADF angewiesen, die einzelnen übereinstimmenden Ordner und Dateien in einer einzigen Quelltransformation zu durchlaufen. Dies ist eine effektive Methode zur Verarbeitung von mehreren Dateien in einem einzigen Datenfluss. Mit dem Pluszeichen (+), das angezeigt wird, wenn Sie mit dem Cursor auf Ihr vorhandenes Platzhaltermuster zeigen, können Sie weitere Platzhaltermuster hinzufügen.

Wählen Sie in Ihrem Quellcontainer eine Reihe von Dateien aus, die einem Muster entsprechen. Nur der Container kann im Dataset angegeben werden. Daher muss Ihr Platzhalterpfad auch den Ordnerpfad des Stammordners enthalten.

Beispiele für Platzhalter:

* ```*```: Stellt eine beliebige Zeichenfolge dar
* ```**```: Stellt rekursive Verzeichnisschachtelung dar
* ```?```: Ersetzt ein Zeichen
* ```[]```: Stimmt mit einem oder mehreren Zeichen in den Klammern überein

* ```/data/sales/**/*.csv```: Ruft alle CSV-Dateien unter „/data/sales“ ab
* ```/data/sales/20??/**```: Ruft alle Dateien aus dem 20. Jahrhundert ab
* ```/data/sales/2004/*/12/[XY]1?.csv```: Ruft alle CSV-Dateien aus Dezember 2004 ab, die mit X oder Y und einer zweistelligen Zahl als Präfix beginnen

**Partitionsstammpfad**: Wenn Ihre Dateiquelle partitionierte Ordner mit dem Format ```key=value``` (z.B. „Jahr=2019“) enthält, können Sie die oberste Ebene dieser Ordnerstruktur einem Spaltennamen in Ihrem Datenfluss-Datenstrom zuweisen.

Legen Sie zunächst einen Platzhalter fest, um darin alle Pfade, die die partitionierten Ordner sind, sowie die zu lesenden Blattdateien einzuschließen.

![Einstellungen für die Partitionsquelldatei](media/data-flow/partfile2.png "Einstellung für die Partitionsdatei")

Verwenden Sie die Einstellung „Partitionsstammpfad“, um zu definieren, was die oberste Ebene der Ordnerstruktur ist. Wenn Sie die Inhalte Ihrer Daten über die Datenvorschau anzeigen, sehen Sie, dass ADF die aufgelösten Partitionen hinzufügen wird, die auf den einzelnen Ordnerebenen gefunden werden.

![Partitionsstammpfad](media/data-flow/partfile1.png "Partitionsstammpfad – Vorschau")

**Liste der Dateien**: Dies ist eine Dateigruppe. Erstellen Sie eine Textdatei mit einer Liste der relativen Pfade der zu verarbeitenden Dateien. Verweisen Sie auf diese Textdatei.

**Spalte für die Speicherung im Dateinamen**: Speichern Sie den Namen der Quelldatei in einer Spalte in den Daten. Geben Sie hier einen neuen Spaltennamen ein, um die Zeichenfolge für den Dateinamen zu speichern.

**Nach der Fertigstellung**: Wählen Sie aus, ob Sie nach dem Ausführen des Datenflusses nichts mit der Quelldatei anstellen, die Quelldatei löschen oder die Quelldateien verschieben möchten. Die Pfade für das Verschieben sind relative Pfade.

Um Quelldateien an einen anderen Speicherort nach der Verarbeitung zu verschieben, wählen Sie zuerst für den Dateivorgang die Option „Verschieben“ aus. Legen Sie dann das Quellverzeichnis („from“/„aus“) fest. Wenn Sie keine Platzhalter für Ihren Pfad verwenden, entspricht die Einstellung „from“ dem Quellordner.

Wenn Sie über einen Quellpfad mit Platzhalter verfügen, sieht Ihre Syntax ähnlich wie hier aus:

```/data/sales/20??/**/*.csv```

Geben Sie „from“ beispielsweise wie folgt an:

```/data/sales```

Und „to“ können Sie wie folgt angeben:

```/backup/priorSales```

In diesem Fall werden alle Dateien, die aus „/Data/Sales“ erstellt wurden, in „/Backup/priorSales“ verschoben.

> [!NOTE]
> Die Dateivorgänge werden nur ausgeführt, wenn der Datenfluss anhand der Aktivität zum Ausführen des Datenflusses in einer Pipeline über eine Pipelineausführung ausgeführt wird (Debuggen der Pipeline oder Ausführung). Dateivorgänge werden *nicht* im Datenfluss-Debugmodus ausgeführt.

**Nach der letzten Änderung filtern**: Sie können einen Datumsbereich angeben, um die zu verarbeitenden Dateien nach der letzten Änderung zu filtern. Alle Zeitangaben sind in UTC. 

### <a name="add-dynamic-content"></a>Dynamischen Inhalt hinzufügen

Alle Quelleinstellungen können mithilfe der [Transformationsausdrucksprache von Mapping Data Flow](data-flow-expression-functions.md) als Ausdrücke angegeben werden. Um dynamische Inhalte hinzuzufügen, klicken oder zeigen Sie im Bereich „Einstellungen“ auf die Felder. Klicken Sie auf den Link für **Dynamischen Inhalt hinzufügen**. Dadurch wird der Ausdrucks-Generator gestartet, in dem Sie Werte dynamisch mithilfe von Ausdrücken, statischen Literalwerten oder Parametern festlegen können.

![Parameter](media/data-flow/params6.png "Parameter")

## <a name="sql-source-options"></a>Optionen für SQL-Quellen

Wenn sich Ihre Quelle in SQL-Datenbank oder SQL Data Warehouse befindet, sind auf der Registerkarte **Quelloptionen** zusätzliche SQL-spezifische Einstellungen verfügbar. 

**Eingabe**: Wählen Sie aus, ob Sie Ihre Quelle auf eine Tabelle verweisen (Äquivalent von ```Select * from <table-name>```) oder eine benutzerdefinierte SQL-Abfrage eingeben.

**Query** (Abfrage): Wenn Sie im Eingabefeld „Abfrage“ auswählen, geben Sie eine SQL-Abfrage für die Quelle ein. Diese Einstellung überschreibt jede Tabelle, die Sie im Dataset ausgewählt haben. **Order By**-Klauseln werden hier nicht unterstützt. Sie können aber eine vollständige SELECT FROM-Anweisung festlegen. Sie können auch benutzerdefinierte Tabellenfunktionen verwenden. **select * from udfGetData()** ist eine benutzerdefinierte Funktion in SQL, die eine Tabelle zurückgibt. Diese Abfrage generiert eine Quelltabelle, die Sie in Ihrem Datenfluss verwenden können.

**Batchgröße**: Geben Sie eine Batchgröße ein, um große Datenmengen in Leseblöcke zu segmentieren.

**Isolationsstufe**: Der Standardwert für SQL-Quellen in Mapping Data Flow lautet „Lesen nicht zugesichert“. Sie können die Isolationsstufe hier in einen der folgenden Werte ändern:
* Lesen zugesichert
* Lesen nicht zugesichert
* Wiederholbarer Lesevorgang
* Serialisierbar
* Keine (Isolationsstufe ignorieren)

![Isolationsstufe](media/data-flow/isolationlevel.png "Isolationsstufe")

## <a name="projection"></a>Projektion

Wie Schemas in Datasets definiert die Projektion in einer Quelle die Datenspalten, Datentypen und Datenformate aus den Quelldaten. Bei den meisten Datasettypen, z.B. SQL und Parquet, wird die Projektion in einer Quelle so festgelegt, dass sie das in einem Dataset definierte Schema widerspiegelt. Wenn Ihre Quelldateien nicht stark typisiert sind (z.B. flache CSV-Dateien im Gegensatz zu Parquet-Dateien), können Sie in der Quelltransformation die Datentypen für jedes Feld definieren.

![Einstellungen auf der Registerkarte „Projektion“](media/data-flow/source3.png "Projektion")

Wenn in Ihrer Textdatei kein Schema definiert ist, wählen Sie **Datentyp erkennen** aus, damit Data Factory Stichproben erstellt und die Datentypen ableitet. Wählen Sie **Standarddatenformat** aus, um die Standarddatenformate automatisch zu ermitteln. 

Sie können die Spaltendatentypen in einer späteren Transformation für nachgeschaltete Spalten ändern. Verwenden Sie eine Transformation, um die Spaltennamen zu ändern.

## <a name="optimize-the-source-transformation"></a>Optimieren der Quelltransformation

Auf der Registerkarte **Optimieren** für die Quelltransformation wird möglicherweise der Partitionstyp **Quelle** angezeigt. Diese Option ist nur verfügbar, wenn die Quelle eine Azure SQL-Datenbank ist. Dies liegt daran, dass Data Factory versucht, parallele Verbindungen herzustellen, um umfangreiche Abfragen für Ihre SQL-Datenbankquelle auszuführen.

![Einstellungen der Quellpartition](media/data-flow/sourcepart3.png "Partitionierung")

Sie verfügen nicht über Partitionsdaten in Ihrer SQL-Datenbank-Quelle, Partitionen sind aber besonders hilfreich für große Abfragen. Sie können Ihre Partitionierung auf eine Spalte oder einer Abfrage basieren.

### <a name="use-a-column-to-partition-data"></a>Verwenden einer Spalte zum Partitionieren von Daten

Wählen Sie in Ihrer Quelltabelle eine Spalte aus, nach der partitioniert werden soll. Legen Sie auch die Anzahl von Partitionen fest.

### <a name="use-a-query-to-partition-data"></a>Verwenden einer Abfrage zum Partitionieren von Daten

Sie können die Verbindungen basierend auf einer Abfrage partitionieren. Geben Sie den Inhalt eines WHERE-Prädikats ein. Geben Sie beispielsweise „year > 1980“ ein.

Weitere Informationen zur Optimierung in Mapping Data Flow finden Sie auf der [Registerkarte „Optimieren“](concepts-data-flow-overview.md#optimize).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem Erstellen einer [Transformation für abgeleitete Spalten](data-flow-derived-column.md) und einer [select-Transformation](data-flow-select.md).
