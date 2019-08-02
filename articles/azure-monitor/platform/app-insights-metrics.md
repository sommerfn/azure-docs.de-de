---
title: Azure Application Insights – Protokollbasierte Metriken | Microsoft Docs
description: Dieser Artikel listet die Metriken von Azure Application Insights mit unterstützten Aggregationen und Dimensionen auf. Zu den Details der protokollbasierten Metriken gehören die zugrunde liegenden Kusto-Abfrageanweisungen.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 07/03/2019
ms.author: vitalyg
ms.subservice: application-insights
ms.openlocfilehash: d4b7a214af23d69f1217d84e9401de230cd358b0
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877432"
---
# <a name="application-insights-log-based-metrics"></a>Application Insights protokollenbasierte Metriken

Mit den protokollbasierten Metriken von Application Insights können Sie den Zustand Ihrer überwachten Anwendungen analysieren, leistungsstarke Dashboards erstellen und Warnmeldungen konfigurieren. Es gibt zwei Arten von Metriken:

* [Protokollbasierte Metriken](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#log-based-metrics) werden nicht sichtbar in [Kusto-Abfragen](https://docs.microsoft.com/azure/kusto/query/) aus gespeicherten Ereignissen übersetzt.
* [Standardmetriken](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics) werden als vorab aggregierte Zeitreihe gespeichert.

Da *Standardmetriken* während der Erfassung voraggregiert werden, haben sie eine bessere Performance bei der Abfragezeit. Dies macht sie zu einer besseren Wahl für Dashboarding und Echtzeit-Alarmierung. Die *protokollbasierten Metriken* haben mehr Dimensionen, was sie zur überlegenen Option für Datenanalyse und Ad-hoc-Diagnosen macht. Verwenden Sie [die Namespace-Auswahl](metrics-getting-started.md#create-your-first-metric-chart), um zwischen protokollbasierten und standardisierten Metriken im [Metrik-Explorer](metrics-getting-started.md) zu wechseln.

## <a name="interpret-and-use-queries-from-this-article"></a>Interpretieren und Verwenden von Abfragen aus diesem Artikel

Dieser Artikel listet Metriken mit unterstützten Aggregationen und Dimensionen auf. Zu den Details der protokollbasierten Metriken gehören die zugrunde liegenden Kusto-Abfrageanweisungen. Zur Vereinfachung verwendet jede Abfrage Standardwerte für Zeitgranularität, Diagrammtyp und manchmal auch Splitting-Dimensionen, was die Verwendung der Abfrage in Log Analytics vereinfacht, ohne dass Änderungen erforderlich sind.

Wenn Sie die gleiche Metrik im[Metrik- Explorer](metrics-getting-started.md) darstellen, gibt es keine Standardeinstellungen – die Abfrage wird dynamisch an Ihre Diagramm-Einstellungen angepasst:

- Der ausgewählte **Zeitbereich** wird in eine zusätzliche Klausel mit *Zeitstempel...* übersetzt, um nur die Ereignisse aus dem ausgewählten Zeitbereich auszuwählen. Beispielsweise enthält ein Diagramm, in dem Daten der letzten 24 Stunden angezeigt werden, die Abfrage  *| where timestamp > ago(24 h)* .

- Die ausgewählte **Zeitgranularität** wird in die endgültige *summarize ... by bin(timestamp, [time grain])* Klausel aufgenommen.

- Alle ausgewählten **Filter**dimensionen werden in zusätzliche *where*-Klauseln übersetzt.

- Die ausgewählte **Split-Diagramm**-Dimension wird in eine zusätzliche Summeneigenschaft übersetzt. Wenn Sie beispielsweise Ihr Diagramm nach *Standort* aufteilen und mit einer Zeitgranularität von 5 Minuten grafisch darstellen, wird die *Summen*klausel zusammengefasst *... by bin(timestamp, 5 m), location*.

> [!NOTE]
> Wenn Sie noch nicht mit der Kusto-Abfragesprache vertraut sind, müssen Sie zunächst die Kusto-Anweisungen kopieren und in den Log Analytics Abfragebereich einfügen, ohne Änderungen vorzunehmen. Klicken Sie auf **Ausführen**, um das grundlegende Diagramm anzuzeigen. Wenn Sie mit der Syntax der Abfragesprache vertraut sind, können Sie damit beginnen, kleine Änderungen vorzunehmen und die Auswirkung ihrer Änderung anzuzeigen. Das untersuchen ihrer eigenen Daten ist eine gute Möglichkeit, um die volle Leistung von [Log Analytics](../../azure-monitor/log-query/get-started-portal.md) und [Azure Monitor](../../azure-monitor/overview.md) zu realisieren.

## <a name="availability-metrics"></a>Verfügbarkeitsmetriken

Metriken in der Kategorie Verfügbarkeit ermöglichen es Ihnen, den Zustand Ihrer Webanwendung zu sehen, wie er von Punkten auf der ganzen Welt aus beobachtet wird. [Konfigurieren Sie die Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md), damit Sie Metriken aus dieser Kategorie verwenden können.

### <a name="availability-availabilityresultsavailabilitypercentage"></a>Verfügbarkeit (VerfügbarkeitErgebnisse/VerfügbarkeitProzentsatz)
Die Metrik *Verfügbarkeit* zeigt den Prozentsatz der Webtestläufe, von denen keine Probleme erkannt wurden. Der niedrigste mögliche Wert ist 0. Dies bedeutet, dass alle Webtestläufe fehlgeschlagen sind. Der Wert 100 bedeutet, dass alle Webtestläufe die Überprüfungskriterien bestanden haben.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Prozentsatz|Durchschnitt|Ausführungsort, Testname|

```Kusto
availabilityResults 
| summarize sum(todouble(success == 1) * 100) / count() by bin(timestamp, 5m), location
| render timechart
```

### <a name="availability-test-duration-availabilityresultsduration"></a>Dauer des Verfügbarkeitstests (VerfügbarkeitErgebnisse/Dauer)

Die Metrik *Verfügbarkeitstestdauer* gibt an, wie lange es gedauert hat, bis der Webtest ausgeführt wurde. Bei den [mehrstufigen Webtests](../../azure-monitor/app/availability-multistep.md) spiegelt die Metrik die gesamte Ausführungszeit aller Schritte wider.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Ausführungsort, Testname, Testergebnis

```Kusto
availabilityResults
| where notempty(duration)
| extend availabilityResult_duration = iif(itemType == 'availabilityResult', duration, todouble(''))
| summarize sum(availabilityResult_duration)/sum(itemCount) by bin(timestamp, 5m), location
| render timechart
```

### <a name="availability-tests-availabilityresultscount"></a>Verfügbarkeitstests (VerfügbarkeitErgebnisse/Anzahl)

Die Metrik *Verfügbarkeitstests* spiegelt die Anzahl der von Azure Monitor ausgeführten Webtests wider.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|---|---|---|
|Count|Count|Ausführungsort, Testname, Testergebnis|

```Kusto
availabilityResults
| summarize sum(itemCount) by bin(timestamp, 5m)
| render timechart
```

## <a name="browser-metrics"></a>Browsermetriken

Browsermetriken werden vom Application Insights JavaScript SDK aus echten Endbenutzer-Browsern gesammelt. Sie bieten nützliche Einblicke in die Erfahrungen Ihrer Benutzer mit Ihrer Webanwendung. Browsermetriken werden in der Regel nicht gesampelt, was bedeutet, dass sie eine höhere Genauigkeit der Nutzungszahlen bieten als serverseitige Metriken, die durch das Sampling verzerrt werden könnten.

> [!NOTE]
> Um Browsermetriken zu erfassen, muss Ihre Anwendung mit dem [Application Insights JavaScript SDK-Ausschnitt](../../azure-monitor/app/javascript.md#add-the-sdk-script-to-your-app-or-web-pages) ausgestattet sein.

### <a name="browser-page-load-time-browsertimingstotalduration"></a>Ladezeit der Browserseite (browserTimings/totalDuration)

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Keine|

```Kusto
browserTimings
| where notempty(totalDuration)
| extend _sum = totalDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="client-processing-time-browsertimingprocessingduration"></a>Clientverarbeitungszeit (browsertiming/Verarbeitungszeit)

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Keine|

```Kusto
browserTimings
| where notempty(processingDuration)
| extend _sum = processingDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum)/sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="page-load-network-connect-time-browsertimingsnetworkduration"></a>Netzwerkverbindungszeit für Laden der Seite (browsertimings/Netzwerkverbindungszeit)

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Keine|

```Kusto
browserTimings
| where notempty(networkDuration)
| extend _sum = networkDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="receiving-response-time-browsertimingsreceiveduration"></a>Empfangene Antwortzeit (browsertimings/Empfangsdauer)

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Keine|

```Kusto
browserTimings
| where notempty(receiveDuration)
| extend _sum = receiveDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="send-request-time-browsertimingssendduration"></a>Sendeanforderungszeit (browserTimings/Sendedauer)

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|
|---|---|---|
|Millisekunden|Durchschnittlich, Min, Max|Keine|

```Kusto
browserTimings
| where notempty(sendDuration)
| extend _sum = sendDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

## <a name="failure-metrics"></a>Fehlermetriken

Die Metriken in **Fehler** zeigen Probleme bei der Verarbeitung von Anforderungen, Abhängigkeitsaufrufen und ausgelösten Ausnahmen.

### <a name="browser-exceptions-exceptionsbrowser"></a>Browserausnahmen (Ausnahmen/Browser)

Diese Metrik spiegelt die Anzahl der ausgelösten Ausnahmen von Ihrem Anwendungscode im Browser wider. Nur Ausnahmen, die mit einem ```trackException()``` Application Insights API-Aufruf verfolgt werden, sind in der Metrik enthalten.

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|Notizen|
|---|---|---|---|
|Count|Count|Keine|Protokollbasierte Version verwendet **Sum**-Aggregation|

```Kusto
exceptions
| where notempty(client_Browser)
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="dependency-call-failures-dependenciesfailed"></a>Fehler beim Aufruf von Abhängigkeiten (Abhängigkeiten/ausgefallen)

Summe fehlgeschlagener Abhängigkeitsaufrufe.

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|Notizen|
|---|---|---|---|
|Count|Count|Keine|Protokollbasierte Version verwendet **Sum**-Aggregation|

```Kusto
dependencies
| where success == 'False'
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="exceptions-exceptionscount"></a>Ausnahmen (Ausnahmen/Anzahl)

Jedes Mal, wenn Sie eine Ausnahme bei Application Insights protokollieren, erfolgt ein Aufruf der [trackException()-Methode](../../azure-monitor/app/api-custom-events-metrics.md#trackexception) des SDK. Die Metrik Ausnahmen zeigt die Anzahl der protokollierten Ausnahmen an.

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|Notizen|
|---|---|---|---|
|Count|Count|Cloud-Rollenname, Cloud-Rolleninstanz, Gerätetyp|Protokollbasierte Version verwendet **Sum**-Aggregation|

```Kusto
exceptions
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="failed-requests-requestsfailed"></a>Fehlgeschlagene Anforderungen (Anforderungen/fehlgeschlagen)

Die Anzahl der verfolgten Serveranfragen, die als *fehlgeschlagen* markiert wurden. Standardmäßig markiert das Application Insights SDK automatisch jede Serveranfrage, die den HTTP-Antwortcode 5xx oder 4xx als fehlerhafte Anforderung zurückgibt. Sie können diese Logik anpassen, indem Sie die *Erfolgs*eigenschaft des Anfragetelemetrieelements in einem [benutzerdefinierten Telemetrie-Initialisierer](../../azure-monitor/app/api-filtering-sampling.md#add-properties-itelemetryinitializer) ändern.

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|Notizen|
|---|---|---|---|
|Count|Count|Cloud-Rolleninstanz, Cloud-Rollenname, Realer oder synthetischer Datenverkehr, Anforderungsperformance, Antwortcode|Protokollbasierte Version verwendet **Sum**-Aggregation|

```Kusto
requests
| where success == 'False'
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="server-exceptions-exceptionsserver"></a>Serverausnahmen (Ausnahmen/Server)

Diese Metrik zeigt die Anzahl der Serverausnahmen an.

|Unit of measure|Unterstützte Aggregationen|Vorab aggregierte Dimensionen|Notizen|
|---|---|---|---|
|Count|Count|Cloud-Rollenname, Cloud-Rolleninstanz|Protokollbasierte Version verwendet **Sum**-Aggregation|

```Kusto
exceptions
| where isempty(client_Browser)
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

## <a name="performance-counters"></a>Leistungsindikatoren

Verwenden Sie Metriken in der Kategorie **Leistungszähler**, um auf die von Application Insights gesammelten [Systemleistungs](../../azure-monitor/app/performance-counters.md)zähler zuzugreifen.

### <a name="available-memory-performancecountersavailablememory"></a>Verfügbarer Speicher (PerformanceCounter/verfügbarer Speicher)

```Kusto
performanceCounters
| where ((category == "Memory" and counter == "Available Bytes") or name == "availableMemory")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="exception-rate-performancecountersexceptionrate"></a>Ausnahmerate (PerformanceCounter/Ausnahmerate)

```Kusto
performanceCounters
| where ((category == ".NET CLR Exceptions" and counter == "# of Exceps Thrown / sec") or name == "exceptionRate")
| extend performanceCounter_value = iif(itemType == 'performanceCounter',value,todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-request-execution-time-performancecountersrequestexecutiontime"></a>Ausführungszeit der HTTP-Anfrage (performanceCounters/Bearbeitungszeit)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Request Execution Time") or name == "requestExecutionTime")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-request-rate-performancecountersrequestspersecond"></a>HTTP-Anforderungsrate (PerformanceCounter/AnfragenProSekunde)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Requests/Sec") or name == "requestsPerSecond")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-requests-in-application-queue-performancecountersrequestsinqueue"></a>HTTP-Anforderungen in der Anwendungswarteschlange (Performance Counters/AnfragenInWarteschlange)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Requests In Application Queue") or name == "requestsInQueue")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-cpu-performancecountersprocesscpupercentage"></a>Prozess-CPU (performanceCounters/processCpuProzent)

