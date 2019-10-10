---
title: REPLICATE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion REPLICATE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5e0b7f29c503daa8a95dcc46238e60728c0cec50
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349573"
---
# <a name="replicate-azure-cosmos-db"></a>REPLICATE (Azure Cosmos DB)
 Wiederholt einen Zeichenfolgenwert mit der angegebenen Häufigkeit.
  
## <a name="syntax"></a>Syntax
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.
  
*num_expr*  
   Ist ein numerischer Ausdruck. Wenn *num_expr* negativ oder unendlich ist, ist das Ergebnis nicht definiert.
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.
  
## <a name="remarks"></a>Anmerkungen
  Die maximale Länge des Ergebnisses beträgt 10.000 Zeichen ((length(*str_expr*)  *  *num_expr*) <= 10.000).

## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `REPLICATE` in einer Abfrage verwendet wird.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Hier ist das Resultset.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
