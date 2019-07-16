---
title: Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter
description: In diesem Tutorial wird beschrieben, wie Sie einen RESTful-Endpunkt für benutzerdefinierte Anbieter erstellen. Es wird ausführlich erläutert, wie Sie Anforderungen und Antworten für die unterstützten RESTful-HTTP-Methoden verarbeiten.
author: jjbfour
ms.service: managed-applications
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 176e3b02cbda7577e306d86363cfe5b41335fb6e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799179"
---
# <a name="authoring-a-restful-endpoint-for-custom-providers"></a>Erstellen eines RESTful-Endpunkts für benutzerdefinierte Anbieter

Benutzerdefinierte Anbieter ermöglichen Ihnen die Anpassung von Workflows in Azure. Ein benutzerdefinierter Anbieter ist ein Vertrag zwischen Azure und einem Endpunkt (`endpoint`). In diesem Tutorial wird der Prozess zum Erstellen eines RESTful-Endpunkts (`endpoint`) für einen benutzerdefinierten Anbieter durchlaufen. Falls Sie noch nicht mit benutzerdefinierten Azure-Anbietern vertraut sind, hilft Ihnen die [Übersicht über benutzerdefinierte Ressourcenanbieter](./custom-providers-overview.md) weiter.

Dieses Tutorial ist in die folgenden Schritte unterteilt:

- Verwenden von benutzerdefinierten Aktionen und Ressourcen
- Partitionieren von benutzerdefinierten Ressourcen im Speicher
- Ermöglichen der Unterstützung von RESTful-Methoden für benutzerdefinierte Anbieter
- Integrieren von RESTful-Vorgängen

Dieses Tutorial baut auf den folgenden Tutorials auf:

- [Einrichten von Azure-Funktionen für benutzerdefinierte Azure-Anbieter](./tutorial-custom-providers-function-setup.md)

> [!NOTE]
> Dieses Tutorial baut auf dem vorherigen Tutorial auf. Einige Schritte im Tutorial funktionieren nur, wenn eine Azure-Funktion für die Nutzung von benutzerdefinierten Anbietern eingerichtet wurde.

## <a name="working-with-custom-actions-and-custom-resources"></a>Verwenden von benutzerdefinierten Aktionen und Ressourcen

In diesem Tutorial aktualisieren wir die Funktion so, dass sie als RESTful-Endpunkt für unseren benutzerdefinierten Anbieter fungiert. In Azure werden Ressourcen und Aktionen basierend auf der grundlegenden RESTful-Spezifikation modelliert: PUT erstellt eine neue Ressource, GET (Instanz) ruft eine vorhandene Ressource ab, DELETE entfernt eine vorhandene Ressource, POST löst eine Aktion aus und GET (Sammlung) listet alle vorhandenen Ressourcen auf. In diesem Tutorial verwenden wir Azure-Tabellen als unseren Speicher, aber ggf. ist jede Datenbank bzw. jeder Speicherdienst geeignet.

## <a name="how-to-partition-custom-resources-in-storage"></a>Partitionieren von benutzerdefinierten Ressourcen im Speicher

Da wir einen RESTful-Dienst erstellen, müssen wir die erstellten Ressourcen im Speicher ablegen. Für Azure-Tabellenspeicher müssen wir Partitions- und Zeilenschlüssel für unsere Daten generieren. Für benutzerdefinierte Anbieter sollten Daten entsprechend partitioniert werden. Wenn eine eingehende Anforderung an den benutzerdefinierten Anbieter gesendet wird, fügt dieser der ausgehenden Anforderung, die an `endpoint` gesendet wird, den Header `x-ms-customproviders-requestpath` hinzu.

Beispielheader `x-ms-customproviders-requestpath` für eine benutzerdefinierte Ressource:

```
X-MS-CustomProviders-RequestPath: /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/{myResourceType}/{myResourceName}
```

Basierend auf dem obigen `x-ms-customproviders-requestpath`-Beispielheader können wir die Elemente „partitionKey“ und „rowKey“ für unseren Speicher wie folgt erstellen:

Parameter | Vorlage | BESCHREIBUNG
---|---
partitionKey | '{subscriptionId}:{resourceGroupName}:{resourceProviderName}' | Mit dem „partitionKey“ wird angegeben, wie die Daten partitioniert sind. In den meisten Fällen sollten die Daten über die Instanz des benutzerdefinierten Anbieters partitioniert werden.
rowKey | '{myResourceType}:{myResourceName}' | „rowKey“ ist der individuelle Bezeichner für die Daten. In den meisten Fällen ist dies der Name der Ressource.

Darüber hinaus müssen wir auch eine neue Klasse erstellen, um unsere benutzerdefinierte Ressource zu modellieren. In diesem Tutorial fügen wir die `CustomResource`-Klasse unserer Funktion hinzu. Hierbei handelt es sich um eine generische Klasse, die eingegebene Daten akzeptiert:

```csharp
// Custom Resource Table Entity
public class CustomResource : TableEntity
{
    public string Data { get; set; }
}
```

Hiermit wird basierend auf `TableEntity` eine einfache Klasse erstellt, die zum Speichern von Daten verwendet wird. Die `CustomResource`-Klasse erbt zwei Eigenschaften von `TableEntity`: „partitionKey“ und „rowKey“.

## <a name="support-custom-provider-restful-methods"></a>Ermöglichen der Unterstützung von RESTful-Methoden für benutzerdefinierte Anbieter

> [!NOTE]
> Wenn Sie den Code nicht direkt aus dem Tutorial kopieren, sollte es sich beim Antwortinhalt um gültigen JSON-Code handeln, und der `Content-Type`-Header sollte als `application/json` festgelegt werden.

Nachdem wir die Datenpartitionierung nun eingerichtet haben, erstellen wir das Gerüst für die einfachen CRUD- und Triggermethoden für benutzerdefinierte Ressourcen und Aktionen. Da benutzerdefinierte Anbieter als Proxy fungieren, müssen die Anforderung und die Antwort über den RESTful-Endpunkt (`endpoint`) modelliert und verarbeitet werden. Die unten angegebenen Codeausschnitte enthalten die Informationen zur Verarbeitung der grundlegenden RESTful-Vorgänge:

### <a name="trigger-custom-action"></a>Auslösen einer benutzerdefinierten Aktion

Für benutzerdefinierte Anbieter wird eine benutzerdefinierte Aktion über `POST`-Anforderungen ausgelöst. Eine benutzerdefinierte Aktion kann optional einen Anforderungstext akzeptieren, der eine Reihe von Eingabeparametern enthält. Die Aktion sollte dann eine Antwort zurückgeben, mit der das Ergebnis der Aktion und der Erfolg bzw. Misserfolg angegeben wird. In diesem Tutorial fügen wir unserer Funktion die `TriggerCustomAction`-Methode hinzu:

```csharp
/// <summary>
/// Triggers a custom action with some side effect.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <returns>The http response result of the custom action.</returns>
public static async Task<HttpResponseMessage> TriggerCustomAction(HttpRequestMessage requestMessage)
{
    var myCustomActionRequest = await requestMessage.Content.ReadAsStringAsync();

    var actionResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    actionResponse.Content = myCustomActionRequest != string.Empty ? 
        new StringContent(JObject.Parse(myCustomActionRequest).ToString(), System.Text.Encoding.UTF8, "application/json") :
        null;
    return actionResponse;
}
```

Die `TriggerCustomAction`-Methode akzeptiert eine eingehende Anforderung und gibt einfach eine identische Antwort mit einem Erfolgsstatuscode zurück. 

### <a name="create-custom-resource"></a>Erstellen einer benutzerdefinierten Ressource

Für benutzerdefinierte Anbieter wird eine benutzerdefinierte Ressource mit `PUT`-Anforderungen erstellt. Der benutzerdefinierte Anbieter akzeptiert einen JSON-Anforderungstext, der eine Reihe von Eigenschaften für die benutzerdefinierte Ressource enthält. In Azure basieren Ressourcen auf einem RESTful-Modell. Mit derselben Anforderungs-URL, die zum Erstellen einer Ressource verwendet wird, sollte die Ressource auch abgerufen und gelöscht werden können. In diesem Tutorial fügen wir die `CreateCustomResource`-Methode hinzu, um neue Ressourcen zu erstellen:

```csharp
/// <summary>
/// Creates a custom resource and saves it to table storage.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="azureResourceId">The parsed Azure resource Id.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the created custom resource.</returns>
public static async Task<HttpResponseMessage> CreateCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, ResourceId azureResourceId, string partitionKey, string rowKey)
{
    // Adds the Azure top-level properties.
    var myCustomResource = JObject.Parse(await requestMessage.Content.ReadAsStringAsync());
    myCustomResource["name"] = azureResourceId.Name;
    myCustomResource["type"] = azureResourceId.FullResourceType;
    myCustomResource["id"] = azureResourceId.Id;

    // Save the resource into storage.
    var insertOperation = TableOperation.InsertOrReplace(
        new CustomResource
        {
            PartitionKey = partitionKey,
            RowKey = rowKey,
            Data = myCustomResource.ToString(),
        });
    await tableStorage.ExecuteAsync(insertOperation);

    var createResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    createResponse.Content = new StringContent(myCustomResource.ToString(), System.Text.Encoding.UTF8, "application/json");
    return createResponse;
}
```

Die `CreateCustomResource`-Methode aktualisiert die eingehende Anforderung so, dass sie die Azure-spezifischen Felder enthält: `id`, `name` und `type`. Dies sind Eigenschaften der obersten Ebene, die von Diensten in der ganzen Azure-Umgebung verwendet werden. Hiermit kann der benutzerdefinierte Anbieter in andere Dienste integriert werden, z. B. Azure Policy, Azure Resource Manager-Vorlagen und Azure-Aktivitätsprotokolle.

Eigenschaft | Beispiel | BESCHREIBUNG
---|---|---
name | '{myCustomResourceName}' | Der Name der benutzerdefinierten Ressource.
type | 'Microsoft.CustomProviders/resourceProviders/{resourceTypeName}' | Der Namespace des Ressourcentyps.
id | '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/<br>providers/Microsoft.CustomProviders/resourceProviders/{resourceProviderName}/<br>{resourceTypeName}/{myCustomResourceName}' | Die Ressourcen-ID.

Zusätzlich zum Hinzufügen der Eigenschaften speichern wir auch das Dokument in Azure Table Storage. 

### <a name="retrieve-custom-resource"></a>Abrufen der benutzerdefinierten Ressource

Für benutzerdefinierte Anbieter wird eine benutzerdefinierte Ressource mit `GET`-Anforderungen abgerufen. Der benutzerdefinierte Anbieter akzeptiert *keinen* JSON-Anforderungstext. Im Fall von `GET`-Anforderungen sollte der **Endpunkt** den `x-ms-customproviders-requestpath`-Header verwenden, um die bereits erstellte Ressource zurückzugeben. In diesem Tutorial fügen wir die `RetrieveCustomResource`-Methode hinzu, um vorhandene Ressourcen abzurufen:

```csharp
/// <summary>
/// Retrieves a custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the existing custom resource.</returns>
public static async Task<HttpResponseMessage> RetrieveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    var retrieveResponse = requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NotFound);

    retrieveResponse.Content = existingCustomResource != null ?
            new StringContent(existingCustomResource.Data, System.Text.Encoding.UTF8, "application/json"):
            null;
    return retrieveResponse;
}
```

In Azure sollten Ressourcen auf einem RESTful-Modell basieren. Über die Anforderungs-URL, mit der die Ressource erstellt wurde, sollte diese auch zurückgegeben werden, wenn eine `GET`-Anforderung durchgeführt wird.

### <a name="remove-custom-resource"></a>Entfernen der benutzerdefinierten Ressource

Für benutzerdefinierte Anbieter wird eine benutzerdefinierte Ressource mit `DELETE`-Anforderungen entfernt. Der benutzerdefinierte Anbieter akzeptiert *keinen* JSON-Anforderungstext. Im Fall von `DELETE`-Anforderungen sollte der **Endpunkt** den `x-ms-customproviders-requestpath`-Header verwenden, um die bereits erstellte Ressource zu löschen. In diesem Tutorial fügen wir die `RemoveCustomResource`-Methode hinzu, um vorhandene Ressourcen zu löschen:

```csharp
/// <summary>
/// Removes an existing custom resource.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="rowKey">The row key for storage. This is '{resourceType}:{customResourceName}'.</param>
/// <returns>The http response containing the result of the delete.</returns>
public static async Task<HttpResponseMessage> RemoveCustomResource(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string rowKey)
{
    // Attempt to retrieve the Existing Stored Value
    var tableQuery = TableOperation.Retrieve<CustomResource>(partitionKey, rowKey);
    var existingCustomResource = (CustomResource)(await tableStorage.ExecuteAsync(tableQuery)).Result;

    if (existingCustomResource != null) {
        var deleteOperation = TableOperation.Delete(existingCustomResource);
        await tableStorage.ExecuteAsync(deleteOperation);
    }

    return requestMessage.CreateResponse(
        existingCustomResource != null ? HttpStatusCode.OK : HttpStatusCode.NoContent);
}
```

In Azure sollten Ressourcen auf einem RESTful-Modell basieren. Über die Anforderungs-URL, mit der die Ressource erstellt wurde, sollte diese auch gelöscht werden, wenn eine `DELETE`-Anforderung durchgeführt wird.

### <a name="list-all-custom-resources"></a>Auflisten aller benutzerdefinierten Ressourcen

Für benutzerdefinierte Anbieter kann eine Liste mit vorhandenen benutzerdefinierten Ressourcen enumeriert werden, indem `GET`-Sammlungsanforderungen verwendet werden. Der benutzerdefinierte Anbieter akzeptiert *keinen* JSON-Anforderungstext. Bei `GET`-Sammlungsanforderungen sollte für `endpoint` der `x-ms-customproviders-requestpath`-Header genutzt werden, um die bereits erstellten Ressourcen zu enumerieren. In diesem Tutorial fügen wir die `EnumerateAllCustomResources`-Methode hinzu, um vorhandene Ressourcen zu enumerieren.

```csharp
/// <summary>
/// Enumerates all the stored custom resources for a given type.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <param name="partitionKey">The partition key for storage. This is the custom provider id.</param>
/// <param name="resourceType">The resource type of the enumeration.</param>
/// <returns>The http response containing a list of resources stored under 'value'.</returns>
public static async Task<HttpResponseMessage> EnumerateAllCustomResources(HttpRequestMessage requestMessage, CloudTable tableStorage, string partitionKey, string resourceType)
{
    // Generate upper bound of the query.
    var rowKeyUpperBound = new StringBuilder(resourceType);
    rowKeyUpperBound[rowKeyUpperBound.Length - 1]++;

    // Create the enumeration query.
    var enumerationQuery = new TableQuery<CustomResource>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, partitionKey),
            TableOperators.And,
            TableQuery.CombineFilters(
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, resourceType),
                TableOperators.And,
                TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, rowKeyUpperBound.ToString()))));
    
    var customResources = (await tableStorage.ExecuteQuerySegmentedAsync(enumerationQuery, null))
        .ToList().Select(customResource => JToken.Parse(customResource.Data));

    var enumerationResponse = requestMessage.CreateResponse(HttpStatusCode.OK);
    enumerationResponse.Content = new StringContent(new JObject(new JProperty("value", customResources)).ToString(), System.Text.Encoding.UTF8, "application/json");
    return enumerationResponse;
}
```

> [!NOTE]
> Der Zeilenschlüssel für „größer als“ und „kleiner als“ ist Azure-Tabellensyntax zum Durchführen einer „startswith“-Abfrage für Zeichenfolgen. 

Zum Auflisten aller vorhandenen Ressourcen generieren wir eine Azure-Tabellenabfrage, mit der sichergestellt wird, dass die Ressourcen unter unserer Partition mit dem benutzerdefinierten Anbieter vorhanden sind. Die Abfrage prüft anschließend, ob der Zeilenschlüssel mit dem gleichen `{myResourceType}`-Element beginnt.

## <a name="integrate-restful-operations"></a>Integrieren von RESTful-Vorgängen

Nachdem alle RESTful-Methoden der Funktion hinzugefügt wurden, können wir die `Run`-Hauptmethode so aktualisieren, dass die Funktionen zum Verarbeiten der unterschiedlichen REST-Anforderungen aufgerufen werden:

