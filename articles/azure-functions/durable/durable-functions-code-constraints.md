---
title: Codeeinschränkungen für dauerhafte Orchestratoren – Azure Functions
description: Wiedergabe- und Codeeinschränkungen für Azure Durable Functions im Zusammenhang mit Orchestrierungsfunktionen.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/18/2019
ms.author: azfuncdf
ms.openlocfilehash: 87851a4879760c76950f765d05de4662ecb04bea
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935539"
---
# <a name="orchestrator-function-code-constraints"></a>Codeeinschränkungen für Orchestratorfunktionen

Durable Functions ist eine Erweiterung von [Azure Functions](../functions-overview.md), mit der Sie zustandsbehaftete Anwendungen erstellen können. Mit einer [Orchestratorfunktion](durable-functions-orchestrations.md) können Sie die Ausführung anderer dauerhafter Funktionen innerhalb einer Funktions-App orchestrieren. Orchestratorfunktionen sind zustandsbehaftet, zuverlässig und können lange ausgeführt werden.

## <a name="orchestrator-code-constraints"></a>Einschränkungen des Orchestratorcodes

Orchestratorfunktionen nutzen [Ereignissourcing](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing), um eine zuverlässige Ausführung zu gewährleisten und den lokalen Variablenzustand beizubehalten. Mit dem [Wiedergabeverhalten](durable-functions-orchestrations.md#reliability) von Orchestratorcode sind Einschränkungen des Codetyps verbunden, der in einer Orchestratorfunktion geschrieben werden kann. Der Orchestratorcode muss beispielsweise *deterministisch* sein.  Orchestratorfunktionen werden mehrfach wiedergegeben und müssen jedes Mal zum gleichen Ergebnis führen.

In den folgenden Abschnitten finden Sie einige Richtlinien, mit denen Sie sicherstellen können, dass Ihr Code deterministisch ist.

### <a name="using-deterministic-apis"></a>Verwenden deterministischer APIs

Orchestratorfunktionen können jede beliebige API in ihrer Zielsprache aufrufen. Es ist jedoch wichtig, dass Orchestratorfunktionen nur *deterministische* APIs aufrufen. Eine *deterministische API* ist eine API, die bei Verwendung der gleichen Eingabe immer genau den gleichen Wert zurückgibt, und zwar unabhängig davon, wann oder wie oft sie aufgerufen wird.

Im Anschluss finden Sie einige Beispiele für APIs, die *nicht* deterministisch sind und daher nicht verwendet werden sollten. Diese Einschränkungen gelten nur für Orchestratorfunktionen. Für andere Funktionstypen gelten keine Einschränkungen dieser Art.

| API-Kategorie | Grund | Problemumgehung |
| ------------ | ------ | ---------- |
| Datums-/Uhrzeitangaben  | APIs, die das _aktuelle_ Datum oder die aktuelle Uhrzeit zurückgeben, sind nicht deterministisch, da der Rückgabewert bei jeder Wiedergabe unterschiedlich ist. | Die API [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (.NET) bzw. `currentUtcDateTime` (JavaScript) ist für die Wiedergabe geeignet. |
| GUIDs/UUIDs  | APIs, die eine _zufällige_ GUID/UUID zurückgeben, sind nicht deterministisch, da der generierte Wert bei jeder Wiedergabe unterschiedlich ist. | Verwenden Sie die API [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) (.NET) oder `newGuid` (JavaScript) zur sicheren Generierung von Zufalls-GUIDs. |
| Zufallszahlen | APIs, die Zufallszahlen zurückgeben, sind nicht deterministisch, da der generierte Wert bei jeder Wiedergabe unterschiedlich ist. | Verwenden Sie eine Aktivitätsfunktion, um Zufallszahlen an eine Orchestrierung zurückzugeben. Die Rückgabewerte von Aktivitätsfunktionen können immer für die Wiedergabe verwendet werden. |
| Bindungen | Eingabe- und Ausgabebindungen werden in der Regel für Eingaben/Ausgaben genutzt und sind nicht deterministisch. Auch die [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client) und die [Entitätsclientbindung](durable-functions-bindings.md#entity-client) dürfen nicht direkt von einer Orchestratorfunktion verwendet werden. | Verwenden Sie Eingabe- und Ausgabebindungen innerhalb von Client- oder Aktivitätsfunktionen. |
| Netzwerk | An Netzwerkaufrufen sind externe Systeme beteiligt. Sie sind nicht deterministisch. | Verwenden Sie für Netzwerkaufrufe Aktivitätsfunktionen. Wenn Sie in Ihrer Orchestratorfunktion einen HTTP-Aufruf ausführen müssen, können Sie auch [dauerhafte HTTP-APIs](durable-functions-http-features.md#consuming-http-apis) verwenden. |
| Blockierende APIs | Blockierende APIs wie `Thread.Sleep` (.NET) oder andere ähnliche APIs können Leistungs- und Skalierungsprobleme für Orchestratorfunktionen verursachen und sollten daher nicht verwendet werden. Im Azure Functions-Verbrauchstarif können sie sogar zu unnötigen Gebühren für die Ausführungszeit führen. | Verwenden Sie zur Verzögerung der Orchestrierungsausführung nach Möglichkeit Alternativen für blockierende APIs – beispielsweise `CreateTimer`. Verzögerungen durch [permanente Timer](durable-functions-timers.md) zählen nicht zur Ausführungszeit einer Orchestratorfunktion. |
| Asynchrone APIs | Orchestratorcode darf **niemals einen asynchronen Vorgang initiieren**, sofern nicht die [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)-API oder die API des `context.df`-Objekts verwendet wird. Beispielsweise kein `Task.Run`, `Task.Delay` oder `HttpClient.SendAsync` in .NET oder `setTimeout()` und `setInterval()` in JavaScript. Das Durable Task Framework führt Orchestratorcode über einen einzelnen Thread aus und kann nicht mit anderen Threads interagieren, die von anderen asynchronen APIs geplant werden können. | Eine Orchestratorfunktion sollte ausschließlich *dauerhafte* asynchrone Aufrufe durchführen. Alle anderen asynchronen API-Aufrufe sollten über Aktivitätsfunktionen durchgeführt werden. |
| Asynchrone JavaScript-Funktion | JavaScript-Orchestratorfunktionen dürfen nicht asynchron (`async`) sein, da bei der Node.js-Runtime nicht garantiert ist, dass asynchrone Funktionen deterministisch sind. | JavaScript-Orchestratorfunktionen müssen als synchrone Generatorfunktionen deklariert werden. |
| Threading-APIs | Das Durable Task Framework führt Orchestratorcode über einen einzelnen Thread aus und kann nicht mit anderen Threads interagieren. Die Einbringung neuer Threads in die Ausführung einer Orchestrierung kann zu einer nicht deterministischen Ausführung oder zu Deadlocks führen. | Threading-APIs sollten so gut wie nie in Orchestratorfunktionen verwendet werden. Falls sie doch erforderlich sind, sollten sie auf Aktivitätsfunktionen beschränkt werden. |
| Statische Variablen | Die Verwendung nicht konstanter statischer Variablen sollte in Orchestratorfunktionen vermieden werden, da sich ihre Werte im Laufe der Zeit ändern können, was zu einem nicht deterministischen Ausführungsverhalten führt. | Verwenden Sie Konstanten, oder beschränken Sie die Verwendung statischer Variablen auf Aktivitätsfunktionen. |
| Umgebungsvariablen | Verwenden Sie in Orchestratorfunktionen keine Umgebungsvariablen. Deren Werte können sich im Laufe der Zeit ändern, was zu einem nicht deterministischen Ausführungsverhalten führt. | Auf Umgebungsvariablen darf nur innerhalb von Client- oder Aktivitätsfunktionen verwiesen werden. |
| Endlosschleifen | Endlosschleifen sollten in Orchestratorfunktionen vermieden werden. Da das Durable Task Framework den Ausführungsverlauf speichert, während die Orchestrierungsfunktion voranschreitet, kann eine Endlosschleife unter Umständen dazu führen, dass für eine Orchestratorinstanz irgendwann nicht mehr genügend Arbeitsspeicher zur Verfügung steht. | Verwenden Sie für Szenarien mit Endlosschleifen APIs wie beispielsweise [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) (.NET) oder `continueAsNew` (JavaScript), um die Funktionsausführung neu zu starten und den vorherigen Ausführungsverlauf zu verwerfen. |

Diese Einschränkungen können zuerst etwas abschreckend wirken, aber in der Praxis ist ihre Einhaltung nicht sehr schwierig. Das Durable Task Framework versucht, Verstöße gegen die obigen Regeln zu erkennen, und löst eine `NonDeterministicOrchestrationException` aus. Dieses Erkennungsverhalten wird aber jeweils nur auf bestmögliche Weise durchgeführt, sodass Sie sich nicht darauf verlassen sollten.

## <a name="versioning"></a>Versionsverwaltung

Eine dauerhafte Orchestrierung kann ununterbrochen über mehrere Tage, Monate oder Jahre hinweg oder sogar [unendlich lange](durable-functions-eternal-orchestrations.md) ausgeführt werden. Codeaktualisierungen für Durable Functions-Anwendungen, die sich auf noch nicht abgeschlossene Orchestrierungen auswirken, können deren Wiedergabeverhalten beeinträchtigen. Daher müssen Codeaktualisierungen sorgfältig geplant werden. Eine ausführlichere Beschreibung der Versionsverwaltung für Ihren Code finden Sie im Artikel [Versionsverwaltung in Durable Functions (Azure Functions)](durable-functions-versioning.md).

## <a name="durable-tasks"></a>Dauerhafte Aufgaben

> [!NOTE]
> In diesem Abschnitt werden Details der internen Implementierung für das Durable Task Framework beschrieben. Sie können Durable Functions nutzen, ohne diese Informationen zu kennen. Sie sollen Ihnen nur als Hilfe beim Verständnis des Wiedergabeverhaltens dienen.

Aufgaben, die in Orchestratorfunktionen auf sichere Weise erwartet werden können, werden auch als *dauerhafte Aufgaben* bezeichnet. Dies Aufgaben werden vom Durable Task Framework erstellt und verwaltet. Beispiele wären etwa die Aufgaben, die in .NET-Orchestratorfunktionen von `CallActivityAsync`, `WaitForExternalEvent` und `CreateTimer` zurückgegeben werden.

Diese *dauerhaften Aufgaben* werden intern mithilfe einer Liste mit Objekten vom Typ `TaskCompletionSource` in .NET verwaltet. Während der Wiedergabe werden diese Aufgaben im Rahmen der Ausführung des Orchestratorcodes erstellt und durchgeführt, wenn der Verteiler die entsprechenden Verlaufsereignisse auflistet. Die Ausführung erfolgt synchron mit einem einzelnen Thread, bis der gesamte Verlauf wiedergegeben wurde. Für alle dauerhaften Aufgaben, die am Ende der Verlaufswiedergabe nicht abgeschlossen wurden, werden entsprechende Aktionen durchgeführt. Beispielsweise kann eine Nachricht in die Warteschlange eingereiht werden, um eine Aktivitätsfunktion aufzurufen.

Das hier beschriebene Ausführungsverhalten verdeutlicht, warum Orchestratorfunktionscode keine Vorgänge vom Typ `await` oder `yield` für eine nicht dauerhafte Aufgabe ausführen darf: Der Verteilerthread kann nicht auf den Abschluss der Ausführung warten, und alle Rückrufe dieser Aufgabe können ggf. eine Beschädigung des Nachverfolgungsstatus der Orchestratorfunktion nach sich ziehen. Es stehen einige Runtimeüberprüfungen zur Verfügung, mit denen versucht werden kann, diese Verstöße zu erkennen.

Falls Sie weitere Informationen dazu benötigen, wie Orchestratorfunktionen im Durable Task Framework ausgeführt werden, hilft Innen der Artikel zum [Durable Task-Quellcode auf GitHub](https://github.com/Azure/durabletask) weiter. Sehen Sie sich vor allem [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) und [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs) an.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informieren Sie sich über das Aufrufen untergeordneter Orchestrierungen.](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Versioning in Durable Functions (Azure Functions)](durable-functions-versioning.md) (Versionsverwaltung in Durable Functions [Azure Functions])
