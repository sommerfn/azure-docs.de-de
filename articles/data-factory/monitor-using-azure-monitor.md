---
title: Überwachen von Data Factorys mit Azure Monitor | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Data Factory-Pipelines mit Azure Monitor überwachen können, indem Sie Diagnoseprotokolle mit Informationen aus Data Factory aktivieren.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 6f5472e42b7ef43123698f01ee76fb0e691aa45e
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827805"
---
# <a name="alert-and-monitor-data-factories-by-using-azure-monitor"></a>Benachrichtigen und Überwachen von Data Factorys mithilfe von Azure Monitor

Cloudanwendungen sind komplex und verfügen über zahlreiche Variablen. Monitore stellen Daten bereit, die dabei helfen, die ordnungsgemäße Ausführung der Anwendung sicherzustellen. Monitore helfen Ihnen auch, potenzielle Probleme zu vermeiden und aufgetretene Probleme zu beheben.

Sie können anhand der Überwachungsdaten umfassende Erkenntnisse zu Ihren Anwendungen gewinnen. Mit diesem Wissen können Sie die Leistung und Wartbarkeit von Anwendungen verbessern. Außerdem können Sie Aktionen automatisieren, die andernfalls einen manuellen Eingriff erfordern.

Azure Monitor stellt grundlegende Metriken und Protokolle der Infrastruktur für die meisten Azure-Dienste bereit. Azure-Diagnoseprotokolle werden von einer Ressource ausgegeben und stellen umfangreiche und in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Und Azure Data Factory schreibt die Diagnoseprotokolle in den Monitor.

Weitere Informationen finden Sie in der [Übersicht über Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor).

## <a name="keeping-azure-data-factory-data"></a>Aufbewahren von Azure Data Factory-Daten

Data Factory speichert Pipelineausführungsdaten nur 45 Tage lang. Verwenden Sie Azure Monitor, wenn Sie diese Daten für einen längeren Zeitraum aufbewahren möchten. Mit Azure Monitor können Sie Diagnoseprotokolle zur Analyse weiterleiten. Sie können sie auch in einem Speicherkonto aufbewahren, damit Sie für die gewählte Dauer über Factoryinformationen verfügen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

* Speichern Sie Diagnoseprotokolle zur Überwachung oder manuellen Überprüfung in einem Speicherkonto. In den Diagnoseeinstellungen können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* Streamen Sie die Protokolle in Azure Event Hubs. Die Protokolle werden zu Eingaben für einen Partnerdienst oder eine benutzerdefinierte Analyselösung wie Power BI.
* Analysieren Sie die Protokolle mit Log Analytics.

Sie können ein Speicherkonto oder einen Event Hub-Namespace verwenden, das/der sich nicht im Abonnement der Ressource befindet, die Protokolle ausgibt. Der Benutzer, der die Einstellung konfiguriert, benötigt entsprechenden Zugriff gemäß der rollenbasierten Zugriffssteuerung auf beide Abonnements.

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen

### <a name="diagnostic-settings"></a>Diagnoseeinstellungen

Verwenden Sie Diagnoseeinstellungen, um Diagnoseprotokolle für Nicht-Computeressourcen zu konfigurieren. Die Einstellungen für eine Ressourcensteuerung verfügen über die folgenden Features:

* Sie geben an, wohin Diagnoseprotokolle gesendet werden, z. B. an ein Azure Storage-Konto, an einen Azure Event Hub oder an Monitor-Protokolle.
* Sie geben an, welche Protokollkategorien gesendet werden.
* Sie geben an, wie lange die einzelnen Protokollkategorien in einem Speicherkonto aufbewahrt werden.
* Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
* Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist, werden die Aufbewahrungsrichtlinien ignoriert. Diese Bedingung kann z.B. auftreten, wenn nur die Optionen „Event Hubs“ oder „Überwachungsprotokolle“ ausgewählt sind.
* Aufbewahrungsrichtlinien werden pro Tag angewendet. Die Grenze zwischen Tagen liegt um Mitternacht (Koordinierte Weltzeit, UTC). Jeweils am Ende eines Tages werden die Protokolle der Tage gelöscht, deren Aufbewahrungsdauer abgelaufen ist. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

### <a name="enable-diagnostic-logs-via-the-azure-monitor-rest-api"></a>Aktivieren von Diagnoseprotokollen über die Azure Monitor-REST-API

