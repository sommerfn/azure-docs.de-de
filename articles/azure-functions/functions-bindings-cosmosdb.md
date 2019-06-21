---
title: Azure Cosmos DB-Bindungen für Functions 1.x
description: Grundlegendes zur Verwendung von Azure Cosmos DB-Triggern und -Bindungen in Azure Functions.
services: functions
author: craigshoemaker
ms.author: cshoe
manager: jeconnoc
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: azure-functions
ms.topic: reference
ms.date: 11/21/2017
ms.custom: seodec18
ms.openlocfilehash: 0421ec62d25bbfaba2909d16498cac5afd038a53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60737277"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-1x"></a>Azure Cosmos DB-Bindungen für Azure Functions 1.x

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version der Azure Functions-Runtime aus: "]
> * [Version 1](functions-bindings-cosmosdb.md)
> * [Version 2](functions-bindings-cosmosdb-v2.md)

Dieser Artikel erläutert das Arbeiten mit [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md)-Bindungen in Azure Functions. Azure Functions unterstützt Trigger sowie Ein- und Ausgabebindungen für Azure Cosmos DB.

> [!NOTE]
> Dieser Artikel gilt für Azure Functions 1.x. Informationen zur Verwendung dieser Bindungen in Functions 2.x finden Sie unter [Azure Cosmos DB-Bindungen für Azure Functions 2.x](functions-bindings-cosmosdb-v2.md).
>
>Diese Bindung hatte ursprünglich die Bezeichnung „DocumentDB“. In Functions Version 1.x wurde nur der Trigger in „Cosmos DB“ umbenannt. Die Eingabebindung, die Ausgabebindung und das NuGet-Paket behalten den Namen „DocumentDB“.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Azure Cosmos DB-Bindungen werden nur in Kombination mit der SQL-API unterstützt. Für alle anderen Azure Cosmos DB-APIs (einschließlich [Azure Cosmos DB-API für MongoDB](../cosmos-db/mongodb-introduction.md), [Cassandra-API](../cosmos-db/cassandra-introduction.md), [Gremlin-API](../cosmos-db/graph-introduction.md) und [Tabellen-API](../cosmos-db/table-introduction.md)) müssen Sie für den Datenbankzugriff aus Ihrer Funktion den statischen Client für Ihre API verwenden.

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Die Azure Cosmos DB-Bindungen für die Functions-Version 1.x werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB) (Version 1.x) bereitgestellt. Den Quellcode für die Bindungen finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.DocumentDB).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="trigger"></a>Trigger

Informationen zum partitionsübergreifenden Lauschen auf Einfügungen und Aktualisierungen durch den Azure Cosmos DB-Trigger finden Sie unter [Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB](../cosmos-db/change-feed.md). Der Änderungsfeed veröffentlicht Einfügungen und Updates, keine Löschungen.

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#trigger---c-example)
* [C#-Skript (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Trigger-Beispiele überspringen](#trigger---attributes)

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die aufgerufen wird, wenn etwas in der angegebenen Datenbank und Sammlung eingefügt oder aktualisiert wird.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV1
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents,
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Trigger-Beispiele überspringen](#trigger---attributes)

### <a name="trigger---c-script-example"></a>Trigger: C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Cosmos DB-Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion schreibt Protokollmeldungen, wenn Cosmos DB-Datensätze geändert werden.

Bindungsdaten in der Datei *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
    }
