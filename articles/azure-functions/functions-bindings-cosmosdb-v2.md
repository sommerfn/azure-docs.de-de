---
title: Azure Cosmos DB-Bindungen für Functions 2.x
description: Grundlegendes zur Verwendung von Azure Cosmos DB-Triggern und -Bindungen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: azure-functions
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: d8aee88f6ef3f6a73beadfdf242d79d9b361de0a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73469393"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x"></a>Azure Cosmos DB-Bindungen für Azure Functions 2.x

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version der Azure Functions-Runtime aus: "]
> * [Version 1](functions-bindings-cosmosdb.md)
> * [Version 2](functions-bindings-cosmosdb-v2.md)

In diesem Artikel wird die Verwendung von [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md)-Bindungen in Azure Functions 2.x erläutert. Azure Functions unterstützt Trigger sowie Ein- und Ausgabebindungen für Azure Cosmos DB.

> [!NOTE]
> Dieser Artikel gilt für [Azure Functions Version 2.x](functions-versions.md).  Informationen zur Verwendung dieser Bindungen in Functions 1.x finden Sie unter [Azure Cosmos DB-Bindungen für Azure Functions](functions-bindings-cosmosdb.md).
>
> Diese Bindung hatte ursprünglich die Bezeichnung „DocumentDB“. In der Functions-Version 2.x wurden der Trigger, die Bindungen und das Paket jeweils mit dem Namen „Cosmos DB“ versehen.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-apis"></a>Unterstützte APIs

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Die Azure Cosmos DB-Bindungen für die Functions-Version 2.x werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) (Version 3.x) bereitgestellt. Den Quellcode für die Bindungen finden Sie im GitHub-Repository [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Trigger

Informationen zum partitionsübergreifenden Lauschen auf Einfügungen und Aktualisierungen durch den Azure Cosmos DB-Trigger finden Sie unter [Verwenden der Unterstützung von Änderungsfeeds in Azure Cosmos DB](../cosmos-db/change-feed.md). Der Änderungsfeed veröffentlicht Einfügungen und Updates, keine Löschungen.

## <a name="trigger---example"></a>Trigger: Beispiel

Sehen Sie sich das sprachspezifische Beispiel an:

* [C#](#trigger---c-example)
* [C#-Skript (.csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

Trigger-Beispiele überspringen

### <a name="trigger---c-example"></a>Trigger: C#-Beispiel

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die aufgerufen wird, wenn etwas in der angegebenen Datenbank und Sammlung eingefügt oder aktualisiert wird.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
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
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

Trigger-Beispiele überspringen

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
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

Trigger-Beispiele überspringen

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

### <a name="trigger---java-example"></a>Trigger: Java-Beispiel

Das folgende Beispiel zeigt eine Cosmos DB-Triggerbindung in einer Datei *function.json* sowie eine [Java-Funktion](functions-reference-java.md), die die Bindung verwendet. Die Funktion wird aufgerufen, wenn etwas in der angegebenen Datenbank und Sammlung eingefügt oder aktualisiert wird.

```json
{
    "type": "cosmosDBTrigger",
    "name": "items",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "AzureCosmosDBConnection",
    "databaseName": "ToDoList",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": false
}
```

Dies ist der Java-Code:

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


Verwenden Sie die `@CosmosDBTrigger`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, deren Wert von Cosmos DB empfangen wird.  Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit Optional\<T> verwendet werden, die NULL-Werte annehmen können.


Trigger-Beispiele überspringen

### <a name="trigger---python-example"></a>Trigger: Beispiel für Python

Das folgende Beispiel zeigt eine Cosmos DB-Triggerbindung in einer Datei namens *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet. Die Funktion schreibt Protokollmeldungen, wenn Cosmos DB-Datensätze geändert werden.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Python-Code:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

## <a name="trigger---c-attributes"></a>Trigger: C#-Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs)-Attribut.

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie unter [Trigger: Konfiguration](#trigger---configuration). Hier ist ein Beispiel für ein `CosmosDBTrigger`-Attribut in einer Methodensignatur:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
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
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Optional) Wenn festgelegt, legt diese Eigenschaft die Höchstzahl von Elementen fest, die pro Funktionsaufruf empfangen werden können. Wenn Vorgänge in der überwachten Sammlung über gespeicherte Prozeduren ausgeführt werden, wird der [Transaktionsbereich](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) beim Lesen von Elementen aus dem Änderungsfeed beibehalten. Dadurch ist es möglich, dass die Anzahl der empfangenen Elemente höher als der angegebene Wert ist, sodass die von derselben Transaktion geänderten Elemente als Teil eines atomischen Batches zurückgegeben werden.
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
* [F#](#input---f-examples)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-examples)
* [Python](#input---python-examples)

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
namespace CosmosDBSamplesV2
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
namespace CosmosDBSamplesV2
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
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

>[!NOTE]
>Beim Wert für den HTTP-Abfragezeichenfolge-Parameter muss die Groß-/Kleinschreibung beachtet werden.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP-Trigger: Suchen der ID in Routendaten (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Routendaten verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP-Trigger: Suchen der ID in Routendaten unter Verwendung von „SqlQuery“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Routendaten verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

Das Beispiel veranschaulicht die Verwendung eines Bindungsausdrucks im Parameter `SqlQuery`. Sie können Routendaten wie gezeigt an den Parameter `SqlQuery` übergeben. [Die Übergabe von Abfragezeichenfolgenwerten ist derzeit allerdings nicht möglich.](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583)


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Die Abfrage wird in der Attributeigenschaft `SqlQuery` angegeben.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP-Trigger: Abrufen mehrerer Dokumente unter Verwendung von „DocumentClient“ (C#)

Das folgende Beispiel zeigt eine [C#-Funktion](functions-dotnet-class-library.md), die eine Liste von Dokumenten abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst. Der Code verwendet eine von der Azure Cosmos DB-Bindung bereitgestellte `DocumentClient`-Instanz, um eine Liste von Dokumenten zu lesen. Die `DocumentClient`-Instanz kann auch für Schreibvorgänge verwendet werden.

> [!NOTE]
> Sie können auch die [IDocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.idocumentclient?view=azure-dotnet)-Schnittstelle verwenden, um das Testen zu vereinfachen.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
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
namespace CosmosDBSamplesV2
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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
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
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
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
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
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
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
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
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
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
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
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
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
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
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
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
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Eingabebeispiele überspringen](#input---attributes)

### <a name="input---javascript-examples"></a>Eingabe: JavaScript-Beispiele

Dieser Abschnitt enthält die folgenden Beispiele, die ein einzelnes Dokument lesen, indem ein ID-Wert aus verschiedenen Quellen angegeben wird:

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
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
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
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
      "type": "cosmosDB",
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
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
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

### <a name="input---python-examples"></a>Eingabe: Beispiele für Python

Dieser Abschnitt enthält die folgenden Beispiele, die ein einzelnes Dokument lesen, indem ein ID-Wert aus verschiedenen Quellen angegeben wird:

* [Warteschlangentrigger: Suchen der ID in JSON-Code](#queue-trigger-look-up-id-from-json-python)
* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge](#http-trigger-look-up-id-from-query-string-python)
* [HTTP-Trigger: Suchen der ID in Routendaten](#http-trigger-look-up-id-from-route-data-python)
* [Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“](#queue-trigger-get-multiple-docs-using-sqlquery-python)

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-python"></a>Warteschlangentrigger: Suchen der ID in JSON-Code (Python)

Das folgende Beispiel zeigt eine Cosmos DB-Eingabebindung in einer Datei namens *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet. Die Funktion liest ein einzelnes Dokument und aktualisiert den Textwert des Dokuments.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Python-Code:

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-python"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge (Python)

Das folgende Beispiel zeigt eine [Python-Funktion](functions-reference-python.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

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
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

Python-Code:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-python"></a>HTTP-Trigger: Suchen der ID in Routendaten (Python)

Das folgende Beispiel zeigt eine [Python-Funktion](functions-reference-python.md), die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument vom Typ `ToDoItem` aus der angegebenen Datenbank und Sammlung abgerufen.

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
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Python-Code:

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

[Eingabebeispiele überspringen](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>Warteschlangentrigger: Abrufen mehrerer Dokumente unter Verwendung von „SqlQuery“ (Python)

Das folgende Beispiel zeigt eine Azure Cosmos DB-Eingabebindung in einer Datei namens *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet. Die Funktion ruft mehrere von einer SQL-Abfrage angegebene Dokumente mithilfe eines Warteschlangentriggers ab, um die Abfrageparameter anzupassen.

Der Warteschlangentrigger stellt den Parameter `departmentId` bereit. Die Warteschlangennachricht `{ "departmentId" : "Finance" }` gibt dann alle Datensätze für die Finanzabteilung zurück.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#input---configuration).

Python-Code:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```


[Eingabebeispiele überspringen](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Eingabe: F#-Beispiele

Das folgende Beispiel zeigt eine Cosmos DB-Eingabebindung in einer Datei *function.json* sowie eine [F#-Funktion](functions-reference-fsharp.md), die die Bindung verwendet. Die Funktion liest ein einzelnes Dokument und aktualisiert den Textwert des Dokuments.

Bindungsdaten in der Datei *function.json*:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
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

### <a name="input---java-examples"></a>Eingabe: Java-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge – Zeichenfolgenparameter](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge – POJO-Parameter](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP-Trigger: Suchen der ID in Routendaten](#http-trigger-look-up-id-from-route-data-java)
* [HTTP-Trigger: Suchen der ID in Routendaten unter Verwendung von „SqlQuery“](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP-Trigger: Abrufen mehrerer Dokumente aus Routendaten unter Verwendung von SqlQuery](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Die Beispiele beziehen sich auf einen einfachen `ToDoItem`-Typ:

```java
public class ToDoItem {

  private String id;
  private String description;

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }

  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

#### <a name="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge – Zeichenfolgenparameter (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument in Zeichenfolgenform aus der angegebenen Datenbank und Sammlung abgerufen.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

Verwenden Sie die `@CosmosDBInput`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Funktionsparameter, deren Wert von Cosmos DB empfangen wird.  Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit Optional\<T> verwendet werden, die NULL-Werte annehmen können.

#### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>HTTP-Trigger: Suchen der ID in einer Abfragezeichenfolge – POJO-Parameter (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die eine Abfragezeichenfolge verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument aus der angegebenen Datenbank und Sammlung abgerufen. Das Dokument wird dann in eine Instanz des zuvor erstellten ```ToDoItem``` POJO konvertiert und als Argument an die Funktion übergeben.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-java"></a>HTTP-Trigger: Suchen der ID in Routendaten (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die einen Routenparameter verwendet, um die zu suchende ID anzugeben. Anhand dieser ID wird ein Dokument aus der angegebenen Datenbank und Sammlung abgerufen und als ```Optional<String>``` zurückgegeben.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>HTTP-Trigger: Suchen der ID in Routendaten unter Verwendung von „SqlQuery“ (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die ein einzelnes Dokument abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die einen Routenparameter verwendet, um die zu suchende ID anzugeben. Diese ID wird verwendet, um ein Dokument aus der angegebenen Datenbank und Sammlung abzurufen, wobei das Resultset in ein ```ToDoItem[]``` konvertiert wird, da viele Dokumente zurückgegeben werden können, je nach den Abfragekriterien.

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>HTTP-Trigger: Abrufen mehrerer Dokumente aus Routendaten unter Verwendung von SqlQuery (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die mehrere Dokumente abruft. Die Funktion wird durch eine HTTP-Anforderung ausgelöst, die den Routenparameter ```desc``` verwendet, um die in dem Feld ```description``` zu suchende Zeichenfolge anzugeben. Der Suchbegriff wird verwendet, um ein Sammlung von Dokumenten aus der angegebenen Datenbank und Sammlung abzurufen, wobei das Resultset in ein ```ToDoItem[]``` konvertiert und dieses als Argument an die Funktion übergeben wird.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] items,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

## <a name="input---attributes"></a>Eingabe: Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das Attribut [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie im [folgenden Konfigurationsabschnitt](#input---configuration).

## <a name="input---configuration"></a>Eingabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `CosmosDB` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**     || Muss auf `cosmosDB` festgelegt sein.        |
|**direction**     || Muss auf `in` festgelegt sein.         |
|**name**     || Der Name des Bindungsparameters, der das Dokument in der Funktion darstellt  |
|**databaseName** |**DatabaseName** |Die Datenbank mit dem Dokument        |
|**collectionName** |**CollectionName** | Der Name der Sammlung mit dem Dokument |
|**id**    | **Id** | Die ID des abzurufenden Dokuments. Diese Eigenschaft unterstützt [Bindungsausdrücke](./functions-bindings-expressions-patterns.md). Legen Sie nicht die beiden Eigenschaften **id** und **sqlQuery** fest. Wenn Sie keine der beiden festlegen, wird die gesamte Sammlung abgerufen. |
|**sqlQuery**  |**SqlQuery**  | Eine SQL-Abfrage in Azure Cosmos DB zum Abrufen mehrerer Dokumente. Die Eigenschaft unterstützt Laufzeitbindungen, wie in diesem Beispiel: `SELECT * FROM c where c.departmentId = {departmentId}`. Legen Sie nicht die beiden Eigenschaften **id** und **sqlQuery** fest. Wenn Sie keine der beiden festlegen, wird die gesamte Sammlung abgerufen.|
|**connectionStringSetting**     |**ConnectionStringSetting**|Der Name der App-Einstellung mit Ihrer Azure Cosmos DB-Verbindungszeichenfolge.        |
|**partitionKey**|**PartitionKey**|Gibt den Wert des Partitionsschlüssels für die Suche an. Kann den Bindungsparameter enthalten.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Eingabe: Verwendung

Wenn in C#- und F#-Funktionen die Funktion erfolgreich beendet wird, werden alle Änderungen am Eingabedokument mithilfe benannter Eingabeparameter automatisch beibehalten.

In JavaScript-Funktionen erfolgen Aktualisierungen bei Beenden der Funktion nicht automatisch. Verwenden Sie stattdessen `context.bindings.<documentName>In` und `context.bindings.<documentName>Out`, um Aktualisierungen vorzunehmen. Siehe das JavaScript-Beispiel.

## <a name="output"></a>Output

Die Azure Cosmos DB-Ausgabebindung ermöglicht das Schreiben eines neuen Dokuments in eine Azure Cosmos DB-Datenbank mithilfe der SQL-API.

## <a name="output---examples"></a>Ausgabe – Beispiele

Sehen Sie sich die sprachspezifischen Beispiele an:

* [C#](#output---c-examples)
* [C#-Skript (.csx)](#output---c-script-examples)
* [F#](#output---f-examples)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-examples)
* [Python](#output---python-examples)

Sehen Sie sich auch das [Eingabebeispiel](#input---c-examples) mit `DocumentClient` an.

[Ausgabebeispiele überspringen](#output---attributes)

### <a name="output---c-examples"></a>Ausgabe: C#-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* Warteschlangentrigger: Schreiben eines einzelnen Dokuments
* Warteschlangentrigger: Schreiben von Dokumenten unter Verwendung von „IAsyncCollector“

Die Beispiele beziehen sich auf einen einfachen `ToDoItem`-Typ:

```cs
namespace CosmosDBSamplesV2
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
using Microsoft.Extensions.Logging;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            ILogger log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.LogInformation($"C# Queue trigger function inserted one row");
            log.LogInformation($"Description={queueMessage}");
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
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.LogInformation($"Description={toDoItem.Description}");
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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der C#-Skriptcode sieht wie folgt aus:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;
    using Microsoft.Extensions.Logging;

    public static void Run(string myQueueItem, out object employeeDocument, ILogger log)
    {
      log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

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
namespace CosmosDBSamplesV2
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
      "connectionStringSetting": "AzureWebJobsStorage"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Der C#-Skriptcode sieht wie folgt aus:

```cs
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.LogInformation($"Description={toDoItem.Description}");
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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
Weitere Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Konfiguration](#output---configuration).

Der F#-Code lautet wie folgt:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json
    open Microsoft.Extensions.Logging

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: ILogger) =
      log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
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

### <a name="output---java-examples"></a>Ausgabe: Java-Beispiele

* [Warteschlangentrigger: Speichern einer Nachricht in einer Datenbank über den Rückgabewert](#queue-trigger-save-message-to-database-via-return-value-java)
* [HTTP-Trigger: Speichern eines Dokuments in einer Datenbank über den Rückgabewert](#http-trigger-save-one-document-to-database-via-return-value-java)
* [HTTP-Trigger: Speichern eines Dokuments in einer Datenbank über OutputBinding](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [HTTP-Trigger: Speichern mehrerer Dokumente in einer Datenbank über OutputBinding](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


#### <a name="queue-trigger-save-message-to-database-via-return-value-java"></a>Warteschlangentrigger: Speichern einer Nachricht in einer Datenbank über den Rückgabewert (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die einer Datenbank ein Dokument hinzufügt und dazu die Daten aus der Meldung in Queue Storage verwendet.

```java
@FunctionName("getItem")
@CosmosDBOutput(name = "database",
  databaseName = "ToDoList",
  collectionName = "Items",
  connectionStringSetting = "AzureCosmosDBConnection")
public String cosmosDbQueryById(
    @QueueTrigger(name = "msg",
      queueName = "myqueue-items",
      connection = "AzureWebJobsStorage")
    String message,
    final ExecutionContext context)  {
     return "{ id: \"" + System.currentTimeMillis() + "\", Description: " + message + " }";
   }
```

#### <a name="http-trigger-save-one-document-to-database-via-return-value-java"></a>HTTP-Trigger: Speichern eines Dokuments in einer Datenbank über den Rückgabewert (Java)

Das folgende Beispiel zeigt eine Java-Funktion, deren Signatur mit ```@CosmosDBOutput``` kommentiert ist und einen Rückgabewert des Typs ```String``` besitzt. Das von der Funktion zurückgegebene JSON-Dokument wird automatisch in die entsprechende COSMOS CosmosDB-Sammlung geschrieben.

```java
    @FunctionName("WriteOneDoc")
    @CosmosDBOutput(name = "database",
      databaseName = "ToDoList",
      collectionName = "Items",
      connectionStringSetting = "Cosmos_DB_Connection_String")
    public String run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " +
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        return jsonDocument;
    }
```

#### <a name="http-trigger-save-one-document-to-database-via-outputbinding-java"></a>HTTP-Trigger: Speichern eines Dokuments in einer Datenbank über OutputBinding (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die ein Dokument über einen ```OutputBinding<T>```-Ausgabeparameter in CosmosDB schreibt. Beachten Sie, dass es in diesem Setup der Parameter ```outputItem``` ist, der mit ```@CosmosDBOutput``` kommentiert sein muss, nicht die Funktionssignatur. Durch die Verwendung von ```OutputBinding<T>``` kann Ihre Funktion die Bindung nutzen, um das Dokument in CosmosDB zu schreiben, und gleichzeitig einen anderen Wert an den Funktionsaufrufer zurückgeben, z. B. ein JSON- oder XML-Dokument.

```java
    @FunctionName("WriteOneDocOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            OutputBinding<String> outputItem,
            final ExecutionContext context) {

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " +
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        // Set outputItem's value to the JSON document to be saved
        outputItem.setValue(jsonDocument);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Document created successfully.")
                      .build();
    }
```

#### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding-java"></a>HTTP-Trigger: Speichern mehrerer Dokumente in einer Datenbank über OutputBinding (Java)

Das folgende Beispiel zeigt eine Java-Funktion, die mehrere Dokumente über einen ```OutputBinding<T>```-Ausgabeparameter in CosmosDB schreibt. Beachten Sie, dass es in diesem Setup der Parameter ```outputItem``` ist, der mit ```@CosmosDBOutput``` kommentiert sein muss, nicht die Funktionssignatur. Der Ausgabeparameter ```outputItem``` enthält eine Liste von ```ToDoItem```-Objekten als deren Vorlagenparametertyp. Durch die Verwendung von ```OutputBinding<T>``` kann Ihre Funktion die Bindung nutzen, um die Dokumente in CosmosDB zu schreiben, und gleichzeitig einen anderen Wert an den Funktionsaufrufer zurückgeben, z. B. ein JSON- oder XML-Dokument.

```java
    @FunctionName("WriteMultipleDocsOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            OutputBinding<List<ToDoItem>> outputItem,
            final ExecutionContext context) {

        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Generate documents
        List<ToDoItem> items = new ArrayList<>();

        for (int i = 0; i < 5; i ++) {
          // Generate random ID
          final int id = Math.abs(new Random().nextInt());

          // Create ToDoItem
          ToDoItem item = new ToDoItem(String.valueOf(id), name);

          items.add(item);
        }

        // Set outputItem's value to the list of POJOs to be saved
        outputItem.setValue(items);
        context.getLogger().info("Document to be saved: " + items);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Documents created successfully.")
                      .build();
    }
```

Verwenden Sie die `@CosmosDBOutput`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, die in Cosmos DB geschrieben werden.  Der Parametertyp der Anmerkung muss ```OutputBinding<T>``` sein, wobei „T“ ein nativer Java-Typ oder ein POJO ist.

### <a name="output---python-examples"></a>Ausgabe: Beispiele für Python

Im folgenden Beispiel wird veranschaulicht, wie ein Dokument als Ausgabe einer Funktion in eine Azure CosmosDB-Datenbank geschrieben wird.

Die Bindungsdefinition ist in *function.json* definiert, wobei *type* auf `cosmosDB` festgelegt ist.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "cosmosDB",
      "direction": "out",
      "name": "doc",
      "databaseName": "demodb",
      "collectionName": "data",
      "createIfNotExists": "true",
      "connectionStringSetting": "AzureCosmosDBConnectionString"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

Um in die Datenbank zu schreiben, übergeben Sie ein Dokumentobjekt an die `set`-Methode des Parameters „database“.

```python
import azure.functions as func

def main(req: func.HttpRequest, doc: func.Out[func.Document]) -> func.HttpResponse:

    request_body = req.get_body()

    doc.set(func.Document.from_json(request_body))

    return 'OK'
```

## <a name="output---attributes"></a>Ausgabe: Attribute

Verwenden Sie in [C#-Klassenbibliotheken](functions-dotnet-class-library.md) das Attribut [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

Der Attributkonstruktor akzeptiert den Datenbanknamen und den Sammlungsnamen. Weitere Informationen zu diesen Einstellungen und anderen Eigenschaften, die Sie konfigurieren können, finden Sie unter [Ausgabe: Konfiguration](#output---configuration). Hier ist ein Beispiel für ein `CosmosDB`-Attribut in einer Methodensignatur:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Ein vollständiges Beispiel finden Sie unter „Ausgabe: C#-Beispiel“.

## <a name="output---configuration"></a>Ausgabe: Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* und im Attribut `CosmosDB` festlegen:

|Eigenschaft von „function.json“ | Attributeigenschaft |BESCHREIBUNG|
|---------|---------|----------------------|
|**type**     || Muss auf `cosmosDB` festgelegt sein.        |
|**direction**     || Muss auf `out` festgelegt sein.         |
|**name**     || Der Name des Bindungsparameters, der das Dokument in der Funktion darstellt  |
|**databaseName** | **DatabaseName**|Die Datenbank mit der Sammlung, in der das neue Dokument erstellt wird     |
|**collectionName** |**CollectionName**  | Der Name der Sammlung, in der das neue Dokument erstellt wird |
|**createIfNotExists**  |**CreateIfNotExists**    | Ein boolescher Wert, der angibt, ob die Sammlung erstellt werden soll, wenn sie nicht vorhanden ist. Der Standardwert ist *FALSE*, da neue Sammlungen mit reserviertem Durchsatz erstellt werden. Dies wirkt sich auf die Kosten aus. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**partitionKey**|**PartitionKey** |Wenn `CreateIfNotExists` den Wert „TRUE“ hat, wird der Partitionsschlüsselpfad für die erstellte Sammlung definiert.|
|**collectionThroughput**|**CollectionThroughput**| Wenn `CreateIfNotExists` den Wert „TRUE“ hat, wird der [Durchsatz](../cosmos-db/set-throughput.md) für die erstellte Sammlung definiert.|
|**connectionStringSetting**    |**ConnectionStringSetting** |Der Name der App-Einstellung mit Ihrer Azure Cosmos DB-Verbindungszeichenfolge.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Ausgabe: Verwendung

Standardmäßig wird beim Schreiben in den Ausgabeparameter in Ihrer Funktion ein Dokument in der Datenbank erstellt. Dieses Dokument besitzt eine automatisch generierte GUID als Dokument-ID. Sie können die Dokument-ID des Ausgabedokuments angeben, indem Sie die `id`-Eigenschaft im JSON-Objekt angeben, das an den Ausgabeparameter übergeben wird.

> [!Note]
> Wenn Sie die ID eines vorhandenen Dokuments angeben, wird dieses vom neuen Ausgabedokument überschrieben.

## <a name="exceptions-and-return-codes"></a>Ausnahmen und Rückgabecodes

| Bindung | Verweis |
|---|---|
| CosmosDB | [CosmosDB-Fehlercodes](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

In diesem Abschnitt werden die verfügbaren globalen Konfigurationseinstellungen für diese Bindung in Version 2.x beschrieben. Weitere Informationen zu globalen Konfigurationseinstellungen in Version 2.x finden Sie unter [host.json-Referenz für Azure Functions 2.x](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "cosmosDB": {
            "connectionMode": "Gateway",
            "protocol": "Https",
            "leaseOptions": {
                "leasePrefix": "prefix1"
            }
        }
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------|
|GatewayMode|Gateway|Der von der Funktion zum Herstellen von Verbindungen mit dem Azure Cosmos DB-Dienst verwendete Verbindungsmodus. Die Optionen sind `Direct` und `Gateway`.|
|Protocol|HTTPS|Das von der Funktion zum Herstellen von Verbindungen mit dem Azure Cosmos DB-Dienst verwendete Verbindungsprotokoll.  Hier finden Sie [eine Erläuterung der beiden Modi](../cosmos-db/performance-tips.md#networking)|
|leasePrefix|–|Lease-Präfix, das durchgängig für alle Funktionen in einer App verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich über serverlose Datenbankberechnungen mit Cosmos DB.](../cosmos-db/serverless-computing-database.md)
* [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
