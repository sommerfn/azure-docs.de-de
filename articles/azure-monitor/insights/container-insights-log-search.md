---
title: Abfragen von Protokollen aus Azure Monitor für Container | Microsoft-Dokumentation
description: Azure Monitor für Container erfasst Metriken und Protokolldaten. In diesem Artikel werden die Datensätze und einige Beispielabfragen beschrieben.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: ae8dd4cccb6795faa02e6705404644f6ccc24864
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71948046"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Abfragen von Protokollen aus Azure Monitor für Container

Azure Monitor für Container erfasst Leistungsmetriken, Inventurdaten und Informationen zum Integritätsstatus von Containerhosts und Containern und leitet diese an den Log Analytics-Arbeitsbereich in Azure Monitor weiter. Alle drei Minuten werden Daten gesammelt. Diese Daten stehen in Azure Monitor für [Abfragen](../../azure-monitor/log-query/log-query-overview.md) zur Verfügung. Diese Daten können in verschiedenen Szenarios von Nutzen sein, z.B. bei der Migrationsplanung, Kapazitätsanalyse, Ermittlung und Ad-hoc-Behebung von Leistungsproblemen.

## <a name="container-records"></a>Containerdatensätze

Die folgende Tabelle zeigt Beispiele für Datensätze, die von Azure Monitor für Container gesammelt werden, und Datentypen, die in Protokollsuchergebnissen angezeigt werden:

| Datentyp | Datentyp in der Protokollsuche | Felder |
| --- | --- | --- |
| Leistung von Hosts und Containern | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Containerinhalt | `ContainerInventory` | TimeGenerated, Computer, Containername, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Containerprotokoll | `ContainerLog` | TimeGenerated, Computer, Image-ID, Containername, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Containerknotenbestand | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Bestand der Pods in einem Kubernetes-Cluster | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Bestand der Knoten als Teil eines Kubernetes-Clusters | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes-Ereignisse | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message,  SourceSystem | 
| Dienste im Kubernetes-Cluster | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Knoten | Perf &#124; where ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Leistungsmetriken für die zum Kubernetes-Cluster zugehörigen Container | Perf &#124; where ObjectName == “K8SContainer” | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Benutzerdefinierte Metriken |`InsightsMetrics` | Computer, Name, Namespace, Origin, SourceSystem, Tags<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> Die Eigenschaft *Tags* repräsentiert [mehrere Dimensionen](../platform/data-platform-metrics.md#multi-dimensional-metrics) für die entsprechende Metrik. Zusätzliche Informationen zu den erfassten und in der Tabelle `InsightsMetrics` gespeicherten Metriken sowie eine Beschreibung der Datensatzeigenschaften finden Sie unter [InsightsMetrics-Übersicht](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md).

>[!NOTE]
>Die Unterstützung für Prometheus ist zurzeit als Feature in der öffentlichen Vorschau verfügbar.
>

## <a name="search-logs-to-analyze-data"></a>Suchen von Protokollen zur Datenanalyse

Mit Azure Monitor-Protokollen können Sie nach Trends suchen, Engpässe diagnostizieren, Prognosen erstellen oder Daten korrelieren, die Ihnen die Einschätzung ermöglichen, ob die aktuelle Clusterkonfiguration optimal funktioniert. Ihnen werden vordefinierte Protokollsuchen für die sofortige Verwendung bereitgestellt. Alternativ können Sie diese so anpassen, dass die Informationen auf die gewünschte Weise zurückgegeben werden.

Im Arbeitsbereich können Sie interaktive Datenanalysen durchführen, indem Sie im Vorschaubereich die Option **Kubernetes-Ereignisprotokolle anzeigen** oder **Containerprotokolle anzeigen** auswählen. Die Seite **Protokollsuche** wird rechts von der Azure-Portalseite angezeigt, die Sie besucht haben.

![Analysieren von Daten in Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)   

Die an Ihren Arbeitsbereich weitergeleiteten Containerprotokollausgaben sind STDOUT und STDERR. Da Azure Monitor AKS (Azure Kubernetes Service) überwacht, wird „kube-system“ aufgrund des großen Umfangs der generierten Daten zurzeit nicht gesammelt. 

