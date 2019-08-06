---
title: Entwickeln effizienter Listenabfragen – Azure Batch | Microsoft-Dokumentation
description: Steigern Sie durch Filtern von Abfragen die Leistung beim Anfordern von Informationen zu Batch-Ressourcen wie Pools, Aufträgen, Aufgaben und Computeknoten.
services: batch
documentationcenter: .net
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 12/07/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 3bf9ba52bc4071755918b842da477384dcd38973
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323509"
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Erstellen von Abfragen zum effizienten Auflisten von Batch-Ressourcen

Hier erfahren Sie, wie Sie die Leistung Ihrer Azure Batch-Anwendung steigern, indem Sie die Datenmenge verringern, die vom Dienst zurückgegeben wird, wenn Sie Aufträge, Aufgaben, Computeknoten und andere Ressourcen mit der [Batch-Bibliothek für .NET][api_net] abfragen.

Nahezu alle Batch-Anwendungen führen eine Überwachung oder eine andere Art von Vorgang aus, die den Batch-Dienst (häufig in regelmäßigen Abständen) abfragt. Wenn Sie etwa bestimmen möchten, ob bei einem Auftrag noch Aufgaben in der Warteschlange vorhanden sind, müssen Daten zu allen Aufgaben des Auftrags abgerufen werden. Wenn Sie den Status von Knoten im Pool ermitteln möchten, müssen Daten zu jedem Knoten im Pool abgerufen werden. In diesem Artikel wird erläutert, wie diese Abfragen auf möglichst effiziente Weise ausgeführt werden.

> [!NOTE]
> Der Batch-Dienst bietet spezielle API-Unterstützung für allgemeine Szenarios, in denen Tasks in einem Auftrag und Compute-Knoten in einem Batch-Pool gezählt werden. Statt dafür eine „list“-Abfrage zu verwenden, können Sie die Vorgänge [Get Task Counts][rest_get_task_counts] and [List Pool Node Counts][rest_get_node_counts] aufrufen. Diese Vorgänge sind effizienter als eine Listenabfrage, geben jedoch eingeschränkte Informationen zurück. Weitere Informationen finden Sie unter [Monitor Batch solutions by counting tasks and nodes by state (Überwachen von Batch-Lösungen durch das Zählen von Tasks und Knoten nach Bundesstaat)](batch-get-resource-counts.md). 


## <a name="meet-the-detaillevel"></a>DetailLevel
In einer Batch-Produktionsanwendung kann die Anzahl von Entitäten wie Aufträgen, Aufgaben und Computeknoten leicht in die Tausende gehen. Beim Anfordern von Informationen zu diesen Ressourcen muss häufig für jede Abfrage eine größere Menge von Daten den Weg vom Batch-Dienst zu Ihrer Anwendung zurücklegen. Durch Einschränken der Anzahl von Elementen und der Art der zurückgegebenen Informationen können Sie die Geschwindigkeit Ihrer Abfragen und damit die Leistung der Anwendung steigern.

