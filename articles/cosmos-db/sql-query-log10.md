---
title: LOG10 in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LOG10 in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 56f1e96e7d4ee1b5f38ee7392665e17819ae554b
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349716"
---
# <a name="log10-azure-cosmos-db"></a>LOG10 (Azure Cosmos DB)
 Gibt den dekadischen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LOG10 (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expression*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Anmerkungen
  
  Die Funktionen LOG10 und POWER sind umgekehrt miteinander verknüpft. Beispiel: 10 ^ LOG10(n) = n.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird eine Variable deklariert und der LOG10-Wert der angegebenen Variablen zurückgegeben (100).  
  
```sql
SELECT LOG10(100) AS log10 
```  
  
 Hier ist das Resultset.  
  
```json
[{log10: 2}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
