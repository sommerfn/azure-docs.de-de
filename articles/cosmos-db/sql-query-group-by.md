---
title: GROUP BY-Klausel in Azure Cosmos DB
description: Erfahren Sie mehr über die GROUP BY-Klausel für Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: tisande
ms.openlocfilehash: d92e24836a0eb5757de9bbdb516be290456deb7f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333419"
---
# <a name="group-by-clause-in-azure-cosmos-db"></a>GROUP BY-Klausel in Azure Cosmos DB

Die GROUP BY-Klausel unterteilt die Ergebnisse der Abfrage anhand der Werte einer oder mehrerer angegebener Eigenschaften.

> [!NOTE]
> Azure Cosmos DB unterstützt derzeit GROUP By ab [.NET SDK 3.3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.3.0).
> Die Unterstützung für SDKs anderer Sprachen und das Azure-Portal ist derzeit nicht verfügbar, aber in Planung.

## <a name="syntax"></a>Syntax

```sql  
<group_by_clause> ::= GROUP BY <scalar_expression_list>

<scalar_expression_list> ::=
          <scalar_expression>
        | <scalar_expression_list>, <scalar_expression>
```  

## <a name="arguments"></a>Argumente

- `<scalar_expression_list>`

   Gibt die Ausdrücke an, die zum Aufteilen von Abfrageergebnissen verwendet werden.

- `<scalar_expression>`
  
   Ein beliebiger Skalarausdruck ist außer für skalare Unterabfragen und skalare Aggregate zulässig. Jeder Skalarausdruck muss mindestens einen Verweis auf eine Eigenschaft enthalten. Es gibt keine Begrenzung der Anzahl einzelner Ausdrücke oder der Kardinalität des jeweiligen Ausdrucks.

## <a name="remarks"></a>Anmerkungen
  
  Wenn eine Abfrage eine GROUP BY-Klausel verwendet, kann die SELECT-Klausel nur die Teilmenge der Eigenschaften und Systemfunktionen enthalten, die in der GROUP BY-Klausel enthalten ist. Eine Ausnahme bilden [aggregierte Systemfunktionen](sql-query-aggregates.md), die in der SELECT-Klausel vorkommen können, ohne in der GROUP BY-Klausel enthalten zu sein. Sie können in die SELECT-Klausel auch stets Literalwerte einschließen.

  Die GROUP BY-Klausel muss hinter der SELECT-, FROM- und WHERE-Klausel und vor der OFFSET LIMIT-Klausel stehen. GROUP BY kann derzeit nicht mit einer ORDER BY-Klausel verwendet werden, was jedoch in Planung ist.

  Die GROUP BY-Klausel erlaubt keines der folgenden Elemente:
  
- Aliase von Eigenschaften oder Systemfunktionen (in der SELECT-Klausel sind Aliase weiterhin zulässig)
- Unterabfragen
- Aggregierte Systemfunktionen (diese sind nur in der SELECT-Klausel zulässig)

## <a name="examples"></a>Beispiele

In diesen Beispielen wird das Dataset zu Nährwerten verwendet, das über den [Azure Cosmos DB-Abfrageplayground](https://www.documentdb.com/sql/demo) verfügbar ist.

Hier ist z. B. eine Abfrage, die die Gesamtzahl der Artikel in jeder Lebensmittelgruppe zurückgibt:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup
FROM Food f
GROUP BY f.foodGroup
```

Es folgen einige Ergebnisse (das Schlüsselwort TOP wird zum Eingrenzen von Ergebnissen verwendet):

```json
[{
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "foodGroup": "Meals, Entrees, and Side Dishes",
  "foodGroupCount": 113
},
{
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Diese Abfrage enthält zwei Ausdrücke, die zum Aufteilen von Ergebnissen verwendet werden:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, f.foodGroup, f.version
FROM Food f
GROUP BY f.foodGroup, f.version
```

Einige Ergebnisse sind wie folgt:

```json
[{
  "version": 1,
  "foodGroup": "Nut and Seed Products",
  "foodGroupCount": 133
},
{
  "version": 1,
  "foodGroup": "Finfish and Shellfish Products",
  "foodGroupCount": 267
},
{
  "version": 1,
  "foodGroup": "Fast Foods",
  "foodGroupCount": 371
},
{
  "version": 1,
  "foodGroup": "Sausages and Luncheon Meats",
  "foodGroupCount": 244
}]
```

Diese Abfrage enthält in der Group By-Klausel eine Systemfunktion:

```sql
SELECT TOP 4 COUNT(1) AS foodGroupCount, UPPER(f.foodGroup) AS upperFoodGroup
FROM Food f
GROUP BY UPPER(f.foodGroup)
```

Einige Ergebnisse sind wie folgt:

```json
[{
  "foodGroupCount": 371,
  "upperFoodGroup": "FAST FOODS"
},
{
  "foodGroupCount": 267,
  "upperFoodGroup": "FINFISH AND SHELLFISH PRODUCTS"
},
{
  "foodGroupCount": 389,
  "upperFoodGroup": "LEGUMES AND LEGUME PRODUCTS"
},
{
  "foodGroupCount": 113,
  "upperFoodGroup": "MEALS, ENTREES, AND SIDE DISHES"
}]
```

Diese Abfrage verwendet im Ausdruck mit der Artikeleigenschaft sowohl Schlüsselwörter als auch Systemfunktionen:

```sql
SELECT COUNT(1) AS foodGroupCount, ARRAY_CONTAINS(f.tags, {name: 'orange'}) AS containsOrangeTag,  f.version BETWEEN 0 AND 2 AS correctVersion
FROM Food f
GROUP BY ARRAY_CONTAINS(f.tags, {name: 'orange'}), f.version BETWEEN 0 AND 2
```

Die Ergebnisse sind wie folgt:

```json
[{
  "correctVersion": true,
  "containsOrangeTag": false,
  "foodGroupCount": 8608
},
{
  "correctVersion": true,
  "containsOrangeTag": true,
  "foodGroupCount": 10
}]
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [SELECT-Klausel](sql-query-select.md)
- [Aggregatfunktionen](sql-query-aggregates.md)
