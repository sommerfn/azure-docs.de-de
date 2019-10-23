---
title: Übersicht über Azure-Ressourcenprotokolle | Microsoft-Dokumentation
description: Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Ressourcenprotokolle.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: rboucher
ms.author: robb
ms.date: 09/20/2019
ms.openlocfilehash: a418e3d1a59379284422d1d24c1457ab61d84a4c
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72551755"
---
# <a name="azure-resource-logs-overview"></a>Übersicht über Azure-Ressourcenprotokolle
Azure-Ressourcenprotokolle sind [Plattformprotokolle](platform-logs-overview.md), die von Azure-Ressourcen ausgegeben werden und deren internen Betrieb beschreiben. Alle Ressourcenprotokolle nutzen ein gemeinsames Schema der obersten Ebene. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben.

> [!NOTE]
> Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.

## <a name="collecting-resource-logs"></a>Erfassen von Ressourcenprotokollen
Ressourcenprotokolle werden von unterstützten Azure-Ressourcen automatisch generiert, jedoch nur dann erfasst, wenn Sie diese mithilfe einer [Diagnoseeinstellung](diagnostic-settings.md) konfigurieren. Erstellen Sie eine Diagnoseeinstellung für jede Azure-Ressource, um die Protokolle an die folgenden Ziele weiterzuleiten:

| Destination | Szenario |
|:---|:---|:---|
| [Log Analytics-Arbeitsbereich](resource-logs-collect-storage.md) | Analysieren Sie die Protokolle mit anderen Überwachungsdaten, und nutzen Sie Azure Monitor-Features wie Protokollabfragen und Protokollwarnungen. |
| [Azure Storage](archive-diagnostic-logs.md) | Archivieren Sie die Protokolle zur Überwachung oder Sicherung. |
| [Event Hub](resource-logs-stream-event-hubs.md) | Streamen Sie die Protokolle an Protokollierungs- und Telemetriesysteme von Drittanbietern.  |

