---
title: Korrelation der Azure Application Insights-Telemetrie | Microsoft-Dokumentation
description: Korrelation der Application Insights-Telemetrie
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: 030259f7773435760c09afd25ca674b63bb1b3ca
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073236"
---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriekorrelation in Application Insights

In der Welt der Mikroservices müssen für jeden logischen Vorgang Aufgaben mit den verschiedenen Dienstkomponenten durchgeführt werden. Jede dieser Komponenten kann separat von [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) überwacht werden. Die Web-App-Komponente kommuniziert mit der Authentifizierungsanbieterkomponente, um die Anmeldeinformationen des Benutzers zu überprüfen, und mit der API-Komponente, um Daten zur Visualisierung abzurufen. Die API-Komponente kann Daten von anderen Diensten abfragen und mithilfe von Cacheanbieterkomponenten die Abrechnungskomponente über diesen Aufruf benachrichtigen. Application Insights unterstützt verteilte Telemetriekorrelation, mit der Sie erkennen können, welche Komponente für Fehler oder Leistungsbeeinträchtigungen verantwortlich ist.

In diesem Artikel wird das von Application Insights verwendete Datenmodell erläutert, mit dem die von mehreren Komponenten gesendeten Telemetriedaten korreliert werden. Es werden Techniken und Protokolle zur Kontextpropagierung erläutert. Darüber hinaus wird auch die Implementierung von Korrelationskonzepten in verschiedenen Sprachen und auf verschiedenen Plattformen behandelt.

## <a name="data-model-for-telemetry-correlation"></a>Datenmodell für Telemetriekorrelation

Application Insights definiert ein [Datenmodell](../../azure-monitor/app/data-model.md) für die verteilte Telemetriekorrelation. Um die Telemetrie mit dem logischen Vorgang zu verknüpfen, ist jedes Telemetrieelement mit einem Kontextfeld namens `operation_Id` versehen. Dieser Bezeichner wird von jedem Telemetrieelement in der verteilten Ablaufverfolgung gemeinsam genutzt. Selbst bei einem Verlust von Telemetriedaten von einer einzelnen Ebene können Sie weiterhin Telemetriedaten zuordnen, die von anderen Komponenten gemeldet wurden.

Der verteilte logische Vorgang besteht in der Regel aus einem Satz von kleineren Vorgängen, nämlich den Anforderungen, die von einer der Komponenten verarbeitet werden. Diese Vorgänge werden von einer [Anforderungstelemetrie](../../azure-monitor/app/data-model-request-telemetry.md) definiert. Jede Anforderungstelemetrie verfügt über eine eigene `id`, die diese eindeutig und global identifiziert. Für sämtliche Telemetrieelemente (z.B. Ablaufverfolgungen und Ausnahmen), die dieser Anforderung zugeordnet sind, sollte die `operation_parentId` auf den Wert der Anforderungs-`id` festgelegt werden.

Alle ausgehenden Vorgänge wie HTTP-Aufrufe an andere Komponenten werden durch eine [Abhängigkeitstelemetrie](../../azure-monitor/app/data-model-dependency-telemetry.md) dargestellt. Abhängigkeitstelemetrien definieren zudem ihre eigene `id`, die global eindeutig ist. Anforderungsabhängigkeiten, die durch diese Anforderungstelemetrie initiiert werden, verwenden diese `id` als `operation_parentId`.

Sie können eine Ansicht des verteilten logischen Vorgangs mit `operation_Id`, `operation_parentId` und `request.id` mit `dependency.id` erstellen. Diese Felder definieren auch die Kausalitätsreihenfolge der Telemetrieaufrufe.

In Microserviceumgebungen können Ablaufverfolgungen von Komponenten an unterschiedliche Speicherelemente weitergeleitet werden. Jede Komponente kann einen eigenen Instrumentierungsschlüssel in Application Insights aufweisen. Um Telemetriedaten für den logischen Vorgang abzurufen, ruft die Benutzeroberfläche von Application Insights Daten von jedem Speicherelement ab. Wenn die Anzahl der Speicherelemente sehr groß ist, benötigen Sie einen Hinweis, wo Sie als Nächstes suchen sollen. Zur Behebung dieses Problems definiert das Application Insights-Datenmodell zwei Felder: `request.source` und `dependency.target`. Das erste Feld identifiziert die Komponente, die die Abhängigkeitsanforderung initiiert hat, und das zweite Feld identifiziert die Komponente, die die Antwort des Abhängigkeitsaufrufs zurückgegeben hat.

