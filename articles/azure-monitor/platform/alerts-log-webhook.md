---
title: Webhookaktionen für Protokollwarnungen in Azure-Warnungen
description: In diesem Artikel wird beschrieben, wie Sie eine Protokollwarnungsregel mit einem Log Analytics-Arbeitsbereich mit Application Insights erstellen, wie die Warnung Daten per Pushvorgang als HTTP-Webhook überträgt sowie die Details der verschiedenen möglichen Anpassungen.
author: yanivlavi
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: 3a072ae64104f8fded49ff6a00f5b58902c39903
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838569"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Webhookaktionen für Protokollwarnungsregeln
Wenn eine [Protokollwarnung in Azure erstellt](alerts-log.md) wird, haben Sie die Möglichkeit, [sie mithilfe von Aktionsgruppen so zu konfigurieren](action-groups.md), dass sie eine oder mehrere Aktionen ausführt. Dieser Artikel beschreibt die verschiedenen verfügbaren Webhookaktionen und zeigt, wie Sie einen benutzerdefinierten, JSON-basierten Webhook konfigurieren.

> [!NOTE]
> Sie können auch das [allgemeine Warnungsschema](https://aka.ms/commonAlertSchemaDocs) für Ihre Webhookintegrationen verwenden. Das allgemeine Warnungsschema bietet den Vorteil einer einzelnen erweiterbaren und einheitlichen Warnungsnutzlast für alle Benachrichtigungsdienste in Azure Monitor. Beachten Sie, dass beim allgemeinen Warnungsschema die benutzerdefinierte JSON-Option für Protokollwarnungen nicht berücksichtigt wird. Es wird die Nutzlast des allgemeinen Warnungsschemas übernommen, wenn diese ausgewählt ist, unabhängig von der Anpassung, die Sie möglicherweise auf Ebene der Warnungsregel vorgenommen haben. [Hier finden Sie Informationen zu den Definitionen des allgemeinen Warnungsschemas](https://aka.ms/commonAlertSchemaDefinitions).

## <a name="webhook-actions"></a>Webhookaktionen

Mit Webhookaktionen können Sie einen externen Prozess über eine einzige HTTP POST-Anforderung aufrufen. Der aufgerufene Dienst sollte Webhooks unterstützen, und es sollte festgelegt werden, wie empfangene Nutzlasten verwendet werden.

Webhook-Aktionen erfordern die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| **Webhook-URL** |Die URL des Webhooks. |
| **Benutzerdefinierte JSON-Nutzlast** |Die benutzerdefinierte Nutzlast, die mit dem Webhook gesendet werden soll, wenn diese Option während der Warnungserstellung ausgewählt wird. Weitere Informationen finden Sie unter [Verwalten von Protokollwarnungen](alerts-log.md).|

> [!NOTE]
> Die Schaltfläche **Webhook anzeigen** zusammen mit der Option **Benutzerdefinierte JSON-Nutzlast für Webhook einschließen** für die Protokollwarnung zeigt die Beispielnutzlast für den Webhook für die bereitgestellte Anpassung an. Es sind keine tatsächlichen Daten enthalten, aber sie ist repräsentativ für ein JSON-Schema, das für Protokollwarnungen verwendet wird. 

Webhooks enthalten eine URL und eine Nutzlast im JSON-Format, wobei es sich um die an den externen Dienst gesendeten Daten handelt. Die Nutzlast enthält standardmäßig die Werte in der folgenden Tabelle. Sie können diese Nutzlast auch durch eine benutzerdefinierte eigene Nutzlast ersetzen. Verwenden Sie in diesem Fall die Variablen in der Tabelle für die einzelnen Parameter, um deren Wert in die benutzerdefinierte Nutzlast einzubinden.


| Parameter | Variable | BESCHREIBUNG |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |Der Name der Warnungsregel. |
| *Severity* |#severity |Festgelegter Schweregrad für die erste ausgelöste Protokollwarnung |
| *AlertThresholdOperator* |#thresholdoperator |Schwellenwertoperator für die Warnungsregel, der größer als oder kleiner als verwendet. |
| *AlertThresholdValue* |#thresholdvalue |Wert des Schwellenwerts für die Warnungsregel |
| *LinkToSearchResults* |#linktosearchresults |Link zum Analyseportal, das die Datensätze aus der Abfrage zurückgibt, mit der die Warnung erstellt wurde. |
| *ResultCount* |#searchresultcount |Anzahl von Datensätzen in den Suchergebnissen |
| *Endzeit für das Suchintervall* |#searchintervalendtimeutc |Abschlusszeit der Abfrage im UTC-Format, mit dem Format mm/tt/jjjj HH:mm:ss AM/PM. |
| *Suchintervall* |#searchinterval |Zeitfenster für die Warnungsregel im Format HH:mm:ss. |
| *Startzeit für das Suchintervall* |#searchintervalstarttimeutc |Startzeit der Abfrage im UTC-Format, mit dem Format mm/tt/jjjj HH:mm:ss AM/PM. 
| *SearchQuery* |#searchquery |Von der Warnungsregel verwendete Protokollsuchabfrage |
| *SearchResults* |"IncludeSearchResults": true|Von der Abfrage als JSON-Tabelle zurückgegebene Datensätze, die auf die ersten 1.000 Datensätze beschränkt sind; wenn „"IncludeSearchResults": true“ in einer benutzerdefinierten JSON-Webhookdefinition als Eigenschaft der obersten Ebene hinzugefügt wird. |
| *Warnungstyp*| #alerttype | Der Typ der konfigurierten Protokollwarnungsregel als [metrische Maßeinheit](alerts-unified-log.md#metric-measurement-alert-rules) oder [Anzahl von Ergebnissen](alerts-unified-log.md#number-of-results-alert-rules).|
| *WorkspaceID* |#workspaceid |ID Ihres Log Analytics-Arbeitsbereichs. |
| *Anwendungs-ID* |#applicationid |ID Ihrer Application Insights-App. |
| *Abonnement-ID* |#subscriptionid |ID des von Ihnen verwendeten Azure-Abonnements. 

> [!NOTE]
> *LinkToSearchResults* übergibt Parameter wie *SearchQuery*, *Startzeit für das Suchintervall* und *Endzeit für das Suchintervall* in der URL an das Azure-Portal, damit sie im Analytics-Abschnitt angezeigt werden können. Das Azure-Portal besitzt ein Limit für die Größe des URIs von ungefähr 2.000 Zeichen. Das Portal öffne *keine* in Warnungen bereitgestellten Links, wenn die Parameterwerte das Limit überschreiten. Sie können Details manuell eingeben, um Ergebnisse im Analytics-Portal anzuzeigen. Oder Sie können auch die [Application Insights Analytics-REST-API](https://dev.applicationinsights.io/documentation/Using-the-API) oder die [Log Analytics-REST-API](/rest/api/loganalytics/) zum programmgesteuerten Abrufen der Ergebnisse verwenden. 

Sie können beispielsweise die folgende benutzerdefinierte Nutzlast angeben, die einen einzelnen Parameter wie *text*enthält. Der Dienst, der von diesem Webhook aufgerufen wird, erwartet diesen Parameter.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Diese Beispielnutzlast wird ähnlich wie hier dargestellt aufgelöst, wenn sie an den Webhook gesendet wird:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Da alle Variablen in einem benutzerdefinierten Webhook innerhalb einer JSON-Umschließung wie „#searchinterval“ angegeben werden müssen, weist auch der resultierende Webhook Variablendaten in Umschließungen auf, etwa „00:05:00“.

Um Suchergebnisse in eine benutzerdefinierte Nutzlast einzuschließen, muss **IncudeSearchResults** als Eigenschaft der obersten Ebene in der JSON-Nutzlast festgelegt werden. 

## <a name="sample-payloads"></a>Beispielnutzlasten
Dieser Abschnitt zeigt Beispielnutzlasten für Webhooks für Protokollwarnungen. Die Beispielnutzlasten enthalten Beispiele dafür, wenn die Nutzlast Standard ist und wenn sie benutzerdefiniert ist.

### <a name="standard-webhook-for-log-alerts"></a>Standardwebhook für Protokollwarnungen 
Beide Beispiele enthalten eine Pseudonutzlast mit nur zwei Spalten und zwei Zeilen.

#### <a name="log-alert-for-log-analytics"></a>Protokollwarnung für Log Analytics
Die folgende Beispielnutzlast ist für eine Standardwebhookaktion *ohne benutzerdefinierte JSON-Option* gedacht, die für Warnungen verwendet wird, die auf Log Analytics basieren.

```json
{
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": "log alert rule",
    "Severity": "Warning",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
 }
 ```

> [!NOTE]
> Der Wert des Felds „Schweregrad“ könnte sich ändern, wenn Sie [Ihre API-Einstellung für Protokollwarnungen in Log Analytics gewechselt](alerts-log-api-switch.md) haben.


#### <a name="log-alert-for-application-insights"></a>Protokollwarnung für Application Insights
Die folgende Beispielnutzlast ist für einen Standardwebhook *ohne benutzerdefinierte JSON-Option* gedacht, wenn er für Protokollwarnungen verwendet wird, die auf Application Insights basieren.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"requests | where resultCode == \"500\"",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
    "AlertType": "Number of results"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Protokollwarnung mit benutzerdefinierter JSON-Nutzlast
Sie können beispielsweise Folgendes verwenden, um eine benutzerdefinierte Nutzlast zu erstellen, die nur den Warnungsnamen und die Suchergebnisse enthält: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Die folgende Beispielnutzlast ist für eine benutzerdefinierte Webhookaktion für eine beliebige Protokollwarnung gedacht:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResults":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Protokollwarnungen in Azure-Warnungen](alerts-unified-log.md).
- Grundlegendes zum [Verwalten von Protokollwarnungen in Azure](alerts-log.md).
- Erstellen und Verwalten von [Aktionsgruppen in Azure](action-groups.md).
- Erfahren Sie mehr über [Application Insights](../../azure-monitor/app/analytics.md).
- Weitere Informationen zum [Analysieren von Protokolldaten in Azure Monitor](../log-query/log-query-overview.md). 

