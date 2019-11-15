---
title: Dauerhafte Entitäten – Azure Functions
description: Hier erfahren Sie, was dauerhafte Entitäten sind und wie sie in der Durable Functions-Erweiterung für Azure Functions verwendet werden.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 62ca71e1b42e000f7528a2963793f9bf40663bf3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818504"
---
# <a name="entity-functions"></a>Entitätsfunktionen

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsteile, bekannt als *dauerhafte Entitäten*. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Im Gegensatz zu Orchestratorfunktionen stellen Entitätsfunktionen den Zustand nicht über eine Ablaufsteuerung dar, sondern verwalten den Zustand einer Entität explizit.
Anwendungen können mithilfe von Entitäten erweitert werden, indem die Arbeit auf zahlreiche Entitäten verteilt wird, deren Zustand jeweils eine bescheidene Größe aufweist.

> [!NOTE]
> Entitätsfunktionen und zugehörige Funktionen sind nur in Durable Functions 2.0 und höher verfügbar.

## <a name="general-concepts"></a>Allgemeine Konzepte

Entitäten verhalten sich ein wenig wie kleine Dienste, die mithilfe von Nachrichten kommunizieren. Jede Entität hat eine eindeutige Identität und einen internen Zustand (sofern vorhanden). Entitäten führen genau wie Dienste oder Objekte Vorgänge aus, wenn sie dazu aufgefordert werden. Der interne Zustand der Entität kann ggf. durch einen ausgeführten Vorgang aktualisiert werden. Ein Vorgang kann auch externe Dienste aufrufen und auf eine Antwort warten. Entitäten kommunizieren mit anderen Entitäten, Orchestrierungen und Clients mithilfe von Nachrichten, die implizit über zuverlässige Warteschlangen gesendet werden. 

Zur Vermeidung von Konflikten werden alle Vorgänge für eine einzelne Entität seriell (also nacheinander) ausgeführt. 

### <a name="entity-id"></a>Entitäts-ID
Der Zugriff auf Entitäten erfolgt über einen eindeutigen Bezeichner: die *Entitäts-ID*. Eine Entitäts-ID ist einfach ein Paar aus Zeichenfolgen, das eine Entitätsinstanz eindeutig identifiziert. Sie besteht aus:

* einem **Entitätsnamen**: Ein Name, der den Typ der Entität identifiziert (z. B. „Zähler“). Dieser Name muss dem Namen der Entitätsfunktion entsprechen, von der die Entität implementiert wird. Die Groß-/Kleinschreibung wird dabei nicht beachtet.
* einem **Entitätsschlüssel**: Eine Zeichenfolge, die die Entität unter allen anderen Entitäten desselben Namens eindeutig identifiziert (z. B. eine GUID).

Beispielsweise könnte eine *Counter*-Entitätsfunktion verwendet werden, um den Punktestand in einem Onlinespiel nachzuhalten. Jede Instanz des Spiels besitzt dann eine eindeutige Entitäts-ID, z. B. `@Counter@Game1`, `@Counter@Game2` usw. Bei allen Vorgängen, die auf eine bestimmte Entität ausgerichtet sind, muss eine Entitäts-ID als Parameter angegeben werden.

### <a name="entity-operations"></a>Entitätsvorgänge ###

Wenn Sie einen Vorgang für eine Entität aufrufen möchten, muss Folgendes angegeben werden:

* Die *Entitäts-ID* der Zielentität.
* Der *Name des Vorgangs* (eine Zeichenfolge, die den auszuführenden Vorgang angibt). Die Counter-Entität könnte beispielsweise Vorgänge vom Typ „add“, „get“ oder „reset“ unterstützen.
* Die *Vorgangseingabe* (ein optionaler Eingabeparameter für den Vorgang). Der Vorgang „add“ akzeptiert beispielsweise eine ganzzahlige Menge als Eingabe.

Vorgänge können einen Ergebniswert oder ein Fehlerergebnis zurückgeben (etwa einen JavaScript-Fehler oder eine .NET-Ausnahme). Dieses Ergebnis bzw. dieser Fehler kann von Orchestrierungen berücksichtigt werden, die den Vorgang aufgerufen haben.

Entitätsvorgänge können auch den Zustand der Entität erstellen, lesen, aktualisieren und löschen. Der Zustand der Entität wird immer dauerhaft im Speicher aufbewahrt.

