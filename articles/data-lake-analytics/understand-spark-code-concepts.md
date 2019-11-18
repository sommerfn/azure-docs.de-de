---
title: Grundlegendes zu Apache Spark-Codekonzepten für Azure Data Lake Analytics-U-SQL-Entwickler.
description: In diesem Artikel werden Apache Spark-Konzepte beschrieben, um U-SQL-Entwicklern zu helfen, Spark-Codekonzepte zu verstehen.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: Understand-apache-spark-code-concepts
ms.date: 10/15/2019
ms.openlocfilehash: 4ed23beae6edb13efabf034c1e87b9cb76048f82
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648175"
---
# <a name="understand-apache-spark-code-for-u-sql-developers"></a>Grundlegendes zu Apache Spark-Code für U-SQL-Entwickler

Dieser Abschnitt bietet allgemeine Informationen zum Transformieren von U-SQL-Skripts zu Apache Spark.

- Er beginnt mit einem [Vergleich der Verarbeitungsparadigmen der beiden Sprachen](#understand-the-u-sql-and-spark-language-and-processing-paradigms).
- Zudem enthält er Tipps zu Folgendem:
   - [Transformieren von Skripts](#transform-u-sql-scripts) einschließlich der [Rowsetausdrücke](#transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions) von U-SQL
   - [.NET-Code](#transform-net-code)
   - [Datentypen](#transform-typed-values)
   - [Katalogobjekte](#transform-u-sql-catalog-objects)

## <a name="understand-the-u-sql-and-spark-language-and-processing-paradigms"></a>Grundlegende Informationen zu den Sprach- und Verarbeitungsparadigmen von U-SQL und Spark

Bevor Sie mit dem Migrieren von U-SQL-Skripts für Azure Data Lake Analytics zu Spark beginnen, sollten Sie die allgemeinen Sprach- und Verarbeitungsphilosophien der beiden Systeme kennen.

U-SQL ist eine deklarative Abfragesprache, die SQL ähnelt und auf einem Datenflussparadigma basiert. U-SQL ermöglicht es, Benutzercode, der in .NET (z. B. C#), Python oder R geschrieben wurde, ganz einfach einzubetten und zu erweitern. Benutzererweiterungen können einfache Ausdrücke oder benutzerdefinierte Funktionen implementieren, dem Benutzer aber auch die Möglichkeit bieten, so genannte benutzerdefinierte Operatoren zu implementieren, die wiederum angepasste Operatoren implementieren, um Transformationen und Extraktionen auf Rowsetebene auszuführen und Ausgaben zu schreiben.

Spark ist ein erweiterbares Framework, das verschiedene Sprachbindungen in Scala, Java, Python, .NET usw. bietet. Damit können Sie Code in einer dieser Sprachen schreiben, als Resilient Distributed Datasets (RDD) bezeichnete Datenabstraktionen sowie Datenrahmen und Datasets erstellen und dann eine LINQ-ähnliche domänenspezifische Sprache (Domain-Specific Language, DSL) verwenden, um diese zu transformieren. Spark bietet zudem SparkSQL als deklarative Untersprache für die Datenrahmen- und Datasetabstraktionen. Die DSL bietet zwei Kategorien von Vorgängen: Transformationen und Aktionen. Durch Anwenden von Transformationen auf die Datenabstraktionen wird die Transformation nicht ausgeführt. Stattdessen wird der Ausführungsplan erstellt, der zur Auswertung mit einer Aktion übermittelt wird (z. B. durch Schreiben des Ergebnisses in eine temporäre Tabelle oder Datei oder durch Ausgeben des Ergebnisses).

Daher müssen Sie beim Übersetzen eines U-SQL-Skripts in ein Spark-Programm entscheiden, welche Sprache Sie zum Generieren der Datenrahmenabstraktion (die zurzeit am häufigsten genutzte Datenabstraktion) verwenden und ob Sie die deklarativen Datenflusstransformationen mithilfe von DSL oder SparkSQL schreiben möchten. In einigen komplexeren Fällen müssen Sie Ihr U-SQL-Skript möglicherweise in eine Spark-Sequenz sowie andere Schritte aufteilen, die mit Azure Batch oder Azure Functions implementiert werden.

Darüber hinaus bietet Azure Data Lake Analytics U-SQL in einer serverlosen Auftragsdienstumgebung an, während sowohl Azure Databricks als auch Azure HDInsight Spark in Form eines Clusterdiensts anbieten. Beim Transformieren Ihrer Anwendung müssen Sie jetzt die Auswirkungen durch die Erstellung, Größenänderung, Skalierung und Außerbetriebnahme von Clustern berücksichtigen.

## <a name="transform-u-sql-scripts"></a>Transformieren von U-SQL-Skripts

Bei U-SQL-Skripts wird das folgende Verarbeitungsmuster angewendet:

1. Daten werden entweder unter Verwendung der `EXTRACT`-Anweisung, der Angabe eines Speicherorts oder Dateisatzes und mit dem integrierten oder benutzerdefinierten Extractor und Schema aus unstrukturierten Dateien oder aber aus U-SQL-Tabellen (verwaltete oder externe Tabellen) gelesen. Die Daten werden als Rowset dargestellt.
2. Die Rowsets werden in mehrere U-SQL-Anweisungen transformiert, die U-SQL-Ausdrücke auf die Rowsets anwenden und neue Rowsets erzeugen.
3. Schließlich werden die resultierenden Rowsets entweder mit der `OUTPUT`-Anweisung, die den Speicherort und einen integrierten oder benutzerdefinierten Ausgabeoperator angibt, in Dateien oder aber in eine U-SQL-Tabelle ausgegeben.

Das Skript wird verzögert ausgewertet. Das bedeutet, dass jeder Extraktions- und Transformationsschritt in eine Ausdrucksbaumstruktur eingefügt und global ausgewertet wird (der Datenfluss).

Spark-Programme funktionieren ähnlich wie Spark-Connectors, die Sie verwenden, um Daten zu lesen und Datenrahmen zu erstellen, die Transformationen mithilfe der LINQ-ähnlichen DSL oder von SparkSQL auf die Datenrahmen anzuwenden und dann das Ergebnis in Dateien, temporäre Spark-Tabellen, einige Arten von Programmiersprachen oder die Konsole zu schreiben.

## <a name="transform-net-code"></a>Transformieren von .NET-Code

Die Ausdruckssprache von U-SQL ist C# – diese bietet eine Reihe von Möglichkeiten zum Erweitern von benutzerdefiniertem .NET-Code.

Da Spark die Ausführung von .NET-Code derzeit nicht nativ unterstützt, müssen Sie entweder Ihre Ausdrücke als entsprechende Spark-, Scala-, Java- oder Python-Ausdrücke neu schreiben oder eine Möglichkeit finden, Aufrufe in Ihrem .NET-Code auszuführen. Wenn Ihr Skript .NET-Bibliotheken verwendet, stehen Ihnen folgende Optionen zur Verfügung:

- Übersetzen Sie Ihren .NET-Code in Scala oder Python.
- Teilen Sie Ihr U-SQL-Skript in verschiedene Schritte auf, in denen Sie Azure Batch-Prozesse verwenden, um die .NET-Transformationen anzuwenden (falls eine akzeptable Skalierung möglich ist).
- Verwenden Sie eine in Open Source verfügbare .NET-Sprachbindung namens Moebius. Dieses Projekt befindet sich nicht in einem unterstützten Zustand.

Auf jeden Fall gilt Folgendes: Wenn in Ihren U-SQL-Skripts sehr viel .NET-Logik vorhanden ist, wenden Sie sich an Ihren Microsoft-Kundenbetreuer, um weitere Informationen zu erhalten.

Die folgenden Details gelten für die verschiedenen Fälle der Verwendung von .NET und C# in U-SQL-Skripts.

### <a name="transform-scalar-inline-u-sql-c-expressions"></a>Transformieren von C#-Inline-Skalarausdrücken in U-SQL

Die Ausdruckssprache von U-SQL ist C#. Viele der U-SQL-Inline-Skalarausdrücke werden zur Leistungsverbesserung nativ implementiert, komplexere Ausdrücke dagegen können durch Aufrufe im .NET-Framework ausgeführt werden.

Spark verfügt über eine eigene skalare Ausdruckssprache (entweder als Teil der DSL oder in SparkSQL) und ermöglicht das Aufrufen von benutzerdefinierten Funktionen, die in der zugehörigen Hostsprache geschrieben sind.

Wenn Sie über Skalarausdrücke in U-SQL verfügen, sollten Sie zuerst den am besten geeigneten nativ verstandenen Spark-Skalarausdruck ermitteln, um eine optimale Leistung zu erzielen. Ordnen Sie danach die anderen Ausdrücke einer benutzerdefinierten Funktion der Spark-Hostsprache Ihrer Wahl zu.

Beachten Sie, dass .NET und C# eine andere Typsemantik verwenden als die Spark-Hostsprachen und die Spark-DSL. Informationen zu den Unterschieden in den Typsystemen finden Sie weiter [unten](#transform-typed-values).

### <a name="transform-user-defined-scalar-net-functions-and-user-defined-aggregators"></a>Transformieren von benutzerdefinierten .NET-Skalarfunktionen und benutzerdefinierten Aggregatoren

U-SQL bietet Möglichkeiten zum Aufrufen von beliebigen .NET-Skalarfunktionen und zum Aufrufen von in .NET geschriebenen benutzerdefinierten Aggregatoren.

Spark bietet auch Unterstützung für benutzerdefinierte Funktionen und benutzerdefinierte Aggregatoren der meisten Hostsprachen, die aus der Spark-DSL und aus SparkSQL aufgerufen werden können.

### <a name="transform-user-defined-operators-udos"></a>Transformieren von benutzerdefinierten Operatoren

U-SQL bietet verschiedene Kategorien benutzerdefinierter Operatoren (User-Defined Operators, UDOs) zum Extrahieren (Extractors), Ausgeben (Outputters), Reduzieren (Reducers), Verarbeiten (Processors), Anwenden (Appliers) und Kombinieren (Combiners), die in .NET (und bis zu einem gewissen Grad auch in Python und R) geschrieben werden können.

Spark bietet nicht dasselbe Erweiterbarkeitsmodell für Operatoren, enthält aber einige äquivalente Funktionen für einige Operatoren.

Das Spark-Äquivalent zu Extractors und Outputters sind Spark-Connectors. In der Spark-Community finden Sie äquivalente Connectors für viele U-SQL-Extractors. Für andere müssen Sie einen benutzerdefinierten Connector schreiben. Wenn der U-SQL-Extractor komplex ist und verschiedene .NET-Bibliotheken verwendet, ist es möglicherweise sinnvoller, einen Connector in Scala zu erstellen, der dank Interoperabilität Aufrufe in der .NET-Bibliothek ausführen kann, die die tatsächliche Verarbeitung der Daten übernimmt. In diesem Fall müssen Sie die .NET Core-Runtime im Spark-Cluster bereitstellen und sicherstellen, dass die referenzierten .NET-Bibliotheken mit dem .NET Standard 2.0 konform sind.

Die anderen Arten von U-SQL-UDOs müssen unter Verwendung der benutzerdefinierten Funktionen und Aggregatoren sowie des semantisch geeigneten Spark-DSL- oder SparkSQL-Ausdrucks neu geschrieben werden. Ein Prozessor kann beispielsweise der SELECT-Anweisung verschiedener UDF-Aufrufe zugeordnet und als Funktion gepackt werden, die einen Datenrahmen als Argument akzeptiert und einen Datenrahmen zurückgibt.

### <a name="transform-u-sqls-optional-libraries"></a>Transformieren von optionalen U-SQL-Bibliotheken

U-SQL stellt eine Reihe optionaler Bibliotheken und Demobibliotheken bereit, die Unterstützung für [Python](data-lake-analytics-u-sql-python-extensions.md), [R](data-lake-analytics-u-sql-r-extensions.md), [JSON, XML und AVRO](https://github.com/Azure/usql/tree/master/Examples/DataFormats) sowie einige [Cognitive Services-Funktionen](data-lake-analytics-u-sql-cognitive.md) bieten.

Spark bietet eine eigene Python- und R-Integration – pySpark bzw. SparkR – sowie Connectors zum Lesen und Schreiben von JSON, XML und AVRO.

Wenn Sie ein Skript transformieren müssen, das auf die Cognitive Services-Bibliothek verweist, empfehlen wir, sich an Ihren Microsoft-Kundenbetreuer zu wenden, um Unterstützung zu erhalten.

## <a name="transform-typed-values"></a>Transformieren von typisierten Werten

Da das Typsystem von U-SQL auf dem .NET-Typsystem basiert und Spark über ein eigenes Typsystem verfügt, das durch die Bindung der Hostsprache beeinflusst wird, müssen Sie sicherstellen, dass Sie mit möglichst ähnlichen Typen arbeiten. Beachten Sie, dass Typbereiche, Präzision und/oder Skalierung bei bestimmten Typen leicht abweichen können. Darüber hinaus behandeln U-SQL und Spark `null`-Werte unterschiedlich.

### <a name="data-types"></a>Datentypen

Die folgende Tabelle zeigt die äquivalenten Typen in Spark, Scala und PySpark für die jeweiligen U-SQL-Typen.

| U-SQL | Spark |  Scala | PySpark |
| ------ | ------ | ------ | ------ |
|`byte`       ||||
|`sbyte`      |`ByteType` |`Byte` | `ByteType`|
|`int`        |`IntegerType` |`Int` | `IntegerType`|
|`uint`       ||||
|`long`       |`LongType` |`Long` | `LongType`|
|`ulong`      ||||
|`float`      |`FloatType` |`Float` | `FloatType`|
|`double`     |`DoubleType` |`Double` | `DoubleType`|
|`decimal`    |`DecimalType` |`java.math.BigDecimal` | `DecimalType`|
|`short`      |`ShortType` |`Short` | `ShortType`|
|`ushort`     ||||
|`char`   | |`Char`||
|`string` |`StringType` |`String` |`StringType` |
|`DateTime`   |`DateType`, `TimestampType` |`java.sql.Date`, `java.sql.Timestamp` | `DateType`, `TimestampType`|
|`bool`   |`BooleanType` |`Boolean` | `BooleanType`|
|`Guid`   ||||
|`byte[]` |`BinaryType` |`Array[Byte]` | `BinaryType`|
|`SQL.MAP<K,V>`   |`MapType(keyType, valueType, valueContainsNull)` |`scala.collection.Map` | `MapType(keyType, valueType, valueContainsNull=True)`|
|`SQL.ARRAY<T>`   |`ArrayType(elementType, containsNull)` |`scala.collection.Seq` | `ArrayType(elementType, containsNull=True)`|

Weitere Informationen finden Sie unter

- [org.apache.spark.sql.types](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.types.package)
- [Spark SQL- und DataFrames-Typen](https://spark.apache.org/docs/latest/sql-reference.html#data-types)
- [Scala-Werttypen](https://www.scala-lang.org/api/current/scala/AnyVal.html)
- [pyspark.sql.types](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#module-pyspark.sql.types)

### <a name="treatment-of-null"></a>Behandlung von NULL

In Spark lassen Typen NULL-Werte standardmäßig zu, wohingegen Sie in U-SQL skalare Elemente, bei denen es sich nicht um Objekte handelt, explizit als „Nullwerte zulassend“ kennzeichnen müssen. In Spark können Sie zwar eine Spalte als „keine Nullwerte zulassend“ definieren, aber die Einschränkung wird nicht erzwungen, was zu [falschen Ergebnissen führen kann](https://medium.com/@weshoffman/apache-spark-parquet-and-troublesome-nulls-28712b06f836).

In Spark bedeutet NULL, dass der Wert unbekannt ist. Ein NULL-Wert in Spark unterscheidet sich von jedem anderen Wert, einschließlich sich selbst. Vergleiche zwischen zwei Spark-NULL-Werten oder zwischen einem NULL- und einem anderen Wert geben „unbekannt“ zurück, weil der Wert jedes NULL-Elements unbekannt ist.  

Dieses Verhalten unterscheidet sich von U-SQL – diese Sprache folgt der C#-Semantik, bei der `null` sich von allen anderen Werten unterscheidet und nur sich selbst entspricht.  

Daher gibt in SparkSQL eine `SELECT`-Anweisung, die `WHERE column_name = NULL` verwendet, 0 Zeilen zurück, auch wenn in `column_name` NULL-Werte enthalten sind. In U-SQL dagegen gibt diese Anweisung die Zeilen zurück, in denen `column_name` auf `null` festgelegt ist. Ebenso gibt in Spark eine `SELECT`-Anweisung, die `WHERE column_name != NULL` verwendet, 0 Zeilen zurück, auch wenn in `column_name` Nicht-NULL-Werte enthalten sind. In U-SQL dagegen gibt diese Anweisung die Zeilen zurück, die Nicht-NULL-Werte enthalten. Wenn Sie die U-SQL-Semantik zur Überprüfung auf NULL verwenden möchten, müssen Sie daher [isnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnull) bzw. [isnotnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnotnull) (oder das jeweilige DSL-Äquivalent) verwenden.

## <a name="transform-u-sql-catalog-objects"></a>Transformieren von U-SQL-Katalogobjekten

Ein wesentlicher Unterschied besteht darin, dass U-SQL-Skripts Katalogobjekte verwenden können, von denen viele kein direktes Äquivalent in Spark aufweisen.

Spark bietet Unterstützung für die Hive-Metaspeicherkonzepte – hauptsächlich Datenbanken und Tabellen –, daher können Sie U-SQL-Datenbanken und -Schemas zu Hive-Datenbanken und U-SQL-Tabellen zu Spark-Tabellen zuordnen (siehe [Verschieben von in U-SQL-Tabellen gespeicherten Daten](understand-spark-data-formats.md#move-data-stored-in-u-sql-tables)). Sichten, Tabellenwertfunktionen (Table-Valued Functions, TVFs), gespeicherte Prozeduren, U-SQL-Assemblys, externe Datenquellen usw. werden dagegen nicht unterstützt.

U-SQL-Codeobjekte wie Sichten, TVFs, gespeicherte Prozeduren und Assemblys können mit Codefunktionen und Bibliotheken in Spark modelliert werden. Verweise auf diese Objekte erfolgen über die Funktions- und Prozedurabstraktionsmechanismen der Hostsprache (beispielweise durch Importieren von Python-Modulen oder Verweisen auf Scala-Funktionen).

Wenn der U-SQL-Katalog zum Freigeben von Daten- und Codeobjekten für Projekte und Teams dient, müssen entsprechende Mechanismen für die Freigabe verwendet werden (z. B. Maven für Freigabe von Codeobjekten).

## <a name="transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions"></a>Transformieren von U-SQL-Rowsetausdrücken und SQL-basierten Skalarausdrücken

Die Kernsprache von U-SQL transformiert Rowsets und basiert auf SQL. Im Folgenden finden Sie eine nicht vollständige Liste der gängigsten Rowsetausdrücke in U-SQL:

- `SELECT`/`FROM`/`WHERE`/`GROUP BY`+Aggregates+`HAVING`/`ORDER BY`+`FETCH`
- `INNER`/`OUTER`/`CROSS`/`SEMI` `JOIN`-Ausdrücke
- `CROSS`/`OUTER` `APPLY`-Ausdrücke
- `PIVOT`/`UNPIVOT`-Ausdrücke
- `VALUES`-Rowsetkonstruktor

- Festgelegte Ausdrücke `UNION`/`OUTER UNION`/`INTERSECT`/`EXCEPT`

Darüber hinaus bietet U-SQL eine Vielzahl SQL-basierter Skalarausdrücke wie diese:

- `OVER`-Fensterausdrücke
- Eine Vielzahl integrierter Aggregatoren und Rangfolgefunktionen (`SUM`, `FIRST` usw.)
- Einige der bekanntesten SQL-Skalarausdrücke: `CASE`, `LIKE`, (`NOT`) `IN`, `AND`, `OR` usw.

Spark bietet sowohl in der DSL- als auch in der SparkSQL-Variante Äquivalente für die meisten dieser Ausdrücke. Einige der in Spark nicht nativ unterstützten Ausdrücke müssen mithilfe einer Kombination aus den nativen Spark-Ausdrücken und den semantisch äquivalenten Mustern neu geschrieben werden. `OUTER UNION` beispielsweise muss in die entsprechende Kombination aus Projektionen und Vereinigungen übersetzt werden.

Aufgrund der unterschiedlichen Behandlung von NULL-Werten gleicht ein Joinvorgang in U-SQL immer eine Zeile ab, wenn beide verglichenen Spalten einen NULL-Wert enthalten. Ein Joinvorgang in Spark dagegen gleicht solche Spalten nicht ab, es sei denn, es werden explizite NULL-Überprüfungen hinzugefügt.

## <a name="transform-other-u-sql-concepts"></a>Transformieren anderer U-SQL-Konzepte

U-SQL bietet eine Reihe weiterer Features und Konzepte – z.B. Verbundabfragen in SQL Server-Datenbanken, Parameter, Skalar- und Lambdaausdruckvariablen, Systemvariablen oder `OPTION`-Hinweise.

### <a name="federated-queries-against-sql-server-databasesexternal-tables"></a>Verbundabfragen in SQL Server-Datenbanken bzw. externen Tabellen

U-SQL stellt Datenquellen und externe Tabellen bereit und ermöglicht direkte Abfragen von Azure SQL-Datenbank. Spark bietet zwar nicht die gleichen Objektabstraktionen, stellt aber den [Spark-Connector für Azure SQL-Datenbank](../sql-database/sql-database-spark-connector.md) bereit, der zum Abfragen von SQL-Datenbanken verwendet werden kann.

### <a name="u-sql-parameters-and-variables"></a>U-SQL-Parameter und -Variablen

Parameter und Benutzervariablen weisen in Spark und den zugehörigen Hostsprachen die gleichen Konzepte auf.

In Scala können Sie beispielsweise eine Variable mit dem Schlüsselwort `var` definieren:

```
var x = 2 * 3;
println(x)
```

Die Systemvariablen von U-SQL (Variablen, die mit `@@` beginnen) können in zwei Kategorien unterteilt werden:

- Festlegbare Systemvariablen, die auf bestimmte Werte festgelegt werden können, um das Skriptverhalten zu beeinflussen
- Informative Systemvariablen, die Informationen auf System- und Auftragsebene abfragen

Die meisten festlegbaren Systemvariablen weisen kein direktes Äquivalent in Spark auf. Einige der informativen Systemvariablen können durch Übergeben der Informationen als Argumente bei der Auftragsausführung modelliert werden, andere weisen eine äquivalente Funktion in der Hostsprache von Spark auf.

### <a name="u-sql-hints"></a>U-SQL-Hinweise

U-SQL bietet verschiedene syntaktische Möglichkeiten, Hinweise für den Abfrageoptimierer und die Ausführungs-Engine bereitzustellen:  

- Festlegen einer U-SQL-Systemvariable
- Eine `OPTION`-Klausel, die dem Rowsetausdruck zugeordnet ist, um einen Daten- oder Planhinweis zu liefern
- Ein Joinhinweis in der Syntax des Joinausdrucks (z. B. `BROADCASTLEFT`)

Der kostenbasierte Abfrageoptimierer von Spark bietet eigene Funktionen zum Bereitstellen von Hinweisen und Optimieren der Abfrageleistung. Weitere Informationen finden Sie in der entsprechenden Dokumentation.

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Spark-Datenformaten für U-SQL-Entwickler](understand-spark-data-formats.md)
- [.NET für Apache Spark](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Upgrade von Big Data-Analyselösungen von Azure Data Lake Storage Gen1 auf Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-upgrade.md)
- [Transformieren von Daten mit der Spark-Aktivität in Azure Data Factory](../data-factory/transform-data-using-spark.md)
- [Transformieren von Daten mit der Hadoop Hive-Aktivität in Azure Data Factory](../data-factory/transform-data-using-hadoop-hive.md)
- [Was ist Apache Spark in Azure HDInsight?](../hdinsight/spark/apache-spark-overview.md)