Die Metrik zeigt, wie viel der gesamten Prozessorleistung von dem Prozess verbraucht wird, der Ihre überwachte App hostet.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Prozentsatz|Durchschnittlich, Min, Max|Cloudrolleninstanz

```Kusto
performanceCounters
| where ((category == "Process" and counter == "% Processor Time Normalized") or name == "processCpuPercentage")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-io-rate-performancecountersprocessiobytespersecond"></a>Prozess-E/A-Rate (PerformanceCounters/processE/ABytesProSekunde)

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Bytes pro Sekunde|Durchschnittlich, Min, Max|Cloudrolleninstanz

```Kusto
performanceCounters
| where ((category == "Process" and counter == "IO Data Bytes/sec") or name == "processIOBytesPerSecond")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-private-bytes-performancecountersprocessprivatebytes"></a>Private Bytes verarbeiten (performanceCounters/processPrivateBytes)

Menge des nicht gemeinsam genutzten Speichers, die der überwachte Prozess für seine Daten zugewiesen hat.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Byte|Durchschnittlich, Min, Max|Cloudrolleninstanz

```Kusto
performanceCounters
| where ((category == "Process" and counter == "Private Bytes") or name == "processPrivateBytes")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="processor-time-performancecountersprocessorcpupercentage"></a>Prozessorzeit (PerformanceCounter/ProzessorCpuProzente)

CPU-Verbrauch durch *alle* Prozesse, die auf der überwachten Serverinstanz laufen.

|Unit of measure|Unterstützte Aggregationen|Unterstützte Dimensionen|
|---|---|---|
|Prozentsatz|Durchschnittlich, Min, Max|Cloudrolleninstanz

>[!NOTE]
> Die Prozessorzeitmetrik ist für die in Azure App Services gehosteten Anwendungen nicht verfügbar. Verwenden Sie die [Prozess-CPU](#process-cpu-performancecountersprocesscpupercentage)-Metrik, um die CPU-Auslastung der in App Services gehosteten Webanwendungen zu verfolgen.

```Kusto
performanceCounters
| where ((category == "Processor" and counter == "% Processor Time") or name == "processorCpuPercentage")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

