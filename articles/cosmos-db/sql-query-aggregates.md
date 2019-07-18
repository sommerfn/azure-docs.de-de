---
title: Aggregatfunktionen in Azure Cosmos DB
description: Erfahren Sie mehr zur Syntax einer SQL-Aggregatfunktion für Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: a6937e9e811ea8e44eda6f2bcb5d2c7d78db4934
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343173"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Aggregatfunktionen in Azure Cosmos DB

Aggregatfunktionen führen eine Berechnung für eine Gruppe von Werten in der SELECT-Klausel durch und geben einen einzelnen Wert zurück. Bei der folgenden Abfrage wird beispielsweise die Anzahl von Elementen im `Families`-Container zurückgegeben:

## <a name="examples"></a>Beispiele

```sql
    SELECT COUNT(1)
    FROM Families f
```

Die Ergebnisse sind wie folgt:

```json
    [{
        "$1": 2
    }]
```

Sie können auch nur den skalaren Wert des Aggregats zurückgeben, indem Sie das Schlüsselwort VALUE verwenden. Bei dieser Abfrage wird beispielsweise die Anzahl der Werte als einzelne Zahl zurückgegeben:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Die Ergebnisse sind wie folgt:

```json
    [ 2 ]
```

Sie können auch Aggregationen mit Filtern kombinieren. Bei der folgenden Abfrage wird beispielsweise die Anzahl von Elementen mit dem Bundesstaat `WA` in der Adresse zurückgegeben.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Die Ergebnisse sind wie folgt:

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>Typen von Aggregatfunktionen

Die SQL-API unterstützt die folgenden Aggregatfunktionen. SUM und AVG werden für numerische Werte ausgeführt. COUNT, MIN und MAX werden für Zahlen, Zeichenfolgen, boolesche Werte und NULL-Werte verwendet.

| Funktion | BESCHREIBUNG |
|-------|-------------|
| COUNT | Gibt die Anzahl der Elemente im Ausdruck zurück. |
| SUM   | Gibt die Summe aller Werte im Ausdruck zurück. |
| MIN   | Gibt den Mindestwert im Ausdruck zurück. |
| MAX   | Gibt den maximalen Wert im Ausdruck zurück. |
| DURCHSCHN.   | Gibt den Durchschnitt aller Werte im Ausdruck zurück. |

Sie können auch die Ergebnisse einer Array-Iteration aggregieren.

> [!NOTE]
> Im Daten-Explorer des Azure-Portals geben Aggregationsabfragen möglicherweise die teilweise aggregierten Ergebnisse für nur eine Abfrageseite zurück. Das SDK erzeugt einen einzelnen kumulativen Wert für alle Seiten. Zum Durchführen von Aggregationsabfragen mithilfe von Code benötigen Sie .NET SDK 1.12.0, .NET Core SDK 1.1.0 oder Java SDK 1.9.5 oder höher.

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [System functions (Systemfunktionen)](sql-query-system-functions.md)
- [User defined functions (UDFs) in Azure Cosmos DB (Benutzerdefinierte Funktionen (UDFs) in Azure Cosmos DB)](sql-query-udfs.md)