---
title: Überwachen von Azure-Ressourcen mit Azure Monitor | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Überwachungsdaten von Ressourcen in Azure mithilfe von Azure Monitor sammeln und analysieren.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019
ms.openlocfilehash: 8ba50e5b55e31c6542e81879068bf231918ce1b0
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72245895"
---
# <a name="monitoring-azure-resources-with-azure-monitor"></a>Überwachen von Azure-Ressourcen mit Azure Monitor
Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure-Ressourcen generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor nutzen können, um diese Daten zu analysieren und Warnungen dafür zu erstellen.

> [!IMPORTANT]
> Dieser Artikel gilt für alle Dienste in Azure, die Azure Monitor verwenden. Computeressourcen, einschließlich virtueller Computer und App Service, generieren die gleichen Überwachungsdaten, die hier beschrieben werden. Sie verfügen aber auch über ein Gastbetriebssystem, das ebenfalls Protokolle und Metriken generieren kann. Einzelheiten zum Erfassen und Analysieren dieser Daten finden Sie in der Dokumentation zur Überwachung dieser Dienste.

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?
Azure Monitor ist ein vollständiger Überwachungsdienst in Azure, der sämtliche Funktionen für das Überwachen Ihrer Azure-Ressourcen sowie der Ressourcen in anderen Clouds und lokaler Ressourcen bereitstellt. Die [Azure Monitor-Datenplattform](../platform/data-platform.md) sammelt Daten in [Protokollen](../platform/data-platform-logs.md) und [Metriken](../platform/data-platform-metrics.md), wo sie mit einem kompletten Satz von Überwachungstools analysiert werden können, die in den folgenden Abschnitten beschrieben werden.

- [Welche Möglichkeiten haben Sie mit Azure Monitor-Metriken?](../platform/data-platform-metrics.md#what-can-you-do-with-azure-monitor-metrics)
- [Welche Möglichkeiten bieten Azure Monitor-Protokolle?](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs)

Sobald Sie eine Azure-Ressource erstellen, wird Azure Monitor aktiviert und beginnt mit dem Sammeln von Metriken und Aktivitätsprotokollen, die Sie im [Azure-Portal anzeigen und analysieren](#monitoring-in-the-azure-portal) können. Bei einigen Konfigurationen können Sie zusätzliche Überwachungsdaten sammeln und weitere Funktionen aktivieren. Weitere Informationen zu den Konfigurationsanforderungen finden Sie unter [Überwachungsdaten](#monitoring-data) weiter unten.


## <a name="costs-associated-with-monitoring"></a>Kosten für die Überwachung
Das Analysieren der Überwachungsdaten, die standardmäßig erfasst werden, ist kostenlos. Dies umfasst Folgendes:

- Sammeln von Plattformmetriken und deren Analyse mit dem Metrik-Explorer
- Sammeln von Aktivitätsprotokollen und deren Analyse im Azure-Portal
- Erstellen einer Aktivitätsprotokollwarnung

Es fallen keine Kosten für Azure Monitor für das Sammeln und Exportieren von Protokollen und Metriken an, möglicherweise fallen jedoch Kosten für das Ziel an:

- Kosten für die Datenerfassung und -aufbewahrung beim Sammeln von Protokollen und Metriken in einem Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/).
- Kosten für die Datenspeicherung beim Erfassen von Protokollen und Metriken in einem Azure Storage-Konto. Weitere Informationen finden Sie unter [Preise für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/).
- Kosten für das Event Hub-Streaming beim Weiterleiten von Protokollen und Metriken an Azure Event Hubs. Weitere Informationen finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

Eventuelle Kosten für Azure Monitor für Folgendes. Weitere Informationen finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/):

- Ausführen einer Protokollabfrage
- Erstellen einer Warnungsregel zu einer Metrik- oder Protokollabfrage
- Senden einer Benachrichtigung für eine beliebige Warnungsregel
- Zugreifen auf Metriken über die API

## <a name="monitoring-data"></a>Überwachungsdaten
Ressourcen in Azure generieren [Protokolle](../platform/data-platform-logs.md) und [Metriken](../platform/data-platform-metrics.md), wie im folgenden Diagramm dargestellt. Informieren Sie sich in der Dokumentation zum jeweiligen Azure-Dienst über die spezifischen generierten Daten und die damit bereitgestellten Lösungen oder Erkenntnisse.

![Übersicht](media/monitor-azure-resource/logs-metrics.png)



