---
title: Leitfaden für gedrosselte Anforderungen
description: Erfahren Sie, wie Sie bessere Abfragen erstellen können, um gedrosselte Anforderungen an Azure Resource Graph zu vermeiden.
ms.date: 10/18/2019
ms.topic: conceptual
ms.openlocfilehash: 651a5daa9e7e19a5dc157ba0cfa17da2c8abe3db
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74038327"
---
# <a name="guidance-for-throttled-requests-in-azure-resource-graph"></a>Leitfaden für gedrosselte Anforderungen in Azure Resource Graph

Beim programmgesteuerten Erstellen und der häufigen Nutzung von Azure Resource Graph-Daten sollten Sie sich Gedanken dazu machen, welche Auswirkungen gedrosselte Anforderungen auf die Ergebnisse der Abfragen haben. Indem Sie die Anforderungsweise von Daten verändern, können Sie und Ihre Organisation die Drosselung von Anforderungen vermeiden und den Flow von zeitgenauen Daten zu Ihren Azure-Ressourcen aufrechterhalten.

In diesem Artikel werden vier Bereiche und Muster behandelt, die im Zusammenhang mit der Erstellung von Abfragen in Azure Resource Graph stehen:

- Grundlegendes zu Drosselungsheadern
- Batchverarbeitungsabfragen
- Staffelungsabfragen
- Auswirkungen der Paginierung

## <a name="understand-throttling-headers"></a>Grundlegendes zu Drosselungsheadern

Azure Resource Graph ordnet, abhängig von einem Zeitfenster, jedem Benutzer eine Kontingentnummer zu. Ein Benutzer kann innerhalb eines Zeitfensters von 5 Sekunden beispielsweise höchstens 15 Abfragen ohne Drosselung senden. Der Kontingentwert wird von vielen Faktoren bestimmt und kann verändert werden.

In jeder Abfrageantwort fügt Azure Resource Graph zwei Drosselungsheader ein:

- `x-ms-user-quota-remaining` (int): Das verbleibende Ressourcenkontingent für den Benutzer. Dieser Wert entspricht der Anzahl von Abfragen.
- `x-ms-user-quota-resets-after` (hh:mm:ss): Der Zeitraum, bis die Kontingentnutzung eines Benutzers zurückgesetzt wird.

Zur Veranschaulichung der Funktionsweise von Headern sehen wir uns eine Abfrageantwort mit dem Header und den Werten von `x-ms-user-quota-remaining: 10` und `x-ms-user-quota-resets-after: 00:00:03` an.

- In den nächsten 3 Sekunden werden höchstens 10 Abfragen ohne Drosselung übermittelt.
- In 3 Sekunden werden die Werte von `x-ms-user-quota-remaining` und `x-ms-user-quota-resets-after` auf `15` bzw. `00:00:05` zurückgesetzt.