## <a name="example"></a>Beispiel

Sehen Sie sich ein Beispiel für eine Anwendung namens Stock Prices an, die den aktuellen Marktpreis eines Aktienkurses mithilfe der externen API `Stock` angibt. Die Anwendung Stock Prices enthält die Seite `Stock page`, die vom Clientwebbrowser mit `GET /Home/Stock` geöffnet wird. Die Anwendung fragt die API `Stock` mit dem HTTP-Aufruf `GET /api/stock/value` ab.

Sie können die daraus resultierenden Telemetriedaten durch Ausführen einer Abfrage analysieren:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Beachten Sie, dass in den Ergebnissen alle Telemetrieelemente die Stamm-`operation_Id` nutzen. Wenn ein Ajax-Aufruf über die Seite erfolgt, wird der Abhängigkeitstelemetrie eine neue eindeutige ID (`qJSXU`) zugewiesen, und die ID der pageView dient als `operation_ParentId`. Die Serveranforderung verwendet dann die Ajax-ID als `operation_ParentId`.

| itemType   | name                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Wenn der Aufruf `GET /api/stock/value` an einen externen Dienst erfolgt, sollten Sie die Identität dieses Servers herausfinden, damit Sie das Feld `dependency.target` entsprechend festlegen können. Wenn der externe Dienst keine Überwachung unterstützt, wird `target` auf den Hostnamen des Diensts festgelegt (z.B. `stock-prices-api.com`). Wenn sich dieser Dienst jedoch durch Zurückgeben eines vordefinierten HTTP-Headers identifiziert, enthält `target` die Dienstidentität, mit der Application Insights verteilte Ablaufverfolgungen durch Abfragen von Telemetriedaten von diesem Dienst erstellen kann.

## <a name="correlation-headers"></a>Korrelations-Header

