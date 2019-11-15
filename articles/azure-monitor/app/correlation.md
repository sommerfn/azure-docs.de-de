---
title: Korrelation der Azure Application Insights-Telemetrie | Microsoft-Dokumentation
description: Korrelation der Application Insights-Telemetrie
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/07/2019
ms.reviewer: sergkanz
ms.openlocfilehash: bcdc6633980ec3684217c8c19b4799befe2af3a3
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73576856"
---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriekorrelation in Application Insights

In der Welt der Mikroservices müssen für jeden logischen Vorgang Aufgaben mit den verschiedenen Dienstkomponenten durchgeführt werden. Jede dieser Komponenten kann separat von [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) überwacht werden. Application Insights unterstützt verteilte Telemetriekorrelation, mit der Sie erkennen können, welche Komponente für Fehler oder Leistungsbeeinträchtigungen verantwortlich ist.

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

| itemType   | name                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Wenn der Aufruf `GET /api/stock/value` an einen externen Dienst erfolgt, sollten Sie die Identität dieses Servers herausfinden, damit Sie das Feld `dependency.target` entsprechend festlegen können. Wenn der externe Dienst keine Überwachung unterstützt, wird `target` auf den Hostnamen des Diensts festgelegt (z.B. `stock-prices-api.com`). Wenn sich dieser Dienst jedoch durch Zurückgeben eines vordefinierten HTTP-Headers identifiziert, enthält `target` die Dienstidentität, mit der Application Insights verteilte Ablaufverfolgungen durch Abfragen von Telemetriedaten von diesem Dienst erstellen kann.

## <a name="correlation-headers"></a>Korrelations-Header

