---
title: 'Azure-Ressourcenprotokolle: Unterstützte Dienste und Schemas'
description: Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Diagnose-Protokolle.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
ms.date: 10/22/2019
author: rboucher
ms.author: robb
ms.openlocfilehash: 09d1a25b83f405b45bbefd39766c82565ea86925
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73476676"
---
# <a name="supported-services-schemas-and-categories-for-azure-resource-logs"></a>Unterstützte Dienste, Schemas und Kategorien für Azure-Ressourcenprotokolle

> [!NOTE]
> Ressourcenprotokolle wurden zuvor als Diagnoseprotokolle bezeichnet.

[Azure Monitor-Ressourcenprotokolle](../../azure-monitor/platform/resource-logs-overview.md) sind von Azure-Diensten ausgegebene Protokolle, die die Vorgänge der jeweiligen Dienste oder Ressourcen beschreiben. Für alle Ressourcenprotokolle, die über Azure Monitor verfügbar sind, wird ein Schema der obersten Ebene gemeinsam genutzt. Auf diese Weise kann jeder Dienst für seine eigenen Ereignisse flexibel eindeutige Eigenschaften ausgeben.

Ein Schema wird mit einer Kombination aus dem Ressourcentyp (in der `resourceId`-Eigenschaft verfügbar) und dem `category`-Element eindeutig identifiziert. In diesem Artikel wird das Schema der obersten Ebene für Ressourcenprotokolle beschrieben, und es sind Links zu den Schemas für jeden Dienst vorhanden.

## <a name="top-level-resource-logs-schema"></a>Ressourcenprotokollschema der obersten Ebene

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

## <a name="service-specific-schemas-for-resource-logs"></a>Dienstspezifische Schemas für Ressourcenprotokolle
Das Schema für Diagnoseprotokolle für Ressourcen variiert abhängig von der Ressource und der Protokollkategorie. In dieser Liste sind alle Dienste aufgeführt, über die Ressourcenprotokolle und Links zum Dienst und zum kategoriespezifischen Schema (falls zutreffend) verfügbar gemacht werden.

