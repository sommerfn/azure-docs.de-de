---
title: CONCAT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion CONCAT in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 1e61d61a3d64ca7d7808619159e4dfc8e8b33d68
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351261"
---
# <a name="concat-azure-cosmos-db"></a>CONCAT (Azure Cosmos DB)
 Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von zwei oder mehr Zeichenfolgenwerten darstellt.  
  
## <a name="syntax"></a>Syntax
  
```sql
CONCAT(<str_expr1>, <str_expr2> [, <str_exprN>])  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ein Zeichenfolgenausdruck, der mit den anderen Werten verkettet werden soll. Die `CONCAT`-Funktion erfordert mindestens zwei *str_expr*-Argumente.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt die verkettete Zeichenfolge der angegebenen Werte zurück.  
  
```sql
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Hier ist das Resultset.  
  
```json
[{"concat": "abcdef"}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
