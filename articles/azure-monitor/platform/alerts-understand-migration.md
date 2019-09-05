---
title: Funktionsweise des Tools für die freiwillige Migration von Azure Monitor-Warnungen
description: Enthält eine Beschreibung der Funktionsweise des Migrationstools für Warnungen sowie Informationen zur Problembehandlung.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: c3d5bb58989fe87ddf9a185dbae926a71edf1590
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70061560"
---
# <a name="understand-how-the-migration-tool-works"></a>Funktionsweise des Migrationstools

Wie [bereits angekündigt](monitoring-classic-retirement.md) werden klassische Warnungen in Azure Monitor ab 31. August 2019 (ursprünglich 30. Juni 2019) eingestellt. Im Azure-Portal steht ein Migrationstool für Kunden bereit, die klassische Warnungsregeln verwenden und die Migration selbst auslösen möchten.

In diesem Artikel wird erläutert, wie das Tool für die freiwillige Migration funktioniert. Außerdem werden Abhilfemaßnahmen für einige häufige Probleme beschrieben.

> [!NOTE]
> Aufgrund einer Verzögerung beim Rollout des Migrationstools wurde der Deaktivierungstermin vom ursprünglich angekündigten Datum, dem 30. Juni 2019, auf den [31. August 2019 verschoben](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/).

## <a name="classic-alert-rules-that-will-not-be-migrated"></a>Klassische Warnungsregeln, die nicht migriert werden

> [!IMPORTANT]
> Aktivitätsprotokollwarnungen (einschließlich Service Health-Warnungen) und Protokollwarnungen sind von der Migration nicht betroffen. Die Migration gilt nur für klassische Warnungsregeln, die [hier](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform) beschrieben werden.

Zwar können mit dem Tool fast alle [klassischen Warnungsregeln](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform) migriert werden, doch gibt es einige Ausnahmen. Die folgenden Warnungsregeln werden mit dem Tool nicht migriert (bzw. werden auch während der automatischen Migration ab September 2019 nicht migriert):

