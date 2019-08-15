---
title: Durable Functions-Previewfunktionen – Azure Functions
description: Hier erhalten Sie Informationen Sie zu den Previewfunktionen für Durable Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: article
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 1609931cd5fcab0977ff64f680fbb1f253f3caaf
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782188"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Durable Functions 2.0 Preview (Azure Functions)

*Durable Functions* ist eine Erweiterung von [Azure Functions](../functions-overview.md) und [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Umgebung schreiben können. Die Erweiterung verwaltet Status, Prüfpunkte und Neustarts für Sie. Wenn Sie nicht bereits mit Durable Functions vertraut sind, lesen Sie die [Übersichtsdokumentation](durable-functions-overview.md).

Durable Functions 1.x ist ein GA-Feature (allgemeine Verfügbarkeit) von Azure Functions, enthält aber auch mehrere Unterfeatures, die derzeit als öffentliche Vorschauversion verfügbar sind. In diesem Artikel werden neu veröffentlichte Previewfunktionen beschrieben sowie Details dazu, wie sie funktionieren und wie Sie mit ihrer Verwendung beginnen können.

> [!NOTE]
> Diese Previewfunktionen sind Teil eines Durable Functions 2.0-Releases, bei dem es sich derzeit um ein **Vorschauqualitätsrelease** mit mehreren Breaking Changes handelt. Die Azure Functions Durable-Erweiterungspaketbuilds finden Sie auf „nuget.org“ mit Versionsangaben in der Form **2.0.0-betaX**. Diese Builds sind nicht für Produktionsworkloads gedacht, und Folgereleases könnten zusätzliche Breaking Changes enthalten.

## <a name="breaking-changes"></a>Wichtige Änderungen

In Durable Functions 2.0 werden mehrere Breaking Changes eingeführt. Bei vorhandenen Anwendungen ist davon auszugehen, dass sie nicht ohne Codeänderungen mit Durable Functions 2.0 kompatible sein werden. In diesem Abschnitt werden einige der Änderungen aufgeführt:

### <a name="hostjson-schema"></a>host.json-Schema

Der folgende Codeausschnitt zeigt das neue Schema für host.json. Die wichtigsten Änderungen, die Sie beachten sollten, sind die neuen Unterabschnitte:

* `"storageProvider"` (und der Unterabschnitt `"azureStorage"`) für die speicherspezifische Konfiguration
* `"tracking"` für die Konfiguration von Nachverfolgung und Protokollierung
* `"notifications"` (und der Unterabschnitt `"eventGrid"`) für die Konfiguration von Ereignisrasterbenachrichtigungen

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Mit der zunehmenden Stabilität von Durable Functions 2.0 werden weitere Änderungen am Abschnitt `durableTask` von host.json eingeführt. Weitere Informationen zu diesen Änderungen finden Sie in [diesem GitHub-Problem](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Änderungen an der öffentlichen Schnittstelle

Die verschiedenen „context“-Objekte, die von Durable Functions unterstützt werden, besaßen abstrakte Basisklassen, die für die Verwendung bei Komponententests gedacht waren. Als Bestandteil von Durable Functions 2.0 wurden diese abstrakten Basisklassen durch Schnittstellen ersetzt. Funktionscode, der die konkreten Typen direkt verwendet, ist davon nicht betroffen.

In der folgenden Tabelle werden die Hauptänderungen dargestellt:

| Alter Typ | Neuer Typ |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |

In dem Fall, wo eine abstrakte Basisklasse virtuelle Methoden enthielt, wurden diese virtuellen Methoden durch Erweiterungsmethoden ersetzt, die in `DurableContextExtensions` definiert sind.

## <a name="entity-functions"></a>Entitätsfunktionen

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsteile, bekannt als *dauerhafte Entitäten*. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Im Gegensatz zu Orchestratorfunktionen müssen Entitätsfunktionen keine spezifischen Codeeinschränkungen besitzen. Entitätsfunktionen verwalten Zustände auch explizit, statt Zustände implizit durch die Ablaufsteuerung darzustellen.

### <a name="net-programing-models"></a>.NET-Programmiermodelle

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
            currentValue += operand;
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

Dieses Modell eignet sich am besten für einfache Entitätsimplementierungen oder Implementierungen, die über einen dynamischen Satz von Vorgängen verfügen. Es gibt jedoch auch ein klassenbasiertes Programmiermodell, das für Entitäten nützlich ist, die statisch sind, aber komplexere Implementierungen aufweisen. Das folgende Beispiel ist eine äquivalente Implementierung der `Counter`-Entität unter Verwendung von .NET-Klassen und -Methoden.

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

Das klassenbasierte Modell ähnelt dem von [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/) bekannt gemachten Programmiermodell. In diesem Modell wird ein Entitätstyp als .NET-Klasse definiert. Jede Methode der Klasse ist ein Vorgang, der von einem externen Client aufgerufen werden kann. Anders als bei Orleans sind .NET-Schnittstellen jedoch optional. Im vorherigen *Counter*-Beispiel wurde keine Schnittstelle verwendet, aber sie kann immer noch über andere Funktionen oder mittels HTTP-API-Aufrufen aufgerufen werden.

Der Zugriff auf Entitäts*instanzen* erfolgt über einen eindeutigen Bezeichner, die *Entitäts-ID*. Eine Entitäts-ID ist einfach ein Paar aus Zeichenfolgen, das eine Entitätsinstanz eindeutig identifiziert. Sie besteht aus:

* einem **Entitätsnamen**: Ein Name, der den Typ der Entität identifiziert (z. B. „Zähler“).
* einem **Entitätsschlüssel**: Eine Zeichenfolge, die die Entität unter allen anderen Entitäten desselben Namens eindeutig identifiziert (z. B. eine GUID).

Beispielsweise könnte eine *Counter*-Entitätsfunktion verwendet werden, um den Punktestand in einem Onlinespiel nachzuhalten. Jede Instanz des Spiels besitzt dann eine eindeutige Entitäts-ID, z. B. `@Counter@Game1`, `@Counter@Game2` usw.

### <a name="comparison-with-virtual-actors"></a>Vergleich mit virtuellen Akteuren

Das Design der dauerhaften Entitäten ist stark vom [Akteur-Modell](https://en.wikipedia.org/wiki/Actor_model) beeinflusst. Wenn Sie mit Akteuren bereits vertraut sind, sollten Ihnen die Konzepte, die dauerhaften Entitäten zugrunde liegen, vertraut sein. Insbesondere sind permanente Entitäten [virtuellen Akteuren](https://research.microsoft.com/projects/orleans/) in vielerlei Hinsicht sehr ähnlich:

* Dauerhafte Entitäten sind über eine *Entitäts-ID* adressierbar.
* Vorgänge mit dauerhaften Entitäten werden seriell ausgeführt, immer einzeln, um Racebedingungen zu verhindern.
* Dauerhafte Entitäten werden automatisch erstellt, wenn sie aufgerufen oder signalisiert werden.
* Wenn keine Vorgänge ausgeführt werden, werden dauerhafte Entitäten automatisch aus dem Arbeitsspeicher entladen.

Es gibt jedoch einige wichtige Unterschiede, die beachtenswert sind:

* Dauerhafte Entitäten priorisieren *Dauerhaftigkeit* über *Latenz*, weshalb sie dann möglicherweise nicht für Anwendungen mit strengen Latenzanforderungen geeignet sind.
* Zwischen Entitäten gesendete Nachrichten werden zuverlässig und in Reihenfolge übermittelt.
* Dauerhafte Entitäten können in Verbindung mit dauerhaften Orchestrierungen verwendet werden und können als verteilte Sperren dienen, die später in diesem Artikel beschrieben werden.
* Anforderung/Antwort-Muster in Entitäten sind auf Orchestrierungen beschränkt. Bei der Kommunikation von Entität zu Entität sind nur unidirektionale Nachrichten zulässig (auch bekannt als „Signalisierung“), wie im ursprüngliche Akteur-Modell. Dieses Verhalten verhindert verteilte Deadlocks.

### <a name="durable-entity-net-apis"></a>.NET-APIs für dauerhafte Entitäten

Die Entitätsunterstützung umfasst mehrere APIs. Einmal gibt es eine neue API zum Definieren von Entitätsfunktionen, wie oben zu sehen, die angeben, was geschehen soll, wenn ein Vorgang für eine Entität aufgerufen wird. Ferner wurden vorhandene APIs für Clients und Orchestrierungen mit neuen Funktionen für die Interaktion mit Entitäten aktualisiert.

#### <a name="implementing-entity-operations"></a>Implementieren von Entitätsvorgängen

Die Ausführung eines Vorgangs für eine Entität kann diese Member für das Context-Objekt aufrufen (`IDurableEntityContext` in .NET):

* **OperationName**: Ruft den Namen des Vorgangs ab.
* **GetInput\<TInput>** : Ruft die Eingabe für den Vorgang ab.
* **GetState\<TState>** : Ruft den aktuelle Zustand der Entität ab.
* **SetState**: Aktualisiert den Zustand der Entität.
* **SignalEntity**: Sendet eine unidirektionale Nachricht an eine Entität.
* **Self**: Ruft die ID der Entität ab.
* **Return**: Gibt einen Wert an den Client oder die Orchestrierung zurück, der/die den Vorgang aufgerufen hat.
* **IsNewlyConstructed**: Gibt `true` zurück, wenn die Entität vor dem Vorgang noch nicht vorhanden war.
* **DestructOnExit**: Löscht die Entität nach Abschluss des Vorgangs.

Vorgänge sind weniger eingeschränkt als Orchestrierungen:

* Vorgänge können mithilfe von synchronen oder asynchronen APIs externe E/A aufrufen (wir empfehlen die ausschließliche Verwendung asynchroner APIs).
* Vorgänge können nichtdeterministisch sein. Beispielsweise ist es sicher, `DateTime.UtcNow`, `Guid.NewGuid()` oder `new Random()` aufzurufen.

#### <a name="accessing-entities-from-clients"></a>Zugriff auf Entitäten aus Clients

Dauerhafte Entitäten können von normalen Funktionen über die `orchestrationClient`-Bindung (`IDurableOrchestrationClient` in .NET) aufgerufen werden. Folgende Methoden werden unterstützt:

* **ReadEntityStateAsync\<T>** : Liest den Zustand einer Entität.
* **SignalEntityAsync**: Sendet eine unidirektional Nachricht an eine Entität und wartet darauf, dass diese in die Warteschlange eingereiht wird.
* **SignalEntityAsync\<T>** : identisch mit `SignalEntityAsync`, verwendet jedoch ein generiertes Proxy Objekt vom Typ `T`.

Der vorherige `SignalEntityAsync`-Aufruf erfordert die Angabe des Namens des Entitätsvorgangs als `string` sowie die Nutzlast des Vorgangs als `object`. Der folgende Beispielcode enthält ein Beispiel für dieses Muster:

```csharp
EntityId id = // ...
object amount = 5;
context.SignalEntityAsync(id, "Add", amount);
```

Es ist auch möglich, ein Proxyobjekt für den typsicheren Zugriff zu generieren. Zum Generieren eines typsicheren Proxys muss der Entitätstyp eine Schnittstelle implementieren. Nehmen wir beispielsweise an, dass die zuvor erwähnte `Counter`-Entität eine `ICounter`-Schnittstelle implementiert hat, die wie folgt definiert ist:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

Der Clientcode könnte dann `SignalEntityAsync<T>` verwenden und die `ICounter`-Schnittstelle als Typparameter angeben, um einen typsicheren Proxy zu generieren. Diese Verwendung von typsicheren Proxys wird im folgenden Codebeispiel veranschaulicht:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [OrchestrationClient] IDurableOrchestrationClient client)
{
    int amount = int.Parse(message);
    var target = new EntityId(nameof(Counter), "MyCounter");
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

Im vorherigen Beispiel ist der `proxy`-Parameter eine dynamisch generierte Instanz von `ICounter`, die den Aufruf von `Add` intern in den entsprechenden (nicht typisierten) Aufruf von `SignalEntityAsync` übersetzt.

Für den Parameter „type“ für `SignalEntityAsync<T>` gelten folgende Einschränkungen:

* Der type-Parameter muss eine Schnittstelle sein.
* Nur Methoden können in der Schnittstelle definiert werden. Eigenschaften werden nicht unterstützt.
* Jede Methode muss entweder einen oder keine Parameter definieren.
* Jede Methode muss entweder `void`,`Task` oder `Task<T>` zurückgeben, wobei `T` ein JSON-serialisierbarer Typ ist.
* Die Schnittstelle muss von genau einem Typ innerhalb der Assembly der Schnittstelle implementiert werden.

In den meisten Fällen führen Schnittstellen, die diese Anforderungen nicht erfüllen, zu einer Laufzeitausnahme.

> [!NOTE]
> Es ist wichtig, zu beachten, dass die Methoden `ReadEntityStateAsync` und `SignalEntityAsync` von `IDurableOrchestrationClient` die Leistung gegenüber der Konsistenz priorisieren. `ReadEntityStateAsync` kann einen veralteten Wert zurückgeben, und `SignalEntityAsync` kann zurückgegeben werden, bevor der Vorgang abgeschlossen ist.

#### <a name="accessing-entities-from-orchestrations"></a>Zugriff auf Entitäten aus Orchestrierungen

Orchestrierungen können mithilfe des `IDurableOrchestrationContext`-Objekts auf Entitäten zugreifen. Sie können zwischen unidirektionaler Kommunikation (auslösen und vergessen) und bidirektionaler Kommunikation (Anforderung und Antwort) wählen. Die entsprechenden Methoden sind:

* **SignalEntity**: Sendet eine unidirektionale Nachricht an eine Entität.
* **CallEntityAsync**: Sendet eine Nachricht an eine Entität und wartet auf eine Antwort, die anzeigt, dass der Vorgang abgeschlossen wurde.
* **CallEntityAsync\<T>** : Sendet eine Nachricht an eine Entität und wartet auf eine Antwort, die ein Ergebnis vom Typ „T“ enthält.

Wenn Sie bidirektionale Kommunikation verwenden, werden auch alle während der Ausführung des Vorgangs ausgelösten Ausnahmen zurück an die aufrufende Orchestrierung übertragen und erneut ausgelöst. Im Gegensatz dazu werden bei der Verwendung von „auslösen und vergessen“ Ausnahmen nicht berücksichtigt.

Für typsicheren Zugriff können Orchestrierungsfunktionen Proxys basierend auf einer Schnittstelle generieren. Die `CreateEntityProxy`-Erweiterungsmethode kann zu diesem Zweck verwendet werden:

```csharp
public interface IAsyncCounter
{
    Task AddAsync(int amount);
    Task ResetAsync();
    Task<int> GetAsync();
}

[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...
    IAsyncCounter proxy = context.CreateEntityProxy<IAsyncCounter>("MyCounter");
    await proxy.AddAsync(5);
    int newValue = await proxy.GetAsync();
    // ...
}
```

Im vorherigen Beispiel wurde angenommen, dass eine „Counter“-Entität vorhanden ist, die die `IAsyncCounter`-Schnittstelle implementiert. Die Orchestrierung konnte dann die `IAsyncCounter`-Typdefinition verwenden, um einen Proxytyp für die synchrone Interaktion mit der Entität zu generieren.

### <a name="locking-entities-from-orchestrations"></a>Sperren von Entitäten aus Orchestrierungen

Orchestrierungen können Entitäten sperren. Diese Funktion bietet eine einfache Möglichkeit, unerwünschte Racebedingungen durch Verwendung *kritischer Abschnitte* zu verhindern.

Das „Context“-Objekt bietet die folgenden Methoden:

* **LockAsync**: Richtet Sperren für eine oder mehrere Entitäten ein.
* **IsLocked**: Gibt „true“ zurück, wenn aktuell in einem kritischen Abschnitt, andernfalls „false“.

Der kritische Abschnitt endet, und alle Sperren werden freigegeben, wenn die Orchestrierung beendet wird. In .NET gibt `LockAsync` ein `IDisposable` zurück, das den kritischen Abschnitt beim Verwerfen beendet, was zusammen mit einer `using`-Klausel verwendet werden kann, um eine syntaktische Darstellung des kritischen Abschnitts zu erhalten.

Nehmen Sie beispielsweise eine Orchestrierung an, die testen muss, ob zwei Spieler zur Verfügung stehen, und dann beide zu einem Spiel zuweisen muss. Diese Aufgabe kann mit einem kritischen Abschnitt wie folgt implementiert werden:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

Innerhalb des kritischen Abschnitts sind beide Player-Entitäten (Spieler) gesperrt, was bedeutet, dass sie keine anderen Vorgänge ausführen, als die, die aus dem kritischen Abschnitt aufgerufen werden. Dieses Verhalten verhindert Racebedingungen mit in Konflikt stehenden Vorgänge, z. B. Spieler, die einem anderen Spiel zugewiesen werden, oder sich abmeldende Spieler.

Wir setzen mehrere Einschränkungen durch, wie kritische Abschnitte verwendet werden können. Diese Einschränkungen dienen zum Verhindern von Deadlocks und Eintrittsinvarianz.

* Kritische Abschnitte können nicht geschachtelt werden.
* Kritische Abschnitte können keine Unterorchestrierungen erstellen.
* Kritische Abschnitte können nur Entitäten aufrufen, die sie gesperrt haben.
* Kritische Abschnitte können nicht dieselbe Entität mit mehreren parallelen Aufrufen aufrufen.
* Kritische Abschnitte können nur Entitäten signalisieren, die sie nicht gesperrt haben.

## <a name="alternate-storage-providers"></a>Alternativer Speicheranbieter

Das Durable Task Framework unterstützt heutzutage mehrere Speicheranbieter, einschließlich [Azure Storage](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), [In-Memory-Emulator](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator) sowie einen experimentellen [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis)-Anbieter. Bisher hat die Durable Task-Erweiterung für Azure Functions jedoch nur den Azure Storage-Anbieter unterstützt. Ab Durable Functions 2.0 wird Unterstützung für alternative Speicheranbieter hinzugefügt, beginnend mit dem Redis-Anbieter.

> [!NOTE]
> Durable Functions 2.0 unterstützt nur .NET Standard 2.0-kompatible Anbieter. Zum Zeitpunkt der Erstellung dieses Artikels unterstützt der Azure Service Bus-Anbieter kein .NET Standard 2.0 und ist deshalb nicht als alternativer Speicheranbieter verfügbar.

### <a name="emulator"></a>Emulator

Der [DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/)-Anbieter ist ein Anbieter im lokalen Speicher für nicht dauerhaften Speicher, geeignet für lokale Testszenarios. Er kann mithilfe des folgenden, minimalen **host.json**-Schemas konfiguriert werden:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (experimentell)

Der [DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/)-Anbieter legt den gesamten Orchestrierungszustand permanent in einem konfigurierten Redis-Cluster ab.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

Der `connectionStringName` muss auf den Namen einer App-Einstellung oder einer Umgebungsvariablen verweisen. Diese App-Einstellung oder Umgebungsvariable sollte einen Redis-Verbindungszeichenfolgenwert in Form von enthalten *Server:Port* enthalten. Z. B. `localhost:6379` zum Herstellen einer Verbindung mit einem lokalen Redis-Cluster.

> [!NOTE]
> Der Redis-Anbieter ist derzeit experimentell und unterstützt nur Funktions-Apps, die auf einem einzelnen Knoten ausgeführt wird. Es ist nicht garantiert, dass der Redis-Anbieter jemals allgemein verfügbar wird, und er kann in einer zukünftigen Version auch entfernt werden.
