---
title: Leistungsoptimierung mit dem sortierten gruppierten Columnstore-Index von Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Empfehlungen und Überlegungen, die Sie kennen sollten, wenn Sie einen sortierten gruppierten Columnstore-Index zur Verbesserung Ihrer Abfrageleistung verwenden
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.openlocfilehash: 41fbebcf4b85f6e48babba30c2d05fedb3e7a5c7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985269"
---
# <a name="performance-tuning-with-ordered-clustered-columnstore-index"></a>Leistungsoptimierung mit einem sortierten gruppierten Columnstore-Index  

Wenn Benutzer eine Columnstore-Tabelle in Azure SQL Data Warehouse abfragen, überprüft der Optimierer die minimalen und maximalen Werte, die in den einzelnen Segmenten gespeichert sind.  Segmente, die sich außerhalb der Grenzen des Abfrageprädikats befinden, werden nicht vom Datenträger in den Arbeitsspeicher gelesen.  Die Leistung einer Abfrage kann gesteigert werden, wenn die Anzahl der zu lesenden Segmente und deren Gesamtgröße gering ist.   

## <a name="ordered-vs-non-ordered-clustered-columnstore-index"></a>Vergleich sortierter und nicht sortierter gruppierter Columnstore-Indizes 
Standardmäßig erstellt eine interne Komponente (Index-Generator) für jede Azure Data Warehouse-Tabelle, die ohne Indexoption erstellt wurde, einen nicht geordneten gruppierten Columnstore-Index (CCI).  Die Daten den einzelnen Spalten werden in einem separaten CCI-Zeilengruppensegment komprimiert.  Für den Wertebereich jedes Segments gibt es Metadaten, sodass Segmente, die sich außerhalb der Grenzen des Abfrageprädikats befinden, während der Abfrageausführung nicht von der Festplatte gelesen werden.  CCI bietet den höchsten Grad an Datenkomprimierung und verringert die Größe der zu lesenden Segmente, damit Abfragen schneller ausgeführt werden. Da der Index-Generator die Daten jedoch nicht sortiert, bevor er sie in Segmente komprimiert, können Segmente mit überlappenden Wertebereichen auftreten, was dazu führt, dass Abfragen mehr Segmente von der Festplatte lesen und die Fertigstellung länger dauert.  

Beim Erstellen eines sortierten CCI sortiert die Azure SQL Data Warehouse-Engine die Daten im Arbeitsspeicher nach den Sortierschlüsseln, bevor der Index-Generator sie in Indexsegmente komprimiert.  Bei sortierten Daten wird die Überlappung von Segmenten verringert, sodass Abfragen Segmente effizienter entfernen können und somit eine schnellere Leistung aufweisen, da die Anzahl der Segmente, die vom Datenträger gelesen werden sollen, kleiner ist.  Die Überlappung von Segmenten lässt sich vermeiden, wenn alle Daten im Arbeitsspeicher gleichzeitig sortiert werden.  Aufgrund der großen Datenmenge in Data Warehouse-Tabellen kommt dieses Szenario nicht häufig vor.  

Wenn Sie die Segmentbereiche für eine Spalte überprüfen möchten, führen Sie diesen Befehl mit dem Tabellen- und Spaltennamen aus:

```sql
SELECT o.name, pnp.index_id, pnp.rows, pnp.data_compression_desc, pnp.pdw_node_id, 
pnp.distribution_id, cls.segment_id, cls.column_id, cls.min_data_id, cls.max_data_id, cls.max_data_id-cls.min_data_id as difference
FROM sys.pdw_nodes_partitions AS pnp
   JOIN sys.pdw_nodes_tables AS Ntables ON pnp.object_id = NTables.object_id AND pnp.pdw_node_id = NTables.pdw_node_id
   JOIN sys.pdw_table_mappings AS Tmap  ON NTables.name = TMap.physical_name AND substring(TMap.physical_name,40, 10) = pnp.distribution_id
   JOIN sys.objects AS o ON TMap.object_id = o.object_id
   JOIN sys.pdw_nodes_column_store_segments AS cls ON pnp.partition_id = cls.partition_id AND pnp.distribution_id  = cls.distribution_id
   JOIN sys.columns as cols ON o.object_id = cols.object_id AND cls.column_id = cols.column_id
WHERE o.name = '<table_name>' and c.name = '<column_name>'
ORDER BY o.name, pnp.distribution_id, cls.min_data_id
```

## <a name="data-loading-performance"></a>Leistung beim Laden von Daten

