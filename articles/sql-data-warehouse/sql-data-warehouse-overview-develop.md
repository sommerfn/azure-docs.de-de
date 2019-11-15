---
title: Ressourcen für die Entwicklung eines Data Warehouse in Azure
description: Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: a6cb7937bdd5dea9eb1a48b2b350db9077431fe0
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73645613"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse
Lesen Sie diese Entwicklungsartikel, um ein besseres Verständnis der wesentlichen Entwurfsentscheidungen, der Empfehlungen und der Programmiertechniken für SQL Data Warehouse zu erlangen.

## <a name="key-design-decisions"></a>Wesentliche Entwurfsentscheidungen
In den folgenden Artikeln werden Konzepte und Entwurfsentscheidungen für die Entwicklung eines verteilten Data Warehouse mit SQL Data Warehouse vorgestellt:

* [connections][connections]
* [Parallelität][concurrency]
* [Transaktionen][transactions]
* [Benutzerdefinierte Schemas][user-defined schemas]
* [Tabellenverteilung][table distribution]
* [Tabellenindizes][table indexes]
* [Tabellenpartitionen][table partitions]
* [CTAS][CTAS]
* [Statistiken][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Entwicklungsempfehlungen und Programmiertechniken
In diesen Artikeln werden bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung eines SQL Data Warehouse behandelt:

* [Gespeicherte Prozeduren][stored procedures]
* [Bezeichnungen][labels]
* [Ansichten][views]
* [Temporäre Tabellen][temporary tables]
* [Dynamisches SQL][dynamic SQL]
* [Schleifen][looping]
* [Gruppierungsoptionen][group by options]
* [Variablenzuweisung][variable assignment]

## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzinformationen finden Sie unter [Transact-SQL-Themen](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
