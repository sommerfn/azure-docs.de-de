---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2019
ms.author: cynthn
ms.openlocfilehash: 93a2554b5d3cc24e1b5fc1e3d0f18ed1bfe0579c
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71692033"
---
Angesichts des starken Wachstums der in Azure gehosteten VMs ist es wichtig, Leistungs- und Integritätsprobleme zu identifizieren, die sich auf Anwendungen und Infrastrukturdienste auswirken, die sie unterstützen. Grundlegende Überwachung wird standardmäßig mit Azure durch die Metriktypen CPU-Auslastung, Datenträgerverwendung, Arbeitsspeicherauslastung und Netzwerkdatenverkehr bereitgestellt, die vom Hosthypervisor erfasst werden. Zusätzliche Metrik-und Protokolldaten können mit [Erweiterungen](../articles/virtual-machines/windows/extensions-features.md) erfasst werden, um die Diagnose für Ihre virtuellen Computer über das Gastbetriebssystem zu konfigurieren.

Um Leistungs- und Integritätsprobleme mit dem Gastbetriebssystem, .NET-basierten oder Java-Webanwendungskomponenten, die innerhalb der VM ausgeführt werden, zu erkennen und zu diagnostizieren, bietet Azure Monitor eine zentrale Überwachung mit umfassenden Funktionen wie Azure Monitor für VMs und Application Insights.

## <a name="diagnostics-and-metrics"></a>Diagnose und Metriken 

Sie können die Sammlung von [Diagnosedaten](https://docs.microsoft.com/cli/azure/vm/diagnostics) mit [Metriken](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) im Azure-Portal, der Azure CLI, Azure PowerShell und per Programmierung von APIs (Applications Programming Interfaces) einrichten und überwachen. Sie haben beispielsweise folgende Möglichkeiten:

- **Verfolgen der grundlegenden Metriken für die VM** Auf der Übersichtsseite im Azure-Portal gehören zu den angezeigten grundlegenden Metriken CPU-Auslastung, Netzwerkauslastung, Gesamtwert der Datenträgerbytes und Datenträgervorgänge pro Sekunde.

- **Aktivieren der Sammlung von Startdiagnosedaten und Anzeigen über das Azure-Portal** Wenn Sie Ihr eigenes Image in Azure verwenden oder sogar eines der Plattformimages starten, kann sich eine VM aus zahlreichen Gründen in einem nicht startfähigen Zustand befinden. Sie können beim Erstellen einer VM leicht die Startdiagnose aktivieren, indem Sie auf der Seite mit den Einstellungen im Abschnitt „Überwachung“ für „Startdiagnose“ auf **Aktiviert** klicken.

    Wenn VMs gestartet werden, erfasst der Agent für die Startdiagnose die Startausgabe und speichert sie in Azure Storage. Diese Daten können zum Beheben von Startproblemen bei virtuellen Computern verwendet werden. Die Startdiagnose wird nicht automatisch aktiviert, wenn Sie mit Befehlszeilentools eine VM erstellen. Vor dem Aktivieren der Startdiagnose muss ein Speicherkonto zum Speichern der Startprotokolle erstellt werden. Wenn Sie die Startdiagnose im Azure-Portal aktivieren, wird für Sie automatisch ein Speicherkonto erstellt.

    Falls Sie die Startdiagnose beim Erstellen der VM nicht aktiviert haben, können Sie dies später immer nachholen, indem Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic) oder eine [Azure Resource Manager-Vorlage](../articles/virtual-machines/windows/extensions-diagnostics-template.md) verwenden.

- **Aktivieren der Sammlung von Diagnosedaten für das Gastbetriebssystem** Beim Erstellen einer VM können Sie auf der Seite mit den Einstellungen die Diagnose für das Gastbetriebssystem aktivieren. Wenn Sie die Sammlung von Diagnosedaten aktivieren, wird der VM die [IaaSDiagnostics-Erweiterung für Linux](../articles/virtual-machines/linux/diagnostic-extension.md) oder die [IaaSDiagnostics-Erweiterung für Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) hinzugefügt. Dies ermöglicht Ihnen die Erfassung von zusätzlichen Datenträger-, CPU- und Arbeitsspeicherdaten.

    Mit den gesammelten Diagnosedaten können Sie für Ihre VMs die automatische Skalierung konfigurieren. Sie können auch [Azure Monitor-Protokolle](../articles/azure-monitor/platform/data-platform-logs.md) zum Speichern der Daten konfigurieren und Warnungen einrichten, damit Sie benachrichtigt werden, wenn die Leistung nicht optimal ist.

## <a name="alerts"></a>Alerts

Sie können [Warnungen](../articles/azure-monitor/platform/alerts-overview.md) auf Grundlage von bestimmten Leistungsmetriken erstellen. Beispiele für die Probleme, über die Sie benachrichtigt werden können, sind das Überschreiten eines bestimmten Schwellenwerts für die durchschnittliche CPU-Auslastung oder das Fallen des verfügbaren freien Datenträger-Speicherplatzes unter einen bestimmten Wert. Warnungen können im [Azure-Portal](../articles/azure-monitor/platform/alerts-metric.md#create-with-azure-portal), mit [Azure Resource Manager-Vorlagen](../articles/azure-monitor/platform/alerts-metric-create-templates.md) oder mit der [Azure CLI](../articles/azure-monitor/platform/alerts-metric.md#with-azure-cli) konfiguriert werden.

## <a name="azure-service-health"></a>Azure Service Health

[Azure Service Health](../articles/service-health/service-health-overview.md) bietet personalisierte Leitfäden und Support, wenn sich Probleme in Azure-Diensten auf Ihre Arbeit auswirken, und hilft Ihnen bei der Vorbereitung auf bevorstehende geplante Wartungen. Azure Service Health warnt Sie und Ihre Teams mit gezielten und flexiblen Benachrichtigungen.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure Resource Health](../articles/service-health/resource-health-overview.md) unterstützt Sie bei der Diagnose und bei Supportanfragen, wenn ein Problem mit Azure Auswirkungen auf Ihre Ressourcen hat. Der Dienst informiert Sie über die aktuelle und frühere Integrität Ihrer Ressourcen und unterstützt Sie beim Beheben von Problemen. Resource Health bietet technischen Support, wenn Sie Unterstützung bei Azure-Dienstproblemen benötigen.

