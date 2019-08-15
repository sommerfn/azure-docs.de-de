---
title: Azure-Protokollierung und -Überwachung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie auf der Grundlage von Protokollierungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.openlocfilehash: 80f90f1788e798261f77bb7a4147763e7ca6cec0
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946505"
---
# <a name="azure-logging-and-auditing"></a>Azure-Protokollierung und -Überwachung

Azure bietet eine breite Palette an konfigurierbaren Optionen für die Sicherheitsüberwachung und -Protokollierung, damit Sie Lücken in Ihren Sicherheitsrichtlinien und -mechanismen bestimmen können. Dieser Artikel erläutert das Generieren, Sammeln und Analysieren von Sicherheitsprotokollen aus Diensten, die in Azure gehostet werden.

> [!Note]
> Bestimmte Empfehlungen in diesem Artikel können zu einer erhöhten Nutzung von Daten, Netzwerken oder Computeressourcen sowie zu höheren Lizenz- oder Abonnementkosten führen.

## <a name="types-of-logs-in-azure"></a>Protokolltypen in Azure

Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Protokolle liefern Daten, mit denen Sie den Betrieb Ihrer Anwendungen gewährleisten können. Mit Protokollen können Sie vergangene Probleme beheben oder potenzielle verhindern. Darüber hinaus können Sie damit die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

Azure-Protokolle werden in der folgenden Typen kategorisiert:
* **Steuerungs-/Verwaltungsprotokolle** enthalten Informationen zu CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager. Weitere Informationen finden Sie in [Azure-Aktivitätsprotokolle](../../azure-monitor/platform/activity-logs-overview.md).

* **Datenebenenprotokolle** enthalten Informationen über Ereignisse, die bei der Nutzung der Azure-Ressource aufgetreten sind. Beispiele für diesen Protokolltyp sind System-, Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer (VM) sowie die über Azure Monitor konfigurierten [Diagnoseprotokolle](../../azure-monitor/platform/diagnostic-logs-overview.md).

* **Verarbeitete Ereignisse** enthalten Informationen zu analysierten Ereignisse/Warnungen, die in Ihrem Auftrag verarbeitet wurden. Beispiele für diesen Typ sind [Azure Security Center-Warnungen](../../security-center/security-center-managing-and-responding-alerts.md), bei denen [Azure Security Center](../../security-center/security-center-intro.md) Ihr Abonnement verarbeitet und analysiert hat und prägnante Sicherheitswarnungen ausgibt.

Die folgende Tabelle enthält die wichtigsten Protokolltypen, die in Azure verfügbar sind:

| Protokollkategorie | Protokolltyp | Verwendung | Integration |
| ------------ | -------- | ------ | ----------- |
|[Aktivitätsprotokolle](../../azure-monitor/platform/activity-logs-overview.md)|Ereignisse der Steuerungsebene für Azure Resource Manager-Ressourcen|  Bieten Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden.|    REST-API und [Azure Monitor](../../azure-monitor/platform/activity-logs-overview.md)|
|[Azure-Diagnoseprotokolle](../../azure-monitor/platform/diagnostic-logs-overview.md)|Häufige Daten zum Betrieb der Azure Resource Manager-Ressourcen im Abonnement|  Bieten einen Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.| Azure Monitor, [Stream](../../azure-monitor/platform/diagnostic-logs-overview.md)|
|[Azure AD-Berichterstellung](../../active-directory/reports-monitoring/overview-reports.md)|Protokolle und Berichte | Enthält Informationen über Aktivitäten zur Benutzeranmeldung und Systemaktivitäten zur Benutzer- und Gruppenverwaltung.|[Graph-API](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Virtuelle Computer und Clouddienste](../../azure-monitor/learn/quick-collect-azurevm.md)|Windows-Ereignisprotokolldienst und Linux-Syslog|  Erfasst Systemdaten und Protokollierungsdaten auf den virtuellen Computern und überträgt die Daten in ein Speicherkonto Ihrer Wahl.|   Windows mit [WAD](../../monitoring-and-diagnostics/azure-diagnostics.md) (Microsoft Azure-Diagnosespeicher) und Linux in Azure Monitor|
|[Azure-Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Speicherprotokollierung, stellt Metrikdaten für ein Speicherkonto bereit|Bietet einen Einblick in Nachverfolgungsanforderungen und ermöglicht die Analyse von Verwendungstrends sowie die Diagnose von Problemen mit dem Speicherkonto.|   REST-API oder [Clientbibliothek](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Datenflussprotokolle für Netzwerksicherheitsgruppen (NSG)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|JSON-Format, zeigt eingehende und ausgehende Datenflüsse auf Regelbasis|Zeigt Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe an.|[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Application Insight](../../azure-monitor/app/app-insights-overview.md)|Protokolle, Ausnahmen und benutzerdefinierte Diagnosen|  Bietet einen für Webentwickler konzipierten Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.| REST-API, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Prozessdaten/Sicherheitswarnungen|    Azure Security Center-Warnungen, Azure Monitor-Protokollwarnungen|    Bietet Sicherheitsinformationen und Warnungen.|  REST-APIs, JSON|

### <a name="activity-logs"></a>Aktivitätsprotokolle

[Azure-Aktivitätsprotokolle](../../azure-monitor/platform/activity-logs-overview.md) bieten Einblicke in Vorgänge, die für Ressourcen in Ihrem Abonnement ausgeführt wurden. Azure-Aktivitätsprotokolle wurden bisher als „Überwachungsprotokolle“ oder „Betriebsprotokolle“ bezeichnet, da sie Ereignisse der [Steuerungsebene](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) für Ihre Abonnements enthalten. 

Mit Aktivitätsprotokollen können Sie bestimmen, wer, wann Schreibvorgänge ausgelöst hat und was diese bewirkt haben (d. h., PUT, POST oder DELETE). Mit Aktivitätsprotokollen können Sie auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Aktivitätsprotokolle umfassen keine Lesevorgänge (GET).

In diesem Artikel beziehen sich PUT, POST und DELETE auf alle Schreibvorgänge, die das Aktivitätsprotokoll für die Ressourcen enthält. Sie können z. B. bei der Problembehandlung mithilfe der Aktivitätsprotokolle einen Fehler ermitteln oder nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat.

![Aktivitätsprotokolldiagramm](./media/log-audit/azure-log-audit-fig1.png)

Sie können Ereignisse per Azure-Portal, [Azure CLI](../../storage/common/storage-azure-cli.md), PowerShell-Cmdlets und [Azure Monitor-REST-API](../../azure-monitor/platform/rest-api-walkthrough.md) aus dem Aktivitätsprotokoll abrufen. Für Aktivitätsprotokolle gilt eine Datenaufbewahrungsdauer von 90 Tagen.

Integrationsszenarien für ein Aktivitätsprotokollereignis:

* [Erstellen einer E-Mail- oder Webhookwarnung, die von einem Aktivitätsprotokoll ausgelöst wird](../../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md).

* [Streamen an einen Event Hub](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI.

* Analysieren in Power BI mit dem [Power BI-Inhaltspaket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Speichern unter einem Speicherkonto zur Archivierung oder manuellen Untersuchung](../../azure-monitor/platform/archive-activity-log.md). Sie können die Aufbewahrungsdauer (in Tagen) mithilfe von Protokollprofilen angeben.

* Abfragen und Anzeigen des Protokolls im Azure-Portal

* Abfragen per PowerShell-Cmdlet, Azure CLI oder REST-API.

* Exportieren des Aktivitätsprotokolls mit Protokollprofilen in [Azure Monitor-Protokolle](../../log-analytics/log-analytics-queries.md)

Sie können ein Speicherkonto oder [Event Hub-Namespace](../../event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-capture.md) verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle ausgibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden Zugriff gemäß der [rollenbasierten Zugriffssteuerung (RBAC)](../../role-based-access-control/role-assignments-portal.md) auf beide Abonnements.

### <a name="azure-diagnostics-logs"></a>Azure-Diagnoseprotokolle

Azure-Diagnoseprotokolle werden von einer Ressource ausgegeben und stellen umfangreiche, in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. [Windows-Systemereignisprotokolle](../../azure-monitor/platform/data-sources-windows-events.md) sind z. B. eine Kategorie des Diagnoseprotokolls für VMs und [Blob-, Tabellen- und Warteschlangenprotokolle](../../storage/common/storage-monitor-storage-account.md) sind Kategorien der Diagnoseprotokolle für Speicherkonten. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen, die Einblicke in die Vorgänge geben, die von Ressourcen in Ihrem Abonnement ausgeführt wurden.

![Azure-Diagnoseprotokolldiagramme](./media/log-audit/azure-log-audit-fig2.png)

Azure-Diagnoseprotokolle bieten mehrere Konfigurationsoptionen, wie z. B. das Azure-Portal, PowerShell, Azure CLI und die REST-API.

**Integrationsszenarien**

* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [Speicherkonto](../../azure-monitor/platform/archive-diagnostic-logs.md) gespeichert werden. In den Diagnoseeinstellungen können Sie eine Aufbewahrungsdauer (in Tagen) angeben.

* [Sie können Diagnoseprotokolle zur Erfassung](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md) durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/) an Event Hubs streamen.

* Sie können sie mit [Azure Monitor-Protokollen](../../log-analytics/log-analytics-queries.md) analysieren.

**Unterstützten Dienste, Schema für Diagnoseprotokolle und unterstützte Protokollkategorien pro Ressourcentyp**


| Dienst | Schema und Dokumentation | Ressourcentyp | Category (Kategorie) |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Azure Monitor-Protokolle für Load Balancer (Vorschauversion)](../../load-balancer/load-balancer-monitor-log.md)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Netzwerksicherheitsgruppen|[Azure Monitor-Protokolle für Netzwerksicherheitsgruppen](../../virtual-network/virtual-network-nsg-manage-log.md)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Diagnoseprotokollierung für Application Gateway](../../application-gateway/application-gateway-diagnostics.md)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Key Vault-Protokolle](../../key-vault/key-vault-logging.md)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../../search/search-traffic-analytics.md)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Zugreifen auf Diagnoseprotokolle für Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Audit<br>Requests|
|Azure Data Lake Analytics|[Zugreifen auf Diagnoseprotokolle für Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Audit<br>Requests|
|Azure Logic Apps|[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch-Diagnoseprotokolle](../../batch/batch-diagnostics.md)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure-Automatisierung|[Azure Monitor-Protokolle für Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Event Hubs-Diagnoseprotokolle](../../event-hubs/event-hubs-diagnostic-logs.md)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[Auftragsdiagnoseprotokolle](../../stream-analytics/stream-analytics-job-diagnostic-logs.md)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Ausführung<br>Erstellen|
|Azure-Servicebus|[Service Bus-Diagnoseprotokolle](../../service-bus-messaging/service-bus-diagnostic-logs.md)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory-Berichterstellung

Azure Active Directory (Azure AD) umfasst Sicherheits-, Aktivitäts- und Prüfberichte für das Verzeichnis eines Benutzers. Der [Azure AD-Prüfbericht](../../active-directory/active-directory-reporting-azure-portal.md) unterstützt Sie bei der Bestimmung privilegierter Aktionen, die in der Azure AD-Instanz des Benutzers aufgetreten sind. Privilegierte Aktionen umfassen Änderungen zur Rechteerweiterung (z.B. das Erstellen von Rollen oder Zurücksetzen von Kennwörtern), das Ändern von Richtlinienkonfigurationen (z.B. Kennwortrichtlinien) oder Änderungen an der Verzeichniskonfiguration (z.B. Änderungen an Domänenverbundeinstellungen).

Die Berichte enthalten den Überwachungsdatensatz für den Ereignisnamen, den Benutzer, der die Aktion ausgeführt hat, die von der Änderung betroffene Zielressource sowie Datum und Uhrzeit (in UTC). Benutzer können die Liste mit den Überwachungsereignissen für ihre Azure AD-Instanz über das [Azure-Portal](https://portal.azure.com/) abrufen, wie in [Anzeigen von Überwachungsprotokollen](../../active-directory/reports-monitoring/overview-reports.md) beschrieben. 

Die enthaltenen Berichte sind in der folgenden Tabelle aufgeführt:

| Sicherheitsberichte | Aktivitätsberichte | Prüfberichte |
| :--------------- | :--------------- | :------------ |
|Anmeldungen von unbekannten Quellen| Anwendungsnutzung: Zusammenfassung| Verzeichnisprüfbericht|
|Anmeldungen nach mehreren Fehlern|  Anwendungsnutzung: detailliert||
|Anmeldungen aus mehreren geografischen Regionen|    Anwendungsdashboard||
|Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten|   Kontobereitstellungsfehler||
|Irreguläre Anmeldeaktivitäten|    Geräte einzelner Benutzer||
|Anmeldungen von möglicherweise infizierten Geräten|   Aktivität einzelner Benutzer||
|Benutzer mit anomalen Anmeldeaktivitäten| Gruppenaktivitätsbericht||
||Bericht zur Registrierung für die Kennwortzurücksetzung||
||Kennwortzurücksetzungsaktivität||

Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM (Security Information and Event Management)-Systeme, Überwachungs- und Business Intelligence-Tools – nützlich sein. Die Azure AD-Berichterstellungs-APIs bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese [APIs](../../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) über verschiedene Programmiersprachen und Tools aufrufen.

Die Vermerkdauer für Ereignisse im Azure AD-Überwachungsbericht variiert je nach dem Typ der Lizenz, die dem Mandanten zugeordnet ist, zwischen 7 und 90 Tagen. 

> [!Note]
> Weitere Informationen zur Aufbewahrung von Berichten finden Sie unter [Aufbewahrungsrichtlinien für Azure AD-Berichte](../../active-directory/reports-monitoring/reference-reports-data-retention.md).

Wenn Sie die Überwachungsereignisse länger beibehalten möchten, verwenden Sie die Reporting-API, um die [Überwachungsereignisse](../../active-directory/active-directory-reporting-activity-audit-logs.md) regelmäßig in einen separaten Datenspeicher abzurufen.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>VM-Protokolle mit der Azure-Diagnose

Die [Azure-Diagnose](../../monitoring-and-diagnostics/azure-diagnostics.md) ist eine Funktion in Azure, mit der Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von beliebigen Quellen aus verwenden. Derzeit werden [Web- und Workerrollen von Azure Cloud-Dienst](../../cloud-services/cloud-services-choose-me.md) unterstützt.

![VM-Protokolle mit der Azure-Diagnose](./media/log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineslearnpathsdeploy-a-website-with-azure-virtual-machines-that-are-running-microsoft-windows-and-service-fabricservice-fabricservice-fabric-overviewmd"></a>[Azure Virtual Machines](/learn/paths/deploy-a-website-with-azure-virtual-machines/), die unter Microsoft Windows und [Service Fabric](../../service-fabric/service-fabric-overview.md) ausgeführt werden

Sie können die Azure-Diagnose wie folgt auf einem virtuellen Computer aktivieren:

* [Mithilfe von Visual Studio zum Verfolgen von Azure Virtual Machines](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Remoteeinrichtung der Azure-Diagnose auf einer Azure Virtual Machine](../../virtual-machines/virtual-machines-dotnet-diagnostics.md)

* [Verwenden von PowerShell zum Einrichten der Diagnose für Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](../../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Speicheranalyse

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) protokolliert Metrikdaten für ein Speicherkonto und stellt diese bereit. Mit diesen Daten können Sie Anforderungen verfolgen, Verwendungstrends analysieren und Probleme mit dem Speicherkonto diagnostizieren. Die Storage Analytics Protokollierung ist für [ Azure Blob-, Azure Queue- und Azure Table Storage-Dienste](../../storage/common/storage-introduction.md) nicht verfügbar. Storage Analytics protokolliert ausführliche Informationen zu erfolgreichen und nicht erfolgreichen Anforderungen an einen Speicherdienst.

Anhand dieser Informationen können Sie einzelne Anforderungen überwachen und Probleme mit einem Speicherdienst untersuchen. Anforderungen werden auf Grundlage der besten Leistung protokolliert. Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Blob-Endpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Blob-Speicherdienst erstellt.

Zum Verwenden von Storage Analytics müssen Sie es einzeln für jeden zu überwachenden Dienst aktivieren. Die Aktivierung ist im [Azure-Portal](https://portal.azure.com/) möglich. Weitere Informationen finden Sie unter [Überwachen eines Speicherkontos im Azure-Portal](../../storage/common/storage-monitor-storage-account.md). Sie können die Speicheranalyse auch programmgesteuert über die REST-API oder die Clientbibliothek aktivieren. Verwenden Sie den Vorgang „Diensteigenschaften festlegen“, um die Speicheranalyse für jeden Dienst einzeln zu aktivieren.

Die aggregierten Daten werden in einem bekannten Blob (zur Protokollierung) und in bekannten Tabellen (als Metrik) gespeichert. Der Zugriff erfolgt über APIs für den Blob Storage- und den Table Storage-Dienst.

Bei Storage Analytics ist die Menge der gespeicherten Daten auf 20 Terabyte (TB) beschränkt. Diese Beschränkung gilt unabhängig vom Gesamtlimit für Ihr Speicherkonto. Alle Protokolle werden in [Block-Blobs](../../storage/common/storage-analytics.md) in einem Container namens „$logs“ gespeichert, der automatisch erstellt wird, wenn Storage Analytics für ein Speicherkonto aktiviert wird.

> [!Note]
> * Weitere Informationen zu Abrechnungs- und Datenaufbewahrungsrichtlinien finden Sie unter [Storage Analytics und Abrechnung](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Weitere Informationen zu Speicherkontobegrenzungen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/common/storage-scalability-targets.md).

Die folgenden Typen authentifizierter und anonymer Anforderungen werden von Storage Analytics protokolliert:

| Authentifiziert  | Anonym|
| :------------- | :-------------|
| Erfolgreiche Anforderungen | Erfolgreiche Anforderungen |
|Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler | Anforderungen mithilfe einer Shared Access Signature, einschließlich fehlerhafter und erfolgreicher Anforderungen |
| Anforderungen mithilfe einer Shared Access Signature, einschließlich fehlerhafter und erfolgreicher Anforderungen |Timeoutfehler für Client und Server |
|   Anforderungen von Analysedaten |    Mit Fehlercode 304 (nicht geändert) misslungene GET-Anforderungen |
| Anforderungen, die durch die Speicheranalyse selbst erfolgen, z. B. Protokollerstellung oder -löschung, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Storage Analytics-Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. | Alle anderen misslungenen anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Storage Analytics-Vorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Storage Analytics-Protokollformat](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. |

### <a name="azure-networking-logs"></a>Azure-Netzwerkprotokolle

Die Netzwerkprotokollierung und -überwachung in Azure ist umfassend und unterteilt sich in zwei übergeordnete Kategorien:

* [Network Watcher:](../../network-watcher/network-watcher-monitoring-overview.md) Die szenariobasierte Netzwerküberwachung wird über Features in Network Watcher bereitgestellt. Dieser Dienst umfasst die Paketerfassung, „Nächster Hop“, die IP-Datenflussüberprüfung, die Sicherheitsgruppenansicht und NSG-Datenflussprotokolle. Die Überwachung auf Szenarioebene bietet im Gegensatz zur Überwachung einzelner Netzwerkressourcen eine End-to-End-Ansicht der Netzwerkressourcen.

* [Ressourcenüberwachung:](../../network-watcher/network-watcher-monitoring-overview.md) Die Überwachung auf Ressourcenebene umfasst vier Features: Diagnoseprotokolle, Metriken, Problembehandlung und Ressourcenintegrität. Alle diese Features arbeiten auf Netzwerkressourcenebene.

![Azure-Netzwerkprotokolle](./media/log-audit/azure-log-audit-fig4.png)

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

### <a name="network-security-group-flow-logging"></a>Protokollierung des Datenflusses von Netzwerksicherheitsgruppen

[NSG-Flussprotokolle](../../network-watcher/network-watcher-nsg-flow-logging-overview.md) sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine NSG anzeigen können. Diese Flussprotokolle werden im JSON-Format geschrieben und zeigen folgendes an:
* Aus- und eingehende Datenflüsse auf Regelbasis
* Die Netzwerkkarte, auf die sich der Datenfluss bezieht
* 5-Tupel-Informationen über den Datenfluss: Quell- oder Ziel-IP, den Quell- oder Zielport und das Protokoll.
* Ob es sich um zulässigen oder verweigerten Datenverkehr handelte.

Auch wenn sich Flussprotokolle auf NSGs beziehen, werden sie nicht wie andere Protokolle angezeigt. Datenflussprotokolle werden nur innerhalb eines Speicherkontos gespeichert.

Für Flussprotokolle gelten die gleichen Aufbewahrungsrichtlinien wie für andere Protokolle. Die Protokolle verfügen über eine Aufbewahrungsrichtlinie, die auf einen Zeitraum zwischen einem und 365 Tagen festgelegt werden kann. Wenn keine Aufbewahrungsrichtlinie festgelegt wurde, werden die Protokolle unbegrenzt aufbewahrt.

**Diagnoseprotokolle**

Regelmäßige und spontane Ereignisse werden von Netzwerkressourcen erstellt, in Speicherkonten protokolliert und dann an Event Hub oder Azure Monitor-Protokolle gesendet. Diese Protokolle geben Einblick in die Integrität einer Ressource. Sie können in Tools wie Power BI und Azure Monitor-Protokollen angezeigt werden. Informationen zum Anzeigen von Diagnoseprotokollen finden Sie unter [Azure Monitor-Protokolle](../../azure-monitor/insights/azure-networking-analytics.md).

![Diagnoseprotokolle](./media/log-audit/azure-log-audit-fig5.png)

Diagnoseprotokolle sind für [Load Balancer](../../load-balancer/load-balancer-monitor-log.md), [Netzwerksicherheitsgruppen](../../virtual-network/virtual-network-nsg-manage-log.md), Routen und [Application Gateway](../../application-gateway/application-gateway-diagnostics.md) verfügbar.

Network Watcher enthält eine Ansicht für die Diagnoseprotokolle. Diese Ansicht enthält alle Netzwerkressourcen, die die Diagnoseprotokollierung unterstützen. In dieser Ansicht können Sie Netzwerkressourcen einfach und schnell aktivieren und deaktivieren.


Zusätzlich zu den oben aufgeführten Protokollierungsfunktionen umfasst Network Watcher derzeit die folgenden Funktionen:
- [Topologie:](../../network-watcher/view-network-topology.md) Bietet eine Ansicht auf Netzwerkebene mit verschiedenen Verbindungen und Beziehungen zwischen Netzwerkressourcen innerhalb einer Ressourcengruppe.

- [Variable Paketerfassung:](../../network-watcher/network-watcher-packet-capture-overview.md) Erfasst die auf dem virtuellen Computer ein- und ausgehenden Paketdaten. Erweiterte Filteroptionen und präzise Steuerelemente, wie z.B. das Festlegen von Zeit- und Größeneinschränkungen, bieten Flexibilität. Die Paketdaten können in einem Blobspeicher oder auf dem lokalen Datenträger im *CAP-Dateiformat* gespeichert werden.

- [IP-Datenflussüberprüfung:](../../network-watcher/network-watcher-ip-flow-verify-overview.md) Überprüft basierend auf 5-Tupel-Paketparametern (d.h. Ziel-IP, Quell-IP, Zielport, Quellport und Protokoll) anhand der Datenflussinformationen, ob ein Paket zugelassen oder verweigert wird. Wenn das Paket durch eine Sicherheitsgruppe verweigert wird, werden die Namen der Regel und der Gruppe, die das Paket verweigert haben, zurückgegeben.

- [Nächster Hop:](../../network-watcher/network-watcher-next-hop-overview.md) Ermittelt den nächsten Hop für Pakete, die im Azure-Netzwerkfabric weitergeleitet werden, sodass Sie eine Diagnose zur Ermittlung von falsch konfigurierten benutzerdefinierten Routen durchführen können.

- [Sicherheitsgruppenansicht:](../../network-watcher/network-watcher-security-group-view-overview.md) Ruft die geltenden und angewendeten Sicherheitsregeln ab, die auf einen virtuellen Computer angewendet werden.

- [Gateway des virtuellen Netzwerks und Problembehandlung für Verbindungen:](../../network-watcher/network-watcher-troubleshoot-manage-rest.md) Hilft Ihnen bei der Problembehandlung von Gateways für virtuelle Netzwerke und von Verbindungen.

- [Grenzwerte für Netzwerkabonnements:](../../network-watcher/network-watcher-monitoring-overview.md) Ermöglicht die Anzeige der Verwendung von Netzwerkressourcen mit bestimmten Grenzwerten.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) ist ein erweiterbarer, für Webentwickler konzipierter APM-Dienst auf mehreren Plattformen. Verwenden Sie ihn, um aktive Webanwendungen zu überwachen. Der Dienst erkennt automatisch Leistungsanomalien. Er verfügt über leistungsstarke Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von den Benutzern verwendet wird.

Application Insights unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer Anwendung.

Der Dienst eignet sich für Apps auf einer Vielzahl von Plattformen, z.B. .NET, Node.js oder Java EE – lokal gehostet oder in der Cloud. Der Dienst lässt sich in Ihren DevOps-Prozess integrieren und verfügt über Verbindungspunkte mit verschiedenen Entwicklungstools.

![Application Insights-Diagramm](./media/log-audit/azure-log-audit-fig6.png)

Application Insights ist für Entwicklerteams konzipiert und hilft Ihnen dabei, die Leistung und Verwendung Ihrer App nachzuvollziehen. Der Dienst überwacht:

* **Anforderungsraten, Antwortzeiten und Fehlerraten:** Finden Sie heraus, welche Seiten zu welchen Tageszeiten am häufigsten verwendet werden und wo sich Ihre Benutzer befinden. Stellen Sie fest, welche Seiten die beste Leistung aufweisen. Wenn die Antwortzeiten und Fehlerraten bei mehr Anforderungen ansteigen, haben Sie möglicherweise ein Ressourcenproblem.

* **Abhängigkeitsraten, Antwortzeiten und Fehlerraten:** Finden Sie heraus, ob externe Dienste die Geschwindigkeit beeinträchtigen.

* **Ausnahmen:** Analysieren Sie die aggregierten Statistiken, oder wählen Sie bestimmte Instanzen aus, und untersuchen Sie die Stapelüberwachung und die zugehörigen Anforderungen. Sowohl die Server- als auch die Browserausnahmen werden gemeldet.

* **Seitenaufrufe und Ladeleistung:** Rufen Sie Berichte von den Browsern der Benutzer ab.

* **AJAX-Aufrufe:** Erhalten Sie Raten, Antwortzeiten und Fehlerraten von Webseiten.

* **Anzahl von Benutzern und Sitzungen**.

* **Leistungsindikatoren:** Erhalten Sie Daten von Ihren Windows- oder Linux-Servercomputern, z.B. CPU, Arbeitsspeicher und Netzwerknutzung.

* **Hostdiagnose:** Erhalten Sie Daten von Docker oder Azure.

* **Diagnose-Ablaufverfolgungsprotokolle:** Erhalten Sie Daten von Ihrer App, sodass Sie Ablaufverfolgungsereignisse mit Anforderungen korrelieren können.

* **Benutzerdefinierte Ereignisse und Metriken:** Erhalten Sie Daten, die Sie selbst in den Client- oder Servercode schreiben, um Geschäftsereignisse zu verfolgen, z.B. verkaufte Objekte oder gewonnene Spiele.

Die folgende Tabelle enthält eine Liste der Integrationsszenarien und eine entsprechende Beschreibung:

| Integrationsszenario | BESCHREIBUNG |
| --------------------- | :---------- |
|[Anwendungszuordnung](../../azure-monitor/app/app-map.md)|Die Komponenten der App mit wichtigen Metriken und Warnungen.|
|[Diagnosesuche für Instanzdaten](../../azure-monitor/app/diagnostic-search.md)| Suchen und filtern Sie Ereignisse wie Anforderungen, Ausnahmen, Abhängigkeitsaufrufe, Protokollablaufverfolgungen und Seitenaufrufe.|
|[Metrik-Explorer für aggregierte Daten](../../azure-monitor/app/metrics-explorer.md)|Durchsuchen, filtern und segmentieren Sie aggregierte Daten wie z.B. Anforderungs-, Fehler- und Ausnahmeraten, Antwortzeiten und Seitenladezeiten.|
|[Dashboards](../../azure-monitor/app/overview-dashboard.md)|Kombinieren Sie Daten aus mehreren Ressourcen, und geben Sie sie für andere frei. Dies ist sehr gut für Anwendungen mit mehreren Komponenten und für die kontinuierliche Anzeige im Teamraum geeignet.|
|[Live Metrics Stream](../../azure-monitor/app/live-stream.md)|Wenn Sie einen neuen Build bereitstellen, sehen Sie sich diese beinahe in Echtzeit verfügbaren Leistungsindikatoren an, um sicherzustellen, dass alles wie erwartet funktioniert.|
|[Analyse](../../azure-monitor/app/analytics.md)|Beantworten Sie schwierige Fragen zur Leistung und Nutzung Ihrer App mithilfe dieser leistungsstarken Abfragesprache.|
|[Automatische und manuelle Warnungen](../../azure-monitor/app/alerts.md)|Automatische Warnungen sind an die normalen Telemetriemuster Ihrer App angepasst und werden ausgelöst, wenn etwas nicht den üblichen Mustern entspricht. Sie können auch Warnungen auf bestimmten Ebenen von benutzerdefinierten oder standardmäßigen Metriken festlegen.|
|[Visual Studio](../../azure-monitor/app/visual-studio.md)|Zeigen Sie Leistungsdaten im Code an. Wechseln Sie von Stapelüberwachungen zum Code.|
|[Power BI](../../azure-monitor/app/export-power-bi.md)|Integrieren Sie Nutzungsmetriken und andere Business Intelligence-Daten.|
|[REST-API](https://dev.applicationinsights.io/)|Schreiben Sie Code zum Ausführen von Abfragen für Ihre Metriken und Rohdaten.|
|[Fortlaufendem Export](../../azure-monitor/app/export-telemetry.md)|Exportieren Sie große Mengen von Rohdaten in den Speicher, wenn sie eintreffen.|

### <a name="azure-security-center-alerts"></a>Azure Security Center-Warnungen

Bei der Bedrohungserkennung von Azure Security Center werden automatisch Sicherheitsinformationen von Ihren Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen gesammelt. Diese Informationen, bei denen es sich häufig um korrelierende Informationen aus mehreren Quellen handelt, werden analysiert, um Bedrohungen zu identifizieren. Sicherheitswarnungen werden in Security Center zusammen mit Empfehlungen zur Lösung der Bedrohung priorisiert. Weitere Informationen finden Sie unter [Azure Security Center](../../security-center/security-center-intro.md).

![Azure Security Center-Diagramm](./media/log-audit/azure-log-audit-fig7.png)

Für Security Center werden professionelle Sicherheitsanalysen genutzt, die weit über signaturbasierte Ansätze hinausgehen. Es werden Durchbrüche in umfangreichen Daten und [Machine Learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/)-Technologien angewendet, um Ereignisse über die gesamte Cloudfabric auswerten. Auf diese Weise werden Bedrohungen erkannt, die mit manuellen Ansätzen und der Vorhersage zur Entwicklung von Angriffen nicht identifiziert werden können. Zu diesen Sicherheitsanalysen gehört Folgendes:

* **Integrierte Informationen zu Bedrohungen:** Es wird nach bekannten Angreifern gesucht, indem globale Bedrohungsinformationen (Threat Intelligence) von Microsoft-Produkten und -Diensten, von der Microsoft Digital Crimes Unit (DCU), vom Microsoft Security Response Center (MSRC) sowie von externen Feeds angewandt werden.

* **Verhaltensanalyse:** Es werden bekannte Muster angewandt, um schädliches Verhalten zu erkennen.

* **Anomalieerkennung:** Es werden statistische Profile erstellt, um typische Verlaufsdaten zu erhalten. Sie werden benachrichtigt, wenn es zu Abweichungen von der Baseline kommt, die einem potenziellen Angriffsvektor entsprechen.

Viele für den Sicherheitsbetrieb und die Bearbeitung von Incidents zuständige Teams nutzen eine SIEM-Lösung als Ausgangspunkt für die Selektierung und Untersuchung von Sicherheitswarnungen. Mit Azure Log Integration können Sie Security Center-Warnungen sowie in Azure-Diagnoseprotokollen und -Überwachungsprotokollen gesammelte Sicherheitsereignisse virtueller Computer nahezu in Echtzeit mit Azure Monitor-Protokollen oder Ihrer SIEM-Lösung synchronisieren.

## <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle

Azure Monitor-Protokolle sind ein Dienst in Azure, mit dem Sie Daten sammeln und analysieren können, die von Ressourcen in Ihren cloudbasierten und lokalen Umgebungen generiert werden. Der Dienst bietet Echtzeiteinblicke mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards, sodass Sie Millionen von Datensätzen über all Ihre Workloads und Server hinweg unabhängig vom physischen Standort analysieren können.

![Diagramm für Azure Monitor-Protokolle](./media/log-audit/azure-log-audit-fig8.png)

Im Mittelpunkt von Azure Monitor-Protokollen steht der Log Analytics-Arbeitsbereich, der in Azure gehostet wird. Azure Monitor-Protokolle konfigurieren Datenquellen und fügen Ihrem Abonnement Lösungen hinzu, um Daten aus verbundenen Quellen im Arbeitsbereich zu sammeln. Die Datenquellen und Lösungen erstellen jeweils verschiedene Datensatztypen mit individuellen Eigenschaften. Aber Quellen und -Lösungen können weiterhin zusammen im Rahmen von Abfragen an den Arbeitsbereich analysiert werden. Durch diese Funktion können Sie die gleichen Tools und Methoden für die Arbeit mit verschiedene Arten von Daten verwenden, die aus unterschiedlichen Quellen gesammelt wurden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Bei den verbundenen Quellen handelt es sich um die Computer und anderen Ressourcen, die die Daten generieren, die von Azure Monitor-Protokollen gesammelt wurden. Zu den Quellen können Agents gehören, die auf direkt verbundenen [Windows](../../log-analytics/log-analytics-agent-windows.md)- oder [Linux](../../log-analytics/log-analytics-quick-collect-linux-computer.md)-Computern installiert sind, oder Agents in einer [verbundenen System Center Operations Manager-Verwaltungsgruppe](../../azure-monitor/platform/om-agents.md). Azure Monitor-Protokolle können auch Daten aus [Azure-Speicherkonten](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md) sammeln.

[Datenquellen](../../azure-monitor/platform/agent-data-sources.md) sind die verschiedenen Arten von Daten, die aus jeder verbundenen Quelle gesammelt werden. Hierzu zählen Ereignisse und [Leistungsdaten](../../azure-monitor/platform/data-sources-performance-counters.md) aus [Windows](../../azure-monitor/platform/data-sources-windows-events.md)- und Linux-Agents sowie Quellen wie [IIS-Protokolle](../../azure-monitor/platform/data-sources-iis-logs.md) und [benutzerdefinierte Textprotokolle](../../azure-monitor/platform/data-sources-custom-logs.md). Sie konfigurieren jede Datenquelle, aus der Sie Daten sammeln möchten, und die Konfiguration wird automatisch an jede verbundene Quelle übermittelt.

Es gibt vier Möglichkeiten, [Protokolle und Metriken für Azure-Dienste](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md) zu erfassen:

* Azure-Diagnosen direkt an Azure Monitor-Protokolle (**Diagnose** in der folgenden Tabelle)

* Azure-Diagnosen über Azure Storage an Azure Monitor-Protokolle (**Storage** in der folgenden Tabelle)

* Connectors für Azure-Dienste (**Connectors** in der folgenden Tabelle)

* Skripts zum Sammeln und anschließenden Veröffentlichen von Daten in Azure Monitor-Protokollen (leere Zellen in der folgenden Tabelle und für nicht aufgeführte Dienste)

| Dienst | Ressourcentyp | Protokolle | metrics | Lösung |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft.Network/<br>applicationGateways|  Diagnose|Diagnose|    [Azure Application](../../azure-monitor/insights/azure-networking-analytics.md)[Gateway-Analyse](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor)|
|Application Insights||     Connector|  Connector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)-[Connector (Vorschau)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Automation-Kontos| Microsoft.Automation/<br>AutomationAccounts|    Diagnose||       [Weitere Informationen](../../automation/automation-manage-send-joblogs-log-analytics.md)|
|Azure Batch-Kontos|  Microsoft.Batch/<br>batchAccounts|  Diagnose|    Diagnose||
|Klassische Clouddienste||       Storage||       [Weitere Informationen](../../azure-monitor/platform/azure-storage-iis-table.md)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|       Diagnose|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnose|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Diagnose|||
|Azure Event Hub-Namespace| Microsoft.EventHub/<br>Namespaces|  Diagnose|    Diagnose||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Diagnose||
|Azure Key Vault|   Microsoft.KeyVault/<br>Tresore|  Diagnose  || [Key Vault-Analysen](../../azure-monitor/insights/azure-key-vault.md)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Diagnose|||
|Azure Logic Apps|  Microsoft.Logic/<br>Workflows|  Diagnose|    Diagnose||
||Microsoft.Logic/<br>integrationAccounts||||
|Netzwerksicherheitsgruppen|   Microsoft.Network/<br>networksecuritygroups|Diagnose||   [Azure-Netzwerksicherheitsgruppen-Analyse](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-and-network-security-group-analytics)|
|Recovery-Tresore|   Microsoft.RecoveryServices/<br>vaults|||[Azure Recovery Services Analytics (Vorschau)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Suchdienste|   Microsoft.Search/<br>searchServices|    Diagnose|    Diagnose||
|Service Bus-Namespace| Microsoft.ServiceBus/<br>namespaces|    Diagnose|Diagnose|    [Service Bus Analytics (Vorschau)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric-Analysen (Vorschau)](../../service-fabric/service-fabric-diagnostics-oms-setup.md)|
|SQL (v12)| Microsoft.Sql/<br>servers/<br>databases||       Diagnose||
||Microsoft.Sql/<br>servers/<br>elasticPools||||
|Storage|||         Skript| [Azure Storage Analytics (Vorschau)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure Virtual Machines|    Microsoft.Compute/<br>virtualMachines|  Durchwahl|  Durchwahl||
||||Diagnose||
|VM-Skalierungsgruppen|    Microsoft.Compute/<br>virtualMachines    ||Diagnose||
||Microsoft.Compute/<br>virtualMachineScaleSets/<br>virtualMachines||||
|Webserverfarmen|Microsoft.Web/<br>serverfarms||   Diagnose
|Websites|  Microsoft.Web/<br>sites ||      Diagnose|    [Weitere Informationen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites/<br>slots||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Protokollintegration mit lokalen SIEM-Systemen

Mit Azure Log Integration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren. AzLog-Downloads wurden am 27. Juni 2018 deaktiviert. Um Unterstützung bei der künftigen Vorgehensweise zu erhalten, lesen Sie den Beitrag [Use Azure Monitor to integrate with SIEM tools](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) (Verwenden von Azure Monitor für die Integration mit SIEM-Tools).

![Log Integration-Diagramm](./media/log-audit/azure-log-audit-fig9.png)

Log Integration erfasst Azure-Diagnosen von Ihren virtuellen Windows-Computern, Azure-Aktivitätsprotokollen, Azure Security Center-Warnungen und Azure-Ressourcenanbieterprotokollen. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Assets – ganz gleich ob lokal oder in der Cloud – sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

Log Integration unterstützt derzeit die Integration von Azure-Aktivitätsprotokollen, Windows-Ereignisprotokollen vom virtuellen Computer unter Windows in Ihr Azure-Abonnement, Azure Security Center-Warnungen, Azure-Diagnoseprotokolle sowie Azure AD-Überwachungsprotokolle.

| Protokolltyp | Azure Monitor-Protokolle unterstützen JSON (Splunk, ArcSight und IBM Qradar) |
| :------- | :-------------------------------------------------------- |
|Azure AD-Überwachungsprotokolle|   Ja|
|Aktivitätsprotokolle| Ja|
|Security Center-Warnungen |Ja|
|Diagnoseprotokolle (Ressourcenprotokolle)|  Ja|
|VM-Protokolle|   Ja, über weitergeleitete Ereignisse und nicht über JSON|

[Erste Schritte mit Azure Log Integration:](azure-log-integration-get-started.md) In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie Azure Log Integration installieren und Protokolle aus Azure Storage sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure AD-Überwachungsprotokolle integrieren.

Integrationsszenarien für SIEM:

* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie Azure Log Integration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.

* [Häufig gestellte Fragen zu Azure Log Integration:](azure-log-integration-faq.md) In diesem Artikel werden Fragen zur Azure-Protokollintegration beantwortet.

* [Integrieren von Security Center-Warnungen mithilfe von Azure Log Integration:](../../security-center/security-center-export-data-to-siem.md) In diesem Artikel erfahren Sie, wie Sie Azure Security Center-Warnungen sowie in Azure-Diagnoseprotokollen und -Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Azure Monitor-Protokollen oder Ihrer SIEM-Lösung synchronisieren.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachung und Protokollierung:](management-monitoring-overview.md) Schützen Sie Daten durch Sichtbarkeit und eine schnelle Reaktion auf frühzeitige Sicherheitswarnhinweise.

- [Sicherheitsprotokollierung und Überwachungprotokollsammlung in Azure:](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/) Erzwingen Sie diese Einstellungen, um sicherzustellen, dass Ihre Azure-Instanzen die richtigen Sicherheits- und Überwachungsprotokolle sammeln.

- [Konfigurieren von Überwachungseinstellungen für eine Websitesammlung:](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US) Wenn Sie Administrator einer Websitesammlung sind, können Sie den Verlauf der Aktionen für einzelne Benutzer und den Verlauf von Aktionen, die während eines bestimmten Zeitraums ausgeführt wurden, abrufen. 

- [Durchsuchen des Überwachungsprotokolls im Office 365 Security & Compliance Center:](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US) Durchsuchen Sie mit dem Office 365 Security & Compliance Center das vereinheitlichte Überwachungsprotokoll, um Benutzer- und Administratoraktivitäten in Ihrer Office 365-Organisation anzuzeigen.


