---
title: Dauerhafte Entitäten – Azure Functions
description: Hier erfahren Sie, was dauerhafte Entitäten sind und wie sie in der Durable Functions-Erweiterung für Azure Functions verwendet werden.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: 864a641968268c439c65996998cbb822746b96f9
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839000"
---
# <a name="entity-functions-preview"></a>Entitätsfunktionen (Vorschauversion)

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsteile, bekannt als *dauerhafte Entitäten*. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Im Gegensatz zu Orchestratorfunktionen besitzen Entitätsfunktionen keine spezifischen Codeeinschränkungen. Entitätsfunktionen verwalten Zustände auch explizit, statt Zustände implizit durch die Ablaufsteuerung darzustellen.

> [!NOTE]
> Entitätsfunktionen und zugehörige Funktionen sind nur in Durable Functions 2.0 und höher verfügbar. Entitätsfunktionen befinden sich momentan in der Public Preview-Phase.

## <a name="entity-identity"></a>Entitätsidentität

Der Zugriff auf Entitäten (die gelegentlich auch als *Entitätsinstanzen* bezeichnet werden) erfolgt über einen eindeutigen Bezeichner: die *Entitäts-ID*. Eine Entitäts-ID ist einfach ein Paar aus Zeichenfolgen, das eine Entitätsinstanz eindeutig identifiziert. Sie besteht aus:

* einem **Entitätsnamen**: Ein Name, der den Typ der Entität identifiziert (z. B. „Zähler“).
* einem **Entitätsschlüssel**: Eine Zeichenfolge, die die Entität unter allen anderen Entitäten desselben Namens eindeutig identifiziert (z. B. eine GUID).

Beispielsweise könnte eine *Counter*-Entitätsfunktion verwendet werden, um den Punktestand in einem Onlinespiel nachzuhalten. Jede Instanz des Spiels besitzt dann eine eindeutige Entitäts-ID, z. B. `@Counter@Game1`, `@Counter@Game2` usw. Bei allen Vorgängen, die auf eine bestimmte Entität ausgerichtet sind, muss eine Entitäts-ID als Parameter angegeben werden.

## <a name="programming-models"></a>Programmmodelle

Dauerhafte Entitäten unterstützen zwei verschiedene Programmiermodelle. Das erste Modell ist ein dynamisches „funktionales“ Modell, bei dem die Entität durch eine einzelne Funktion definiert wird. Das zweite Modell ist ein objektorientiertes Modell, bei dem die Entität durch eine Klasse und Methoden definiert wird. Diese Modelle sowie die Programmiermodelle für die Interaktion mit Entitäten werden in den folgenden Abschnitten beschrieben.

### <a name="defining-entities"></a>Definieren von Entitäten

Es gibt zwei optionale Programmiermodelle zum Erstellen dauerhafter Entitäten. Der folgende Code ist ein Beispiel für eine einfache *Counter*-Entität (Zähler), die als Standardfunktion implementiert ist. Diese Funktion definiert drei *Vorgänge*, `add`, `reset` und `get`, von denen jeder einen ganzzahligen Zustandswert `currentValue` verarbeitet.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += amount;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Dieses Modell eignet sich am besten für einfache Entitätsimplementierungen oder Implementierungen, die über einen dynamischen Satz von Vorgängen verfügen. Für statische Entitäten mit komplexeren Implementierungen kann auch ein klassenbasiertes Programmiermodell verwendet werden. Beim folgenden Beispiel handelt es sich um eine äquivalente Implementierung der Entität `Counter` unter Verwendung von Klassen und Methoden:

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

> [!NOTE]
> Die Methode für den Funktionseinstiegspunkt mit dem Attribut `[FunctionName]` *muss* bei Verwendung von Entitätsklassen als `static` deklariert werden. Nicht statische Einstiegspunktmethoden können zu einer Mehrfachinitialisierung von Objekten und möglicherweise zu anderen undefinierten Verhaltensweisen führen.

Im klassenbasierten Programmiermodell ist das Objekt `IDurableEntityContext` in der statischen Eigenschaft `Entity.Current` verfügbar.

Das klassenbasierte Modell ähnelt dem von [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/) bekannt gemachten Programmiermodell. In diesem Modell wird ein Entitätstyp als .NET-Klasse definiert. Jede Methode der Klasse ist ein Vorgang, der von einem externen Client aufgerufen werden kann. Anders als bei Orleans sind .NET-Schnittstellen jedoch optional. Im vorherigen *Counter*-Beispiel wurde keine Schnittstelle verwendet, aber sie kann immer noch über andere Funktionen oder mittels HTTP-API-Aufrufen aufgerufen werden.

