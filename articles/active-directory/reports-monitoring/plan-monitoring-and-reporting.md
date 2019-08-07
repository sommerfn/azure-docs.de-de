---
title: Planen einer Azure Active Directory-Bereitstellung für Berichterstellung und Überwachung
description: In diesem Artikel wird beschrieben, wie Sie die Implementierung der Berichterstellung und Überwachung planen und durchführen.
services: active-directory
documentationcenter: ''
author: BarbaraSelden
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: plan
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: baselden
ms.reviewer: plenzke
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f68b3a312ff7681fde65154542a66767c5195ff
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406395"
---
# <a name="plan-an-azure-active-directory-reporting-and-monitoring-deployment"></a>Planen einer Azure Active Directory-Bereitstellung für Berichterstellung und Überwachung

Ihre Azure Active Directory-Lösung für die Berichterstellung und Überwachung hängt von gesetzlichen, betrieblichen und sicherheitstechnischen Anforderungen und vorhandenen Umgebungen und Prozessen ab. In diesem Artikel werden die unterschiedlichen Entwurfsoptionen vorgestellt. Zudem dient er als Richtlinie zur Ausarbeitung der geeigneten Bereitstellungsstrategie.

### <a name="benefits-of-azure-ad-reporting-and-monitoring"></a>Vorteile der Azure AD-Berichterstellung und -Überwachung

Die Azure AD-Berichterstellung bietet eine umfangreiche Ansicht sowie Protokolle zu den Azure AD-Aktivitäten in Ihrer Umgebung, die unter anderem Anmeldeereignisse, Überwachungsereignisse und Änderungen an Ihrem Verzeichnis enthalten.

Die bereitgestellten Daten ermöglichen Ihnen Folgendes:

* Ermitteln, wie Ihre Apps und Dienste genutzt werden

* Erkennen potenzieller Risiken für die Integrität Ihrer Umgebung

* Behandeln von Problemen, die Ihre Benutzer an der Erledigung ihrer Arbeit hindern

* Gewinnen von Erkenntnissen durch die Überwachungsereignisse für Änderungen am Azure AD-Verzeichnis

> [!IMPORTANT]
> Mit Azure AD-Überwachung können Sie die von der Azure AD-Berichterstellung generierten Protokolle an verschiedene Zielsysteme weiterleiten. Diese können dann entweder zur langfristigen Verwendung gespeichert oder in SIEM-Drittanbietertools (Security Information & Event Management) integriert werden, um Einblicke in Ihre Umgebung zu gewinnen.

Mit Azure AD-Überwachung können Sie Protokolle folgendermaßen weiterleiten:

* an ein Azure-Speicherkonto zu Archivierungszwecken
* an Azure Monitor-Protokolle (früher Log Analytics-Arbeitsbereich), um die Daten dort zu analysieren, Dashboards zu erstellen und Warnungen für bestimmte Ereignisse auszulösen
* einen Azure Event Hub, in dem die Protokolle mit vorhandenen SIEM-Tools wie Splunk, Sumologic oder QRadar integriert werden können

> [!NOTE]
Seit Kurzem wird der Begriff „Azure Monitor-Protokolle“ anstelle von „Log Analytics“ verwendet. Protokolldaten werden immer noch in einem Log Analytics-Arbeitsbereich gespeichert und weiterhin mit dem gleichen Log Analytics-Dienst erfasst und analysiert. Die Terminologie hat sich geändert, um der Rolle von [Protokollen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection) besser Rechnung zu tragen. Weitere Informationen finden Sie unter [Terminologieänderungen bei Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/azure-monitor-rebrand).

[Weitere Informationen zu Aufbewahrungsrichtlinien für Berichte](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-reports-data-retention)

### <a name="licensing-and-prerequisites-for-azure-ad-reporting-and-monitoring"></a>Lizenzierung und Voraussetzungen für die Azure AD-Berichterstellung und -Überwachung

Sie benötigen eine Azure AD-Premium-Lizenz, um auf die Azure AD-Anmeldeprotokolle zuzugreifen.

