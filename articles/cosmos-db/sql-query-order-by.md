---
title: ORDER BY-Klausel in Azure Cosmos DB
description: Erfahren Sie mehr über die ORDER BY-Klausel von SQL für Azure Cosmos DB. Verwenden Sie SQL als eine JSON-Abfragesprache in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 14f61d14b59dca4bcf2e0f4b93e918f101a61833
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326848"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>ORDER BY-Klausel in Azure Cosmos DB

Die optionale ORDER BY-Klausel gibt die Sortierreihenfolge für die von der Abfrage zurückgegebenen Ergebnisse an.

## <a name="syntax"></a>Syntax
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Argumente
  
- `<sort_specification>`  
  
   Gibt eine Eigenschaft oder einen Ausdruck an, wonach das Resultset der Abfrage sortiert werden soll. Eine Sortierspalte kann als Name oder Eigenschaftenalias angegeben werden.  
  
   Mehrere Eigenschaften können angegeben werden. Namen von Eigenschaften müssen eindeutig sein. Die Sequenz der Sortiereigenschaften in der ORDER BY-Klausel definiert die Anordnung des sortierten Resultsets. Dies bedeutet: Das Resultset wird nach der ersten Eigenschaft sortiert, und dann wird diese sortierte Liste nach der zweiten Eigenschaft sortiert usw.  
  
   Die Eigenschaftennamen, auf die in der ORDER BY-Klausel verwiesen wird, müssen entweder einer Eigenschaft in der Auswahlliste oder einer Eigenschaft entsprechen, die in der Sammlung definiert ist, die in der FROM-Klausel eindeutig angegeben ist.  
  
- `<sort_expression>`  
  
   Hiermit wird mindestens eine Eigenschaft oder ein Ausdruck festgelegt, nach dem die Abfrageergebnisse sortiert werden.  
  
- `<scalar_expression>`  
  
   Weitere Informationen finden Sie im Abschnitt [Skalarausdrücke](sql-query-scalar-expressions.md).  
  
- `ASC | DESC`  
  
   Gibt an, dass die Werte in der angegebenen Spalte in aufsteigender oder absteigender Reihenfolge sortiert werden sollen. ASC sortiert vom niedrigsten Wert zum höchsten Wert. DESC sortiert vom höchsten Wert zum niedrigsten Wert. ASC ist die Standardsortierreihenfolge. NULL-Werte werden als die niedrigsten möglichen Werte behandelt.  
  
## <a name="remarks"></a>Anmerkungen  
  
   Die ORDER BY-Klausel erfordert, dass die Indizierungsrichtlinie einen Index für die Felder enthält, die sortiert werden. Die Abfragelaufzeit von Azure Cosmos DB unterstützt die Sortierung für einen Eigenschaftennamen und nicht für berechnete Eigenschaften. Azure Cosmos DB unterstützt mehrere ORDER BY-Eigenschaften. Zum Ausführen einer Abfrage mit mehreren ORDER BY-Eigenschaften sollten Sie einen [zusammengesetzten Index](index-policy.md#composite-indexes) für die Felder definieren, die sortiert werden.

## <a name="examples"></a>Beispiele

Hier sehen Sie beispielsweise eine Abfrage, mit der Familien sortiert nach dem Namen des Wohnorts in aufsteigender Reihenfolge abgerufen werden:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

Die Ergebnisse sind wie folgt:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Mit der folgenden Abfrage wird die `id` von Familien in der Reihenfolge abgerufen, in der das Element erstellt wurde. Das Element `creationDate` ist eine Zahl, die die *Epoche* repräsentiert, also die seit dem 1. Januar 1970 verstrichene Zeit in Sekunden.

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

Die Ergebnisse sind wie folgt:

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

Darüber hinaus können Sie nach mehreren Eigenschaften sortieren. Eine Abfrage, die nach mehreren Eigenschaften sortiert, erfordert einen [zusammengesetzten Index](index-policy.md#composite-indexes). Betrachten Sie die folgende Abfrage:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Diese Abfrage ruft die `id` der Familien in aufsteigender Reihenfolge nach dem Namen der Stadt ab. Wenn mehrere Elemente den gleichen Ort aufweisen, sortiert die Abfrage in absteigender Reihenfolge nach dem `creationDate`.

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [SELECT-Klausel](sql-query-select.md)
- [OFFSET LIMIT-Klausel](sql-query-offset-limit.md)
