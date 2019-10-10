---
title: ST_WITHIN in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ST_WITHIN in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 40ecb26e7ac782d7831e6ef94c9d3cfc6a370cbb
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349330"
---
# <a name="st_within-azure-cosmos-db"></a>ST_WITHIN (Azure Cosmos DB)
 Gibt einen booleschen Ausdruck zurück, der angibt, ob das im ersten Argument angegebene GeoJSON-Objekt (Punkt, Polygon oder LineString) im zweiten Argument im GeoJSON-Objekt (Punkt, Polygon oder LineString) enthalten ist.  
  
## <a name="syntax"></a>Syntax
  
```sql
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*spatial_expr*  
   Ist ein GeoJSON-Point-, -Polygon- oder -LineString-Objektausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Wert zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel zeigt, wie alle Familiendokumente mit `ST_WITHIN` innerhalb eines Polygons gefunden werden.  
  
```sql
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier ist das Resultset.  
  
```json
[{ "id": "WakefieldFamily" }]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Räumliche Funktionen in Azure Cosmos DB](sql-query-spatial-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
