---
title: HTTP-Features in Durable Functions – Azure Functions
description: Erfahren Sie mehr über die integrierten HTTP-Features in der Durable Functions-Erweiterung für Azure Functions.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: b909918ff4f9abc1dd64d4c7e5ccb35954b233f7
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935547"
---
# <a name="http-features"></a>HTTP-Features

Durable Functions verfügt über mehrere Features, die die Einbindung dauerhafter Orchestrierungen und Entitäten in HTTP-Workflows vereinfachen. In diesem Artikel werden einige dieser Features ausführlich erläutert.

## <a name="exposing-http-apis"></a>Verfügbarmachen von HTTP-APIs

Orchestrierungen und Entitäten können mithilfe von HTTP-Anforderungen aufgerufen und verwaltet werden. Die Durable Functions-Erweiterung macht integrierte HTTP-APIs verfügbar und stellt APIs für die Interaktion mit Orchestrierungen und Entitäten aus über HTTP ausgelösten Funktionen heraus bereit.

### <a name="built-in-http-apis"></a>Integrierte HTTP-APIs

Die Durable Functions-Erweiterung fügt dem Azure Functions-Host automatisch eine Reihe von HTTP-APIs hinzu. Diese APIs ermöglichen es Ihnen, mit Orchestrierungen und Entitäten zu interagieren und diese zu verwalten, ohne dass hierfür Programmieraufwand erforderlich ist.

Die folgenden integrierten HTTP-APIs werden unterstützt.

