---
title: Parallele Ausführung von Tasks zur effizienten Nutzung von Computeressourcen – Azure Batch | Microsoft-Dokumentation
description: Steigern der Effizienz und Reduzieren von Kosten durch Verringern der Serverknotenzahl und Ausführen paralleler Aufgaben auf jedem Knoten in einem Azure Batch-Pool
services: batch
documentationcenter: .net
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/17/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f45c35e6d9fb611ebf73c4eab8b517d8575b8e82
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70094945"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Gleichzeitige Ausführung von Tasks zur optimalen Nutzung von Batch-Computeknoten 

Wenn Sie mehrere Aufgaben gleichzeitig in jedem Computeknoten in Ihrem Azure Batch-Pool ausführen, wird die Maximierung der Ressourcenauslastung auf einer kleineren Anzahl von Knoten im Pool möglich. Bei einigen Workloads kann dies zu kürzeren Auftragszeiten und niedrigeren Kosten führen.

In manchen Szenarien profitieren Sie davon, dass alle Ressourcen eines Knotens einer einzigen Aufgabe zugewiesen werden können. Dagegen profitieren Sie in vielen Situationen davon, diese Ressourcen von mehreren Aufgaben nutzen zu lassen:

* **Minimieren des Datenübertragung** , wenn Aufgaben Daten gemeinsam nutzen können. In diesem Szenario können Sie die Gebühren für die Datenübertragung deutlich reduzieren, indem Sie freigegebene Daten auf eine kleinere Anzahl Knoten kopieren und Aufgaben parallel an jedem Knoten ausführen. Dies trifft besonders dann zu, wenn die zu jedem Knoten zu kopierenden Daten zwischen verschiedenen geografischen Regionen übertragen werden müssen.
* **Maximieren der Speicherauslastung** , wenn Aufgaben viel Speicherplatz beanspruchen – jedoch nur für kurze Zeiträumen und zu unterschiedlichen Zeiten während der Ausführung. Sie können weniger, aber dafür größere Computeknoten mit mehr Arbeitsspeicher einsetzen, um solche Spitzen effizient zu bewältigen. Auf diesen Knoten werden dann mehrere Aufgaben parallel ausgeführt, doch jede Aufgabe kann den reichlich vorhandenen Arbeitsspeicher zu verschiedenen Zeiten nutzen.
* **Verringern der Beschränkungen der Knotenanzahl** , wenn die Kommunikation zwischen Knoten in einem Pool erforderlich ist. Derzeit sind Pools, die für die Kommunikation zwischen Knoten konfiguriert sind, auf 50 Computeknoten beschränkt. Wenn jeder Knoten in einem Pool Aufgaben parallel ausführen kann, kann also eine größere Anzahl von Aufgaben gleichzeitig ausgeführt werden.
* **Replizieren eines lokalen Computeclusters**, z.B. beim ersten Auslagern einer Compute-Umgebung in Azure. Wenn Ihre aktuelle lokale Lösung mehrere Aufgaben pro Computeknoten ausführt, können Sie die maximale Anzahl von Knotenaufgaben erhöhen, um diese Konfiguration besser widerzuspiegeln.

## <a name="example-scenario"></a>Beispielszenario
Um die Vorteile der parallelen Aufgabenausführung zu veranschaulichen, nehmen wir an, dass für Ihre Aufgabenanwendung CPU- und Speicheranforderungen vorliegen, sodass die Knotengröße [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) geeignet ist. Um den Auftrag in der geforderten Zeit abzuschließen, sind jedoch 1.000 dieser Knoten nötig.

Anstatt Standard\_D1-Knoten mit einem CPU-Kern zu verwenden, können Sie [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md)-Knoten mit je 16 Kernen verwenden und so die parallele Ausführung von Aufgaben ermöglichen. Daher könnte *etwa ein Sechzehntel der Knoten* verwendet werden – statt 1.000 Knoten wären nur 63 erforderlich. Wenn große Anwendungsdateien oder Verweisdaten für jeden Knoten erforderlich sind, werden Auftragsdauer und Effizienz außerdem erneut verbessert, da die Daten nur auf 63 Knoten kopiert werden.

## <a name="enable-parallel-task-execution"></a>Aktivieren der parallelen Aufgabenausführung
Sie konfigurieren die Computeknoten für die parallele Aufgabenausführung auf der Pool-Ebene. Bei der Batch .NET-Bibliothek legen Sie die Eigenschaft [CloudPool.MaxTasksPerComputeNode][maxtasks_net] fest, wenn Sie einen Pool erstellen. Bei Verwendung der Batch-REST-API legen Sie das [maxTasksPerNode][rest_addpool]-Element bei der Erstellung des Pools im Anforderungstext fest.

In Azure Batch kann die Anzahl von Aufgaben pro Knoten auf die bis zu vierfache Anzahl von Kernknoten festgelegt werden. Ist der Pool beispielsweise mit Knoten der Größe „Groß“ (vier Kerne) konfiguriert, kann für „ `maxTasksPerNode` ” 16 festgelegt werden. Unabhängig von der Anzahl von Kernen, über die der Knoten verfügt, können maximal 256 Aufgaben pro Knoten vorliegen. Ausführliche Informationen zur Anzahl der Kerne für jede Knotengröße finden Sie unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md). Weitere Informationen zu den Grenzen des Dienstes finden Sie in [Kontingente und Einschränkungen für den Azure Batch-Dienst](batch-quota-limit.md).

