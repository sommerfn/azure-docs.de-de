---
title: LOWER in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LOWER in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 3a9c122ef65772458b832d3b1651e7e63e42985e
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349708"
---
# <a name="lower-azure-cosmos-db"></a>LOWER (Azure Cosmos DB)
 Gibt eine Zeichenfolge zurück, nachdem Großbuchstaben in Kleinbuchstaben konvertiert wurden.  
  
## <a name="syntax"></a>Syntax
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie `LOWER` in einer Abfrage verwendet wird.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Hier ist das Resultset.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
