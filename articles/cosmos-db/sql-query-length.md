---
title: LENGTH in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion LENGTH in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: aa430152415b1662a73a388a03ba6d4721c730f0
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349758"
---
# <a name="length-azure-cosmos-db"></a>LENGTH (Azure Cosmos DB)
 Gibt die Anzahl der Zeichen im angegebenen Zeichenfolgenausdruck zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
LENGTH(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist der auszuwertende Zeichenfolgenausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt die Länge einer Zeichenfolge zurück.  
  
```sql
SELECT LENGTH("abc") AS len 
```  
  
 Hier ist das Resultset.  
  
```json
[{"len": 3}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