| Dienst | Schema und Dokumente |
| --- | --- |
| Azure Active Directory | [Übersicht](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Überwachungsprotokollschema](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) und [Anmeldeschema](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [API Management-Ressourcenprotokolle](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Anwendungsgateways |[Protokollierung für Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure-Automatisierung |[Protokollanalysen für Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch-Protokollierung](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database for MySQL-Diagnoseprotokolle](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL-Protokolle](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Azure-Daten-Explorer | [Azure Data Explorer-Protokolle](../../data-explorer/using-diagnostic-logs.md) |
| Cognitive Services | [Protokollierung für Azure Cognitive Services](../../cognitive-services/diagnostic-logging.md) |
| Containerregistrierung | [Protokollierung für Azure Container Registry](../../container-registry/container-registry-diagnostics-audit-logs.md) |
| Content Delivery Network | [Azure-Protokolle für CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB-Protokollierung](../../cosmos-db/logging.md) |
| Data Factory | [Überwachen von Data Factorys mit Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Zugreifen auf Protokolle für Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Zugreifen auf Protokolle für Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs-Protokolle](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Schema nicht verfügbar. |
| Azure Firewall | Schema nicht verfügbar. |
| IoT Hub | [IoT Hub-Vorgänge](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Azure-Schlüsseltresor-Protokollierung](../../key-vault/key-vault-logging.md) |
| Kubernetes Service |[Azure Kubernetes-Protokollierung](../../aks/view-master-logs.md#log-event-schema) |
| Load Balancer |[Protokollanalysen für den Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netzwerksicherheitsgruppen |[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDoS-Schutz | [Verwalten von Azure DDoS Protection Standard](../../virtual-network/manage-ddos-protection.md) |
| Power BI Dedicated | [Protokollierung für Power BI Embedded in Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Datenmodell für Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Suchen, |[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../../search/search-traffic-analytics.md) |
| Service Bus |[Azure Service Bus-Protokolle](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL-Datenbank | [Azure SQL-Datenbank-Protokollierung](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Auftragsprotokolle](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | [Traffic Manager-Protokollschema](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Virtuelle Netzwerke | Schema nicht verfügbar. |
| Gateways für virtuelle Netzwerke | Schema nicht verfügbar. |

## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Protokollkategorien pro Ressourcentyp
|Ressourcentyp|Category|Anzeigename der Kategorie|
|---|---|---|
|Microsoft.AAD/domainServices|SystemSecurity|SystemSecurity|
|Microsoft.AAD/domainServices|AccountManagement|AccountManagement|
|Microsoft.AAD/domainServices|LogonLogoff|LogonLogoff|
|Microsoft.AAD/domainServices|ObjectAccess|ObjectAccess|
|Microsoft.AAD/domainServices|PolicyChange|PolicyChange|
|Microsoft.AAD/domainServices|PrivilegeUse|PrivilegeUse|
|Microsoft.AAD/domainServices|DetailTracking|DetailTracking|
|Microsoft.AAD/domainServices|DirectoryServiceAccess|DirectoryServiceAccess|
|Microsoft.AAD/domainServices|AccountLogon|AccountLogon|
|microsoft.aadiam/tenants|Signin|Signin|
|Microsoft.AnalysisServices/servers|Motor|Motor|
|Microsoft.AnalysisServices/servers|Dienst|Dienst|
|Microsoft.ApiManagement/service|GatewayLogs|Protokolle im Zusammenhang mit dem ApiManagement-Gateway|
|Microsoft.AppPlatform/Spring|ApplicationConsole|Anwendungskonsole|
|Microsoft.Automation/automationAccounts|JobLogs|Auftragsprotokolle|
|Microsoft.Automation/automationAccounts|JobStreams|Auftragsdatenströme|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC-Knotenstatus|
|Microsoft.Batch/batchAccounts|ServiceLog|Dienstprotokolle|
|Microsoft.BatchAI/workspaces|BaiClusterEvent|BaiClusterEvent|
|Microsoft.BatchAI/workspaces|BaiClusterNodeEvent|BaiClusterNodeEvent|
|Microsoft.BatchAI/workspaces|BaiJobEvent|BaiJobEvent|
|Microsoft.Blockchain/blockchainMembers|BlockchainApplication|Blockchainanwendung|
|Microsoft.Blockchain/blockchainMembers|Proxy|Proxy|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Ruft die Metriken des Endpunkts ab, z.B. Bandbreite, ausgehenden Datenverkehr usw.|
|Microsoft.ClassicNetwork/networksecuritygroups|Regelflussereignis der Netzwerksicherheitsgruppe|Regelflussereignis der Netzwerksicherheitsgruppe|
|Microsoft.CognitiveServices/accounts|Audit|Überwachungsprotokolle|
|Microsoft.CognitiveServices/accounts|RequestResponse|Anforderungs- und Antwortprotokolle|
|Microsoft.ContainerRegistry/registries|ContainerRegistryRepositoryEvents|RepositoryEvent-Protokolle (Vorschau)|
|Microsoft.ContainerRegistry/registries|ContainerRegistryLoginEvents|Anmeldeereignisse (Vorschau)|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Kubernetes-API-Server|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Kubernetes-Controller-Manager|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Kubernetes-Scheduler|
|Microsoft.ContainerService/managedClusters|kube-audit|Kubernetes-Überwachung|
|Microsoft.ContainerService/managedClusters|cluster-autoscaler|Automatische Kubernetes-Clusterskalierung|
|Microsoft.Databricks/workspaces|dbfs|Databricks-Dateisystem|
|Microsoft.Databricks/workspaces|clusters|Databricks-Cluster|
|Microsoft.Databricks/workspaces|accounts|Databricks-Konten|
|Microsoft.Databricks/workspaces|jobs|Databricks-Aufträge|
|Microsoft.Databricks/workspaces|notebook|Databricks-Notebook|
|Microsoft.Databricks/workspaces|ssh|Databricks-SSH|
|Microsoft.Databricks/workspaces|Arbeitsbereich|Databricks-Arbeitsbereich|
|Microsoft.Databricks/workspaces|secrets|Databricks-Geheimnisse|
|Microsoft.Databricks/workspaces|sqlPermissions|Databricks-SQL-Berechtigungen|
|Microsoft.Databricks/workspaces|instancePools|Instanzenpools|
|Microsoft.DataCatalog/datacatalogs|ScanStatusLogEvent|ScanStatus|
|Microsoft.DataFactory/factories|ActivityRuns|Pipeline-Aktivitätsausführungsprotokoll|
|Microsoft.DataFactory/factories|PipelineRuns|Pipelineausführungsprotokoll|
|Microsoft.DataFactory/factories|TriggerRuns|Triggerausführungsprotokoll|
|Microsoft.DataLakeAnalytics/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeAnalytics/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DataLakeStore/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeStore/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DataShare/accounts|Freigaben|Freigaben|
|Microsoft.DataShare/accounts|ShareSubscriptions|Freigabeabonnements|
|Microsoft.DataShare/accounts|SentShareSnapshots|Gesendete Freigabemomentaufnahmen|
|Microsoft.DataShare/accounts|ReceivedShareSnapshots|Empfangene Freigabemomentaufnahmen|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|MySQL Server-Protokolle|
|Microsoft.DBforMySQL/servers|MySqlAuditLogs|MySQL-Überwachungsprotokolle|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL-Serverprotokolle|
|Microsoft.DBforPostgreSQL/servers|QueryStoreRuntimeStatistics|Laufzeitstatistik des PostgreSQL-Abfragespeichers|
|Microsoft.DBforPostgreSQL/servers|QueryStoreWaitStatistics|Wartestatistik des PostgreSQL-Abfragespeichers|
|Microsoft.DBforPostgreSQL/serversv2|PostgreSQLLogs|PostgreSQL-Serverprotokolle|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreRuntimeStatistics|Laufzeitstatistik des PostgreSQL-Abfragespeichers|
|Microsoft.DBforPostgreSQL/serversv2|QueryStoreWaitStatistics|Wartestatistik des PostgreSQL-Abfragespeichers|
|Microsoft.DesktopVirtualization/workspaces|Prüfpunkt|Prüfpunkt|
|Microsoft.DesktopVirtualization/workspaces|Error|Error|
|Microsoft.DesktopVirtualization/workspaces|Verwaltung|Verwaltung|
|Microsoft.DesktopVirtualization/workspaces|Feed|Feed|
|Microsoft.DesktopVirtualization/applicationGroups|Prüfpunkt|Prüfpunkt|
|Microsoft.DesktopVirtualization/applicationGroups|Error|Error|
|Microsoft.DesktopVirtualization/applicationGroups|Verwaltung|Verwaltung|
|Microsoft.DesktopVirtualization/hostPools|Prüfpunkt|Prüfpunkt|
|Microsoft.DesktopVirtualization/hostPools|Error|Error|
|Microsoft.DesktopVirtualization/hostPools|Verwaltung|Verwaltung|
|Microsoft.DesktopVirtualization/hostPools|Verbindung|Verbindung|
|Microsoft.DesktopVirtualization/hostPools|HostRegistration|HostRegistration|
|Microsoft.Devices/IotHubs|Verbindungen|Verbindungen|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Gerätetelemetrie|
|Microsoft.Devices/IotHubs|C2DCommands|C2D-Befehle|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Geräteidentitätsvorgänge|
|Microsoft.Devices/IotHubs|FileUploadOperations|Dateiuploadvorgänge|
|Microsoft.Devices/IotHubs|Routen|Routen|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D-Zwillingsvorgänge|
|Microsoft.Devices/IotHubs|TwinQueries|Zwillingsabfragen|
|Microsoft.Devices/IotHubs|JobsOperations|Auftragsvorgänge|
|Microsoft.Devices/IotHubs|DirectMethods|Direkte Methoden|
|Microsoft.Devices/IotHubs|DistributedTracing|Verteilte Ablaufverfolgung (Vorschauversion)|
|Microsoft.Devices/IotHubs|Configurations|Configurations|
|Microsoft.Devices/IotHubs|DeviceStreams|Gerätestreams (Vorschau)|
|Microsoft.Devices/provisioningServices|DeviceOperations|Gerätevorgänge|
|Microsoft.Devices/provisioningServices|ServiceOperations|Dienstoperationen|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.DocumentDB/databaseAccounts|PartitionKeyStatistics|PartitionKeyStatistics|
|Microsoft.DocumentDB/databaseAccounts|ControlPlaneRequests|ControlPlaneRequests|
|Microsoft.EnterpriseKnowledgeGraph/services|AuditEvent|AuditEvent-Protokoll|
|Microsoft.EnterpriseKnowledgeGraph/services|DataIssue|DataIssue-Protokoll|
|Microsoft.EnterpriseKnowledgeGraph/services|Requests|Konfigurationsprotokoll|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archivprotokolle|
|Microsoft.EventHub/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Protokolle zur automatischen Skalierung|
|Microsoft.EventHub/namespaces|KafkaCoordinatorLogs|Kafka-Koordinatorprotokolle|
|Microsoft.EventHub/namespaces|KafkaUserErrorLogs|Kafka-Benutzerfehlerprotokolle|
|Microsoft.EventHub/namespaces|EventHubVNetConnectionEvent|VNet/IP-Filterung-Verbindungsprotokolle|
|Microsoft.EventHub/namespaces|CustomerManagedKeyUserLogs|Protokolle für kundenseitig verwalteten Schlüssel|
|Microsoft.HealthcareApis/services|AuditLogs|Überwachungsprotokolle|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Bewertungen der Autoskalierung|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Skalierungsaktionen der Autoskalierung|
|Microsoft.IoTSpaces/Graph|Trace|Trace|
|Microsoft.IoTSpaces/Graph|Bei Betrieb|Bei Betrieb|
|Microsoft.IoTSpaces/Graph|Audit|Audit|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Eingehende Daten|Eingehende Daten|
|Microsoft.IoTSpaces/Graph|Ausgehende Daten|Ausgehende Daten|
|Microsoft.KeyVault/vaults|AuditEvent|Überwachungsprotokolle|
|Microsoft.Kusto/Clusters|SucceededIngestion|Erfolgreiche Erfassungsvorgänge|
|Microsoft.Kusto/Clusters|FailedIngestion|Fehlgeschlagene Erfassungsvorgänge|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnoseereignisse zur Workflowlaufzeit|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integrationskonto –Nachverfolgen von Ereignissen|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterEvent|AmlComputeClusterEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeClusterNodeEvent|AmlComputeClusterNodeEvent|
|Microsoft.MachineLearningServices/workspaces|AmlComputeJobEvent|AmlComputeJobEvent|
|Microsoft.Media/mediaservices|KeyDeliveryRequests|Schlüsselübermittlungsanforderungen|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ereignis der Netzwerksicherheitsgruppe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Regelzähler der Netzwerksicherheitsgruppe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Regelflussereignis der Netzwerksicherheitsgruppe|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS-Schutz-Benachrichtigungen|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Datenflussprotokolle von Entscheidungen zur DDoS-Risikominderung|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|Berichte zur DDoS-Risikominderung|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM-Schutz-Warnungen|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway-Zugriffsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway-Leistungsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway-Firewallprotokoll|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure Firewall-Anwendungsregel|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure Firewall-Netzwerkregel|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Gatewaydiagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tunneldiagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Routendiagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE-Diagnoseprotokolle|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|P2S-Diagnoseprotokolle|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager-Testintegritätsergebnisse (Ereignis)|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Routentabellenprotokolle zum Peering|
|Microsoft.Network/vpnGateways|GatewayDiagnosticLog|Gatewaydiagnoseprotokolle|
|Microsoft.Network/vpnGateways|TunnelDiagnosticLog|Tunneldiagnoseprotokolle|
|Microsoft.Network/vpnGateways|RouteDiagnosticLog|Routendiagnoseprotokolle|
|Microsoft.Network/vpnGateways|IKEDiagnosticLog|IKE-Diagnoseprotokolle|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Frontdoor-Zugriffsprotokoll|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Frontdoor-Web Application Firewall-Protokoll|
|Microsoft.Network/p2sVpnGateways|GatewayDiagnosticLog|Gatewaydiagnoseprotokolle|
|Microsoft.Network/p2sVpnGateways|IKEDiagnosticLog|IKE-Diagnoseprotokolle|
|Microsoft.Network/p2sVpnGateways|P2SDiagnosticLog|P2S-Diagnoseprotokolle|
|Microsoft.Network/bastionHosts|BastionAuditLogs|Bastion-Überwachungsprotokolle|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer-Warnereignisse|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Integritätsstatus der Load Balancer-Stichprobe|
|Microsoft.PowerBIDedicated/capacities|Motor|Motor|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup-Berichtsdaten|
|Microsoft.RecoveryServices/Vaults|CoreAzureBackup|Kern: Azure Backup-Daten|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupJobs|Add-On: Azure Backup-Auftragsdaten|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupAlerts|Add-On: Azure Backup-Warnungsdaten|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupPolicy|Add-On: Azure Backup-Richtliniendaten|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupStorage|Add-On: Azure Backup-Speicherdaten|
|Microsoft.RecoveryServices/Vaults|AddonAzureBackupProtectedInstance|Add-On: Azure Backup-Daten für geschützte Instanz|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-Aufträge|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-Ereignisse|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Replizierte Azure Site Recovery-Elemente|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery-Replikationsstatistiken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery-Wiederherstellungspunkte|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Uploadrate für Azure Site Recovery-Replikationsdaten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Datenänderungen auf mit Azure Site Recovery geschützten Datenträgern|
|Microsoft.Search/searchServices|OperationLogs|Vorgangsprotokolle|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.Sql/servers/databases|SQLInsights|SQL-Informationen|
|Microsoft.Sql/servers/databases|AutomaticTuning|Automatische Optimierung|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Laufzeitstatistik des Abfragespeichers|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Wartestatistik des Abfragespeichers|
|Microsoft.Sql/servers/databases|Errors|Errors|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Wartestatistik der Datenbank|
|Microsoft.Sql/servers/databases|Zeitlimits|Zeitlimits|
|Microsoft.Sql/servers/databases|Blöcke|Blöcke|
|Microsoft.Sql/servers/databases|Deadlocks|Deadlocks|
|Microsoft.Sql/servers/databases|Audit|Überwachungsprotokolle|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|
|Microsoft.Sql/servers/databases|DmsWorkers|DMS-Worker|
|Microsoft.Sql/servers/databases|ExecRequests|Ausführungsanforderungen|
|Microsoft.Sql/servers/databases|RequestSteps|Anforderungsschritte|
|Microsoft.Sql/servers/databases|SqlRequests|SQL-Anforderungen|
|Microsoft.Sql/servers/databases|Wartevorgänge|Wartevorgänge|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Nutzungsstatistiken zu Ressourcen|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|SQL-Sicherheitsüberwachungsereignis|
|Microsoft.Sql/managedInstances/databases|SQLInsights|SQL-Informationen|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Laufzeitstatistik des Abfragespeichers|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Wartestatistik des Abfragespeichers|
|Microsoft.Sql/managedInstances/databases|Errors|Errors|
|Microsoft.Storage/storageAccounts/tableServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/tableServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/tableServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/blobServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/blobServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/blobServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/fileServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/fileServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/fileServices|StorageDelete|StorageDelete|
|Microsoft.Storage/storageAccounts/queueServices|StorageRead|StorageRead|
|Microsoft.Storage/storageAccounts/queueServices|StorageWrite|StorageWrite|
|Microsoft.Storage/storageAccounts/queueServices|StorageDelete|StorageDelete|
|Microsoft.StreamAnalytics/streamingjobs|Ausführung|Ausführung|
|Microsoft.StreamAnalytics/streamingjobs|Erstellen|Erstellen|
|microsoft.web/hostingenvironments|AppServiceEnvironmentPlatformLogs|Plattformprotokolle der App Service-Umgebung|
|microsoft.web/sites|FunctionAppLogs|Funktionsanwendungsprotokolle|
|microsoft.web/sites|AppServiceHTTPLogs|HTTP-Protokolle|
|microsoft.web/sites|AppServiceConsoleLogs|App Service-Konsolenprotokolle|
|microsoft.web/sites|AppServiceAppLogs|App Service-Anwendungsprotokolle|
|microsoft.web/sites|AppServiceFileAuditLogs|Überwachungsprotokolle für Website-Inhaltsänderungen|
|microsoft.web/sites|AppServiceAuditLogs|Zugriffsüberwachungsprotokolle|
|microsoft.web/sites/slots|FunctionAppLogs|Funktionsanwendungsprotokolle|
|microsoft.web/sites/slots|AppServiceHTTPLogs|HTTP-Protokolle|
|microsoft.web/sites/slots|AppServiceConsoleLogs|Konsolenprotokolle|
|microsoft.web/sites/slots|AppServiceAppLogs|Anwendungsprotokolle|
|microsoft.web/sites/slots|AppServiceFileAuditLogs|Überwachungsprotokolle für Website-Inhaltsänderungen|
|microsoft.web/sites/slots|AppServiceAuditLogs|Zugriffsüberwachungsprotokolle|

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Ressourcenprotokollen](../../azure-monitor/platform/resource-logs-overview.md)
* [Streamen von Ressourcenprotokollen an **Event Hubs**](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)
* [Ändern der Diagnoseeinstellungen für Ressourcenprotokolle mithilfe der Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../../azure-monitor/platform/collect-azure-metrics-logs.md)
