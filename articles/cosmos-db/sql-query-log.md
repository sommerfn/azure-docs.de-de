---
title: LOG in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LOG in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 16f2321eb48afacbc9636b5c0588b1ea3a01a284
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349735"
---
# <a name="log-azure-cosmos-db"></a>LOG (Azure Cosmos DB)
 Gibt den natürlichen Logarithmus des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
*base*  
   Optionales numerisches Argument, das die Basis des Logarithmus festlegt.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="remarks"></a>Anmerkungen
  
  Standardmäßig gibt LOG() den natürlichen Logarithmus zurück. Sie können die Basis des Logarithmus mithilfe des optionalen base-Parameters in einen anderen Wert ändern.  
  
  Der natürliche Logarithmus ist der Logarithmus zur Basis **e**, wobei **e** eine Irrationale Konstante ist, die ungefähr 2,718281828 entspricht.  
  
  Der natürliche Logarithmus des Exponentialwerts einer Zahl ist die Zahl selbst: LOG(EXP(n)) = n. Und der Exponentialwert des natürlichen Logarithmus einer Zahl ist die Zahl selbst: EXP(LOG(n)) = n.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird eine Variable deklariert und der Logarithmuswert der angegebenen Variablen zurückgegeben (10).  
  
```sql
SELECT LOG(10) AS log  
```  
  
 Hier ist das Resultset.  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 Im folgenden Beispiel wird `LOG` für den Exponenten einer Zahl berechnet.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Hier ist das Resultset.  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