```

[Trigger-Beispiele überspringen](#trigger---attributes)

### <a name="trigger---javascript-example"></a>Trigger: JavaScript-Beispiel

Das folgende Beispiel zeigt eine Cosmos DB-Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion schreibt Protokollmeldungen, wenn Cosmos DB-Datensätze geändert werden.

Bindungsdaten in der Datei *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

## <a name="trigger---attributes"></a>Trigger: Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs)-Attribut.

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie unter [Trigger: Konfiguration](#trigger---configuration). Hier ist ein Beispiel für ein `CosmosDBTrigger`-Attribut in einer Methodensignatur:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Ein vollständiges Beispiel finden Sie unter [Trigger: C#-Beispiel](#trigger---c-example).

## <a name="trigger---configuration"></a>Trigger: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `CosmosDBTrigger` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type** || Muss auf `cosmosDBTrigger` festgelegt sein. |
|**direction** || Muss auf `in` festgelegt sein. Dieser Parameter wird automatisch festgelegt, wenn Sie den Trigger im Azure Portal erstellen. |
|**name** || Der im Code der Funktion verwendete Variablenname, der die Liste der Dokumente mit Änderungen darstellt. |
|**connectionStringSetting**|**ConnectionStringSetting** | Der Name einer App-Einstellung, die die Verbindungszeichenfolge enthält, die zum Herstellen der Verbindung mit dem überwachten Azure Cosmos DB-Konto verwendet wird. |
|**databaseName**|**DatabaseName**  | Der Name der Azure Cosmos DB-Datenbank mit der überwachten Sammlung. |
|**collectionName** |**CollectionName** | Der Name der überwachten Sammlung. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Optional) Der Name einer App-Einstellung, die die Verbindungszeichenfolge für den Dienst enthält, in dem die Leasesammlung enthalten ist. Wenn nicht festgelegt, wird der Wert `connectionStringSetting` verwendet. Dieser Parameter wird automatisch festgelegt, wenn die Bindung im Portal erstellt wird. Die Verbindungszeichenfolge für die Leasesammlung muss über Schreibberechtigungen verfügen.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Optional) Der Name der Datenbank, in der die Sammlung zum Speichern von Leases enthalten ist. Wenn nicht festgelegt, wird der Wert der `databaseName`-Einstellung verwendet. Dieser Parameter wird automatisch festgelegt, wenn die Bindung im Portal erstellt wird. |
|**leaseCollectionName** | **LeaseCollectionName** | (Optional) Der Name der Sammlung, die zum Speichern von Leases verwendet wird. Wenn nicht festgelegt, wird der Wert `leases` verwendet. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Optional) Bei Festlegung auf `true` wird die Sammlung von Leases automatisch erstellt, wenn sie nicht bereits vorhanden ist. Standardwert: `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (Optional) Definiert die Anzahl von Anforderungseinheiten, die zugewiesen werden, wenn die Leasesammlung erstellt wird. Diese Einstellung wird nur verwendet, wenn `createLeaseCollectionIfNotExists` auf `true` festgelegt ist. Dieser Parameter wird automatisch festgelegt, wenn die Bindung im Portal erstellt wird.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| (Optional) Wenn gesetzt, wird ein Präfix zu den Leases hinzugefügt, die in der Leasesammlung für diese Funktion erstellt wurden, sodass zwei separate Azure-Funktionen dieselbe Leasesammlung mit unterschiedlichen Präfixen gemeinsam nutzen können.
|**feedPollDelay**| **FeedPollDelay**| (Optinal) Wenn gesetzt, wird die Verzögerung in Millisekunden zwischen den Abfragen an eine Partition nach neuen Änderungen auf dem Feed definiert, nachdem alle aktuellen Änderungen beseitigt wurden. Der Standardwert ist 5000 (5 Sekunden).
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (Optional) Wenn gesetzt, wird das Intervall in Millisekunden definiert, das eine Aufgabe anstößt, die berechnet, ob Partitionen unter den bekannten Hostinstanzen gleichmäßig verteilt sind. Der Standardwert ist 13000 (13 Sekunden).
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (Optional) Wenn gesetzt, wird das Intervall in Millisekunden definiert, für das die Lease für eine Lease, die eine Partition darstellt, ausgeführt wird. Wenn die Lease innerhalb dieses Intervalls nicht erneuert wird, läuft sie ab, und der Besitz der Partition wechselt zu einer anderen Instanz. Der Standardwert ist 60000 (60 Sekunden).
|**leaseRenewInterval**| **LeaseRenewInterval**| (Optional) Wenn gesetzt, wird das Erneuerungsintervall in Millisekunden für alle Leases für Partitionen definiert, die aktuell in einer Instanz vorhanden sind. Der Standardwert ist 17000 (17 Sekunden).
|**checkpointFrequency**| **CheckpointFrequency**| (Optional) Wenn gesetzt, wird das Intervall in Millisekunden zwischen Leaseprüfpunkten definiert. Dies ist standardmäßig immer nach einem erfolgreichen Funktionsaufruf der Fall.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Optional) Wenn gesetzt, wird die Höchstzahl von Elementen angepasst, die von einem Funktionsaufruf empfangen werden können.
|**startFromBeginning**| **StartFromBeginning**| (Optional) Wenn dieser Parameter festgelegt ist, liest der Trigger Änderungen beginnend vom Anfang des Verlaufs der Sammlung anstatt ab der aktuellen Zeit. Dies funktioniert nur beim ersten Start des Triggers. Bei nachfolgenden Ausführungen sind die Prüfpunkte bereits gespeichert. Wenn die Leases bereits erstellt sind, hat das Festlegen auf den Wert `true` keine Auswirkungen.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Trigger: Verwendung

