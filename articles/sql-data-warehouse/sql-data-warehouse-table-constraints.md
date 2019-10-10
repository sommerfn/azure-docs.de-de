---
title: Primärschlüssel, Fremdschlüssel und eindeutiger Schlüssel in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Unterstützung von Tabellenconstraints in Azure SQL Data Warehouse
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.openlocfilehash: b02f219e549f2206f71c08c9d465b2bc05a6d526
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309328"
---
# <a name="primary-key-foreign-key-and-unique-key-in-azure-sql-data-warehouse"></a>Primärschlüssel, Fremdschlüssel und eindeutiger Schlüssel in Azure SQL Data Warehouse

Erfahren Sie mehr über Tabellenconstraints in Azure SQL Data Warehouse, einschließlich Primärschlüssel, Fremdschlüssel und eindeutigem Schlüssel.

## <a name="table-constraints"></a>Tabellenconstraints 
Azure SQL Data Warehouse unterstützt diese Tabellenconstraints: 
- PRIMARY KEY wird nur unterstützt, wenn sowohl NONCLUSTERED als auch NOT ENFORCED verwendet werden.    
- Der UNIQUE-Constraint wird nur unterstützt, wenn NOT ENFORCED verwendet wird.   

Die FOREIGN KEY-Constraint wird in Azure SQL Data Warehouse nicht unterstützt.  

## <a name="remarks"></a>Anmerkungen
Mit einem Primärschlüssel und/oder einem eindeutigen Schlüssel kann die Data Warehouse-Engine einen optimalen Ausführungsplan für eine Abfrage erstellen.  Alle Werte in einer Primärschlüsselspalte oder einer Spalte für einen eindeutigen Constraint müssen eindeutig sein. 

Nach dem Erstellen einer Tabelle mit Hauptschlüssel- oder Eindeutig-Constraint in Azure Data Warehouse müssen die Benutzer sicherstellen, dass alle Werte in den betreffenden Spalten eindeutig sind.  Eine Verletzung dieser Bedingung kann dazu führen, dass die Abfrage ungenaue Ergebnisse zurückgibt.  Dieses Beispiel zeigt, wie eine Abfrage ungenaue Ergebnisse zurückgeben kann, falls die Hauptschlüsselspalte oder die Spalte mit dem Eindeutig-Contraint doppelte Werte enthält.  

```sql
 -- Create table t1
CREATE TABLE t1 (a1 INT NOT NULL, b1 INT) WITH (DISTRIBUTION = ROUND_ROBIN)

-- Insert values to table t1 with duplicate values in column a1.
INSERT INTO t1 VALUES (1, 100)
INSERT INTO t1 VALUES (1, 1000)
INSERT INTO t1 VALUES (2, 200)
INSERT INTO t1 VALUES (3, 300)
INSERT INTO t1 VALUES (4, 400)

-- Run this query.  No primary key or unique constraint.  4 rows returned. Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
1           2
2           1
3           1
4           1

(4 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 ADD CONSTRAINT unique_t1_a1 unique (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, count(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key constraint
ALTER TABLE t1 add CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Incorrect result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
4           1
1           1
3           1
1           1

(5 rows affected)
*/

-- Manually fix the duplicate values in a1
UPDATE t1 SET a1 = 0 WHERE b1 = 1000

-- Verify no duplicate values in column a1 
SELECT * FROM t1

/*
a1          b1
----------- -----------
2           200
3           300
4           400
0           1000
1           100

(5 rows affected)
*/

-- Add unique constraint
ALTER TABLE t1 add CONSTRAINT unique_t1_a1 UNIQUE (a1) NOT ENFORCED  

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) as total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

-- Drop unique constraint.
ALTER TABLE t1 DROP CONSTRAINT unique_t1_a1

-- Add primary key contraint
ALTER TABLE t1 ADD CONSTRAINT PK_t1_a1 PRIMARY KEY NONCLUSTERED (a1) NOT ENFORCED

-- Re-run this query.  5 rows returned.  Correct result.
SELECT a1, COUNT(*) AS total FROM t1 GROUP BY a1

/*
a1          total
----------- -----------
2           1
3           1
4           1
0           1
1           1

(5 rows affected)
*/

```

## <a name="examples"></a>Beispiele
Erstellen einer Data Warehouse-Tabelle mit einem Primärschlüssel: 

```sql 
CREATE TABLE mytable (c1 INT PRIMARY KEY NONCLUSTERED NOT ENFORCED, c2 INT);
```
Erstellen einer Data Warehouse-Tabelle mit einem Eindeutig-Constraint:

```sql
CREATE TABLE t6 (c1 INT UNIQUE NOT ENFORCED, c2 INT);
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen der Tabellen für das Data Warehouse werden im nächsten Schritt Daten in die Tabelle geladen. Ein Tutorial zum Laden von Daten finden Sie unter [Laden von Daten in SQL Data Warehouse](load-data-wideworldimportersdw.md).