## <a name="server-metrics"></a>Servermetriken

### <a name="dependency-calls-dependenciescount"></a>Aufruf von Abhängigkeiten (Abhängigkeiten/Anzahl)

Diese Metrik bezieht sich auf die Anzahl der Abhängigkeitsaufrufe.

```Kusto
dependencies
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="dependency-duration-dependenciesduration"></a>Abhängigkeitsdauer (Abhängigkeiten/Dauer)

Diese Metrik bezieht sich auf die Dauer von Abhängigkeitsaufrufen.

```Kusto
dependencies
| where notempty(duration)
| extend dependency_duration = iif(itemType == 'dependency',duration,todouble(''))
| extend _sum = dependency_duration
| extend _count = itemCount
| extend _sum = _sum*_count
| summarize sum(_sum)/sum(_count) by bin(timestamp, 1m)
| render timechart
```

### <a name="server-requests-requestscount"></a>Serveranfragen (Anfragen/Anzahl)

Diese Metrik spiegelt die Anzahl der eingehenden Serveranfragen wider, die von Ihrer Webanwendung empfangen wurden.

```Kusto
requests
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="server-response-time-requestsduration"></a>Server Antwortzeit (Anforderungen/Dauer)

Diese Metrik spiegelt die Zeit wider, die die Server für die Verarbeitung eingehender Anfragen benötigt haben.

