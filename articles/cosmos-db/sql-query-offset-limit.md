---
title: OFFSET LIMIT-Klausel in Azure Cosmos DB
description: Erfahren Sie mehr zur OFFSET LIMIT-Klausel für Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 7aae56783f83f13b50321c88d69f07d910e589dd
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326871"
---
# <a name="offset-limit-clause-in-azure-cosmos-db"></a>OFFSET LIMIT-Klausel in Azure Cosmos DB

Die OFFSET LIMIT-Klausel ist eine optionale Klausel zum Überspringen einer bestimmten Anzahl von Werten aus der Abfrage. In der OFFSET LIMIT-Klausel müssen sowohl für OFFSET als auch für LIMIT Werte angegeben werden.

Wenn OFFSET LIMIT in Verbindung mit einer ORDER BY-Klausel verwendet wird, wird das Resultset erstellt, indem zunächst Werte übersprungen und dann die sortierten Werte angenommen werden. Wenn keine ORDER BY-Klausel verwendet wird, gilt eine deterministische Reihenfolge der Werte.

## <a name="syntax"></a>Syntax
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Argumente

- `<offset_amount>`

   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse überspringen sollen.

- `<limit_amount>`
  
   Mit diesem Argument wird die ganzzahlige Anzahl von Elementen festgelegt, die die Abfrageergebnisse enthalten sollen.

## <a name="remarks"></a>Anmerkungen
  
  In der OFFSET LIMIT-Klausel müssen sowohl für OFFSET als auch für LIMIT Werte angegeben werden. Wenn eine optionale `ORDER BY`-Klausel verwendet wird, wird das Resultset durch Überspringen der sortierten Werte erzeugt. Andernfalls gibt die Abfrage eine feste Reihenfolge der Werte zurück. Derzeit wird diese Klausel nur für Abfragen in einer einzelnen Partition unterstützt, partitionsübergreifende Abfragen unterstützen sie noch nicht.

## <a name="examples"></a>Beispiele

Dies ist z. B. eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (in der Reihenfolge der Wohnortnamen) zurückgibt:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind wie folgt:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Dies ist eine Abfrage, die den ersten Wert überspringt und den zweiten Wert (ohne Sortierung) zurückgibt:

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Die Ergebnisse sind wie folgt:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte](sql-query-getting-started.md)
- [SELECT-Klausel](sql-query-select.md)
- [ORDER BY-Klausel](sql-query-order-by.md)