Ausführliche Informationen zu Features und Lizenzen finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

Zur Bereitstellung der Azure AD-Überwachung und -Berichterstellung ist ein globaler Administrator oder ein Sicherheitsadministrator für den Azure AD-Mandanten erforderlich.

Abhängig vom endgültigen Ziel ihrer Protokolldaten ist eine der folgenden Voraussetzungen nötig:

* ein Azure-Speicherkonto, für das Sie über ListKeys-Berechtigungen verfügen Wir empfehlen Ihnen, anstelle eines Blobspeicherkontos ein allgemeines Speicherkonto zu verwenden. Preisinformationen zur Speicherung erhalten Sie über den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=storage).

* ein Azure Event Hubs-Namespace, der mit SIEM-Drittanbieterlösungen integriert werden kann

* Einen Azure Log Analytics-Arbeitsbereich, um Protokolle an Azure Monitor-Protokolle zu senden.

## <a name="plan-an-azure-reporting-and-monitoring-deployment-project"></a>Planen eines Bereitstellungsprojekts für die Azure-Berichterstellung und -Überwachung

In diesem Projekt definieren Sie die Zielgruppen, die Berichte nutzen und überwachen, und Ihre Azure AD-Überwachungsarchitektur.

### <a name="engage-the-right-stakeholders"></a>Einbeziehen der richtigen Beteiligten