- [Plattformmetriken](../platform/data-platform-metrics.md) sind numerische Werte, die automatisch in regelmäßigen Abständen erfasst werden und einen Aspekt einer Ressource zu einem bestimmten Zeitpunkt beschreiben. 
- [Ressourcenprotokolle](../platform/resource-logs-overview.md) bieten einen Einblick in Vorgänge, die innerhalb einer Azure-Ressource (der Datenebene) ausgeführt wurden, z. B. das Abrufen eines Geheimnisses aus einem Schlüsseltresor oder die Ausgabe einer Anforderung an eine Datenbank. Der Inhalt und die Struktur dieser Protokolle variieren je nach Azure-Dienst und Ressourcentyp.
- [Aktivitätsprotokolle](../platform/activity-logs-overview.md) bieten Einblicke in die Vorgänge für jede Azure-Ressource im Abonnement von außen (die Verwaltungsebene), z. B. die Erstellung einer neuen Ressource oder den Start eines virtuellen Computers. Diese Informationen bieten Antworten auf die Fragen Was, Wer und Wann für alle Schreibvorgänge (PUT, POST, DELETE), die für die Ressourcen Ihres Abonnements durchgeführt wurden.


## <a name="configuration-requirements"></a>Konfigurationsanforderungen
Einige Überwachungsdaten werden automatisch erfasst. Sie müssen jedoch je nach Ihren Anforderungen eventuell einige Konfigurationen vornehmen. Weiter unten finden Sie spezifische Informationen zu den einzelnen Typen von Überwachungsdaten.

- [Plattformmetriken](../platform/data-platform-metrics.md) werden automatisch in [Azure Monitor-Metriken](../platform/data-platform-metrics.md) erfasst, ohne dass eine Konfiguration erforderlich ist. Erstellen Sie eine Diagnoseeinstellung, um Einträge an Azure Monitor-Protokolle zu senden oder an ein Ziel außerhalb von Azure weiterzuleiten.
- [Ressourcenprotokolle](../platform/resource-logs-overview.md) werden automatisch von Azure-Ressourcen generiert, aber nicht ohne Diagnoseeinstellung erfasst.  Erstellen Sie eine Diagnoseeinstellung, um Einträge an Azure Monitor-Protokolle zu senden oder an ein Ziel außerhalb von Azure weiterzuleiten.
- [Aktivitätsprotokolle](../platform/activity-logs-overview.md) werden automatisch gesammelt, ohne dass eine Konfiguration erforderlich ist. Sie können im Azure-Portal angezeigt werden. Erstellen Sie eine Diagnoseeinstellung, um sie in Azure Monitor-Protokolle zu kopieren oder an ein Ziel außerhalb von Azure weiterzuleiten.


## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Diagnoseeinstellungen definieren, wohin Ressourcenprotokolle und -metriken für eine bestimmte Ressource gesendet werden sollen. Mögliche Ziele:

- [Log Analytics-Arbeitsbereiche](../platform/resource-logs-collect-workspace.md) ermöglichen Ihnen das Analysieren von Daten zusammen mit anderen Überwachungsdaten, die mithilfe leistungsstarker Protokollabfragen von Azure Monitor erfasst werden, sowie die Nutzung anderer Azure Monitor-Features wie Warnungen und Visualisierungen. 
- [Event Hubs](../platform/resource-logs-stream-event-hubs.md) ermöglichen das Streamen von Daten an externe Systeme, z. B. SIEMs und andere Protokollanalyselösungen von Drittanbietern. 
- [Azure Storage-Konten](../platform/resource-logs-collect-storage.md) sind hilfreich für Überwachung, statische Analyse oder Sicherung.

Befolgen Sie die Vorgehensweise unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](../platform/diagnostic-settings.md), um Diagnoseeinstellungen über das Azure-Portal zu erstellen und zu verwalten. Unter [Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage](../platform/diagnostic-settings-template.md) erfahren Sie, wie Sie sie in einer Vorlage definieren und die komplette Überwachung für eine Ressource bei ihrer Erstellung aktivieren.


## <a name="monitoring-in-the-azure-portal"></a>Überwachen im Azure-Portal
 Sie können über das Ressourcenmenü im Azure-Portal auf die Überwachungsdaten für die meisten Azure-Ressourcen zugreifen. Dadurch erhalten Sie Zugriff auf die Daten einer einzelnen Ressource mithilfe der Standardtools von Azure Monitor. Einige Azure-Dienste bieten unterschiedliche Optionen, daher sollten Sie die Dokumentation des jeweiligen Diensts lesen, um weitere Informationen zu erhalten. Verwenden Sie das Menü **Azure Monitor**, um Daten aus allen überwachten Ressourcen zu analysieren. 

