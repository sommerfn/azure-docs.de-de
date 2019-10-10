---
title: ToString in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion ToString in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 53630a0ecd76459f23a978e98040a86152d7c0d8
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349133"
---
# <a name="tostring-azure-cosmos-db"></a>ToString (Azure Cosmos DB)
 Gibt eine Zeichenfolgendarstellung eines skalaren Ausdrucks zurück. 
  
## <a name="syntax"></a>Syntax
  
```sql
ToString(<expr>)
```  
  
## <a name="arguments"></a>Argumente
  
*expr*  
   Ist ein skalarer Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Zeichenfolgenausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `ToString` veranschaulicht.   
  
```sql
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 Hier ist das Resultset.  
  
```json
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 Die folgende Eingabe ist gegeben:
```json
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 Im folgenden Beispiel wird veranschaulicht, wie `ToString` mit anderen Zeichenfolgenfunktionen wie `CONCAT` verwendet werden kann.   
 
```sql
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

Hier ist das Resultset.  
  
```json
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
Die folgende Eingabe ist gegeben:
```json
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
Im folgenden Beispiel wird veranschaulicht, wie `ToString` mit anderen Zeichenfolgenfunktionen wie `REPLACE` verwendet werden kann.   
```sql
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
Hier ist das Resultset.  
 ```json
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