* [Starten einer neuen Orchestrierung](durable-functions-http-api.md#start-orchestration)
* [Abfragen der Orchestrierungsinstanz](durable-functions-http-api.md#get-instance-status)
* [Beenden der Orchestrierungsinstanz](durable-functions-http-api.md#terminate-instance)
* [Senden eines externen Ereignisses an eine Orchestrierung](durable-functions-http-api.md#raise-event)
* [Löschen des Orchestrierungsverlaufs](durable-functions-http-api.md#purge-single-instance-history)
* [Senden eines Vorgangsereignisses an eine Entität](durable-functions-http-api.md#signal-entity)
* [Abfragen des Zustands einer Entität](durable-functions-http-api.md#query-entity)

Im Artikel [HTTP-APIs](durable-functions-http-api.md) finden Sie eine vollständige Beschreibung aller integrierten HTTP-APIs, die von der Durable Functions-Erweiterung verfügbar gemacht werden.

### <a name="http-api-url-discovery"></a>Ermittlung der HTTP-API-URL

Die [Orchestrierungsclientbindung](durable-functions-bindings.md#orchestration-client) macht APIs verfügbar, die zum Generieren praktischer HTTP-Antwortnutzlasten verwendet werden können. Sie kann beispielsweise eine Antwort mit Links zu Verwaltungs-APIs für eine bestimmte Orchestrierungsinstanz erstellen. Im Folgenden finden Sie ein Beispiel für eine HTTP-Triggerfunktion, das zeigt, wie diese API für eine neue Orchestrierungsinstanz verwendet wird:

#### <a name="precompiled-c"></a>Vorkompilierter C#-Code

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

#### <a name="c-script"></a>C#-Skript

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

#### <a name="javascript-functions-2x-only"></a>JavaScript (nur Functions 2.x)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

#### <a name="functionjson"></a>Function.json

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json)]

Das Starten einer Orchestratorfunktion mithilfe der oben gezeigten HTTP-Triggerfunktionen kann über einen beliebigen HTTP-Client erfolgen. Der folgende cURL-Befehl startet eine Orchestratorfunktion mit dem Namen `DoWork`.

```bash
curl -X POST https://localhost:7071/orchestrators/DoWork -H "Content-Length: 0" -i
```

Im Folgenden sehen Sie eine Beispielantwort für eine Orchestrierung mit der ID `abc123` (einige Details wurden der Übersichtlichkeit halber entfernt):

```http
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX
Retry-After: 10

{
    "id": "abc123",
    "purgeHistoryDeleteUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/raiseEvent/{eventName}?code=XXX",
    "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/terminate?reason={text}&code=XXX"
}
```

Alle `~Uri`-Felder im vorherigen Beispiel entsprechen integrierten HTTP-APIs. Diese APIs können verwendet werden, um die Zielorchestrierungsinstanz zu verwalten.

> [!NOTE]
> Das Format der Webhook-URLs kann in Abhängigkeit davon variieren, welche Version des Azure Functions-Hosts Sie ausführen. Das obige Beispiel gilt für den Azure Functions 2.0-Host.

Eine Beschreibung aller integrierten HTTP-APIs finden Sie unter [HTTP-APIs in Durable Functions (Azure Functions)](durable-functions-http-api.md).

### <a name="async-operation-tracking"></a>Nachverfolgen von asynchronen Vorgängen

Die oben erwähnte HTTP-Antwort wurde als Hilfe bei der Implementierung von asynchronen HTTP-APIs mit langer Ausführungsdauer per Durable Functions konzipiert. Dieses Muster wird manchmal auch als *Polling Consumer Pattern* (Consumerabrufmuster) bezeichnet. Der Client/Server-Datenfluss funktioniert wie folgt:

1. Der Client sendet eine HTTP-Anforderung, um einen Prozess mit langer Ausführungsdauer zu starten, z.B. eine Orchestratorfunktion.
2. Der HTTP-Zieltrigger gibt eine HTTP 202-Antwort mit dem Header `Location` und dem Wert `statusQueryGetUri` zurück.
3. Der Client fragt die URL im Header `Location` ab. Er erhält weiterhin die HTTP 202-Antworten mit dem Header `Location`.
4. Wenn die Instanz abgeschlossen (oder fehlgeschlagen) ist, gibt der Endpunkt im Header `Location` die Antwort „HTTP 200“ zurück.

Dieses Protokoll ermöglicht die Koordination von Prozessen mit langer Ausführungsdauer mit externen Clients oder Diensten, die das Abfragen eines HTTP-Endpunkts unterstützen, und das Beachten des Headers `Location`. Die Client- und Serverimplementierungen dieses Musters sind in die Durable Functions-HTTP-APIs integriert.

> [!NOTE]
> Standardmäßig unterstützen alle HTTP-basierten Aktionen, die von [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) bereitgestellt werden, das Standardmuster für asynchrone Vorgänge. Diese Funktion ermöglicht das Einbetten eines Durable Functions-Elements als Teil eines Logic Apps-Workflows. Weitere Informationen zur Unterstützung von Logik-Apps für asynchrone HTTP-Muster finden Sie im Artikel [Workflowaktionen und -trigger für Azure Logic Apps](../../logic-apps/logic-apps-workflow-actions-triggers.md).

> [!NOTE]
> Die Interaktion mit Orchestrierungen kann von jedem Funktionstyp aus erfolgen, nicht nur durch über HTTP ausgelöste Funktionen.

Weitere Informationen zum Verwalten von Orchestrierungen und Entitäten mithilfe von Client-APIs finden Sie im Artikel zum [Verwalten von Instanzen](durable-functions-instance-management.md).

## <a name="consuming-http-apis"></a>Verarbeiten von HTTP-APIs

Orchestratorfunktionen dürfen E/A-Vorgänge nicht direkt ausführen (wie unter [Codeeinschränkungen für Orchestratorfunktionen](durable-functions-code-constraints.md) beschrieben). Stattdessen rufen Orchestratorfunktionen in der Regel [Aktivitätsfunktionen](durable-functions-types-features-overview.md#activity-functions) auf, die E/A-Vorgänge ausführen.

Ab Durable Functions 2.0 können Orchestrierungen mithilfe der [Orchestrierungstriggerbindung](durable-functions-bindings.md#orchestration-trigger) HTTP-APIs nativ nutzen.

> [!NOTE]
> Das direkte Aufrufen von HTTP-Endpunkten über Orchestratorfunktionen ist in JavaScript noch nicht möglich.

Der folgende Beispielcode zeigt eine C#-Orchestratorfunktion, die eine ausgehende HTTP-Anforderung mithilfe der `CallHttpAsync`-.NET-API vornimmt:

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

Mit der Aktion „call HTTP“ ist in Ihren Orchestratorfunktionen Folgendes möglich:

* Direktes Aufrufen von HTTP-APIs aus Orchestrierungsfunktionen (mit einigen später beschriebenen Einschränkungen)
* Automatisches Unterstützen clientseitiger HTTP 202-Statusabrufmuster
* Verwenden [verwalteter Azure-Identitäten](../../active-directory/managed-identities-azure-resources/overview.md) für autorisierte HTTP-Aufrufe an andere Azure-Endpunkte

Die Möglichkeit, HTTP-APIs direkt von Orchestratorfunktionen zu nutzen, ist bei einigen häufigen Szenarien sehr praktisch. Sie können alle diese Features selbst mithilfe von Aktivitätsfunktionen implementieren. In vielen Fällen bieten Ihnen Aktivitätsfunktionen mehr Flexibilität.

### <a name="http-202-handling"></a>HTTP 202-Verarbeitung

Die API „call HTTP“ kann die Clientseite des *Polling Consumer Pattern* automatisch implementieren. Wenn eine aufgerufene API eine HTTP 202-Antwort mit einem `Location`-Header zurückgibt, ruft die Orchestratorfunktion automatisch die `Location`-Ressource ab, bis eine Antwort zurückgegeben wird, die nicht 202 ist. Dies ist die Antwort, die an den Orchestratorfunktionscode zurückgegeben wird.

> [!NOTE]
> Orchestratorfunktionen bieten auch native Unterstützung für das serverseitige *Polling Consumer Pattern*, wie unter [Nachverfolgen von asynchronen Vorgängen](#async-operation-tracking) beschrieben. Dies bedeutet, dass Orchestrierungen in einer Funktions-App die Orchestratorfunktionen in anderen Funktions-Apps problemlos koordinieren können. Dies ähnelt dem Konzept der [untergeordneten Orchestrierung](durable-functions-sub-orchestrations.md), jedoch mit Unterstützung für die App-übergreifende Kommunikation. Dies ist insbesondere beim Entwickeln von Anwendung im Stil von Microservices praktisch.

### <a name="managed-identities"></a>Verwaltete Identitäten

Durable Functions bietet native Unterstützung für das Aufrufen von APIs, die Azure AD-Token für die Autorisierung akzeptieren. Diese Unterstützung verwendet [verwaltete Azure -Identitäten](../../active-directory/managed-identities-azure-resources/overview.md), um diese Token abzurufen.

Der folgende Code ist ein Beispiel für eine .NET-Orchestratorfunktion, die authentifizierte Aufrufe zum Neustart eines virtuellen Computers mithilfe der [Virtual Machines-REST-API](https://docs.microsoft.com/rest/api/compute/virtualmachines) von Azure Resource Manager vornimmt.

```csharp
[FunctionName("RestartVm")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string subscriptionId = "mySubId";
    string resourceGroup = "myRG";
    string vmName = "myVM";
    
    // Automatically fetches an Azure AD token for resource = https://management.core.windows.net
    // and attaches it to the outgoing Azure Resource Manager API call.
    var restartRequest = new DurableHttpRequest(
        HttpMethod.Post, 
        new Uri($"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Compute/virtualMachines/{vmName}/restart?api-version={apiVersion}"),
        tokenSource: new ManagedIdentityTokenSource("https://management.core.windows.net"));
    DurableHttpResponse restartResponse = await context.CallHttpAsync(restartRequest);
    if (restartResponse.StatusCode != HttpStatusCode.OK)
    {
        throw new ArgumentException($"Failed to restart VM: {restartResponse.StatusCode}: {restartResponse.Content}");
    }
}
```

Im vorherigen Beispiel ist der `tokenSource`-Parameter so konfiguriert, dass Azure AD-Token für [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) abgerufen werden (identifiziert durch den „Ressourcen-URI“ `https://management.core.windows.net`). Im Beispiel wird davon ausgegangen, dass die aktuelle Funktions-App entweder lokal ausgeführt wird oder als Azure Functions-App mit einer verwalteten Identität bereitgestellt wurde. Es wird davon ausgegangen, dass die lokale Identität oder die verwaltete Identität über Berechtigungen zum Verwalten virtueller Computer in der angegebenen Ressourcengruppe `myRG` verfügt.

Zur Laufzeit wird von der konfigurierten Tokenquelle automatisch ein OAuth 2.0-Zugriffstoken zurückgegeben und dem `Authorization`-Header der ausgehenden Anforderung als Bearertoken hinzugefügt. Dieses Modell ist aus folgenden Gründen besser als einen Autorisierungsheader manuell zu HTTP-Anforderungen hinzuzufügen:

* Die Tokenaktualisierung wird automatisch durchgeführt. Sie müssen sich nicht um abgelaufene Token kümmern.
* Token werden niemals im Zustand der dauerhaften Orchestrierung gespeichert.
* Sie müssen keinen Code schreiben, um Token abzurufen.

Ein ausführlicheres Beispiel finden Sie in den [vorkompilierten C#-„RestartVMs“-Beispielen](https://github.com/Azure/azure-functions-durable-extension/blob/v2/samples/v2/precompiled/RestartVMs.cs).

Verwaltete Identitäten sind nicht auf die Azure-Ressourcenverwaltung beschränkt. Verwaltete Identitäten können für den Zugriff auf beliebige APIs verwendet werden, die Azure AD-Bearertoken akzeptieren, einschließlich der Azure-Dienste oder Webanwendungen von Drittanbietern. Die Drittanbieter-Web-App könnte sogar eine andere Funktions-App sein. Eine Liste mit Azure-Diensten, die die Authentifizierung mit Azure AD unterstützen, finden Sie unter [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

### <a name="limitations"></a>Einschränkungen

Die integrierte Unterstützung für das Aufrufen von HTTP-APIs ist ein praktisches Feature und nicht für alle Szenarien geeignet. Von Orchestratorfunktionen gesendete HTTP-Anforderungen und ihre Antworten werden serialisiert und als Warteschlangennachrichten dauerhaft gespeichert. Dieses Warteschlangenverhalten stellt sicher, dass HTTP-Aufrufe [zuverlässig und beim Wiedergeben der Orchestrierung sicher sind](durable-functions-orchestrations.md#reliability). Dieses Warteschlangenverhalten bedeutet jedoch auch Folgendes:

* Jede HTTP-Anforderung umfasst eine zusätzliche Wartezeit im Vergleich zu einem nativen HTTP-Client.
* Umfangreiche Anforderungs- oder Antwortnachrichten, die nicht in eine Warteschlangennachricht passen, können die Orchestrierungsleistung erheblich beeinträchtigen. Die potenzielle Leistungsminderung liegt an dem Mehraufwand für die Abladung von Nachrichtennutzlasten im Blobspeicher.
* Streaming und segmentierte und binäre Nutzlasten werden nicht unterstützt.
* Die Möglichkeit, das Verhalten des HTTP-Clients anzupassen, ist begrenzt.

Wenn Sie durch eine dieser Einschränkungen beeinträchtigt sind, nutzen Sie stattdessen ggf. Aktivitätsfunktionen und sprachspezifische HTTP-Clientbibliotheken, um ausgehende HTTP-Aufrufe vorzunehmen.

> [!NOTE]
> Wenn Sie .NET-Entwickler sind, fragen Sie sich vielleicht, weshalb diese Funktion die Typen `DurableHttpRequest` und `DurableHttpResponse` anstelle der integrierten .NET-Typen `HttpRequestMessage` und `HttpResponseMessage` verwendet. Diese Entscheidung war beabsichtigt. In erster Linie stellen benutzerdefinierte Typen sicher, dass Benutzer keine falschen Annahmen über unterstützte Verhaltensweisen des internen HTTP-Clients treffen. Mit spezifischen, dauerhaften Typen können Sie außerdem den API-Entwurf und spezielle Features vereinfachen, wie beispielweise die [Integration verwalteter Identitäten](#managed-identities) und das [Polling Consumer Pattern](#http-202-handling).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über dauerhafte Entitäten](durable-functions-entities.md)