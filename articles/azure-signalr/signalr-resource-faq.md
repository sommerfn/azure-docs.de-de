---
title: Häufig gestellte Fragen zum Azure SignalR Service
description: FAQ für Azure SignalR Service.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: e9e41ffa335aa95b139a5d5658424c1c5915b569
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2019
ms.locfileid: "64914965"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR Service FAQ

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Ist Azure SignalR Service bereit für den Produktionseinsatz?

Ja.
Unsere Ankündigung der allgemeinen Verfügbarkeit, finden Sie unter [Azure SignalR Service jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) wird vollständig unterstützt.

Unterstützung für ASP.NET SignalR befindet sich noch in der *öffentlichen Vorschauphase*. Hier finden Sie ein [Codebeispiel](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>Die Clientverbindung wird mit der Fehlermeldung „Kein Server verfügbar“ geschlossen. Was bedeutet das?

Dieser Fehler tritt nur auf, wenn Clients Meldungen an den SignalR Service senden.

Wenn Sie keinen Anwendungsserver besitzen und nur die SignalR Service REST-API verwenden, ist dieses Verhalten **beabsichtigt**.
In einer serverlosen Architektur befinden sich Clientverbindungen im **LAUSCHEN**-Modus und senden keine Meldungen an SignalR Service.
Weitere Informationen zur [REST-API](./signalr-quickstart-rest-api.md).

Wenn Sie über Anwendungsserver verfügen, bedeutet diese Fehlermeldung, dass kein Anwendungsserver mit Ihrer SignalR Service-Instanz verbunden ist.

Mögliche Ursachen sind:
- Kein Anwendungsserver ist mit SignalR Service verbunden. Überprüfen Sie die Anwendungsserverprotokolle auf mögliche Verbindungsfehler. Dieser Fall kommt selten in Hochverfügbarkeitsszenarien mit mehr als einem Anwendungsserver vor.
- Es liegen Konnektivitätsprobleme mit SignalR Service-Instanzen vor. Dieses Problem ist vorübergehend und wird automatisch behoben.
Wenn es länger als eine Stunde auftritt, [öffnen Sie ein Problem auf GitHub](https://github.com/Azure/azure-signalr/issues/new), oder [erstellen Sie eine Supportanfrage in Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Wenn mehrere Anwendungsserver vorhanden sind, werden Clientmeldungen an alle Server oder nur an einen von ihnen gesendet?

Zwischen Client und Anwendungsserver besteht eine 1: 1-Zuordnung. Meldungen von einem Client werden immer an denselben Anwendungsserver gesendet.

Die Zuordnung zwischen Client und Anwendungsserver bleibt bestehen, bis der Client oder der Anwendungsserver die Verbindung trennt.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Wenn einer meiner Anwendungsserver nicht betriebsbereit ist, wie kann ich ihn auffinden und darüber informiert werden?

SignalR Service überwacht den Heartbeat der Anwendungsserver.
Wenn über einen angegebenen Zeitraum hinweg keine Heartbeats empfangen werden, gilt der Anwendungsserver als offline. Alle Clientverbindungen, die diesem Anwendungsserver zugeordnet sind, werden getrennt.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>Warum löst mein benutzerdefinierter `IUserIdProvider` eine Ausnahme aus, wenn vom ASP.NET Core SignalR SDK zum Azure SignalR Service SDK gewechselt wird?

Der Parameter `HubConnectionContext context` unterscheidet sich zwischen ASP.NET Core SignalR SDK und Azure SignalR Service-SDK, wenn `IUserIdProvider` aufgerufen wird.

In ASP.NET Core SignalR ist `HubConnectionContext context` der Kontext aus der physischen Clientverbindung mit gültigen Werten für alle Eigenschaften.

In der Azure SignalR Service SDK ist `HubConnectionContext context` der Kontext aus der logischen Clientverbindung. Die physische Clientverbindung besteht mit der SignalR Service-Instanz, weshalb also nur eine begrenzte Anzahl von Eigenschaften bereitgestellt wird.

Vorerst sind nur `HubConnectionContext.GetHttpContext()` und `HubConnectionContext.User` für den Zugriff verfügbar.
Den Quellcode können Sie sich [hier](https://github.com/Azure/azure-signalr/blob/kevinzha/faq/src/Microsoft.Azure.SignalR/ServiceHubConnectionContext.cs) ansehen.

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>Kann ich die in SignalR Service verfügbaren Transporte so konfigurieren, wie sie sich serverseitig bei ASP.NET Core SignalR konfigurieren lassen? Kann ich beispielsweise den WebSocket-Transport deaktivieren?

 Nein.

Azure SignalR Service bietet alle drei Transporte, die von ASP.NET Core SignalR standardmäßig unterstützt werden. Dies ist nicht konfigurierbar. SignalR Service verarbeitet Verbindungen und Transporte für alle Clientverbindungen.

Sie können die clientseitigen Transporte wie [hier](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1#configure-allowed-transports) dokumentiert konfigurieren.