## <a name="defining-entities"></a>Definieren von Entitäten

Zum Definieren von Entitäten stehen aktuell zwei unterschiedliche APIs zur Verfügung:

Eine **funktionsbasierte Syntax**, bei der Entitäten als Funktionen dargestellt und Vorgänge explizit von der Anwendung verteilt werden. Diese Syntax eignet sich gut für Entitäten mit einfachem Zustand, wenigen Vorgängen oder einem dynamischen Satz von Vorgängen (beispielsweise in Anwendungsframeworks). Die Verwaltung kann sich allerdings als aufwendig erweisen, da Typfehler zur Kompilierzeit nicht abgefangen werden.

Eine **klassenbasierte Syntax**, bei der Entitäten und Vorgänge durch Klassen und Methoden dargestellt werden. Bei dieser Syntax ist der Code besser lesbar, und Vorgänge können typsicher aufgerufen werden. Da es sich bei der klassenbasierten Syntax lediglich um eine dünne Schicht über der funktionsbasierten Syntax handelt, können beide Varianten in der gleichen Anwendung verwendet werden.

### <a name="example-function-based-syntax---c"></a>Beispiel: Funktionsbasierte Syntax – C#

Der folgende Code ist ein Beispiel für eine einfache Entität vom Typ *Counter*, die als dauerhafte Funktion implementiert wird. Diese Funktion definiert drei Vorgänge (`add`, `reset` und `get`), die jeweils auf einem ganzzahligen Zustand basieren.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            ctx.SetState(ctx.GetState<int>() + ctx.GetInput<int>());
            break;
        case "reset":
            ctx.SetState(0);
            break;
        case "get":
            ctx.Return(ctx.GetState<int>());
            break;
    }
}
```

Weitere Informationen zur funktionsbasierten Syntax sowie zu deren Verwendung finden Sie unter [Funktionsbasierte Syntax](durable-functions-dotnet-entities.md#function-based-syntax).

### <a name="example-class-based-syntax---c"></a>Beispiel: Klassenbasierte Syntax – C#

Beim folgenden Beispiel handelt es sich um eine äquivalente Implementierung der Entität `Counter` unter Verwendung von Klassen und Methoden:

```csharp
[JsonObject(MemberSerialization.OptIn)]
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