Die Leistung beim Laden von Daten in eine sortierte CCI-Tabelle ähnelt dem Laden von Daten in eine partitionierte Tabelle.  
Das Laden von Daten in eine sortierte CCI-Tabelle kann aufgrund der Datensortierung mehr Zeit in Anspruch nehmen als das Laden von Daten in eine nicht sortierte CCI-Tabelle.  

Im Folgenden finden Sie ein Beispiel für einen Leistungsvergleich beim Laden von Daten in Tabellen mit unterschiedlichen Schemas.
![Performance_comparison_data_loading](media/performance-tuning-ordered-cci/cci-data-loading-performance.png)
 
## <a name="reduce-segment-overlapping"></a>Verringern der Überlappung von Segmenten
Im Folgenden finden Sie Optionen zum weiteren Verringern der Überlappung von Segmenten bei der Erstellung eines sortierten CCI für eine neue Tabelle über CTAS oder für eine vorhandene Tabelle mit Daten:

- Verwenden Sie eine größere Ressourcenklasse, um zuzulassen, dass mehr Daten im Arbeitsspeicher gleichzeitig sortiert werden, bevor sie vom Index-Generator in Segmente komprimiert werden.  In einem Indexsegment kann der physische Speicherort der Daten nicht mehr geändert werden.  Innerhalb eines Segments oder segmentübergreifend gibt es keine Datensortierung.  

- Verwenden Sie einen niedrigeren Parallelitätsgrad (z. B. 1).  Jeder Thread, der für die Erstellung eines sortierten CCI verwendet wird, verarbeitet eine Teilmenge an Daten und sortiert diese lokal.  Für Daten, die von unterschiedlichen Threads sortiert wurden, gibt es keine globale Sortierung.  Mithilfe paralleler Threads kann die Zeit zum Erstellen eines sortierten CCI verkürzt werden. Dabei werden jedoch mehr überlappende Segmente generiert als bei der Verwendung eines einzelnen Threads. 
- Sortieren Sie die Daten vorab nach den Sortierschlüsseln, bevor Sie sie in Azure SQL Data Warehouse-Tabellen laden.

## <a name="create-ordered-cci-on-large-tables"></a>Erstellen eines sortierten CCI für große Tabellen
Das Erstellen eines sortierten CCI ist ein Offlinevorgang.  Bei Tabellen ohne Partitionen sind die Daten für Benutzer erst verfügbar, wenn der Erstellungsprozess des sortierten CCI abgeschlossen ist.   Da die Engine sortierte CCI-Partitionen für partitionierte Tabellen nach Partition erstellt, können die Benutzer auf die Daten in den Partitionen weiterhin zugreifen, für die die Erstellung des sortierten CCI nicht gerade erfolgt.   Mit dieser Option können Sie die Ausfallzeiten beim Erstellen eines sortierten CCI für große Tabellen minimieren: 

1.  Erstellen Sie Partitionen für die große Zieltabelle (Tabelle A).
2.  Erstellen Sie eine leere sortierte CCI-Tabelle (Tabelle B), die dieselben Tabellen- und Partitionsschemas hat wie Tabelle A.
3.  Fügen Sie eine Partition von Tabelle A in Tabelle B ein.
4.  Führen Sie „ALTER INDEX <Ordered_CCI_Index> REBUILD“ für Tabelle B aus, um die eingefügte Partition neu zu erstellen.  
5.  Wiederholen Sie die Schritte 3 und 4 für jede Partition in Tabelle A.
6.  Nachdem alle Partitionen von Tabelle A in Tabelle B eingefügt und neu erstellt wurden, löschen Sie Tabelle A und benennen Tabelle B in Tabelle A um. 

## <a name="examples"></a>Beispiele

**A. So suchen Sie sortierte Spalten und die Ordnungszahl der Sortierung:**
```sql
SELECT object_name(c.object_id) table_name, c.name column_name, i.column_store_order_ordinal 
FROM sys.index_columns i 
JOIN sys.columns c ON i.object_id = c.object_id AND c.column_id = i.column_id
WHERE column_store_order_ordinal <>0
```

**B. Fügen Sie Spalten aus der Sortierliste hinzu, oder entfernen Sie sie, wenn Sie die Ordnungszahl der Spalte ändern möchten oder von einem CCI zu einem sortierten CCI wechseln möchten:**
```sql
CREATE CLUSTERED COLUMNSTORE INDEX InternetSales ON  InternetSales
ORDER (ProductKey, SalesAmount)
WITH (DROP_EXISTING = ON)
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse](sql-data-warehouse-overview-develop.md).
