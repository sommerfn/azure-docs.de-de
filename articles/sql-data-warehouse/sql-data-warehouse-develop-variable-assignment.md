---
title: Zuweisen von Variablen in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Tipps zum Zuweisen von T-SQL-Variablen in Azure SQL Data Warehouse für die Entwicklung von Lösungen
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 6c943478f3904aac17a572f012f2b2b69ffa2223
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479564"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Zuweisen von Variablen in Azure SQL Data Warehouse

Tipps zum Zuweisen von T-SQL-Variablen in Azure SQL Data Warehouse für die Entwicklung von Lösungen

## <a name="setting-variables-with-declare"></a>Festlegen von Variablen mit DECLARE

Variablen werden in SQL Data Warehouse mit der `DECLARE`-Anweisung oder der `SET`-Anweisung festgelegt. Das Initialisieren von Variablen mit DECLARE ist eine der flexibelsten Möglichkeiten zum Festlegen eines Variablenwerts in SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

Sie können mit DECLARE auch mehrere Variablen gleichzeitig festlegen. Mithilfe von SELECT oder UPDATE können Sie Folgendes:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Eine Variable kann nicht in der gleichen DECLARE-Anweisung initialisiert und verwendet werden. Zur Veranschaulichung: Im folgenden Beispiel, das **nicht** zulässig ist, wird @p1 in der gleichen DECLARE-Anweisung sowohl initialisiert als auch verwendet. Das folgende Beispiel zeigt einen Fehler.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Festlegen von Werten mit SET

SET ist eine allgemeine Methode zum Festlegen einer einzelnen Variablen.

Die folgenden Anweisungen sind alle gültige Möglichkeiten zum Festlegen einer Variablen mit SET:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Sie können mit SET nur jeweils eine einzige Variable festlegen. Allerdings sind zusammengesetzte Operatoren zulässig.

## <a name="limitations"></a>Einschränkungen

Sie können UPDATE nicht für die Zuweisung von Variablen verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](sql-data-warehouse-overview-develop.md).
