---
title: ARRAY_LENGTH in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ARRAY_LENGTH in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b27b3f09212047e2e8937a4bf649fa3335e15cb2
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348672"
---
# <a name="array_length-azure-cosmos-db"></a>ARRAY_LENGTH (Azure Cosmos DB)
 Gibt die Anzahl der Elemente des angegebenen Arrayausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
ARRAY_LENGTH(<arr_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*arr_expr*  
   Ist ein Arrayausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird die Länge eines Arrays mit `ARRAY_LENGTH` abgerufen.  
  
```sql
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Hier ist das Resultset.  
  
```json
[{"len": 3}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Arrayfunktionen in Azure Cosmos DB](sql-query-array-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
