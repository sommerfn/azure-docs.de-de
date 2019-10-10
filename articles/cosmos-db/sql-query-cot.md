---
title: COT in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion COT in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d980319730e69fdcb529272ba1b8fb48d2b5b230
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351253"
---
# <a name="cot-azure-cosmos-db"></a>COT (Azure Cosmos DB)
 Gibt den trigonometrischen Kotangens des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen numerischen Ausdruck zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
COT(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Durch das folgende Beispiel wird der `COT` des angegebenen Winkels berechnet.  
  
```sql
SELECT COT(124.1332) AS cot  
```  
  
 Hier ist das Resultset.  
  
```json
[{"cot": -0.040311998371148884}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
