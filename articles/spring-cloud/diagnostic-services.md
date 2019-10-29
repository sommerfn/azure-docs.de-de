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
ms.openlocfilehash: 955641f3511989baa5bfc3c0fa4d7df7ccbf9bfa
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554579"
---
# <a name="analyze-logs-and-metrics-with-diagnostics-settings"></a>Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen

Mithilfe der Diagnosefunktionen von Azure Spring Cloud können Sie Protokolle und Metriken mit jedem der folgenden Dienste analysieren:

* Verwenden Sie Azure Log Analytics, bei dem die Daten sofort geschrieben werden, ohne dass Sie zuerst in den Speicher geschrieben werden müssten.
* Speichern Sie sie zur Überwachung oder manuellen Überprüfung in einem Speicherkonto. Sie können die Vermerkdauer (in Tagen) angeben.
* Sie können sie zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung an Event Hubs streamen.

Zum Einstieg aktivieren Sie einen dieser Dienste, um die Daten empfangen zu können. Weitere Informationen zum Konfigurieren von Log Analytics finden Sie unter [Erste Schritte mit Log Analytics in Azure Monitor](../azure-monitor/log-query/get-started-portal.md). 

## <a name="configure-diagnostics-settings"></a>Konfigurieren von Diagnoseeinstellungen

1. Wechseln Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Instanz.
1. Wählen Sie die Option **Diagnoseeinstellungen** aus, und wählen Sie dann die **Diagnoseeinstellung hinzufügen** aus.
1. Geben Sie einen Namen für die Einstellung ein, und wählen Sie dann aus, wohin die Protokolle gesendet werden sollen. Sie können eine beliebige Kombination der folgenden drei Optionen auswählen:
    * **In einem Speicherkonto archivieren**
    * **An einen Event Hub streamen**
    * **An Log Analytics senden**

1. Wählen Sie die zu überwachende Protokoll- und Metrikkategorie aus, und geben Sie dann die Vermerkdauer (in Tagen) an. Die Vermerkdauer gilt nur für das Speicherkonto.
1. Wählen Sie **Speichern** aus.

> [!NOTE]
> Zwischen der Ausgabe der Protokolle oder Metriken und der Anzeige in Ihrem Speicherkonto, Ihrem Event Hub oder in Log Analytics kann eine Verzögerung von bis zu 15 Minuten liegen.

## <a name="view-the-logs"></a>Anzeigen der Protokolle

### <a name="use-log-analytics"></a>Verwenden von Log Analytics

1. Wählen Sie im Azure-Portal im linken Bereich **Log Analytics** aus.
1. Wählen Sie den Log Analytics-Arbeitsbereich aus, den Sie beim Hinzufügen Ihrer Diagnoseeinstellungen ausgewählt haben.
1. Um den Bereich **Protokollsuche** zu öffnen, wählen Sie **Protokolle** aus.
1. Geben Sie in das **Protokoll**suchfeld eine einfache Abfrage ein.

    ```sql
    AppPlatformLogsforSpring
    | limit 50
    ```

1. Um das Suchergebnis anzuzeigen, wählen Sie **Ausführen** aus.
1. Sie können die Protokolle der jeweiligen Anwendung oder Instanz suchen, indem Sie eine Filterbedingung festlegen:

    ```sql
    AppPlatformLogsforSpring
    | where ServiceName == "YourServiceName" and AppName == "YourAppName" and InstanceName == "YourInstanceName"
    | limit 50
    ```

Weitere Informationen zu der in Log Analytics verwendeten Abfragesprache erhalten Sie unter [Azure Monitor-Protokollabfragen](../azure-monitor/log-query/query-language.md).

### <a name="use-your-storage-account"></a>Verwenden Ihres Speicherkontos 

1. Wählen Sie im Azure-Portal im linken Bereich **Speicherkonten** aus.

1. Wählen Sie das Speicherkonto aus, das Sie beim Hinzufügen Ihrer Diagnoseeinstellungen ausgewählt haben.
1. Um den Bereich **Blobcontainer** zu öffnen, wählen Sie **Blobs** aus.
1. Um Anwendungsprotokolle zu überprüfen, suchen Sie nach einem Container mit dem Namen **insights-logs-applicationconsole**.
1. Um Anwendungsmetriken zu überprüfen, suchen Sie nach einem Container mit dem Namen **insights-metrics-pt1m**.

