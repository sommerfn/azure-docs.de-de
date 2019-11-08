---
title: Überwachen von Azure Machine Learning | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Monitor verwenden können, um Metriken aus Azure Machine Learning anzuzeigen und zu erstellen sowie Warnungen zu solchen Metriken zu erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 11/04/2019
ms.openlocfilehash: dddc39a0b7b3c43f283750c95d4feaef2cf15b36
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510727"
---
# <a name="monitoring-azure-machine-learning"></a>Überwachen von Azure Machine Learning

In diesem Artikel werden die von Azure Machine Learning generierten Überwachungsdaten beschrieben. Außerdem wird beschrieben, wie Sie Azure Monitor verwenden können, um Ihre Daten zu analysieren und Warnungen zu definieren.

## <a name="azure-monitor"></a>Azure Monitor

Azure Machine Learning protokolliert Überwachungsdaten mit Azure Monitor, das ein Azure-Dienst zur vollständigen Stapelüberwachung ist. Azure Monitor bietet einen vollständigen Satz von Funktionen zum Überwachen Ihrer Azure-Ressourcen. Mit Azure Monitor können außerdem Ressourcen in anderen Clouds und lokal überwacht werden.

Beginnen Sie mit dem Artikel [Azure Monitor – Übersicht](/azure/azure-monitor/overview), der eine Übersicht über die Überwachungsfunktionen bietet. Die folgenden Abschnitte bauen auf diesen Informationen auf, indem in ihnen Einzelheiten zur Verwendung von Azure Monitor mit Azure Machine Learning beschrieben werden.

Informationen zu den mit Azure Monitor verbundenen Kosten finden Sie unter [Überwachen der Nutzung und geschätzten Kosten in Azure Monitor](/azure/azure-monitor/platform/usage-estimated-costs). Informationen hinsichtlich der Zeit, die benötigt wird, bis Ihre Daten in Azure Monitor angezeigt werden, finden Sie unter [Protokolldatenerfassungszeit in Azure Monitor](/azure/azure-monitor/platform/data-ingestion-time).

## <a name="monitoring-data-from-azure-machine-learning"></a>Überwachen von Daten aus Azure Machine Learning

Azure Machine Learning sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachen von Azure-Ressourcen mit Azure Monitor](/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data-from-Azure-resources) beschrieben sind. Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Machine Learning erstellt werden, finden Sie unter [Überwachen von Azure Machine Learning-Daten – Referenz](monitor-resource-reference.md).

## <a name="analyzing-metric-data"></a>Analysieren von Metrikdaten

Sie können Metriken für Azure Machine Learning analysieren, indem Sie **Metriken** über das **Azure Monitor**-Menü öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](/azure/azure-monitor/platform/metrics-getting-started).

Alle Metriken für Azure Machine Learning befinden sich im Namespace **Machine Learning Service-Arbeitsbereich**.

![Metrik-Explorer mit ausgewähltem Machine Learning Service-Arbeitsbereich](media/monitor-azure-machine-learning/metrics.png)

### <a name="filtering-and-splitting"></a>Filtern und Teilen

Für Metriken, die Dimensionen unterstützen, können Sie Filter mit einem Dimensionswert anwenden. Beispielsweise können Sie **Active Cores** (Aktive Kerne) nach dem **Clusternamen** `cpu-cluster` filtern. 

Sie können eine Metrik auch nach Dimension teilen, um visuell darzustellen, wie verschiedene Segmente der Metrik miteinander zu vergleichen sind. Beispielsweise können Sie den **Pipeline Step Type** (Pipelineschritttyp) teilen, um die Anzahl der in der Pipeline verwendeten Typen von Schritten anzuzeigen.

Weitere Informationen zum Filtern und Teilen finden Sie unter [Erweiterte Funktionen von Azure Metrik-Explorer](/azure/azure-monitor/platform/metrics-charts).

## <a name="alerts"></a>Alerts

Sie können auf Warnungen für Azure Machine Learning zugreifen, indem Sie **Warnungen** über das **Azure Monitor**-Menü öffnen. Ausführliche Informationen zum Erstellen von Warnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](/azure/azure-monitor/platform/alerts-metric).

In der folgenden Tabelle sind allgemeine und empfohlene Metrikwarnungsregeln für Azure Machine Learning aufgeführt:

| Warnungstyp | Bedingung | BESCHREIBUNG |
|:---|:---|:---|
| Model Deploy Failed (Fehler bei der Modellimplementierung) | Aggregationstyp: Total (Gesamt), Operator: Größer als, Schwellenwert: 0 | Mindestens eine Modellimplementierung ist fehlgeschlagen. |
| Quota Utilization Percentage (Prozentsatz der Kontingentnutzung) | Aggregationstyp: Average (Mittelwert), Operator: Größer als, Schwellenwert: 90| Trifft zu, wenn die Kontingentnutzung größer als 90 % ist. |
| Unusable Nodes (Nicht verwendbare Knoten) | Aggregationstyp: Total (Gesamt), Operator: Größer als, Schwellenwert: 0 | Es gibt mindestens einen nicht verwendbaren Knoten. |

