---
title: Dauerhafte Orchestrierungen – Azure Functions
description: Einführung in das Orchestrierungsfeature für Azure Durable Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 09/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 4e11070f4e766f83b0e7ead7757c675de3fef33f
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73614769"
---
# <a name="durable-orchestrations"></a>Dauerhafte Orchestrierungen

Durable Functions ist eine Erweiterung von [Azure Functions](../functions-overview.md). Mit einer *Orchestratorfunktion* können Sie die Ausführung anderer dauerhafter Funktionen innerhalb einer Funktions-App orchestrieren. Orchestratorfunktionen verfügen über folgende Merkmale:

* Orchestratorfunktionen definieren Funktionsworkflows unter Verwendung von prozeduralem Code. Es werden keine deklarativen Schemas oder Designer benötigt.
* Orchestratorfunktionen können andere dauerhafte Funktionen synchron und asynchron aufrufen. Ausgaben aufgerufener Funktionen können zuverlässig in lokalen Variablen gespeichert werden.
* Orchestratorfunktionen sind dauerhaft und zuverlässig. Für den Ausführungsstatus werden automatisch Prüfpunkte erstellt, wenn die Funktion wartet oder angehalten wird. Der lokale Zustand geht nie verloren, wenn der Prozess oder virtuelle Computer neu gestartet wird.
* Orchestratorfunktionen können eine lange Ausführungsdauer haben. Die Gesamtlebensdauer einer *Orchestrierungsinstanz* kann Sekunden, Tage oder Monate betragen oder unbegrenzt sein.

Dieser Artikel enthält eine Übersicht über Orchestratorfunktionen und erläutert, wie sie zur Bewältigung verschiedener Herausforderungen bei der App-Entwicklung beitragen können. Sollten Sie noch nicht mit den Funktionstypen vertraut sein, die in einer Durable Functions-App zur Verfügung stehen, lesen Sie zunächst den Artikel [Durable Functions-Typen und -Features (Azure Functions)](durable-functions-types-features-overview.md).

## <a name="orchestration-identity"></a>Orchestrierungsidentität

Jede *Instanz* einer Orchestrierung verfügt über einen Instanzbezeichner (auch *Instanz-ID* genannt). Standardmäßig handelt es sich bei jeder Instanz-ID um eine automatisch generierte GUID. Eine Instanz-ID kann aber auch ein beliebiger benutzergenerierter Zeichenfolgenwert sein. Jede Orchestrierungsinstanz-ID muss innerhalb eines [Aufgabenhubs](durable-functions-task-hubs.md) eindeutig sein.

Im Anschluss folgen einige Regeln für Instanz-IDs:

* Instanz-IDs müssen zwischen einem und 256 Zeichen lang sein.
* Instanz-IDs dürfen nicht mit `@` beginnen.
* Instanz-IDs dürfen keines der folgenden Zeichen enthalten: `/`, `\`, `#` und `?`.
* Instanz-IDs dürfen keine Steuerzeichen enthalten.

> [!NOTE]
> Im Allgemeinen empfiehlt es sich, nach Möglichkeit automatisch generierte Instanz-IDs zu verwenden. Benutzergenerierte Instanz-IDs sind für Szenarien gedacht, in denen eine 1:1-Zuordnung zwischen einer Orchestrierungsinstanz und einer externen anwendungsspezifischen Entität (beispielsweise eine Bestellung oder ein Dokument) besteht.

