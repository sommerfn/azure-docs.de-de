---
title: Benutzerdefinierte Funktionen (UDFs) in Azure Cosmos DB
description: Erfahren Sie mehr zu benutzerdefinierten Funktionen in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: b67202da7293ef55cfe3390ca676f7944da80fba
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69614338"
---
# <a name="user-defined-functions-udfs-in-azure-cosmos-db"></a>Benutzerdefinierte Funktionen (UDFs) in Azure Cosmos DB

Die SQL-API bietet Unterstützung für benutzerdefinierte Funktionen (UDFs). Bei skalaren UDFs können null oder mehrere Argumente übergeben und ein einziges Argumentergebnis zurückgegeben werden. Die API überprüft, ob jedes Argument ein gültiger JSON-Wert ist.  

Die API erweitert die SQL-Syntax, um benutzerdefinierte Anwendungslogik mit benutzerdefinierten Funktionen (UDFs) zu unterstützen. Sie können UDFs mit der SQL-API registrieren und in SQL-Abfragen referenzieren. Genau genommen sind UDFs für den Aufruf in Abfragen konzipiert. Als Nebeneffekt haben UDFs im Gegensatz zu anderen JavaScript-Typen, z.B. gespeicherte Prozeduren und Trigger, keinen Zugriff auf das Kontextobjekt. Abfragen sind schreibgeschützt und können entweder auf primären oder sekundären Replikaten ausgeführt werden. UDFs wurden im Gegensatz zu anderen JavaScript-Typen für die Ausführung auf sekundären Replikaten entwickelt.

Im folgenden Beispiel wird eine UDF unter einem Elementcontainer in der Cosmos-Datenbank registriert. Im Beispiel wird eine UDF mit dem Namen `REGEX_MATCH` erstellt. Es werden die beiden JSON-Zeichenfolgenwerte `input` und `pattern` akzeptiert, und es wird überprüft, ob der erste Wert mit dem im zweiten Wert angegebenen Muster übereinstimmt. Hierfür wird die JavaScript-Funktion `string.match()` verwendet.

## <a name="examples"></a>Beispiele

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                      return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

Verwenden Sie nun diese UDF in einer Abfrageprojektion. Sie müssen UDFs mit dem Präfix `udf.` unter Berücksichtigung der Groß-/Kleinschreibung qualifizieren, wenn sie innerhalb von Abfragen aufgerufen werden.

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

Die Ergebnisse sind wie folgt:

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

Sie können die UDF, die mit dem Präfix `udf.` qualifiziert ist, innerhalb eines Filters verwenden, wie es im folgenden Beispiel gezeigt wird:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

Die Ergebnisse sind wie folgt:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

UDFs sind gültige skalare Ausdrücke, die Sie sowohl in Projektionen als auch in Filtern verwenden können.

Zum Erweitern des Funktionsumfangs von UDFs sehen Sie hier ein weiteres Beispiel mit konditionaler Logik:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

Hier sehen Sie ein Verwendungsbeispiel für die UDF:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

Die Ergebnisse sind wie folgt:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Wenn die von den UDF-Parametern referenzierten Eigenschaften nicht im JSON-Wert verfügbar sind, wird der Parameter als „Undefined“ betrachtet und der UDF-Aufruf übersprungen. Wenn das Ergebnis der UDF „Undefined“ ist, wird es ebenfalls nicht in das Ergebnis aufgenommen.

Wie im vorherigen Beispiel gezeigt, vereinen UDFs den Funktionsumfang der JavaScript-Sprache mit der SQL-API. UDFs bieten eine umfassende, programmierbare Schnittstelle für komplexe verfahrensorientierte und konditionale Logik mithilfe der Fähigkeiten der integrierten JavaScript-Laufzeit. Die SQL-API stellt den UDFs die Argumente für jedes Quellelemente in der aktuellen Ausführungsphase der WHERE- oder SELECT-Klausel bereit. Das Ergebnis wird nahtlos in die Gesamt-Ausführungspipeline aufgenommen. UDFs sind also hervorragende Werkzeuge, um komplexe Geschäftslogik als Teil der Abfragen zu integrieren.

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [Systemfunktionen](sql-query-system-functions.md)
- [Aggregate](sql-query-aggregates.md)