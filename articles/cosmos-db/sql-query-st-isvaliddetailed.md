---
title: ST_ISVALIDDETAILED in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ST_ISVALIDDETAILED in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9e640c223c2fef844b9b53e1f4afa3a5d398c8c0
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349353"
---
# <a name="st_isvaliddetailed-azure-cosmos-db"></a>ST_ISVALIDDETAILED (Azure Cosmos DB)
 Gibt einen JSON-Wert mit einem booleschen Wert zurück, wenn der angegebene GeoJSON-Punkt-, -Polygon- oder -LineString-Ausdruck gültig ist. Falls ungültig, wird außerdem der Grund als Zeichenfolge zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*spatial_expr*  
   Ist ein GeoJSON-Point- oder -Polygon-Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen JSON-Wert mit einem booleschen Wert zurück, wenn der angegebene GeoJSON-Punkt oder -Polygon-Ausdruck gültig ist. Falls ungültig, wird außerdem der Grund als Zeichenfolge zurückgegeben.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel veranschaulicht, wie die Gültigkeit (einschließlich Details) mithilfe von `ST_ISVALIDDETAILED` überprüft wird.  
  
```sql
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 Hier ist das Resultset.  
  
```json
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Räumliche Funktionen in Azure Cosmos DB](sql-query-spatial-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