#### <a name="create-or-update-a-diagnostics-setting-in-the-monitor-rest-api"></a>Erstellen oder Aktualisieren einer Diagnoseeinstellung in der REST-API von Azure Monitor

##### <a name="request"></a>Anforderung

```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Header

* Ersetzen Sie `{api-version}` durch `2016-09-01`.
* Ersetzen Sie `{resource-id}` durch die ID der Ressource, deren Diagnoseeinstellungen Sie bearbeiten möchten. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/manage-resource-groups-portal.md).
* Legen Sie den Header `Content-Type` auf `application/json` fest.
* Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure Active Directory (Azure AD) abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen](../active-directory/develop/authentication-scenarios.md).

##### <a name="body"></a>Body

```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Eigenschaft | Typ | BESCHREIBUNG |
| --- | --- | --- |
| **storageAccountId** |Zeichenfolge | Die Ressourcen-ID des Speicherkontos, an das Diagnoseprotokolle gesendet werden sollen. |
| **serviceBusRuleId** |Zeichenfolge | Die Service Bus-Regel-ID des Service Bus-Namespace, in dem Event Hubs für das Streaming von Diagnoseprotokollen erstellt werden sollen. Die Regel-ID weist das Format `{service bus resource ID}/authorizationrules/{key name}` auf.|
| **workspaceId** | Komplexer Typ | Ein Array metrischer Zeiteinheiten und ihrer Aufbewahrungsrichtlinien. Der Wert dieser Eigenschaft ist leer. |
|**metrics**| Parameterwerte der Pipelineausführung, die an die aufgerufene Pipeline übergeben werden sollen| Ein JSON-Objekt, das Parameternamen Argumentwerten zuordnet. |
| **logs**| Komplexer Typ| Der Name einer Diagnoseprotokollkategorie für einen Ressourcentyp. Um die Liste der Diagnoseprotokollkategorien für eine Ressource zu erhalten, führen Sie einen GET-Vorgang zum Abrufen von Diagnoseeinstellungen aus. |
| **category**| Zeichenfolge| Ein Array von Protokollkategorien und ihrer Aufbewahrungsrichtlinien. |
| **timeGrain** | Zeichenfolge | Die Granularität von Metriken, die im Zeitformat ISO 8601 erfasst werden. Der Eigenschaftswert muss `PT1M` sein, was eine Minute angibt. |
| **enabled**| Boolean | Gibt an, ob die Sammlung der Metrik- oder Protokollkategorie für diese Ressource aktiviert ist. |
| **retentionPolicy**| Komplexer Typ| Beschreibt die Aufbewahrungsrichtlinie für eine Metrik- oder Protokollkategorie. Diese Eigenschaft wird nur für Speicherkonten verwendet. |
|**days**| Int| Die Anzahl der Tage, die Metriken oder Protokolle aufbewahrt werden sollen. Wenn der Eigenschaftswert „0“ ist, werden die Protokolle dauerhaft aufbewahrt. Diese Eigenschaft wird nur für Speicherkonten verwendet. |

##### <a name="response"></a>response

200 OK.


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

#### <a name="get-information-about-diagnostics-settings-in-the-monitor-rest-api"></a>Abrufen von Informationen zu Diagnoseeinstellungen in der Monitor-REST-API

##### <a name="request"></a>Anforderung

```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

##### <a name="headers"></a>Header

* Ersetzen Sie `{api-version}` durch `2016-09-01`.
* Ersetzen Sie `{resource-id}` durch die ID der Ressource, deren Diagnoseeinstellungen Sie bearbeiten möchten. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/manage-resource-groups-portal.md).
* Legen Sie den Header `Content-Type` auf `application/json` fest.
* Legen Sie den Autorisierungsheader auf ein JSON-Webtoken fest, das Sie aus Azure AD abgerufen haben. Weitere Informationen finden Sie unter [Authentifizieren von Anforderungen](../active-directory/develop/authentication-scenarios.md).

##### <a name="response"></a>response

200 OK.

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}

```
Weitere Informationen finden Sie unter [Diagnoseeinstellungen](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).

## <a name="schema-of-logs-and-events"></a>Schema von Protokollen und Ereignissen

### <a name="monitor-schema"></a>Monitor-Schema

