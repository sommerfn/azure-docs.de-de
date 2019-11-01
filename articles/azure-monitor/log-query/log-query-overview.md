---
title: Übersicht über Protokollabfragen in Azure Monitor | Microsoft-Dokumentation
description: Antworten auf häufig zu Protokollabfragen gestellte Fragen und erste Schritte zur Verwendung dieser Abfragen
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/19/2019
ms.openlocfilehash: 08af0ff3121d6721c9f5c1457655c345f50f91e0
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900268"
---
# <a name="overview-of-log-queries-in-azure-monitor"></a>Übersicht über Protokollabfragen in Azure Monitor
Mithilfe von Protokollabfragen können Sie die Daten, die in [Azure Monitor-Protokollen](../platform/data-platform-logs.md) erfasst werden, in vollem Umfang nutzen. Eine leistungsstarke Abfragesprache ermöglicht es Ihnen, Daten aus mehreren Tabellen zusammenzufügen, größere Mengen an Daten zu aggregieren und komplexe Vorgänge mit möglichst wenig Code auszuführen. Beinahe jede Frage kann beantwortet und jede Analyse durchgeführt werden, solange unterstützende Daten erfasst wurden und Sie wissen, wie die richtige Abfrage erstellt werden muss.

Einige Features in Azure Monitor (z. B. [Einblicke](../insights/insights-overview.md) und [Lösungen](../insights/solutions-inventory.md)) verarbeiten Protokolldaten, ohne die zugrunde liegenden Abfragen für Sie offenzulegen. Sie sollten wissen, wie Abfragen aufgebaut sind und wie Sie sie verwenden können, um Daten auf interaktive Weise in Azure Monitor-Protokollen analysieren zu können, damit Sie auch die anderen Features von Azure Monitor in vollem Umfang nutzen können.

Verwenden Sie diesen Artikel als Ausgangspunkt, um sich über Protokollabfragen in Azure Monitor zu informieren. Im Laufe des Artikels werden häufig gestellte Fragen beantwortet sowie Links zu anderen Dokumentationen angegeben, die weitere Informationen sowie Lerneinheiten umfassen.

## <a name="how-can-i-learn-how-to-write-queries"></a>Wie erfahre ich, wie ich Abfragen schreiben kann?
Wenn Sie direkt loslegen möchten, können Sie mit den folgenden Tutorials beginnen:

- [Get started with Log Analytics in Azure Monitor (Erste Schritte mit Log Analytics in Azure Monitor)](get-started-portal.md)
- [Get started with log queries in Azure Monitor (Erste Schritte mit Abfragen in Azure Monitor)](get-started-queries.md)

Sobald Sie die Grundlagen kennen, können Sie einige Lerneinheiten durchgehen, für die Sie entweder Ihre eigenen Daten oder Daten aus unserer Demoumgebung verwenden. Beginnen Sie mit der folgenden Einheit: 

- [Arbeiten mit Zeichenfolgen in Azure Monitor-Protokollabfragen](string-operations.md)
 
## <a name="what-language-do-log-queries-use"></a>Welche Sprache verwenden Protokollabfragen?
Azure Monitor-Protokolle basieren auf [Azure Data Explorer](/azure/data-explorer), und Protokollabfragen werden in der Abfragesprache Kusto (Kusto Query Language, KQL) geschrieben. Es handelt sich dabei um eine umfangreiche Sprache, die einfach zu lesen und zu verwenden ist. Sie sollten sie nach kurzer Einarbeitung verwenden können.

Die vollständige Dokumentation zur Verwendung von KQL mit Azure Data Explorer und Verweise auf die verschiedenen verfügbaren Funktionen [finden Sie hier](/azure/kusto/query).<br>
Eine kurze exemplarische Vorgehensweise für die Sprache bei der Verwendung von Daten aus Azure Monitor-Protokollen finden Sie unter [Erste Schritte mit Azure Monitor-Protokollabfragen](get-started-queries.md).
Informationen zu kleineren Unterschieden bei der KQL-Version, die von Azure Monitor verwendet wird, finden Sie unter [Azure Monitor – Unterschiede in der Protokollabfragesprache](data-explorer-difference.md).

## <a name="what-data-is-available-to-log-queries"></a>Welchen Daten stehen für Protokollabfragen zur Verfügung?
Sämtliche Daten, die in Azure Monitor-Protokollen erfasst werden, können bei Protokollabfragen abgerufen und analysiert werden. Verschiedene Datenquellen schreiben ihre Daten auch in unterschiedliche Tabellen. Sie können aber auch mehrere Tabellen in einer einzelnen Abfrage zusammenfassen, um Daten über mehrere Quellen hinweg zu analysieren. Wenn Sie eine Abfrage erstellen möchten, beginnen Sie damit zu ermitteln, welche Tabellen die Daten enthalten, die Sie suchen. Dafür benötigen Sie zumindest grundlegende Kenntnisse darüber, wie Daten in Azure Monitor-Protokollen strukturiert sind.

