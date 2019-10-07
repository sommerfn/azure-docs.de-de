---
title: Streamen von Azure-Überwachungsdaten an einen Event Hub
description: Erfahren Sie, wie Sie Ihre Azure-Überwachungsdaten an einen Event Hub streamen, um die Daten in einem SIEM- oder Analysetool von Partnern abzurufen.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/20/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: 535c74fd161019db28e691ff916ad03eaaf07c90
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71260375"
---
# <a name="stream-azure-monitoring-data-to-an-event-hub"></a>Streamen von Azure-Überwachungsdaten an einen Event Hub
Azure Monitor bietet eine vollständige Lösung für die Stapelüberwachung für Anwendungen und Dienste in Azure, in anderen Clouds und lokal. Sie sollten diese Daten nicht nur mit Azure Monitor analysieren und für verschiedene Überwachungsszenarios nutzen, sondern sie auch an andere Überwachungstools in Ihrer Umgebung senden. Die effektivste Methode zum Streamen von Überwachungsdaten an externe Tools ist in den meisten Fällen die Verwendung von [Azure Event Hubs](/azure/event-hubs/). Dieser Artikel enthält eine kurze Beschreibung der Vorgehensweise beim Streamen von Überwachungsdaten aus verschiedenen Quellen an einen Event Hub und Links zu ausführlichen Anleitungen.


## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace

Bevor Sie das Streaming für eine Datenquelle konfigurieren, müssen Sie [einen Event Hubs-Namespace und einen Event Hub erstellen](../../event-hubs/event-hubs-create.md). Dieser Namespace und Event Hub sind das Ziel für sämtliche Ihrer Überwachungsdaten. Ein Event Hubs-Namespace ist eine logische Gruppierung von Event Hubs mit derselben Zugriffsrichtlinie, er ähnelt damit einem Speicherkonto mit mehreren Blobs. Beachten Sie die folgenden Hinweise zum Event Hubs-Namespace und zu den Event Hubs, die Sie zum Streamen von Überwachungsdaten verwenden:

* Über die Anzahl der Durchsatzeinheiten können Sie den Durchsatz für Ihre Event Hubs erhöhen. In der Regel ist nur eine Durchsatzeinheit erforderlich. Wenn Sie aufgrund steigender Protokollnutzung zentral hochskalieren müssen, können Sie die Anzahl der Durchsatzeinheiten für den Namespace manuell erhöhen oder die automatische Aufstockung aktivieren.
* Über die Anzahl von Partitionen können Sie den Verbrauch vieler Consumer parallelisieren. Eine einzelne Partition kann bis zu 20 MBit/s oder ungefähr 20.000 Nachrichten pro Sekunde unterstützen. Je nach Tool, das die Daten nutzt, ist es auch möglich, Daten von mehreren Partitionen zu verarbeiten. Wenn Sie sich nicht sicher sind, welche Anzahl von Partitionen festgelegt werden soll, ist es sinnvoll, mit vier Partitionen zu beginnen.
* Legen Sie die Nachrichtenvermerkdauer für Ihren Event Hub auf mindestens 7 Tage fest. Wenn Ihr Tool mehr als einen Tag ausfällt, wird dadurch sichergestellt, dass das Tool für bis zu 7 Tage alte Ereignisse dort fortfahren kann, wo es aufgehört hatte.
* Sie sollten die Standardconsumergruppe für Ihren Event Hub verwenden. Es ist nur dann erforderlich, andere Consumergruppen zu erstellen oder eine separate Consumergruppe zu verwenden, wenn Sie zwei verschiedenen Tools verwenden möchten, um dieselben Daten vom selben Event Hub zu verarbeiten.
* Für das Azure-Aktivitätsprotokoll wählen Sie einen Event Hubs-Namespace aus. Azure Monitor erstellt dann in diesem Namespace den Event Hub _insights-logs-operational-logs_. Für andere Protokolltypen können Sie entweder einen vorhandenen Event Hub verwenden oder Azure Monitor für jede Protokollkategorie einen Event Hub erstellen lassen.
* In der Regel müssen die ausgehenden Ports 5671 und 5672 auf dem Computer oder im VNET offen sein, der bzw. das die Daten vom Event Hub verarbeitet.


## <a name="monitoring-data-available"></a>Verfügbare Überwachungsdaten
Im Artikel [Quellen für Überwachungsdaten für Azure Monitor](data-sources.md) werden die unterschiedlichen Datenebenen für Azure-Anwendungen und die jeweils verfügbaren Arten von Überwachungsdaten beschrieben. In der folgenden Tabelle werden die einzelnen Ebenen aufgeführt und es wird beschrieben, wie diese Daten an einen Event Hub gestreamt werden können. Über die Links erhalten Sie weitere Informationen.

