---
title: ACOS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ACOS in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2c9d29fba6b5dc55a98bf90cfafe0940d7bf9674
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348729"
---
# <a name="acos-azure-cosmos-db"></a>ACOS (Azure Cosmos DB)
 Gibt den Winkel in „rad“ (Bogenmaß) zurück, dessen Kosinus der angegebene numerische Ausdruck ist. Wird auch als Arkuskosinus bezeichnet.  
  
## <a name="syntax"></a>Syntax
  
```sql
ACOS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird `ACOS` für -1 zurückgegeben.  
  
```sql
SELECT ACOS(-1) AS acos 
```  
  
 Hier ist das Resultset.  
  
```json
[{"acos": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
