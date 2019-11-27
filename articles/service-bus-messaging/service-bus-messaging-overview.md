---
title: Übersicht über Azure Service Bus-Messaging | Microsoft-Dokumentation
description: Beschreibung von Service Bus-Messaging
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: overview
ms.date: 11/04/2019
ms.custom: mvc
ms.author: aschhab
ms.openlocfilehash: e2460ab760811a3db39058eac74d519ca09046c6
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73889810"
---
# <a name="what-is-azure-service-bus"></a>Was ist Azure Service Bus?

Microsoft Azure Service Bus ist ein vollständig verwalteter Nachrichtenbroker für die Unternehmensintegration. Mit Service Bus lassen sich Anwendungen und Dienste entkoppeln. Service Bus bietet eine zuverlässige und sichere Plattform für die asynchrone Übertragung von Daten und Zuständen.

Die Datenübertragung zwischen verschiedenen Anwendungen und Diensten erfolgt mithilfe von *Nachrichten*. Nachrichten liegen im Binärformat vor und können JSON-Code, XML-Code oder einfach nur Text enthalten. Weitere Informationen finden Sie unter [Integrationsdienste](https://azure.com/integration).

Im Anschluss finden Sie einige gängige Messagingszenarien:

* *Messaging*: Übertragung von Geschäftsdaten (beispielsweise Verkäufe/Bestellungen, Journale oder Bestandsbewegungen)
* *Entkoppelung von Anwendungen*: Höhere Zuverlässigkeit und bessere Skalierbarkeit von Anwendungen und Diensten. Client und Dienst müssen nicht gleichzeitig online sein.
* *Themen und Abonnements*: Ermöglichung von 1:*n*-Beziehungen zwischen Herausgebern und Abonnenten
* *Nachrichtensitzungen*: Implementierung von Workflows, die die Sortierung oder Verzögerung von Nachrichten erfordern

## <a name="namespaces"></a>Namespaces

Ein Namespace ist ein Container für alle Messagingkomponenten. Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer.

## <a name="queues"></a>Warteschlangen

Nachrichten werden an *Warteschlangen* gesendet und daraus empfangen. Warteschlangen dienen zum Speichern von Nachrichten, bis die empfangende Anwendung empfangs- und verarbeitungsbereit ist.

![Warteschlange](./media/service-bus-messaging-overview/about-service-bus-queue.png)

Nachrichten in Warteschlangen werden bei ihrem Eingang sortiert und mit einem Zeitstempel versehen. Nachdem eine Nachricht akzeptiert wurde, wird sie sicher in redundantem Speicher gespeichert. Nachrichten werden im *Pull-Modus* (also nur auf Anforderung) zugestellt.

## <a name="topics"></a>Themen

Nachrichten können auch unter Verwendung von *Themen* gesendet und empfangen werden. Themen sind in Veröffentlichungs-/Abonnementszenarien hilfreich. Bei der Punkt-zu-Punkt-Kommunikation werden dagegen häufig Warteschlangen verwendet.

![Thema](./media/service-bus-messaging-overview/about-service-bus-topic.png)

Themen können über mehrere unabhängige Abonnements verfügen. Ein Abonnent eines Themas kann eine Kopie jeder Nachricht erhalten, die an das Thema gesendet wird. Bei Abonnements handelt es sich um benannte Entitäten. Abonnements bleiben bestehen, können aber ablaufen oder automatisch gelöscht werden.

Manche Abonnements sollen unter Umständen nicht alle Nachrichten erhalten, die an ein Thema gesendet werden. In diesem Fall können Sie mithilfe von *Regeln* und *Filtern* Bedingungen zum Auslösen optionaler *Aktionen* definieren. Sie können angegebene Nachrichten filtern sowie Nachrichteneigenschaften festlegen oder ändern. Weitere Informationen finden Sie unter [Themenfilter und -aktionen](topic-filters.md).

## <a name="advanced-features"></a>Erweiterte Funktionen

Service Bus enthält auch erweiterte Features für komplexere Messagingszenarien. In den folgenden Abschnitten werden einige dieser Features beschrieben.

### <a name="message-sessions"></a>Nachrichtensitzungen

Mithilfe von Sitzungen können Sie in Service Bus eine FIFO-Garantie (First In, First Out) erstellen. Nachrichtensitzungen ermöglichen die gemeinsame und geordnete Verarbeitung unbegrenzter Sequenzen verwandter Nachrichten. Weitere Informationen finden Sie unter [Nachrichtensitzungen: FIFO (First In, First Out)](message-sessions.md).

### <a name="autoforwarding"></a>Automatische Weiterleitung

Die automatische Weiterleitung verkettet eine Warteschlange oder ein Abonnement mit einer anderen Warteschlange oder mit einem anderen Thema. Diese müssen dem gleichen Namespace angehören. Mit der automatischen Weiterleitung entfernt Service Bus automatisch Nachrichten aus einer Warteschlange oder aus einem Abonnement und platziert sie in einer anderen Warteschlange oder in einem anderen Thema. Weitere Informationen finden Sie unter [Verketten von Service Bus-Entitäten mit automatischer Weiterleitung](service-bus-auto-forwarding.md).

### <a name="dead-letter-queue"></a>Warteschlange für unzustellbare Nachrichten

Service Bus unterstützt eine Warteschlange für unzustellbare Nachrichten (Dead-Letter Queue, DLQ). Eine DLQ enthält Nachrichten, die an keinen Empfänger übermittelt werden können. Sie enthält Nachrichten, die nicht verarbeitet werden können. Mit Service Bus können Sie Nachrichten aus der DLQ entfernen und untersuchen. Weitere Informationen finden Sie unter [Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten](service-bus-dead-letter-queues.md).

### <a name="scheduled-delivery"></a>Zeitgesteuerte Zustellung

Sie können Nachrichten zur verzögerten Verarbeitung an eine Warteschlange oder an ein Thema senden. Sie können einen Auftrag so planen, dass er zu einem bestimmten Zeitpunkt für die Verarbeitung durch ein System verfügbar wird. Weitere Informationen finden Sie unter [Geplante Nachrichten](message-sequencing.md#scheduled-messages).

### <a name="message-deferral"></a>Nachrichtenverzögerung

Ein Warteschlangen- oder Abonnementclient kann das Abrufen einer Nachricht auf einen späteren Zeitpunkt verschieben. Diese Verzögerung kann durch besondere Umstände in der Anwendung bedingt sein. Die Nachricht bleibt in der Warteschlange oder im Abonnement, wird jedoch zurückgestellt. Weitere Informationen finden Sie unter [Nachrichtenverzögerung](message-deferral.md).

### <a name="batching"></a>Batchverarbeitung

Durch die clientseitige Batchverarbeitung kann ein Warteschlangen- oder Themenclient das Senden einer Nachricht für einen bestimmten Zeitraum verzögern. Wenn der Client während dieses Zeitraums weitere Nachrichten sendet, werden die Nachrichten in einem einzigen Batch übertragen. Weitere Informationen finden Sie unter [Clientseitige Batchverarbeitung](service-bus-performance-improvements.md#client-side-batching).

### <a name="transactions"></a>Transaktionen

Eine Transaktion fasst mehrere Vorgänge zu einem *Ausführungsbereich* zusammen. Service Bus unterstützt Gruppierungsvorgänge für eine einzelne Nachrichtenentität innerhalb eines einzelnen Transaktionsbereichs. Eine Nachrichtenentität kann eine Warteschlange, ein Thema oder ein Abonnement sein. Weitere Informationen finden Sie unter [Übersicht über die Service Bus-Transaktionsverarbeitung](service-bus-transactions.md).

### <a name="filtering-and-actions"></a>Filtern und Aktionen

Abonnenten können definieren, welche Nachrichten von einem Thema empfangen werden sollen. Diese Nachrichten werden in Form von benannten Abonnementregeln angegeben. Für jede übereinstimmende Regelbedingung erzeugt das Abonnement eine Kopie der Nachricht, die für jede übereinstimmende Regel anders kommentiert werden kann. Weitere Informationen finden Sie unter [Themenfilter und -aktionen](topic-filters.md).

### <a name="autodelete-on-idle"></a>Automatisches Löschen nach Leerlauf

Automatisches Löschen nach Leerlauf ermöglicht die Angabe eines Leerlaufintervalls, nach dem eine Warteschlange automatisch gelöscht wird. Die Mindestdauer ist fünf Minuten. Weitere Informationen finden Sie unter [Eigenschaft „QueueDescription.AutoDeleteOnIdle“](/dotnet/api/microsoft.servicebus.messaging.queuedescription.autodeleteonidle).

### <a name="duplicate-detection"></a>Duplikaterkennung

Ein Fehler kann dazu führen, dass der Client sich beim Ergebnis eines Sendevorgangs nicht sicher ist. Dank Duplikaterkennung kann der Absender die gleiche Nachricht erneut senden. Eine weitere Möglichkeit ist das Verwerfen von Duplikaten durch die Warteschlange oder das Thema. Weitere Informationen finden Sie unter [Duplikaterkennung](duplicate-detection.md).

### <a name="security-protocols"></a>Sicherheitsprotokolle
<a name="sas-rbac-and-managed-identities-for-azure-resources"></a>

Service Bus unterstützt Sicherheitsprotokolle wie [Shared Access Signatures](service-bus-sas.md) (SAS), die [rollenbasierte Zugriffssteuerung](authenticate-application.md) (Role Based Access Control, RBAC) und [verwaltete Identitäten für Azure-Ressourcen](service-bus-managed-service-identity.md).

### <a name="geo-disaster-recovery"></a>Georedundante Notfallwiederherstellung

Sollte eine Azure-Region oder ein Azure-Rechenzentrum ausfallen, kann die Datenverarbeitung dank georedundanter Notfallwiederherstellung in einer anderen Region oder in einem anderen Rechenzentrum fortgesetzt werden. Weitere Informationen finden Sie unter [Georedundante Notfallwiederherstellung in Azure Service Bus](service-bus-geo-dr.md).

### <a name="security"></a>Sicherheit

Service Bus unterstützt die Standardprotokolle [AMQP 1.0](service-bus-amqp-overview.md) und [HTTP/REST](/rest/api/servicebus/).

## <a name="client-libraries"></a>Clientbibliotheken

Service Bus unterstützt Clientbibliotheken für [.NET](https://github.com/Azure/azure-service-bus-dotnet/tree/master), [Java](https://github.com/Azure/azure-service-bus-java/tree/master) und [JMS](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client).

## <a name="integration"></a>Integration

Service Bus lässt sich vollständig in folgende Azure-Dienste integrieren:

* [Event Grid](https://azure.microsoft.com/services/event-grid/)
* [Logik-Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure-Funktionen](https://azure.microsoft.com/services/functions/)
* [Dynamics 365](https://dynamics.microsoft.com)
* [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit Service Bus-Messaging finden Sie in folgenden Artikeln:

* Eine Gegenüberstellung von Azure-Messagingdiensten finden Sie unter [Auswahl von Azure-Messagingdiensten: Event Grid, Event Hubs und Service Bus](../event-grid/compare-messaging-services.md?toc=%2fazure%2fservice-bus-messaging%2ftoc.json&bc=%2fazure%2fservice-bus-messaging%2fbreadcrumb%2ftoc.json).
* Sehen Sie sich die Schnellstartanleitung für [.NET](service-bus-dotnet-get-started-with-queues.md), [Java](service-bus-java-how-to-use-queues.md) oder [JMS](service-bus-java-how-to-use-jms-api-amqp.md) an.
* Informationen zur Verwaltung von Service Bus-Ressourcen finden Sie unter [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases).
* Informationen zu Standard- und Premium-Tarifen und den jeweiligen Preisen finden Sie unter [Service Bus – Preise](https://azure.microsoft.com/pricing/details/service-bus/).
* Informationen zu Leistung und Wartezeit für den Premium-Tarif finden Sie unter [Premium-Messaging: Wie schnell ist es?](https://techcommunity.microsoft.com/t5/Service-Bus-blog/Premium-Messaging-How-fast-is-it/ba-p/370722).