| Tarif | Data | Methode |
|:---|:---|:---|
| [Azure-Mandant](data-sources.md#azure-tenant) | Azure Active Directory-Überwachungsprotokolle | Konfigurieren Sie eine Diagnoseeinstellung für Ihren AAD-Mandanten. Weitere Informationen finden Sie unter [Tutorial: Streamen von Azure Active Directory-Protokollen an einen Azure Event Hub](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |
| [Azure-Abonnement](data-sources.md#azure-subscription) | Azure-Aktivitätsprotokoll | Erstellen Sie ein Protokollprofil zum Exportieren von Ereignissen des Aktivitätsprotokolls an Event Hubs.  Weitere Informationen finden Sie unter [Exportieren des Aktivitätsprotokolls in den Speicher oder in Azure Event Hubs](activity-log-export.md). |
| [Azure-Ressourcen](data-sources.md#azure-resources) | Plattformmetriken<br>Diagnoseprotokolle |Beide Arten von Daten werden mithilfe einer Ressourcendiagnoseeinstellung an einen Event Hub gesendet. Weitere Informationen finden Sie unter [Streamen von Azure-Ressourcenprotokollen an einen Event Hub](resource-logs-stream-event-hubs.md). |
| [Betriebssystem (Gast)](data-sources.md#operating-system-guest) | Azure Virtual Machines | Installieren Sie die [Azure-Diagnoseerweiterung](diagnostics-extension-overview.md) auf virtuellen Windows- und Linux-Computern in Azure. Weitere Informationen zu virtuellen Windows-Computern finden Sie unter [Streamen von Azure-Diagnosedaten auf dem langsamsten Pfad mithilfe von Event Hubs](diagnostics-extension-stream-event-hubs.md) und zu virtuellen Linux-Computern unter [Verwenden der Linux-Diagnoseerweiterung zum Überwachen von Metriken und Protokollen](../../virtual-machines/extensions/diagnostics-linux.md#protected-settings). |
| [Anwendungscode](data-sources.md#application-code) | Application Insights | Application Insights bietet keine direkte Methode zum Streamen von Daten an Event Hubs. Sie können einen [fortlaufenden Export](../../azure-monitor/app/export-telemetry.md) der Application Insights-Daten an ein Speicherkonto einrichten und die Daten dann mithilfe einer Logik-App an einen Event Hub senden, wie unter [Manuelles Streamen mit einer Logik-App](#manual-streaming-with-logic-app) beschrieben. |

## <a name="manual-streaming-with-logic-app"></a>Manuelles Streamen mit einer Logik-App
Für Daten, die sich nicht direkt an einen Event Hub streamen lassen, können Sie einen Azure-Speicher nutzen und dann eine zu bestimmten Zeiten ausgelöste Logik-App verwenden, die [die Daten aus dem Blob-Speicher holt](../../connectors/connectors-create-api-azureblobstorage.md#add-action) und [sie als Nachricht an den Event Hub überträgt](../../connectors/connectors-create-api-azure-event-hubs.md#add-action). 


## <a name="tools-with-azure-monitor-integration"></a>Tools mit Azure Monitor-Integration

Die Weiterleitung Ihrer Überwachungsdaten an einen Event Hub mit Azure Monitor ermöglicht eine einfache Integration in externe SIEM- und Überwachungstools. Hier einige Beispiele für Tools mit Azure Monitor-Integration:

| Tool | BESCHREIBUNG |
|:---|:---|
|  IBM QRadar | Das DSM und Event Hub-Protokoll von Microsoft Azure sind zum Download auf der [Website des IBM-Supports](https://www.ibm.com/support) erhältlich. Weitere Informationen zur Integration in Azure finden Sie unter [QRadar DSM configuration (QRadar DSM-Konfiguration)](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0). |
| Splunk | Das [Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/) steht als Open-Source-Projekt in der Splunkbase zur Verfügung. Die Dokumentation ist unter [Azure Monitor-Add-On für Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk) erhältlich.<br><br> Falls Sie kein Add-On in Ihrer Splunk-Instanz installieren können, z. B. bei Verwendung eines Proxys oder bei Ausführung in Splunk Cloud, können Sie diese Ereignisse an die HTTP-Ereignissammlung von Splunk weiterleiten. Verwenden Sie dazu die [Azure-Funktion für Splunk](https://github.com/Microsoft/AzureFunctionforSplunkVS), die durch neue Nachrichten im Event Hub ausgelöst wird. |
| sumologic | Anweisungen zum Einrichten von SumoLogic für die Nutzung von Daten aus einem Event Hub finden Sie unter [Collect Logs for the Azure Audit App from Event Hub (Sammeln von Protokollen für die Azure Audit App aus einem Event Hub)](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub). |
| ArcSight | Der intelligente Azure Event Hub-Connector von ArcSight wird im Rahmen dieser [ArcSight-Sammlung von intelligenten Connectors](https://community.softwaregrp.com/t5/Discussions/Announcing-General-Availability-of-ArcSight-Smart-Connectors-7/m-p/1671852) zur Verfügung gestellt. |
| Syslog-Server | Wenn Sie Azure Monitor-Daten direkt an einen Syslog-Server streamen möchten, können Sie eine [auf einer Azure-Funktion basierende Lösung](https://github.com/miguelangelopereira/azuremonitor2syslog/) nutzen.


## <a name="next-steps"></a>Nächste Schritte
* [Archivieren Sie das Aktivitätsprotokoll in einem Speicherkonto.](../../azure-monitor/platform/archive-activity-log.md)
* [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](../../azure-monitor/platform/activity-logs-overview.md)
* [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](../../azure-monitor/platform/alerts-log-webhook.md)