Der Trigger erfordert eine zweite Sammlung, die er zum Speichern von _Leases_ auf den Partitionen verwendet. Sowohl die überwachte Sammlung als auch die, die die Leases enthält, muss verfügbar sein, damit der Trigger funktioniert.

>[!IMPORTANT]
> Falls mehrere Funktionen für die Verwendung eines Cosmos DB-Triggers für die gleiche Sammlung konfiguriert sind, muss jede dieser Funktionen eine dedizierte Leasesammlung verwenden oder für jede Funktion ein anderes `LeaseCollectionPrefix` angeben. Andernfalls wird nur eine der Funktionen ausgelöst. Weitere Informationen zu diesem Präfix finden Sie im [Konfigurationsabschnitt](#trigger---configuration).

Der Trigger gibt nicht an, ob ein Dokument aktualisiert oder eingefügt wurde, er stellt das Dokument lediglich bereit. Wenn Sie Aktualisierungen und Einfügungen unterschiedlich verarbeiten müssen, können Sie dazu Zeitstempelfelder für Einfügung oder Aktualisierung implementieren.

## <a name="input"></a>Eingabe

Die Azure Cosmos DB-Eingabebindung verwendet die SQL-API, um mindestens ein Azure Cosmos DB-Dokument abzurufen und an den Eingabeparameter der Funktion zu übergeben. Die Dokument-ID oder die Abfrageparameter können basierend auf dem Trigger, der die Funktion aufruft, ermittelt werden.

## <a name="input---examples"></a>Eingabe: Beispiele

Sehen Sie sich die sprachspezifischen Beispiele an, in denen ein ID-Wert angegeben wird, um ein einzelnes Dokument zu lesen:

* [C#](#input---c-examples)
* [C#-Skript (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Eingabebeispiele überspringen](#input---attributes)

### <a name="input---c-examples"></a>Eingabe: C#-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* [Warteschlangentrigger: Suchen der ID in JSON-Code](#queue-trigger-look-up-id-from-json-c)
* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge](#http-trigger-look-up-id-from-query-string-c)
* [HTTP-Trigger: Suchen der ID in Routendaten](#http-trigger-look-up-id-from-route-data-c)
* [HTTP-Trigger: Suchen der ID in Routendaten unter Verwendung von „SqlQuery“](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „DocumentClient“](#http-trigger-get-multiple-docs-using-documentclient-c)

Die Beispiele beziehen sich auf einen einfachen `ToDoItem`-Typ:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Warteschlangentrigger: Suchen der ID in JSON-Code (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine Warteschlangennachricht ausgelöst, die ein JSON-Objekt enthält. Der Warteschlangentrigger zerlegt den JSON-Code in ein Objekt namens `ToDoItemLookup`, das die zu suchende ID enthält. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP-Trigger: Suchen der ID in Routendaten (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Routendaten verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static HttpResponseMessage Run(
            [HttpTrigger(
                AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP-Trigger: Suchen der ID in Routendaten unter Verwendung von „SqlQuery“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Routendaten verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")] IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Die Abfrage wird in der Attributeigenschaft `SqlQuery` angegeben.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „DocumentClient“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Der Code verwendet eine von der Azure Cosmos DB-Bindung bereitgestellte `DocumentClient`-Instanz, um eine Liste von Dokumenten zu lesen. Die `DocumentClient`-Instanz kann auch für Schreibvorgänge verwendet werden.

```cs
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
            string searchterm = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
                .Value;

            if (searchterm == null)
            {
                return req.CreateResponse(HttpStatusCode.NotFound);
            }

            log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

### <a name="input---c-script-examples"></a>Eingabe: C#-Skriptbeispiele

Dieser Abschnitt enthält folgende Beispiele:

* [Warteschlangentrigger: Suchen der ID in einer Zeichenfolge](#queue-trigger-look-up-id-from-string-c-script)
* [Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP-Trigger: Suchen der ID in Routendaten](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „DocumentClient“](#http-trigger-get-multiple-docs-using-documentclient-c-script)

Die Beispiele für einen HTTP-Trigger beziehen sich auf einen einfachen `ToDoItem`-Typ:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Warteschlangentrigger: Suchen der ID in einer Zeichenfolge (C#-Skript)

Das folgende Beispiel zeigt eine Cosmos DB-Eingabebindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion liest ein einzelnes Dokument und aktualisiert den Textwert des Dokuments.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
}
```
Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
        inputDocument.text = "This has changed.";
    }
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (C#-Skript)

Das folgende Beispiel zeigt eine Azure Cosmos DB-Eingabebindung in einer *function.json*-Datei sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion ruft mehrere von einer SQL-Abfrage angegebene Dokumente mithilfe eines Warteschlangentriggers ab, um die Abfrageparameter anzupassen.

Der Warteschlangentrigger stellt den Parameter `departmentId` bereit. Die Warteschlangennachricht `{ "departmentId" : "Finance" }` gibt dann alle Datensätze für die Finanzabteilung zurück.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {
        foreach (var doc in documents)
        {
            // operate on each document
        }
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge (C#-Skript)

Das folgende Beispiel zeigt eine [C#-Skriptfunktion](functions-reference-csharp.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>HTTP-Trigger: Suchen der ID in Routendaten (C#-Skript)

Das folgende Beispiel zeigt eine [C#-Skriptfunktion](functions-reference-csharp.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Routendaten verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
        log.Info($"ToDo item not found");
    }
    else
    {
        log.Info($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (C#-Skript)

Das folgende Beispiel zeigt eine [C#-Skriptfunktion](functions-reference-csharp.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Die Abfrage wird in der Attributeigenschaft `SqlQuery` angegeben.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.Info(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „DocumentClient“ (C#-Skript)

Das folgende Beispiel zeigt eine [C#-Skriptfunktion](functions-reference-csharp.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Der Code verwendet eine von der Azure Cosmos DB-Bindung bereitgestellte `DocumentClient`-Instanz, um eine Liste von Dokumenten zu lesen. Die `DocumentClient`-Instanz kann auch für Schreibvorgänge verwendet werden.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.Info(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Eingabebeispiele überspringen](#input---attributes)

### <a name="input---javascript-examples"></a>Eingabe: JavaScript-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* [Warteschlangentrigger: Suchen der ID in JSON-Code](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP-Trigger: Suchen der ID in Routendaten](#http-trigger-look-up-id-from-route-data-javascript)
* [Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Warteschlangentrigger: Suchen der ID in JSON-Code (JavaScript)

Das folgende Beispiel zeigt eine Cosmos DB-Eingabebindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion liest ein einzelnes Dokument und aktualisiert den Textwert des Dokuments.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "inputDocumentIn",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge (JavaScript)

Das folgende Beispiel zeigt eine [JavaScript-Skriptfunktion](functions-reference-node.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>HTTP-Trigger: Suchen der ID in Routendaten (JavaScript)

Das folgende Beispiel zeigt eine [JavaScript-Skriptfunktion](functions-reference-node.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

Die Datei *function.json* sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Eingabebeispiele überspringen](#input---attributes)



#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (JavaScript)

Das folgende Beispiel zeigt eine Azure Cosmos DB-Eingabebindung in einer *function.json*-Datei sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion ruft mehrere von einer SQL-Abfrage angegebene Dokumente mithilfe eines Warteschlangentriggers ab, um die Abfrageparameter anzupassen.

Der Warteschlangentrigger stellt den Parameter `departmentId` bereit. Die Warteschlangennachricht `{ "departmentId" : "Finance" }` gibt dann alle Datensätze für die Finanzabteilung zurück.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
    module.exports = function (context, input) {
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }
        context.done();
    };
```

[Eingabebeispiele überspringen](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Eingabe: F#-Beispiele

Das folgende Beispiel zeigt eine Cosmos DB-Eingabebindung in einer Datei *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion liest ein einzelnes Dokument und aktualisiert den Textwert des Dokuments.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "in"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Der F#-Code lautet wie folgt:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Dieses Beispiel erfordert die Datei `project.json`, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Informationen zum Hinzufügen einer `project.json`-Datei finden Sie unter [Paketverwaltung](functions-reference-fsharp.md#package).

## <a name="input---attributes"></a>Eingabe: Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs)-Attribut.

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie im [folgenden Konfigurationsabschnitt](#input---configuration).

## <a name="input---configuration"></a>Eingabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `DocumentDB` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**     || Muss auf `documentdb` festgelegt sein.        |
|**direction**     || Muss auf `in` festgelegt sein.         |
|**name**     || Der Name des Bindungsparameters, der das Dokument in der Funktion darstellt  |
|**databaseName** |**DatabaseName** |Die Datenbank mit dem Dokument        |
|**collectionName** |**CollectionName** | Der Name der Sammlung mit dem Dokument |
|**id**    | **Id** | Die ID des abzurufenden Dokuments. Diese Eigenschaft unterstützt [Bindungsausdrücke](./functions-bindings-expressions-patterns.md). Legen Sie nicht die beiden Eigenschaften **id** und **sqlQuery** fest. Wenn Sie keine der beiden festlegen, wird die gesamte Sammlung abgerufen. |
|**sqlQuery**  |**SqlQuery**  | Eine SQL-Abfrage in Azure Cosmos DB zum Abrufen mehrerer Dokumente. Die Eigenschaft unterstützt Laufzeitbindungen, wie in diesem Beispiel: `SELECT * FROM c where c.departmentId = {departmentId}`. Legen Sie nicht die beiden Eigenschaften **id** und **sqlQuery** fest. Wenn Sie keine der beiden festlegen, wird die gesamte Sammlung abgerufen.|
|**Verbindung**     |**ConnectionStringSetting**|Der Name der App-Einstellung mit Ihrer Azure Cosmos DB-Verbindungszeichenfolge.        |
|**partitionKey**|**PartitionKey**|Gibt den Wert des Partitionsschlüssels für die Suche an. Kann den Bindungsparameter enthalten.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Eingabe: Verwendung

Wenn in C#- und F#-Funktionen die Funktion erfolgreich beendet wird, werden alle Änderungen am Eingabedokument mithilfe benannter Eingabeparameter automatisch beibehalten.

In JavaScript-Funktionen erfolgen Aktualisierungen bei Beenden der Funktion nicht automatisch. Verwenden Sie stattdessen `context.bindings.<documentName>In` und `context.bindings.<documentName>Out`, um Aktualisierungen vorzunehmen. Siehe das [JavaScript-Beispiel](#input---javascript-examples).

## <a name="output"></a>Output

Die Azure Cosmos DB-Ausgabebindung ermöglicht das Schreiben eines neuen Dokuments in eine Azure Cosmos DB-Datenbank mithilfe der SQL-API.

## <a name="output---examples"></a>Ausgabe – Beispiele

Sehen Sie sich die sprachspezifischen Beispiele an:

* [C#](#output---c-examples)
* [C#-Skript (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Sehen Sie sich auch das [Eingabebeispiel](#input---c-examples) mit `DocumentClient` an.

[Ausgabebeispiele überspringen](#output---attributes)

### <a name="output---c-examples"></a>Ausgabe: C#-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* Warteschlangentrigger: Schreiben eines einzelnen Dokuments
* Warteschlangentrigger: Schreiben von Dokumenten unter Verwendung von „IAsyncCollector“

Die Beispiele beziehen sich auf einen einfachen `ToDoItem`-Typ:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Ausgabebeispiele überspringen](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Warteschlangentrigger: Schreiben eines einzelnen Dokuments (C#)

Das folgende Beispiel zeigt eine [ C#-Funktion](functions-dotnet-class-library.md), die einer Datenbank ein Dokument hinzufügt und dazu die Daten aus der Meldung vom Warteschlangenspeicher verwendet.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV1
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Ausgabebeispiele überspringen](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Warteschlangentrigger: Schreiben von Dokumenten unter Verwendung von „IAsyncCollector“ (C#)

Das folgende Beispiel zeigt eine [ C#-Funktion](functions-dotnet-class-library.md), die einer Datenbank eine Sammlung von Dokumenten hinzufügt und dabei Daten aus dem JSON-Code einer Warteschlangennachricht verwendet.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Ausgabebeispiele überspringen](#output---attributes)

### <a name="output---c-script-examples"></a>Ausgabe: C#-Skriptbeispiele

Dieser Abschnitt enthält folgende Beispiele:

* Warteschlangentrigger: Schreiben eines einzelnen Dokuments
* Warteschlangentrigger: Schreiben von Dokumenten unter Verwendung von „IAsyncCollector“

[Ausgabebeispiele überspringen](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Warteschlangentrigger: Schreiben eines einzelnen Dokuments (C#-Skript)

Das folgende Beispiel zeigt eine Azure Cosmos DB-Ausgabebindung in einer *function.json*-Datei sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet. Die Funktion verwendet eine Warteschlangeneingabebindung für eine Warteschlange, die JSON-Code im folgenden Format empfängt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Die Funktion erstellt Azure Cosmos DB-Dokumente im folgenden Format für die einzelnen Datensätze:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        dynamic employee = JObject.Parse(myQueueItem);

        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }
```

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Warteschlangentrigger: Schreiben von Dokumenten unter Verwendung von „IAsyncCollector“

Für das Erstellen mehrerer Dokumente können Sie eine Bindung mit `ICollector<T>` oder `IAsyncCollector<T>` erstellen, wobei `T` einer der unterstützten Typen ist.

Dieses Beispiel bezieht sich auf einen einfachen `ToDoItem`-Typ:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Die Datei „function.json“ sieht wie folgt aus:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "documentDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.Info($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Ausgabebeispiele überspringen](#output---attributes)

### <a name="output---javascript-examples"></a>Ausgabe: JavaScript-Beispiele

Das folgende Beispiel zeigt eine Azure Cosmos DB-Ausgabebindung in einer *function.json*-Datei sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet. Die Funktion verwendet eine Warteschlangeneingabebindung für eine Warteschlange, die JSON-Code im folgenden Format empfängt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Die Funktion erstellt Azure Cosmos DB-Dokumente im folgenden Format für die einzelnen Datensätze:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der JavaScript-Code sieht wie folgt aus:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Ausgabebeispiele überspringen](#output---attributes)

### <a name="output---f-examples"></a>Ausgabe: F#-Beispiele

Das folgende Beispiel zeigt eine Azure Cosmos DB-Ausgabebindung in einer *function.json*-Datei sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion verwendet eine Warteschlangeneingabebindung für eine Warteschlange, die JSON-Code im folgenden Format empfängt:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Die Funktion erstellt Azure Cosmos DB-Dokumente im folgenden Format für die einzelnen Datensätze:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der F#-Code lautet wie folgt:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
      log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

Dieses Beispiel erfordert die Datei `project.json`, die die NuGet-Abhängigkeiten `FSharp.Interop.Dynamic` und `Dynamitey` angibt:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Informationen zum Hinzufügen einer `project.json`-Datei finden Sie unter [Paketverwaltung](functions-reference-fsharp.md#package).

## <a name="output---attributes"></a>Ausgabe: Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs)-Attribut.

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie unter [Ausgabe: Konfiguration](#output---configuration). Hier ist ein Beispiel für ein `DocumentDB`-Attribut in einer Methodensignatur:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Ein vollständiges Beispiel finden Sie unter [Ausgabe: C#-Beispiel](#output---c-examples).

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `DocumentDB` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**     || Muss auf `documentdb` festgelegt sein.        |
|**direction**     || Muss auf `out` festgelegt sein.         |
|**name**     || Der Name des Bindungsparameters, der das Dokument in der Funktion darstellt  |
|**databaseName** | **DatabaseName**|Die Datenbank mit der Sammlung, in der das neue Dokument erstellt wird     |
|**collectionName** |**CollectionName**  | Der Name der Sammlung, in der das neue Dokument erstellt wird |
|**createIfNotExists**  |**CreateIfNotExists**    | Ein boolescher Wert, der angibt, ob die Sammlung erstellt werden soll, wenn sie nicht vorhanden ist. Der Standardwert ist *FALSE*, da neue Sammlungen mit reserviertem Durchsatz erstellt werden. Dies wirkt sich auf die Kosten aus. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**partitionKey**|**PartitionKey** |Wenn `CreateIfNotExists` den Wert „TRUE“ hat, wird der Partitionsschlüsselpfad für die erstellte Sammlung definiert.|
|**collectionThroughput**|**CollectionThroughput**| Wenn `CreateIfNotExists` den Wert „TRUE“ hat, wird der [Durchsatz](../cosmos-db/set-throughput.md) für die erstellte Sammlung definiert.|
|**Verbindung**    |**ConnectionStringSetting** |Der Name der App-Einstellung mit Ihrer Azure Cosmos DB-Verbindungszeichenfolge.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Ausgabe: Verwendung

Standardmäßig wird beim Schreiben in den Ausgabeparameter in Ihrer Funktion ein Dokument in der Datenbank erstellt. Dieses Dokument besitzt eine automatisch generierte GUID als Dokument-ID. Sie können die Dokument-ID des Ausgabedokuments angeben, indem Sie die `id`-Eigenschaft im JSON-Objekt angeben, das an den Ausgabeparameter übergeben wird.

> [!Note]
> Wenn Sie die ID eines vorhandenen Dokuments angeben, wird dieses vom neuen Ausgabedokument überschrieben.

## <a name="exceptions-and-return-codes"></a>Ausnahmen und Rückgabecodes

| Bindung | Verweis |
|---|---|
| CosmosDB | [CosmosDB-Fehlercodes](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich über serverlose Datenbankberechnungen mit Cosmos DB.](../cosmos-db/serverless-computing-database.md)
* [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
