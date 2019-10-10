---
title: ASIN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ASIN in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5a64fc8d8f87d38f001bf2bc9dd581692c97fe64
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348495"
---
# <a name="asin-azure-cosmos-db"></a>ASIN (Azure Cosmos DB)
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Sinus der angegebene numerische Ausdruck ist. Wird auch als Arkussinus bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ASIN` für -1 zurückgegeben.  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 Hier ist das Resultset.  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
