---
title: SIGN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion SIGN in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 11a9d0ad0c68e1ef9728c0180f136d075776abb6
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349482"
---
# <a name="sign-azure-cosmos-db"></a>SIGN (Azure Cosmos DB)
 Gibt das positive Vorzeichen (+1), null (0) oder das negative Vorzeichen (-1) des angegebenen numerischen Ausdrucks zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
SIGN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden die `SIGN`-Werte der Zahlen von -2 bis 2 zurückgegeben.  
  
```sql
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 Hier ist das Resultset.  
  
```json
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
