---
title: Übersicht über Protokolle der Azure-Plattform | Microsoft-Dokumentation
description: Übersicht über Diagnoseprotokolle in Azure, die regelmäßig umfassende Daten zum Betrieb einer Azure-Ressource liefern.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: c1602b9beb5e667caf1688901c7ae4e67e5f5839
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263041"
---
# <a name="overview-of-azure-platform-logs"></a>Übersicht über Protokolle der Azure-Plattform
Plattformprotokolle liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. Sie werden automatisch generiert, obwohl Sie bestimmte Plattformprotokolle so konfigurieren müssen, dass sie an mindestens ein Ziel weitergeleitet werden, damit sie gespeichert werden. Dieser Artikel bietet einen Überblick über die Plattformprotokolle, einschließlich der Informationen, die sie liefern und wie Sie sie für die Erfassung und Analyse konfigurieren können.

## <a name="types-of-platform-logs"></a>Typen von Plattformprotokollen
In der folgenden Tabelle werden die spezifischen Plattformprotokolle aufgeführt, die auf unterschiedlichen Ebenen von Azure verfügbar sind.

| Ebene | Protokolle | BESCHREIBUNG |
|:---|:---|:---|
| Azure-Ressourcen | [Ressourcenprotokolle](resource-logs-overview.md) | Sie bieten einen Einblick in Vorgänge, die innerhalb einer Azure-Ressource (der *Datenebene*) ausgeführt wurden, z.B. das Abrufen eines Geheimnisses aus einem Key Vault oder die Ausgabe einer Anforderung an eine Datenbank. Der Inhalt dieser Protokolle variiert je nach Azure-Dienst und -Ressourcentyp.<br>*Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.*  |
| Azure-Abonnement | [Aktivitätsprotokoll](activity-logs-overview.md) | Bietet Einblicke in die Vorgänge für jede Azure-Ressource im Abonnement von außen (*die Verwaltungsebene*) sowie Aktualisierungen zu Service Health-Ereignissen. Es gibt jeweils ein Aktivitätsprotokoll für jedes Azure-Abonnement.   |
| Azure-Mandant | [Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/overview-reports.md)  | Sie enthalten den Verlauf der Anmeldeaktivität und das Überwachungsprotokoll der Änderungen, die in Azure Active Directory für einen bestimmten Mandanten vorgenommen wurden.   |


![Übersicht über Plattformprotokolle](media/platform-logs-overview/logs-overview.png)

## <a name="viewing-platform-logs"></a>Anzeigen von Plattformprotokollen
Sie können das [Aktivitätsprotokoll](activity-log-view.md) und [Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/overview-reports.md) im Azure-Portal anzeigen. Sie müssen Ressourcenprotokolle an ein [Ziel](#destinations) senden, um Sie anzeigen zu können.


## <a name="destinations"></a>Ziele
Abhängig von den Überwachungsanforderungen können Sie Plattformprotokolle an mindestens ein in der folgenden Tabelle genanntes Ziel senden. 

| Ziel | Szenario | Referenzen |
|:---|:---|:---|:---|
| Log Analytics-Arbeitsbereich | Analysieren Sie die Protokolle mit anderen Überwachungsdaten, und nutzen Sie Azure Monitor-Features, etwa Protokollabfragen und Warnungen. | [Ressourcenprotokolle](resource-logs-collect-storage.md)<br>[Aktivitätsprotokoll](activity-log-collect.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Azure-Speicher | Archivieren Sie die Protokolle für Überwachung, statische Analyse oder Sicherung. |[Ressourcenprotokolle](archive-diagnostic-logs.md)<br>[Aktivitätsprotokoll](activity-log-export.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Event Hub | Streamen Sie die Protokolle an Protokollierungs- und Telemetriesysteme von Drittanbietern.  |[Ressourcenprotokolle](resource-logs-stream-event-hubs.md)<br>[Aktivitätsprotokoll](activity-log-export.md)<br>[Azure Active Directory-Protokolle](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md) |


## <a name="diagnostic-settings-and-log-profiles"></a>Diagnoseeinstellungen und Protokollprofile
Konfigurieren Sie Ziele für Ressourcenprotokolle und Azure Active Directory-Protokolle, indem Sie eine [Diagnoseeinstellung erstellen](diagnostic-settings.md). Konfigurieren Sie Ziele für das Aktivitätsprotokoll, indem Sie [ein Protokollprofil erstellen](activity-log-export.md) oder [es mit einem Log Analytics-Arbeitsbereich verbinden](activity-log-collect.md).

Die Diagnoseeinstellung und das Protokollprofil definieren Folgendes:

- Mindestens ein Ziel zum Senden ausgewählter Protokolle und Metriken.
- Welche Protokollkategorien und Metriken der Ressource an die Ziele gesendet werden.
- Ob ein Speicherkonto als Ziel ausgewählt wird und wie lange die einzelnen Protokollkategorien beibehalten werden sollen.



## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Aktivitätsprotokoll](activity-logs-overview.md)
* [Weitere Informationen zu Ressourcenprotokollen](resource-logs-overview.md)
* [Anzeigen des Ressourcenprotokollschemas für verschiedene Azure-Dienste](diagnostic-logs-schema.md)
