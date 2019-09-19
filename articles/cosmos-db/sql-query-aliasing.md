---
title: Aliase in Azure Cosmos DB
description: Erfahren Sie mehr über Aliaswerte in SQL-Abfragen in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: 3b17cbc7710647b1e1875025a1db1849034ec1dc
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002069"
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

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT-Klausel](sql-query-select.md)
- [FROM-Klausel](sql-query-from.md)