#### <a name="activity-run-log-attributes"></a>Attribute des Aktivitätsausführungsprotokolls

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Eigenschaft | Typ | BESCHREIBUNG | Beispiel |
| --- | --- | --- | --- |
| **Level** |Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie für Aktivitätsausführungsprotokolle den Eigenschaftswert auf „4“ fest. | `4` |
| **correlationId** |Zeichenfolge | Die eindeutige ID für die Nachverfolgung einer bestimmten Anforderung. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Zeichenfolge | Die Zeitpunkt des Ereignisses im TimeSpan-UTC-Format `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**activityRunId**| Zeichenfolge| Die ID der Aktivitätsausführung. | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|**pipelineRunId**| Zeichenfolge| Die ID der Pipelineausführung. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**Ressourcen-ID**| Zeichenfolge | Die mit der Data Factory-Ressource verknüpfte ID. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| Zeichenfolge | Die Kategorie der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `ActivityRuns` fest. | `ActivityRuns` |
|**level**| Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `Informational` fest. | `Informational` |
|**operationName**| Zeichenfolge | Der Name der Aktivität zusammen mit ihrem Status. Wenn die Aktivität der Startheartbeat ist, lautet der Eigenschaftswert `MyActivity -`. Wenn die Aktivität der Endheartbeat ist, lautet der Eigenschaftswert `MyActivity - Succeeded`. | `MyActivity - Succeeded` |
|**pipelineName**| Zeichenfolge | Der Name der Pipeline. | `MyPipeline` |
|**activityName**| Zeichenfolge | Der Name der Aktivität. | `MyActivity` |
|**start**| Zeichenfolge | Die Startzeit der Aktivitätsausführungen im TimeSpan-UTC-Format. | `2017-06-26T20:55:29.5007959Z`|
|**end**| Zeichenfolge | Die Endzeit der Aktivitätsausführungen im TimeSpan-UTC-Format. Wenn das Diagnoseprotokoll anzeigt, dass eine Aktivität gestartet, aber noch nicht beendet wurde, lautet der Eigenschaftswert `1601-01-01T00:00:00Z`. | `2017-06-26T20:55:29.5007959Z` |

#### <a name="pipeline-run-log-attributes"></a>Attribute des Pipelineausführungsprotokolls

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Eigenschaft | Typ | BESCHREIBUNG | Beispiel |
| --- | --- | --- | --- |
| **Level** |Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie für Aktivitätsausführungsprotokolle den Eigenschaftswert auf „4“ fest. | `4` |
| **correlationId** |Zeichenfolge | Die eindeutige ID für die Nachverfolgung einer bestimmten Anforderung. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Zeichenfolge | Die Zeitpunkt des Ereignisses im TimeSpan-UTC-Format `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**runId**| Zeichenfolge| Die ID der Pipelineausführung. | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|**Ressourcen-ID**| Zeichenfolge | Die mit der Data Factory-Ressource verknüpfte ID. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| Zeichenfolge | Die Kategorie der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `PipelineRuns` fest. | `PipelineRuns` |
|**level**| Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `Informational` fest. | `Informational` |
|**operationName**| Zeichenfolge | Der Name der Pipeline zusammen mit ihrem Status. Nachdem die Pipelineausführung abgeschlossen ist, lautet der Eigenschaftswert `Pipeline - Succeeded`. | `MyPipeline - Succeeded`. |
|**pipelineName**| Zeichenfolge | Der Name der Pipeline. | `MyPipeline` |
|**start**| Zeichenfolge | Die Startzeit der Aktivitätsausführungen im TimeSpan-UTC-Format. | `2017-06-26T20:55:29.5007959Z`. |
|**end**| Zeichenfolge | Die Endzeit der Aktivitätsausführungen im TimeSpan-UTC-Format. Wenn das Diagnoseprotokoll anzeigt, dass eine Aktivität gestartet, aber noch nicht beendet wurde, lautet der Eigenschaftswert `1601-01-01T00:00:00Z`.  | `2017-06-26T20:55:29.5007959Z` |
|**status**| Zeichenfolge | Der abschließende Status der Pipelineausführung. Mögliche Eigenschaftswerte sind `Succeeded` und `Failed`. | `Succeeded`|

