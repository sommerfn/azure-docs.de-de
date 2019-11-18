---
title: Behandeln von externen Ereignissen in Durable Functions – Azure
description: Es wird beschrieben, wie Sie externe Ereignisse in der Erweiterung Durable Functions für Azure Functions behandeln.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 1c2a2f08c31c427241bbd5e482906118a90ee178
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73614833"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Behandeln von externen Ereignissen in Durable Functions (Azure Functions)

Orchestratorfunktionen können warten und auf externe Ereignisse lauschen. Das Feature [Durable Functions](durable-functions-overview.md) ist oft hilfreich für die Behandlung menschlicher Interaktion oder anderer externer Trigger.

> [!NOTE]
> Externe Ereignisse sind unidirektionale, asynchrone Vorgänge. Sie sind nicht geeignet für Situationen, in denen der Client, der das Ereignis sendet, eine synchrone Antwort von der Orchestratorfunktion benötigt.

## <a name="wait-for-events"></a>Warten auf Ereignisse

Mit den Methoden `WaitForExternalEvent` (.NET) und `waitForExternalEvent` (JavaScript) der [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) kann eine Orchestratorfunktion asynchron auf ein externes Ereignis warten und lauschen. Die lauschende Orchestratorfunktion deklariert den *Namen* des Ereignisses und die *Form der Daten*, die sie zu empfangen erwartet.

### <a name="c"></a>C#

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

> [!NOTE]
> Der vorherige C#-Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

Im vorangehenden Beispiel wird auf ein bestimmtes einzelnes Ereignis gelauscht und bei Empfang eine entsprechende Aktion ausgeführt.

Sie können wie im folgenden Beispiel, in dem auf eine von drei möglichen Ereignisbenachrichtigungen gewartet wird, auf mehrere Ereignisse gleichzeitig lauschen.

#### <a name="c"></a>C#

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

> [!NOTE]
> Der vorherige C#-Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

Im vorherigen Beispiel wird auf *alle* von mehreren Ereignissen gelauscht. Es ist auch möglich, auf *alle* Ereignisse zu warten.

#### <a name="c"></a>C#

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

> [!NOTE]
> Der vorherige Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

#### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivity("IssueBuildingPermit", applicationId);
});
```

`WaitForExternalEvent` wartet unbegrenzt auf Eingaben.  Die Funktionen-App kann während des Wartens problemlos entladen werden. Wenn ein Ereignis für diese Orchestrierungsinstanz eingeht, wird sie automatisch aktiviert und verarbeitet das Ereignis sofort.

> [!NOTE]
> Wenn Ihre Funktions-App den Verbrauchstarif verwendet, fallen, während eine Orchestratorfunktion auf eine Aufgabe von `WaitForExternalEvent` (.NET) oder `waitForExternalEvent` (JavaScript) wartet, keine Kosten an, unabhängig davon, wie lange sie wartet.

Wenn in .NET die Ereignisnutzlast nicht in den erwarteten Typ `T` konvertiert werden kann, wird eine Ausnahme ausgelöst.

## <a name="send-events"></a>Senden von Ereignisse

Die `RaiseEventAsync`-Methode (.NET) oder `raiseEvent`-Methode (JavaScript) der [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client) sendet die Ereignisse, auf die `WaitForExternalEvent` (.NET) oder `waitForExternalEvent` (JavaScript) wartet.  Die `RaiseEventAsync`-Methode übernimmt *eventName* und *eventData* als Parameter. Die Ereignisdaten müssen JSON-serialisierbar sein.

Im Folgenden finden Sie eine durch eine Warteschlange ausgelöste Beispielfunktion, die ein „Genehmigungs“-Ereignis an eine Orchestratorfunktionsinstanz sendet. Die Orchestrierungsinstanz-ID stammt aus dem Text der Warteschlangennachricht.

### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [DurableClient] IDurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

> [!NOTE]
> Der vorherige C#-Code ist für Durable Functions 2.x vorgesehen. Für Durable Functions 1.x müssen Sie das `OrchestrationClient`-Attribut anstelle des `DurableClient`-Attributs verwenden, und Sie müssen den `DurableOrchestrationClient`-Parametertyp anstelle von `IDurableOrchestrationClient` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

### <a name="javascript-functions-20-only"></a>JavaScript (nur Functions 2.0)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

Intern fügt `RaiseEventAsync` (.NET) oder `raiseEvent` (JavaScript) eine Nachricht in die Warteschlange ein, die von der wartenden Orchestratorfunktion übernommen wird. Falls die Instanz nicht auf den angegebenen *Ereignisnamen* wartet, wird die Ereignisnachricht einer In-Memory-Warteschlange hinzugefügt. Wenn die Orchestrierungsinstanz später auf den *Ereignisnamen* lauscht, überprüft sie die Warteschlange auf Ereignisnachrichten.

> [!NOTE]
> Ist keine Orchestrierungsinstanz mit der angegebenen *Instanz-ID* vorhanden, wird die Ereignisnachricht verworfen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie, wie Fehlerbehandlung implementiert wird](durable-functions-error-handling.md)

> [!div class="nextstepaction"]
> [Human interaction in Durable Functions - Phone verification sample](durable-functions-phone-verification.md) (Menschliche Interaktion in Durable Functions – Telefonüberprüfungsbeispiel)