---
title: ARRAY_SLICE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ARRAY_SLICE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1df4177bb8b56bc98977af0f5180e8df5affb257
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348549"
---
# <a name="array_slice-azure-cosmos-db"></a>ARRAY_SLICE (Azure Cosmos DB)
 Gibt einen Teil eines Arrayausdrucks zurück.
  
## <a name="syntax"></a>Syntax
  
```sql
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
## <a name="arguments"></a>Argumente
  
*arr_expr*  
   Ist ein Arrayausdruck.  
  
*num_expr*  
   Nullbasierter numerischer Index, bei dem das Array beginnt. Negative Werte können verwendet werden, um den Startindex relativ zum letzten Element des Arrays anzugeben, d.h. -1 bezieht sich auf das letzte Element des Arrays.  

*num_expr* – Ist ein optionaler numerischer Ausdruck, der die maximale Anzahl von Elementen im resultierenden Array festlegt.    

## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Arrayausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel veranschaulicht, wie Sie mit `ARRAY_SLICE` verschiedene Slices eines Arrays abrufen.  
  
```sql
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Hier ist das Resultset.  
  
```json
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Arrayfunktionen in Azure Cosmos DB](sql-query-array-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
