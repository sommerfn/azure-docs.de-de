---
title: Abfragen von Daten mit der API für MongoDB von Azure Cosmos DB
description: Erfahren Sie, wie Sie Daten mit der API für MongoDB von Azure Cosmos DB abfragen können.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: tutorial
ms.date: 12/26/2018
ms.reviewer: sngun
ms.openlocfilehash: 40385524e85f950fb32b69817fec27d842370736
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754765"
---
# <a name="query-data-by-using-azure-cosmos-dbs-api-for-mongodb"></a>Abfragen von Daten mit der API für MongoDB von Azure Cosmos DB

Die [API für MongoDB von Azure Cosmos DB](mongodb-introduction.md) unterstützt [MongoDB-Abfragen](https://docs.mongodb.com/manual/tutorial/query-documents/). 

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Abfragen von in der Cosmos-Datenbank gespeicherten Daten mithilfe der MongoDB-Shell

Sie können mithilfe der Beispiele in diesem Dokument loslegen und sich das Video [Query Azure Cosmos DB with MongoDB shell (Abfragen von Azure Cosmos DB mit MongoDB-Shell)](https://azure.microsoft.com/resources/videos/query-azure-cosmos-db-data-by-using-the-mongodb-shell/) ansehen.

## <a name="sample-document"></a>Beispieldokument

Die Abfragen in diesem Artikel verwenden das folgende Beispieldokument.

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
        "gender": "female", "grade": 1,
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
## <a id="examplequery1"></a> Beispielabfrage 1 

Im Beispiel des obigen Familienbeispieldokuments gibt die folgende Abfrage die Dokumente zurück, in denen das Feld „id“ mit `WakefieldFamily` übereinstimmt.

**Abfrage**
    
    db.families.find({ id: "WakefieldFamily"})

**Ergebnisse**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
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
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Beispielabfrage 2 

Die nächste Abfrage gibt alle Kinder in der Familie zurück. 

**Abfrage**
    
    db.families.find( { id: "WakefieldFamily" }, { children: true } )

**Ergebnisse**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
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
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a> Beispielabfrage 3 

Die nächste Abfrage gibt alle registrierten Familien zurück. 

**Abfrage**
    
    db.families.find( { "isRegistered" : true })
**Ergebnisse** Kein Dokument wird zurückgegeben. 

## <a id="examplequery4"></a> Beispielabfrage 4

Die nächste Abfrage gibt alle nicht registrierten Familien zurück. 

**Abfrage**
    
    db.families.find( { "isRegistered" : false })
**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a> Beispielabfrage 5

Die nächste Abfrage gibt alle nicht registrierten Familien im Bundesstaat NY zurück. 

**Abfrage**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a> Beispielabfrage 6

Die nächste Abfrage gibt alle Familien zurück, deren Kinder in Klasse 8 gehen.

**Abfrage**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Ergebnisse**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a> Beispielabfrage 7

Die nächste Abfrage gibt alle Familien zurück, wo das Kinderarray die Größe 3 hat.

**Abfrage**
  
      db.Family.find( {children: { $size:3} } )

**Ergebnisse**

Da keine Familien mit mehr als zwei Kindern vorhanden sind, werden keine Ergebnisse zurückgegeben. Nur wenn der Parameter 2 ist, ist diese Abfrage erfolgreich und gibt das vollständige Dokument zurück.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Sie haben erfahren, wie Sie Abfragen mithilfe der API für MongoDB von Cosmos DB ausführen können.

Sie können jetzt mit dem nächsten Tutorial fortfahren, um zu erfahren, wie Sie Ihre Daten global verteilen.

> [!div class="nextstepaction"]
> [Globales Verteilen Ihrer Daten](tutorial-global-distribution-sql-api.md)

