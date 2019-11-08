---
title: Azure-Sicherheitsprotokollierung und -Überwachung | Microsoft-Dokumentation
description: Informieren Sie sich über die in Azure verfügbaren Protokolle und die Einblicke in die Sicherheit, die Sie erhalten können.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2019
ms.author: terrylan
ms.openlocfilehash: f53c7e7b045805a28223fb5670afc45c980454bc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498398"
---
# <a name="azure-security-logging-and-auditing"></a>Azure-Sicherheitsprotokollierung und -Überwachung

Azure bietet eine breite Palette an konfigurierbaren Optionen für die Sicherheitsüberwachung und -Protokollierung, damit Sie Lücken in Ihren Sicherheitsrichtlinien und -mechanismen bestimmen können. Dieser Artikel erläutert das Generieren, Sammeln und Analysieren von Sicherheitsprotokollen aus Diensten, die in Azure gehostet werden.

> [!Note]
> Bestimmte Empfehlungen in diesem Artikel können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen sowie zu höheren Lizenz- oder Abonnementkosten führen.

## <a name="types-of-logs-in-azure"></a>Protokolltypen in Azure

Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Protokollierung von Daten kann Ihnen Erkenntnisse über Ihre Anwendungen bieten und Ihnen in folgenden Punkten helfen:

- Beheben aufgetretener Probleme oder verhindern potenzieller neuer Probleme
- Verbessern der Anwendungsleistung oder Wartbarkeit
- Automatisieren von Aktionen, die andernfalls manuellen Eingriff erfordern würden

Azure-Protokolle werden in der folgenden Typen kategorisiert:
* **Steuerungs-/Verwaltungsprotokolle** enthalten Informationen zu CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager. Weitere Informationen finden Sie in [Azure-Aktivitätsprotokolle](../../azure-monitor/platform/activity-logs-overview.md).

* **Datenebenenprotokolle** enthalten Informationen über Ereignisse, die bei der Nutzung der Azure-Ressource aufgetreten sind. Beispiele für diesen Protokolltyp sind System-, Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer (VM) sowie die über Azure Monitor konfigurierten [Diagnoseprotokolle](../../azure-monitor/platform/resource-logs-overview.md).

* **Verarbeitete Ereignisse** enthalten Informationen zu analysierten Ereignisse/Warnungen, die in Ihrem Auftrag verarbeitet wurden. Beispiele für diesen Typ sind [Azure Security Center-Warnungen](../../security-center/security-center-managing-and-responding-alerts.md), bei denen [Azure Security Center](../../security-center/security-center-intro.md) Ihr Abonnement verarbeitet und analysiert hat und prägnante Sicherheitswarnungen ausgibt.

Die folgende Tabelle enthält die wichtigsten Protokolltypen, die in Azure verfügbar sind:

| Protokollkategorie | Protokolltyp | Verwendung | Integration |
| ------------ | -------- | ------ | ----------- |
|[Aktivitätsprotokolle](../../azure-monitor/platform/activity-logs-overview.md)|Ereignisse der Steuerungsebene für Azure Resource Manager-Ressourcen|  Bieten Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden.|    REST-API und [Azure Monitor](../../azure-monitor/platform/activity-logs-overview.md)|
|[Azure-Ressourcenprotokolle](../../azure-monitor/platform/resource-logs-overview.md)|Häufige Daten zum Betrieb der Azure Resource Manager-Ressourcen im Abonnement|   Bieten einen Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.| Azure Monitor|
|[Azure Active Directory-Berichterstellung](../../active-directory/reports-monitoring/overview-reports.md)|Protokolle und Berichte | Enthält Informationen über Aktivitäten zur Benutzeranmeldung und Systemaktivitäten zur Benutzer- und Gruppenverwaltung.|[Graph-API](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Virtuelle Computer und Clouddienste](../../azure-monitor/learn/quick-collect-azurevm.md)|Windows-Ereignisprotokolldienst und Linux-Syslog|  Erfasst Systemdaten und Protokollierungsdaten auf den virtuellen Computern und überträgt die Daten in ein Speicherkonto Ihrer Wahl.|   Windows mit [WAD](../../monitoring-and-diagnostics/azure-diagnostics.md) (Microsoft Azure-Diagnosespeicher) und Linux in Azure Monitor|
|[Azure-Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Speicherprotokollierung, stellt Metrikdaten für ein Speicherkonto bereit|Bietet einen Einblick in Nachverfolgungsanforderungen und ermöglicht die Analyse von Verwendungstrends sowie die Diagnose von Problemen mit dem Speicherkonto.|   REST-API oder [Clientbibliothek](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[NSG-Flussprotokolle (Netzwerksicherheitsgruppe)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|JSON-Format, zeigt eingehende und ausgehende Datenflüsse auf Regelbasis|Zeigt Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe an.|[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Application Insight](../../azure-monitor/app/app-insights-overview.md)|Protokolle, Ausnahmen und benutzerdefinierte Diagnosen|  Bietet einen für Webentwickler konzipierten Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.| REST-API, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|[Prozessdaten/Sicherheitswarnungen](../../security-center/security-center-intro.md)|  Azure Security Center-Warnungen, Azure Monitor-Protokollwarnungen|    Bietet Sicherheitsinformationen und Warnungen.|  REST-APIs, JSON|

## <a name="log-integration-with-on-premises-siem-systems"></a>Protokollintegration mit lokalen SIEM-Systemen
In dem Artikel [Integrieren von Sicherheitslösungen in Azure Security Center](../../security-center/security-center-export-data-to-siem.md) erfahren Sie, wie Sie Azure Security Center-Warnungen sowie in Azure-Diagnoseprotokollen und -Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Azure Monitor-Protokollen oder Ihrer SIEM-Lösung synchronisieren.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachung und Protokollierung:](management-monitoring-overview.md) Schützen Sie Daten durch Sichtbarkeit und eine schnelle Reaktion auf frühzeitige Sicherheitswarnhinweise.

- [Sicherheitsprotokollierung und Überwachungprotokollsammlung in Azure:](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/) Erzwingen Sie diese Einstellungen, um sicherzustellen, dass Ihre Azure-Instanzen die richtigen Sicherheits- und Überwachungsprotokolle sammeln.

- [Konfigurieren von Überwachungseinstellungen für eine Websitesammlung:](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US) Wenn Sie Administrator einer Websitesammlung sind, können Sie den Verlauf der Aktionen für einzelne Benutzer und den Verlauf von Aktionen, die während eines bestimmten Zeitraums ausgeführt wurden, abrufen.

- [Durchsuchen des Überwachungsprotokolls im Office 365 Security & Compliance Center:](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US) Durchsuchen Sie mit dem Office 365 Security & Compliance Center das vereinheitlichte Überwachungsprotokoll, um Benutzer- und Administratoraktivitäten in Ihrer Office 365-Organisation anzuzeigen.
