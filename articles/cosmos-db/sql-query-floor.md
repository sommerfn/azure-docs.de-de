---
title: FLOOR in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion FLOOR in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 94bcb6794dce9ccc22001e6d0e0e3e71d133ac42
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351214"
---
# <a name="floor-azure-cosmos-db"></a>FLOOR (Azure Cosmos DB)
 Gibt den größten ganzzahligen Wert zurück, der kleiner oder gleich dem angegebenen numerischen Ausdruck ist.  
  
## <a name="syntax"></a>Syntax
  
```sql
FLOOR (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden positive numerische, negative und Nullwerte mit der `FLOOR`-Funktion angezeigt.  
  
```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Hier ist das Resultset.  
  
```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
