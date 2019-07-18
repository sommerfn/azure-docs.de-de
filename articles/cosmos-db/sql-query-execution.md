---
title: Ausführen von SQL-Abfragen in Azure Cosmos DB
description: Erfahren Sie mehr über das Ausführen von SQL-Abfragen in Azure Cosmos DB.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: e4e26b658bd29e4589be40e4d29935059836c909
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343207"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>Ausführen von SQL-Abfragen in Azure Cosmos DB

Die Cosmos DB-REST-API kann mit jeder Sprache aufgerufen werden, die HTTP/HTTPS-Anfragen unterstützt. Cosmos DB bietet außerdem Programmierbibliotheken für die Programmiersprachen .NET, Node.js, JavaScript und Python. Die REST-API und Bibliotheken unterstützen allesamt SQL-Abfragen, und das .NET SDK unterstützt auch [LINQ-Abfragen](sql-query-linq-to-sql.md).

Die folgenden Beispiele zeigen, wie Sie eine Abfrage erstellen und auf einem Cosmos DB-Datenbankkonto ausführen können.

## <a id="REST-API"></a>REST-API

Cosmos DB bietet ein RESTful-Programmiermodell über HTTP. Das Ressourcenmodell besteht aus einem Satz von Ressourcen in einem Datenbankkonto, das von einem Azure-Abonnement bereitgestellt wird. Das Datenbankkonto besteht aus einem Satz von *Datenbanken*, die jeweils mehrere *Container* enthalten können, die wiederum *Elemente*, UDFs und andere Ressourcentypen enthalten. Jede Cosmos DB-Ressource kann über einen logischen und beständigen URI adressiert werden. Ein Satz von Ressourcen wird als *Feed* bezeichnet. 

Die Interaktion mit diesen Ressourcen erfolgt über die HTTP-Verben `GET`, `PUT`, `POST` und `DELETE` mit deren Standardinterpretation. Verwenden Sie `POST` zum Erstellen einer neuen Ressource, Ausführen einer gespeicherten Prozedur oder Ausgeben einer Cosmos DB-Abfrage. Abfragen werden immer als schreibgeschützte Operationen ohne Nebeneffekte ausgeführt.

Die folgenden Beispiele zeigen einen `POST`-Vorgang für eine SQL-API-Abfrage der Beispielelemente. Die Abfrage enthält einen einfachen Filter auf die JSON-Eigenschaft `name`. Die Header `x-ms-documentdb-isquery` und Content-Type: `application/query+json` geben an, dass es sich bei diesem Vorgang um eine Abfrage handelt. Ersetzen Sie `mysqlapicosmosdb.documents.azure.com:443` durch den URI für Ihr Cosmos DB-Konto.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

Die Ergebnisse sind wie folgt:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

Die nächste, komplexere Abfrage gibt mehrere Ergebnisse aus einer Verknüpfung zurück:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": []
    }
