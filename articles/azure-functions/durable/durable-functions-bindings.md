---
title: Bindungen für Durable Functions – Azure
description: Erfahren Sie, wie Sie Trigger und Bindungen für die Durable Functions-Erweiterung für Azure Functions verwenden.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: f297c89d2c3ba5692a44fab631c0d46c75f48692
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71033580"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Bindungen für Durable Functions (Azure Functions)

Mit der Erweiterung [Durable Functions](durable-functions-overview.md) (Langlebige Funktionen) werden zwei neue Triggerbindungen eingeführt, mit denen die Ausführung von Orchestrator- und Aktivitätsfunktionen gesteuert wird. Außerdem wird eine Ausgabebindung eingeführt, die als Client für die Laufzeit von Durable Functions dient.

## <a name="orchestration-trigger"></a>Orchestrierungstrigger

Mit einem Orchestrierungstrigger können Sie [langlebige Orchestratorfunktionen](durable-functions-types-features-overview.md#orchestrator-functions) erstellen. Dieser Trigger unterstützt das Starten von neuen Orchestratorfunktionsinstanzen und das Fortsetzen von vorhandenen Orchestratorfunktionsinstanzen, die auf eine Aufgabe „warten“.

Wenn Sie die Visual Studio-Tools für Azure Functions verwenden, wird der Orchestrierungstrigger mit dem .NET-Attribut [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) konfiguriert.

Wenn Sie Orchestratorfunktionen in Skriptsprachen schreiben (z. B. in JavaScript- oder C#-Skripts), wird der Orchestrierungstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` ist der Name der Orchestrierung. Dies ist der Wert, den Clients verwenden müssen, wenn sie neue Instanzen dieser Orchestratorfunktion starten möchten. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird der Name der Funktion verwendet.

Intern fragt diese Triggerbindung eine Reihe von Warteschlangen im Standardspeicherkonto für die Funktionen-App ab. Da es sich bei diesen Warteschlangen um interne Implementierungsdetails der Erweiterung handelt, werden sie in den Bindungseigenschaften nicht explizit konfiguriert.

### <a name="trigger-behavior"></a>Triggerverhalten

Hier sind einige Hinweise zum Orchestrierungstrigger angegeben:

* **Single-Threading**: Ein einzelner Verteilerthread wird für die gesamte Ausführung von Orchestratorfunktionen auf einer einzelnen Hostinstanz verwendet. Daher sollten Sie unbedingt sicherstellen, dass der Orchestratorfunktionscode effizient ist und keine E/A-Vorgänge durchführt. Außerdem sollten Sie sicherstellen, dass dieser Thread nur dann asynchrone Arbeitsschritte ausführt, wenn auf Aufgabentypen gewartet wird, die sich auf Durable Functions beziehen.
* **Behandlung von nicht verarbeitbaren Nachrichten:** Orchestrierungstrigger weisen keine Unterstützung für nicht verarbeitbare Nachrichten auf.
* **Sichtbarkeit von Nachrichten**: Nachrichten des Orchestrierungstriggers werden aus der Warteschlange entfernt und bleiben für eine konfigurierbare Dauer unsichtbar. Die Sichtbarkeit dieser Nachrichten wird automatisch erneuert, solange die Funktionen-App ausgeführt wird und fehlerfrei ist.
* **Rückgabewerte**: Rückgabewerte werden in JSON-Code serialisiert und in der Tabelle mit dem Orchestrierungsverlauf im Azure-Tabellenspeicher beibehalten. Diese Rückgabewerte können von der Orchestrierungsclientbindung abgefragt werden, die weiter unten beschrieben wird.

> [!WARNING]
> Für Orchestratorfunktionen sollten niemals andere Eingabe- oder Ausgabebindungen als die Orchestrierungstriggerbindung verwendet werden. Wenn dies nicht beachtet wird, können sich unter Umständen Probleme mit der Erweiterung für langlebige Aufgaben ergeben, da sich diese Bindungen ggf. nicht an die Regeln für Single-Threading und E/A-Abläufe halten. Wenn Sie andere Bindungen verwenden möchten, fügen Sie sie einer Aktivitätsfunktion hinzu, die von der Orchestratorfunktion aufgerufen wird.

> [!WARNING]
> JavaScript-Orchestratorfunktionen sollten niemals als `async` deklariert werden.

### <a name="trigger-usage-net"></a>Verwendung von Triggern (.NET)

Die Orchestrierungstriggerbindung unterstützt sowohl Ein- als auch Ausgaben. Hier sind einige Dinge aufgeführt, die Sie in Bezug auf die Eingabe- und Ausgabeverarbeitung wissen sollten:

* **Eingaben**: .NET-Orchestrierungsfunktionen unterstützen nur [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) als Parametertyp. Deserialisierung von Eingaben direkt in die Funktionssignatur wird nicht unterstützt. Im Code muss die [GetInput\<T>](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)-Methode (.NET) oder `getInput`-Methode (JavaScript) zum Abrufen von Eingaben der Orchestratorfunktion verwendet werden. Bei diesen Eingaben muss es sich um Typen handeln, für die eine JSON-Serialisierung möglich ist.
* **Ausgaben**: Orchestrierungstrigger unterstützen sowohl Ausgabewerte als auch Eingaben. Der Rückgabewert der Funktion wird verwendet, um den Ausgabewert zuzuweisen, und er muss per JSON serialisierbar sein. Wenn eine .NET-Funktion `Task` oder `void` zurückgibt, wird als Ausgabe der Wert `null` gespeichert.

### <a name="trigger-sample"></a>Triggerbeispiel

Der folgende Beispielcode zeigt, wie eine sehr einfache Orchestratorfunktion vom Typ „Hello World“ aussehen könnte:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> Das `context`-Objekt in JavaScript stellt nicht den „DurableOrchestrationContext“ dar, sondern den [Funktionskontext als Ganzes](../functions-reference-node.md#context-object). Sie können über die `df`-Eigenschaft des `context`-Objekts auf Orchestrierungsmethoden zugreifen.

> [!NOTE]
> JavaScript-Orchestratoren sollten `return` verwenden. Die Bibliothek `durable-functions` übernimmt den Aufruf der `context.done`-Methode.

Die meisten Orchestratorfunktionen rufen Aktivitätsfunktionen auf. In diesem „Hello World“-Beispiel wird veranschaulicht, wie eine Aktivitätsfunktion aufgerufen wird:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-trigger"></a>Aktivitätstrigger

Mit dem Aktivitätstrigger können Sie Funktionen erstellen, die von Orchestratorfunktionen aufgerufen werden, die als [Aktivitätsfunktionen](durable-functions-types-features-overview.md#activity-functions) bezeichnet werden.

Wenn Sie Visual Studio verwenden, wird der Aktivitätstrigger mit dem .NET-Attribut [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) konfiguriert.

Wenn Sie VS Code oder das Azure-Portal zum Entwickeln verwenden, wird der Aktivitätstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` ist der Name der Aktivität. Dieser Wert ist der Name, der von Orchestratorfunktionen verwendet wird, um diese Aktivitätsfunktion aufzurufen. Diese Eigenschaft ist optional. Wenn sie nicht angegeben ist, wird der Name der Funktion verwendet.

Intern fragt diese Triggerbindung eine Warteschlange im Standardspeicherkonto für die Funktionen-App ab. Da es sich bei dieser Warteschlange um ein internes Implementierungsdetail der Erweiterung handelt, wird sie in den Bindungseigenschaften nicht explizit konfiguriert.

### <a name="trigger-behavior"></a>Triggerverhalten

Hier sind einige Hinweise zum Aktivitätstrigger aufgeführt:

* **Threading**: Im Gegensatz zum Orchestrierungstrigger gelten für Aktivitätstrigger keine Einschränkungen in Bezug auf das Threading oder den E/A-Ablauf. Sie können wie reguläre Funktionen behandelt werden.
* **Behandlung von nicht verarbeitbaren Nachrichten**: Aktivitätstrigger weisen keine Unterstützung für nicht verarbeitbare Nachrichten auf.
* **Sichtbarkeit von Nachrichten**: Nachrichten des Aktivitätstriggers werden aus der Warteschlange entfernt und bleiben für eine konfigurierbare Dauer unsichtbar. Die Sichtbarkeit dieser Nachrichten wird automatisch erneuert, solange die Funktionen-App ausgeführt wird und fehlerfrei ist.
* **Rückgabewerte**: Rückgabewerte werden in JSON-Code serialisiert und in der Tabelle mit dem Orchestrierungsverlauf im Azure-Tabellenspeicher beibehalten.

> [!WARNING]
> Das Speicher-Back-End für Aktivitätsfunktionen ist ein Implementierungsdetail, und Benutzercode sollte mit diesen Speicherentitäten nicht direkt interagieren.

### <a name="trigger-usage-net"></a>Verwendung von Triggern (.NET)

Die Aktivitätstriggerbindung unterstützt sowohl Ein- als auch Ausgaben – genau wie der Orchestrierungstrigger. Hier sind einige Dinge aufgeführt, die Sie in Bezug auf die Eingabe- und Ausgabeverarbeitung wissen sollten:

* **Eingaben**: .NET-Aktivitätsfunktionen verwenden nativ [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als Parametertyp. Alternativ dazu kann eine Aktivitätsfunktion mit jedem Parametertyp deklariert werden, der per JSON serialisierbar ist. Bei Verwendung von `DurableActivityContext` können Sie [GetInput\<T>](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) aufrufen, um die Eingabe der Aktivitätsfunktion abzurufen und zu deserialisieren.
* **Ausgaben**: Aktivitätsfunktionen unterstützen sowohl Ausgabewerte als auch Eingaben. Der Rückgabewert der Funktion wird verwendet, um den Ausgabewert zuzuweisen, und er muss per JSON serialisierbar sein. Wenn eine .NET-Funktion `Task` oder `void` zurückgibt, wird als Ausgabe der Wert `null` gespeichert.
* **Metadaten**: .NET-Aktivitätsfunktionen können an einen `string instanceId`-Parameter gebunden werden, um die Instanz-ID der übergeordneten Orchestrierung abzurufen.

### <a name="trigger-sample"></a>Triggerbeispiel

Der folgende Beispielcode zeigt, wie eine sehr einfache Aktivitätsfunktion vom Typ „Hello World“ aussehen könnte:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

Der Standardparametertyp für die .NET-Bindung `ActivityTriggerAttribute` lautet `DurableActivityContext`. .NET-Aktivitätstrigger unterstützen aber auch die direkte Bindung an JSON-serialisierbare Typen (z. B. primitive Typen). Die gleiche Funktion kann daher wie folgt vereinfacht werden:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

```javascript
module.exports = async function(context) {
    return `Hello ${context.bindings.name}!`;
};
```

JavaScript-Bindungen können auch als zusätzliche Parameter übergeben werden. Die gleiche Funktion kann daher wie folgt vereinfacht werden:

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```


### <a name="using-input-and-output-bindings"></a>Verwenden von Eingabe- und Ausgabebindungen

Sie können neben der Aktivitätstriggerbindung auch reguläre Eingabe- und Ausgabebindungen verwenden. Beispielsweise können Sie die Eingabe in die Aktivitätsbindung verwenden und eine Nachricht mithilfe der EventHub-Ausgabebindung an einen EventHub senden:

```json
{
  "bindings": [
    {
      "name": "message",
      "type": "activityTrigger",
      "direction": "in"
    },
    {
      "type": "eventHub",
      "name": "outputEventHubMessage",
      "connection": "EventhubConnectionSetting",
      "eventHubName": "eh_messages",
      "direction": "out"
  }
  ]
}
```

```javascript
module.exports = async function (context) {
    context.bindings.outputEventHubMessage = context.bindings.message;
};
```

## <a name="orchestration-client"></a>Orchestrierungsclient

Mit der Bindung des Orchestrierungsclients können Sie Funktionen schreiben, die mit Orchestratorfunktionen interagieren. Diese Funktionen werden manchmal auch als [Clientfunktionen](durable-functions-types-features-overview.md#client-functions)bezeichnet. Beispielsweise können Sie für Orchestrierungsinstanzen folgende Aktionen durchführen:

* Starten
* Abfragen des Status
* Beenden
* Senden von Ereignissen an die Instanzen während der Ausführung
* Löschen des Instanzverlaufs

Bei Verwendung von Visual Studio können Sie die Bindung an den Orchestrierungsclient durchführen, indem Sie das .NET-Attribut [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) für Durable Functions 1.0 verwenden. Ab der Vorschauversion Durable Functions 2.0 können Sie mithilfe des .NET-Attributs `DurableClientAttribute` eine Bindung mit dem Orchestrierungsclient herstellen.

Wenn Sie Skriptsprachen (z.B. *CSX*- oder *JS*-Dateien) für die Entwicklung nutzen, wird der Orchestrierungstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub`: Wird in Szenarien verwendet, in denen mehrere Funktionen-Apps dasselbe Speicherkonto nutzen, aber voneinander isoliert werden müssen. Wenn Sie hier nichts angeben, wird der Standardwert aus `host.json` verwendet. Dieser Wert muss mit dem Wert übereinstimmen, der von den Zielorchestratorfunktionen verwendet wird.
* `connectionName`: Der Name einer App-Einstellung, die eine Speicherkonto-Verbindungszeichenfolge enthält. Das Speicherkonto, für das diese Verbindungszeichenfolge steht, muss dem Speicherkonto entsprechen, das von den Zielorchestratorfunktionen verwendet wird. Wenn nichts angegeben ist, wird die Speicherkonto-Standardverbindungszeichenfolge für die Funktionen-App verwendet.

> [!NOTE]
> In den meisten Fällen ist es empfehlenswert, diese Eigenschaften wegzulassen und das Standardverhalten zu nutzen.

### <a name="client-usage"></a>Clientnutzung

In .NET-Funktionen richten Sie normalerweise eine Bindung an `DurableOrchestrationClient` ein, sodass Sie Vollzugriff auf alle Client-APIs erhalten, die von Durable Functions unterstützt werden. Ab Durable Functions 2.0 stellen Sie stattdessen eine Bindung mit der `IDurableOrchestrationClient`-Schnittstelle her. In JavaScript werden die gleichen APIs vom Objekt zur Verfügung gestellt, das von `getClient` zurückgegeben wird. Zu den APIs für das Clientobjekt gehören:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_)
* [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_)
* [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_)

Alternativ können .NET-Funktionen an `IAsyncCollector<T>` gebunden werden, wobei `T` für [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) oder `JObject` steht.

Weitere Informationen zu diesen Vorgängen finden Sie in der API-Dokumentation zu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html).

### <a name="client-sample-visual-studio-development"></a>Clientbeispiel (Visual Studio-Entwicklung)

Hier ist ein Beispiel für eine per Warteschlange ausgelöste Funktion angegeben, mit der eine „HelloWorld“-Orchestrierung gestartet wird.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>Clientbeispiel (nicht Visual Studio)

Wenn Sie für die Entwicklung nicht Visual Studio nutzen, können Sie die folgende *function.json*-Datei erstellen. In diesem Beispiel wird veranschaulicht, wie Sie eine per Warteschlange ausgelöste Funktion konfigurieren, für die die langlebige Orchestrierungsclientbindung verwendet wird:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

Im Folgenden sind sprachspezifische Beispiele angegeben, in denen neue Instanzen von Orchestratorfunktionen gestartet werden.

#### <a name="c-sample"></a>C#-Beispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie die langlebige Orchestrierungsclientbindung nutzen, um eine neue Funktionsinstanz über eine C#-Skriptfunktion zu starten:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>JavaScript-Beispiel

Im folgenden Beispiel wird veranschaulicht, wie Sie die langlebige Orchestrierungsclientbindung nutzen, um eine neue Funktionsinstanz über eine JavaScript-Funktion zu starten:

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    return instanceId = await client.startNew("HelloWorld", undefined, context.bindings.input);
};
```

Weitere Informationen zum Starten von Instanzen finden Sie unter [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="entity-trigger"></a>Entitätstrigger

Entitätstrigger ermöglichen es Ihnen, [Entitätsfunktionen](durable-functions-entities.md) zu erstellen. Dieser Trigger unterstützt die Verarbeitung von Ereignissen für eine bestimmte Entitätsinstanz.

Wenn Sie die Visual Studio-Tools für Azure Functions verwenden, wird der Entitätstrigger mit dem .NET-Attribut `EntityTriggerAttribute` konfiguriert.

> [!NOTE]
> Entitätstrigger sind in Durable Functions 2.0 und höher verfügbar. Entitätstrigger sind für JavaScript noch nicht verfügbar.

Intern fragt diese Triggerbindung eine Reihe von Warteschlangen im Standardspeicherkonto für die Funktionen-App ab. Da es sich bei diesen Warteschlangen um interne Implementierungsdetails der Erweiterung handelt, werden sie in den Bindungseigenschaften nicht explizit konfiguriert.

### <a name="trigger-behavior"></a>Triggerverhalten

Hier sind einige Hinweise zum Entitätstrigger aufgeführt:

* **Singlethread**: Ein einzelner Verteilerthread wird zum Verarbeiten von Vorgängen für eine bestimmte Entität verwendet. Wenn mehrere Nachrichten gleichzeitig an eine einzelne Entität gesendet werden, werden die Vorgänge einzeln verarbeitet.
* **Behandlung von nicht verarbeitbaren Nachrichten**: Entitätstrigger weisen keine Unterstützung für nicht verarbeitbare Nachrichten auf.
* **Sichtbarkeit von Nachrichten**: Nachrichten des Entitätstriggers werden aus der Warteschlange entfernt und bleiben für eine konfigurierbare Dauer unsichtbar. Die Sichtbarkeit dieser Nachrichten wird automatisch erneuert, solange die Funktionen-App ausgeführt wird und fehlerfrei ist.
* **Rückgabewerte**: Entitätsfunktionen unterstützen keine Rückgabewerte. Es gibt bestimmte APIs, die zum Speichern des Zustands oder zum Zurückgeben von Werten an Orchestrierungen verwendet werden können.

Alle Zustandsänderungen, die während der Ausführung an einer Entität vorgenommen werden, werden nach Abschluss der Ausführung automatisch persistent gespeichert.

### <a name="trigger-usage-net"></a>Verwendung von Triggern (.NET)

Jede Entitätsfunktion weist den Parametertyp `IDurableEntityContext` auf, der über die folgenden Member verfügt:

* **EntityName**: Ruft den Namen der derzeit ausgeführten Entität ab.
* **EntityKey**: Ruft den Schlüssel der derzeit ausgeführten Entität ab.
* **EntityId**: Ruft die ID der derzeit ausgeführten Entität ab.
* **OperationName**: Ruft den Namen des aktuellen Vorgangs ab.
* **IsNewlyConstructed**: Gibt `true` zurück, wenn die Entität vor dem Vorgang noch nicht vorhanden war.
* **GetState\<TState>()** : Ruft den aktuellen Zustand der Entität ab. Der `TState`-Parameter muss ein primitiver oder JSON-serialisierbarer Typ sein.
* **SetState(object)** : Aktualisiert den Zustand der Entität. Der `object`-Parameter muss ein primitives oder JSON-serialisierbares Objekt sein.
* **GetInput\<TInput>()** : Ruft die Eingabe für den aktuellen Vorgang ab. Der `TInput`-Typparameter muss einen primitiven oder JSON-serialisierbaren Typ darstellen.
* **Return(object)** : Gibt einen Wert an die Orchestrierung zurück, die den Vorgang aufgerufen hat. Der `object`-Parameter muss ein primitives oder JSON-serialisierbares Objekt sein.
* **DestructOnExit()** : Löscht die Entität nach Abschluss des aktuellen Vorgangs.
* **SignalEntity(EntityId, string, object)** : Sendet eine unidirektionale Nachricht an eine Entität. Der `object`-Parameter muss ein primitives oder JSON-serialisierbares Objekt sein.

Wenn Sie den klassenbasierten Entitätsprogrammiermodus verwenden, kann auf das `IDurableEntityContext`-Objekt mithilfe der threadstatischen Eigenschaft `Entity.Current` verwiesen werden.

### <a name="trigger-sample---entity-function"></a>Triggerbeispiel: Entitätsfunktion

Der folgende Code ist ein Beispiel für eine einfache *Counter*-Entität (Zähler), die als Standardfunktion implementiert ist. Diese Funktion definiert drei *Vorgänge*, `add`, `reset` und `get`, von denen jeder einen ganzzahligen Zustandswert `currentValue` verarbeitet.

```csharp
[FunctionName(nameof(Counter))]
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

### <a name="trigger-sample---entity-class"></a>Triggerbeispiel: Entitätsklasse

Beim folgenden Beispiel handelt es sich um eine äquivalente Implementierung der vorherigen `Counter`-Entität unter Verwendung von .NET-Klassen und -Methoden.

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
> Die Methode für den Funktionseinstiegspunkt mit dem `[FunctionName]`-Attribut *muss* bei Verwendung von Entitätsklassen als `static` deklariert werden. Nicht statische Einstiegspunktmethoden können zu einer Mehrfachinitialisierung von Objekten und möglicherweise zu anderen undefinierten Verhaltensweisen führen.

Entitätsklassen verfügen über spezielle Mechanismen für die Interaktion mit Bindungen und .NET-Abhängigkeitsinjektion (Dependency Injection). Weitere Informationen finden Sie im Artikel [Permanente Entitäten](durable-functions-entities.md).

## <a name="entity-client"></a>Entitätsclient

Die Entitätsclientbindung ermöglicht es Ihnen, [Entitätsfunktionen](#entity-trigger) asynchron auszulösen. Diese Funktionen werden manchmal auch als [Clientfunktionen](durable-functions-types-features-overview.md#client-functions)bezeichnet.

Bei Verwendung von Visual Studio können Sie die Bindung an den Entitätsclient vornehmen, indem Sie das .NET-Attribut `DurableClientAttribute` verwenden.

> [!NOTE]
> `[DurableClientAttribute]` kann auch verwendet werden, um eine Bindung mit dem [Orchestrierungsclient](#orchestration-client) herzustellen.

Wenn Sie Skriptsprachen (z.B. *CSX*- oder *JS*-Dateien) für die Entwicklung nutzen, wird der Entitätstrigger mit dem folgenden JSON-Objekt im `bindings`-Array der Datei *function.json* definiert:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "durableClient",
    "direction": "out"
}
```

* `taskHub`: Wird in Szenarien verwendet, in denen mehrere Funktionen-Apps dasselbe Speicherkonto nutzen, aber voneinander isoliert werden müssen. Wenn Sie hier nichts angeben, wird der Standardwert aus `host.json` verwendet. Dieser Wert muss mit dem Wert übereinstimmen, der von den Zielentitätsfunktionen verwendet wird.
* `connectionName`: Der Name einer App-Einstellung, die eine Speicherkonto-Verbindungszeichenfolge enthält. Das Speicherkonto, für das diese Verbindungszeichenfolge steht, muss dem Speicherkonto entsprechen, das von den Zielentitätsfunktionen verwendet wird. Wenn nichts angegeben ist, wird die Speicherkonto-Standardverbindungszeichenfolge für die Funktionen-App verwendet.

> [!NOTE]
> In den meisten Fällen ist es empfehlenswert, die optionalen Eigenschaften nicht zu verwenden und das Standardverhalten zu nutzen.

### <a name="entity-client-usage"></a>Syntax des Entitätsclients

In .NET-Funktionen richten Sie normalerweise eine Bindung mit `IDurableEntityClient` ein, sodass Sie Vollzugriff auf alle Client-APIs erhalten, die von Durable Entities unterstützt werden. Sie können auch eine Bindung mit der `IDurableClient`-Schnittstelle herstellen, die den Zugriff auf Client-APIs für Entitäten und Orchestrierungen ermöglicht. Zu den APIs für das Clientobjekt gehören:

* **ReadEntityStateAsync\<T>** : Liest den Zustand einer Entität.
* **SignalEntityAsync**: Sendet eine unidirektional Nachricht an eine Entität und wartet darauf, dass diese in die Warteschlange eingereiht wird.
* **SignalEntityAsync\<TEntityInterface>** : Identisch mit `SignalEntityAsync`, verwendet jedoch ein generiertes Proxyobjekt vom Typ `TEntityInterface`.
* **CreateEntityProxy\<TEntityInterface>** : Erstellt dynamisch einen dynamischen Proxy vom Typ `TEntityInterface` für typsichere Aufrufe von Entitäten.

> [!NOTE]
> Es ist wichtig zu verstehen, dass die vorherigen „Signal“vorgänge alle asynchron sind. Es ist nicht möglich, eine Entitätsfunktion aufzurufen und einen Rückgabewert von einem Client zu erhalten. Ebenso kann die Rückgabe von `SignalEntityAsync` erfolgen, bevor die Entität mit der Ausführung des Vorgangs beginnt. Nur Orchestratorfunktionen können Entitätsfunktionen synchron aufrufen und Rückgabewerte verarbeiten.

Die `SignalEntityAsync` APIs erfordern die Angabe des eindeutigen Bezeichners der Entität als `EntityId`. Diese APIs nehmen optional auch den Namen des Entitätsvorgangs als `string` und die Nutzlast des Vorgangs als JSON-serialisierbares `object` an. Wenn die Zielentität nicht vorhanden ist, wird sie automatisch mit der angegebenen Entitäts-ID erstellt.

### <a name="client-sample-untyped"></a>Clientbeispiel (nicht typisiert)

Dies ist ein Beispiel für eine von der Warteschlange ausgelöste Funktion, die eine Entität „Counter“ aufruft.

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

### <a name="client-sample-typed"></a>Clientbeispiel (typisiert)

Es ist möglich, ein Proxyobjekt für den typsicheren Zugriff auf Entitätsvorgänge zu generieren. Zum Generieren eines typsicheren Proxys muss der Entitätstyp eine Schnittstelle implementieren. Nehmen wir beispielsweise an, dass die zuvor erwähnte `Counter`-Entität eine `ICounter`-Schnittstelle implementiert hat, die wie folgt definiert ist:

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

Der Clientcode könnte dann `SignalEntityAsync<TEntityInterface>` verwenden und die `ICounter`-Schnittstelle als Typparameter angeben, um einen typsicheren Proxy zu generieren. Diese Verwendung von typsicheren Proxys wird im folgenden Codebeispiel veranschaulicht:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [DurableClient] IDurableEntityClient client)
{
    var target = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(message);
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

Im vorherigen Beispiel ist der `proxy`-Parameter eine dynamisch generierte Instanz von `ICounter`, die den Aufruf von `Add` intern in den entsprechenden (nicht typisierten) Aufruf von `SignalEntityAsync` übersetzt.

Es gibt einige Regeln für die Definition von Entitätsschnittstellen:

* Der Typparameter `TEntityInterface` in `SignalEntityAsync<TEntityInterface>` muss eine Schnittstelle sein.
* Entitätsschnittstellen dürfen nur Methoden definieren.
* Entitätsschnittstellenmethoden dürfen nicht mehr als einen Parameter definieren.
* Entitätsschnittstellenmethoden müssen `void`, `Task` oder `Task<T>` zurückgeben, wobei `T` ein Rückgabewert ist.
* Entitätsschnittstellen müssen genau eine konkrete Implementierungsklasse innerhalb derselben Assembly (d.h. der Entitätsklasse) haben.

Wenn eine dieser Regeln verletzt wird, wird zur Laufzeit eine `InvalidOperationException` ausgelöst. Die Ausnahmemeldung erläutert, welche Regel verletzt wurde.

> [!NOTE]
> Die `SignalEntityAsync`-APIs stellen unidirektionale Vorgänge dar. Wenn eine Entitätsschnittstelle `Task<T>` zurückgibt, ist der Wert des `T`-Parameters immer NULL oder `default`.

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Integrierte in HTTP API-Referenz für Instanzverwaltung](durable-functions-http-api.md)