```csharp
/// <summary>
/// Entry point for the Azure Function webhook and acts as the service behind a custom provider.
/// </summary>
/// <param name="requestMessage">The http request message.</param>
/// <param name="log">The logger.</param>
/// <param name="tableStorage">The Azure Storage Account table.</param>
/// <returns>The http response for the custom Azure API.</returns>
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log, CloudTable tableStorage)
{
    // Get the unique Azure request path from request headers.
    var requestPath = req.Headers.GetValues("x-ms-customproviders-requestpath").FirstOrDefault();

    if (requestPath == null)
    {
        var missingHeaderResponse = req.CreateResponse(HttpStatusCode.BadRequest);
        missingHeaderResponse.Content = new StringContent(
            new JObject(new JProperty("error", "missing 'x-ms-customproviders-requestpath' header")).ToString(),
            System.Text.Encoding.UTF8, 
            "application/json");
    }

    log.LogInformation($"The Custom Provider Function received a request '{req.Method}' for resource '{requestPath}'.");

    // Determines if it is a collection level call or action.
    var isResourceRequest = requestPath.Split('/').Length % 2 == 1;
    var azureResourceId = isResourceRequest ? 
        ResourceId.FromString(requestPath) :
        ResourceId.FromString($"{requestPath}/");

    // Create the Partition Key and Row Key
    var partitionKey = $"{azureResourceId.SubscriptionId}:{azureResourceId.ResourceGroupName}:{azureResourceId.Parent.Name}";
    var rowKey = $"{azureResourceId.FullResourceType.Replace('/', ':')}:{azureResourceId.Name}";

    switch (req.Method)
    {
        // Action request for an custom action.
        case HttpMethod m when m == HttpMethod.Post && !isResourceRequest:
            return await TriggerCustomAction(
                requestMessage: req);

        // Enumerate request for all custom resources.
        case HttpMethod m when m == HttpMethod.Get && !isResourceRequest:
            return await EnumerateAllCustomResources(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                resourceType: rowKey);

        // Retrieve request for a custom resource.
        case HttpMethod m when m == HttpMethod.Get && isResourceRequest:
            return await RetrieveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Create request for a custom resource.
        case HttpMethod m when m == HttpMethod.Put && isResourceRequest:
            return await CreateCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                azureResourceId: azureResourceId,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Remove request for a custom resource.
        case HttpMethod m when m == HttpMethod.Delete && isResourceRequest:
            return await RemoveCustomResource(
                requestMessage: req,
                tableStorage: tableStorage,
                partitionKey: partitionKey,
                rowKey: rowKey);

        // Invalid request received.
        default:
            return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}
``` 

Die aktualisierte `Run`-Methode enthält jetzt die `tableStorage`-Eingabebindung, die für Azure-Tabellenspeicher hinzugefügt wurde. Im ersten Teil der Methode wird jetzt der `x-ms-customproviders-requestpath`-Header gelesen und die `Microsoft.Azure.Management.ResourceManager.Fluent`-Bibliothek verwendet, um den Wert als Ressourcen-ID zu analysieren. Der `x-ms-customproviders-requestpath`-Header wird vom benutzerdefinierten Anbieter gesendet und gibt den Pfad der eingehenden Anforderung an. Mit der analysierten Ressourcen-ID können wir jetzt den Partitionsschlüssel (partitionKey) und Zeilenschlüssel (rowKey) für die Daten generieren, um nach benutzerdefinierten Ressourcen zu suchen oder diese zu speichern.

Zusätzlich zum Hinzufügen der Methoden und Klassen müssen wir die using-Methoden für die Funktion aktualisieren. Fügen Sie am Anfang der Datei Folgendes hinzu:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"
#r "../bin/Microsoft.Azure.Management.ResourceManager.Fluent.dll"

using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Configuration;
using System.Text;
using System.Threading;
using System.Globalization;
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Falls Sie während dieses Tutorials durcheinander gekommen sein sollten, ist dies kein Problem. Sie finden das vollständige Codebeispiel in der [Referenz zu C#-RESTful-Endpunkten für benutzerdefinierte Anbieter](./reference-custom-providers-csharp-endpoint.md). Speichern Sie nach der Fertigstellung der Funktion die Funktions-URL, die verwendet werden kann, um die Funktion auszulösen. Sie wird in späteren Tutorials noch benötigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir einen RESTful-Endpunkt erstellt, der als Azure-Endpunkt (`endpoint`) für einen benutzerdefinierten Anbieter verwendet werden kann. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie einen benutzerdefinierten Anbieter erstellen.

- [Tutorial: Erstellen eines benutzerdefinierten Anbieters](./tutorial-custom-providers-create.md)
