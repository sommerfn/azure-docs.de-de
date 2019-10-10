---
title: COS in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion COS in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 7e64127b7af749af87b90331c3f175957cfa7f09
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351070"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 Gibt den trigonometrischen Kosinus des angegebenen Winkels in „rad“ (Bogenmaß) im angegebenen Ausdruck zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*numeric_expr*  
   Ist ein numerischer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Durch das folgende Beispiel wird der `COS` des angegebenen Winkels berechnet.  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 Hier ist das Resultset.  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
