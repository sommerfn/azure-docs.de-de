---
title: Arbeiten mit großen Datasets
description: Hier erfahren Sie, wie Sie bei der Verwendung von Azure Resource Graph umfangreiche Datasets abrufen und steuern.
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/18/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: bcc272a8189ebb175f546f6a50c2c117a7975216
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72800184"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Arbeiten mit großen Datasets von Azure-Ressourcen

Azure Resource Graph ist für das Arbeiten mit Informationen zu Ressourcen in Ihrer Azure-Umgebung und das Abrufen von diesen konzipiert. Mit Resource Graph erhalten Sie diese Daten schnell, selbst beim Abfragen von Tausenden von Datensätzen. Resource Graph umfasst mehrere Optionen für das Arbeiten mit diesen großen Datasets.

Anleitungen zum Arbeiten mit sehr häufigen Abfragen finden Sie im [Leitfaden für gedrosselte Anforderungen](./guidance-for-throttled-requests.md).

## <a name="data-set-result-size"></a>Ergebnisgröße von Datasets

Standardmäßig sind in Resource Graph alle Abfragen so beschränkt, dass nur **100** Datensätze zurückgegeben werden. Durch diese Steuerung werden sowohl Benutzer als auch der Dienst vor unbeabsichtigten Abfragen geschützt, die zu großen Datasets führen. Dieser Fall tritt meistens auf, wenn ein Kunde Abfragen testet, um Ressourcen seinen speziellen Anforderungen entsprechend zu suchen und zu filtern. Dieses Steuerelement unterscheidet sich von der Verwendung der Azure Data Explorer-Sprachoperatoren [top](/azure/kusto/query/topoperator) und [limit](/azure/kusto/query/limitoperator) zum Einschränken der Ergebnisse.

> [!NOTE]
> Bei Verwendung von **First** sollten die Ergebnisse nach mindestens einer Spalte mit `asc` oder `desc` sortiert werden. Ohne Sortierung sind die zurückgegebenen Ergebnisse zufällig und nicht wiederholbar.

Das Standardlimit kann über alle Methoden für die Interaktion mit Resource Graph überschrieben werden. Die folgenden Beispiele zeigen, wie die Größenbeschränkung von Datasets in _200_ geändert wird:

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -First 200
```

In der [REST-API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources) wird das Steuerelement **$top** verwendet, das Teil von **QueryRequestOptions** ist.

Das jeweils _restriktivste_ Steuerelement hat Vorrang. Wenn in einer Abfrage beispielsweise die Operatoren **top** oder **limit** verwendet werden und sich dabei mehr Datensätze als bei **First** ergeben, entspricht die maximale Anzahl der zurückgegebenen Datensätze der durch **First** vorgegebenen Anzahl. Wenn **top** oder **limit** kleiner ist als **First**, wird als Datensatzgruppe jeweils der kleinere Wert der durch **top** oder **limit** konfigurierten Werte zurückgegeben.

Für **First** ist derzeit _5000_ als maximal zulässiger Wert festgelegt.

## <a name="skipping-records"></a>Überspringen von Datensätzen

Als weitere Option für das Arbeiten mit großen Datasets kann das Steuerelement **Skip** verwendet werden. Über dieses Steuerelement kann bei der Abfrage die definierte Anzahl von Datensätzen vor dem Zurückgeben der Ergebnisse übersprungen werden. **Skip** eignet sich für Abfragen, die Ergebnisse auf sinnvolle Weise sortieren, um so Datensätze abzurufen, die sich in der Mitte des Resultsets befinden. Wenn sich die erforderlichen Ergebnisse am Ende des zurückgegebenen Datasets befinden, ist es effizienter, eine andere Sortierungskonfiguration zu verwenden und stattdessen die Ergebnisse vom Anfang des Datasets abzurufen.

> [!NOTE]
> Bei Verwendung von **Skip** sollten die Ergebnisse nach mindestens einer Spalte mit `asc` oder `desc` sortiert werden. Ohne Sortierung sind die zurückgegebenen Ergebnisse zufällig und nicht wiederholbar.

Die folgenden Beispiele zeigen, wie die ersten _10_ Datensätze übersprungen werden, die sich bei einer Abfrage ergeben, anstatt das zurückgegebene Resultset mit dem 11. Datensatz zu beginnen:

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -Skip 10
```

In der [REST-API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources) wird das Steuerelement **$skip** verwendet, das Teil von **QueryRequestOptions** ist.

## <a name="paging-results"></a>Auslagerungsergebnisse

Wenn es erforderlich ist, ein Resultset in kleinere Datensatzgruppen zu unterteilen, um sie zu verarbeiten oder weil das Resultset den maximal zulässigen Wert von _1000_ zurückgegebenen Datensätzen überschreitet, verwenden Sie die Paginierung. Die **QueryResponse**-[REST-API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources) enthält folgende Werte, um anzugeben, dass ein Resultset aufgeteilt wurde: **resultTruncated** und **$skipToken**.
**resultTruncated** ist ein boolescher Wert, über den der Consumer informiert wird, wenn zusätzliche Datensätze vorhanden sind, die in der Antwort nicht zurückgegeben werden. Diese Bedingung kann auch erkannt werden, wenn der Wert der Eigenschaft **count** kleiner ist als der Wert der Eigenschaft **totalRecords**. **totalRecords** definiert die Anzahl der Datensätze, die mit der Abfrage übereinstimmen.

