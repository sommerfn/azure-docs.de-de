---
title: Aliase in Azure Cosmos DB
description: Erfahren Sie mehr über Aliaswerte in SQL-Abfragen in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: e532fb7180af8a21de6ae9a2e4d798abd9e93e7b
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343243"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Aliase in Azure Cosmos DB

Sie können explizit Aliase für Werte in Abfragen verwenden. Wenn eine Abfrage zwei Eigenschaften mit demselben Namen hat, verwenden Sie Aliase, um mindestens eine der Eigenschaften umzubenennen, sodass sie im projizierten Ergebnis eindeutig sind.

## <a name="examples"></a>Beispiele

Das für die Aliasvergabe verwendete AS-Schlüsselwort ist optional, wie es im folgenden Beispiel bei der Projektion des zweiten Werts als `NameInfo` gezeigt wird:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Die Ergebnisse sind wie folgt:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmosdb-dotnet)
- [SELECT-Klausel](sql-query-select.md)
- [FROM-Klausel](sql-query-from.md)
