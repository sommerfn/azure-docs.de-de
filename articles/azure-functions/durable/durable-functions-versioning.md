---
title: Versionsverwaltung in Durable Functions – Azure
description: Es wird beschrieben, wie Sie die Versionsverwaltung in der Erweiterung „Durable Functions“ für Azure Functions implementieren.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 4b4e82acbd3037c70b87731c0661605041090435
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73614508"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Versionsverwaltung in Durable Functions (Azure Functions)

Es ist unvermeidbar, dass Funktionen während der Lebensdauer einer Anwendung hinzugefügt, entfernt und geändert werden. Mithilfe von [Durable Functions](durable-functions-overview.md) (Langlebige Funktionen) können Funktionen auf eine Art und Weise verkettet werden, die vorher nicht möglich war. Diese Verkettung wirkt sich darauf aus, wie Sie die Versionsverwaltung nutzen können.

## <a name="how-to-handle-breaking-changes"></a>Behandeln von wichtigen Änderungen

Es gibt einige Beispiele für wichtige Änderungen, mit denen Sie vertraut sein sollten. In diesem Artikel werden die gängigsten Änderungen beschrieben. Für alle Änderungen gilt der Grundsatz, dass sich sowohl durch neue als auch durch vorhandene Funktionsorchestrierungen Auswirkungen auf den Funktionscode ergeben.

### <a name="changing-activity-or-entity-function-signatures"></a>Ändern der Signaturen von Aktivitäts- oder Entitätsfunktionen

Eine Signaturänderung bezieht sich auf eine Änderung des Namens, der Eingabe oder der Ausgabe einer Funktion. Wenn an einer Aktivitäts- oder Entitätsfunktion diese Art von Änderung vorgenommen wird, kann dies für jegliche davon abhängige Orchestratorfunktion zu einem Fehler führen. Wenn Sie die Orchestratorfunktion aktualisieren, um diese Änderung zu berücksichtigen, kann dies wiederum zu Fehlern bei vorhandenen ausgeführten Instanzen führen.

Angenommen, wir verfügen beispielsweise über die folgende Orchestratorfunktion.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bei dieser einfachen Funktion werden die Ergebnisse von **Foo** an **Bar** übergeben. Angenommen, wir müssen den Rückgabewert von **Foo** von `bool` in `int` ändern, um mehr Ergebniswerte zu unterstützen. Das Ergebnis sieht wie folgt aus:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

Diese Änderung funktioniert für alle neuen Instanzen der Orchestratorfunktion ohne Probleme, aber für ausgeführte Instanzen treten Fehler auf. Nehmen Sie beispielsweise den Fall, dass eine Orchestrierungsinstanz eine Funktion namens `Foo` aufruft, einen booleschen Wert zurückerhält und dann einen Prüfpunkt einrichtet. Wenn die Signaturänderung an diesem Punkt bereitgestellt wird, tritt für die Instanz mit dem Prüfpunkt sofort ein Fehler auf, wenn der Vorgang fortgesetzt und der Aufruf von `context.CallActivityAsync<int>("Foo")` wiedergegeben wird. Dieser Fehler tritt auf, weil das Ergebnis in der Verlaufstabelle `bool` lautet, während der neue Code versucht, die Deserialisierung in `int` durchzuführen.

Dieses Beispiel ist nur eine von vielen verschiedenen Möglichkeiten, wie eine Signaturänderung für vorhandene Instanzen zu Fehlern führen kann. Wenn ein Orchestrator die Vorgehensweise zum Aufrufen einer Funktion ändern muss, ist die Änderung im Allgemeinen problematisch.

### <a name="changing-orchestrator-logic"></a>Ändern der Orchestratorlogik

Die andere Klasse der Probleme mit der Versionsverwaltung basiert auf einer Änderung des Orchestratorfunktionscodes, die für die Wiedergabelogik von ausgeführten Instanzen verwirrend ist.