> [!NOTE]
> Entitätstriggerfunktionen stehen ab Durable Functions 2.0 zur Verfügung. Aktuell sind Entitätstriggerfunktionen nur für .NET-Funktions-Apps verfügbar.

### <a name="accessing-entities-from-clients"></a>Zugriff auf Entitäten aus Clients

Dauerhafte Entitäten können über normale Funktionen (auch *Clientfunktionen* genannt) unter Verwendung der [Entitätsclient-Ausgabebindung](durable-functions-bindings.md#entity-client) aufgerufen oder abgefragt werden. Das folgende Beispiel zeigt eine durch eine Warteschlange ausgelöste Funktion zur *Signalisierung* einer Entität unter Verwendung dieser Bindung:

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

> [!NOTE]
> .NET-Funktionen unterstützen sowohl lose typisierte als auch typsichere Methoden zur Signalisierung von Entitäten. Ausführlichere Informationen finden Sie in der [Referenzdokumentation zur Entitätsclientbindung](durable-functions-bindings.md#entity-client-usage).

*Signalisierung* bedeutet, dass der Entitäts-API-Aufruf unidirektional und asynchron ist. Einer *Clientfunktion* ist nicht bekannt, wann die Entität den Vorgang verarbeitet hat, und eine Entitätsfunktion kann auch keinen Wert an eine Clientfunktion zurückgeben. Für dauerhafte Entitäten wurde bewusst ein unidirektionales warteschlangenbasiertes Messaging-Design gewählt, um der Dauerhaftigkeit eine höhere Priorität zu geben als der Leistung. Diese Designentscheidung ist einer der Kompromisse bei dauerhaften Entitäten im Vergleich zu ähnlichen Technologien. Aktuell können Rückgabewerte von Entitäten nur von Orchestrierungen behandelt werden. Dies wird im nächsten Abschnitt erläutert.

Clientfunktionen können auch den Zustand von Entitäten abfragen, wie im folgenden Beispiel gezeigt:

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

Entitätszustandsabfragen werden an den dauerhaften Nachverfolgungsspeicher gesendet und geben den zuletzt *gespeicherten* Zustand der Entität zurück. Es kann sein, dass der zurückgegebene Zustand im Vergleich zum In-Memory-Zustand der Entität bereits veraltet ist. Der In-Memory-Zustand einer Entität kann nur von Orchestrierungen gelesen werden, wie im folgenden Abschnitt beschrieben.

### <a name="accessing-entities-from-orchestrations"></a>Zugriff auf Entitäten aus Orchestrierungen

Orchestratorfunktionen können unter Verwendung von APIs für die [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) auf Entitäten zugreifen. Bei Orchestratorfunktionen kann zwischen unidirektionaler Kommunikation („Fire and Forget“, auch *Signalisierung* genannt) und bidirektionaler Kommunikation (Anforderung und Antwort, auch *Aufruf* genannt) gewählt werden. Der folgende Beispielcode zeigt eine Orchestratorfunktion mit *Aufruf* und *Signalisierung* einer Entität vom Typ *Counter*:

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Synchronous call to the entity which returns a value - will await a response
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // Asynchronous call which updates the value - will not await a response
        context.SignalEntity(entityId, "Add", 1);
    }
}
```

Nur Orchestrierungen können Entitäten aufrufen und eine Antwort erhalten. Dabei kann es sich entweder um einen Rückgabewert oder um eine Ausnahme handeln. Clientfunktionen, von denen die [Clientbindung](durable-functions-bindings.md#entity-client) verwendet wird, können Entitäten nur *signalisieren*.

> [!NOTE]
> Das Aufrufen einer Entität über eine Orchestratorfunktion ist mit dem Aufrufen einer [Aktivitätsfunktion](durable-functions-types-features-overview.md#activity-functions) über eine Orchestratorfunktion vergleichbar. Der Hauptunterschied besteht darin, dass Entitätsfunktionen dauerhafte Objekte mit einer Adresse (*Entitäts-ID*) sind und die Angabe eines Vorgangsnamens unterstützen. Aktivitätsfunktionen sind dagegen zustandslos und verfügen über kein Vorgangskonzept.

### <a name="dependency-injection-in-entity-classes-net"></a>Abhängigkeitsinjektion in Entitätsklassen (.NET)

Entitätsklassen unterstützen die [Abhängigkeitsinjektion in Azure Functions](../functions-dotnet-dependency-injection.md). Das folgende Beispiel zeigt die Registrierung eines Diensts vom Typ `IHttpClientFactory` bei einer klassenbasierten Entität:

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
        }
    }
}
```