Wenn **resultTruncated** auf **true** festgelegt ist, wird in der Antwort die Eigenschaft **$skipToken** festgelegt. Dieser Wert wird mit den gleichen Werten für die Abfrage und das Abonnement verwendet, um die nächste Datensatzgruppe abzurufen, die mit der Abfrage übereinstimmt.

Die folgenden Beispiele zeigen, wie Sie mit Azure CLI und Azure PowerShell die ersten 3000 Datensätze **überspringen** und die **ersten** 1000 Datensätze nach den übersprungenen Datensätzen zurückgeben:

```azurecli-interactive
az graph query -q "Resources | project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> Die Abfrage muss das Feld **id** **projizieren**, damit die Paginierung funktioniert. Wenn das Feld in der Abfrage nicht vorhanden ist, enthält die Antwort nicht die Eigenschaft **$skipToken**.

Ein Beispiel finden Sie unter [Next page query](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources#next-page-query) (Abfrage der nächsten Seite) in der REST-API-Dokumentation.

## <a name="formatting-results"></a>Formatieren von Ergebnissen

Die Ergebnisse einer Resource Graph-Abfrage werden in zwei Formaten bereitgestellt: _Table_ und _ObjectArray_. Das Format wird als Teil der Anforderungsoptionen mit dem Parameter **resultFormat**  konfiguriert. Das _Table_-Format ist der Standardwert für **resultFormat**.

Ergebnisse von Azure CLI werden standardmäßig im JSON-Format bereitgestellt. Ergebnisse in Azure PowerShell sind standardmäßig **PSCustomObject**, können jedoch schnell mithilfe des Cmdlets `ConvertTo-Json` in das JSON-Format konvertiert werden. Bei anderen SDKs können die Abfrageergebnisse für die Ausgabe im _ObjectArray_-Format konfiguriert werden.

### <a name="format---table"></a>Format „Table“

Das Standardformat _Table_ gibt Ergebnisse in einem JSON-Format zurück, das den Spaltenentwurf und die Zeilenwerte der von der Abfrage zurückgegebenen Eigenschaften hervorhebt. Dieses Format ähnelt Daten, die in einer strukturierten Tabelle oder Kalkulationstabelle definiert sind, wobei die Spalten zuerst angegeben sind, gefolgt von den einzelnen Zeilen zur Darstellung der Daten, die diesen Spalten zugeordnet sind.

Es folgt ein Beispiel für ein Abfrageergebnis mit _Table_-Formatierung:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": {
        "columns": [{
                "name": "name",
                "type": "string"
            },
            {
                "name": "type",
                "type": "string"
            },
            {
                "name": "location",
                "type": "string"
            },
            {
                "name": "subscriptionId",
                "type": "string"
            }
        ],
        "rows": [
            [
                "veryscaryvm2-nsg",
                "microsoft.network/networksecuritygroups",
                "eastus",
                "11111111-1111-1111-1111-111111111111"
            ]
        ]
    },
    "facets": [],
    "resultTruncated": "true"
}
```

### <a name="format---objectarray"></a>Format „ObjectArray“

Das _ObjectArray_-Format gibt ebenfalls Ergebnisse in einem JSON-Format zurück. Dieser Entwurf richtet sich jedoch nach der in JSON üblichen Schlüssel-Wert-Paarbeziehung, wobei die Spalten und die Zeilendaten in Arraygruppen abgeglichen werden.

Es folgt ein Beispiel für ein Abfrageergebnis mit _ObjectArray_-Formatierung:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": [{
        "name": "veryscaryvm2-nsg",
        "type": "microsoft.network/networksecuritygroups",
        "location": "eastus",
        "subscriptionId": "11111111-1111-1111-1111-111111111111"
    }],
    "facets": [],
    "resultTruncated": "true"
}
```

Es folgen einige Beispiele für das Festlegen von **resultFormat** für die Verwendung des _ObjectArray_-Formats:

```csharp
var requestOptions = new QueryRequestOptions( resultFormat: ResultFormat.ObjectArray);
var request = new QueryRequest(subscriptions, "Resources | limit 1", options: requestOptions);
```

```python
request_options = QueryRequestOptions(
    result_format=ResultFormat.object_array
)
request = QueryRequest(query="Resources | limit 1", subscriptions=subs_list, options=request_options)
response = client.resources(request)
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur verwendeten Sprache finden Sie unter [Einfache Abfragen](../samples/starter.md).
- Informationen zur anspruchsvolleren Nutzung finden Sie unter [Erweiterte Abfragen](../samples/advanced.md).
- Erfahren Sie, wie Sie [Ressourcen untersuchen](explore-resources.md).