Fehler in Technologieprojekten sind in der Regel auf nicht erfüllte Erwartungen auf den Gebieten Auswirkungen, Ergebnisse und Zuständigkeiten zurückzuführen. Diese können Sie vermeiden, indem Sie [sicherstellen, dass Sie die richtigen Beteiligten](https://aka.ms/deploymentplans) hinzuziehen. Achten Sie zudem darauf, die Rolle der Beteiligten im Projekt präzise zu definieren, indem Sie die Beteiligten, ihren Projektbeitrag und ihre Zuständigkeiten dokumentieren.

### <a name="plan-communications"></a>Planen der Benachrichtigungen

Kommunikation ist ein kritischer Faktor für den Erfolg jedes neuen Diensts. Kommunizieren Sie proaktiv mit Ihren Benutzern darüber, wie sich die Nutzung ändern wird, wann sie sich ändert und wie sie Unterstützung erhalten, wenn sie auf Probleme stoßen.

### <a name="document-your-current-infrastructure-and-policies"></a>Dokumentieren der aktuellen Infrastruktur und Richtlinien

Ihre aktuelle Infrastruktur und Richtlinien wirken sich auf den Entwurf für die Berichterstellung und Überwachung aus. Sie sollten deshalb über Folgendes Bescheid wissen:

* welche SIEM-Tools Sie einsetzen (falls zutreffend)

* Ihre Azure-Infrastruktur, einschließlich der vorhandenen Speicherkonten und der eingesetzten Überwachung

* die Beibehaltungsrichtlinien Ihrer Organisation für Protokolle, einschließlich der erforderlichen geltenden Compliancevorgaben. 

## <a name="plan-an-azure-ad-reporting-and-monitoring-deployment"></a>Planen einer Bereitstellung für eine Azure AD-Berichterstellung und -Überwachung

Die Berichterstellung und Überwachung werden verwendet, um Ihre geschäftlichen Anforderungen zu erfüllen, Einblicke in die Verwendungsmuster zu gewinnen und den Sicherheitsstatus Ihrer Organisation zu verbessern.

### <a name="business-use-cases"></a>Anwendungsfälle für Unternehmen

* Notwendig, damit die Lösung geschäftliche Anforderungen erfüllt
* Geeignet, um geschäftliche Anforderungen zu erfüllen
* Nicht zutreffend

|Bereich |BESCHREIBUNG |
|-|-|
|Aufbewahrung| **Aufbewahrungsdauer für Protokolle von über 30 Tagen:** ‎Aufgrund gesetzlicher oder geschäftlicher Anforderungen ist es erforderlich, Überwachungsprotokolle und Anmeldeprotokolle von Azure AD länger als 30 Tage zu speichern. |
|Analytics| **Protokolle müssen durchsuchbar sein:** ‎Die gespeicherten Protokolle müssen mit Analysetools durchsuchbar sein. |
| Operational Insights| **Einblicke für verschiedene Teams:** Der Zugriff für verschiedene Benutzer ist erforderlich, um betriebliche Einblicke wie Anwendungsnutzung, Anmeldefehler, Self-Service-Nutzung und Trends zu erhalten. |
| Einblicke in die Sicherheit| **Einblicke für verschiedene Teams:** Der Zugriff für verschiedene Benutzer ist erforderlich, um betriebliche Einblicke wie Anwendungsnutzung, Anmeldefehler, Self-Service-Nutzung und Trends zu erhalten. |
| Integration in SIEM-Systeme      | **SIEM-Integration:** ‎Wird benötigt, wenn Azure AD-Anmeldeprotokolle und -Überwachungsprotokolle in vorhandene SIEM-Systeme integriert und gestreamt werden sollen. |

### <a name="choose-a-monitoring-solution-architecture"></a>Auswahl einer Lösungsarchitektur für die Überwachung

Mit der Azure AD-Überwachung können Sie Ihre Azure AD-Aktivitätsprotokolle an ein System weiterleiten, das Ihren Geschäftsanforderungen am besten entspricht. Sie können diese dann für die langfristige Berichterstellung und Analyse aufbewahren, um Einblicke in Ihre Umgebung zu erhalten und sie in SIEM-Tools zu integrieren.

#### <a name="decision-flow-chartan-image-showing-what-is-described-in-subsequent-sectionsmediareporting-deployment-plandeploy-reporting-flow-diagrampng"></a>Flussdiagramm zum Entscheidungsprozess![Bild: Ablauf in aufeinanderfolgenden Abschnitten](media/reporting-deployment-plan/deploy-reporting-flow-diagram.png)

#### <a name="archive-logs-in-a-storage-account"></a>Archivieren von Protokollen in einem Speicherkonto

Wenn Sie Protokolle an ein Azure-Speicherkonto weiterleiten, können Sie sie länger aufbewahren als die Standardaufbewahrungsdauer aus unseren [Aufbewahrungsrichtlinien](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-reports-data-retention) vorgibt. Verwenden Sie diese Methode, wenn Sie Ihre Protokolle archivieren müssen, sie aber nicht mit einem SIEM-System integriert werden müssen und keine laufenden Abfragen und Analysen benötigt werden. Sie können weiterhin Suchvorgänge nach Bedarf durchführen.

Informationen zum Weiterleiten von Daten an Ihr Speicherkonto finden Sie [hier](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account).

#### <a name="send-logs-to-azure-monitor-logs"></a>Senden von Protokollen an Azure Monitor-Protokolle

[Azure Monitor-Protokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) konsolidiert Überwachungsdaten aus verschiedenen Quellen. Zudem sind eine Abfragesprache und eine Analyse-Engine vorhanden, die Ihnen Einblicke in den Betrieb Ihrer Anwendungen und die Nutzung Ihrer Ressourcen geben. Sie können gesammelte Daten schnell abrufen, überwachen und für Warnungen heranziehen, indem Sie Ihre Azure AD-Aktivitätsprotokolle an Azure Monitor-Protokolle senden. Verwenden Sie diese Methode, wenn Sie nicht über eine vorhandene SIEM-Lösung verfügen, an die Sie Ihre Daten direkt senden möchten, aber Abfragen und Analysen ausführen möchten. Sobald Ihre Daten sich in Azure Monitor-Protokollen befinden, können Sie diese an den Event Hub und von dort aus bei Bedarf an SIEM senden.

Lesen Sie, wie Sie [Daten an Azure Monitor-Protokolle senden](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics).

Sie können auch die vordefinierte Ansichten für Azure AD-Aktivitätsprotokolle installieren, um allgemeine Szenarios mit Anmeldungen und Überprüfungsereignissen zu überwachen.

Informationen zum Installieren und Verwenden von Log Analytics-Ansichten für Azure AD-Aktivitätsprotokolle finden Sie [hier](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views).

#### <a name="stream-logs-to-your-azure-event-hub"></a>Streamen von Protokollen an Ihren Azure-Event Hub

Das Weiterleiten von Protokollen an eine Azure-Event Hub ermöglicht die Integration mit SIEM-Drittanbietertools. Diese Integration ermöglicht es Ihnen, Daten des Azure AD-Aktivitätsprotokolls mit anderen Daten zu kombinieren, die von Ihrer SIEM-Lösung verwaltetet werden, um umfassendere Einblicke in Ihre Umgebung zu gewähren. 

Informationen zum Streamen von Protokollen an einen Event Hub finden Sie [hier](https://docs.microsoft.com//azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub).

## <a name="plan-operations-and-security-for-azure-ad-reporting-and-monitoring"></a>Planen von Vorgängen und Sicherheit für die Azure AD-Berichterstellung und -Überwachung

Beteiligte müssen auf Azure AD-Protokolle zugreifen können, um Einblicke in den Betrieb zu erhalten. Das betrifft am wahrscheinlichsten Mitglieder des Sicherheitsteams, interne oder externe Prüfer und das Betriebsteam für die Identitäts- und Zugriffsverwaltung.

Mit Azure AD Rollen können Sie die Möglichkeit zum Konfigurieren und Anzeigen von Azure AD-Berichten auf ihrer Rolle basierend delegieren. Legen Sie fest, wer in Ihrer Organisation die Berechtigung zum Lesen von Azure AD-Berichten benötigt und welche Rolle für diese Benutzer geeignet ist. 

Folgende Rollen können Azure AD-Berichte lesen:

* Globaler Administrator

* Sicherheitsadministrator

* Sicherheitsleseberechtigter

* Report Reader (Leseberechtigter für Berichte)

Im verlinkten Artikel finden Sie weitere Informationen zu [Azure AD-Administratorrollen](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal).

*Wenden Sie immer das Konzept der geringsten Rechte an, um das Risiko einer Kontogefährdung zu verringern*. Sie sollten [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) implementieren, um Ihre Organisation noch besser zu schützen.

##  

## <a name="deploy-azure-ad-reporting-and-monitoring"></a>Bereitstellen der Azure AD-Berichterstellung und -Überwachung

In diesem Abschnitt finden Sie die Dokumentationen zu den unterschiedlichen Bereitstellungsoptionen, die von den Entscheidungen abhängen, die Sie zuvor anhand des Entwurfsleitfadens getroffen haben.

### <a name="consume-and-archive-azure-ad-logs"></a>Nutzen und Archivieren von Azure AD-Protokollen

[Speicherorte von Aktivitätsberichten im Azure-Portal](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-find-activity-reports)

[Verwenden des Power BI-Inhaltspakets für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-power-bi-content-pack)

[Tutorial: Archivieren von Azure AD-Protokollen in einem Azure-Speicherkonto](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account)

### <a name="implement-monitoring-and-analytics"></a>Implementieren von Überwachung und Analyse

[Senden von Protokollen an Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

[Installieren und Verwenden der Log Analytics-Ansichten für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-install-use-log-analytics-views)

[Analysieren von Azure AD-Aktivitätsprotokollen mit Azure Monitor-Protokollen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics)

* [Interpret audit logs schema in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema) (Interpretieren des Überwachungsprotokollschemas in Azure Monitor)

* [Interpretieren des Azure AD-Anmeldeprotokollschemas in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema)

 * [Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub)

* [Integrate Azure AD logs with Splunk by using Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-integrate-activity-logs-with-splunk) (Integrieren von Azure AD-Protokollen in Splunk mithilfe von Azure Monitor)

* [Integrieren von Azure AD-Protokollen in SumoLogic mit Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic)

 

 

## <a name="next-steps"></a>Nächste Schritte

Implementierung von [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) 

Implementierung der [rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/overview)

 