### <a name="example-log-search-queries"></a>Beispielabfragen für die Protokollsuche

Es ist oft hilfreich, die Abfrageerstellung ausgehend von einem oder zwei Beispielen zu beginnen und diese dann den Anforderungen entsprechend anzupassen. Sie können mit den folgenden Beispielabfragen experimentieren, um komplexere Abfragen zu erstellen:

| Abfragen | BESCHREIBUNG | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Liste mit Lebenszyklusinformationen aller Container| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Kubernetes-Ereignisse|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Imagebestand | 
| **Wählen Sie die Anzeigeoption Liniendiagramm aus**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" &#124; summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Container-CPU | 
| **Wählen Sie die Anzeigeoption Liniendiagramm aus**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes" &#124; summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Containerspeicher |
| InsightsMetrics<br> &#124; where Name == "requests_count"<br> &#124; summarize Val=any(Val) by TimeGenerated=bin(TimeGenerated, 1m)<br> &#124; sort by TimeGenerated asc<br> &#124; project RequestsPerMinute = Val - prev(Val), TimeGenerated <br> &#124; render barchart  | Anforderungen pro Minute mit benutzerdefinierten Metriken |

Das folgende Beispiel zeigt eine Prometheus-Metrikabfrage. Die erfassten Metriken sind Zählungen. Um zu bestimmen, wie viele Fehler innerhalb eines bestimmten Zeitraums aufgetreten sind, ist eine Subtraktion von der Anzahl erforderlich. Das Dataset wird nach *partitionKey* partitioniert, d.h. für jeden eindeutigen Satz aus *Name*, *HostName* und *OperationType* wird eine Unterabfrage für diesen Satz ausgeführt, der die Protokolle nach *TimeGenerated* sortiert. Dieser Prozess ermöglicht es, den vorherigen *TimeGenerated*-Wert und die aufgezeichnete Anzahl für diesen Zeitpunkt zu ermitteln, um ein Verhältnis zu bestimmen.

```
let data = InsightsMetrics 
| where Namespace contains 'prometheus' 
| where Name == 'kubelet_docker_operations' or Name == 'kubelet_docker_operations_errors'    
| extend Tags = todynamic(Tags) 
| extend OperationType = tostring(Tags['operation_type']), HostName = tostring(Tags.hostName) 
| extend partitionKey = strcat(HostName, '/' , Name, '/', OperationType) 
| partition by partitionKey ( 
    order by TimeGenerated asc 
    | extend PrevVal = prev(Val, 1), PrevTimeGenerated = prev(TimeGenerated, 1) 
    | extend Rate = iif(TimeGenerated == PrevTimeGenerated, 0.0, Val - PrevVal) 
    | where isnull(Rate) == false 
) 
| project TimeGenerated, Name, HostName, OperationType, Rate; 
let operationData = data 
| where Name == 'kubelet_docker_operations' 
| project-rename OperationCount = Rate; 
let errorData = data 
| where Name == 'kubelet_docker_operations_errors' 
| project-rename ErrorCount = Rate; 
operationData 
| join kind = inner ( errorData ) on TimeGenerated, HostName, OperationType 
| project-away TimeGenerated1, Name1, HostName1, OperationType1 
| extend SuccessPercentage = iif(OperationCount == 0, 1.0, 1 - (ErrorCount / OperationCount))
```

Die Ausgabe enthält ähnliche Ergebnisse wie die folgenden:

![Protokollabfrageergebnisse zur Menge der erfassten Daten](./media/container-insights-log-search/log-query-example-prometheus-metrics.png)

## <a name="next-steps"></a>Nächste Schritte

Azure Monitor für Container umfasst keinen vordefinierten Satz von Warnungen. Informationen zum Erstellen von empfohlenen Warnungen für hohe CPU- und Arbeitsspeicherauslastung zur Unterstützung Ihrer DevOps oder Betriebsprozesse und -verfahren finden Sie unter [Erstellen von Leistungswarnungen mit Azure Monitor für Container](container-insights-alerts.md). 
