---
title: UPPER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion UPPER in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6b0f025948803a23c5b3c8bb6415c0e111b946b2
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349040"
---
# <a name="upper-azure-cosmos-db"></a>UPPER (Azure Cosmos DB)
 Gibt eine Zeichenfolge zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden.  
  
## <a name="syntax"></a>Syntax
  
```sql
UPPER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `UPPER` in einer Abfrage verwendet wird.  
  
```sql
SELECT UPPER("Abc") AS upper  
```  
  
 Hier ist das Resultset.  
  
```json
[{"upper": "ABC"}]  
```

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
