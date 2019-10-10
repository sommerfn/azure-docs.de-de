---
title: LEFT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LEFT in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2e175e1ed62a4afb2a532add161dd2ab63ba9b1c
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349778"
---
# <a name="left-azure-cosmos-db"></a>LEFT (Azure Cosmos DB)
 Gibt den linken Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LEFT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist der Zeichenfolgenausdruck, aus dem Zeichen extrahiert werden sollen.  
  
*num_expr*  
   Ist ein numerischer Ausdruck, der die Anzahl von Zeichen angibt.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt den linken Teil von „abc“ für verschiedene Längenwerte zurück.  
  
```sql
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Hier ist das Resultset.  
  
```json
[{"l1": "a", "l2": "ab"}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