```Kusto
requests
| where notempty(duration)
| extend request_duration = iif(itemType == 'request', duration, todouble(''))
| extend _sum = request_duration
| extend _count = itemCount
| extend _sum = _sum*_count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 1m)
| render timechart
```

## <a name="usage-metrics"></a>Nutzungsmetriken

### <a name="page-view-load-time-pageviewsduration"></a>Ladezeit der Seitenansicht (Pageviews/Dauer)

Diese Metrik bezieht sich auf die Zeit, die das Laden von PageView-Ereignissen benötigt hat.

```Kusto
pageViews
| where notempty(duration)
| extend pageView_duration = iif(itemType == 'pageView', duration, todouble(''))
| extend _sum = pageView_duration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render barchart
```

### <a name="page-views-pageviewscount"></a>Seitenaufrufe (Pageviews/Anzahl)

Die Anzahl der PageView-Ereignisse, die mit der TrackPageView() Application Insights API protokolliert wurden.

```Kusto
pageViews
| summarize sum(itemCount) by bin(timestamp, 1h)
| render barchart
```

### <a name="sessions-sessionscount"></a>Sitzungen (Sitzungen/Anzahl)

Diese Metrik bezieht sich auf die Anzahl der unterschiedlichen Sitzungs-IDs.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(session_Id)
| summarize dcount(session_Id) by bin(timestamp, 1h)
| render barchart
```

### <a name="traces-tracescount"></a>Ablaufverfolgungen (Ablaufverfolgungen/Anzahl)

Die Anzahl der mit dem TrackTrace() Application Insights API-Aufruf protokollierten Überwachungsanweisung.

```Kusto
traces
| summarize sum(itemCount) by bin(timestamp, 1h)
| render barchart
```

### <a name="users-userscount"></a>Benutzer (Benutzer/Anzahl)

Die Anzahl der unterschiedlichen Benutzer, die auf Ihre Anwendung zugegriffen haben. Die Genauigkeit dieser Metrik kann durch die Verwendung von Telemetriestichproben und Filtern erheblich beeinträchtigt werden.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(user_Id)
| summarize dcount(user_Id) by bin(timestamp, 1h)
| render barchart
```

### <a name="users-authenticated-usersauthenticated"></a>Benutzer, authentifiziert (Benutzer/authentifiziert)

Die Anzahl der unterschiedlichen Benutzer, die sich bei Ihrer Anwendung authentifiziert haben.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(user_AuthenticatedId)
| summarize dcount(user_AuthenticatedId) by bin(timestamp, 1h)
| render barchart
```
