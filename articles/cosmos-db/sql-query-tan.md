---
title: TAN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion TAN in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 70636dbc00307ced8ad0cf84f41fe025c759997e
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349163"
---
# <a name="tan-azure-cosmos-db"></a>TAN (Azure Cosmos DB)
 Gibt den Tangens des angegebenen Winkels in Bogenmaß im angegebenen Ausdruck zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
TAN (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel berechnet den Tangens von PI()/2.  
  
```sql
SELECT TAN(PI()/2) AS tan 
```  
  
 Hier ist das Resultset.  
  
```json
[{"tan": 16331239353195370 }]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