Wir arbeiten an einem RFC-Vorschlag für das [Korrelations-HTTP-Protokoll](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Diese Vorschläge definieren zwei Header:

- `Request-Id`: Enthält die global eindeutige ID des Aufrufs.
- `Correlation-Context`: Enthält die Sammlung von Name/Wert-Paaren der Eigenschaften von verteilten Ablaufverfolgungen.

Der Standard definiert außerdem zwei Schemas für `Request-Id`-Generierungen: flache und hierarchische Schemas. Beim flachen Schema ist ein bekannter `Id`-Schlüssel für die Sammlung `Correlation-Context` definiert.

Application Insights definiert die [Erweiterung](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) für das Korrelations-HTTP-Protokoll. Er verwendet Name/Wert-Paare für `Request-Context`, die die vom unmittelbaren Aufrufer oder Aufgerufenen verwendete Sammlung von Eigenschaften propagieren. Das Application Insights SDK legt mithilfe dieses Header die Felder `dependency.target` und `request.source` fest.

### <a name="w3c-distributed-tracing"></a>W3C – verteilte Ablaufverfolgung

Wir gehen über zum [W3C-Format für verteilte Ablaufverfolgung](https://w3c.github.io/trace-context/). Dieses definiert:

- `traceparent`: Trägt die global eindeutige Vorgangs-ID und den eindeutigen Bezeichner des Aufrufs.
- `tracestate`: Trägt einen für das Ablaufverfolgungssystem spezifischen Kontext.

#### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für klassische ASP.NET-Apps

Dieses Feature steht ab Version 2.8.0-beta1 in den Paketen `Microsoft.ApplicationInsights.Web` und `Microsoft.ApplicationInsights.DependencyCollector` zur Verfügung.
Standardmäßig ist es deaktiviert. Um es zu aktivieren, ändern Sie `ApplicationInsights.config`:

- Fügen Sie unter `RequestTrackingTelemetryModule` das `EnableW3CHeadersExtraction`-Element mit dem Wert `true` hinzu.
- Fügen Sie unter `DependencyTrackingTelemetryModule` das `EnableW3CHeadersInjection`-Element mit dem Wert `true` hinzu.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für ASP.NET Core-Apps

Dieses Feature befindet sich in `Microsoft.ApplicationInsights.AspNetCore` Version 2.5.0-beta1 und `Microsoft.ApplicationInsights.DependencyCollector` Version 2.8.0-beta1.
Standardmäßig ist es deaktiviert. Um es zu aktivieren, legen Sie `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` auf `true` fest:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für Java-Apps

- **Eingangskonfiguration**

  - Fügen Sie für Java EE-Apps dem Tag `<TelemetryModules>` in „ApplicationInsights.xml“ Folgendes hinzu:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
  - Für Spring Boot-Apps fügen Sie die folgenden Eigenschaften hinzu:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Ausgangskonfiguration**

  Fügen Sie der Datei „AI-Agent.xml“ Folgendes hinzu:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Der Abwärtskompatibilitätsmodus ist standardmäßig aktiviert, und der Parameter `enableW3CBackCompat` ist optional. Verwenden sie ihn nur, wenn Sie die Abwärtskompatibilität deaktivieren möchten.
  >
  > Idealerweise deaktivieren Sie ihn, wenn alle Dienste auf neuere Versionen von SDKs aktualisiert wurden, die das W3C-Protokoll unterstützen. Es wird dringend empfohlen, so bald wie möglich zu diesen neueren SDKs zu wechseln.

> [!IMPORTANT]
> Stellen Sie sicher, dass die Eingangs- und Ausgangskonfiguration genau gleich sind.

## <a name="opentracing-and-application-insights"></a>OpenTracing und Application Insights

Die [OpenTracing-Datenmodellspezifikation](https://opentracing.io/) und Application Insights-Datenmodelle sind in folgender Weise zugeordnet:

| Application Insights                  | OpenTracing                                       |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` mit `span.kind = server`                  |
| `Dependency`                          | `Span` mit `span.kind = client`                  |
| `Id` der `Request` und `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` vom Typ `ChildOf` (die übergeordnete Spanne)   |

Weitere Informationen finden Sie unter [Application Insights-Telemetriedatenmodell](../../azure-monitor/app/data-model.md). 

Definitionen von OpenTracing-Konzepten finden Sie in den [Spezifikationen](https://github.com/opentracing/specification/blob/master/specification.md) und [semantischen Konventionen](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) für OpenTracing.

## <a name="telemetry-correlation-in-net"></a>Telemetriekorrelation in .NET

Im Lauf der Zeit wurden in .NET verschiedene Möglichkeiten zur Korrelation von Telemetrie- und Diagnoseprotokollen definiert:

- `System.Diagnostics.CorrelationManager` ermöglicht die Nachverfolgung von [LogicalOperationStack und ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). 
- `System.Diagnostics.Tracing.EventSource` und die Ereignisablaufverfolgung für Windows (ETW) definieren die [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx)-Methode.
- `ILogger` verwendet [Protokollbereiche](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). 
- Windows Communication Foundation (WCF) und HTTP bieten eine Propagierung des Kontexts „aktuell“.

Allerdings wurde durch diese Methoden nicht die Unterstützung für automatische verteilte Ablaufverfolgung aktiviert. `DiagnosticSource` ist eine Möglichkeit zur Unterstützung von automatischen computerübergreifenden Korrelationen. .NET-Bibliotheken unterstützen Diagnosequellen und ermöglichen die automatische computerübergreifende Propagierung des Korrelationskontexts durch die Übertragungsmethode, z.B. HTTP.

Im [Leitfaden zu Aktivitäten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in `DiagnosticSource` werden die Grundlagen von Nachverfolgungsaktivitäten erläutert.

ASP.NET Core 2.0 unterstützt die Extraktion von HTTP-Headern und das Starten einer neuen Aktivität.

`System.Net.HttpClient` ab Version 4.1.0 unterstützt die automatische Injektion der Korrelations-HTTP-Header und die Nachverfolgung des HTTP-Aufrufs als Aktivität.

Für das klassische ASP.NET ist das neue HTTP-Modul [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) verfügbar. Dieses Modul implementiert Telemetriekorrelationen mithilfe von `DiagnosticSource`. Es startet Aktivitäten basierend auf den eingehenden Anforderungsheadern. Außerdem korreliert es Telemetriedaten aus den verschiedenen Phasen der Anforderungsverarbeitung, selbst in Fällen, in denen jede Phase der Verarbeitung von Internetinformationsdienste (Internet Information Services, IIS) in einem anderen verwalteten Thread ausgeführt wird.

Das Application Insights SDK ab Version 2.4.0-beta1 verwendet `DiagnosticSource` und `Activity`, um Telemetriedaten zu sammeln und mit der aktuellen Aktivität zu verknüpfen.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Telemetriekorrelation im Java SDK

Das [Application Insights SDK für Java](../../azure-monitor/app/java-get-started.md) unterstützt ab Version 2.0.0 die automatische Korrelation von Telemetriedaten. Das SDK füllt `operation_id` automatisch für alle Telemetriedaten (z.B. Ablaufverfolgungen, Ausnahmen und benutzerdefinierte Ereignisse) auf, die im Zusammenhang mit einer Anforderung ausgegeben werden. Es sorgt auch dafür, dass die Korrelationsheader (weiter oben beschrieben) für Dienst-zu-Dienst-Aufrufe über HTTP weitergeben werden, wenn der [Java SDK-Agent](../../azure-monitor/app/java-agent.md) konfiguriert ist.

> [!NOTE]
> Für die Korrelationsfunktion werden nur Aufrufe unterstützt, die über Apache HttpClient erfolgen. Wenn Sie mit Spring Rest Template oder Feign arbeiten, können beide aufgesetzt auf Apache HttpClient verwendet werden.

Derzeit wird die automatische Kontextweitergabe über verschiedene Messagingtechnologien (z.B. Kafka, RabbitMQ oder Azure Service Bus) nicht unterstützt. Es ist jedoch möglich, solche Szenarien manuell mithilfe der APIs `trackDependency` und `trackRequest` zu schreiben. In diesen APIs entsprechen Telemetriedaten den Abhängigkeiten einer Nachricht, die von einem Producer in die Warteschlange eingereiht wird, und die Anforderung entspricht einer Nachricht, die von einem Consumer verarbeitet wird. In diesem Fall müssen sowohl `operation_id` als auch `operation_parentId` in den Eigenschaften der Nachricht weitergegeben werden.

### <a name="telemetry-correlation-in-asynchronous-java-application"></a>Telemetriekorrelation in asynchroner Java-Anwendung

Zum Korrelieren von Telemetriedaten in einer asynchronen Spring Boot-Anwendung folgen Sie [diesem](https://github.com/Microsoft/ApplicationInsights-Java/wiki/Distributed-Tracing-in-Asynchronous-Java-Applications) ausführlichen Artikel. Er enthält Anweisungen für das Instrumentieren von [ThreadPoolTaskExecutor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html) und [ThreadPoolTaskScheduler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskScheduler.html) für Spring. 


<a name="java-role-name"></a>
## <a name="role-name"></a>Rollenname

Gelegentlich möchten Sie möglicherweise die Art und Weise anpassen, wie Komponentennamen in der [Anwendungsübersicht](../../azure-monitor/app/app-map.md) angezeigt werden. Dazu können Sie `cloud_RoleName` manuell festlegen, indem Sie eine der folgenden Aktionen ausführen:

- Bei Verwendung von Spring Boot mit dem Application Insights Spring Boot-Startprogramm besteht die einzige erforderliche Änderung darin, Ihren benutzerdefinierten Namen für die Anwendung in der Datei „application.properties“ festzulegen.

  `spring.application.name=<name-of-app>`

  Der Spring Boot-Starter weist `cloudRoleName` automatisch den Wert zu, den Sie für die `spring.application.name`-Eigenschaft eingeben.

- Wenn Sie `WebRequestTrackingFilter` verwenden, legt der `WebAppNameContextInitializer` automatisch den Anwendungsnamen fest. Fügen Sie Folgendes zu Ihrer Konfigurationsdatei (ApplicationInsights.xml) hinzu:

  ```XML
  <ContextInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
  </ContextInitializers>
  ```

- Wenn Sie die Cloudkontextklasse verwenden:

  ```Java
  telemetryClient.getContext().getCloud().setRole("My Component Name");
  ```

## <a name="next-steps"></a>Nächste Schritte

- Schreiben Sie [benutzerdefinierte Telemetriedaten](../../azure-monitor/app/api-custom-events-metrics.md).
- Erfahren Sie mehr über das [Festlegen von cloud_RoleName](../../azure-monitor/app/app-map.md#set-cloud-role-name) für andere SDKs.
- Integrieren Sie alle Komponenten Ihres Microservices in Application Insights. Sehen Sie sich die [unterstützten Plattformen](../../azure-monitor/app/platforms.md) an.
- Lesen Sie die Informationen zu den Application Insights-Typen im [Datenmodell](../../azure-monitor/app/data-model.md).
- Informationen zum [Erweitern und Filtern von Telemetriedaten](../../azure-monitor/app/api-filtering-sampling.md).
- Lesen Sie die [Konfigurationsreferenz für Application Insights](configuration-with-applicationinsights-config.md).