#### <a name="trigger-run-log-attributes"></a>Attribute des Triggerausführungsprotokolls

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Eigenschaft | Typ | BESCHREIBUNG | Beispiel |
| --- | --- | --- | --- |
| **Level** |Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie für Aktivitätsausführungsprotokolle den Eigenschaftswert auf „4“ fest. | `4` |
| **correlationId** |Zeichenfolge | Die eindeutige ID für die Nachverfolgung einer bestimmten Anforderung. | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| **time** | Zeichenfolge | Die Zeitpunkt des Ereignisses im TimeSpan-UTC-Format `YYYY-MM-DDTHH:MM:SS.00000Z`. | `2017-06-28T21:00:27.3534352Z` |
|**triggerId**| Zeichenfolge| Die ID der Triggerausführung. | `08587023010602533858661257311` |
|**Ressourcen-ID**| Zeichenfolge | Die mit der Data Factory-Ressource verknüpfte ID. | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|**category**| Zeichenfolge | Die Kategorie der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `PipelineRuns` fest. | `PipelineRuns` |
|**level**| Zeichenfolge | Die Ebene der Diagnoseprotokolle. Legen Sie den Eigenschaftswert auf `Informational` fest. | `Informational` |
|**operationName**| Zeichenfolge | Der Name des Triggers mit seinem abschließenden Status, der angibt, ob der Trigger erfolgreich ausgelöst wurde. Wenn der Heartbeat erfolgreich war, lautet der Eigenschaftswert `MyTrigger - Succeeded`. | `MyTrigger - Succeeded` |
|**triggerName**| Zeichenfolge | Der Name des Triggers. | `MyTrigger` |
|**triggerType**| Zeichenfolge | Typ des Triggers. Mögliche Eigenschaftswerte sind `Manual Trigger` und `Schedule Trigger`. | `ScheduleTrigger` |
|**triggerEvent**| Zeichenfolge | Das Ereignis des Triggers. | `ScheduleTime - 2017-07-06T01:50:25Z` |
|**start**| Zeichenfolge | Die Startzeit der Triggerauslösung im TimeSpan-UTC-Format. | `2017-06-26T20:55:29.5007959Z`|
|**status**| Zeichenfolge | Der abschließende Status, der anzeigt, ob der Trigger erfolgreich ausgelöst wurde. Mögliche Eigenschaftswerte sind `Succeeded` und `Failed`. | `Succeeded`|

### <a name="log-analytics-schema"></a>Log Analytics-Schema

Log Analytics erbt das Schema von Monitor. Dabei gelten jedoch folgende Ausnahmen:

* Der erste Buchstabe in den einzelnen Spaltennamen wird in Großbuchstaben geschrieben. Der Spaltenname „correlationId“ in Monitor lautet in Log Analytics beispielsweise „CorrelationId“.
* Es gibt keine Spalte „Level“.
* Die dynamische Spalte „Eigenschaften“ wird als der folgende dynamische JSON-Blob-Typ beibehalten:

    | Azure Monitor-Spalte | Log Analytics-Spalte | type |
    | --- | --- | --- |
    | $.properties.UserProperties | UserProperties | Dynamisch |
    | $.properties.Annotations | Anmerkungen | Dynamisch |
    | $.properties.Input | Eingabe | Dynamisch |
    | $.properties.Output | Output | Dynamisch |
    | $.properties.Error.errorCode | ErrorCode | int |
    | $.properties.Error.message | ErrorMessage | Zeichenfolge |
    | $.properties.Error | Error | Dynamisch |
    | $.properties.Predecessors | Predecessors | Dynamisch |
    | $.properties.Parameters | Parameter | Dynamisch |
    | $.properties.SystemParameters | SystemParameters | Dynamisch |
    | $.properties.Tags | `Tags` | Dynamisch |
    
## <a name="metrics"></a>metrics

Mit Monitor erhalten Sie Einblicke in die Leistung und Integrität ihrer Azure-Workloads. Metriken sind die wichtigsten Monitor-Datentypen. Sie werden auch als Leistungsindikatoren bezeichnet. Metriken werden von den meisten Azure-Ressourcen ausgegeben. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese Metriken für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.

Version 2 von Azure Data Factory gibt die folgenden Metriken aus:

| **Metrik**           | **Anzeigename der Metrik**         | **Einheit** | **Aggregationstyp** | **Beschreibung**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRuns | Metriken zu erfolgreichen Pipelineausführungen | Count    | Gesamt                | Die Gesamtanzahl der erfolgreichen Pipelineausführungen innerhalb eines Minutenfensters. |
| PipelineFailedRuns   | Metriken zu fehlerhaften Pipelineausführungen    | Count    | Gesamt                | Die Gesamtanzahl der fehlgeschlagenen Pipelineausführungen innerhalb eines Minutenfensters.    |
| ActivitySucceededRuns | Metriken zu erfolgreichen Aktivitätsausführungen | Count    | Gesamt                | Die Gesamtanzahl der erfolgreichen Aktivitätsausführungen innerhalb eines Minutenfensters.  |
| ActivityFailedRuns   | Metriken zu fehlerhaften Aktivitätsausführungen    | Count    | Gesamt                | Die Gesamtanzahl der fehlgeschlagenen Aktivitätsausführungen innerhalb eines Minutenfensters.     |
| TriggerSucceededRuns | Metriken zu erfolgreichen Triggerausführungen  | Count    | Gesamt                | Die Gesamtanzahl der erfolgreichen Triggerausführungen innerhalb eines Minutenfensters.   |
| TriggerFailedRuns    | Metriken zu fehlerhaften Triggerausführungen     | Count    | Gesamt                | Die Gesamtanzahl der fehlgeschlagenen Triggerausführungen innerhalb eines Minutenfensters.      |

Folgen Sie den Anweisungen unter [Azure Monitor-Datenplattform](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), um auf die Metriken zuzugreifen.

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Überwachen von Data Factory-Metriken mit Azure Monitor

Sie können die Data Factory-Integration in Monitor zur Weiterleitung von Daten an Monitor verwenden. Diese Integration ist in den folgenden Szenarien nützlich:

* Sie möchten komplexe Abfragen auf einen umfangreichen Satz von Metriken schreiben, die von Data Factory an Monitor veröffentlicht werden. Sie können eigene Benachrichtigungen zu diesen Abfragen über Monitor erstellen.

* Sie möchten Data Factorys übergreifend überwachen. Sie können Daten aus mehreren Data Factorys an einen einzigen Monitor-Arbeitsbereich weiterleiten.

Das folgende Video enthält eine siebenminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Konfigurieren von Diagnoseeinstellungen und Arbeitsbereich

Erstellen Sie Diagnoseeinstellungen für Ihre Data Factory, oder fügen Sie diese hinzu.

1. Navigieren Sie im Portal zu „Monitor“. Wählen Sie **Einstellungen** > **Diagnoseeinstellungen** aus.

1. Wählen Sie die Data Factory aus, für die Sie eine Diagnoseeinstellung festlegen möchten.

1. Sollten für die ausgewählte Data Factory keine Einstellungen vorhanden sein, werden Sie aufgefordert, eine Einstellung zu erstellen. Wählen Sie **Diagnose aktivieren**.

   ![Erstellen einer Diagnoseeinstellung, wenn keine Einstellungen vorhanden sind](media/data-factory-monitor-oms/monitor-oms-image1.png)

   Sind Einstellungen für die Data Factory vorhanden, wird eine Liste mit den Einstellungen angezeigt, die bereits für die Data Factory konfiguriert sind. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.

   ![Hinzufügen einer Diagnoseeinstellung, wenn Einstellungen vorhanden sind](media/data-factory-monitor-oms/add-diagnostic-setting.png)

1. Legen Sie einen Namen für Ihre Einstellung fest, wählen Sie **An Log Analytics senden** und dann unter **Log Analytics-Arbeitsbereich** einen Arbeitsbereich aus.

    ![Benennen Ihrer Einstellungen und Auswählen eines Log Analytics-Arbeitsbereichs](media/data-factory-monitor-oms/monitor-oms-image2.png)

1. Wählen Sie **Speichern** aus.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Data Factory angezeigt. Diagnoseprotokolle werden in diesen Arbeitsbereich gestreamt, sobald neue Ereignisdaten generiert werden. Zwischen der Ausgabe eines Ereignisses und dessen Anzeige in Log Analytics können bis zu 15 Minuten vergehen.

* Im Modus _Ressourcenspezifisch_ werden Diagnoseprotokolle vom Azure Data Factory-Flow in die Tabellen _ADFPipelineRun_, _ADFTriggerRun_ und _ADFActivityRun_ übernommen.
* Im Modus _Azure-Diagnose_ werden Diagnoseprotokolle in die Tabelle _AzureDiagnostics_ übernommen.

