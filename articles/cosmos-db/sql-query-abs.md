---
title: ABS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ABS in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 480c5ffcfaccfac5061e53612a2ee57235cfa626
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348760"
---
# <a name="abs-azure-cosmos-db"></a>ABS (Azure Cosmos DB)
 Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
ABS (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel enthält die Ergebnisse, die aus der Verwendung der `ABS`-Funktion für drei verschiedene Zahlen resultieren.  
  
```sql
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 Hier ist das Resultset.  
  
```json
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