Der Zustand dieser Entität ist ein Objekt vom Typ `Counter`. Dieses enthält ein Feld, in dem der aktuelle Wert des Zählers gespeichert wird. Um dieses Objekt dauerhaft im Speicher aufzubewahren, wird es von der Bibliothek [Json.NET](https://www.newtonsoft.com/json) serialisiert und deserialisiert. 

Weitere Informationen zur klassenbasierten Syntax sowie zu deren Verwendung finden Sie unter [Definieren von Entitätsklassen](durable-functions-dotnet-entities.md#defining-entity-classes).

### <a name="example-javascript-entity"></a>Beispiel: JavaScript-Entität

Dauerhafte Entitäten sind ab JavaScript-Version **1.3.0** des `durable-functions`-npm-Pakets verfügbar. Der folgende Code ist die Entität *Counter*, implementiert als dauerhafte Funktion in JavaScript.

**function.json**
```json
{
  "bindings": [
    {
      "name": "context",
      "type": "entityTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

**index.js**
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

## <a name="accessing-entities"></a>Zugreifen auf Entitäten

Auf Entitäten kann mittels unidirektionaler oder bidirektionaler Kommunikation zugegriffen werden. Zur Unterscheidung wird folgende Terminologie verwendet: 

* Beim **Aufrufen** einer Entität kommt die bidirektionale Kommunikation (Roundtrip) zum Einsatz: Hierbei wird eine Vorgangsnachricht an die Entität gesendet und auf die Antwortnachricht gewartet, bevor der Vorgang fortgesetzt wird. Die Antwortnachricht kann einen Ergebniswert oder ein Fehlerergebnis zurückgeben (etwa einen JavaScript-Fehler oder eine .NET-Ausnahme). Das Ergebnis oder der Fehler wird dann vom Aufrufer berücksichtigt.
* Beim **Signalisieren** einer Entität kommt die unidirektionale Kommunikation (Fire and Forget) zum Einsatz: Hierbei wird eine Vorgangsnachricht gesendet, aber nicht auf eine Antwort gewartet. Die Nachricht wird zwar garantiert übermittelt, der Absender weiß jedoch nicht, wann dies der Fall ist, und er kann auch keinen Ergebniswert und keine Fehler berücksichtigen.

Auf Entitäten kann innerhalb von Clientfunktionen, innerhalb von Orchestratorfunktionen oder innerhalb von Entitätsfunktionen zugegriffen werden. Nicht alle Kommunikationsarten werden von allen Kontexten unterstützt:

* Innerhalb von Clients können Sie Entitäten *signalisieren* und den Entitätszustand *lesen*.
* Innerhalb von Orchestrierungen können Sie Entitäten *signalisieren* und *aufrufen*.
* Innerhalb von Entitäten können Sie Entitäten *signalisieren*.

Im Anschluss finden Sie einige Beispiele zur Veranschaulichung der verschiedenen Entitätszugriffsmethoden.

> [!NOTE]
> Der Einfachheit halber wird in den folgenden Beispielen die lose typisierte Syntax für den Entitätszugriff verwendet. Aufgrund der besseren Typüberprüfung empfiehlt es sich im Allgemeinen, [über Schnittstellen auf Entitäten zuzugreifen](durable-functions-dotnet-entities.md#accessing-entities-through-interfaces).

### <a name="example-client-signals-an-entity"></a>Beispiel: Client signalisiert eine Entität

Wenn Sie von einer normalen Azure-Funktion (auch *Clientfunktion* genannt) aus auf Entitäten zugreifen möchten, verwenden Sie die [Entitätsclient-Ausgabebindung](durable-functions-bindings.md#entity-client). Das folgende Beispiel zeigt eine durch eine Warteschlange ausgelöste Funktion zur *Signalisierung* einer Entität unter Verwendung dieser Bindung:

```csharp
[FunctionName("AddFromQueue")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [DurableClient] IDurableEntityClient client)
{
    // Entity operation input comes from the queue message content.
    var entityId = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(input);
    return client.SignalEntityAsync(entityId, "Add", amount);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await context.df.signalEntity(entityId, "add", 1);
};
```

*Signalisierung* bedeutet, dass der Entitäts-API-Aufruf unidirektional und asynchron ist. Einer *Clientfunktion* ist nicht bekannt, wann der Vorgang durch die Entität verarbeitet wurde. Außerdem kann die Clientfunktion keine Ergebniswerte oder Ausnahmen berücksichtigen. 

### <a name="example-client-reads-an-entity-state"></a>Beispiel: Client liest einen Entitätszustand

Clientfunktionen können auch den Zustand einer Entität abfragen, wie im folgenden Beispiel gezeigt:

```csharp
[FunctionName("QueryCounter")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function)] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");
    JObject state = await client.ReadEntityStateAsync<JObject>(entityId);
    return req.CreateResponse(HttpStatusCode.OK, state);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    return context.df.readEntityState(entityId);
};
```

Entitätszustandsabfragen werden an den dauerhaften Nachverfolgungsspeicher gesendet und geben den zuletzt *gespeicherten* Zustand der Entität zurück. Dieser Zustand ist immer ein committeter Zustand. Anders ausgedrückt: Hierbei handelt es sich niemals um einen temporären Übergangszustand während der Vorgangsausführung. Es kann jedoch sein, dass dieser Zustand im Vergleich zum In-Memory-Zustand der Entität veraltet ist. Der In-Memory-Zustand einer Entität kann nur von Orchestrierungen gelesen werden, wie im folgenden Abschnitt beschrieben.

### <a name="example-orchestration-signals-and-calls-an-entity"></a>Beispiel: Orchestrierung signalisiert eine Entität und ruft sie auf

Orchestratorfunktionen können unter Verwendung von APIs für die [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) auf Entitäten zugreifen. Der folgende Beispielcode zeigt eine Orchestratorfunktion mit *Aufruf* und *Signalisierung* einer Entität vom Typ *Counter*:

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // One-way signal to the entity which updates the value - does not await a response
        context.SignalEntity(entityId, "Add", 1);
    }
}
```

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    const entityId = new df.EntityId("Counter", "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    currentValue = yield context.df.callEntity(entityId, "get");
    if (currentValue < 10) {
        // One-way signal to the entity which updates the value - does not await a response
        yield context.df.signalEntity(entityId, "add", 1);
    }
});
```

Nur Orchestrierungen können Entitäten aufrufen und eine Antwort erhalten. Dabei kann es sich entweder um einen Rückgabewert oder um eine Ausnahme handeln. Clientfunktionen, von denen die [Clientbindung](durable-functions-bindings.md#entity-client) verwendet wird, können Entitäten nur *signalisieren*.

> [!NOTE]
> Das Aufrufen einer Entität über eine Orchestratorfunktion ist mit dem Aufrufen einer [Aktivitätsfunktion](durable-functions-types-features-overview.md#activity-functions) über eine Orchestratorfunktion vergleichbar. Der Hauptunterschied besteht darin, dass Entitätsfunktionen dauerhafte Objekte mit einer Adresse (*Entitäts-ID*) sind und die Angabe eines Vorgangsnamens unterstützen. Aktivitätsfunktionen sind dagegen zustandslos und verfügen über kein Vorgangskonzept.

### <a name="example-entity-signals-an-entity"></a>Beispiel: Entität signalisiert eine Entität

Eine Entitätsfunktion kann während der Vorgangsausführung Signale an andere Entitäten (und sogar an sich selbst) senden.
Das obige Beispiel mit der Counter-Entität kann beispielsweise so angepasst werden, dass ein entsprechendes Signal an eine Überwachungsentität gesendet wird, wenn der Zähler den Wert 100 erreicht:

```csharp
   case "add":
        var currentValue = ctx.GetState<int>();
        var amount = ctx.GetInput<int>();
        if (currentValue < 100 && currentValue + amount >= 100)
        {
            ctx.SignalEntity(new EntityId("MonitorEntity", ""), "milestone-reached", ctx.EntityKey);
        }

        ctx.SetState(currentValue + amount);
        break;
