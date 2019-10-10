---
title: ARRAY_CONCAT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ARRAY_CONCAT in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ad973650ac205313f9045c170f99e15e385a82d1
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348714"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
 Gibt ein Array zurück, das das Ergebnis der Verkettung von zwei oder mehr Arraywerten darstellt.  
  
## <a name="syntax"></a>Syntax
  
```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  
  
## <a name="arguments"></a>Argumente
  
*arr_expr*  
   Ist ein Arrayausdruck, der mit den anderen Werten verkettet werden soll. Die `ARRAY_CONCAT`-Funktion erfordert mindestens zwei *arr_expr*-Argumente.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Arrayausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird veranschaulicht, wie zwei Arrays verkettet werden.  
  
```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Hier ist das Resultset.  
  
```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Arrayfunktionen in Azure Cosmos DB](sql-query-array-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