> [!NOTE]
> Da eine Azure-Protokolltabelle nicht mehr als 500 Spalten enthalten darf, wird dringend empfohlen, den ressourcenspezifischen Modus auszuwählen. Weitere Informationen finden Sie unter [Bekannte Einschränkung: Spaltenlimit in Azure-Diagnose](../azure-monitor/platform/resource-logs-collect-workspace.md#column-limit-in-azurediagnostics).

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Installieren von Azure Data Factory-Analysen aus dem Azure Marketplace

![Navigieren Sie zu „Azure Marketplace“, geben Sie „Analysefilter“ ein, und wählen Sie „Azure Data Factory-Analyse (Vorschau)“ aus.](media/data-factory-monitor-oms/monitor-oms-image3.png)

![Details zu „Azure Data Factory-Analyse (Vorschau)“](media/data-factory-monitor-oms/monitor-oms-image4.png)

Wählen Sie **Erstellen** und dann **OMS-Arbeitsbereich** und **OMS-Arbeitsbereichseinstellungen** aus.

![Erstellen einer neuen Lösung](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Überwachen von Data Factory-Metriken

Durch die Installation von Azure Data Factory-Analysen wird ein Standardsatz von Ansichten für die folgenden Metriken erstellt:

- ADF-Ausführungen – 1) Pipelineausführungen nach Data Factory
 
- ADF-Ausführungen – 2) Aktivitätsausführungen nach Data Factory

- ADF-Ausführungen – 3) Triggerausführungen nach Data Factory

- ADF-Fehler – 1) Top 10-Pipelinefehler nach Data Factory

- ADF-Fehler – 2) Top 10-Aktivitätsausführungen nach Data Factory

- ADF-Fehler – 3) Top 10-Triggerfehler nach Data Factory

- ADF-Statistik – 1) Aktivitätsausführungen nach Typ

- ADF-Statistik – 2) Triggerausführungen nach Typ

- ADF-Statistik – 3) Höchstdauer der Pipelineausführungen

![Fenster mit den hervorgehobenen Optionen „Arbeitsmappen (Vorschau)“ und „AzureDataFactoryAnalytics“](media/data-factory-monitor-oms/monitor-oms-image6.png)

Sie können u.a. die voranstehenden Metriken visualisieren, die Abfragen hinter diesen Metriken betrachten, die Abfragen bearbeiten Benachrichtigungen erstellen oder andere Aktionen ausführen.

![Grafische Darstellung von Pipelineausführungen durch Data Factory](media/data-factory-monitor-oms/monitor-oms-image8.png)

> [!NOTE]
> Azure Data Factory Analytics (Vorschau) sendet Diagnoseprotokolle an _ressourcenspezifische_ Zieltabellen. Sie können Abfragen für die folgenden Tabellen schreiben: _ADFPipelineRun_, _ADFTriggerRun_ und _ADFActivityRun_.

## <a name="alerts"></a>Alerts

Melden Sie sich beim Azure-Portal an, und wählen Sie **Überwachen** > **Warnungen** aus, um Warnungen zu erstellen.

![Warnungen im Portalmenü](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Erstellen von Warnungen

1. Wählen Sie **+ Neue Warnungsregel** aus, um eine neue Warnung zu erstellen.

    ![Neue Warnungsregel](media/monitor-using-azure-monitor/alerts_image4.png)

1. Definieren Sie die Warnungsbedingung.

    > [!NOTE]
    > Achten Sie darauf, dass in der Dropdownliste **Nach Ressourcentyp filtern** die Option **Alle** ausgewählt ist.

    ![„Warnungsbedingung definieren“ > „Ziel auswählen“ öffnet den Bereich „Ressource auswählen“ ](media/monitor-using-azure-monitor/alerts_image5.png)

    ![„Warnungsbedingung definieren“ > „Kriterien hinzufügen“ öffnet den Bereich „Signallogik konfigurieren“](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Bereich „Signallogik konfigurieren“](media/monitor-using-azure-monitor/alerts_image7.png)

1. Definieren Sie die Warnungsdetails.

    ![Warnungsdetails](media/monitor-using-azure-monitor/alerts_image8.png)

1. Definieren Sie die Aktionsgruppe.

    ![Erstellen einer Regel, mit hervorgehobener Option „Neue Aktionsgruppe“](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Erstellen einer neuen Aktionsgruppe](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Konfigurieren von E-Mail, SMS, Push und Voice](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Definieren einer Aktionsgruppe](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Nächste Schritte
[Programmseitiges Überwachen und Verwalten von Pipelines](monitor-programmatically.md)