Weitere Informationen zum Senden von Diagnoseinformationen an ein Speicherkonto finden Sie unter [Speichern und Anzeigen von Diagnosedaten in Azure Storage](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-to-storage).

### <a name="use-your-event-hub"></a>Verwenden Ihres Event Hubs

1. Wählen Sie im Azure-Portal im linken Bereich **Event Hubs** aus.

1. Suchen Sie nach dem Event Hub, den Sie beim Hinzufügen Ihrer Diagnoseeinstellungen ausgewählt haben, und wählen Sie ihn aus.
1. Um den Bereich **Event Hub-Liste** zu öffnen, wählen Sie **Event Hubs** aus.
1. Um Anwendungsprotokolle zu überprüfen, suchen Sie nach einem Event Hub mit dem Namen **insights-logs-applicationconsole**.
1. Um Anwendungsmetriken zu überprüfen, suchen Sie nach einem Event Hub mit dem Namen **insights-metrics-pt1m**.

Weitere Informationen zum Senden von Diagnoseinformationen an einen Event Hub finden Sie unter [Streamen von Azure-Diagnosedaten im langsamsten Pfad mithilfe von Event Hubs](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-stream-event-hubs).

## <a name="analyze-the-logs"></a>Analysieren der Protokolle

Azure Log Analytics stellt Kusto bereit, damit Sie Ihre Protokolle zur Analyse abfragen können. Eine kurze Einführung in das Abfragen mithilfe von Kusto finden Sie im [Log Analytics-Tutorial ](../azure-monitor/log-query/get-started-portal.md).

Anwendungsprotokolle bieten wichtige Informationen zu Ihrer Anwendung wie Integrität, Leistung u. v. m. In den nächsten Abschnitten finden Sie einige einfache Abfragen, die Ihnen helfen, die aktuellen und früheren Zustände Ihrer Anwendung besser zu verstehen.

### <a name="show-application-logs-from-azure-spring-cloud"></a>Anzeigen von Anwendungsprotokollen aus Azure Spring Cloud

Um eine Liste mit Anwendungsprotokollen aus Azure Spring Cloud zu überprüfen, sortiert nach der Zeit mit den neuesten Protokollen zuerst, führen Sie die folgende Abfrage aus:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| sort by TimeGenerated desc
```

### <a name="show-logs-entries-containing-errors-or-exceptions"></a>Anzeigen von Protokolleinträgen mit Fehlern oder Ausnahmen

Um unsortierte Protokolleinträge zu überprüfen, die einen Fehler oder eine Ausnahme enthalten, führen Sie die folgende Abfrage aus:

```sql
AppPlatformLogsforSpring
| project TimeGenerated , ServiceName , AppName , InstanceName , Log
| where Log contains "error" or Log contains "exception"
```

Verwenden Sie diese Abfrage, um Fehler zu finden, oder ändern Sie die Abfragebedingungen, um bestimmte Fehlercodes oder Ausnahmen zu suchen. 

### <a name="show-the-number-of-errors-and-exceptions-reported-by-your-application-over-the-last-hour"></a>Anzeigen der Anzahl von Fehlern und Ausnahmen, die von Ihrer Anwendung in der letzten Stunde gemeldet wurden

Um ein Kreisdiagramm zu erstellen, das die Anzahl der Fehler und Ausnahmen darstellt, die in der letzten Stunde von Ihrer Anwendung protokolliert wurden, führen Sie die folgende Abfrage aus:

```sql
AppPlatformLogsforSpring
| where TimeGenerated > ago(1h)
| where Log contains "error" or Log contains "exception"
| summarize count_per_app = count() by AppName
| sort by count_per_app desc
| render piechart
```

### <a name="learn-more-about-querying-application-logs"></a>Weitere Informationen zum Abfragen von Anwendungsprotokollen

Azure Monitor bietet umfassende Unterstützung für das Abfragen von Anwendungsprotokollen mithilfe von Log Analytics. Weitere Informationen zu diesem Dienst finden Sie unter [Erste Schritte mit Protokollabfragen in Azure Monitor](../azure-monitor/log-query/get-started-queries.md). Weitere Informationen zum Erstellen von Abfragen zur Analyse Ihrer Anwendungsprotokolle finden Sie unter [Übersicht über Protokollabfragen in Azure Monitor](../azure-monitor/log-query/log-query-overview.md).