Viele Dienste stellen auf der Seite **Übersicht** Überwachungsdaten bereit, die einen kurzen Einblick in ihren Betrieb erlauben. Die Informationen basieren in der Regel auf einer Teilmenge der Plattformmetriken, die in Azure Monitor-Metriken gespeichert werden. Zusätzliche Überwachungsoptionen sind normalerweise im Abschnitt **Überwachung** der einzelnen Dienste verfügbar. Menü.

![Seite „Übersicht“](media/monitor-azure-resource/overview-page.png)

## <a name="analyzing-metrics"></a>Analysieren von Metriken
Analysieren Sie einzelne Metriken, und stellen Sie mehrere Metriken mithilfe des [Metrik-Explorers](../platform/metrics-getting-started.md) gegenüber, um Korrelationen und Trends zu ermitteln. Einige Dienste bieten eine angepasste Umgebung für die Arbeit mit ihren Metriken, wenn Sie im Menü der Ressource die Option **Metriken** öffnen.

- Grundlegendes zur Verwendung des Metrik-Explorers finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../platform/metrics-getting-started.md).
- Weitere Informationen zu erweiterten Features des Metrik-Explorers, wie z. B. Verwenden mehrerer Metriken und Anwenden von Filtern und Aufteilungen, finden Sie unter [Erweiterte Funktionen von Azure Metrik-Explorer](../platform/metrics-charts.md).

![metrics](media/monitor-azure-resource/metrics.png)


## <a name="analyzing-logs"></a>Analysieren von Protokollen

### <a name="activity-log"></a>Aktivitätsprotokoll 
Zeigen Sie die Einträge im Aktivitätsprotokoll im Azure-Portal an, wobei der anfängliche Filter auf die aktuelle Ressource festgelegt ist. Kopieren Sie das Aktivitätsprotokoll in einen Log Analytics-Arbeitsbereich, um darauf zuzugreifen und es in Protokollabfragen und Arbeitsmappen zu verwenden. 

- Weitere Informationen zum Anzeigen des Aktivitätsprotokolls und zum Abrufen von Einträgen mithilfe verschiedener Methoden finden Sie unter [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](../platform/activity-log-view.md).
- Informationen zu den spezifischen Ereignissen, die protokolliert werden, finden Sie in der Dokumentation zum jeweiligen Azure-Dienst.

![Aktivitätsprotokoll](media/monitor-azure-resource/activity-log.png)

### <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle
Azure Monitor-Protokolle konsolidieren Protokolle und Metriken aus mehreren Diensten und anderen Datenquellen für die Analyse mit einem leistungsfähigen Abfragetool. Erstellen Sie wie oben beschrieben eine Diagnoseeinstellung zum Erfassen von Plattformmetriken, Aktivitätsprotokollen und Ressourcenprotokollen in einem Log Analytics-Arbeitsbereich in Azure Monitor.