> [!TIP]
> Berücksichtigen Sie unbedingt den Wert `maxTasksPerNode`, wenn Sie für Ihren Pool eine [Formel für das automatische Skalieren][enable_autoscaling] erstellen. Beispielsweise könnte eine Formel zum Auswerten von `$RunningTasks` erheblich von einer Steigerung der Aufgaben pro Knoten betroffen sein. Weitere Informationen finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md) .
>
>

## <a name="distribution-of-tasks"></a>Verteilung von Aufgaben
Wenn die Computeknoten in einem Pool Aufgaben parallel ausführen können, müssen Sie angeben, wie die Aufgaben auf die Knoten innerhalb des Pools verteilt werden sollen.

Mithilfe der [CloudPool.TaskSchedulingPolicy][task_schedule]-Eigenschaft können Sie angeben, dass Aufgaben gleichmäßig über alle Knoten im Pool zugewiesen werden sollen („Verteilen“). Oder Sie können angeben, dass jedem Knoten so viele Aufgaben wie möglich zugewiesen werden sollen, bevor sie einem anderen Knoten im Pool zugewiesen werden („Packen“).

Als Beispiel für den Wert dieser Eigenschaft betrachten Sie den Pool aus [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md)-Knoten (im vorstehenden Beispiel), der mit dem [CloudPool.MaxTasksPerComputeNode][maxtasks_net]-Wert „16“ konfiguriert wurde. Bei Konfiguration von [CloudPool.TaskSchedulingPolicy][task_schedule] mit einem [ComputeNodeFillType][fill_type] der Art *Pack* wird die Auslastung aller 16 Kerne für die einzelnen Knoten maximiert und ein [Pool mit automatischer Skalierung](batch-automatic-scaling.md) ermöglicht, der nicht verwendete Knoten (Knoten, denen keine Aufgaben zugewiesen sind) aus dem Pool löscht. Dies minimiert die Ressourcenverwendung und spart Geld.

## <a name="batch-net-example"></a>Beispiel für Batch .NET
Dieser [Batch .NET][api_net]-API-Codeausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus vier Knoten mit maximal vier Aufgaben pro Knoten. Er gibt eine Richtlinie für die Aufgabenplanung vor, die besagt, dass jeder Knoten mit Aufgaben gefüllt werden soll, bevor diese den anderen Knoten im Pool zugewiesen werden. Weitere Informationen zum Hinzufügen von Pools mit der Batch .NET-API finden Sie unter [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Beispiel für Batch REST
Dieser [Batch-REST][api_rest]-API-Ausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus zwei großen Knoten mit maximal vier Aufgaben pro Knoten. Weitere Informationen zum Hinzufügen von Pools mit der REST-API finden Sie unter [Add a pool to an account][rest_addpool] (Hinzufügen eines Pools zu einem Konto).

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Sie können das Element `maxTasksPerNode` und die [MaxTasksPerComputeNode][maxtasks_net]-Eigenschaft nur zum Zeitpunkt der Poolerstellung festlegen. Nach der Erstellung eines Pools können sie nicht mehr geändert werden.
>
>

## <a name="code-sample"></a>Codebeispiel
Das [ParallelNodeTasks][parallel_tasks_sample]-Projekt auf GitHub veranschaulicht die Verwendung der [CloudPool.MaxTasksPerComputeNode][maxtasks_net]-Eigenschaft.

Diese C#-Konsolenanwendung verwendet die [Batch-Bibliothek für .NET][api_net] zum Erstellen eines Pools mit einem oder mehreren Serverknoten. Sie führt eine konfigurierbare Anzahl an Aufgaben auf diesen Knoten aus, um eine variable Auslastung zu simulieren. Die Ausgabe der Anwendung gibt an, welcher Knoten welche Aufgabe ausgeführt hat. Die Anwendung liefert auch eine Zusammenfassung der Aufgabenparameter und der -dauer. Die Zusammenfassung der Ausgabe von zwei verschiedenen Ausführungen der Beispielanwendung wird unten angezeigt.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Die erste Ausführung der Beispielanwendung zeigt, dass die Aufgabe bei Nutzung eines einzigen Knotens im Pool und einer Aufgabe pro Knoten mehr als 30 Minuten dauert.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Die zweite Ausführung des Beispiels zeigt eine deutliche Verringerung der Aufgabendauer. Das liegt daran, dass der Pool mit vier Aufgaben pro Knoten konfiguriert wurde, was die parallele Aufgabenausführung in etwa einem Viertel der Zeit ermöglicht.

> [!NOTE]
> In der Aufgabedauer in den oben aufgeführten Zusammenfassungen ist die Erstellung des Pools nicht berücksichtigt. Jede der oben aufgeführten Aufgaben wurde an bereits erstellte Pools übermittelt, deren Serverknoten sich zum Übermittlungszeitpunkt im *Idle* -Status befanden.
>
>

## <a name="next-steps"></a>Nächste Schritte
### <a name="batch-explorer-heat-map"></a>Heat Map für Batch Explorer
Der [Batch Explorer][batch_labs] ist ein kostenloses eigenständiges Clienttool mit zahlreichen Features, das Sie beim Erstellen, Debuggen und Überwachen von Azure Batch-Anwendungen unterstützt. Der Batch Explorer enthält ein *Wärmebild*-Feature, das die Visualisierung der Taskausführung ermöglicht. Wenn Sie die Beispielanwendung [ParallelTasks][parallel_tasks_sample] ausführen, können Sie die Heat Map-Funktion für eine einfache Visualisierung der Ausführung paralleler Aufgaben auf jedem Knoten verwenden.


[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

