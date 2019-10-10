---
title: INDEX_OF in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion INDEX_OF in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 50e489fdf57398d486b07944782ecbb3fd1d6a43
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350988"
---
# <a name="index_of-azure-cosmos-db"></a>INDEX_OF (Azure Cosmos DB)
 Gibt die Anfangsposition des ersten Vorkommens des zweiten Zeichenfolgenausdrucks innerhalb des ersten angegebenen Zeichenfolgenausdrucks zurück, oder -1, wenn die Zeichenfolge nicht gefunden wird.  
  
## <a name="syntax"></a>Syntax
  
```sql
INDEX_OF(<str_expr1>, <str_expr2> [, <numeric_expr>])  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr1*  
   Ist der Zeichenfolgenausdruck, der durchsucht werden soll.  
  
*str_expr2*  
   Ist der Zeichenfolgenausdruck, der gesucht werden soll.  

*numeric_expr* – Ist ein optionaler numerischer Ausdruck, der die Position festlegt, an der die Suche beginnt. Die erste Position in *str_expr1* ist 0 (null). 
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt den Index der verschiedenen Teilzeichenfolgen innerhalb von „abc“ zurück.  
  
```sql
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Hier ist das Resultset.  
  
```json
[{"i1": 0, "i2": 1, "i3": -1}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