Der folgende Codeausschnitt zeigt die Integration des eingefügten Diensts in Ihre Entitätsklasse:

```csharp
public class HttpEntity
{
    private readonly HttpClient client;

    public class HttpEntity(IHttpClientFactory factory)
    {
        this.client = factory.CreateClient();
    }

    public Task<int> GetAsync(string url)
    {
        using (var response = await this.client.GetAsync(url))
        {
            return (int)response.StatusCode;
        }
    }

    // The function entry point must be declared static
    [FunctionName(nameof(HttpEntity))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<HttpEntity>();
}
```

> [!NOTE]
> Anders als bei Verwendung der Konstruktorinjektion in regulären .NET-Azure-Funktionen *muss* die Funktionseinstiegspunkt-Methode für klassenbasierte Entitäten als `static` deklariert werden. Wenn Sie einen nicht statischen Funktionseinstiegspunkt deklarieren, kommt es unter Umständen zu Konflikten zwischen dem normalen Azure Functions-Objektinitialisierer und dem Durable Entities-Objektinitialisierer.

### <a name="bindings-in-entity-classes-net"></a>Bindungen in Entitätsklassen (.NET)

Im Gegensatz zu regulären Funktionen haben Entitätsklassenmethoden keinen direkten Zugriff auf Eingabe- und Ausgabebindungen. Stattdessen müssen Bindungsdaten in der Einstiegspunkt-Funktionsdeklaration erfasst und anschließend an die Methode `DispatchAsync<T>` übergeben werden. Alle an `DispatchAsync<T>` übergebenen Objekte werden automatisch als Argument an den Entitätsklassenkonstruktor übergeben.

