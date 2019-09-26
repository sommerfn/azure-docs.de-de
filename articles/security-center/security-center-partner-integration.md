---
title: Integrieren von Sicherheitslösungen in Azure Security Center | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Partner in Azure Security Center integriert werden, um die allgemeine Sicherheit Ihrer Azure-Ressourcen zu verbessern.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: memildin
ms.openlocfilehash: ce8aa0229d9a9b873a489209065ada588adbffff
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71257740"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrieren von Sicherheitslösungen in Azure Security Center
Dieses Dokument unterstützt Sie bei der Verwaltung von bereits mit Azure Security Center verbundenen Sicherheitslösungen sowie beim Hinzufügen neuer Lösungen.

> [!NOTE]
> Eine Teilmenge der Sicherheitslösungen lief am 31. Juli 2019 aus. Weitere Informationen und alternative Dienste finden Sie unter [Auslaufen von Security Center-Funktionen (Juli 2019)](security-center-features-retirement-july2019.md#menu_solutions).

## <a name="integrated-azure-security-solutions"></a>Integrierte Azure-Sicherheitslösungen
Mit Security Center können Sie ganz einfach integrierte Sicherheitslösungen in Azure aktivieren. Dies hat unter anderem folgende Vorteile:

- **Vereinfachte Bereitstellung:** Security Center bietet eine optimierte Bereitstellung integrierter Partnerlösungen. Für Lösungen wie Antischadsoftware und Sicherheitsrisikobewertungen kann Security Center den erforderlichen Agent auf Ihren virtuellen Computern bereitstellen, und für Firewallgeräte kann sich Security Center um den Großteil der erforderlichen Netzwerkkonfiguration kümmern.
- **Integrierte Erkennungen:** Sicherheitsereignisse von Partnerlösungen werden automatisch gesammelt, aggregiert und im Rahmen der Security Center-Warnungen und -Vorfälle angezeigt. Diese Ereignisse werden mit Erkennungen anderer Quellen zusammengeführt, um erweiterte Bedrohungserkennungsfunktionen bereitzustellen.
- **Einheitliche Integritätsüberwachung und -verwaltung:** Mithilfe integrierter Integritätsereignisse können Kunden alle Partnerlösungen auf einen Blick überwachen. Es ist eine grundlegende Verwaltung verfügbar – mit einfachem Zugriff auf die erweiterte Konfiguration über die Partnerlösung.

Derzeit umfassen die integrierten Sicherheitslösungen Sicherheitsrisikobewertung durch [Qualys](https://www.qualys.com/public-cloud/#azure), [Rapid7](https://www.rapid7.com/products/insightvm/) und Microsoft Application Gateway-Webanwendungsfirewall.

> [!NOTE]
> Security Center installiert Microsoft Monitoring Agent nicht auf virtuellen Partnergeräten, da die meisten Sicherheitsanbieter die Ausführung externer Agents auf ihrem Gerät verbieten.
>
>

## <a name="how-security-solutions-are-integrated"></a>Informationen zur Integration von Sicherheitslösungen
Über Security Center bereitgestellte Azure-Sicherheitslösungen werden automatisch verbunden. Sie können auch andere Sicherheitsdatenquellen verwenden, so auch Computer, die lokal oder in anderen Clouds ausgeführt werden.

![Integration von Partnerlösungen](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Verwalten integrierter Azure-Sicherheitslösungen und anderer Datenquellen

1. Melden Sie sich beim [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) an.

2. Wählen Sie im **Microsoft Azure-Menü** die Option **Security Center**. **Security Center – Übersicht** wird geöffnet.

3. Klicken Sie im Menü von Security Center auf **Sicherheitslösungen**.

   ![Übersicht über Security Center](./media/security-center-partner-integration/overview.png)

Unter **Sicherheitslösungen** können Sie Informationen zur Integrität integrierter Azure-Sicherheitslösungen anzeigen und grundlegende Verwaltungsaufgaben durchführen.

### <a name="connected-solutions"></a>Verbundene Lösungen

Der Abschnitt **Verbunde Lösungen** enthält Sicherheitslösungen, die derzeit mit Security Center verbunden sind, sowie Informationen zum Integritätsstatus der einzelnen Lösungen.  

![Verbundene Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Eine Partnerlösung kann den folgenden Status aufweisen:

* Fehlerfrei (grün): Es liegt kein Integritätsproblem vor.
* Fehlerhaft (rot): Es liegt ein Integritätsproblem vor, das sofort untersucht werden muss.
* Integritätsprobleme (orange): Die Lösung hat das Melden der Integrität beendet.
* Nicht gemeldet (grau): Die Lösung hat noch nichts berichtet. Der Status einer Lösung kann unter Umständen noch nicht gemeldet worden sein, wenn die Verbindung kürzlich hergestellt wurde und die Bereitstellung noch nicht abgeschlossen ist, oder es sind keine Integritätsdaten verfügbar.

> [!NOTE]
> Wenn keine Integritätsstatusdaten verfügbar sind, zeigt Security Center das Datum und die Uhrzeit des letzten empfangenen Ereignisses an, um anzugeben, ob die Lösung Berichte übermittelt. Wenn keine Integritätsdaten verfügbar sind und innerhalb der letzten 14 Tage keine Warnungen empfangen wurden, gibt Security Center an, dass die Lösung fehlerhaft ist oder nicht berichtet.
>
>

1. Wählen Sie **ANSICHT**, um zusätzliche Informationen und Optionen anzuzeigen, darunter:

   - **Lösungskonsole**. Öffnet die Verwaltungsoberfläche für diese Lösung.
   - **Virtuellen Computer verknüpfen**. Öffnet das Blatt „Anwendungen verknüpfen“. Hier können Sie Ressourcen mit der Partnerlösung verknüpfen.
   - **Lösung löschen**.
   - **Konfigurieren**.

   ![Details der Partnerlösung](./media/security-center-partner-solutions/partner-solutions-detail.png)

### <a name="discovered-solutions"></a>Ermittelte Lösungen

Security Center erkennt Sicherheitslösungen automatisch, die in Azure ausgeführt werden, aber nicht mit Security Center verbunden sind, und zeigt die Lösungen im Abschnitt **Ermittelte Lösungen** an. Dazu gehören sowohl Azure-Lösungen wie [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) als auch Partnerlösungen.

> [!NOTE]
> Für das ermittelte Lösungsfeature ist der Standardtarif von Security Center ist auf Abonnementebene erforderlich. Weitere Informationen zu den Tarifen von Security finden Sie unter [Preise](security-center-pricing.md).
>
>

Wählen Sie unterhalb einer Lösung die Option **VERBINDEN**, um die Integration in Security Center durchzuführen und über Sicherheitswarnungen benachrichtigt zu werden.

![Ermittelte Lösungen](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

### <a name="add-data-sources"></a>Hinzufügen von Datenquellen

Der Abschnitt **Datenquellen hinzufügen** enthält weitere verfügbare Datenquellen, mit denen eine Verbindung hergestellt werden kann. Klicken Sie **HINZUFÜGEN**, um eine Anleitung zum Hinzufügen von Daten aus einer dieser Quellen anzuzeigen.

![Datenquellen](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)

## <a name="exporting-data-to-a-siem"></a>Exportieren von Daten in ein SIEM-Tool

Von Azure Security Center erzeugte verarbeitete Ereignisse werden im [Aktivitätsprotokoll](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) von Azure veröffentlicht, einem der über Azure Monitor verfügbaren Protokolltypen. Azure Monitor bietet eine konsolidierte Pipeline zum Routing beliebiger Überwachungsdaten zu einem SIEM-Tool. Dies erfolgt durch Streaming der Daten an einen Event Hub, von wo aus sie dann in ein Partnertool hereingezogen werden können.

Diese Pipe verwendet die [einzelne Azure-Überwachungspipeline](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) für den Zugriff auf die Überwachungsdaten aus Ihrer Azure-Umgebung. So können Sie mühelos SIEMs und Überwachungstools zum Verarbeiten der Daten einrichten.

In den nächsten Abschnitten wird beschrieben, wie Sie Daten zum Streamen an einen Event Hub konfigurieren können. Die Schritte setzen voraus, dass Sie bereits Azure Security Center in Ihrem Azure-Abonnement konfiguriert haben.

Allgemeine Übersicht

![Allgemeine Übersicht](media/security-center-export-data-to-siem/overview.png)

### <a name="what-is-the-azure-security-data-exposed-to-siem"></a>Was sind die für SIEM verfügbar gemachten Azure-Sicherheitsdaten?

In dieser Version stellen wir die [Sicherheitswarnungen](../security-center/security-center-managing-and-responding-alerts.md) zur Verfügung. In zukünftigen Versionen wird das Dataset mit Empfehlungen zur Sicherheit erweitert.

### <a name="how-to-setup-the-pipeline"></a>Einrichten der Pipeline

#### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Bevor Sie beginnen, müssen Sie [einen Event Hubs-Namespace erstellen](../event-hubs/event-hubs-create.md). Dieser Namespace und Event Hub sind das Ziel für sämtliche Ihrer Überwachungsdaten.

#### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Streamen des Azure-Aktivitätsprotokolls an Event Hubs

Bitte lesen Sie den folgenden Artikel: [Streamen des Azure-Aktivitätsprotokolls an Event Hubs](../azure-monitor/platform/activity-logs-stream-event-hubs.md).

#### <a name="install-a-partner-siem-connector"></a>Installieren eines Partner-SIEM-Connectors 

Die Weiterleitung Ihrer Überwachungsdaten an einen Event Hub mit Azure Monitor ermöglicht eine einfache Integration in SIEM- und Überwachungstools von Partnern.

Unter dem folgenden Link finden Sie die Liste [unterstützter SIEMs](../azure-monitor/platform/resource-logs-stream-event-hubs.md#what-you-can-do-with-resource-logs-sent-to-an-event-hub).

### <a name="example-for-querying-data"></a>Beispiel zum Abfragen von Daten 

Hier sind einige Splunk-Abfragen, mit denen Sie Warnungsdaten erhalten können:

| **Beschreibung der Abfrage** | **Abfrage** |
|----|----|
| Alle Warnungen| index=main Microsoft.Security/locations/alerts|
| Zusammenfassen der Anzahl der Vorgänge nach deren Namen| index=main sourcetype="amal:security" \| table operationName \| stats count by operationName|
| Abrufen von Informationen zu Warnungen: Zeit, Name, Status, ID und Abonnement | index=main Microsoft.Security/locations/alerts \| table \_time, properties.eventName, State, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Partnerlösungen in Security Center integrieren. Weitere Informationen zu Security Center finden Sie in den folgenden Artikeln:

* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung von Security Center.
* [Azure Security-Blog](https://blogs.msdn.com/b/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