- Klassische Warnungsregeln für VM-Gastmetriken (sowohl Windows als auch Linux). Informationen finden Sie in der [Anleitung zum erneuten Erstellen solcher Warnungsregeln in neuen Metrikwarnungen](#guest-metrics-on-virtual-machines) weiter unten in diesem Artikel.
- Klassische Warnungsregeln für Metriken von klassischem Speicher. Informationen finden Sie in der [Anleitung zum Überwachen Ihrer klassischen Speicherkonten](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Klassische Warnungsregeln in einigen Speicherkontometriken. Informationen finden Sie unter den [Details](#storage-account-metrics) weiter unten in diesem Artikel.
- Klassische Warnungsregeln für einige Cosmos DB-Metriken. Informationen finden Sie unter den [Details](#cosmos-db-metrics) weiter unten in diesem Artikel.
- Klassische Warnungsregeln für alle Metriken klassischer virtueller Computer und Clouddienste (Microsoft.ClassicCompute/virtualMachines and Microsoft.ClassicCompute/domainNames/slots/roles). Informationen finden Sie unter den [Details](#classic-compute-metrics) weiter unten in diesem Artikel.

Wenn Ihr Abonnement über klassische Regeln dieser Art verfügt, müssen Sie sie manuell migrieren. Da wir keine automatische Migration bereitstellen können, funktionieren alle vorhandenen klassischen Metrikwarnungen noch bis Juni 2020. Diese Verlängerung gibt Ihnen Zeit für die Umstellung auf neue Warnungen. Sie können auch bis Juni 2020 weiterhin neue klassische Warnungen zu den oben aufgeführten Ausnahmen erstellen. Für alles andere können aber nach August 2019 keine neuen klassischen Warnungen mehr erstellt werden.

> [!NOTE]
> Wenn Ihre klassischen Warnungsregeln, neben den oben aufgeführten Ausnahmen, ungültig sind (d.h. sie gelten für [veraltete Metriken](#classic-alert-rules-on-deprecated-metrics) oder Ressourcen, die gelöscht wurden) werden sie während der freiwilligen Migration nicht migriert. Derartige ungültige klassische Warnungsregeln werden bei der automatischen Migration gelöscht.

### <a name="guest-metrics-on-virtual-machines"></a>Gastmetriken auf virtuellen Computern

Bevor Sie neue Metrikwarnungen für Gastmetriken erstellen können, müssen die Gastmetriken an den benutzerdefinierten Metrikspeicher von Azure Monitor gesendet werden. Befolgen Sie die nachstehenden Anweisungen, um die Azure Monitor-Senke in den Diagnoseeinstellungen zu aktivieren:

- [Aktivieren von Gastmetriken für Windows-VMs](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Aktivieren von Gastmetriken für Linux-VMs](collect-custom-metrics-linux-telegraf.md)

Nachdem die oben genannten Schritte ausgeführt wurden, können Sie neue Metrikwarnungen für Gastmetriken erstellen. Und nachdem Sie neue Metrikwarnungen erstellt haben, können Sie klassische Warnungen löschen.

### <a name="storage-account-metrics"></a>Speicherkontometriken

Alle klassischen Warnungen in Speicherkonten können migriert werden, mit Ausnahme von Warnungen in den folgenden Metriken:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError
- ThrottlingError

Klassische Warnungsregeln für Metriken vom Typ „Percent“ müssen basierend auf der [Zuordnung zwischen alten und neuen Speichermetriken](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics) migriert werden. Schwellenwerte müssen entsprechend geändert werden, da die neue verfügbare Metrik eine absolute Metrik ist.

Die klassischen Warnungsregeln für AnonymousThrottlingError, SASThrottlingError und ThrottlingError müssen in zwei neue Warnungen aufgeteilt werden, da es keine kombinierte Metrik gibt, die über die gleiche Funktionalität verfügt. Schwellenwerte müssen entsprechend angepasst werden.

### <a name="cosmos-db-metrics"></a>Cosmos DB-Metriken

Alle klassischen Warnungen für Cosmos DB-Metriken können migriert werden, mit Ausnahme von Warnungen für die folgenden Metriken:

- Mittelwert der Anforderungen pro Sekunde
- Konsistenzebene
- HTTP 2xx
- HTTP 3xx
- HTTP 400
- HTTP 401
- Interner Serverfehler
- Max RUPM Consumed Per Minute (Maximal verwendete U/min)
- Max RUs Per Second (Maximale RUs pro Sekunde)
- Mongo Count Failed Requests (Fehlgeschlagene Mongo-Zählungsanforderungen)
- Mongo Delete Failed Requests (Fehlgeschlagene Mongo-Löschanforderungen)
- Mongo Insert Failed Requests (Fehlgeschlagene Mongo-Einfügeanforderungen)
- Mongo Other Failed Requests (Sonstige fehlgeschlagene Mongo-Anforderungen)
- Mongo Other Request Charge (Kosten sonstiger Mongo-Anforderungen)
- Mongo Other Request Rate (Rate sonstiger Mongo-Anforderungen)
- Mongo Query Failed Requests (Fehlgeschlagene Mongo-Abfrageanforderungen)
- Mongo Query Failed Requests (Fehlgeschlagene Mongo-Aktualisierungsanforderungen)
- Beobachtete Leselatenz
- Beobachtete Schreiblatenz
- Dienstverfügbarkeit
- Speicherkapazität
- Gedrosselte Anforderungen
- Anzahl von Anforderungen

„Mittelwert der Anforderungen pro Sekunde“, „Konsistenzebene“, „Max RUPM Consumed Per Minute“ (Maximal verwendete U/min), „Max RUs Per Second“ (Maximale RUs pro Sekunde), „Beobachtete Leselatenz“, „Beobachtete Schreiblatenz“ und „Speicherkapazität" sind im [neuen System](metrics-supported.md#microsoftdocumentdbdatabaseaccounts) derzeit nicht verfügbar.

Warnungen zu Anforderungsmetriken wie „HTTP 2xx“, „HTTP 3xx“, „HTTP 400“, „HTTP 401“, „Interner Serverfehler“, „Dienstverfügbarkeit“, „Gedrosselte Anforderungen“ und „Anforderungen insgesamt“ werden nicht migriert, da sich die Zählung der Anforderungen zwischen klassischen Metriken und neuen Metriken unterscheidet. Warnungen für diese Metriken müssen manuell mit angepassten Schwellenwerten neu erstellt werden.

Warnungen zu Metriken für fehlgeschlagene Mongo-Anforderungen müssen in mehrere Warnungen unterteilt werden, da keine gemeinsame Metrik vorhanden ist, die die gleiche Funktionalität bietet. Schwellenwerte müssen entsprechend angepasst werden.

### <a name="classic-compute-metrics"></a>Klassische Computemetriken

Die Warnungen zu klassischen Computemetriken werden nicht mithilfe des Migrationstools migriert, da klassische Computeressourcen für neue Warnungen noch nicht unterstützt werden. Die Unterstützung für neue Warnungen zu diesen Ressourcentypen wird in Zukunft hinzugefügt. Sobald sie verfügbar ist, müssen die Kunden vor Juni 2020 neue entsprechende Warnungsregeln basierend auf ihren klassischen Warnungsregeln neu erstellen.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Klassische Warnungsregeln für veraltete Metriken

Hierbei handelt es sich um klassische Warnungsregeln für Metriken, die früher unterstützt wurden, irgendwann aber veraltet sind. Ein kleiner Prozentsatz der Kunden verfügt möglicherweise über ungültige klassische Warnungsregeln für diese Metriken. Da diese Warnungsregeln ungültig sind, werden sie nicht migriert.

| Ressourcentyp| Veraltete Metrik(en) |
|-------------|----------------- |
| Microsoft.DBforMySQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAddresses | defaultddostriggerrate |
| Microsoft.SQL/servers/databases | service_level_objective, storage_limit, storage_used, throttling, dtu_consumption_percent, storage_used |
| Microsoft.Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft.Web/hostingEnvironments/workerpools | bytesreceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Erstellen entsprechender neuer Warnungsregeln und Aktionsgruppen

Das Migrationstool konvertiert klassische Warnungsregeln in entsprechende neue Warnungsregeln und Aktionsgruppen. Bei den meisten klassischen Warnungsregeln gelten die entsprechenden neuen Warnungsregeln für dieselben Metriken mit denselben Eigenschaften (z. B. `windowSize` und `aggregationType`). Es gibt jedoch einige klassische Warnungsregeln, die für Metriken gelten, die im neuen System eine andere Metrikentsprechung haben. Die folgenden Prinzipien gelten für die Migration von klassischen Warnungen, sofern im folgenden Abschnitt nichts anderes angegeben ist:

- **Häufigkeit**: Definiert, wie oft eine klassische oder neue Warnungsregel die Bedingung überprüft. Die Häufigkeit (`frequency`) bei klassischen Warnungsregeln konnte vom Benutzer nicht konfiguriert werden. Sie war für alle Ressourcentypen auf 5 Minuten eingestellt (mit Ausnahme von Application Insights-Komponenten, für die 1 Minute festgelegt war). Daher wird die Häufigkeit bei den entsprechenden Regeln ebenfalls auf 5 Minuten bzw. 1 Minute festgelegt.
- **Aggregationstyp**: Definiert, wie die Metrik über das interessierende Zeitfenster aggregiert wird. Auch der Aggregationstyp (`aggregationType`) bleibt für klassische und neue Warnungen und die meisten Metriken unverändert. Da bei klassischen und neuen Warnungen manchmal die Metrik unterschiedlich ist, wird das entsprechende, für die Metrik definierte Attribut `aggregationType` oder `primary Aggregation Type` verwendet.
- **Einheiten**: Eigenschaft der Metrik, für die die Warnung erstellt wird. Einige Metrikentsprechungen weisen unterschiedliche Einheiten auf. Der Schwellenwert wird nach Bedarf entsprechend angepasst. Beispiel: Wenn die ursprüngliche Metrik Sekunden, die neue entsprechende Metrik jedoch Millisekunden als Einheit hat, wird der ursprüngliche Schwellenwert mit 1000 multipliziert, um das gleiche Verhalten sicherzustellen.
- **Fenstergröße**: Definiert das Zeitfenster, über das die Metrikdaten aggregiert und mit dem Schwellenwert verglichen werden. An den Standardwerten für die Fenstergröße (`windowSize`) (z. B. 5 Min., 15 Min., 30 Min., 1 Std., 3 Std., 6 Std., 12 Std., 1 Tag) wurden bei der entsprechenden neuen Warnungsregel keine Änderung vorgenommen. Bei anderen Werten wird die nächstliegende Fenstergröße (`windowSize`) ausgewählt und verwendet. Bei den meisten Kunden hat diese Änderung keine Auswirkungen. Ein kleiner Prozentsatz der Kunden muss den Schwellenwert möglicherweise optimieren, um genau dasselbe Verhalten zu erzielen.

In den folgenden Abschnitten werden die Metriken im Detail beschrieben, die im neuen System eine andere Metrikentsprechung haben. Alle Metriken, die bei klassischen und neuen Warnungsregeln unverändert bleiben, sind nicht aufgeführt. Eine Liste der im neuen System unterstützten Metriken finden Sie [hier](metrics-supported.md).

### <a name="microsoftstorageaccountsservices"></a>Microsoft.StorageAccounts/services

Bei Speicherkontodiensten wie Blob Storage, Table Storage, Azure Files und Queue Storage werden die folgenden Metriken wie nachstehend gezeigt den entsprechenden Metriken zugeordnet:

| Metrik in klassischen Warnungen | Entsprechende Metrik in neuen Warnungen | Kommentare|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| Transaktionsmetrik mit den Dimensionen „ResponseType“=„AuthorizationError“ und „Authentication“ = „Anonymous“| |
| AnonymousClientOtherError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ClientOtherError“ und „Authentication“ = „Anonymous“ | |
| AnonymousClientTimeOutError| Transaktionsmetrik mit den Dimensionen „ResponseType“=„ClientTimeOutError“ und „Authentication“ = „Anonymous“ | |
| AnonymousNetworkError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„NetworkError“ und „Authentication“ = „Anonymous“ | |
| AnonymousServerOtherError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ServerOtherError“ und „Authentication“ = „Anonymous“ | |
| AnonymousServerTimeOutError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ServerTimeOutError“ und „Authentication“ = „Anonymous“ | |
| AnonymousSuccess | Transaktionsmetrik mit den Dimensionen „ResponseType“=„Success“ und „Authentication“ = „Anonymous“ | |
| AuthorizationError | Transaktionsmetrik mit der Dimension „ResponseType“=„AuthorizationError“ | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | SuccessServerLatency | |
| Capacity | BlobCapacity | Verwenden Sie als `aggregationType` „average“ anstelle von „last“. Die Metrik gilt nur für Blob-Dienste. |
| ClientOtherError | Transaktionsmetrik mit der Dimension „ResponseType“=„ClientOtherError“  | |
| ClientTimeoutError | Transaktionsmetrik mit der Dimension „ResponseType“=„ClientTimeOutError“ | |
| ContainerCount | ContainerCount | Verwenden Sie als `aggregationType` „average“ anstelle von „last“. Die Metrik gilt nur für Blob-Dienste. |
| NetworkError | Transaktionsmetrik mit der Dimension „ResponseType“=„NetworkError“ | |
| ObjectCount | BlobCount| Verwenden Sie als `aggregationType` „average“ anstelle von „last“. Die Metrik gilt nur für Blob-Dienste. |
| SASAuthorizationError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„AuthorizationError“ und „Authentication“ = „SAS“ | |
| SASClientOtherError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ClientOtherError“ and „Authentication“ = „SAS“ | |
| SASClientTimeOutError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ClientTimeOutError“ und „Authentication“ = „SAS“ | |
| SASNetworkError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„NetworkError“ und „Authentication“ = „SAS“ | |
| SASServerOtherError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ServerOtherError“ und „Authentication“ = „SAS“ | |
| SASServerTimeOutError | Transaktionsmetrik mit den Dimensionen „ResponseType“=„ServerTimeOutError“ und „Authentication“ = „SAS“ | |
| SASSuccess | Transaktionsmetrik mit den Dimensionen „ResponseType“=„Success“ und „Authentication“ = „SAS“ | |
| ServerOtherError | Transaktionsmetrik mit der Dimension „ResponseType“=„ServerOtherError“ | |
| ServerTimeOutError | Transaktionsmetrik mit der Dimension „ResponseType“=„ServerTimeOutError“  | |
| Erfolgreich | Transaktionsmetrik mit der Dimension „ResponseType“=„Success“ | |
| TotalBillableRequests| Transaktionen | |
| TotalEgress | Ausgehende Daten | |
| TotalIngress | Eingehende Daten | |
| TotalRequests | Transaktionen | |

### <a name="microsoftinsightscomponents"></a>Microsoft.insights/components

Für Application Insights gibt es die folgenden entsprechenden Metriken:

| Metrik in klassischen Warnungen | Entsprechende Metrik in neuen Warnungen | Kommentare|
|--------------------------|---------------------------------|---------|
| availability.availabilityMetric.value | availabilityResults/availabilityPercentage|   |
| availability.durationMetric.value | availabilityResults/duration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| basicExceptionBrowser.count | exceptions/browser|  Verwenden Sie für `aggregationType` „count“ anstelle von „sum“. |
| basicExceptionServer.count | exceptions/server| Verwenden Sie für `aggregationType` „count“ anstelle von „sum“.  |
| clientPerformance.clientProcess.value | browserTimings/processingDuration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| clientPerformance.networkConnection.value | browserTimings/networkDuration|  Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist. |
| clientPerformance.receiveRequest.value | browserTimings/receiveDuration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| clientPerformance.sendRequest.value | browserTimings/sendDuration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| clientPerformance.total.value | browserTimings/totalDuration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| performanceCounter.available_bytes.value | performanceCounters/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec.value | performanceCounters/processIOBytesPerSecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec.value | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized.value | performanceCounters/processCpuPercentage|   |
| performanceCounter.percentage_processor_time.value | performanceCounters/processCpuPercentage| Der Schwellenwert muss entsprechend geändert werden, da die ursprüngliche Metrik alle Kerne einbezog und die neue Metrik auf einen Kern normalisiert wird. Das Migrationstool ändert die Schwellenwerte nicht.  |
| performanceCounter.percentage_processor_total.value | performanceCounters/processorCpuPercentage|   |
| performanceCounter.process_private_bytes.value | performanceCounters/processPrivateBytes|   |
| performanceCounter.request_execution_time.value | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue.value | performanceCounters/requestsInQueue|   |
| performanceCounter.requests_per_sec.value | performanceCounters/requestsPerSecond|   |
| request.duration | requests/duration| Multiplizieren Sie den ursprüngliche Schwellenwert mit 1000, da die Einheiten für die klassische Metrik in Sekunden und für die neue in Millisekunden angegeben ist.  |
| request.rate | requests/rate|   |
| requestFailed.count | requests/failed| Verwenden Sie für `aggregationType` „count“ anstelle von „sum“.   |
| view.count | pageViews/count| Verwenden Sie für `aggregationType` „count“ anstelle von „sum“.   |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

Für Cosmos DB gibt es die folgenden entsprechenden Metriken:

| Metrik in klassischen Warnungen | Entsprechende Metrik in neuen Warnungen | Kommentare|
|--------------------------|---------------------------------|---------|
| AvailableStorage     |AvailableStorage|   |
| Datengröße | DataUsage| |
| Dokumentanzahl | DocumentCount||
| Indexgröße | IndexUsage||
| Mongo Count Request Charge (Kosten von Mongo-Zählungsanforderung)| MongoRequestCharge mit Dimension „CommandName“ = „count“||
| Mongo Count Request Rate (Rate von Mongo-Zählungsanforderung) | MongoRequestsCount mit Dimension „CommandName“ = „count“||
| Mongo Delete Request Charge (Kosten von Mongo-Löschanforderung) | MongoRequestCharge mit Dimension „CommandName“ = „delete“||
| Mongo Delete Request Rate (Rate von Mongo-Löschanforderung) | MongoRequestsCount mit Dimension „CommandName“ = „delete“||
| Mongo Insert Request Charge (Kosten von Mongo-Einfügeanforderung) | MongoRequestCharge mit Dimension „CommandName“ = „insert“||
| Mongo Insert Request Rate (Rate von Mongo-Einfügeanforderung) | MongoRequestsCount mit Dimension „CommandName“ = „insert“||
| Mongo Query Request Charge (Kosten von Mongo-Abfrageanforderung) | MongoRequestCharge mit Dimension „CommandName“ = „find“||
| Mongo Query Request Rate (Rate von Mongo-Abfrageanforderung) | MongoRequestsCount mit Dimension „CommandName“ = „find“||
| Mongo Update Request Charge (Kosten von Mongo-Aktualisierungsanforderung) | MongoRequestCharge mit Dimension „CommandName“ = „update“||
| Dienst nicht verfügbar| ServiceAvailability||
| TotalRequestUnits | TotalRequestUnits||

### <a name="how-equivalent-action-groups-are-created"></a>Erstellen entsprechender Aktionsgruppen

Bei klassischen Warnungsregeln waren E-Mail-, Webhook-, Logik-App-und Runbook-Aktionen direkt mit der eigentlichen Warnungsregel verbunden. Die neuen Warnungsregeln verwenden Aktionsgruppen, die übergreifend über mehrere Warnungsregeln wiederverwendet werden können. Das Migrationstool erstellt eine einzelne Aktionsgruppe für gleiche Aktionen, und zwar unabhängig davon, wie viele Warnungsregeln die Aktion verwenden. Vom Migrationstool erstellte Aktionsgruppen erhalten das Namensformat „Migrated_AG*“.

> [!NOTE]
> Für klassische Warnungen wurden lokalisierte E-Mails gesendet, die auf dem Gebietsschema des klassischen Administrators basierten, wenn sie zum Benachrichtigen klassischer Administratorrollen verwendet wurden. Neue Warnungs-E-Mails werden über Aktionsgruppen gesendet und sind nur in englischer Sprache verfügbar.

## <a name="rollout-phases"></a>Rolloutphasen

Der Rollout des Migrationstools erfolgt für Kunden, die klassische Warnungsregeln verwenden, in mehreren Phasen. Abonnementbesitzer erhalten eine E-Mail, wenn das Abonnement für die Migration mit dem Tool bereit ist.

> [!NOTE]
> Da der Rollout des Tools in Phasen erfolgt, wird Ihnen in den frühen Phasen möglicherweise angezeigt, dass einige Ihrer Abonnements noch nicht für die Migration bereit sind.

Derzeit sind die meisten Abonnements als migrationsbereit gekennzeichnet. Nur Abonnements mit klassischen Warnungsregeln für die folgenden Ressourcentypen sind noch nicht für die Migration bereit.

- Microsoft.classicCompute/domainNames/slots/roles
- Microsoft.insights/components

## <a name="who-can-trigger-the-migration"></a>Wer kann die Migration auslösen?

Alle Benutzer, die auf Abonnementebene über die integrierte Rolle „Mitwirkender an der Überwachung“ verfügen, können die Migration auslösen. Benutzer, die über eine benutzerdefinierte Rolle mit den folgenden Berechtigungen verfügen, können die Migration ebenfalls auslösen:

- */Lesen
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*
- Microsoft.AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> Ihr Abonnement sollte nicht nur über die obigen Berechtigungen verfügen, sondern außerdem beim Microsoft.AlertsManagement-Ressourcenanbieter registriert werden. Dies ist für eine erfolgreiche Migration von Fehleranomaliewarnungen für Application Insights erforderlich. 

## <a name="common-problems-and-remedies"></a>Häufige Probleme und Abhilfemaßnahmen

Nachdem Sie [die Migration ausgelöst haben](alerts-using-migration-tool.md), erhalten Sie E-Mails an die Adressen, die Sie für die Benachrichtigung über den Abschluss der Migration oder die erforderliche Ausführung einer Aktion angegeben haben. In diesem Abschnitt sind einige häufige Probleme und der Umgang damit beschrieben.

### <a name="validation-failed"></a>Fehler bei der Überprüfung

Aufgrund von einigen kürzlich durchgeführten Änderungen an den klassischen Warnungsregeln in Ihrem Abonnement kann das Abonnement nicht migriert werden. Dies ist ein vorübergehendes Problem. Sie können die Migration neu starten, nachdem der Migrationsstatus nach einigen Tagen wieder **Bereit für die Migration** lautet.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Bereichssperre verhindert die Migration Ihrer Regeln

Im Rahmen der Migration werden neue Metrikwarnungen und neue Aktionsgruppen erstellt und anschließend klassische Warnungsregeln gelöscht. Eine Bereichssperre kann jedoch das Erstellen oder Löschen von Ressourcen verhindern. Je nach Bereichssperre können einige oder alle Regeln nicht migriert werden. Sie können dieses Problem beheben, indem Sie die Bereichssperre für das Abonnement, die Ressourcengruppe oder die Ressource aufheben, die im [Migrationstool](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel) aufgeführt ist, und die Migration erneut auslösen. Die Bereichssperre kann nicht deaktiviert werden und muss für die Dauer des Migrationsprozesses entfernt werden. [Erfahren Sie mehr über das Verwalten von Bereichssperren](../../azure-resource-manager/resource-group-lock-resources.md#portal).

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>Richtlinie mit Auswirkung „deny“ verhindert die Migration Ihrer Regeln

Im Rahmen der Migration werden neue Metrikwarnungen und neue Aktionsgruppen erstellt und anschließend klassische Warnungsregeln gelöscht. Eine Richtlinie kann jedoch das Erstellen von Ressourcen verhindern. Je nach Richtlinie können einige oder alle Regeln nicht migriert werden. Die Richtlinien, die den Prozess blockieren, werden im [Migrationstool](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel) aufgeführt. Dieses Problem lässt sich auf eine der folgenden Arten beheben:

- Ausschließen der Abonnements oder Ressourcengruppen von der Richtlinienzuweisung für die Dauer des Migrationsprozesses. [Erfahren Sie mehr über das Verwalten eines Ausschlussbereichs für Richtlinien](../../governance/policy/tutorials/create-and-manage.md#exempt-a-non-compliant-or-denied-resource-using-exclusion).
- Entfernen oder Ändern des Effekts in „audit“ oder „append“ (wodurch beispielsweise Probleme im Zusammenhang mit fehlenden Tags gelöst werden können). [Erfahren Sie mehr über das Verwalten eines Effekts für Richtlinien](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="next-steps"></a>Nächste Schritte

- [How to use the migration tool](alerts-using-migration-tool.md) (Verwenden des Migrationstools)
- [Vorbereiten der Migration](alerts-prepare-migration.md)