```

Die Ergebnisse sind wie folgt: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Wenn die Abfrageergebnisse nicht auf eine einzelne Seite passen, gibt die REST-API ein Fortsetzungstoken im `x-ms-continuation-token`-Antwortheader zurück. Clients können einzelne Ergebnisseiten abfragen, indem sie den Header in den nachfolgenden Abfragen angeben. Sie können auch die Anzahl der Ergebnisse pro Seite über den `x-ms-max-item-count`-Header steuern.

Wenn eine Abfrage eine Aggregatfunktion wie COUNT aufweist, gibt die Abfrageseite möglicherweise einen teilweise aggregierten Wert für nur eine Ergebnisseite zurück. Clients müssen eine Aggregation auf zweiter Ebene für diese Ergebnisse ausführen, um die Endergebnisse zu erzeugen, z.B. die Summe für die auf den einzelnen Seiten zurückgegebenen Anzahlen, um die Gesamtanzahl zurückzugeben.

Zur Verwaltung der Datenkonsistenzrichtlinie für Abfragen verwenden Sie den `x-ms-consistency-level`-Header wie in allen REST-API-Anforderungen. Für die Sitzungskonsistenz muss außerdem der letzte `x-ms-session-token` -Cookie-Header in der Abfrageanforderung wiederholt werden. Die Indizierungsrichtlinie des abgefragten Containers kann auch die Konsistenz der Abfrageergebnisse beeinflussen. Mit den Standardeinstellungen für Indizierungsrichtlinien stimmt der Index eines Containers immer mit dem aktuellen Elementinhalt überein, und die Abfrageergebnisse geben die Daten mit der gewünschten Konsistenz zurück. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB][consistency-levels].

Wenn die konfigurierte Indizierungsrichtlinie des Containers die angegebene Abfrage nicht unterstützen kann, gibt der Azure Cosmos DB-Server den Fehler 400 (unzulässige Anforderung) zurück. Diese Fehlermeldung wird bei Abfragen mit Pfaden zurückgegeben, die ausdrücklich von der Indizierung ausgeschlossen sind. Sie können den `x-ms-documentdb-query-enable-scan`-Header angeben, damit die Abfrage einen Suchvorgang durchführen darf, wenn kein Index verfügbar ist.

Sie können ausführliche Metriken zur Abfrageausführung abrufen, indem Sie den `x-ms-documentdb-populatequerymetrics`-Header auf `true` festlegen. Weitere Informationen finden Sie unter [SQL-Abfragemetriken für Azure Cosmos DB](sql-api-query-metrics.md).

## <a name="c-net-sdk"></a>C# (.NET SDK)

Das .NET SDK unterstützt Abfragen per LINQ und SQL. Das folgende Beispiel zeigt, wie Sie die vorherige Filterabfrage mit .NET ausführen:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Das folgende Beispiel vergleicht zwei Eigenschaften auf Gleichheit in den einzelnen Elementen und verwendet anonyme Projektionen.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Das nächste Beispiel zeigt Verknüpfungen mithilfe von LINQ `SelectMany`.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

Der .NET-Client durchläuft automatisch alle Seiten der Abfrageergebnisse in den `foreach`-Blöcken wie im vorherigen Beispiel gezeigt. Die im Abschnitt zur [REST-API](#REST-API) vorgestellten Abfrageoptionen sind auch im .NET SDK über die Klassen `FeedOptions` and `FeedResponse` in der `CreateDocumentQuery`-Methode verfügbar. Sie können die Anzahl der Seiten über die `MaxItemCount`-Einstellung steuern.

Sie können die Seitenaufteilung steuern, indem Sie mithilfe des `IQueryable`-Objekts ein `IDocumentQueryable`-Element erstellen, die `ResponseContinuationToken`-Werte lesen und sie anschließend als `RequestContinuationToken` in `FeedOptions` zurückgeben. Sie können `EnableScanInQuery` festlegen, um Suchvorgänge zu ermöglichen, wenn die Abfrage nicht von der konfigurierten Indizierungsrichtlinie unterstützt wird. Für partitionierte Container können Sie `PartitionKey` verwenden, um die Abfrage für eine einzelne Partition auszuführen, auch wenn Azure Cosmos DB dies automatisch aus dem Abfragetext extrahieren kann. Mit `EnableCrossPartitionQuery` können Sie Abfragen für mehrere Partitionen ausführen.

Weitere Beispiele für .NET mit Abfragen finden Sie unter den [Azure Cosmos DB-.NET-Beispielen](https://github.com/Azure/azure-cosmosdb-dotnet) in GitHub.

## <a id="JavaScript-server-side-API"></a>Serverseitige JavaScript-API

Azure Cosmos DB bietet ein Programmiermodell zur [Ausführung JavaScript-basierter Anwendungslogik](stored-procedures-triggers-udfs.md) direkt für Container über gespeicherte Prozeduren und Trigger. Die auf Containerebene registrierte JavaScript-Logik kann anschließend Datenbankvorgänge für die Elemente der jeweiligen Container ausführen, die in ACID-Transaktionen der jeweiligen Umgebung gekapselt werden.

Das folgende Beispiel zeigt, wie Sie mithilfe von `queryDocuments` in der serverseitigen JavaScript-API Abfragen aus gespeicherten Prozeduren und Triggern heraus ausführen können:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Cosmos DB](introduction.md)
- [Azure Cosmos DB-.NET-Beispiele](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)
