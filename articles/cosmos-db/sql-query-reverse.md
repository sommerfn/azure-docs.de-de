---
title: REVERSE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion REVERSE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8237918232bd8ba8edb2b8f71440ffd73a913334
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349544"
---
# <a name="reverse-azure-cosmos-db"></a>REVERSE (Azure Cosmos DB)
 Gibt einen Zeichenfolgenwert in umgekehrter Reihenfolge zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
REVERSE(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `REVERSE` in einer Abfrage verwendet wird.  
  
```sql
SELECT REVERSE("Abc") AS reverse  
```  
  
 Hier ist das Resultset.  
  
```json
[{"reverse": "cbA"}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