Die Instanz-ID einer Orchestrierung ist ein erforderlicher Parameter für die meisten [Instanzverwaltungsvorgänge](durable-functions-instance-management.md). Darüber hinaus ist sie wichtig für Diagnosen – etwa beim [Durchsuchen der Nachverfolgungsdaten für die Orchestrierung](durable-functions-diagnostics.md#application-insights) in Application Insights zu Problembehandlungs- oder Analysezwecken. Aus diesem Grund empfiehlt es sich, generierte Instanz-IDs an einem externen Speicherort (etwa in einer Datenbank oder in Anwendungsprotokollen) zu speichern, sodass später problemlos auf sie verwiesen werden kann.

## <a name="reliability"></a>Zuverlässigkeit

Orchestratorfunktionen verwalten ihren Ausführungsstatus zuverlässig mithilfe eines als [Ereignissourcing](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) bezeichneten Entwurfsmusters. Anstatt den aktuellen Zustand einer Orchestrierung direkt zu speichern, verwendet das Durable Task Framework einen ausschließlich zum Anfügen bestimmten Speicher, um die vollständige Aktionsreihe zu erfassen, die von der Funktionsorchestrierung ausgeführt wird. Ein reiner Anfügespeicher bietet viele Vorteile im Vergleich mit dem „Abladen“ des gesamten Laufzeitstatus. Zu den Vorteilen zählen die gesteigerte Leistung, Skalierbarkeit und Reaktionsfähigkeit. Sie erhalten außerdem endgültige Konsistenz von Transaktionsdaten und vollständige Überwachungspfade sowie vollständigen Verlauf. Die Überwachungspfade unterstützen zuverlässige kompensierende Aktionen.

Durable Functions verwendet Ereignissourcing transparent. Im Hintergrund gibt der `await`-Operator (C#) oder `yield`-Operator (JavaScript) in einer Orchestratorfunktion die Steuerung des Orchestratorthreads an den Durable Task Framework-Verteiler zurück. Der Verteiler committet dann alle neuen Aktionen, die die Orchestratorfunktion geplant hat (z.B. Aufrufen mindestens einer untergeordneten Funktion oder Planen eines permanenten Timers) in den Speicher. Dieser transparente Commitvorgang wird dem Ausführungsverlauf der Orchestrierungsinstanz angefügt. Der Verlauf wird in einer Speichertabelle gespeichert. Dann fügt die Commitaktion Nachrichten an eine Warteschlange an, um die eigentliche Arbeit zu planen. An diesem Punkt kann die Orchestratorfunktion aus dem Arbeitsspeicher entladen werden.

Wenn eine Orchestrierungsfunktion weitere Aufgaben ausführen muss (z.B. wird eine Antwortnachricht empfangen, oder ein permanenter Timer läuft ab), wird der Orchestrator reaktiviert und führt erneut die gesamte Funktion von Beginn an neu aus, um den lokalen Status wiederherzustellen. Wenn der Code während dieser Wiedergabe versucht, eine Funktion aufzurufen (oder eine andere asynchrone Aktion auszuführen), zieht Durable Task Framework den Ausführungsverlauf der aktuellen Orchestrierung zu Rate. Wenn festgestellt wird, dass die [Aktivitätsfunktion](durable-functions-types-features-overview.md#activity-functions) bereits ausgeführt wurde und ein Ergebnis erbracht hat, wird dieses Funktionsergebnis wiedergegeben und der Orchestratorcode weiter ausgeführt. Die Wiedergabe wird fortgesetzt, bis der Funktionscode abgeschlossen ist oder geplante neue asynchrone Arbeit ansteht.

> [!NOTE]
> Damit das Wiedergabemuster ordnungsgemäß und zuverlässig funktioniert, muss der Orchestratorfunktionscode *deterministisch* sein. Weitere Informationen zu Codeeinschränkungen für Orchestratorfunktionen finden Sie im Thema [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md).

> [!NOTE]
> Wenn eine Orchestratorfunktion Protokollmeldungen ausgibt, kann das Wiedergabeverhalten zur Ausgabe doppelter Protokollmeldungen führen. Informationen zum Grund für dieses Verhalten sowie zur Umgehung dieses Problems finden Sie im Thema [Protokollierung](durable-functions-diagnostics.md#logging).

## <a name="orchestration-history"></a>Orchestrierungsverlauf

Das Ereignissourcingverhalten des Durable Task Frameworks ist eng an den von Ihnen geschriebenen Orchestratorfunktionscode gekoppelt. Angenommen, Sie verfügen über eine Orchestratorfunktion zur Verkettung von Aktivitäten, wie etwa die folgende C#-Orchestratorfunktion:

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

In JavaScript sieht Ihre Orchestratorfunktion zur Verkettung von Aktivitäten unter Umständen wie im folgenden Codebeispiel aus:

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return output;
});
```

Bei jeder `await`-Anweisung (C#) oder `yield`-Anweisung (JavaScript) erstellt das Durable Task Framework einen Prüfpunkt für den Ausführungszustand der Funktion in einem dauerhaften Speicher-Back-End (üblicherweise Azure Table Storage). Dieser Zustand wird als *Orchestrierungsverlauf* bezeichnet.

### <a name="history-table"></a>Verlaufstabelle

Im Allgemeinen führt das Durable Task Framework an jedem Prüfpunkt Folgendes durch:

1. Der Ausführungsverlauf wird in Azure Storage-Tabellen gespeichert.
2. Die Nachrichten für Funktionen, die der Orchestrator aufrufen möchte, werden in die Warteschlange eingereiht.
3. Nachrichten, die für den Orchestrator selbst bestimmt sind – z.B. Nachrichten des permanenten Timers –, werden in die Warteschlange eingereiht.

Nachdem der Prüfpunktvorgang abgeschlossen ist, kann die Orchestratorfunktion aus dem Arbeitsspeicher entfernt werden, bis weitere Arbeitsschritte damit ausgeführt werden müssen.

> [!NOTE]
> Azure Storage gibt keine Transaktionsgarantie für das Speichern von Daten in Tabellenspeicher und Warteschlangen. Zur Behandlung von Ausfällen nutzt der Anbieter von Durable Functions-Speicher Muster vom Typ *Letztliche Konsistenz*. Mit diesen Mustern wird sichergestellt, dass keine Daten verloren gehen, wenn es zu einem Absturz kommt oder während des Prüfpunktprozesses die Verbindung getrennt wird.

Nach Abschluss des Vorgangs sieht der Verlauf der obigen Funktion in Azure Table Storage in etwa wie in der folgenden Tabelle aus (zur Veranschaulichung gekürzt):

| PartitionKey (InstanceId)                     | EventType             | Timestamp               | Eingabe | NAME             | Ergebnis                                                    | Status |
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     |
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | null  | E1_HelloSequence |                                                           |                     |
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | """Hello Tokyo!"""                                        |                     |
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | """Hello Seattle!"""                                      |                     |
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | """Hello London!"""                                       |                     |
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | "[""Hello Tokyo!"",""Hello Seattle!"",""Hello London!""]" | Abgeschlossen           |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     |

Einige Hinweise zu den Spaltenwerten:

* **PartitionKey**: Enthält die Instanz-ID der Orchestrierung.
* **EventType**: Steht für den Typ des Ereignisses. Es kann sich um einen der folgenden Typen handeln:
  * **OrchestrationStarted**: Die Orchestratorfunktion wurde aus einem Wartezustand fortgesetzt oder wird zum ersten Mal ausgeführt. Die Spalte `Timestamp` wird verwendet, um den deterministischen Wert für die `CurrentUtcDateTime`- (.NET) und `currentUtcDateTime`-APIs (JavaScript) aufzufüllen.
  * **ExecutionStarted**: Die Ausführung der Orchestratorfunktion wurde zum ersten Mal gestartet. Dieses Ereignis enthält auch die Funktionseingabe in der Spalte `Input`.
  * **TaskScheduled**: Eine Aktivitätsfunktion wurde geplant. Der Name der Aktivitätsfunktion wird in der Spalte `Name` erfasst.
  * **TaskCompleted**: Eine Aktivitätsfunktion wurde abgeschlossen. Das Ergebnis der Funktion ist in der Spalte `Result` enthalten.
  * **TimerCreated**: Ein permanenter Timer wurde erstellt. Die Spalte `FireAt` enthält die geplante UTC-Uhrzeit, zu der der Timer abläuft.
  * **TimerFired**: Ein permanenter Timer hat ausgelöst.
  * **EventRaised**: Ein externes Ereignis wurde an die Orchestrierungsinstanz gesendet. In der Spalte `Name` wird der Name des Ereignisses und in der Spalte `Input` die Nutzlast des Ereignisses erfasst.
  * **OrchestratorCompleted**: Die wartende Orchestratorfunktion.
  * **ContinueAsNew**: Die Orchestratorfunktion wurde abgeschlossen und mit einem neuen Zustand neu gestartet. Die Spalte `Result` enthält den Wert, der als Eingabe in der neu gestarteten Instanz verwendet wird.
  * **ExecutionCompleted**: Die Orchestratorfunktion wurde bis zum Abschluss (oder Fehler) ausgeführt. Die Ausgaben der Funktion bzw. die Fehlerdetails werden in der Spalte `Result` gespeichert.
* **Timestamp** (Zeitstempel): Der UTC-Zeitstempel des Verlaufsereignisses.
* **Name**: Der Name der Funktion, die aufgerufen wurde.
* **Eingabe**: Die Eingabe der Funktion im JSON-Format.
* **Result**: Die Ausgabe der Funktion, also ihr Rückgabewert.

> [!WARNING]
> Die Tabelle ist zwar als Debugtool nützlich, aber Sie sollten keine Abhängigkeiten dafür einrichten. Dies kann sich im Rahmen der Weiterentwicklung der Erweiterung Durable Functions ändern.

Jedes Mal, wenn die Funktion aus einem `await` (C#)- oder `yield` (JavaScript)-Zustand fortgesetzt wird, führt das Durable Task Framework die Orchestratorfunktion von Grund auf neu aus. Bei jeder erneuten Ausführung wird der Ausführungsverlauf herangezogen, um zu ermitteln, ob der aktuelle asynchrone Vorgang ausgeführt wurde.  Wenn ja, gibt das Framework die Ausgabe dieses Vorgangs sofort wieder und fährt mit dem nächsten `await` (C#)- oder `yield` (JavaScript)-Element fort. Dieser Prozess wird fortgesetzt, bis der gesamte Verlauf wiedergegeben wurde. Nachdem der aktuelle Verlauf wiedergegeben wurde, werden die lokalen Variablen wieder auf Ihre vorherigen Werte zurückgesetzt.

## <a name="features-and-patterns"></a>Funktionen und Muster

In den nächsten Abschnitten werden die Features und Muster von Orchestratorfunktionen beschrieben.

### <a name="sub-orchestrations"></a>Untergeordnete Orchestrierungen

Orchestratorfunktionen können nicht nur Aktivitätsfunktionen aufrufen, sondern auch andere Orchestratorfunktionen. Beispielsweise können Sie aus einer Bibliothek mit Orchestratorfunktionen eine größere Orchestrierung erstellen. Alternativ können Sie mehrere Instanzen einer Orchestratorfunktion parallel ausführen.

Weitere Informationen und Beispiele finden Sie im Artikel [Untergeordnete Orchestrierungen in Durable Functions (Azure Functions)](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Permanente Timer

Orchestrierungen können mithilfe *permanenter Timer* Verzögerungen implementieren oder die Timeoutbehandlung für asynchrone Aktionen einrichten. Verwenden Sie permanente Timer in Orchestratorfunktionen anstelle von `Thread.Sleep` und `Task.Delay` (C#) oder `setTimeout()` und `setInterval()` (JavaScript).

Weitere Informationen und Beispiele finden Sie im Artikel [Timer in Durable Functions (Azure Functions)](durable-functions-timers.md).

### <a name="external-events"></a>Externe Ereignisse

Orchestratorfunktionen können auf externe Ereignisse warten, um eine Orchestrierungsinstanz zu aktualisieren. Dieses Feature von Durable Functions ist oft bei der Verarbeitung von Benutzerinteraktionen oder anderer externer Rückrufe nützlich.

Weitere Informationen und Beispiele finden Sie im Artikel [Behandeln von externen Ereignissen in Durable Functions (Azure Functions)](durable-functions-external-events.md).

### <a name="error-handling"></a>Fehlerbehandlung

Orchestratorfunktionen können die Fehlerbehandlungsfeatures der Programmiersprache verwenden. Vorhandene Muster wie `try`/`catch` werden in Orchestrierungscode unterstützt.

Orchestratorfunktionen können den Aktivitäts- oder untergeordneten Orchestratorfunktionen, die sie aufrufen, auch Wiederholungsrichtlinien hinzufügen. Sollte bei einer Aktivitäts- oder untergeordneten Orchestratorfunktion ein Ausnahmefehler auftreten, kann die angegebene Wiederholungsrichtlinie die Ausführung automatisch verzögern und eine bestimmte Anzahl von Wiederholungsversuchen durchführen.

> [!NOTE]
> Im Falle eines unbehandelten Ausnahmefehlers in einer Orchestratorfunktion wird die Orchestrierungsinstanz im Zustand `Failed` beendet. Eine fehlerhafte Orchestrierungsinstanz kann nicht wiederholt werden.

Weitere Informationen und Beispiele finden Sie im Artikel [Fehlerbehandlung in Durable Functions (Azure Functions)](durable-functions-error-handling.md).

### <a name="critical-sections-durable-functions-2x"></a>Kritische Abschnitte (Durable Functions 2. x)

Da es sich bei Orchestrierungsinstanzen um Singlethread-Instanzen handelt, müssen Sie sich keine Gedanken über Racebedingungen *innerhalb* einer Orchestrierung machen. Racebedingung sind jedoch möglich, wenn Orchestrierungen mit externen Systemen interagieren. Um Racebedingungen bei der Interaktion mit externen Systemen entgegenzuwirken, können Orchestratorfunktionen in .NET mithilfe einer Methode vom Typ `LockAsync` *kritische Abschnitte* definieren.

Der folgende Beispielcode zeigt eine Orchestratorfunktion, die einen kritischen Abschnitt definiert. Der kritische Abschnitt wird mithilfe der Methode `LockAsync` gestartet. Diese Methode erfordert die Übergabe mindestens eines Verweises an eine [dauerhafte Entität](durable-functions-entities.md), die dauerhaft den Sperrzustand verwaltet. Der Code im kritischen Abschnitt kann jeweils nur von einer einzelnen Instanz dieser Orchestrierung ausgeführt werden.

```csharp
[FunctionName("Synchronize")]
public static async Task Synchronize(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var lockId = new EntityId("LockEntity", "MyLockIdentifier");
    using (await context.LockAsync(lockId))
    {
        // critical section - only one orchestration can enter at a time
    }
}
```

`LockAsync` ruft die dauerhaften Sperren ab und gibt `IDisposable` zurück, um den kritischen Abschnitt beim Verwerfen zu beenden. Dieses Ergebnis vom Typ `IDisposable` kann zusammen mit einem Block vom Typ `using` verwendet werden, um eine syntaktische Darstellung des kritischen Abschnitts zu erhalten. Wenn eine Orchestratorfunktion in einen kritischen Abschnitt eintritt, kann dieser Codeblock nur von einer einzelnen Instanz ausgeführt werden. Alle anderen Instanzen, die versuchen, in den kritischen Abschnitt einzutreten, werden blockiert, bis die vorherige Instanz den kritischen Abschnitt verlässt.

Das Feature „kritischer Abschnitt“ ist auch hilfreich, um Änderungen an dauerhaften Entitäten zu koordinieren. Weitere Informationen zu kritischen Abschnitten finden Sie im Thema [Koordination von Entitäten](durable-functions-entities.md#entity-coordination).

> [!NOTE]
> Kritische Abschnitte stehen ab Durable Functions 2.0 zur Verfügung. Aktuell wird dieses Feature nur von .NET-Orchestrierungen implementiert.

### <a name="calling-http-endpoints-durable-functions-2x"></a>Aufrufen von HTTP-Endpunkten (Durable Functions 2.x)

Von Orchestratorfunktionen dürfen keine E/A-Vorgänge ausgeführt werden, wie unter [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md) beschrieben. Zur Umgehung dieses Problems wird der Code, der E/A-Vorgänge ausführen muss, in der Regel in eine Aktivitätsfunktion eingeschlossen. Orchestrierungen, die mit externen Systemen interagieren, verwenden häufig Aktivitätsfunktionen, um HTTP-Aufrufe durchzuführen und das Ergebnis an die Orchestrierung zurückzugeben.

Zur Vereinfachung dieses gängigen Musters können Orchestratorfunktionen in .NET die Methode `CallHttpAsync` verwenden, um HTTP-APIs direkt aufzurufen. Neben der Unterstützung grundlegender Anforderungs-/Antwortmuster unterstützt `CallHttpAsync` auch die automatische Behandlung gängiger asynchroner HTTP 202-Abrufmuster sowie die Authentifizierung mit externen Diensten unter Verwendung [verwalteter Identitäten](../../active-directory/managed-identities-azure-resources/overview.md).

```csharp
[FunctionName("CheckSiteAvailable")]
public static async Task CheckSiteAvailable(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    Uri url = context.GetInput<Uri>();

    // Makes an HTTP GET request to the specified endpoint
    DurableHttpResponse response = 
        await context.CallHttpAsync(HttpMethod.Get, url);

    if (response.StatusCode >= 400)
    {
        // handling of error codes goes here
    }
}
```

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const url = context.df.getInput();
    var res = yield context.df.callHttp("GET", url);
    if (res.statusCode >= 400) {
        // handling of error codes goes here
    }
});
```

Weitere Informationen und ausführliche Beispiele finden Sie im Artikel [HTTP-Features](durable-functions-http-features.md).

> [!NOTE]
> Das direkte Aufrufen von HTTP-Endpunkten über Orchestratorfunktionen ist ab Durable Functions 2.0 möglich.

### <a name="passing-multiple-parameters"></a>Übergeben von mehreren Parametern

Es ist nicht möglich, mehrere Parameter direkt an eine Aktivitätsfunktion zu übergeben. Es wird empfohlen, ein Array von Objekten zu übergeben oder Objekte vom Typ [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) in .NET zu verwenden.

Das folgende Beispiel verwendet neue Features von [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples), die mit [C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples) hinzugefügt wurden:

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<object> RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    object courseRecommendations = await context.CallActivityAsync<object>(
        "CourseRecommendations",
        (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<object> Mapper([ActivityTrigger] IDurableActivityContext inputs)
{
    // parse input for student's major and year in university
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new
    {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md)
