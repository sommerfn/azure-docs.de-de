---
title: POWER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion POWER in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 683c53c369f136ad4b917b93e9a92a71072d05e0
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349637"
---
# <a name="power-azure-cosmos-db"></a>POWER (Azure Cosmos DB)
 Gibt den Wert des angegebenen Ausdrucks gemäß der angegebenen Potenz zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
POWER (<numeric_expr1>, <numeric_expr2>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr1*  
   Ist ein numerischer Ausdruck.  
  
*numeric_expr2*  
   Ist der Wert, mit dem *numeric_expr1* potenziert werden soll.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt das Potenzieren einer Zahl mit 3 (dritte Potenz der Zahl).  
  
```sql
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 Hier ist das Resultset.  
  
```json
[{pow1: 8, pow2: 15.625}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