## <a name="compute-resources"></a>Computeressourcen
Ressourcenprotokolle unterscheiden sich von Protokollen auf Gastbetriebssystemebene auf Azure-Computeressourcen. Computeressourcen erfordern, dass ein Agent Protokolle und Metriken vom jeweiligen Gastbetriebssystem erfasst, einschließlich solcher Daten wie Ereignisprotokolle, Syslog und Leistungsindikatoren. Verwenden Sie die [Diagnoseerweiterung](agents-overview.md#azure-diagnostic-extension), um Protokolldaten von virtuellen Azure-Computern und dem [Log Analytics-Agent](agents-overview.md#log-analytics-agent) weiterzuleiten und so Protokolle und Metriken von virtuellen Computern in Azure, in anderen Clouds und lokal in einem Log Analytics Arbeitsbereich zu erfassen. Ausführliche Informationen finden Sie unter [Quellen für Überwachungsdaten für Azure Monitor](data-sources.md).

## <a name="resource-logs-schema"></a>Ressourcenprotokollschema
Jeder Azure-Dienst verfügt über ein eigenes Schema, wenn Ressourcenprotokolle an eines der Ziele geschrieben werden, aber alle haben das Schema der obersten Ebene in der folgenden Tabelle gemeinsam. Links zum Schema für die einzelnen Dienste finden Sie unter [Dienstspezifische Schemas](#service-specific-schemas) weiter unten. 

| NAME | Erforderlich/Optional | BESCHREIBUNG |
|---|---|---|
| time | Erforderlich | Der Zeitstempel (UTC) des Ereignisses. |
| resourceId | Erforderlich | Die Ressourcen-ID der Ressource, die das Ereignis ausgegeben hat. Für Mandantendienste sieht das Format folgendermaßen aus: /tenants/tenant-id/providers/provider-name. |
| tenantId | Erforderlich für Mandantenprotokolle | Die Mandanten-ID des Active Directory-Mandanten, mit dem dieses Ereignis verknüpft ist. Diese Eigenschaft wird nur für Protokolle auf Mandantenebene verwendet. Sie erscheint nicht in Protokollen auf Ressourcenebene. |
| operationName | Erforderlich | Der Name des Vorgangs, für den dieses Ereignis steht. Wenn das Ereignis für einen RBAC-Vorgang steht, ist dies der RBAC-Vorgangsname (z.B. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Ist normalerweise in Form eines Resource Manager-Vorgangs modelliert, auch wenn es sich nicht tatsächlich um dokumentierte Resource Manager-Vorgänge handelt (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`). |
| operationVersion | Optional | Die API-Version, die dem Vorgang zugeordnet ist, wenn für „operationName“ eine API verwendet wurde (z.B. `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Wenn keine API für diesen Vorgang vorhanden ist, entspricht die Version der Version dieses Vorgangs für den Fall, dass sich die dem Vorgang zugeordneten Eigenschaften in Zukunft ändern. |
| category | Erforderlich | Die Protokollkategorie des Ereignisses. „category“ ist die Granularität, mit der Sie Protokolle für eine bestimmte Ressource aktivieren oder deaktivieren können. Die Eigenschaften, die im Eigenschaftenblob eines Ereignisses angezeigt werden, sind für eine bestimmte Protokollkategorie und einen Ressourcentyp gleich. Häufige Protokollkategorien sind „Audit“, „Operational“, „Execution“ und „Request“ (Überwachung, Betrieb, Ausführung, Anforderung). |
| resultType | Optional | Der Status des Ereignisses. Häufige Werte sind „Started“, „In Progress“, „Succeeded“, „Failed“, „Active“ und „Resolved“ (Gestartet, In Bearbeitung, Erfolgreich, Fehler, Aktiv, Gelöst). |
| resultSignature | Optional | Der Unterstatus des Ereignisses. Wenn dieser Vorgang einem REST-API-Aufruf entspricht, ist dies der HTTP-Statuscode des entsprechenden REST-Aufrufs. |
| resultDescription | Optional | Die statische Textbeschreibung dieses Vorgangs, z.B. „Get storage file“ (Speicherdatei abrufen). |
| durationMs | Optional | Die Dauer des Vorgangs in Millisekunden. |
| callerIpAddress | Optional | Die IP-Adresse des Aufrufers, wenn der Vorgang einem API-Aufruf entspricht, der von einer Entität mit einer öffentlich verfügbaren IP-Adresse stammt. |
| correlationId | Optional | Eine GUID, die zum Gruppieren eines Satzes mit verwandten Ereignissen verwendet wird. Wenn zwei Ereignisse über den gleichen „operationName“ verfügen, aber über einen unterschiedlichen Status (z.B. „Started“ (Gestartet) und „Succeeded“ (Erfolgreich)), weisen sie die gleiche Korrelations-ID auf. Hiermit können auch andere Beziehungen zwischen Ereignissen dargestellt werden. |
| identity | Optional | Ein JSON-Blob zum Beschreiben der Identität des Benutzers oder der Anwendung, der bzw. die den Vorgang durchgeführt hat. Normalerweise sind hierin auch die Autorisierung und die Ansprüche bzw. das JWT-Token aus Active Directory enthalten. |
| Level | Optional | Der Schweregrad des Ereignisses. Er kann „Informational“ (Information), „Warning“ (Warnung), „Error“ (Fehler) oder „Critical“ (Kritisch) lauten. |
| location | Optional | Die Region der Ressource, die das Ereignis ausgibt, z.B. „USA, Osten“ oder „Frankreich, Süden“. |
| properties | Optional | Alle erweiterten Eigenschaften, die sich auf die jeweilige Kategorie der Ereignisse beziehen. Alle benutzerdefinierten bzw. eindeutigen Eigenschaften müssen in diesem „Teil B“ des Schemas angeordnet werden. |

## <a name="service-specific-schemas"></a>Dienstspezifische Schemas
Das Schema für Ressourcendiagnoseprotokolle variiert je nach Ressourcentyp, der durch die Eigenschaft `resourceId` und die `category`-Eigenschaften definiert ist. In der folgenden Liste sind alle Azure-Dienste, die Ressourcenprotokolle unterstützen, mit Links zum Dienst und zum kategoriespezifischen Schema (falls zutreffend) aufgeführt.

| Dienst | Schema und Dokumente |
| --- | --- |
| Azure Active Directory | [Übersicht](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Überwachungsprotokollschema](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) und [Anmeldeschema](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [API Management-Diagnoseprotokolle](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Anwendungsgateways |[Diagnoseprotokollierung für Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure-Automatisierung |[Protokollanalysen für Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Diagnoseprotokolle für Azure Batch](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database for MySQL-Diagnoseprotokolle](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL-Diagnoseprotokolle](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Cognitive Services | [Diagnoseprotokollierung für Azure Cognitive Services](../../cognitive-services/diagnostic-logging.md) |
| Content Delivery Network | [Azure-Diagnoseprotokolle für CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB-Protokollierung](../../cosmos-db/logging.md) |
| Data Factory | [Überwachen von Data Factorys mit Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs-Diagnoseprotokolle](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Schema nicht verfügbar. |
| Azure Firewall | Schema nicht verfügbar. |
| IoT Hub | [IoT Hub-Vorgänge](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure-Schlüsseltresor-Protokollierung](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Protokollanalysen für den Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netzwerksicherheitsgruppen |[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDoS-Schutz | [Verwalten von Azure DDoS Protection Standard](../../virtual-network/manage-ddos-protection.md) |
| Power BI Dedicated | [Diagnoseprotokollierung für Power BI Embedded in Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Datenmodell für Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Suchen, |[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../../search/search-traffic-analytics.md) |
| Service Bus |[Azure Service Bus-Diagnoseprotokolle](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL-Datenbank | [Azure SQL-Datenbank-Diagnoseprotokollierung](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Auftragsdiagnoseprotokolle](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Traffic Manager-Protokollschema](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Virtuelle Netzwerke | Schema nicht verfügbar. |
| Gateways für virtuelle Netzwerke | Schema nicht verfügbar. |

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr über andere Protokolle der Azure-Plattform](platform-logs-overview.md), die Sie zum Überwachen von Azure verwenden können.