## <a name="azure-activity-log"></a>Azure-Aktivitätsprotokoll

Das [Azure-Aktivitätsprotokoll](../articles/azure-monitor/platform/activity-logs-overview.md) ist ein Abonnementprotokoll, das Einblicke in Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dieses Protokoll schließt einen Datenbereich von Azure Resource Manager-Betriebsdaten bis hin zu Aktualisierungen für Dienstintegritätsereignisse ein. Sie können im Azure-Portal auf „Aktivitätsprotokoll“ klicken, um das Protokoll für Ihre VM anzuzeigen.

Hier sind einige Verwendungsmöglichkeiten für das Aktivitätsprotokoll aufgeführt:

- [Erstellen einer Warnung zu einem Aktivitätsprotokollereignis](../articles/azure-monitor/platform/activity-logs-overview.md)
- [Streamen an einen Event Hub](../articles/azure-monitor/platform/activity-logs-stream-event-hubs.md) zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI.
- Analysieren in Power BI mit dem [Power BI-Inhaltspaket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Speichern unter einem Speicherkonto](../articles/azure-monitor/platform/archive-activity-log.md) zur Archivierung oder manuellen Untersuchung Sie können die Aufbewahrungsdauer (in Tagen) mithilfe des Protokollprofils angeben.

Außerdem können Sie auf die Daten des Aktivitätsprotokolls zugreifen, indem Sie [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), die [Azure CLI](https://docs.microsoft.com/cli/azure/monitor) oder [Monitor-REST-APIs](https://docs.microsoft.com/rest/api/monitor/) verwenden.

[Azure-Ressourcenprotokolle](../articles/azure-monitor/platform/resource-logs-overview.md) sind von Ihrer VM ausgegebene Protokolle, die ausführliche Daten über den Betrieb der VM enthalten. Ressourcenprotokolle unterscheiden sich vom Aktivitätsprotokoll, indem sie Einblick in Vorgänge gewähren, die auf der VM durchgeführt wurden.

Hier sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle aufgeführt:

- Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [Speicherkonto](../articles/azure-monitor/platform/archive-diagnostic-logs.md) gespeichert werden. Mithilfe der Diagnoseeinstellungen für Ressourcen können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
- Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI [an Event Hubs streamen](../articles/azure-monitor/platform/resource-logs-stream-event-hubs.md).
- Analysieren Sie sie mit [Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Erweiterte Überwachung

Um die von der Azure-VM und den VM-Skalierungsgruppen unterstützte Anwendung oder den Dienst sichtbar zu machen, Probleme mit dem Gastbetriebssystem oder der in der VM ausgeführten Workload zu identifizieren, um zu verstehen, ob dies die Verfügbarkeit oder Leistung der Anwendung beeinträchtigt oder ein Problem mit der Anwendung ist, aktivieren Sie sowohl [Azure Monitor für VMs](../articles/azure-monitor/insights/vminsights-overview.md) als auch [Application Insights](../articles/azure-monitor/app/app-insights-overview.md).

Azure Monitor für VMs überwacht Ihre virtuellen Azure-Computer im gewünschten Umfang, indem eine Analyse der Leistung und Integrität Ihrer Windows- und Linux-VMs durchgeführt wird. Dies schließt die verschiedenen Prozesse und Abhängigkeiten von anderen Ressourcen und externen Prozessen ein, die erkannt werden. Das Tool umfasst mehrere Trendleistungsdiagramme, die bei der Untersuchung von Problemen und der Bewertung der Kapazität ihrer VMs helfen. Die Abhängigkeitszuordnung zeigt überwachte und nicht überwachte Computer, ausgefallene und aktive Netzwerkverbindungen zwischen Prozessen und diesen Computern sowie Trenddiagramme mit Netzwerkverbindungs-Standardmetriken. In Kombination mit Application Insights überwachen Sie Ihre Anwendung und erfassen Telemetriedaten, etwa HTTP-Anforderungen, Ausnahmen usw., damit Sie Probleme zwischen den VMs und ihrer Anwendung korrelieren können. Konfigurieren Sie [Azure Monitor-Warnungen](../articles/azure-monitor/platform/alerts-overview.md), um Sie über wichtige Bedingungen zu informieren, die von den durch Azure Monitor für VMs erfassten Überwachungsdaten erkannt werden.

## <a name="next-steps"></a>Nächste Schritte

- Führen Sie die Schritte unter [Überwachen eines virtuellen Windows-Computers mit Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) oder [Überwachen eines virtuellen Linux-Computers mit der Azure CLI](../articles/virtual-machines/linux/tutorial-monitoring.md) aus.

- Informieren Sie sich über die bewährten Methoden zum Durchführen der [Überwachung und Diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