```

```javascript
    case "add":
        const amount = context.df.getInput();
        if (currentValue < 100 && currentValue + amount >= 100) {
            const entityId = new df.EntityId("MonitorEntity", "");
            context.df.signalEntity(entityId, "milestone-reached", context.df.instanceId);
        }
        context.df.setState(currentValue + amount);
        break;
```

## <a name="entity-coordination"></a>Koordination von Entitäten

Manchmal müssen Vorgänge über mehrere Entitäten hinweg koordiniert werden. Eine Bankinganwendung kann beispielsweise Entitäten enthalten, die einzelne Bankkonten darstellen. Wenn Sie Geld von einem Konto auf ein anderes überweisen, muss sichergestellt sein, dass auf dem _Quellkonto_ genügend Guthaben vorhanden ist und dass Aktualisierungen für das _Quellkonto_ und das _Zielkonto_ transaktionskonsistent erfolgen.

### <a name="example-transfer-funds-c"></a>Beispiel: Überweisung (C#)

Der folgende Beispielcode führt eine Überweisung zwischen zwei _Kontoentitäten_ unter Verwendung einer Orchestratorfunktion durch. Für die Koordinierung von Entitätsaktualisierungen muss mithilfe der Methode `LockAsync` ein _kritischer Abschnitt_ in der Orchestrierung erstellt werden:

> [!NOTE]
> Der Einfachheit halber wird in diesem Beispiel erneut die zuvor definierte Entität `Counter` verwendet. In einer echten Anwendung empfiehlt es sich allerdings, stattdessen eine ausführlichere Entität vom Typ `BankAccount` zu definieren.

```csharp
// This is a method called by an orchestrator function
public static async Task<bool> TransferFundsAsync(
    string sourceId,
    string destinationId,
    int transferAmount,
    IDurableOrchestrationContext context)
{
    var sourceEntity = new EntityId(nameof(Counter), sourceId);
    var destinationEntity = new EntityId(nameof(Counter), destinationId);

    // Create a critical section to avoid race conditions.
    // No operations can be performed on either the source or
    // destination accounts until the locks are released.
    using (await context.LockAsync(sourceEntity, destinationEntity))
    {
        ICounter sourceProxy = 
            context.CreateEntityProxy<ICounter>(sourceEntity);
        ICounter destinationProxy =
            context.CreateEntityProxy<ICounter>(destinationEntity);

        int sourceBalance = await sourceProxy.Get();

        if (sourceBalance >= transferAmount)
        {
            await sourceProxy.Add(-transferAmount);
            await destinationProxy.Add(transferAmount);

            // the transfer succeeded
            return true;
        }
        else
        {
            // the transfer failed due to insufficient funds
            return false;
        }
    }
}
```

In .NET gibt `LockAsync` ein Ergebnis vom Typ `IDisposable` zurück, das den kritischen Abschnitt beim Verwerfen beendet. Dieses Ergebnis vom Typ `IDisposable` kann zusammen mit einem Block vom Typ `using` verwendet werden, um eine syntaktische Darstellung des kritischen Abschnitts zu erhalten.

Im vorherigen Beispiel wurde mithilfe einer Orchestratorfunktion eine Überweisung von einer _Quellentität_ zu einer _Zielentität_ getätigt. Die Methode `LockAsync` hat sowohl die _Quellkontoentität_ als auch die _Zielkontoentität_ gesperrt. Diese Sperrung sorgt dafür, dass kein anderer Client den Zustand der Konten abfragen oder ändern kann, bis die Orchestrierungslogik den _kritischen Abschnitt_ am Ende der Anweisung `using` verlassen hat. Durch dieses Verhalten wird eine Überziehung des _Quellkontos_ verhindert.

> [!NOTE] 
> Wenn eine Orchestrierung beendet wird (entweder normal oder mit einem Fehler), werden alle kritischen Abschnitte, die noch aktiv sind, implizit beendet und alle Sperren aufgehoben.

### <a name="critical-section-behavior"></a>Verhalten des kritischen Abschnitts

Die Methode `LockAsync` erstellt einen _kritischen Abschnitt_ in einer Orchestrierung. Diese _kritischen Abschnitte_ sorgen dafür, dass andere Orchestrierungen keine überschneidenden Änderungen an einem angegebenen Satz von Entitäten vornehmen können. Intern sendet die API `LockAsync` Sperrvorgänge an die Entitäten und wird wieder verfügbar, wenn sie von diesen Entitäten jeweils eine Antwortnachricht vom Typ „Sperre abgerufen“ erhält. *Sperren* und *Entsperren* sind integrierte Vorgänge und werden von allen Entitäten unterstützt.

Für eine gesperrte Entität können keine Vorgänge von anderen Clients ausgeführt werden. Dadurch wird sichergestellt, dass immer nur eine Orchestrierungsinstanz eine Entität sperren kann. Wenn ein Aufrufer versucht, einen Vorgang für eine Entität aufzurufen, die durch eine Orchestrierung gesperrt wurde, wird der Vorgang in einer *Warteschlange für ausstehende Vorgänge* platziert. Ausstehende Vorgänge werden erst verarbeitet, nachdem die Sperre dieser Orchestrierung aufgehoben wurde.

> [!NOTE] 
> Dies unterscheidet sich geringfügig von den Synchronisierungsprimitiven, die in den meisten Programmiersprachen zum Einsatz kommen – etwa die Anweisung `lock` in C#. Die Anweisung `lock` muss in C# beispielsweise von allen Threads verwendet werden, um eine ordnungsgemäße threadübergreifende Synchronisierung sicherzustellen. Eine Entität muss dagegen nicht von allen Aufrufern explizit _gesperrt_ werden. Wenn ein Aufrufer eine Entität sperrt, werden alle anderen Vorgänge für diese Entität blockiert und in einer Warteschlange hinter dieser Sperre platziert.

Sperren für Entitäten sind dauerhaft und bleiben bestehen, auch wenn der ausführende Prozess recycelt wird. Sperren werden intern als Teil des dauerhaften Zustands einer Entität beibehalten.

Im Gegensatz zu Transaktionen wird bei kritischen Abschnitten im Falle von Fehlern kein automatisches Änderungsrollback durchgeführt. Stattdessen muss explizit eine Fehlerbehandlung (Rollback, Wiederholung oder Sonstiges) programmiert werden, um beispielsweise Fehler oder Ausnahmen abzufangen. Dies ist eine bewusste Entscheidung. Ein automatisches Rollback aller Auswirkungen einer Orchestrierung ist im Allgemeinen schwierig bis unmöglich, da Orchestrierungen unter Umständen Aktivitäten ausführen und externe Dienste aufrufen, für die kein Rollback ausgeführt werden kann. Darüber hinaus kann es vorkommen, dass auch Rollbackversuche nicht erfolgreich sind, was eine weitere Fehlerbehandlung erforderlich macht.

### <a name="critical-section-rules"></a>Regeln für kritische Abschnitte

Im Gegensatz zu Sperrprimitiven auf niedriger Ebene der meisten Programmiersprachen verfügen kritische Abschnitte über einen **garantierten Deadlockschutz**. Zur Verhinderung von Deadlocks werden folgende Einschränkungen erzwungen: 

* Kritische Abschnitte können nicht geschachtelt werden.
* Kritische Abschnitte können keine Unterorchestrierungen erstellen.
* Kritische Abschnitte können nur Entitäten aufrufen, die sie gesperrt haben.
* Kritische Abschnitte können nicht dieselbe Entität mit mehreren parallelen Aufrufen aufrufen.
* Kritische Abschnitte können nur Entitäten signalisieren, die sie nicht gesperrt haben.

Jeglicher Verstoß gegen diese Regeln hat einen Laufzeitfehler (beispielsweise `LockingRulesViolationException` in .NET) mit Angaben zum jeweiligen Regelverstoß zur Folge.

## <a name="comparison-with-virtual-actors"></a>Vergleich mit virtuellen Akteuren

Viele der Features dauerhafter Entitäten wurden durch das [Akteurmodell](https://en.wikipedia.org/wiki/Actor_model) inspiriert. Wenn Sie bereits mit Akteuren vertraut sind, kommen Ihnen wahrscheinlich viele der in diesem Artikel beschriebenen Konzepte bekannt vor. Dauerhafte Entitäten ähneln sehr stark [virtuellen Akteuren](https://research.microsoft.com/projects/orleans/) (oder *Grains*; siehe [Orleans-Projekt](http://dotnet.github.io/orleans/)). Beispiel:

* Dauerhafte Entitäten sind über eine *Entitäts-ID* adressierbar.
* Vorgänge mit dauerhaften Entitäten werden seriell ausgeführt, immer einzeln, um Racebedingungen zu verhindern.
* Dauerhafte Entitäten werden implizit erstellt, wenn sie aufgerufen oder signalisiert werden.
* Wenn keine Vorgänge ausgeführt werden, werden dauerhafte Entitäten automatisch aus dem Arbeitsspeicher entladen.

Es gibt jedoch einige wichtige Unterschiede, die beachtenswert sind:

* Dauerhafte Entitäten priorisieren *Dauerhaftigkeit* über *Latenz*, weshalb sie dann möglicherweise nicht für Anwendungen mit strengen Latenzanforderungen geeignet sind.
* Dauerhafte Entitäten verfügen über keine integrierten Timeouts für Nachrichten. In Orleans gilt für alle Nachrichten ein konfigurierbares Timeout (standardmäßig 30 Sekunden).
* Zwischen Entitäten gesendete Nachrichten werden zuverlässig und in Reihenfolge übermittelt. In Orleans wird eine zuverlässige oder geordnete Übermittlung für Inhalte unterstützt, die über Streams gesendet werden. Sie wird jedoch nicht für alle Nachrichten zwischen Grains garantiert.
* Anforderung/Antwort-Muster in Entitäten sind auf Orchestrierungen beschränkt. Innerhalb von Entitäten sind genau wie im ursprünglichen Akteurmodell (und im Gegensatz zu Grains in Orleans) nur unidirektionale Nachrichten zulässig (Stichwort: Signalisierung). 
* Bei dauerhaften Entitäten treten keine Deadlocks auf. In Orleans können Deadlocks auftreten (und sie werden erst dem Nachrichtentimeout aufgelöst).
* Dauerhafte Entitäten können in Verbindung mit dauerhaften Orchestrierungen verwendet werden und unterstützen verteilte Sperrmechanismen. 


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwicklerhandbuch für dauerhafte Entitäten in .NET (Vorschauversion)](durable-functions-dotnet-entities.md)

> [!div class="nextstepaction"]
> [Machen Sie sich mit Aufgabenhubs vertraut.](durable-functions-task-hubs.md)