## <a name="configuration"></a>Konfiguration

> [!IMPORTANT]
> __Metriken für Azure Machine Learning müssen nicht konfiguriert werden__. Sie werden automatisch gesammelt und sind im Metrik-Explorer zur Überwachung und Warnungserstellung verfügbar.

Sie können eine Diagnoseeinstellung hinzufügen, um die folgenden Funktionalität zu konfigurieren:

* Archivieren von Protokoll-und Metrikinformationen in einem Azure-Speicherkonto
* Streamen von Protokoll-und Metrikinformationen an einen Azure Event Hub
* Senden von Protokoll-und Metrikinformationen an Azure Monitor Log Analytics

Ein Aktivieren dieser Einstellungen erfordert zusätzliche Azure-Dienste (Speicherkonto, Event Hub oder Log Analytics). Dadurch können sich Ihre Kosten erhöhen. Um geschätzte Kosten zu berechnen, wechseln Sie zum [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator).

Weitere Informationen zum Erstellen einer Diagnoseeinstellung finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](/azure/azure-monitor/platform/diagnostic-settings).

Sie können die folgenden Protokolle für Azure Machine Learning konfigurieren:

| Category | BESCHREIBUNG |
|:---|:---|
| AmlComputeClusterEvent | Ereignisse von Azure Machine Learning-Computeclustern |
| AmlComputeClusterNodeEvent | Ereignisse von Knoten in einem Azure Machine Learning-Computecluster |
| AmlComputeJobEvent | Ereignisse von Knoten, die in Azure Machine Learning-Compute ausgeführt werden |

> [!NOTE]
> Wenn Sie Metriken in einer Diagnoseeinstellung aktivieren, sind Dimensionsinformationen derzeit nicht in den Informationen enthalten, die an ein Speicherkonto, an einen Event Hub oder an Log Analytics gesendet werden.

## <a name="analyzing-log-data"></a>Analysieren von Protokolldaten

Um Azure Monitor Log Analytics verwenden zu können, müssen Sie eine Diagnosekonfiguration erstellen und __Send information to Log Analytics__ (Informationen an Log Analytics senden) aktivieren. Weitere Informationen finden Sie im Abschnitt [Konfiguration](#configuration).

Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat. In Azure Machine Learning werden Daten in den folgenden Tabellen gespeichert:

| Table | BESCHREIBUNG |
|:---|:---|
| AmlComputeClusterEvent | Ereignisse von Azure Machine Learning-Computeclustern |
| AmlComputeClusterNodeEvent | Ereignisse von Knoten in einem Azure Machine Learning-Computecluster |
| AmlComputeJobEvent | Ereignisse von Knoten, die in Azure Machine Learning-Compute ausgeführt werden |

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Machine Learning auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf den aktuellen Arbeitsbereich festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Datenbanken oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](/azure/azure-monitor/log-query/scope/).

Eine ausführliche Referenz zu den Protokollen und Metriken finden Sie unter [Überwachen von Azure Machine Learning-Daten – Referenz](monitor-resource-reference.md).

### <a name="sample-queries"></a>Beispielabfragen

Die folgenden Abfragen sind Abfragen, mit denen Sie Ihre Azure Machine Learning-Ressourcen überwachen können: 

+ Abrufen der Aufträge, die in den letzten fünf Tagen fehlerhaft waren:

    ```Kusto
    AmlComputeJobEvent
    | where TimeGenerated > ago(5d) and EventType == "JobFailed"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Abrufen der Datensätze für einen bestimmten Auftragsnamen:

    ```Kusto
    AmlComputeJobEvent
    | where JobName == "automl_a9940991-dedb-4262-9763-2fd08b79d8fb_setup"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Abrufen von Clusterereignissen, die in den letzten fünf Tagen für Cluster aufgetreten sind, in denen die VM-Größe gleich „Standard_D1_V2“ ist:

    ```Kusto
    AmlComputeClusterEvent
    | where TimeGenerated > ago(4d) and VmSize == "STANDARD_D1_V2"
    | project  ClusterName , InitialNodeCount , MaximumNodeCount , QuotaAllocated , QuotaUtilized
    ```

+ Abrufen der Knoten, die in den letzten acht Tagen zugeordnet wurden:

    ```Kusto
    AmlComputeClusterNodeEvent
    | where TimeGenerated > ago(8d) and NodeAllocationTime  > ago(8d)
    | distinct NodeId
    ```

## <a name="next-steps"></a>Nächste Schritte

- Eine Referenz zu den Protokollen und Metriken finden Sie unter [Überwachen von Azure Machine Learning-Daten – Referenz](monitor-resource-reference.md).
- Informationen zum Arbeiten mit Kontingenten im Zusammenhang mit Azure Machine Learning finden Sie unter [Verwalten und Anfordern von Kontingenten für Azure-Ressourcen](how-to-manage-quotas.md).
- Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](/azure/azure-monitor/insights/monitor-azure-resource).