[Log Analytics](../log-query/get-started-portal.md) ermöglicht Ihnen die Arbeit mit [Protokollabfragen](../log-query/log-query-overview.md). Hierbei handelt es sich um ein leistungsstarkes Feature von Azure Monitor, mit dem Sie mithilfe einer Abfragesprache mit vollem Funktionsumfang eine erweiterte Analyse der Protokolldaten durchführen können. Öffnen Sie Log Analytics über **Protokolle** im Menü **Überwachung** für eine Azure-Ressource, um mit Protokollabfragen mit dieser Ressource als [Abfragebereich](../log-query/scope.md#query-scope) zu arbeiten. Auf diese Weise können Sie Daten über mehrere Tabellen hinweg ausschließlich für diese Ressource analysieren. Verwenden Sie **Protokolle** im Menü von Azure Monitor, um auf Protokolle für alle Ressourcen zuzugreifen. 

- Eine exemplarische Vorgehensweise zum Schreiben einer Abfrage mit Log Analytics und zum Arbeiten mit den Ergebnissen finden Sie unter [Erste Schritte mit Log Analytics in Azure Monitor](../log-query/get-started-portal.md).
- Ein Tutorial zur Verwendung der Abfragesprache zum Schreiben von Protokollabfragen finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](../log-query/get-started-queries.md).
- Informationen zur Erfassung von Ressourcenprotokollen in Azure Monitor-Protokollen und Details dazu, wie Sie in einer Abfrage darauf zugreifen, finden Sie unter [Erfassen von Azure-Ressourcenprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor](../platform/resource-logs-collect-workspace.md).
- Eine Erläuterung der Struktur von Ressourcenprotokolldaten in Azure Monitor-Protokollen finden Sie unter [Sammlungsmodus](../platform/resource-logs-collect-workspace.md#collection-mode).
- Ausführliche Informationen zu den Tabellen der einzelnen Azure-Dienste in Azure Monitor Protokollen finden Sie in der jeweiligen Dokumentation.

![Protokolle](media/monitor-azure-resource/logs.png)


## <a name="alerts"></a>Alerts
[Warnungen](../platform/alerts-overview.md) informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden, und ermöglichen damit zeitnahe Aktionen. Sie erstellen eine Warnungsregel, mit der ein Ziel für die Warnung, die Bedingungen für das Erstellen einer Warnung und alle Aktionen definiert werden, die als Reaktion ausgeführt werden sollen.

Für die verschiedenen Arten von Warnungsregeln werden unterschiedliche Arten von Überwachungsdaten verwendet.

- [Aktivitätsprotokollwarnungen](../platform/alerts-activity-log.md) erstellen eine Warnung, wenn im Aktivitätsprotokoll ein Eintrag erstellt wird, der bestimmten Kriterien entspricht. Dadurch können Sie beispielsweise benachrichtigt werden, wenn ein bestimmter Ressourcentyp erstellt wird oder wenn eine Konfigurationsänderung zu einem Fehler führt.
- [Metrikwarnungen](../platform/alerts-metric.md) erstellen eine Warnung, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet. Metrikwarnungen reagieren schneller als andere Warnungen und können automatisch aufgelöst werden, wenn das Problem korrigiert wurde.
- [Protokollabfragewarnungen](../platform/alerts-log.md) führen in regelmäßigen Abständen eine Protokollabfrage aus und erstellen eine Warnung, wenn eine bestimmte Bedingung erfüllt wurde. Auf diese Weise können Sie komplexe Analysen für mehrere Datensätze durchführen.

Verwenden Sie **Warnungen** im Menü einer Ressource, um Warnungen anzuzeigen und die Benachrichtigungsregeln für diese Ressource zu verwalten. Nur Aktivitätsprotokollwarnungen und Metrikwarnungen verwenden einzelne Azure-Ressourcen als Ziel. Protokollabfragewarnungen verwenden einen Log Analytics-Arbeitsbereich als Ziel. Sie basieren auf einer Abfrage, die auf alle in diesem Arbeitsbereich gespeicherten Protokolle zugreifen kann. Verwenden Sie das Azure Monitor-Menü, um Warnungen für alle Ressourcen anzuzeigen und zu verwalten und die Regeln für die Protokollabfragewarnungen zu verwalten.

- Ausführliche Informationen zum Erstellen von Warnungsregeln finden Sie in den Artikeln zu den verschiedenen Arten von Warnungen weiter oben.
- Ausführliche Informationen zum Erstellen einer Aktionsgruppe, mit der Sie Reaktionen auf Warnungen verwalten können, finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../platform/action-groups.md).

## <a name="insights-and-solutions"></a>Erkenntnisse und Lösungen 
Einige Dienste bieten Tools, die über die Standardfeatures von Azure Monitor hinausgehen. [Lösungen](../insights/solutions.md) bieten eine vordefinierte Überwachungslogik, die auf Standardfeatures von Azure Monitor basiert. [Erkenntnisse](../insights/insights-overview.md) bieten eine angepasste Überwachungsumgebung, die auf der Azure Monitor-Datenplattform und den Standardfeatures basiert.

Wenn für eine Ressource Erkenntnisse verfügbar sind, können Sie über die Option **Erkenntnisse** im Ressourcenmenü darauf zugreifen. Sie können auf alle Erkenntnisse und Lösungen über das Azure Monitor-Menü zugreifen.

- Sehen Sie sich die Dokumentation zur Überwachung für die einzelnen Dienste an, um zu ermitteln, ob Erkenntnisse oder Lösungen verfügbar sind.

![Einblicke](media/monitor-azure-resource/insights.png)

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zu Ressourcenprotokollen für die einzelnen Azure-Dienste finden Sie unter [Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle](../platform/diagnostic-logs-schema.md).  
