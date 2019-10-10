---
title: CONTAINS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion CONTAINS in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5d5018d0173c316a0a31bd2f70e15e5ff972d153
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351094"
---
# <a name="contains-azure-cosmos-db"></a>CONTAINS (Azure Cosmos DB)
 Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck den zweiten enthält.  
  
## <a name="syntax"></a>Syntax
  
```sql
CONTAINS(<str_expr1>, <str_expr2>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr1*  
   Ist der Zeichenfolgenausdruck, der durchsucht werden soll.  
  
*str_expr2*  
   Ist der Zeichenfolgenausdruck, der gefunden werden soll.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird überprüft, ob „ab“ und „d“ in „abc“ enthalten sind.  
  
```sql
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Hier ist das Resultset.  
  
```json
[{"c1": true, "c2": false}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
