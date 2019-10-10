---
title: ATAN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ATAN in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4ee3867f3a4938358e8d61aa8a98f747756ee3e8
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348565"
---
# <a name="atan-azure-cosmos-db"></a>ATAN (Azure Cosmos DB)
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Tangens der angegebene numerische Ausdruck ist. Wird auch als Arkustangens bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ATAN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ATAN` für den angegebenen Wert zurückgegeben.  
  
```sql
SELECT ATAN(-45.01) AS atan  
```  
  
 Hier ist das Resultset.  
  
```json
[{"atan": -1.5485826962062663}]  
```  
  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
