---
title: Struktur von Azure Monitor-Protokollen | Microsoft-Dokumentation
description: Sie benötigen eine Protokollabfrage, um Protokolldaten aus Azure Monitor abzurufen.  In diesem Artikel wird beschrieben, wie neue Protokollabfragen in Azure Monitor verwendet werden. Er enthält Konzepte, die Ihnen vor dem Erstellen bekannt sein sollten.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/22/2019
ms.openlocfilehash: 091d7f598a9841ae45b4248ad8a07a355203445a
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72894246"
---
# <a name="structure-of-azure-monitor-logs"></a>Struktur von Azure Monitor-Protokollen
Die Möglichkeit, mit einer [Protokollabfrage](log-query-overview.md) schnell Einblicke in Ihre Daten zu gewinnen, ist ein leistungsstarkes Feature von Azure Monitor. Sie sollten mit den grundlegenden Konzepten vertraut sein, um effiziente und nützliche Abfragen zu erstellen. Dazu gehört beispielsweise, dass Sie wissen, wo sich die gewünschten Daten befinden und wie diese strukturiert sind. Dieser Artikel enthält die grundlegenden Konzepte, die Sie für den Einstieg benötigen.

## <a name="overview"></a>Übersicht
Daten in Azure Monitor-Protokollen werden entweder in einem Log Analytics-Arbeitsbereich oder einer Application Insights-Anwendung gespeichert. Beide basieren auf [Azure Data Explorer](/azure/data-explorer/) und verwenden daher dessen leistungsstarke Daten-Engine und Abfragesprache.

Daten in Arbeitsbereichen und Anwendungen sind jeweils in Tabellen organisiert, von denen jede unterschiedliche Arten von Daten speichert und über mehrere eindeutige Eigenschaften verfügt. Die meisten [Datenquellen](../platform/data-sources.md) schreiben in ihre eigenen Tabellen in einem Log Analytics-Arbeitsbereich, während Application Insights in eine Reihe von vordefinierten Tabellen in einer Application Insights-Anwendung schreibt. Protokollabfragen sind sehr flexibel und ermöglichen Ihnen das einfache Kombinieren von Daten aus mehreren Tabellen, das Verwenden einer ressourcenübergreifenden Abfrage zum Kombinieren von Daten aus Tabellen in mehreren Arbeitsbereichen und das Schreiben von Abfragen, die Arbeitsbereiche und Anwendungsdaten kombinieren.

Die folgende Abbildung zeigt Beispiele für Datenquellen, die in verschiedene Tabellen schreiben, die in den Beispielabfragen verwendet wurden.

![Tabellen](media/logs-structure/queries-tables.png)

## <a name="log-analytics-workspace"></a>Log Analytics-Arbeitsbereich
Alle von Azure Monitor-Protokollen erfasste Daten (außer Application Insights) werden in einem [Log Analytics-Arbeitsbereich](../platform/manage-access.md) gespeichert. Sie können je nach Ihren Anforderungen einen oder mehrere Arbeitsbereiche erstellen. [Datenquellen](../platform/data-sources.md) wie Aktivitäts- und Diagnoseprotokolle aus Azure-Ressourcen, Agents auf virtuellen Computern und Daten aus Insights- und Überwachungslösungen schreiben Daten in einen oder mehrere Arbeitsbereiche, die Sie als Teil des Onboardings konfigurieren. Andere Dienste wie [Azure Security Center](/azure/security-center/) und [Azure Sentinel](/azure/sentinel/) verwenden ebenfalls einen Log Analytics-Arbeitsbereich zum Speichern von Daten, sodass dieser mithilfe von Protokollabfragen und Überwachungsdaten aus anderen Quellen analysiert werden kann.

Verschiedene Arten von Daten werden in unterschiedlichen Tabellen im Arbeitsbereich gespeichert, und jede Tabelle verfügt über mehrere eindeutige Eigenschaften. Beim Erstellen eines Arbeitsbereichs werden diesem mehrere Standardtabellen sowie neue Tabellen für verschiedene Datenquellen, Lösungen und Dienste hinzugefügt, da diese integriert sind. Sie können mit der [Datensammler-API](../platform/data-collector-api.md) auch benutzerdefinierte Tabellen erstellen.

Sie können die Tabellen in einem Arbeitsbereich sowie deren Schema auf der Registerkarte **Schema** in Log Analytics für den Arbeitsbereich durchsuchen.

![Arbeitsbereichsschema](media/scope/workspace-schema.png)

