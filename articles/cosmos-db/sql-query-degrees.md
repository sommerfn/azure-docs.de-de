---
title: DEGREES in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion DEGREES in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d8c2109c2c11f137b1966741a95d1d0c02895016
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351201"
---
# <a name="degrees-azure-cosmos-db"></a>DEGREES (Azure Cosmos DB)
 Gibt den entsprechenden Winkel in Grad für einen in „rad“ (Bogenmaß) angegebenen Winkel zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
DEGREES (<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt die Gradzahl in einem Winkel des PI/2-Bogenmaßes zurück.  
  
```sql
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Hier ist das Resultset.  
  
```json
[{"degrees": 90}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
