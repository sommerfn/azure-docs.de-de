---
title: Parametrisierte Abfragen in Azure Cosmos DB
description: Erfahren Sie mehr über parametrisierte SQL-Abfragen
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 2bfc22346c1dd43d7d3c2937ffc286e48ae774d0
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343258"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Parametrisierte Abfragen in Azure Cosmos DB

Cosmos DB unterstützt Abfragen mit Parametern, die durch die bekannte @-Notation ausgedrückt werden. Parametrisiertes SQL bietet stabile Fehlerbehandlung und Schutz von Benutzereingaben und verhindert eine versehentliche Offenlegung von Daten durch SQL-Injektion.

## <a name="examples"></a>Beispiele

Sie können z.B. eine Abfrage erstellen, die `lastName` und `address.state` als Parameter verwendet, und sie für unterschiedliche `lastName`- und `address.state`-Werte auf Grundlage von Benutzereingaben ausführen.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Sie können diese Anforderung dann als parametrisierte JSON-Abfrage wie die folgende an Cosmos DB senden:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Im folgenden Beispiel wird das Argument für TOP mit einer parametrisierten Abfrage festgelegt: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parameterwerte können alle gültigen JSON-Werte sein: Zeichenfolgen, Zahlen, boolesche Werte, Null, Arrays oder verschachteltes JSON. Da Cosmos DB schemalos ist, werden Parameter für keinen Typ überprüft.


## <a name="next-steps"></a>Nächste Schritte

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Modelldokumentdaten](modeling-data.md)
