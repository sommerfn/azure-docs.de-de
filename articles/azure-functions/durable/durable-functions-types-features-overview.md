---
title: Funktionstypen in der Durable Functions-Erweiterung von Azure Functions
description: Machen Sie sich mit Funktionstypen und -rollen vertraut, die in einer Durable Functions-Orchestrierung in Azure Functions die direkte Kommunikation zwischen Funktionen ermöglicht.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: azfuncdf
ms.openlocfilehash: 7b395bd6024beb52b9263ac4fe655b5328a8e662
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "70933155"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Durable Functions-Typen und -Features (Azure Functions)

Durable Functions ist eine Erweiterung von [Azure Functions](../functions-overview.md). Sie eignet sich für die zustandsbehaftete Orchestrierung der Funktionsausführung. Eine Durable Functions-App stellt eine Lösung dar, die sich aus verschiedenen Azure-Funktionen zusammensetzt. Funktionen können in einer Durable Functions-Orchestrierung unterschiedliche Rollen spielen. 

In Azure Functions stehen zurzeit vier Durable Functions-Typen zur Verfügung: Aktivitäts-, Orchestrator-, Entitäts- und Clientfunktionen. Im weiteren Verlauf dieses Abschnitts wird näher auf den Typ der Funktionen eingegangen, die an einer Orchestrierung beteiligt sind.

## <a name="orchestrator-functions"></a>Orchestratorfunktionen