Der folgende [Batch .NET][api_net]-API-Codeausschnitt listet *jede* Aufgabe im Zusammenhang mit einem Auftrag sowie *alle* Eigenschaften der einzelnen Aufgaben auf:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Durch Anwenden einer Detailebene auf die Abfrage lässt sich die Effizienz von Listenabfragen deutlich erhöhen. Dazu stellen Sie ein [ODATADetailLevel][odata] object to the [JobOperations.ListTasks][net_list_tasks]-Methode bereit. Dieser Codeausschnitt gibt nur die ID-, Befehlszeilen- und Computeknoteneigenschaften abgeschlossener Aufgaben zurück:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Wenn der Auftrag wie in diesem Beispielszenario Tausende von Aufgaben umfasst, werden die Ergebnisse der zweiten Abfrage meist viel schneller als die der ersten zurückgegeben. Weitere Informationen zum Verwenden von ODATADetailLevel beim Auflisten von Elementen mit der Batch .NET-API finden Sie [weiter unten](#efficient-querying-in-batch-net)in diesem Dokument.

> [!IMPORTANT]
> Es wird dringend empfohlen, *immer* ein ODATADetailLevel-Objekt für Ihre .NET-API-Listenaufrufe zu verwenden, um eine maximale Effizienz und optimale Leistung der Anwendung zu gewährleisten. Das Angeben einer Detailebene hilft dem Batch-Dienst beim Verkürzen von Antwortzeiten, Verbessern der Netzwerkauslastung und Minimieren der Speicherbelegung von Clientanwendungen.
> 
> 

## <a name="filter-select-and-expand"></a>Filtern, Auswählen und Erweitern
Die [Batch .NET][api_net] and [Batch REST][api_rest]-APIs ermöglichen es, sowohl die Anzahl der in einer Liste zurückgegebenen Elemente als auch die Menge der jeweils zurückgegebenen Informationen zu verringern. Dazu geben Sie beim Ausführen von Listenabfragen Zeichenfolgen zum **Filtern**, **Auswählen** und **Erweitern** an.

### <a name="filter"></a>Filter
Die Filterzeichenfolge ist ein Ausdruck, der die Anzahl der zurückgegebenen Elemente reduziert. Sie können beispielsweise die derzeit ausgeführten Aufgaben für einen Auftrag oder nur Computeknoten auflisten, die zum Ausführen von Aufgaben bereit sind.

* Eine Filterzeichenfolge besteht aus mindestens einem Ausdruck, wobei ein Ausdruck aus einem Eigenschaftsnamen, einem Operator und einem Wert besteht. Die Eigenschaften, die angegeben werden können, sind spezifisch für jeden Entitätstyp, den Sie abfragen. Dies gilt auch für die für jede Eigenschaft unterstützten Operatoren.
* Mehrere Ausdrücke können mithilfe der logischen Operatoren `and` und `or` kombiniert werden.
* Das folgende Beispiel für eine Filterzeichenfolge listet nur die aktuell ausgeführten Renderaufgaben auf: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Select
Die Auswählzeichenfolge begrenzt die Eigenschaftswerte, die für jedes Element zurückgegeben werden. Sie geben eine Liste mit Eigenschaftsnamen an, woraufhin nur diese Eigenschaftswerte für die Elemente in den Abfrageergebnissen zurückgegeben werden.

* Die Auswählzeichenfolge besteht aus einer durch Trennzeichen getrennte Liste von Eigenschaftsnamen. Sie können beliebige Eigenschaften für den Entitätstyp angeben, den Sie abfragen möchten.
* Das folgende Beispiel für eine Auswählzeichenfolge gibt an, dass für jede Aufgabe nur drei Eigenschaftswerte zurückgegeben werden sollen: `id, state, stateTransitionTime`.

### <a name="expand"></a>Expand
Die Erweiterungszeichenfolge verringert die Anzahl der API-Aufrufe, die zum Abrufen bestimmter Informationen erforderlich sind. Wenn Sie eine Erweiterungszeichenfolge verwenden, können mit einem einzigen API-Aufruf weitere Informationen zu den einzelnen Elementen abgerufen werden. Anstatt zunächst die Liste der Entitäten abzurufen und dann Informationen zu den einzelnen Elementen in der Liste anzufordern, können Sie dieselben Informationen mithilfe einer Erweiterungszeichenfolge mit einem einzigen API-Aufruf abrufen. Je weniger API-Aufrufe, desto besser die Leistung.

* Ähnlich wie die Auswählzeichenfolge steuert die Erweiterungszeichenfolge, ob bestimmte Daten in die Listenabfrageergebnisse einbezogen werden.
* Die Erweiterungszeichenfolge wird nur beim Auflisten von Aufträgen, Auftragszeitplänen, Aufgaben und Pools unterstützt. Derzeit bietet sie nur Unterstützung für Statistikinformationen.
* Wenn alle Eigenschaften erforderlich sind und keine Auswählzeichenfolge angegeben wurde, *muss* die Erweiterungszeichenfolge zum Abrufen von Statistikinformationen verwendet werden. Wenn eine Auswählzeichenfolge zum Abrufen einer Teilmenge der Eigenschaften verwendet wird, kann `stats` in der Auswählzeichenfolge angegeben werden, und die Erweiterungszeichenfolge muss nicht angegeben werden.
* Das folgende Beispiel für eine Erweiterungszeichenfolge gibt an, dass für jedes Element in der Liste Statistikinformationen zurückgegeben werden sollen: `stats`.

> [!NOTE]
> Beim Erstellen dieser drei Typen von Abfragezeichenfolgen (Filtern, Auswählen, Erweitern) müssen Sie sicherstellen, dass die Namen und die Groß-/Kleinschreibung der Eigenschaften mit den entsprechenden REST-API-Elementen übereinstimmen. Wenn Sie beispielsweise mit der .NET-Klasse [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) arbeiten, müssen Sie **state** anstelle von **State** angeben, obwohl die .NET-Eigenschaft [CloudTask.State](/dotnet/api/microsoft.azure.batch.cloudtask.state#Microsoft_Azure_Batch_CloudTask_State) lautet. In den folgenden Tabellen finden Sie Eigenschaftszuordnungen zwischen der .NET- und der REST-API.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Regeln für Filter-, Auswähl- und Erweiterungszeichenfolgen
* Eigenschaftsnamen in Filter-, Auswähl- und Erweiterungszeichenfolgen sollten angezeigt werden wie in der [Batch REST][api_rest] API--even when you use [Batch .NET][api_net] oder eines der anderen Batch SDKs verwenden.
* Bei allen Eigenschaftsnamen muss die Groß- und Kleinschreibung beachtet werden, bei Eigenschaftswerten dagegen nicht.
* Datums-/Uhrzeitzeichenfolgen können eines von zwei Formaten besitzen, und ihnen muss `DateTime`vorangestellt sein.
  
  * Beispiel für das W3C-DTF-Format: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * Beispiel für das RFC 1123-Format: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Boolesche Zeichenfolgen sind entweder `true` oder `false`.
* Bei Angabe einer ungültigen Eigenschaft oder eines ungültigen Operators tritt der Fehler `400 (Bad Request)` auf.

## <a name="efficient-querying-in-batch-net"></a>Effizientes Abfragen in Batch .NET
Innerhalb der [Batch .NET][api_net] API, the [ODATADetailLevel][odata]-Klasse zur Bereitstellung von Filter-, Auswähl- und Erweiterungszeichenfolgen für Auflistungsvorgänge verwendet. Die „ODataDetailLevel“-Klasse verfügt über drei öffentliche Zeichenfolgeneigenschaften, die im Konstruktor angegeben oder direkt für das Objekt festgelegt werden können. Sie übergeben dann das „ODataDetailLevel“-Objekt als Parameter an die verschiedenen Auflistungsvorgänge wie [ListPools][net_list_pools], [ListJobs][net_list_jobs], and [ListTasks][net_list_tasks].

* [ODATADetailLevel][odata].[FilterClause][odata_filter]: Begrenzt die Anzahl der zurückgegebenen Elemente.
* [ODATADetailLevel][odata].[SelectClause][odata_select]: Gibt an, welche Eigenschaftswerte mit jedem Element zurückgegeben werden sollen.
* [ODATADetailLevel][odata].[ExpandClause][odata_expand]: Ruft Daten für alle Elemente in einem einzigen API-Aufruf statt mit separaten Aufrufen für jedes Element ab.

Im folgenden Codeausschnitt wird die Batch .NET-API verwendet, um den Batch-Dienst effizient auf die Statistik zu einer bestimmten Gruppe von Pools abzufragen. In diesem Szenario verfügt der Batch-Benutzer über Test- und Produktionspools. Den Testpool-IDs ist das Präfix „test“ vorangestellt, und den Produktionspool-IDs ist das Präfix „prod“ vorangestellt. Im Codeausschnitt ist *myBatchClient* eine ordnungsgemäß initialisierte Instanz der [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient) -Klasse.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Eine Instanz von [ODATADetailLevel][odata], die mit „Select“- und „Expand“-Klauseln konfiguriert wurde, kann auch an entsprechende „Get“-Methoden, z.B. [PoolOperations.GetPool](/dotnet/api/microsoft.azure.batch.pooloperations.getpool#Microsoft_Azure_Batch_PoolOperations_GetPool_System_String_Microsoft_Azure_Batch_DetailLevel_System_Collections_Generic_IEnumerable_Microsoft_Azure_Batch_BatchClientBehavior__) übergeben werden, um die Menge der zurückgegebenen Daten zu begrenzen.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Zuordnungen zwischen der Batch REST-API und .NET-API
Eigenschaftsnamen in Filter-, Auswähl- und Erweiterungszeichenfolgen *müssen* ihren Gegenstücken in der REST-API hinsichtlich Name und Groß-/Kleinschreibung entsprechen. Die folgenden Tabellen enthalten die Zuordnungen zwischen den .NET-APIs und ihren REST-API-Entsprechungen.

### <a name="mappings-for-filter-strings"></a>Zuordnungen für Filterzeichenfolgen
* **.NET-Listenmethoden**: Jede der .NET API-Methoden in dieser Spalte akzeptiert ein [ODATADetailLevel][odata]-Objekt als Parameter.
* **REST-Listenanforderungen**: Jede mit dieser Spalte verknüpfte REST-API-Seite enthält eine Tabelle mit den Eigenschaften und Vorgängen, die in *Filterzeichenfolgen* zulässig sind. Sie verwenden diese Eigenschaftsnamen und Vorgänge beim Erstellen einer [ODATADetailLevel.FilterClause][odata_filter]-Zeichenfolge.

| .NET-Listenmethoden | REST-Listenanforderungen |
| --- | --- |
| [CertificateOperations.ListCertificates][net_list_certs] |[Auflisten der Zertifikate in einem Konto][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Auflisten der einer Aufgabe zugeordneten Dateien][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[Auflisten des Status der Aufgaben zur Auftragsvorbereitung und Auftragsfreigabe für einen Auftrag][rest_list_jobprep_status] |
| [JobOperations.ListJobs][net_list_jobs] |[Auflisten der Aufträge in einem Konto][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Auflisten der Dateien auf einem Knoten][rest_list_nodefiles] |
| [JobOperations.ListTasks][net_list_tasks] |[Auflisten der einem Auftrag zugeordneten Aufgaben][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Auflisten der Auftragszeitpläne in einem Konto][rest_list_job_schedules] |
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] |[Auflisten der einem Auftragszeitplan zugeordneten Aufträge][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Auflisten der Computeknoten in einem Pool][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Auflisten der Pools in einem Konto][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Zuordnungen für Auswählzeichenfolgen
* **Batch .NET-Typen**: Batch .NET-API-Typen.
* **REST-API-Entitäten**: Jede Seite in dieser Spalte enthält mindestens eine Tabelle mit den Namen der REST-API-Eigenschaften für den Typ. Diese Eigenschaftsnamen werden beim Erstellen von *Auswählzeichenfolgen* verwendet. Sie verwenden dieselben Eigenschaftsnamen, wenn Sie eine [ODATADetailLevel.SelectClause][odata_select]-Zeichenfolge erstellen.

| Batch .NET-Typen | REST-API-Entitäten |
| --- | --- |
| [Certificate][net_cert] |[Abrufen von Informationen zu einem Zertifikat][rest_get_cert] |
| [CloudJob][net_job] |[Abrufen von Informationen zu einem Auftrag][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[Abrufen von Informationen zu einem Auftragszeitplan][rest_get_schedule] |
| [ComputeNode][net_node] |[Abrufen von Informationen zu einem Knoten][rest_get_node] |
| [CloudPool][net_pool] |[Abrufen von Informationen zu einem Pool][rest_get_pool] |
| [CloudTask][net_task] |[Abrufen von Informationen zu einer Aufgabe][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Beispiel: Erstellen einer Filterzeichenfolge
Beim Erstellen einer Filterzeichenfolge für [ODATADetailLevel.FilterClause][odata_filter], consult the table above under "Mappings for filter strings" to find the REST API documentation page that corresponds to the list operation that you wish to perform. You will find the filterable properties and their supported operators in the first multirow table on that page. If you wish to retrieve all tasks whose exit code was nonzero, for example, this row on [List the tasks associated with a job][rest_list_tasks] gibt die anwendbare Eigenschaftszeichenfolge und anwendbare Operatoren an: Eigenschaft Zulässige Vorgänge

| type | Daher lautet die Filterzeichenfolge zum Auflisten aller Aufgaben mit einem Exitcode ungleich 0 wie folgt: | Beispiel: Erstellen einer Auswählzeichenfolge |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Zum Erstellen von [ODATADetailLevel.SelectClause][odata_select], consult the table above under "Mappings for select strings" and navigate to the REST API page that corresponds to the type of entity that you are listing. You will find the selectable properties and their supported operators in the first multirow table on that page. If you wish to retrieve only the ID and command line for each task in a list, for example, you will find these rows in the applicable table on [Get information about a task][rest_get_task]:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Eigenschaft
type Notizen Die Auswählzeichenfolge zum Einbeziehen von ausschließlich der ID und Befehlszeile für jede aufgeführte Aufgabe lautet dann wie folgt:

| Codebeispiele | Codebeispiel für effiziente Listenabfragen | Sehen Sie sich das [EfficientListQueries][efficient_query_sample] sample project on GitHub to see how efficient list querying can affect performance in an application. This C# console application creates and adds a large number of tasks to a job. Then, it makes multiple calls to the [JobOperations.ListTasks][net_list_tasks]-Methode und übergibt [ODATADetailLevel][odata]-Objekte, die mit verschiedenen Eigenschaftswerten konfiguriert werden, damit eine variierende Datenmenge zurückgegeben wird. |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Sie erzeugt eine Ausgabe ähnlich der Folgenden:

`id, commandLine`

## <a name="code-samples"></a>Wie anhand der verstrichenen Zeit zu sehen, können Sie durch eine Begrenzung der Eigenschaften und der Anzahl von zurückgegebenen Elementen die Antwortzeiten für Abfragen erheblich verkürzen.
### <a name="efficient-list-queries-code-sample"></a>Sie finden dieses Beispielprojekt und weitere Beispielprojekte im [azure-batch-samples][github_samples]-Repository auf GitHub.
BatchMetrics-Bibliothek und Codebeispiel Zusätzlich zum vorstehenden EfficientListQueries-Codebeispiel ist das [BatchMetrics][batch_metrics]project in the [azure-batch-samples][github_samples]-GitHub-Repository zu finden. Das BatchMetrics-Beispielprojekt veranschaulicht die effiziente Überwachung des Azure Batch-Auftragsstatus mithilfe der Batch-API. Das [BatchMetrics][batch_metrics]-Beispiel enthält ein .NET-Klassenbibliotheksprojekt, das Sie in Ihre eigenen Projekte integrieren können, sowie ein einfaches Befehlszeilenprogramm zum Testen und Veranschaulichen der Bibliotheksverwendung.

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Die Beispielanwendung innerhalb des Projekts veranschaulicht folgende Vorgänge: Auswählen bestimmter Attribute, um nur die benötigten Eigenschaften herunterzuladen

### <a name="batchmetrics-library-and-code-sample"></a>Filtern nach Statusübergangszeiten, um nur Änderungen seit der letzten Abfrage herunterzuladen
Die BatchMetrics-Bibliothek enthält beispielsweise die folgende Methode. Diese gibt ein ODATADetailLevel-Objekt zurück, mit dem angegeben wird, dass für die abgefragten Entitäten nur die Eigenschaften `id` und `state` abgerufen werden sollen.

Außerdem gibt es an, dass nur Entitäten zurückgegeben werden sollen, deren Status sich seit dem angegebenen `DateTime` -Parameter geändert hat.

Nächste Schritte

1. Parallele Knotenaufgaben
2. [Maximieren der Azure Batch Compute-Ressourcenauslastung mit parallelen Knotenaufgaben](batch-parallel-node-tasks.md) ist ein weiterer Artikel zur Leistung von Batch-Anwendungen.

Einige Arten von Workloads profitieren von der Ausführung paralleler Aufgaben auf größeren (und dafür weniger) Computeknoten. Ausführlichere Informationen zu einem solchen Szenario finden Sie im [Beispielszenario](batch-parallel-node-tasks.md#example-scenario) des Artikels. It also specifies that only entities whose state has changed since the specified <ph id="ph1">`DateTime`</ph> parameter should be returned.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Next steps
### <a name="parallel-node-tasks"></a>Parallel node tasks
<bpt id="p1">[</bpt>Maximize Azure Batch compute resource usage with concurrent node tasks<ept id="p1">](batch-parallel-node-tasks.md)</ept> is another article related to Batch application performance. Some types of workloads can benefit from executing parallel tasks on larger--but fewer--compute nodes. Check out the <bpt id="p1">[</bpt>example scenario<ept id="p1">](batch-parallel-node-tasks.md#example-scenario)</ept> in the article for details on such a scenario.


[api_net]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: /rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
