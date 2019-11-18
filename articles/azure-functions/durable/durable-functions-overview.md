---
title: Übersicht zu Durable Functions – Azure
description: Einführung in die Durable Functions-Erweiterung für Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 08/07/2019
ms.author: cgillum
ms.reviewer: azfuncdf
ms.openlocfilehash: 0b85d6fbe8e66b94bad372ccb29e5489dd81587b
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73614787"
---
# <a name="what-are-durable-functions"></a>Was ist Durable Functions?

*Durable Functions* ist eine Erweiterung von [Azure Functions](../functions-overview.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Compute-Umgebung schreiben können. Mit der Erweiterung können Sie mithilfe des Azure Functions-Programmiermodells zustandsbehaftete Workflows durch Schreiben von [*Orchestratorfunktionen*](durable-functions-orchestrations.md) und zustandsbehaftete Entitäten durch Schreiben von [*Entitätsfunktionen*](durable-functions-entities.md) definieren. Im Hintergrund verwaltet die Erweiterung Zustand, Prüfpunkte und Neustarts für Sie, sodass Sie sich auf Ihre Geschäftslogik konzentrieren können.

## <a name="language-support"></a>Unterstützte Sprachen

Durable Functions unterstützt derzeit die folgenden Sprachen:

* **C#** : sowohl [vorkompilierte Klassenbibliotheken](../functions-dotnet-class-library.md) als auch [ C#-Skript](../functions-reference-csharp.md).
* **F#** : vorkompilierte Klassenbibliotheken und F#-Skript. F#-Skript wird nur für Version 1.x der Azure Functions-Runtime unterstützt.
* **JavaScript**: nur für Version 2.x der Azure Functions-Runtime unterstützt. Erfordert mindestens Version 1.7.0 der Durable Functions-Erweiterung. 

Das Ziel von Durable Functions ist die Unterstützung aller [Azure Functions-Sprachen](../supported-languages.md). In der [Liste der Durable Functions-Probleme](https://github.com/Azure/azure-functions-durable-extension/issues) finden Sie den aktuellen Status der Arbeit, um zusätzliche Sprachen zu unterstützen.

Wie bei Azure Functions sind Vorlagen verfügbar, um Sie bei der Entwicklung von Durable Functions mit [Visual Studio 2019](durable-functions-create-first-csharp.md), [Visual Studio Code](quickstart-js-vscode.md) und dem [Azure-Portal](durable-functions-create-portal.md) zu unterstützen.

## <a name="application-patterns"></a>Anwendungsmuster

Der primäre Anwendungsfall für Durable Functions ist die Vereinfachung komplexer, zustandsbehafteter Koordinationsanforderungen in serverlosen Anwendungen. In den folgenden Abschnitten werden typische Anwendungsmuster beschrieben, die von Durable Functions profitieren können:

* [Funktionsverkettung](#chaining)
* [Auffächern nach außen/innen](#fan-in-out)
* [Asynchrone HTTP-APIs](#async-http)
* [Überwachung](#monitoring)
* [Benutzerinteraktion](#human)
* [Aggregator](#aggregator)

### <a name="chaining"></a>Muster 1: Funktionsverkettung

Beim Muster der Funktionsverkettung wird eine Abfolge von Funktionen in einer bestimmten Reihenfolge ausgeführt. Bei diesem Muster wird die Ausgabe einer Funktion als Eingabe einer weiteren Funktion verwendet.

![Ein Diagramm des Funktionsverkettungsmusters](./media/durable-functions-concepts/function-chaining.png)

Mithilfe von Durable Functions können Sie das Funktionsverkettungsmuster präzise wie im folgenden Beispiel dargestellt implementieren:

#### <a name="c"></a>C#

```csharp
[FunctionName("Chaining")]
public static async Task<object> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1", null);
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // Error handling or compensation goes here.
    }
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return    yield context.df.callActivity("F4", z);
});
```

In diesem Beispiel sind die Werte `F1`, `F2`, `F3` und `F4` die Namen weiterer Funktionen in der Funktions-App. Sie können die Ablaufsteuerung mithilfe normaler imperativer Codierungskonstrukte implementieren. Der Code wird von oben nach unten ausgeführt. Der Code kann bestehende sprachliche Ablaufsteuerungssemantik wie Bedingungsanweisungen und Schleifen umfassen. Sie können Logik zur Fehlerbehandlung in `try`/`catch`/`finally`-Blöcken einschließen.

Sie können den `context`-Parameter [IDurableOrchestrationContext] \(.NET\) und das `context.df`-Objekt (JavaScript) verwenden, um andere Funktionen anhand des Namens aufzurufen, Parameter zu übergeben und Funktionsausgaben zurückzugeben. Jedes Mal, wenn der Code `await` (C#) oder `yield` (JavaScript) aufruft, erstellt das Durable Functions Framework Prüfpunkte zum Status der aktuellen Funktionsinstanz. Wenn der Prozess oder die VM mitten in der Ausführung neu gestartet wird, wird die Funktionsinstanz ab dem vorherigen `await`- bzw. `yield`-Aufruf fortgesetzt. Weitere Informationen finden Sie im nächsten Abschnitt, Muster 2: Auffächern auswärts/einwärts.

> [!NOTE]
> Das `context`-Objekt in JavaScript stellt den gesamten [Funktionskontext](../functions-reference-node.md#context-object) dar, nicht nur den [IDurableOrchestrationContext]-Parameter.

### <a name="fan-in-out"></a>Muster 2: Auffächern auswärts/einwärts

Beim Muster Auffächern auswärts/einwärts werden mehrere Funktionen parallel ausgeführt und anschließend auf den Abschluss aller gewartet. Häufig werden die von den Funktionen zurückgegebenen Ergebnisse einer Aggregation unterzogen.

![Ein Diagramm des Musters „Auffächern auswärts/einwärts“](./media/durable-functions-concepts/fan-out-fan-in.png)

Bei normalen Funktionen kann das Auffächern auswärts erfolgen, indem die Funktion mehrere Nachrichten an eine Warteschlange sendet. Das Auffächern zurück nach innen ist wesentlich schwieriger. Für das Auffächern nach innen schreiben Sie in einer normalen Funktion Code, der nachverfolgt, wann die von der Warteschlange ausgelösten Funktionen enden und speichern dann ihre Ausgaben.

Die Erweiterung Durable Functions wird diesem Muster mit relativ einfachem Code gerecht:

#### <a name="c"></a>C#

```csharp
[FunctionName("FanOutFanIn")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1", null);
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // Get a list of N work items to process in parallel.
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Das Auffächern nach außen ist auf mehrere Instanzen der `F2`-Funktion verteilt. Die Arbeit wird mithilfe einer dynamischen Aufgabenliste nachverfolgt. Die .NET-API `Task.WhenAll` oder die JavaScript-API `context.df.Task.all` wird aufgerufen, um auf den Abschluss der aufgerufenen Funktionen zu warten. Anschließend werden die Ausgaben der `F2`-Funktion aus der dynamischen Aufgabenliste aggregiert und an die `F3`-Funktion übergeben.

Die automatische Prüfpunkterstellung, die beim `await`- oder `yield`-Aufruf für `Task.WhenAll` bzw. `context.df.Task.all` erfolgt, stellt sicher, dass ein potentieller Absturz oder Neustart während der Ausführung keinen Neustart bereits abgeschlossener Aufgaben erfordert.

> [!NOTE]
> In seltenen Fällen kann es im Fenster zu einem Absturz kommen, nachdem eine Aktivitätsfunktion abgeschlossen wurde und bevor ihr Abschluss im Orchestrierungsverlauf gespeichert wurde. In dem Fall wird die Aktivitätsfunktion von Anfang an erneut ausgeführt, wenn der Prozess wieder hergestellt wurde.

### <a name="async-http"></a>Muster 3: Asynchrone HTTP-APIs

Das asynchrone HTTP-API-Muster ist geeignet, um den Status von Vorgängen mit langer Ausführungsdauer mit externen Clients zu koordinieren. Ein gängiges Verfahren zum Implementieren dieses Musters besteht darin, die Aktion mit langer Ausführungsdauer von einem HTTP-Endpunkt auslösen zu lassen. Leiten Sie dann den Client zu einem Statusendpunkt um, den der Client abfragt, um herauszufinden, wenn der Vorgang abgeschlossen ist.

![Ein Diagramm des HTTP-API-Musters](./media/durable-functions-concepts/async-http-api.png)

Durable Functions bietet **integrierte Unterstützung** für dieses Muster und vereinfacht oder entfernt den Code, den Sie für die Interaktion mit langen Funktionsausführungen schreiben. Die Schnellstartbeispiele für Durable Functions ([C#](durable-functions-create-first-csharp.md) und [JavaScript](quickstart-js-vscode.md)) zeigen beispielsweise einen einfachen REST-Befehl, den Sie zum Starten neuer Orchestratorfunktionsinstanzen verwenden können. Nach dem Start einer Instanz macht die Erweiterung Webhook-HTTP-APIs verfügbar, die den Status der Orchestratorfunktion abfragen. 

Das folgende Beispiel zeigt REST-Befehle zum Starten eines Orchestrators und zum Abfragen seines Status. Zur besseren Übersichtlichkeit werden im Beispiel einige Protokolldetails weggelassen.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2019-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/runtime/webhooks/durabletask/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2019-03-16T21:20:57Z", ...}
```

Da der Status von der Durable Functions-Runtime für Sie verwaltet wird, müssen Sie keinen eigenen Mechanismus zur Statusnachverfolgung implementieren.

Die Durable Functions-Erweiterung macht integrierte HTTP-APIs verfügbar, die Orchestrierungen mit langer Ausführungsdauer verwalten. Sie können dieses Muster auch selbst implementieren, indem Sie Ihre eigenen Funktionstrigger (wie etwa HTTP, eine Warteschlange oder Azure Event Hubs) und die [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client) verwenden. Sie können beispielsweise eine Warteschlangennachricht verwenden, um die Beendigung auszulösen. Sie könnten anstelle der integrierten HTTP-APIs, die einen generierten Schlüssel für die Authentifizierung verwenden, auch einen HTTP-Trigger verwenden, der durch eine Azure Active Directory-Authentifizierungsrichtlinie geschützt ist.

Weitere Informationen finden Sie im Artikel [HTTP-Features](durable-functions-http-features.md), in dem erläutert wird, wie Sie asynchrone Prozesse mit langer Ausführungszeit über HTTP mithilfe der Durable Functions-Erweiterung verfügbar machen können.

### <a name="monitoring"></a>Muster 4: Überwachen

Das Überwachen-Muster bezieht sich auf einen flexiblen, wiederkehrenden Vorgang in einem Workflow. Ein Beispiel besteht im Abfragen, bis bestimmte Bedingungen erfüllt sind. Sie können einen normalen [Timertrigger](../functions-bindings-timer.md) für ein einfaches Szenario verwenden, beispielsweise einen periodischen Bereinigungsauftrag. Sein Intervall ist jedoch statisch, und die Verwaltung der Instanzlebensdauer wird komplex. Mithilfe von Durable Functions können Sie flexible Wiederholungsintervalle erstellen, die Lebensdauer von Aufgaben verwalten und mehrere Überwachungsprozesse aus einer einzelnen Orchestrierung erstellen.

Ein Beispiel für das Überwachen-Muster besteht in der Umkehrung des früheren asynchronen HTTP-API-Szenarios. Anstatt einen Endpunkt für einen externen Client freizugeben, um einen lang laufenden Vorgang zu überwachen, belegt der lang laufende Monitor einen externen Endpunkt und wartet dann auf einen Zustandswechsel.

![Ein Diagramm des Überwachen-Musters](./media/durable-functions-concepts/monitor.png)

Mit ein paar Codezeilen können Sie Durable Functions dazu verwenden, mehrere Monitore zu erstellen, die beliebige Endpunkte beobachten. Die Monitore können die Ausführung beenden, wenn eine Bedingung erfüllt ist, oder der `IDurableOrchestrationClient` kann die Monitore beenden. Sie können das `wait`-Intervall eines Monitors auf der Grundlage einer spezifischen Bedingung (z.B. exponentielles Backoff) ändern. 

Der folgende Code implementiert einen einfachen Monitor:

#### <a name="c"></a>C#

```csharp
[FunctionName("MonitorJobStatus")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform an action when a condition is met.
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform more work here, or let the orchestration end.
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform an action when a condition is met.
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform more work here, or let the orchestration end.
});
```

Wenn eine Anforderung empfangen wird, wird eine neue Orchestrierungsinstanz für diese Auftrags-ID erstellt. Die Instanz fragt den Status ab, bis eine Bedingung erfüllt ist und die Schleife beendet wird. Ein permanenter Timer steuert das Abrufintervall. Anschließend können weitere Arbeitsschritte ausgeführt werden, oder die Orchestrierung wird beendet. Wenn `context.CurrentUtcDateTime` (.NET) oder `context.df.currentUtcDateTime` (JavaScript) den Wert von `expiryTime` überschreitet, wird der Monitor beendet.

### <a name="human"></a>Muster 5: Benutzerinteraktion

Viele automatisierte Prozesse enthalten eine Form der Benutzerinteraktion. Das Einbeziehen von Menschen in einen automatisierten Prozess ist schwierig, da Personen nicht im gleichen hohen Maß verfügbar und reaktionsfähig sind wie Clouddienste. Ein automatisierter Prozess kann diese Interaktion mithilfe von Zeitlimits und Kompensationslogik ermöglichen.

Ein Genehmigungsprozess ist ein Beispiel für einen Geschäftsprozesses, der Benutzerinteraktion umfasst. Beispielsweise kann für eine Spesenabrechnung, die einen bestimmten Betrag überschreitet, die Genehmigung eines Vorgesetzten erforderlich sein. Wenn der Vorgesetzte die Spesenabrechnung nicht innerhalb von 72 Stunden genehmigt (vielleicht weil er im Urlaub ist), wird ein Eskalationsverfahren wirksam, um die Genehmigung von einer anderen Person (z.B. dem Vorgesetzten des Vorgesetzten) zu erhalten.

![Ein Diagramm des Musters „Benutzerinteraktion“](./media/durable-functions-concepts/approval.png)

Sie können das Muster aus diesem Beispiel mithilfe einer Orchestrierungsfunktion implementieren. Der Orchestrator verwendet einen [permanenten Timer](durable-functions-timers.md), um die Genehmigung anzufordern. Der Orchestrator führt die Eskalation aus, wenn das Timeout auftritt. Der Orchestrator wartet auf ein [externes Ereignis](durable-functions-external-events.md), beispielsweise eine Benachrichtigung, die durch eine Benutzerinteraktion generiert wird.

In diesen Beispielen wird ein Genehmigungsprozess erstellt, um das Muster der Benutzerinteraktion zu veranschaulichen:

#### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalWorkflow")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval", null);
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate", null);
        }
    }
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

Rufen Sie zum Erstellen des permanenten Timers `context.CreateTimer` (.NET) oder `context.df.createTimer` (JavaScript) auf. Die Benachrichtigung wird von `context.WaitForExternalEvent` (.NET) oder `context.df.waitForExternalEvent` (JavaScript) empfangen. Anschließend wird `Task.WhenAny` (.NET) oder `context.df.Task.any` (JavaScript) aufgerufen, um zu entscheiden, ob eine Eskalation erfolgt (das Timeout tritt zuerst auf) oder die Genehmigung verarbeitet wird (die Genehmigung wird vor dem Timeout empfangen).

Ein externer Client kann die Ereignisbenachrichtigung entweder über die [integrierten HTTP-APIs](durable-functions-http-api.md#raise-event) oder die `RaiseEventAsync`-Methode (.NET) oder `raiseEvent`-Methode (JavaScript) einer anderen Funktion an eine wartende Orchestratorfunktion senden:

```csharp
[FunctionName("RaiseEventToOrchestration")]
public static async Task Run(
    [HttpTrigger] string instanceId,
    [DurableClient] IDurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

```bash
curl -d "true" http://localhost:7071/runtime/webhooks/durabletask/instances/{instanceId}/raiseEvent/ApprovalEvent -H "Content-Type: application/json"
```

### <a name="aggregator"></a>Muster 6: Aggregator

Beim sechsten Muster geht es um Aggregierung von Ereignisdaten über einen bestimmten Zeitraum in einer einzigen, adressierbaren *Entität*. In diesem Muster können die aggregierten Daten aus mehreren Quellen stammen, in Batches geliefert werden und über lange Zeiträume verteilt sein. Der Aggregator muss möglicherweise Aktionen für Ereignisdaten durchführen, wenn er diese empfängt, und es kann sein, dass externe Daten die aggregierten Daten abfragen müssen.

![Aggregatordiagramm](./media/durable-functions-concepts/aggregator.png)

Das Schwierige an der Implementierung dieses Musters mit normalen, zustandslosen Funktionen ist die Tatsache, dass das Steuern der Parallelität zur Herausforderung wird. Sie müssen sich nicht nur um mehrere Threads kümmern, die gleichzeitig dieselben Daten anpassen, sondern Sie müssen auch sicherstellen, dass der Aggregator immer nur auf einer VM ausgeführt wird.

Sie können [dauerhafte Entitäten](durable-functions-entities.md) verwenden, um dieses Muster problemlos als einzelne Funktion zu implementieren.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();
    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            ctx.SetState(currentValue + amount);
            break;
        case "reset":
            ctx.SetState(0);
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }
}
```

```javascript
const df = require("durable-functions");

module.exports = df.entity(function(context) {
    const currentValue = context.df.getState(() => 0);
    switch (context.df.operationName) {
        case "add":
            const amount = context.df.getInput();
            context.df.setState(currentValue + amount);
            break;
        case "reset":
            context.df.setState(0);
            break;
        case "get":
            context.df.return(currentValue);
            break;
    }
});
```

Dauerhafte Entitäten können auch als Klassen in .NET modelliert werden. Dieses Modell ist bei einer festen Liste von Vorgängen hilfreich, die recht groß wird. Beim folgenden Beispiel handelt es sich um eine äquivalente Implementierung der `Counter`-Entität unter Verwendung von .NET-Klassen und -Methoden.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;

    public void Reset() => this.CurrentValue = 0;

    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

Clients können *Vorgänge* mithilfe der [Entitätsclientbindung](durable-functions-bindings.md#entity-client) für eine Entity-Funktion in eine Warteschlange einreihen.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static async Task Run(
    [EventHubTrigger("device-sensor-events")] EventData eventData,
    [DurableClient] IDurableOrchestrationClient entityClient)
{
    var metricType = (string)eventData.Properties["metric"];
    var delta = BitConverter.ToInt32(eventData.Body, eventData.Body.Offset);

    // The "Counter/{metricType}" entity is created on-demand.
    var entityId = new EntityId("Counter", metricType);
    await entityClient.SignalEntityAsync(entityId, "add", delta);
}
```

> [!NOTE]
> Außerdem stehen dynamisch generierte Proxys in .NET für signalisierende Entitäten auf typsichere Weise zur Verfügung. Zusätzlich zur Signalisierung können Clients auch den Zustand einer Entitätsfunktion mithilfe [typsicherer Methoden](durable-functions-bindings.md#entity-client-usage) für die Orchestrierungsclientbindung abfragen.


```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await context.df.signalEntity(entityId, "add", 1);
};
```

Entitätsfunktionen stehen ab [Durable Functions 2.0](durable-functions-versions.md) zur Verfügung.

## <a name="the-technology"></a>Die Technologie

Im Hintergrund baut die Durable Functions-Erweiterung auf dem [Durable Task Framework](https://github.com/Azure/durabletask) auf, einer Open-Source-Bibliothek auf GitHub, die zum Erstellen von Workflows im Code verwendet wird. Wie Azure Functions die serverlose Weiterentwicklung von Azure WebJobs ist, so ist Durable Functions die serverlose Weiterentwicklung von Durable Task Framework. Microsoft und andere Unternehmen verwenden das Durable Task Framework intensiv zum Automatisieren unternehmenswichtiger Prozesse. Es ist wie geschaffen für die serverlose Azure Functions-Umgebung.

## <a name="code-constraints"></a>Codeeinschränkungen

Damit eine zuverlässige und lange Ausführung gewährleistet ist, gelten für Orchestratorfunktionen einige Programmierregeln, die beachtet werden müssen. Weitere Informationen finden Sie im Artikel [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md).

## <a name="billing"></a>Abrechnung

Durable Functions wird genau wie Azure Functions in Rechnung gestellt. Weitere Informationen finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/). Beim Ausführen von Orchestratorfunktionen im [Nutzungsplan](../functions-scale.md#consumption-plan) von Azure Functions sind einige Verhaltensweisen zu beachten, die bei der Abrechnung auftreten. Weitere Informationen zu diesen Verhaltensweisen finden Sie im Artikel [Abrechnung von Durable Functions](durable-functions-billing.md).

## <a name="jump-right-in"></a>Sofort loslegen

Sie können die ersten Schritte mit Durable Functions in weniger als 10 Minuten durchführen, indem Sie eines dieser sprachspezifischen Schnellstarttutorials abschließen:

* [C# mit Visual Studio 2019](durable-functions-create-first-csharp.md)
* [JavaScript mit Visual Studio Code](quickstart-js-vscode.md)

In beiden Schnellstarts erstellen Sie lokal eine dauerhafte „Hallo Welt“-Funktion und testen diese. Anschließend veröffentlichen Sie den Funktionscode in Azure. Mit der von Ihnen erstellten Funktion werden Aufrufe anderer Funktionen orchestriert und miteinander verkettet.

## <a name="learn-more"></a>Weitere Informationen

Im folgenden Video werden die Vorteile von Durable Functions aufgezeigt:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Eine ausführlichere Erläuterung von Durable Functions und der zugrunde liegenden Technologie finden Sie im folgenden Video (es bezieht sich auf .NET, aber die Konzepte gelten aber auch für andere unterstützte Sprachen):

> [!VIDEO https://channel9.msdn.com/Events/dotnetConf/2018/S204/player]

Da es sich bei Durable Functions um eine fortgeschrittene Erweiterung für [Azure Functions](../functions-overview.md) handelt, ist sie nicht für alle Anwendungen geeignet. Einen Vergleich mit anderen Azure-Orchestrierungstechnologien finden Sie unter [Vergleich zwischen Azure Functions und Azure Logic Apps](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durable Functions-Typen und -Features (Azure Functions)](durable-functions-types-features-overview.md)
