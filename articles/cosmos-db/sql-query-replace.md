---
title: REPLACE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion REPLACE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2cb7d82efd010fd7c3395a4f6a9217370d9e5779
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349579"
---
# <a name="replace-azure-cosmos-db"></a>REPLACE (Azure Cosmos DB)
 Ersetzt alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert.  
  
## <a name="syntax"></a>Syntax
  
```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr1*  
   Ist der Zeichenfolgenausdruck, der durchsucht werden soll.  
  
*str_expr2*  
   Ist der Zeichenfolgenausdruck, der gesucht werden soll.  
  
*str_expr3*  
   Ist der Zeichenfolgenausdruck, durch den Vorkommen von *str_expr2* in *str_expr1* ersetzt werden sollen.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel veranschaulicht die Verwendung von `REPLACE` in einer Abfrage.  
  
```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 Hier ist das Resultset.  
  
```json
[{"replace": "This is a desk"}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
