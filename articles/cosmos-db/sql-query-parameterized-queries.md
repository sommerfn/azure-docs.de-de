---
title: Parametrisierte Abfragen in Azure Cosmos DB
description: Erfahren Sie mehr über parametrisierte SQL-Abfragen
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 45c1344c32e35f60f35ba8ed105e912d92574cce
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003613"
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

- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Modelldokumentdaten](modeling-data.md)
