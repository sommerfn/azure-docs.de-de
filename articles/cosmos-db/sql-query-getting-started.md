---
title: Erste Schritte mit SQL-Abfragen in Azure Cosmos DB
description: Einführung in SQL-Abfragen
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 8de5140d0146ccbb18f41867e1c716aa2f3897b7
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71001909"
---
# <a name="getting-started-with-sql-queries"></a>Erste Schritte mit SQL-Abfragen

SQL-API-Konten in Azure Cosmos DB unterstützen das Abfragen von Elementen mit SQL (Structured Query Language) als JSON-Abfragesprache. Der Entwurf der Azure Cosmos DB-Abfragesprache hat folgende Zielsetzungen:

* Unterstützung von SQL, einer der meistverwendeten und beliebtesten Abfragesprachen, anstelle der Entwicklung einer neuen Abfragesprache. SQL bietet ein formelles Programmiermodell zur Durchführung umfassender Abfragen für JSON-Elemente.  

* Verwendung des Programmiermodells von JavaScript als Grundlage für die Abfragesprache. Das Typsystem, die Ausdrucksauswertung und der Funktionsaufruf von JavaScript stellen die Grundlage der SQL-API dar. Mit dieser Grundlage erhalten Sie ein natürliches Programmiermodell für Features wie relationale Projektionen, hierarchische übergreifende Navigation für JSON-Elemente, Selbstverknüpfungen, Abfragen räumlicher Daten und das Aufrufen vollständig in JavaScript geschriebener, benutzerdefinierter Funktionen (User Defined Functions, UDFs).

## <a name="upload-sample-data"></a>Hochladen von Beispieldaten

Erstellen Sie in Ihrem Cosmos DB-Konto der SQL-API einen Container mit dem Namen `Families`. Erstellen Sie zwei einfache JSON-Elemente im Container. Sie können die meisten Beispielabfragen in der Azure Cosmos DB-Abfragedokumentation mithilfe dieses Datasets ausführen.

### <a name="create-json-items"></a>Erstellen von JSON-Elementen

Der folgende Code erstellt zwei einfache JSON-Elemente zu Familien. Die einfachen JSON-Elemente für die Familien Andersen und Wakefield umfassen Eltern, Kinder und deren Haustiere, Adressen und Registrierungsinformationen. Das erste Element enthält Zeichenfolgen, Zahlen, boolesche Werte, Arrays und geschachtelte Eigenschaften.


```json
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

Das zweite Element verwendet `givenName` und `familyName` anstelle von `firstName` und `lastName`.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>Abfragen der JSON-Elemente

Führen Sie einige Abfragen für die JSON-Daten aus, um einige der Schlüsselaspekte der SQL-Abfragesprache von Azure Cosmos DB besser zu verstehen.

Die folgende Abfrage gibt die Elemente zurück, in denen das `id`-Feld dem Text `AndersenFamily` entspricht. Da es sich um eine `SELECT *`-Abfrage handelt, ist die Ausgabe der Abfrage das komplette JSON-Element. Weitere Informationen zur SELECT-Syntax finden Sie unter [SELECT-Anweisung](sql-query-select.md). 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Die Abfrage hat folgende Ergebnisse: 

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

Mit der folgenden Abfrage wird die JSON-Ausgabe auf andere Weise neu formatiert. Die Abfrage projiziert ein neues JSON-Objekt `Family` mit zwei ausgewählten Feldern (`Name` und `City`), wenn die Stadt in der Adresse dem Staat entspricht. Dies ist bei „NY, NY“ der Fall.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Die Abfrage hat folgende Ergebnisse:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

Die folgende Abfrage gibt alle Vornamen von Kindern der Familie zurück, deren `id` dem Text `WakefieldFamily` entspricht, und zwar geordnet nach der Stadt.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Die Ergebnisse sind wie folgt:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

## <a name="remarks"></a>Anmerkungen

Die Beispiele oben zeigen verschiedene Aspekte der Cosmos DB-Abfragesprache:  

* Da die SQL-API mit JSON-Werten arbeitet, werden baumförmige Entitäten anstelle von Spalten und Zeilen verarbeitet. Sie können auf die Baumknoten in beliebiger Tiefe verweisen, z.B. `Node1.Node2.Node3…..Nodem`, ähnlich dem zweiteiligen Verweis `<table>.<column>` in ANSI SQL.

* Da die Abfragesprache mit schemalosen Daten arbeitet, muss das Typsystem dynamisch gebunden werden. Der gleiche Ausdruck kann unterschiedliche Typen in unterschiedlichen Elementen ergeben. Das Ergebnis einer Abfrage ist ein gültiger JSON-Wert, aber nicht garantiert innerhalb eines festen Schemas.  

* Azure Cosmos DB unterstützt nur strikte JSON-Elemente. Typsystem und Ausdrücke sind auf JSON-Typen beschränkt. Weitere Informationen finden Sie in der [JSON-Spezifikation](https://www.json.org/).  

* Ein Cosmos-Container ist eine schemalose Sammlung von JSON-Elementen. Die Beziehungen innerhalb und zwischen Containerelementen werden implizit nach Eigenständigkeit erfasst, nicht nach Beziehungen von primären Schlüsseln und Fremdschlüsseln. Dieses Feature ist für die elementinternen Verknüpfungen wichtig, die später in diesem Artikel erläutert werden.

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT-Klausel](sql-query-select.md)