Orchestratorfunktionen beschreiben, wie und in welcher Reihenfolge Aktionen ausgeführt werden. Orchestratorfunktionen beschreiben die Orchestrierung in Code (C# oder JavaScript), wie unter [Anwendungsmuster von Durable Functions](durable-functions-overview.md#application-patterns) gezeigt. Eine Orchestrierung kann viele verschiedene Aktionstypen umfassen, z. B. [Aktivitätsfunktionen](#activity-functions), [untergeordnete Orchestrierungen](durable-functions-orchestrations.md#sub-orchestrations), [das Warten auf externe Ereignisse](durable-functions-orchestrations.md#external-events), [HTTP](durable-functions-orchestrations.md#calling-http-endpoints) und [Timer](durable-functions-orchestrations.md#durable-timers). Orchestratorfunktionen können auch mit [Entitätsfunktionen](#entity-functions) interagieren.

> [!NOTE]
> Orchestratorfunktionen werden mithilfe von normalem Code geschrieben, aber es gibt strenge Anforderungen zum Schreiben des Codes. Insbesondere muss Orchestratorfunktionscode *deterministisch* sein. Die Nichteinhaltung dieser Determinismusanforderungen kann dazu führen, dass die Orchestratorfunktionen nicht ordnungsgemäß ausgeführt werden. Detaillierte Informationen zu diesen Anforderungen und zu deren Umgehung finden Sie im Thema [Codeeinschränkungen](durable-functions-code-constraints.md).

Ausführlichere Informationen zu Orchestratorfunktionen und deren Features finden Sie im Artikel [Dauerhafte Orchestrierungen](durable-functions-orchestrations.md).

## <a name="activity-functions"></a>Aktivitätsfunktionen

Aktivitätsfunktionen sind die grundlegende Arbeitseinheit in der Orchestrierung einer permanenten Funktion. Sie sind die Funktionen und Aufgaben, die im Prozess orchestriert werden. Mit einer Orchestratorfunktion können Sie z.B. eine Bestellung verarbeiten. Die Aufgaben würden dann die Inventarprüfung, die Abrechnung von Kundengebühren und die Erstellung einer Lieferung beinhalten. Jede Aufgabe wäre dann eine separate Aktivitätsfunktion. Diese Aktivitätsfunktionen können seriell, parallel oder in einer Kombination von beidem ausgeführt werden.

Im Gegensatz zu Orchestratorfunktionen bestehen für Aktivitätsfunktionen hinsichtlich der Aufgaben, die mit ihnen ausgeführt werden können, keine Einschränkungen. Aktivitätsfunktionen werden häufig verwendet, um Netzwerkaufrufe zu tätigen oder CPU-intensive Vorgänge auszuführen. Eine Aktivitätsfunktion kann außerdem Daten an die Orchestratorfunktion zurückgeben. Das Durable Task-Framework garantiert, dass jede aufgerufene Aktivitätsfunktion *mindestens ein Mal* während der Ausführung einer Orchestrierung ausgeführt wird.

> [!NOTE]
> Da Aktivitätsfunktionen nur *mindestens eine* Ausführung garantieren, wird empfohlen, die Aktivitätsfunktionslogik der nach Möglichkeit *idempotent* zu gestalten.

Verwenden Sie einen [Aktivitätstrigger](durable-functions-bindings.md#activity-trigger), um eine Aktivitätsfunktion zu definieren. .NET-Funktionen nehmen einen [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) als Parameter entgegen. Sie können den Trigger auch an beliebige andere JSON-serialisierbare Objekte binden, um Eingaben an die Funktion zu übergeben. In JavaScript können Sie über die `<activity trigger binding name>`-Eigenschaft des [`context.bindings`-Objekts](../functions-reference-node.md#bindings) auf die Eingabe zugreifen. An Aktivitätsfunktionen kann nur ein einzelner Wert übergeben werden. Um mehrere Werte zu übergeben, müssen Sie Tupel, Arrays oder komplexe Typen verwenden.

> [!NOTE]
> Eine Aktivitätsfunktion können Sie nur über eine Orchestratorfunktion auslösen.

## <a name="entity-functions"></a>Entitätsfunktionen

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsinformationen. Diese zustandsbehafteten Entitäten werden häufig als *permanente Entitäten* bezeichnet. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Sie können auch aus Clientfunktionen oder Orchestratorfunktionen aufgerufen werden. Im Gegensatz zu Orchestratorfunktionen müssen Entitätsfunktionen keine spezifischen Codeeinschränkungen besitzen. Entitätsfunktionen verwalten Zustände auch explizit, statt Zustände implizit durch die Ablaufsteuerung darzustellen.

> [!NOTE]
> Entitätsfunktionen und zugehörige Funktionen sind nur in Durable Functions 2.0 und höher verfügbar. Entitätsfunktionen sind zurzeit als öffentliche Vorschau verfügbar.

Weitere Informationen zu Entitätsfunktionen finden Sie im Artikel [Permanente Entitäten](durable-functions-entities.md).

## <a name="client-functions"></a>Clientfunktionen

Orchestratorfunktionen werden durch eine [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) und Entitätsfunktionen durch eine [Entitätstriggerbindung](durable-functions-bindings.md#entity-trigger) ausgelöst. Beide Trigger funktionieren, indem sie auf Nachrichten reagieren, die in eine Warteschlange eines [Aufgabenhubs](durable-functions-task-hubs.md) eingereiht werden. Die primäre Art und Weise, diese Nachrichten zu übermitteln, besteht in der Verwendung einer [Orchestratorclientbindung](durable-functions-bindings.md#orchestration-client) oder einer [Entitätsclientbindung](durable-functions-bindings.md#entity-client) aus einer *Clientfunktion*. Jede Nicht-Orchestratorfunktion kann eine *Clientfunktion* sein. Sie können den Orchestrator beispielsweise aus einer durch HTTP ausgelösten Funktion, einer durch Azure Event Hub ausgelösten Funktion usw. auslösen. Was eine Funktion zu einer *Clientfunktion* macht, ist die Verwendung der permanenten Clientausgabebindung.

> [!NOTE]
> Im Gegensatz zu anderen Funktionstypen können Orchestrator- und Entitätsfunktionen nicht direkt über die Schaltflächen im Azure-Portal ausgelöst werden. Wenn Sie eine Orchestrator- oder Entitätsfunktion im Azure Portal testen möchten, müssen Sie stattdessen eine *Clientfunktion* ausführen, die eine Orchestrator- oder Entitätsfunktion als Teil ihrer Implementierung startet. Für die einfachste Testerfahrung wird eine *manuelle Triggerfunktion* empfohlen.

Zusätzlich zur Auslösung von Orchestrator- oder Entitätsfunktionen kann die *permanente Clientbindung* verwendet werden, um mit ausgeführten Orchestrierungen und Entitäten zu interagieren. Beispielsweise können Orchestrierungen abgefragt oder beendet werden, und es können Ereignisse für sie ausgelöst werden. Weitere Informationen zur Verwaltung von Orchestrierungen und Entitäten finden Sie im Artikel [Instanzverwaltung](durable-functions-instance-management.md).

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie Ihre erste permanente Funktion in [C#](durable-functions-create-first-csharp.md) oder [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Weitere Informationen zu Durable Functions-Orchestrierungen](durable-functions-orchestrations.md)