Verwenden Sie die folgende Abfrage zum Auflisten der Tabellen im Arbeitsbereich und der Anzahl der am vorherigen Tag darin gesammelten Datensätze. 

```Kusto
union withsource = table * 
| where TimeGenerated > ago(1d)
| summarize count() by table
| sort by table asc
```
Weitere Details zu den Tabellen, die erstellt werden, finden Sie in der Dokumentation für jede Datenquelle. Die Beispiele enthalten Artikel zu [ Agent-Datenquellen](../platform/agent-data-sources.md), [Diagnoseprotokollen](../platform/diagnostic-logs-schema.md) und [Überwachungslösungen](../insights/solutions-inventory.md).

### <a name="workspace-permissions"></a>Arbeitsbereichberechtigungen
Weitere Informationen zur Zugriffssteuerungsstrategie und Empfehlungen für das Gewähren von Zugriff auf Daten in einem Arbeitsbereich finden Sie unter [Entwerfen einer Azure Monitor-Protokollbereitstellung](../platform/design-logs-deployment.md). Zusätzlich zum Erteilen des Zugriffs auf den Arbeitsbereich selbst können Sie mithilfe von [RBAC auf Tabellenebene](../platform/manage-access.md#table-level-rbac) den Zugriff auf einzelne Tabellen beschränken.

## <a name="application-insights-application"></a>Application Insights-Anwendung
Wenn Sie eine Anwendung in Application Insights erstellen, wird in Azure Monitor-Protokollen automatisch eine entsprechende Anwendung erstellt. Für das Sammeln von Daten ist keine Konfiguration erforderlich, und die Anwendung schreibt automatisch Überwachungsdaten wie Seitenansichten, Anforderungen und Ausnahmen.

Im Gegensatz zu einem Log Analytics-Arbeitsbereich verwendet eine Application Insights-Anwendung eine festgelegte Gruppe von Tabellen. Sie können andere Datenquellen nicht so konfigurieren, dass diese in die Anwendung schreiben, damit keine weiteren Tabellen erstellt werden können. 

| Table | BESCHREIBUNG | 
|:---|:---|
| availabilityResults | Zusammenfassungsdaten aus Verfügbarkeitstests. |
| browserTimings      | Daten über die Leistung des Clients, z. B. die angefallene Zeit zum Verarbeiten der eingehenden Daten. |
| customEvents        | Benutzerdefinierte Ereignisse, die von Ihrer Anwendung erstellt werden. |
| customMetrics       | Benutzerdefinierte Metriken, die von Ihrer Anwendung erstellt werden. |
| dependencies        | Aufrufe der Anwendung von externen Komponenten. |
| Ausnahmen          | Ausnahmen, die von der Anwendungsruntime ausgelöst werden. |
| pageViews           | Daten über jede Websiteansicht mit Browserinformationen. |
| performanceCounters | Leistungsmessungen der Computeressourcen zur Unterstützung der Anwendung. |
| requests            | Details zu jeder Anwendungsanforderung.  |
| traces              | Ergebnisse der verteilten Ablaufverfolgung. |

Sie können das Schema für jede Tabelle auf der Registerkarte **Schema** in Log Analytics für die Anwendung anzeigen lassen.

![Anwendungsschema](media/scope/application-schema.png)

## <a name="standard-properties"></a>Standardeigenschaften
Während jede Tabelle in Azure Monitor-Protokollen über ein eigenes Schema verfügt, gibt es Standardeigenschaften, die von allen Tabellen verwendet werden. Details zu den Standardeigenschaften finden Sie unter [Standard properties in Azure Monitor Logs (Standardeigenschaften in Azure Monitor-Protokollen)](../platform/log-standard-properties.md).

| Log Analytics-Arbeitsbereich | Application Insights-Anwendung | BESCHREIBUNG |
|:---|:---|:---|
| TimeGenerated | timestamp  | Datum und Uhrzeit der Erstellung des Datensatzes. |
| type          | itemType   | Name der Tabelle, aus der der Datensatz abgerufen wurde. |
| _ResourceId   |            | Eindeutiger Bezeichner für die Ressource, der der Datensatz zugeordnet ist. |
| _IsBillable   |            | Gibt an, ob erfasste Daten gebührenpflichtig sind. |
| _BilledSize   |            | Gibt die Größe der in Rechnung zu stellenden Daten in Bytes an. |

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über die Verwendung von [Log Analytics zum Erstellen und Bearbeiten von Protokollsuchen](../log-query/portals.md).
- Sehen Sie sich ein [Tutorial zum Schreiben von Abfragen](../log-query/get-started-queries.md) mit der neuen Abfragesprache an.
