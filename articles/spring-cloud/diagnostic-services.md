---
title: Analysieren von Protokollen und Metriken in Azure Spring Cloud | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Diagnosedaten in Azure Spring Cloud analysieren.
services: spring-cloud
author: jpconnock
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: e6b90fb09c536f68bee7fd5d57507fe3920bcf1e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038717"
---
# <a name="analyze-logs-and-metrics-with-diagnostic-settings"></a>Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen

Mithilfe der Diagnosefunktionen von Azure Spring Cloud können Sie Protokolle und Metriken mit einem der folgenden Dienste analysieren:

* Analysieren Sie sie mit Azure Log Analytics, wobei die Daten sofort in Azure Log Analytics geschrieben werden, ohne sie zuerst in den Speicher schreiben zu müssen.
* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem Speicherkonto gespeichert werden. Sie können die Vermerkdauer (in Tagen) angeben.
* Sie können sie zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung an Event Hubs streamen.

Zum Einstieg müssen Sie einen dieser Dienste aktivieren, um die Daten empfangen zu können.  Weitere Informationen zum Konfigurieren von Log Analytics finden Sie in [diesem Tutorial](../azure-monitor/log-query/get-started-portal.md).  

## <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Instanz.
1. Wählen Sie die Menüoption **Diagnoseeinstellungen** aus.
1. Wählen Sie die Schaltfläche **Diagnoseeinstellung hinzufügen** aus.
1. Geben Sie einen Namen für die Einstellung ein, und wählen Sie aus, wohin die Protokolle übermittelt werden sollen. Sie können eine beliebige Kombination der folgenden drei Optionen auswählen:
    * In einem Speicherkonto archivieren
    * An einen Event Hub streamen
    * An Log Analytics senden

1. Wählen Sie die zu überwachende Protokoll- und Metrikkategorie aus, und geben Sie die Vermerkdauer (in Tagen) an. Die Vermerkdauer gilt nur für das Speicherkonto.
1. Wählen Sie **Speichern** aus, um die Einstellung zu übernehmen.

> [!NOTE]
> Zwischen der Ausgabe der Protokolle oder Metriken und der Anzeige Speicherkonto, in Event Hubs oder in Log Analytics können bis zu 15 Minuten vergehen.

## <a name="viewing-logs"></a>Anzeigen von Protokollen

### <a name="using-log-analytics"></a>Verwenden von Log Analytics

1. Wählen Sie im Azure-Portal im Navigationsmenü auf der linken Seite „Log Analytics“ aus.
1. Wählen Sie den Log Analytics-Arbeitsbereich aus, dem Sie die Diagnoseeinstellungen hinzugefügt haben.
1. Wählen Sie `Logs` aus, um das Blatt „Protokollsuche“ zu öffnen.
1. Geben Sie im Feld „Protokollsuche“ eine einfache Abfrage ein.  Beispiel:

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```

1. Wählen Sie `Run` aus, um die Suchergebnisse anzuzeigen.
1. Sie können die Protokolle der jeweiligen Anwendung oder Instanz suchen, indem Sie eine Filterbedingung festlegen:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```

[In diesem Artikel](../azure-monitor/log-query/query-language.md) erfahren Sie mehr über die Abfragesprache, die in Log Analytics verwendet wird.

### <a name="using-logs-and-metrics-in-storage-account"></a>Verwenden von Protokollen und Metriken in Speicherkonten

1. Wählen Sie im Azure-Portal im Navigationsmenü auf der linken Seite „Speicherkonten“ aus.
1. Wählen Sie das Speicherkonto aus, dem Sie die Diagnoseeinstellungen hinzugefügt haben.
1. Wählen Sie den Eintrag `Blobs` aus, um das Blatt „Blobcontainer“ zu öffnen.
1. Suchen Sie den Container `insights-logs-applicationconsole`, um die Anwendungsprotokolle zu überprüfen.
1. Suchen Sie den Container `insights-metrics-pt1m`, um die Anwendungsmetriken zu überprüfen.

[Erfahren Sie mehr über das Senden von Diagnoseinformationen an ein Speicherkonto.](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-to-storage)

### <a name="using-event-hubs"></a>Verwenden von Event Hubs

1. Wählen Sie im Azure-Portal im Navigationsmenü auf der linken Seite „Event Hubs“ aus.
1. Wählen Sie die Event Hubs-Instanz aus, der Sie die Diagnoseeinstellungen hinzugefügt haben.
1. Wählen Sie `Event Hubs` aus, um das Blatt „Event Hub-Liste“ zu öffnen.
1. Suchen Sie den Event Hub `insights-logs-applicationconsole`, um die Anwendungsprotokolle zu überprüfen.
1. Suchen Sie den Event Hub `insights-metrics-pt1m`, um die Anwendungsmetriken zu überprüfen.

[Erfahren Sie mehr über das Senden von Diagnoseinformationen an einen Event Hub.](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs)

## <a name="analyzing-logs"></a>Analysieren von Protokollen

Azure Log Analytics stellt Kusto bereit, damit Sie Ihre Protokolle zur Analyse abfragen können.  Lesen Sie das [Log Analytics-Tutorial ](../azure-monitor/log-query/get-started-portal.md), um eine kurze Einführung in das Abfragen von Protokollen mithilfe von Kusto zu erhalten.

Anwendungsprotokolle bieten wichtige Informationen zu Ihrer Anwendung wie Integrität, Leistung u. v. m.  Im Folgenden finden Sie einige einfache Abfragen, die Ihnen helfen, die aktuellen und früheren Zustände Ihrer Anwendung besser zu verstehen.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Anzeigen von Anwendungsprotokollen aus Azure Spring Cloud

So überprüfen Sie eine Liste mit Anwendungsprotokollen aus Azure Spring Cloud, sortiert nach der Zeit mit den neuesten Protokollen zuerst

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Anzeigen von Protokolleinträgen mit Fehlern oder Ausnahmen

Mit dieser Abfrage können Sie Protokolleinträge überprüfen, die einen Fehler oder eine Ausnahme enthalten.  Die Ergebnisse sind nicht sortiert.

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Verwenden Sie diese Abfrage, um Fehler zu finden, oder ändern Sie die Abfragebedingungen, um bestimmte Fehlercodes oder Ausnahmen zu suchen.  

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Anzeigen der Anzahl von Fehlern und Ausnahmen, die von Ihrer Anwendung in der letzten Stunde gemeldet wurden

Diese Abfrage erstellt ein Kreisdiagramm mit der Anzahl der Fehler und Ausnahmen, die in der letzten Stunde von Ihrer Anwendung protokolliert wurden:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Weitere Informationen zum Abfragen von Anwendungsprotokollen

Azure Monitor bietet umfassende Unterstützung für das Abfragen von Anwendungsprotokollen mithilfe von Log Analytics.  Weitere Informationen zu diesem Dienst finden Sie im Tutorial zu [Protokollabfragen](../azure-monitor/log-query/get-started-queries.md) mit Azure Monitor. Die [Übersicht über Protokollabfragen in Azure Monitor](../azure-monitor/log-query/log-query-overview.md) bietet weitere Informationen zum Erstellen von Abfragen für die Analyse Ihrer Anwendungsprotokolle.