Ein Beispiel für die Verwendung der Header zum Durchführen eines _Backoff_ für Abfrageanforderungen finden Sie im Beispiel in [Parallele Abfrage](#query-in-parallel).

## <a name="batching-queries"></a>Batchverarbeitungsabfragen

Die Batchverarbeitung von Abfragen eines Abonnements, einer Ressourcengruppe oder einer einzelnen Ressource ist effizienter als das Parallelisieren von Abfragen. Die Kontingentkosten für eine größere Abfrage sind oft geringer als die Kontingentkosten für viele kleine und gezielte Abfragen. Die Batchgröße sollte kleiner als _300_ sein.

- Beispiel für einen schlecht optimierten Batchverarbeitungsansatz

  ```csharp
  // NOT RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  foreach (var subscriptionId in subscriptionIds)
  {
      var userQueryRequest = new QueryRequest(
          subscriptions: new[] { subscriptionId },
          query: "Resoures | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

  // ...
  }
  ```

- Beispiel 1 für einen optimierten Batchverarbeitungsansatz

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var subscriptionIds = /* A big list of subscriptionIds */

  const int batchSize = 100;
  for (var i = 0; i <= subscriptionIds.Count / batchSize; ++i)
  {
      var currSubscriptionBatch = subscriptionIds.Skip(i * batchSize).Take(batchSize).ToList();
      var userQueryRequest = new QueryRequest(
          subscriptions: currSubscriptionBatch,
          query: "Resources | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

- Beispiel 2 für einen optimierten Batchverarbeitungsansatz

  ```csharp
  // RECOMMENDED
  var header = /* your request header */
  var resourceIds = /* A big list of resourceIds */

  const int batchSize = 100;
  for (var i = 0; i <= resourceIds.Count / batchSize; ++i)
  {
      var resourceIdBatch = string.Join(",",
          resourceIds.Skip(i * batchSize).Take(batchSize).Select(id => string.Format("'{0}'", id)));
      var userQueryRequest = new QueryRequest(
          subscriptions: subscriptionList,
          query: $"Resources | where id in~ ({resourceIds}) | project name, type");

      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
          .ConfigureAwait(false);

    // ...
  }
  ```

## <a name="staggering-queries"></a>Staffelungsabfragen

Aufgrund der Art und Weise, wie die Drosselung erzwungen wird, werden gestaffelte Abfragen empfohlen. Dies bedeutet, dass Sie die Abfragen in vier Zeitfenster von jeweils 5 Sekunden staffeln, anstatt 60 Abfragen gleichzeitig zu senden:

- Nicht gestaffelter Abfragezeitplan

  | Abfrageanzahl         | 60  | 0    | 0     | 0     |
  |---------------------|-----|------|-------|-------|
  | Zeitintervall (in Sekunden) | 0-5 | 5-10 | 10-15 | 15 – 20 |

- Gestaffelter Abfragezeitplan

  | Abfrageanzahl         | 15  | 15   | 15    | 15    |
  |---------------------|-----|------|-------|-------|
  | Zeitintervall (in Sekunden) | 0-5 | 5-10 | 10-15 | 15 – 20 |

Im Folgenden finden Sie ein Beispiel für die Verwendung von Drosselungsheadern beim Abfragen von Azure Resource Graph:

```csharp
while (/* Need to query more? */)
{
    var userQueryRequest = /* ... */
    // Send post request to Azure Resource Graph
    var azureOperationResponse = await this.resourceGraphClient
        .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
        .ConfigureAwait(false);

    var responseHeaders = azureOperationResponse.response.Headers;
    int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
    TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
    if (remainingQuota == 0)
    {
        // Need to wait until new quota is allocated
        await Task.Delay(resetAfter).ConfigureAwait(false);
    }
}
```

### <a name="query-in-parallel"></a>Gleichzeitige Abfrage

Auch wenn die Batchverarbeitung gegenüber der Parallelisierung empfohlen wird, gibt es Situationen, in denen Abfragen nicht einfach verarbeitet werden können. In diesen Fällen sollten Sie Azure Resource Graph abfragen, indem Sie mehrere Abfragen parallel senden. Nachfolgend sehen Sie ein auf den Drosselungsheadern in solchen Szenarios basierendes Beispiel für das _Backoff_:

```csharp
IEnumerable<IEnumerable<string>> queryBatches = /* Batches of queries  */
// Run batches in parallel.
await Task.WhenAll(queryBatches.Select(ExecuteQueries)).ConfigureAwait(false);

async Task ExecuteQueries(IEnumerable<string> queries)
{
    foreach (var query in queries)
    {
        var userQueryRequest = new QueryRequest(
            subscriptions: subscriptionList,
            query: query);
        // Send post request to Azure Resource Graph.
        var azureOperationResponse = await this.resourceGraphClient
            .ResourcesWithHttpMessagesAsync(userQueryRequest, header)
            .ConfigureAwait(false);
        
        var responseHeaders = azureOperationResponse.response.Headers;
        int remainingQuota = /* read and parse x-ms-user-quota-remaining from responseHeaders */
        TimeSpan resetAfter = /* read and parse x-ms-user-quota-resets-after from responseHeaders */
        if (remainingQuota == 0)
        {
            // Delay by a random period to avoid bursting when the quota is reset.
            var delay = (new Random()).Next(1, 5) * resetAfter;
            await Task.Delay(delay).ConfigureAwait(false);
        }
    }
}
```

## <a name="pagination"></a>Paginierung

Da Azure Resource Graph in einer einzelnen Abfrageantwort höchstens 1000 Einträge zurückgibt, müssen Sie Ihre Abfragen möglicherweise [paginieren](./work-with-data.md#paging-results), um das vollständige Dataset zu erhalten, das Sie suchen. Einige Azure Resource Graph-Clients verarbeiten die Paginierung jedoch unterschiedlich.

- C# SDK

  Wenn Sie das „ResourceGraph“-SDK verwenden, erfolgt die Paginierung durch das Übergeben des von der vorherigen Abfrageantwort an die paginierte Abfrage zurückgegebenen Skiptokens. Das bedeutet, dass Sie Ergebnisse aus allen paginierten Aufrufen sammeln und diese am Ende vereinen müssen. In diesem Fall beansprucht jede gesendete paginierte Abfrage ein Abfragekontingent:

  ```csharp
  var results = new List<object>();
  var queryRequest = new QueryRequest(
      subscriptions: new[] { mySubscriptionId },
      query: "Resources | project id, name, type | top 5000");
  var azureOperationResponse = await this.resourceGraphClient
      .ResourcesWithHttpMessagesAsync(queryRequest, header)
      .ConfigureAwait(false);
  while (!string.Empty(azureOperationResponse.Body.SkipToken))
  {
      queryRequest.SkipToken = azureOperationResponse.Body.SkipToken;
      // Each post call to ResourceGraph consumes one query quota
      var azureOperationResponse = await this.resourceGraphClient
          .ResourcesWithHttpMessagesAsync(queryRequest, header)
          .ConfigureAwait(false);
      results.Add(azureOperationResponse.Body.Data.Rows);

      // Inspect throttling headers in query response and delay the next call if needed.
  }
  ```

- Azure CLI/Azure PowerShell

  Wenn Sie die Azure CLI oder Azure PowerShell verwenden, werden Abfragen an Azure Resource Graph automatisch paginiert, um maximal 5000 Einträge zu fetchen. Die Abfrageergebnisse geben eine kombinierte Liste mit Einträgen aus allen paginierten Aufrufen zurück. In diesem Fall nutzt eine einzelne paginierte Abfrage (je nach Anzahl der Einträge im Abfrageergebnis) möglicherweise mehr als ein Abfragekontingent. Im folgenden Beispiel werden bei der einzelnen Ausführung einer Abfrage bis zu fünf Abfragekontingente verwendet:

  ```azurecli-interactive
  az graph query -q 'Resources | project id, name, type' --first 5000
  ```

  ```azurepowershell-interactive
  Search-AzGraph -Query 'Resources | project id, name, type' -First 5000
  ```

## <a name="still-get-throttled"></a>Werden die Anforderungen weiterhin gedrosselt?

Kontaktieren Sie das Microsoft-Team über [resourcegraphsupport@microsoft.com](mailto:resourcegraphsupport@microsoft.com), wenn die Anforderungen nach dem Ausführen der oben genannten Empfehlungen auch weiterhin gedrosselt werden.

Geben Sie die folgenden Informationen an:

- Ihren spezifischen Anwendungsfall und Anforderungen an den Gerätetreiber Ihres Unternehmens für ein höheres Drosselungslimit.
- Auf wie viele Ressourcen haben Sie Zugriff? Wie viele davon werden von einer einzelnen Abfrage zurückgegeben?
- Welche Arten von Ressourcen möchten Sie verwenden?
- Welches Abfragemuster verwenden Sie? X Abfragen pro Y Sekunden usw.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Erfahren Sie mehr über das [Erkunden von Ressourcen](explore-resources.md).