Im folgenden Beispiel wird gezeigt, wie ein Verweis vom Typ `CloudBlobContainer` aus der [Blobeingabebindung](../functions-bindings-storage-blob.md#input) für eine klassenbasierte Entität verfügbar gemacht werden kann.

```csharp
public class BlobBackedEntity
{
    private readonly CloudBlobContainer container;

    public BlobBackedEntity(CloudBlobContainer container)
    {
        this.container = container;
    }

    // ... entity methods can use this.container in their implementations ...
    
    [FunctionName(nameof(BlobBackedEntity))]
    public static Task Run(
        [EntityTrigger] IDurableEntityContext context,
        [Blob("my-container", FileAccess.Read)] CloudBlobContainer container)
    {
        // passing the binding object as a parameter makes it available to the
        // entity class constructor
        return context.DispatchAsync<BlobBackedEntity>(container);
    }
}
```

Weitere Informationen zu Bindungen in Azure Functions finden Sie in der Dokumentation [Konzepte für Azure Functions-Trigger und -Bindungen](../functions-triggers-bindings.md).

## <a name="entity-coordination"></a>Koordination von Entitäten

Manchmal müssen Vorgänge über mehrere Entitäten hinweg koordiniert werden. Eine Bankinganwendung kann beispielsweise Entitäten enthalten, die einzelne Bankkonten darstellen. Wenn Sie Geld von einem Konto auf ein anderes überweisen, muss sichergestellt sein, dass auf dem _Quellkonto_ genügend Guthaben vorhanden ist und dass Aktualisierungen für das _Quellkonto_ und das _Zielkonto_ transaktionskonsistent erfolgen.

### <a name="transfer-funds-example-in-c"></a>Überweisungsbeispiel in C#

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

Im vorherigen Beispiel wurde mithilfe einer Orchestratorfunktion eine Überweisung von einer _Quellentität_ zu einer _Zielentität_ getätigt. Die Methode `LockAsync` hat sowohl die _Quellkontoentität_ als auch die _Zielkontoentität_ gesperrt. Diese Sperrung sorgt dafür, dass kein anderer Client den Zustand der Konten abfragen oder ändern kann, bis die Orchestrierungslogik den _kritischen Abschnitt_ am Ende der Anweisung `using` verlassen hat. Dadurch wurde effektiv eine Überziehung des _Quellkontos_ verhindert.

### <a name="critical-section-behavior"></a>Verhalten des kritischen Abschnitts

Die Methode `LockAsync` erstellt einen _kritischen Abschnitt_ in einer Orchestrierung. Diese _kritischen Abschnitte_ sorgen dafür, dass andere Orchestrierungen keine überschneidenden Änderungen an einem angegebenen Satz von Entitäten vornehmen können. Intern sendet die API `LockAsync` Sperrvorgänge an die Entitäten und wird wieder verfügbar, wenn sie von diesen Entitäten jeweils eine Antwortnachricht vom Typ „Sperre abgerufen“ erhält. *Sperren* und *Entsperren* sind integrierte Vorgänge und werden von allen Entitäten unterstützt.

Für eine gesperrte Entität können keine Vorgänge von anderen Clients ausgeführt werden. Dadurch wird sichergestellt, dass immer nur eine Orchestrierungsinstanz eine Entität sperren kann. Wenn ein Aufrufer versucht, einen Vorgang für eine Entität aufzurufen, die durch eine Orchestrierung gesperrt wurde, wird der Vorgang in einer *Warteschlange für ausstehende Vorgänge* platziert. Ausstehende Vorgänge werden erst verarbeitet, nachdem die Sperre dieser Orchestrierung aufgehoben wurde.

> [!NOTE] 
> Dies unterscheidet sich geringfügig von den Synchronisierungsprimitiven, die in den meisten Programmiersprachen zum Einsatz kommen – etwa die Anweisung `lock` in C#. Die Anweisung `lock` muss in C# beispielsweise von allen Threads verwendet werden, um eine ordnungsgemäße threadübergreifende Synchronisierung sicherzustellen. Eine Entität muss dagegen nicht von allen Aufrufern explizit _gesperrt_ werden. Wenn ein Aufrufer eine Entität sperrt, werden alle anderen Vorgänge für diese Entität blockiert und in einer Warteschlange hinter dieser Sperre platziert.

Sperren für Entitäten sind dauerhaft und bleiben bestehen, auch wenn der ausführende Prozess recycelt wird. Sperren werden intern als Teil des dauerhaften Zustands einer Entität beibehalten.

### <a name="critical-section-restrictions"></a>Einschränkungen für kritische Abschnitte

Wir setzen mehrere Einschränkungen durch, wie kritische Abschnitte verwendet werden können. Diese Einschränkungen dienen zum Verhindern von Deadlocks und Eintrittsinvarianz.

* Kritische Abschnitte können nicht geschachtelt werden.
* Kritische Abschnitte können keine Unterorchestrierungen erstellen.
* Kritische Abschnitte können nur Entitäten aufrufen, die sie gesperrt haben.
* Kritische Abschnitte können nicht dieselbe Entität mit mehreren parallelen Aufrufen aufrufen.
* Kritische Abschnitte können nur Entitäten signalisieren, die sie nicht gesperrt haben.

## <a name="comparison-with-virtual-actors"></a>Vergleich mit virtuellen Akteuren

Viele der Features dauerhafter Entitäten wurden durch das [Akteurmodell](https://en.wikipedia.org/wiki/Actor_model) inspiriert. Wenn Sie bereits mit Akteuren vertraut sind, kommen Ihnen wahrscheinlich viele der in diesem Artikel beschriebenen Konzepte bekannt vor. Insbesondere sind permanente Entitäten [virtuellen Akteuren](https://research.microsoft.com/projects/orleans/) in vielerlei Hinsicht sehr ähnlich:

* Dauerhafte Entitäten sind über eine *Entitäts-ID* adressierbar.
* Vorgänge mit dauerhaften Entitäten werden seriell ausgeführt, immer einzeln, um Racebedingungen zu verhindern.
* Dauerhafte Entitäten werden automatisch erstellt, wenn sie aufgerufen oder signalisiert werden.
* Wenn keine Vorgänge ausgeführt werden, werden dauerhafte Entitäten automatisch aus dem Arbeitsspeicher entladen.

Es gibt jedoch einige wichtige Unterschiede, die beachtenswert sind:

* Dauerhafte Entitäten priorisieren *Dauerhaftigkeit* über *Latenz*, weshalb sie dann möglicherweise nicht für Anwendungen mit strengen Latenzanforderungen geeignet sind.
* Zwischen Entitäten gesendete Nachrichten werden zuverlässig und in Reihenfolge übermittelt.
* Dauerhafte Entitäten können in Verbindung mit dauerhaften Orchestrierungen verwendet werden und unterstützen verteilte Sperrmechanismen.
* Anforderung/Antwort-Muster in Entitäten sind auf Orchestrierungen beschränkt. Bei der Kommunikation zwischen *Client und Entität* bzw. zwischen *Entität und Entität* sind genau wie im ursprünglichen Akteurmodell nur unidirektionale Nachrichten zulässig (Stichwort: Signalisierung). Dieses Verhalten verhindert verteilte Deadlocks.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Machen Sie sich mit Aufgabenhubs vertraut.](durable-functions-task-hubs.md)