Eine Liste der verschiedenen Datenquellen, die Azure Monitor-Protokolle auffüllen, finden Sie unter [Quellen von Azure Monitor-Protokollen](../platform/data-platform-logs.md#sources-of-azure-monitor-logs).<br>
Eine Erklärung zur Strukturierung der Daten finden Sie unter [Structure of Azure Monitor Logs (Struktur von Azure Monitor-Protokollen)](logs-structure.md).

## <a name="what-does-a-log-query-look-like"></a>Wie sieht eine Protokollabfrage aus?
Eine Abfrage kann beispielsweise aus einem einfachen Tabellennamen bestehen, über den alle Einträge einer Tabelle abgerufen werden können:

```Kusto
Syslog
```

Alternativ kann auch nach bestimmten Einträgen gefiltert werden, die anschließend zusammengefasst und die Ergebnisse in einem Diagramm dargestellt werden:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| where EventID == 4625
| summarize count() by Computer, bin(TimeGenerated, 1h)
| render timechart 
```

Bei komplexeren Analysen können Sie Daten aus mehreren Tabellen abrufen und eine Verknüpfung verwenden, um die Ergebnisse zusammen zu analysieren.

```Kusto
app("ContosoRetailWeb").requests
| summarize count() by bin(timestamp,1hr)
| join kind= inner (Perf
    | summarize avg(CounterValue) 
      by bin(TimeGenerated,1hr))
on $left.timestamp == $right.TimeGenerated
```
Auch wenn Sie nicht mit KQL vertraut sind, sollten Sie zumindest die zugrunde liegende Logik ermitteln können, die von diesen Abfragen verwendet wird. Diese Abfragen beginnen mit dem Namen einer Tabelle und fügen anschließend mehrere Befehle hinzu, um die Daten zu filtern und zu verarbeiten. Eine Abfrage kann beliebig viele Befehle verwenden, und Sie können komplexere Abfragen schreiben, wenn Sie erst einmal mit den verschiedenen verfügbaren KQL-Befehlen vertraut sind.

Ein Tutorial zu Protokollabfragen, in dem die Sprache sowie allgemeine Funktionen erläutert werden, finden Sie unter [Erste Schritte mit Azure Monitor-Protokollabfragen](get-started-queries.md).<br>


## <a name="what-is-log-analytics"></a>Was ist Log Analytics?
Log Analytics ist das wichtigste Tool, das im Azure-Portal zum Schreiben von Protokollabfragen und interaktiven Analysieren von deren Ergebnissen verwendet wird. Auch wenn eine Protokollabfrage an einer anderen Stelle in Azure Monitor verwendet wird, sollten Sie die Abfrage in der Regel zunächst mithilfe von Log Analytics schreiben und testen.

Sie können Log Analytics über verschiedene Orte im Azure-Portal starten. Der Umfang der Daten, die für Log Analytics verfügbar sind, ist von der Art und Weise abhängig, wie Sie das Tool starten. Weitere Informationen finden Sie unter [Query Scope (Abfrageumfang)](scope.md).

- Klicken Sie im Menü **Azure Monitor** oder im Menü **Log Analytics-Arbeitsbereiche** auf **Protokolle**.
- Klicken Sie in einer Application Insights-Anwendung auf der Seite **Übersicht** auf **Analytics**.
- Klicken Sie im Menü einer Azure-Ressource auf **Protokolle**.

![Log Analytics](media/log-query-overview/log-analytics.png)

Ein Tutorial zu Log Analytics, in dem verschiedene Features vorgestellt werden, finden Sie unter [Get started with Log Analytics in Azure Monitor (Erste Schritte mit Log Analytics in Azure Monitor)](get-started-portal.md).

## <a name="where-else-are-log-queries-used"></a>Wofür werden Protokollabfragen außerdem noch verwendet?
Neben dem interaktiven Arbeiten mit Protokollabfragen und deren Ergebnissen in Log Analytics können Sie außerdem in den folgenden Bereichen in Azure Monitor Abfragen verwenden:

- **Warnungsregeln.** [Warnungsregeln](../platform/alerts-overview.md) identifizieren proaktiv Probleme durch die Daten in Ihrem Arbeitsbereich.  Jede Warnungsregel basiert auf einer Protokollsuche, die in regelmäßigen Abständen automatisch ausgeführt wird.  Die Ergebnisse werden überprüft, um zu ermitteln, ob eine Warnung erstellt werden soll.
- **Dashboards.** Sie können die Ergebnisse beliebiger Abfragen in einem [Azure-Dashboard](../learn/tutorial-logs-dashboards.md) anheften, was es Ihnen ermöglicht, Protokoll- und Metrikdaten gemeinsam zu visualisieren und optional mit anderen Azure-Benutzern zu teilen.
- **Ansichten.**  Sie können mit [Ansicht-Designer](../platform/view-designer.md) Visualisierungen von Daten erstellen, die in Benutzerdashboards einbezogen werden sollen.  Protokollabfragen stellen die von [Kacheln](../platform/view-designer-tiles.md) und [Visualisierungsparts](../platform/view-designer-parts.md) in jeder Ansicht verwendeten Daten bereit.  
- **Export:**  Wenn Sie Protokolldaten aus Azure Monitor nach Excel oder [Power BI](../platform/powerbi.md) importieren, erstellen Sie eine Protokollabfrage, um die zu exportierenden Daten zu definieren.
- **PowerShell.** Sie können ein PowerShell-Skript über eine Befehlszeile oder ein Azure Automation-Runbook ausführen, das Protokolldaten mithilfe von [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) aus Azure Monitor abruft.  Dieses Cmdlet erfordert eine Abfrage, um die abzurufenden Daten festzulegen.
- **Azure Monitor-Protokolle-API.**  Die [Azure Monitor-Protokolle-API](https://dev.loganalytics.io) ermöglicht einem beliebigen REST-API-Client, Protokolldaten aus dem Arbeitsbereich abzurufen.  Die API-Anforderung enthält eine Abfrage, die für Azure Monitor ausgeführt wird, um die abzurufenden Daten zu ermitteln.


## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen erhalten Sie im [Tutorial zum Verwenden von Log Analytics im Azure-Portal](get-started-portal.md).
- Weitere Informationen erhalten Sie im [Tutorial zum Schreiben von Abfragen](get-started-queries.md).