Wir gehen zum [W3C Trace-Context](https://w3c.github.io/trace-context/) über, der Folgendes definiert:

- `traceparent`: Trägt die global eindeutige Vorgangs-ID und den eindeutigen Bezeichner des Aufrufs.
- `tracestate`: Trägt einen für das Ablaufverfolgungssystem spezifischen Kontext.

Die neuesten Versionen von Application Insights SDKs unterstützen das „Trace-Context“-Protokoll, aber möglicherweise müssen Sie sich explizit dafür entscheiden (die Abwärtskompatibilität mit dem alten Korrelationsprotokoll, das von Application Insights SDKs unterstützt wird, bleibt erhalten).

Das [Korrelations-HTTP-Protokoll, auch als „Request-ID“ bezeichnet,](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) befindet sich im Pfad für die Einstellung. Dieses Protokoll definiert zwei Header:

- `Request-Id`: Enthält die global eindeutige ID des Aufrufs.
- `Correlation-Context`: Enthält die Sammlung von Name/Wert-Paaren der Eigenschaften von verteilten Ablaufverfolgungen.

Application Insights definiert ferner die [Erweiterung](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) für das Korrelations-HTTP-Protokoll. Er verwendet Name/Wert-Paare für `Request-Context`, die die vom unmittelbaren Aufrufer oder Aufgerufenen verwendete Sammlung von Eigenschaften propagieren. Das Application Insights SDK legt mithilfe dieses Header die Felder `dependency.target` und `request.source` fest.

### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für klassische ASP.NET-Apps
 
  > [!NOTE]
  > Ab `Microsoft.ApplicationInsights.Web` und `Microsoft.ApplicationInsights.DependencyCollector` ist keine Konfiguration mehr erforderlich. 

Die Unterstützung für den W3C Trace-Context erfolgt in abwärtskompatibler Weise, und es wird erwartet, dass die Korrelation mit Anwendungen funktioniert, die mit früheren Versionen des SDK instrumentiert sind (ohne W3C-Unterstützung). 

Wenn Sie aus irgendeinem Grund das weiterhin das `Request-Id`-Legacyprotokoll verwenden möchten, können Sie den Trace-Context mit folgender Konfiguration *deaktivieren*.

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

Wenn Sie eine ältere Version des SDK ausführen, empfiehlt es sich, dieses zu aktualisieren oder die folgende Konfiguration anzuwenden, um den Trace-Context zu ermöglichen.
Dieses Feature steht ab Version 2.8.0-beta1 in den Paketen `Microsoft.ApplicationInsights.Web` und `Microsoft.ApplicationInsights.DependencyCollector` zur Verfügung.
Standardmäßig ist es deaktiviert. Um es zu aktivieren, ändern Sie `ApplicationInsights.config`:

- Fügen Sie unter `RequestTrackingTelemetryModule` das `EnableW3CHeadersExtraction`-Element mit dem Wert `true` hinzu.
- Fügen Sie unter `DependencyTrackingTelemetryModule` das `EnableW3CHeadersInjection`-Element mit dem Wert `true` hinzu.
- Fügen Sie `W3COperationCorrelationTelemetryInitializer` unter `TelemetryInitializers` hinzu, ähnlich wie im Folgenden: 

```xml
<TelemetryInitializers>
  <Add Type="Microsoft.ApplicationInsights.Extensibility.W3C.W3COperationCorrelationTelemetryInitializer, Microsoft.ApplicationInsights"/>
   ...
</TelemetryInitializers> 
```

### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für ASP.NET Core-Apps

 > [!NOTE]
  > Ab `Microsoft.ApplicationInsights.AspNetCore`, Version 2.8.0, ist keine Konfiguration mehr erforderlich.
 
Die Unterstützung für den W3C Trace-Context erfolgt in abwärtskompatibler Weise, und es wird erwartet, dass die Korrelation mit Anwendungen funktioniert, die mit früheren Versionen des SDK instrumentiert sind (ohne W3C-Unterstützung). 

Wenn Sie aus irgendeinem Grund das weiterhin das `Request-Id`-Legacyprotokoll verwenden möchten, können Sie den Trace-Context mit folgender Konfiguration *deaktivieren*.

```csharp
  Activity.DefaultIdFormat = ActivityIdFormat.Hierarchical;
  Activity.ForceDefaultIdFormat = true;
```

Wenn Sie eine ältere Version des SDK ausführen, empfiehlt es sich, dieses zu aktualisieren oder die folgende Konfiguration anzuwenden, um den Trace-Context zu ermöglichen.

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

### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Aktivieren der Unterstützung für die verteilte Ablaufverfolgung von W3C für Java-Apps

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

### <a name="enable-w3c-distributed-tracing-support-for-web-apps"></a>Aktivieren der Unterstützung der verteilten W3C-Ablaufverfolgung für Web-Apps

Dieses Feature befindet sich in `Microsoft.ApplicationInsights.JavaScript`. Standardmäßig ist es deaktiviert. Um es zu aktivieren, verwenden Sie die Konfiguration von `distributedTracingMode`. AI_AND_W3C wird für Abwärtskompatibilität mit beliebigen mit älteren Application Insights-Versionen instrumentierten Diensten bereitgestellt:

- **NPM-Setup (bei Verwendung von Snippet-Setup ignorieren)**

  ```javascript
  import { ApplicationInsights, DistributedTracingModes } from '@microsoft/applicationinsights-web';

  const appInsights = new ApplicationInsights({ config: {
    instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
    distributedTracingMode: DistributedTracingModes.W3C
    /* ...Other Configuration Options... */
  } });
  appInsights.loadAppInsights();
  ```
  
- **Snippet-Setup (bei Verwendung von NPM-Setup ignorieren)**

  ```
  <script type="text/javascript">
  var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){i[e]=function(){var n=arguments;i.queue.push(function(){i[e].apply(i,n)})}}var i={config:e};i.initialize=!0;var a=document,t=window;setTimeout(function(){var n=a.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",a.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{i.cookie=a.cookie}catch(e){}i.queue=[],i.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var o="Track"+r[0];if(n("start"+o),n("stop"+o),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var s=t[r];t[r]=function(e,n,a,t,o){var c=s&&s(e,n,a,t,o);return!0!==c&&i["_"+r]({message:e,url:n,lineNumber:a,columnNumber:t,error:o}),c},e.autoExceptionInstrumented=!0}return i}
  (
    {
      instrumentationKey:"INSTRUMENTATION_KEY",
      distributedTracingMode: 2 // DistributedTracingModes.W3C
      /* ...Other Configuration Options... */
    }
  );
  window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
  </script>
  ```

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

## <a name="telemetry-correlation-in-opencensus-python"></a>Telemetriekorrelation in OpenCensus Python

OpenCensus Python folgt den oben beschriebenen Spezifikationen für das `OpenTracing`-Datenmodell. Außerdem wird [W3C Trace-Context](https://w3c.github.io/trace-context/) unterstützt, ohne dass eine Konfiguration erforderlich ist.

### <a name="incoming-request-correlation"></a>Korrelation eingehender Anforderungen

OpenCensus Python korreliert W3C Trace Context-Header von eingehenden Anforderungen mit den Spannen, die aus den Anforderungen selbst generiert werden. OpenCensus führt dies automatisch mit Integrationen für die folgenden gängigen Webanwendungsframeworks aus: `flask`, `django` und `pyramid`. Die W3C Trace Context-Header müssen einfach nur mit dem [korrekten Format](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format) aufgefüllt und mit der Anforderung gesendet werden. Es folgt eine `flask`-Beispielanwendung, die das veranschaulicht.

```python
from flask import Flask
from opencensus.ext.azure.trace_exporter import AzureExporter
from opencensus.ext.flask.flask_middleware import FlaskMiddleware
from opencensus.trace.samplers import ProbabilitySampler

app = Flask(__name__)
middleware = FlaskMiddleware(
    app,
    exporter=AzureExporter(),
    sampler=ProbabilitySampler(rate=1.0),
)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='localhost', port=8080, threaded=True)
```

Hiermit wird eine `flask`-Beispielanwendung auf dem lokalen Computer ausgeführt, der an Port `8080` lauscht. Zum Korrelieren des Ablaufverfolgungskontexts wird eine Anforderung an den Endpunkt gesendet. In diesem Beispiel kann ein `curl`-Befehl verwendet werden.
```
curl --header "traceparent: 00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01" localhost:8080
```
Durch einen Blick auf das [Format des Kontextheaders für die Ablaufverfolgung](https://www.w3.org/TR/trace-context/#trace-context-http-headers-format) können die folgenden Informationen abgeleitet werden: `version`: `00`
`trace-id`: `4bf92f3577b34da6a3ce929d0e0e4736`
`parent-id/span-id`: `00f067aa0ba902b7`
`trace-flags`: `01`

Wenn Sie sich den Anforderungseintrag ansehen, der an Azure Monitor gesendet wurde, sind dort Felder enthalten, die mit den Informationen des Ablaufverfolgungsheaders aufgefüllt wurden. Diese Daten sind in der Azure Monitor Application Insights-Ressource unter „Protokolle (Analytics)“ zu finden.

![Screenshot der Anforderungstelemetrie in „Protokolle (Analytics)“ mit rot umrandeten Feldern für den Ablaufverfolgungsheader](./media/opencensus-python/0011-correlation.png)

Das Feld `id` hat das Format `<trace-id>.<span-id>`. Hierbei wird `trace-id` aus dem in der Anforderung übergebenen Ablaufverfolgungsheader entnommen, und `span-id` ist ein generiertes 8-Byte-Array für diese Spanne. 

Das Feld `operation_ParentId` hat das Format `<trace-id>.<parent-id>`. Hierbei werden sowohl `trace-id` als auch `parent-id` aus dem in der Anforderung übergebenen Ablaufverfolgungsheader entnommen.

### <a name="logs-correlation"></a>Korrelation von Protokollen

OpenCensus Python ermöglicht die Korrelation von Protokollen, indem Protokolldatensätze um Ablaufverfolgungs-ID, Spannen-ID und Stichprobenflag erweitert werden. Dazu wird die OpenCensus-[Protokollintegration](https://pypi.org/project/opencensus-ext-logging/) installiert. Python `LogRecord` werden die folgenden Attribute hinzugefügt: `traceId`, `spanId` und `traceSampled`. Beachten Sie, dass dies nur für Protokollierungen wirksam wird, die nach der Integration erstellt wurden.
Es folgt eine Beispielanwendung, die das veranschaulicht.

```python
import logging

from opencensus.trace import config_integration
from opencensus.trace.samplers import AlwaysOnSampler
from opencensus.trace.tracer import Tracer

config_integration.trace_integrations(['logging'])
logging.basicConfig(format='%(asctime)s traceId=%(traceId)s spanId=%(spanId)s %(message)s')
tracer = Tracer(sampler=AlwaysOnSampler())

logger = logging.getLogger(__name__)
logger.warning('Before the span')
with tracer.span(name='hello'):
    logger.warning('In the span')
logger.warning('After the span')
```
Bei Ausführung dieses Codes wird Folgendes in der Konsole angezeigt:
```
2019-10-17 11:25:59,382 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 Before the span
2019-10-17 11:25:59,384 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=70da28f5a4831014 In the span
2019-10-17 11:25:59,385 traceId=c54cb1d4bbbec5864bf0917c64aeacdc spanId=0000000000000000 After the span
```
Sie können sehen, dass für die Protokollnachricht, die innerhalb der Spanne liegt, eine Spannen-ID vorhanden ist. Diese entspricht der Spannen-ID, die zur Spanne mit dem Namen `hello` gehört.

Sie können die Protokolldaten mithilfe von `AzureLogHandler`exportieren. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python#logs)

## <a name="telemetry-correlation-in-net"></a>Telemetriekorrelation in .NET

Im Lauf der Zeit wurden in .NET verschiedene Möglichkeiten zur Korrelation von Telemetrie- und Diagnoseprotokollen definiert:

- `System.Diagnostics.CorrelationManager` ermöglicht die Nachverfolgung von [LogicalOperationStack und ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). 
- `System.Diagnostics.Tracing.EventSource` und die Ereignisablaufverfolgung für Windows (ETW) definieren die [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx)-Methode.
- `ILogger` verwendet [Protokollbereiche](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). 
- Windows Communication Foundation (WCF) und HTTP bieten eine Propagierung des Kontexts „aktuell“.

Allerdings wurde durch diese Methoden nicht die Unterstützung für automatische verteilte Ablaufverfolgung aktiviert. `DiagnosticSource` ist eine Möglichkeit zur Unterstützung von automatischen computerübergreifenden Korrelationen. .NET-Bibliotheken unterstützen Diagnosequellen und ermöglichen die automatische computerübergreifende Propagierung des Korrelationskontexts durch die Übertragungsmethode, z.B. HTTP.

Im [Leitfaden zu Aktivitäten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in `DiagnosticSource` werden die Grundlagen von Nachverfolgungsaktivitäten erläutert.

ASP.NET Core 2.0 unterstützt die Extraktion von HTTP-Headern und das Starten einer neuen Aktivität.

`System.Net.Http.HttpClient` ab Version 4.1.0 unterstützt die automatische Injektion der Korrelations-HTTP-Header und die Nachverfolgung des HTTP-Aufrufs als Aktivität.

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

- Ab Application Insights Java SDK 2.5.0 können Sie den Cloudrollennamen angeben, indem Sie der Datei `<RoleName>` den Eintrag `ApplicationInsights.xml` hinzufügen.

  ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
     <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
     <RoleName>** Your role name **</RoleName>
     ...
  </ApplicationInsights>
  ```

- Bei Verwendung von Spring Boot mit dem Application Insights Spring Boot-Startprogramm besteht die einzige erforderliche Änderung darin, Ihren benutzerdefinierten Namen für die Anwendung in der Datei „application.properties“ festzulegen.

  `spring.application.name=<name-of-app>`

  Der Spring Boot-Starter weist `cloudRoleName` automatisch den Wert zu, den Sie für die `spring.application.name`-Eigenschaft eingeben.

## <a name="next-steps"></a>Nächste Schritte

- Schreiben Sie [benutzerdefinierte Telemetriedaten](../../azure-monitor/app/api-custom-events-metrics.md).
- Informationen zu Szenarien mit erweiterter Korrelation in ASP.NET Core und ASP.NET finden Sie im Artikel zum [Nachverfolgen von benutzerdefinierten Vorgängen](custom-operations-tracking.md).
- Erfahren Sie mehr über das [Festlegen von cloud_RoleName](../../azure-monitor/app/app-map.md#set-cloud-role-name) für andere SDKs.
- Integrieren Sie alle Komponenten Ihres Microservices in Application Insights. Sehen Sie sich die [unterstützten Plattformen](../../azure-monitor/app/platforms.md) an.
- Lesen Sie die Informationen zu den Application Insights-Typen im [Datenmodell](../../azure-monitor/app/data-model.md).
- Informationen zum [Erweitern und Filtern von Telemetriedaten](../../azure-monitor/app/api-filtering-sampling.md).
- Lesen Sie die [Konfigurationsreferenz für Application Insights](configuration-with-applicationinsights-config.md).