Sehen Sie sich die folgende Orchestratorfunktion an:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Nehmen wir nun an, Sie möchten eine anscheinend unbedeutende Änderung vornehmen, um einen weiteren Funktionsaufruf hinzuzufügen.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    if (result)
    {
        await context.CallActivityAsync("SendNotification");
    }

    await context.CallActivityAsync("Bar", result);
}
```

> [!NOTE]
> Die vorherigen C#-Beispiele gelten für Durable Functions 2.x. Für Durable Functions 1.x müssen Sie `DurableOrchestrationContext` anstelle von `IDurableOrchestrationContext` verwenden. Weitere Informationen zu den Unterschieden zwischen den Versionen finden Sie im Artikel [Durable Functions-Versionen](durable-functions-versions.md).

Mit dieser Änderung wird zwischen **Foo** und **Bar** ein neuer Funktionsaufruf von **SendNotification** hinzugefügt. Es werden keine Signaturänderungen vorgenommen. Das Problem tritt auf, wenn die Ausführung einer vorhandenen Instanz nach dem Aufruf von **Bar** fortgesetzt wird. Wenn für den ursprünglichen Aufruf von **Foo** das Ergebnis `true` zurückgegeben wurde, wird bei der Wiedergabe des Orchestrators das **SendNotification**-Element aufgerufen, das im dazugehörigen Ausführungsverlauf nicht enthalten ist. Daher tritt für das Durable Task Framework der Fehler `NonDeterministicOrchestrationException` auf, da ein Aufruf von **SendNotification** erfolgt ist, während ein Aufruf von **Bar** erwartet wurde. Dieselbe Art von Problem kann beim Hinzufügen von Aufrufen an „permanente“ APIs auftreten, einschließlich `CreateTimer`, `WaitForExternalEvent` usw.

## <a name="mitigation-strategies"></a>Lösungsstrategien

Hier sind einige Strategien zum Umgang mit Problemen bei der Versionsverwaltung angegeben:

* Keine Maßnahmen
* Beendigung aller ausgeführten Instanzen
* Parallele Bereitstellungen

### <a name="do-nothing"></a>Keine Maßnahmen

Die einfachste Möglichkeit für den Umgang mit einer wichtigen Änderung besteht darin, ausgeführte Orchestrierungsinstanzen fehlschlagen zu lassen. Auf neuen Instanzen ist die Ausführung des geänderten Codes erfolgreich.

Es hängt von der Wichtigkeit Ihrer ausgeführten Instanzen ab, ob dieser Fehler ein Problem darstellt. Wenn Sie sich in der Phase der aktiven Entwicklung befinden und ausgeführte Instanzen nicht besonders wichtig sind, kann diese Lösung ausreichen. In Ihrer Diagnosepipeline kommt es in diesem Fall dann aber zu Ausnahmen und Fehlern. Wenn Sie diese Probleme vermeiden möchten, sollten Sie die anderen Lösungsmöglichkeiten für die Versionsverwaltung erwägen.

### <a name="stop-all-in-flight-instances"></a>Beendigung aller ausgeführten Instanzen

Eine andere Möglichkeit besteht darin, alle ausgeführten Instanzen zu beenden. Sie erreichen das Beenden aller Instanzen, indem Sie den Inhalt der internen Warteschlangen **control-queue** und **workitem-queue** löschen. Die Instanzen bleiben so für immer am jeweiligen Punkt hängen, aber Ihre Protokolle enthalten nicht übermäßig viele Fehlermeldungen. Dieser Ansatz ist für die schnelle Prototypentwicklung ideal.

> [!WARNING]
> Da sich die Details dieser Warteschlangen im Laufe der Zeit ändern können, sollten Sie dieses Verfahren nicht für Produktionsworkloads verwenden.

### <a name="side-by-side-deployments"></a>Parallele Bereitstellungen

Die sicherste Möglichkeit zum Sicherstellen, dass wichtige Änderungen auf sichere Weise bereitgestellt werden, ist die parallele Bereitstellung mit Ihren älteren Versionen. Hierfür können Sie die folgenden Verfahren verwenden:

* Bereitstellen aller Updates als völlig neue Funktionen, wobei bestehende Funktionen unverändert bleiben. Dies kann schwierig sein, da die Aufrufer der neuen Funktionsversionen ebenfalls unter Einhaltung derselben Richtlinien aktualisiert werden müssen.
* Bereitstellen aller Updates als neue Funktionen-App mit einem anderen Speicherkonto
* Bereitstellen einer neuen Kopie der Funktions-App mit demselben Speicherkonto, aber mit einem aktualisierten `taskHub`-Namen. Parallele Bereitstellungen werden empfohlen.

### <a name="how-to-change-task-hub-name"></a>Ändern des Aufgabenhubnamens

Sie können den Aufgabenhub in der Datei *host.json* wie folgt konfigurieren:

#### <a name="functions-1x"></a>Functions 1.x

```json
{
    "durableTask": {
        "hubName": "MyTaskHubV2"
    }
}
```

#### <a name="functions-20"></a>Functions 2.0

```json
{
    "extensions": {
        "durableTask": {
            "hubName": "MyTaskHubV2"
        }
    }
}
```

Der Standardwert für Durable Functions v1. x ist `DurableFunctionsHub`. Ab Durable Functions v 2.0 ist der Standardname des Aufgabenhubs identisch mit dem Namen der Funktions-App in Azure, oder er lautet `TestHubName`, wenn die Ausführung außerhalb von Azure erfolgt.

Alle Azure Storage-Entitäten werden basierend auf dem Konfigurationswert `hubName` benannt. Indem Sie dem Aufgabenhub einen neuen Namen geben, stellen Sie sicher, dass für die neue Version Ihrer Anwendung die Warteschlangen und die Verlaufstabelle separat erstellt werden. Die Funktions-App beendet jedoch die Verarbeitung von Ereignissen für Orchestrierungen oder Entitäten, die unter dem vorherigen Aufgabenhubnamen erstellt wurden.

Es wird empfohlen, die neue Version der Funktionen-App in einem neuen [Bereitstellungsslot](../functions-deployment-slots.md) bereitzustellen. Mit Bereitstellungsslots können Sie mehrere Kopien Ihrer Funktionen-App parallel ausführen, wobei nur eine davon als aktiver *Produktionsslot* dient. Wenn alles bereit ist, um die neue Orchestrierungslogik für Ihre vorhandene Infrastruktur verfügbar zu machen, kann dies ein einfacher Vorgang sein, weil ggf. nur die neue Version in den Produktionsslot eingefügt werden muss.

> [!NOTE]
> Diese Strategie eignet sich am besten, wenn Sie HTTP und Webhooktrigger für Orchestratorfunktionen verwenden. Für andere Trigger als HTTP-Trigger, z.B. Warteschlangen oder Event Hubs, sollte die Triggerdefinition [von einer App-Einstellung abgeleitet sein](../functions-bindings-expressions-patterns.md#binding-expressions---app-settings), die im Rahmen des Austauschvorgangs aktualisiert wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Umgang mit Leistungs- und Skalierungsproblemen](durable-functions-perf-and-scale.md)
